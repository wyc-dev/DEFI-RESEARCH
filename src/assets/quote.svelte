<script>

import { fade } from 'svelte/transition';

let sell_btc = null;
let buy_btc = null;

function round(number, decimalPlaces) {
  return Number(Math.round(number + 'e' + decimalPlaces) + 'e-' + decimalPlaces);
}

  async function updateBTC() {
    const response = await fetch('https://api.bybit.com/v5/market/orderbook?category=spot&limit=1&symbol=BTCUSDT');
    const data = await response.json();
    const aValue = parseFloat(data.result.a[0][0]);
    const bValue = parseFloat(data.result.b[0][0]);
    const average = Math.floor((aValue + bValue) / 2);
    sell_btc = Math.floor(average * 1.025 * 7.85);
    buy_btc = Math.floor(average * 0.975 * 7.8);
  }

// Initial fetch
updateBTC();
// Set interval to fetch data every 3 seconds (3000 milliseconds)

setInterval(updateBTC, 3000);

</script>

<main>
  <h2 class="fancy" in:fade>L2 Bitcoin Trading</h2>

  <br>
  {#if sell_btc !== null && buy_btc !== null}
    <p class="green">Buying price in $HKD:</p>
    <h2 class="fancy" in:fade>{sell_btc}</h2>
    <p class="red">Selling price in $HKD:</p>
    <h2 class="fancy" in:fade>{buy_btc}</h2>
  {:else}
    <p>Loading real-time price data...</p>
  {/if}
  <br>
  
  <p>(request your desired cryptocurrency)</p>

</main>

<style>
/* Style for red text */
.red {
  color: #FF000099;
}
/* Style for green text */
.green {
  color: #00FF0099;
}
</style>
