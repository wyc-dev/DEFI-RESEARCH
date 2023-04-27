<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-1031236648197201"
     crossorigin="anonymous">

import { fade } from 'svelte/transition';
import Logo from './l2logo.svelte';
import Twitter from './twitter.svelte';
import Linkedin from './linkedin.svelte';


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
      index_cell.style.backgroundColor = newPrice > oldPrice ? "lightgreen" : "tomato";
      setTimeout(() => {
        index_cell.style.transition = "background-color 1s";
        index_cell.style.backgroundColor = "";
      }, 1000);
    }

    if (oracle_cell && newOracle !== oldOracle) {
      oracle_cell.style.backgroundColor = newOracle > oldOracle ? "lightgreen" : "tomato";
      setTimeout(() => {
        oracle_cell.style.transition = "background-color 1s";
        oracle_cell.style.backgroundColor = "";
      }, 1000);
    }


    // if (newOracle < newPrice) 
    // {
    // if (newPrice > document.getElementById(`bybit-ask-${market}`)) 
    // {signal_cell.style.backgroundColor = "green";}
    // }

    // if (newOracle > newPrice)
    // {
    // if (newPrice < document.getElementById(`bybit-bid-${market}`)) 
    // {signal_cell.style.backgroundColor = "red";}
    // }

  });
}



  async function getDyDxClosePrices() {
    const response = await fetch("https://api.dydx.exchange/v3/stats/");
    const data = await response.json();
    return data.markets;
  }

  function updateCellColor(cell, newValue, oldValue) {
    if (cell && newValue !== oldValue) {
      cell.style.backgroundColor = newValue > oldValue ? "lightgreen" : "tomato";
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
          cell.style.backgroundColor = newValue > oldValue ? "lightgreen" : "tomato";
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
      const spot = parseFloat(document.getElementById(`dydx-close-${market}`).textContent)
      const bybitAsk = parseFloat(document.getElementById(`bybit-ask-${market}`).textContent);
      const bybitBid = parseFloat(document.getElementById(`bybit-bid-${market}`).textContent);

      if (bybitAsk > newPrice && newOracle < newPrice && newOracle > spot) {
        signal_cell.style.backgroundColor = "green";
      } else if (bybitBid < newPrice && newOracle > newPrice && newOracle < spot) {
        signal_cell.style.backgroundColor = "red";
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

<html class="bordered">

<main in:fade>

<head>
  <title>L2 Research - Real-time Cryptocurrency Trend Analysis with DeFi x CeFi data</title>
  <meta name="description" content="L2 Research provides real-time cryptocurrency trend analysis with decentralized and centralized data. Stay updated with the latest market trends and make informed decisions.">
</head>

<br><Logo/><br><br>
<h1>L2 Research</h1>
<h2>real-time cryptocurrency trend analysis</h2>
<h2>with decentralized and centralized data</h2>
<br>

{#if Object.keys(markets).length > 0}
  <table>
    <thead in:fade>
      <tr>
        <th></th>
        <th>DEX Spot</th>
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
        <tr>
          <td class="big" id={`signal-${market}`}>{markets[market].market.replace("-", "").replace(" ", "").replace("USD", "")}</td>
          <td id={`dydx-close-${market}`}></td>
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

<br>
<div><a data-sveltekit-reload href="https://twitter.com/l2research"><Twitter/></a>
<a data-sveltekit-reload href="https://www.linkedin.com/company/91319068/"><Linkedin/></a>
<br>
</div> <br> <h2>🖤 WAGMI ♠️</h2> 

<amp-auto-ads type="adsense"
        data-ad-client="ca-pub-1031236648197201">
</amp-auto-ads>
</main> </html>


<style>
*{
  padding:0px;
  margin:0px;
  font-family: 'Nunito Sans';
  flex-direction: column;
  text-align: center;
  justify-content: center;
}
.big{
  padding: 3px;
  font-size: 1.1em;
  color : white;
}
a{
  margin:9px;
}
h1{
  font-size: 1.4em;
  color : #444444;
}
h2{
  font-size: 1em;
  color : #444444;
}
html{
  padding: 6px;
  left: 50%;
  right:50%;
  background: black;
}
main{
  width: 100%;
  min-height:100vh;
  background: #DDDDDD;
  font-family: 'Nunito Sans';
  border-radius: 90px;
}
thead{
  color : black;
}
table {
  color : #444444;
  width: 100%;
  margin: auto;
  font-size: 0.8em;
  border-collapse: collapse;
}
@media (max-width: 610px) {
  h1{
    font-size: 1.1em;
  }
  h2{
    font-size: 0.8em;
  }
  thead{
    font-size: 1em;
  }
  table {
    font-size: 0.59em;
  }
}
@media (max-width: 390px) {
  h1{
    font-size: 0.86em;
  }
  h2{
    font-size: 0.6em;
  }
  thead{
    font-size: 1em;
  }
  table {
    font-size: 0.41em;
  }
}
.bordered {
  border: 3px solid;
  animation: colorChange 0.5s infinite alternate;
}

@keyframes colorChange {
  0% {
    border-color: red;
  }
  100% {
    border-color: green;
  }
}
</style>