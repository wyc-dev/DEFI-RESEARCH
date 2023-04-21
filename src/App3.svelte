<script>
  import Logo from './Logo.svelte';


let markets = {};

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
        // Fade out the background color after 0.5 second
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

      signal_cell.style.backgroundColor = newOracle < newPrice ? "green" : "red";

    });
  }

  setInterval(updateData, 500);

</script>

<Logo/>

{#if Object.keys(markets).length > 0}
  <table>
      <thead>
      <tr>
        <th>Market</th>
        <!-- <th>Status</th> -->
        <!-- <th>Base Asset</th>
        <th>Quote Asset</th> -->
        <!-- <th>Step Size</th>
        <th>Tick Size</th> -->
        <th>Index Price</th>
        <th>Oracle Price</th>
        <th>Price Change 24H</th>
        <!-- <th>Next Funding Rate</th> -->
        <!-- <th>Next Funding At</th> -->
        <!-- <th>Min Order Size</th> -->
        <!-- <th>Type</th> -->
        <!-- <th>Initial Margin Fraction</th>
        <th>Maintenance Margin Fraction</th>
        <th>Transfer Margin Fraction</th> -->
        <th>Volume 24H</th>
        <th>Trades 24H</th>
        <th>Open Interest</th>
        <!-- <th>Incremental Initial Margin Fraction</th>
        <th>Incremental Position Size</th>
        <th>Max Position Size</th>
        <th>Baseline Position Size</th>
        <th>Asset Resolution</th> -->
        <!-- <th>Synthetic Asset Id</th> -->
      </tr>
    </thead>
    <tbody>
      {#each Object.keys(markets) as market}
        <tr>
          <td id={`signal-${market}`}>{markets[market].market}</td>
          <!-- <td>{markets[market].status}</td> -->
          <!-- <td>{markets[market].baseAsset}</td>
          <td>{markets[market].quoteAsset}</td> -->
          <!-- <td>{markets[market].stepSize}</td>
          <td>{markets[market].tickSize}</td> -->
          <td id={`price-${market}`}>{markets[market].indexPrice}</td>
          <td id={`oracle-${market}`}>{markets[market].oraclePrice}</td>
          <td>{markets[market].priceChange24H}</td>
          <!-- <td>{markets[market].nextFundingRate}</td> -->
          <!-- <td>{markets[market].nextFundingAt}</td> -->
          <!-- <td>{markets[market].minOrderSize}</td> -->
          <!-- <td>{markets[market].type}</td> -->
          <!-- <td>{markets[market].initialMarginFraction}</td>
          <td>{markets[market].maintenanceMarginFraction}</td>
          <td>{markets[market].transferMarginFraction}</td> -->
          <td>{markets[market].volume24H}</td>
          <td>{markets[market].trades24H}</td>
          <td>{markets[market].openInterest}</td>
          <!-- <td>{markets[market].incrementalInitialMarginFraction}</td>
          <td>{markets[market].incrementalPositionSize}</td>
          <td>{markets[market].maxPositionSize}</td>
          <td>{markets[market].baselinePositionSize}</td>
          <td>{markets[market].assetResolution}</td> -->
          <!-- <td>{markets[market].syntheticAssetId}</td> -->
        </tr>
      {/each}
    </tbody>
  </table>
{/if}

<style>

thead{
  color : #5E1CEC;
}
table{
  width:100%;
  font-family: 'Nunito Sans';
}

</style>