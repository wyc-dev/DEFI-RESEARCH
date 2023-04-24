<script>
import { fade } from 'svelte/transition';
import Logo from './l2logo.svelte';


let markets = {};

// async function getDydxOrderbook(symbol) {
//   const response = await fetch(`https://api.dydx.exchange/v3/orderbook/${symbol}`);
//   const data = await response.json();

//   if (data && data.asks && data.asks.length > 0 && data.bids && data.bids.length > 0) {
//     const bestAsk = data.asks[0];
//     const bestBid = data.bids[0];

//     return {
//       a: [bestAsk.price, bestAsk.size],
//       b: [bestBid.price, bestBid.size]
//     };
//   } else {
//     return null;
//   }
// }

async function getBybitOrderbook(symbol) {
  const baseUrl = "https://api.bybit.com/v5/market/orderbook";
  const params = new URLSearchParams({
    category: "linear",
    symbol: symbol,
    limit: 1,
  });

  try {
    const response = await fetch(`${baseUrl}?${params.toString()}`);
    const data = await response.json();

    if (data.retCode === 0) {
      const result = data.result;
      const b = result.b[0];
      const a = result.a[0];
      return { b, a };
    } else {
      console.error(`Error fetching orderbook: ${data.retMsg}`);
      return null;
    }
  } catch (error) {
    console.error(`Error fetching orderbook: ${error}`);
    return null;
  }
}

// async function getBinanceOrderbook(symbol) {
//   try {
//     const response = await fetch(`https://api.binance.com/api/v3/depth?limit=1&symbol=${symbol}`);
//     const data = await response.json();

//     if (data && data.asks && data.asks.length > 0 && data.bids && data.bids.length > 0) {
//       const bestAsk = data.asks[0];
//       const bestBid = data.bids[0];

//       return {
//         a: [bestAsk[0], bestAsk[1]],
//         b: [bestBid[0], bestBid[1]]
//       };
//     } else {
//       return null;
//     }
//   } catch (error) {
//     console.error(`Error fetching Binance orderbook for symbol ${symbol}:`, error);
//     return null;
//   }
// }




async function updateData() {

  const response = await fetch("https://api.dydx.exchange/v3/markets");
  const data = await response.json();
  const oldMarkets = markets;
  markets = data.markets;

  // Loop through each market and update the background color of the cell
  Object.keys(markets).forEach(market => {

    const oldPrice = oldMarkets[market]?.indexPrice;
    const newPrice = markets[market].indexPrice;
    const index_cell = document.getElementById(`price-${market}`);
    const oldOracle = oldMarkets[market]?.oraclePrice;
    const newOracle = markets[market].oraclePrice;
    const oracle_cell = document.getElementById(`oracle-${market}`);
    const signal_cell = document.getElementById(`signal-${market}`);

    if (index_cell && newPrice !== oldPrice) {
      index_cell.style.backgroundColor = newPrice > oldPrice ? "lightgreen" : "tomato";
      setTimeout(() => {
        index_cell.style.transition = "background-color 1s";
        index_cell.style.backgroundColor = "";
      }, 500);
    }

    if (oracle_cell && newOracle !== oldOracle) {
      oracle_cell.style.backgroundColor = newOracle > oldOracle ? "lightgreen" : "tomato";
      setTimeout(() => {
        oracle_cell.style.transition = "background-color 1s";
        oracle_cell.style.backgroundColor = "";
      }, 500);
    }

    if(newOracle < newPrice && document.getElementById(`bybit-ask-${market}`) < newPrice)
    {
      signal_cell.style.backgroundColor = "green" ;
    }
    else if(newOracle > newPrice && document.getElementById(`bybit-bid-${market}`) > newPrice)
    {
      signal_cell.style.backgroundColor = "red" ;
    }
    else
    {
      signal_cell.style.backgroundColor = "#999999" ;
    }


    // signal_cell.style.backgroundColor = newOracle < newPrice && document.getElementById(`bybit-bid-${market}`) < newPrice ? "green" : "red";

  });

  Object.keys(markets).forEach(async (market) => {
    const symbol = market.replace("-", "").replace(" ", "").replace("USD", "USDT");
    const orderbook = await getBybitOrderbook(symbol);
    if (orderbook) {
      const bybit_bid_cell = document.getElementById(`bybit-bid-${market}`);
      const bybit_ask_cell = document.getElementById(`bybit-ask-${market}`);
      const bybit_bidq_cell = document.getElementById(`bybit-bidq-${market}`);
      const bybit_askq_cell = document.getElementById(`bybit-askq-${market}`);



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
    }
  });

  

  // Object.keys(markets).forEach(async (market) => {
  // const dydxOrderbook = await getDydxOrderbook(market);
  // if (dydxOrderbook) {
  //   const dydx_orderbook_cell = document.getElementById(`dydx-orderbook-${market}`);
  //   if (dydx_orderbook_cell) {
  //     dydx_orderbook_cell.textContent = `B: ${dydxOrderbook.b.join(", ")} | A: ${dydxOrderbook.a.join(", ")}`;
  //   }
  // }
  // });

  // Object.keys(markets).forEach(async (market) => {
  // const binanceSymbol = market.replace("-", "").replace(" ", "").replace("USD", "USDT");
  // const binanceOrderbook = await getBinanceOrderbook(binanceSymbol);

  // if (binanceOrderbook) {
  //   const binance_orderbook_cell = document.getElementById(`binance-orderbook-${market}`);
  //   if (binance_orderbook_cell) {
  //     binance_orderbook_cell.textContent = `B: ${binanceOrderbook.b.join(", ")} | A: ${binanceOrderbook.a.join(", ")}`;
  //   }
  // }
  // });
}

