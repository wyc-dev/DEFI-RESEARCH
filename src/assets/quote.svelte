<script>

let aPrice = null;
let bPrice = null;

function round(number, decimalPlaces) {
  return Number(Math.round(number + 'e' + decimalPlaces) + 'e-' + decimalPlaces);
}

async function fetchData() {
  try {
    const response = await fetch('https://api.bybit.com/v5/market/orderbook?category=spot&limit=1&symbol=BTCUSDT');
    const data = await response.json();
    const { a, b } = data.result;

    aPrice = round(a[0][0] * 1.05 * 7.85, 0); // Round to 0 decimal places
    bPrice = round(b[0][0] * 0.95 * 7.8, 0); // Round to 0 decimal places

  } catch (error) {
    console.error('Error fetching data:', error);
  }
}

fetchData(); // Initial fetch
// Set interval to fetch data every 3 seconds (3000 milliseconds)

setInterval(fetchData, 5000);

</script>

<main>
  <h2 class="fancy" in:fade>L2 Bitcoin Trading</h2>
  {#if aPrice !== null && bPrice !== null}
    <p class="green">Buying price in $HKD:</p>
    <h2 class="fancy" in:fade>{aPrice}</h2>
    <p class="red">Selling price in $HKD:</p>
    <h2 class="fancy" in:fade>{bPrice}</h2>
  {:else}
    <p>Loading real-time price data...</p>
  {/if}

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
