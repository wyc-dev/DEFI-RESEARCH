<script>
import { fade } from 'svelte/transition';
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
<main in:fade>
<Logo/>

{#if Object.keys(markets).length > 0}
  <table>
      <thead in:fade>
      <tr>
        <th>Market</th>
        <th>Index Price</th>
        <th>Oracle Price</th>
        <th>Price Change 24H</th>
        <th>Volume 24H</th>
        <th>Open Interest</th>
      </tr>
    </thead>
    <tbody in:fade>
      {#each Object.keys(markets) as market}
        <tr>
          <td id={`signal-${market}`}>{markets[market].market}</td>
          <td id={`price-${market}`}>{markets[market].indexPrice}</td>
          <td id={`oracle-${market}`}>{markets[market].oraclePrice}</td>
          <td>{markets[market].priceChange24H}</td>
          <td>{markets[market].volume24H}</td>
          <td>{markets[market].openInterest}</td>
        </tr>
      {/each}
    </tbody>
  </table>
{/if}
</main>

<style>

main{
  width: 100%;
  font-family: 'Nunito Sans';
}
thead{
  color : #5E1CEC;
}
td{
  font-size:14px;
}

</style>