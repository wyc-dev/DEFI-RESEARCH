<svelte:head>
  <script>
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
  </script>
</svelte:head>

<script>



import { fade } from 'svelte/transition';
import Logo from './l2logo.svelte';
import Twitter from './twitter.svelte';

let markets = {};

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

    if (newOracle < newPrice && document.getElementById(`bybit-ask-${market}`) < newPrice) {
      signal_cell.style.backgroundColor = "green";
    } else if (newOracle > newPrice && document.getElementById(`bybit-bid-${market}`) > newPrice) {
      signal_cell.style.backgroundColor = "red";
    } else {
      signal_cell.style.backgroundColor = "#999999";
    }
  });

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
          cell.style.backgroundColor = newValue > oldValue ? "lightgreen" : "tomato";
          setTimeout(() => {
            cell.style.transition = "background-color 1s";
            cell.style.backgroundColor = "";
          }, 500);
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
    }
  });

}

setInterval(updateData, 1000);

</script>

<head>
  <title>L2 Research - Real-time Cryptocurrency Trend Analysis with DeFi x CeFi data</title>
  <meta name="description" content="L2 Research provides real-time cryptocurrency trend analysis with decentralized and centralized data. Stay updated with the latest market trends and make informed decisions.">
</head>

<main in:fade>

<br><Logo/><br><br>
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
        <th>Orderbook Bid</th>
        <th>Bid Quantity</th>
        <th>Orderbook Ask</th>
        <th>Ask Quantity</th>
      </tr>
    </thead>
    <tbody in:fade>
      {#each Object.keys(markets) as market}
        <tr>
          <td id={`signal-${market}`}>{markets[market].market}</td>
          <td id={`price-${market}`}>{markets[market].indexPrice}</td>
          <td id={`oracle-${market}`}>{markets[market].oraclePrice}</td>
          <td id={`bybit-bid-${market}`}></td>
          <td id={`bybit-bidq-${market}`}></td>
          <td id={`bybit-ask-${market}`}></td>
          <td id={`bybit-askq-${market}`}></td>
        </tr>
      {/each}
    </tbody>
  </table>
{/if}

<br><a data-sveltekit-reload href="https://twitter.com/l2research"><Twitter/></a> <br>
</main>


<style>
*{
  padding:0px;
  margin:0px;
  font-family: 'Nunito Sans';
  flex-direction: column;
  text-align: center;
  justify-content: center;
}
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

main{
  width: 100%;
  min-height:100vh;
  background: #CCCCCC;
  font-family: 'Nunito Sans';
}
thead{
  color : #111111;
}
table {
  color : #444444;
  width: 100%;
  margin: auto;
  font-size: 0.8em;
  border-collapse: collapse;
}
@media (max-width: 600px) {
  table {
    font-size: 0.59em;
  }
}
</style>