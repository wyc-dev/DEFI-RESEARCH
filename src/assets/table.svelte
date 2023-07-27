

<script>
  import { fade } from 'svelte/transition';

  const desiredSymbols = ["BTC-USD", "ETH-USD", "SOL-USD", "DOGE-USD", "EOS-USD", "NEAR-USD", "TRX-USD", "YFI-USD", "UNI-USD", "ATOM-USD", "FIL-USD", "XMR-USD", "MATIC-USD", "DOT-USD"];


  const schema = {
    "@context": "https://schema.org",
    "@type": "WebSite",
    "name": "L2 Research",
    "url": "https://l2-research.vercel.app/",
    "description": "Real-time cryptocurrency trend analysis with decentralized and centralized data.",
    "publisher": {
      "@type": "Organization",
      "name": "L2 Research",
      "logo": {
        "@type": "ImageObject",
        "url": "https://i.ibb.co/k0MG7Xs/L2R.png"
      }
    }
  };

  const script = document.createElement('script');
  script.setAttribute('type', 'application/ld+json');
  script.innerHTML = JSON.stringify(schema);
  document.head.appendChild(script);

  let markets = {};

  async function getBybitOrderbook(symbol) {
    const baseUrl1 = "https://api.bytick.com/v5/market/orderbook";
    const baseUrl2 = "https://api.bybit.com/v5/market/orderbook";
    const baseUrls = [baseUrl1, baseUrl2];

    const params = new URLSearchParams({
      category: "linear",
      symbol: symbol,
      limit: 1,
    });

    for (const baseUrl of baseUrls) {
      try {
        const response = await fetch(`${baseUrl}?${params.toString()}`);
        const data = await response.json();

        if (data.retCode === 0 && data.result && data.result.b && data.result.a) {
          const result = data.result;
          const b = result.b[0];
          const a = result.a[0];
          return { b, a };
        } else {
          console.error(`Error fetching orderbook from ${baseUrl}: ${data.retMsg}`);
        }
      } catch (error) {
        console.error(`Error fetching orderbook from ${baseUrl}: ${error}`);
      }
    }

    return null;
  }

  async function updateData() {
    const dydxResponse = await fetch("https://api.dydx.exchange/v3/markets");
    const dydxData = await dydxResponse.json();
    const dydxClosePrices = await getDyDxClosePrices();
    const oldMarkets = markets;
    markets = dydxData.markets;

    Object.keys(markets).forEach(market => {
      const oldPrice = oldMarkets[market]?.indexPrice;
      const newPrice = markets[market].indexPrice;
      const index_cell = document.getElementById(`price-${market}`);
      const oldOracle = oldMarkets[market]?.oraclePrice;
      const newOracle = markets[market].oraclePrice;
      const oracle_cell = document.getElementById(`oracle-${market}`);
      const signal_cell = document.getElementById(`signal-${market}`);

      if (index_cell && newPrice !== oldPrice) {
        index_cell.style.backgroundColor = newPrice > oldPrice ? "#aaaaaa" : "#06cca9";
        setTimeout(() => {
          index_cell.style.transition = "background-color 1s";
          index_cell.style.backgroundColor = "";
        }, 1000);
      }

      if (oracle_cell && newOracle !== oldOracle) {
        oracle_cell.style.backgroundColor = newOracle > oldOracle ? "#aaaaaa" : "#06cca9";
        setTimeout(() => {
          oracle_cell.style.transition = "background-color 1s";
          oracle_cell.style.backgroundColor = "";
        }, 1000);
      }
    });
  }

  async function getDyDxClosePrices() {
    const response = await fetch("https://api.dydx.exchange/v3/stats/");
    const data = await response.json();
    return data.markets;
  }

  function updateCellColor(cell, newValue, oldValue) {
    if (cell && newValue !== oldValue) {
      cell.style.backgroundColor = newValue > oldValue ? "#aaaaaa" : "#06cca9";
      setTimeout(() => {
        cell.style.transition = "background-color 1s";
        cell.style.backgroundColor = "";
      }, 1000);
    }
  }

  async function updateDyDxClosePrices() {
    const dydxClosePrices = await getDyDxClosePrices();

    Object.keys(dydxClosePrices).forEach((market) => {
      const dydx_close_cell = document.getElementById(`dydx-close-${market}`);
      const dydxClosePrice = parseFloat(dydxClosePrices[market].close);
      const oldDyDxClosePrice = parseFloat(dydx_close_cell.textContent);

      updateCellColor(dydx_close_cell, dydxClosePrice, oldDyDxClosePrice);

      if (dydx_close_cell) {
        dydx_close_cell.textContent = dydxClosePrice;
      }
    });
  }

  async function updateOrderbook() {
    // Update Bybit orderbook data
    Object.keys(markets).forEach(async (market) => {
      const symbol = market.replace("-", "").replace(" ", "").replace("USD", "USDT");
      const orderbook = await getBybitOrderbook(symbol);
      if (orderbook) {
        const bybit_bid_cell = document.getElementById(`bybit-bid-${market}`);
        const bybit_ask_cell = document.getElementById(`bybit-ask-${market}`);
        const bybit_bidq_cell = document.getElementById(`bybit-bidq-${market}`);
        const bybit_askq_cell = document.getElementById(`bybit-askq-${market}`);

        const updateCellColor = (cell, newValue, oldValue) => {
          if (cell && newValue !== oldValue) {
            cell.style.backgroundColor = newValue > oldValue ? "#aaaaaa" : "#06cca9";
            setTimeout(() => {
              cell.style.transition = "background-color 1s";
              cell.style.backgroundColor = "";
            }, 1000);
          }
        };

        updateCellColor(bybit_bid_cell, orderbook.b[0], bybit_bid_cell.textContent);
        updateCellColor(bybit_ask_cell, orderbook.a[0], bybit_ask_cell.textContent);
        updateCellColor(bybit_bidq_cell, orderbook.b[1], bybit_bidq_cell.textContent);
        updateCellColor(bybit_askq_cell, orderbook.a[1], bybit_askq_cell.textContent);

        if (bybit_bid_cell) {
          bybit_bid_cell.textContent = orderbook.b[0];
        }
        if (bybit_bidq_cell) {
          bybit_bidq_cell.textContent = orderbook.b[1];
        }
        if (bybit_ask_cell) {
          bybit_ask_cell.textContent = orderbook.a[0];
        }
        if (bybit_askq_cell) {
          bybit_askq_cell.textContent = orderbook.a[1];
        }

        const signal_cell = document.getElementById(`signal-${market}`);
        const newOracle = parseFloat(markets[market].oraclePrice);
        const newPrice = parseFloat(markets[market].indexPrice);
        const spot = parseFloat(document.getElementById(`dydx-close-${market}`).textContent);
        const bybitAsk = parseFloat(document.getElementById(`bybit-ask-${market}`).textContent);
        const bybitBid = parseFloat(document.getElementById(`bybit-bid-${market}`).textContent);

        if (bybitAsk > newPrice && newOracle < newPrice && newOracle > spot) {
          signal_cell.style.backgroundColor = "#63be44";
        } else if (bybitBid < newPrice && newOracle > newPrice && newOracle < spot) {
          signal_cell.style.backgroundColor = "#aaaaaa";
        } else {
          signal_cell.style.backgroundColor = "black";
        }
      }
    });
  }

  setInterval(updateData, 500);
  setInterval(updateOrderbook, 6000);
  setInterval(updateDyDxClosePrices, 1000);
