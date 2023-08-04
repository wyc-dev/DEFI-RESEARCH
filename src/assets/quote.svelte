<script>

  import B from './btc.svelte';

  let sell_btc;
  let buy_btc;

  async function updateBTC() {
    const response = await fetch('https://api.bybit.com/v5/market/orderbook?category=spot&limit=1&symbol=BTCUSDT');
    const data = await response.json();
    const aValue = parseFloat(data.result.a[0][0]);
    const bValue = parseFloat(data.result.b[0][0]);
    const average = Math.floor((aValue + bValue) / 2);
    sell_btc = Math.floor(average * 1.025 * Sell_U);
    buy_btc = Math.floor(average * 0.975 * Buy_U);
  }

  function updateValues() {
    updateBTC();
  }

  setInterval(updateValues, 5000);
</script>

<div in:fade>
  <h4 ><B/>BTC-HKD：</h4>
  <h2 class="fancy" in:fade on:click={()=>{BTC()}}>{buy_btc} / {sell_btc}</h2>
  <h4 >（ Client Selling / Client Buying ）</h4>
</div>
