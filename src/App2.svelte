<!-- App.svelte -->
<script>
  import { onMount } from 'svelte';
  import { scale } from 'svelte/transition';
  let pools = [];
  let eth_pools = [];
  
  async function fetchBNB() {
    const response = await fetch('https://api.geckoterminal.com/api/v2/networks/bsc/tokens/0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c/pools');
    const data = await response.json();
    pools = data.data.filter(pool => pool.attributes.name.includes('WBNB'));
  }
  // async function fetchETH() {
  //   const response = await fetch('https://api.geckoterminal.com/api/v2/networks/eth/tokens/0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2/pools');
  //   const data = await response.json();
  //   eth_pools = data.data.filter(pool => pool.attributes.name.includes('WETH'));
  // }
  
  onMount(() => {
    fetchBNB();
    fetchETH();
    setInterval(fetchBNB, 60000);
  });
  
</script>

<style>
  img{
    height: 4em;
  }

  h1 {
    text-align: center;
    font-size: 2.5em;
    margin-bottom: 14px;
    font-family: 'Nunito Sans';
    color: #8c8c8c;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    background-color: white;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    font-family: 'Nunito Sans';
  }
  
  th, td {
    padding: 16px;
    border: 1px solid #ccc;
    text-align: center;
    font-family: 'Nunito Sans';
    color: #8c8c8c;
  }
  
  th {
    background-color: #f0b90b;
    color: white;
    font-weight: bold;
    font-family: 'Nunito Sans';
  }

  a {
    color: #f0b90b;
    text-decoration: none;
    font-family: 'Nunito Sans';
  }

  a:hover {
    text-decoration: underline;
  }

  @media (max-width: 767px) {
    body {
      padding: 9px;
    }

    h1 {
      font-size: 1.5em;
      margin-bottom: 16px;
    }

    th, td {
      padding: 9px;
      font-size: 0.9em;
    }
  }
</style>

<!-- <img in:scale src="https://cryptologos.cc/logos/ethereum-eth-logo.svg?v=024" alt="ETH Logo" class="bnb-logo" />
<h1>$ETH DEFI VALUE</h1> -->
<img in:scale src="https://cryptologos.cc/logos/bnb-bnb-logo.svg?v=024" alt="BNB Logo" class="bnb-logo" />
<h1>$BNB DEFI VALUE</h1>

<table>
  <thead in:scale>
    <tr>
      <th>POOL LP</th>
      <th>SWAPPING POOL ADDRESS</th>
      <th>WBNB VALUE（USD）</th>
      <th>TOKEN VALUE（USD）</th>
    </tr>
  </thead>
  <tbody in:scale>
    {#each pools as pool}
      {#if pool.attributes.name.split(' / ')[1] === 'WBNB'}
        <tr>
          <td>{pool.attributes.name.split('/')[1]}/{pool.attributes.name.split('/')[0]}</td>
          <td><a href="https://bscscan.com/address/{pool.attributes.address}" target="_blank">BSCscan</a></td>
          <td>{pool.attributes.quote_token_price_usd}</td>
          <td>{pool.attributes.base_token_price_usd}</td>
        </tr>
      {:else if pool.attributes.name.split(' / ')[0] === 'WBNB'}
        <tr>
          <td>{pool.attributes.name.split('/')[0]}/{pool.attributes.name.split('/')[1]}</td>
          <td><a href="https://bscscan.com/address/{pool.attributes.address}" target="_blank">BSCscan</a></td>
          <td>{pool.attributes.base_token_price_usd}</td>
          <td>{pool.attributes.quote_token_price_usd}</td>
        </tr>
      {/if}
    {/each}
  </tbody>
</table>