</script>

    {#if Object.keys(markets).length > 0}
      <table>
        <thead in:fade>
          <tr>
            <th></th>
            <!-- <th>DEX Spot</th> -->
            <th>DEX Index</th>
            <th>DEX Oracle</th>
            <th>Bid</th>
            <th>Quantity</th>
            <th>Ask</th>
            <th>Quantity</th>
          </tr>
        </thead>
        <tbody in:fade>
          {#each Object.keys(markets) as market}
            {#if desiredSymbols.includes(market)}
              <tr>
                <td class="big" id={`signal-${market}`}>{markets[market].market.replace("-", "").replace(" ", "").replace("USD", "")}</td>
                <!-- <td id={`dydx-close-${market}`}></td> -->
                <td id={`price-${market}`}>{markets[market].indexPrice}</td>
                <td id={`oracle-${market}`}>{markets[market].oraclePrice}</td>
                <td id={`bybit-bid-${market}`}></td>
                <td id={`bybit-bidq-${market}`}></td>
                <td id={`bybit-ask-${market}`}></td>
                <td id={`bybit-askq-${market}`}></td>
              </tr>
            {/if}
          {/each}
        </tbody>
      </table>
    {/if}

<style>
  * {
    /* color: black; */
    padding: 0;
    margin: 0;
    font-family: 'Nunito Sans';
    flex-direction: column;
    text-align: center;
    justify-content: center;
  }

  .big {
    padding: 3px;
    font-size: 1.1em;
  }


  table {
    color : #aaaaaa;
    /* position: fixed; */
    /* top: 360px; */
    /* left: -1%; */
    font-size: 0.1em;
    width: 50vw;
    margin: auto;
    font-size: 0.8em;
    border-collapse: collapse;
  }

  @media (max-width: 610px) {
    thead {
      font-size: 1em;
    }
    table {
      font-size: 0.59em;
    }
  }

  @media (max-width: 390px) {
    thead {
      font-size: 1em;
    }
    table {
      font-size: 0.41em;
    }
  }
</style>
