<script>
  let aPrice;
  let bPrice;

  function round(number, decimalPlaces) {
    return Number(Math.round(number + 'e' + decimalPlaces) + 'e-' + decimalPlaces);
  }

  async function fetchData() {
    try {
      const response = await fetch('https://api.bybit.com/v5/market/orderbook?category=spot&limit=1&symbol=BTCUSDT');
      const data = await response.json();
      const { a, b } = data.result;

      aPrice = round(a[0][0] * 1.05);
      bPrice = round(b[0][0] * 0.95);

    } catch (error) {
      console.error('Error fetching data:', error);
    }
  }

  fetchData();
</script>

<main>
  
  <h2 class="fancy" in:fade>L2 Bitcoin Trading</h2>
  {#if aPrice && bPrice}
    <p>{aPrice} {bPrice}</p>
  {:else}
    <p>Loading real-time price data...</p>
  {/if}

</main>