setInterval(updateData, 1000);

</script>

<main in:fade>

<Logo/>
<h1>L2 Research</h1>
<h2>real-time cryptocurrency trend analysis</h2>
<h2>with decentralized and centralized data</h2>
<h2 class="g">Green = 📈<h2><h2 class="r">Red = 📉<h2>
<br>

{#if Object.keys(markets).length > 0}
  <table>
    <thead in:fade>
      <tr>
        <th>Market</th>
        <th>Index Price</th>
        <th>Oracle Price</th>
        <!-- <th>Price Change 24H</th>
        <th>Volume 24H</th>
        <th>Open Interest</th> -->
        <th>Orderbook Bid</th>
        <th>Bid Quantity</th>
        <th>Orderbook Ask</th>
        <th>Ask Quantity</th>
        <!-- <th>DYDX Orderbook</th>
        <th>Binance Orderbook</th> -->
      </tr>
    </thead>
    <tbody in:fade>
      {#each Object.keys(markets) as market}
        <tr>
          <td id={`signal-${market}`}>{markets[market].market}</td>
          <td id={`price-${market}`}>{markets[market].indexPrice}</td>
          <td id={`oracle-${market}`}>{markets[market].oraclePrice}</td>
          <!-- <td>{markets[market].priceChange24H}</td>
          <td>{markets[market].volume24H}</td>
          <td>{markets[market].openInterest}</td> -->
          <td id={`bybit-bid-${market}`}></td>
          <td id={`bybit-bidq-${market}`}></td>
          <td id={`bybit-ask-${market}`}></td>
          <td id={`bybit-askq-${market}`}></td>
          <!-- <td id={`dydx-orderbook-${market}`}></td>
          <td id={`binance-orderbook-${market}`}></td> -->
        </tr>
      {/each}
    </tbody>
  </table>
{/if}
</main>


<style>
h1{
  font-size: 1.4em;
  color : #444444;
}
h2{
  font-size: 1em;
  color : #444444;
}
.r{
  color : red;
}
.g{
  color : green;
}
.gr{
  color : #999999;
}
main{
  width: 100%;
  background: #DDDDDD;
  font-family: 'Nunito Sans';
}
thead{
  color : #111111;
}
table {
  color : #444444;
  width: 100%;
  margin: auto;
  font-size: 0.68em;
  border-collapse: collapse;
}

@media (max-width: 600px) {
  table {
    font-size: 0.59em;
  }
}

</style>