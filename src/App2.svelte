<!-- App.svelte -->
<script>
  import { onMount } from 'svelte';
  import { scale } from 'svelte/transition';
  let wbnb_pools = [];
  let weth_pools = [];
  
async function fetchData() {
  const bnb_response = await fetch('https://api.geckoterminal.com/api/v2/networks/bsc/tokens/0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c/pools');
  const eth_response = await fetch('https://api.geckoterminal.com/api/v2/networks/eth/tokens/0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2/pools');
  const bnb_data = await bnb_response.json();
  const eth_data = await eth_response.json();

  }
  
  onMount(() => {
    fetchData();
    setInterval(fetchData, 100000);
  });

  let eth = false;
  let bnb = false;
  let arb = false;

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
  .eth{
    background-color: #8c8c8c;
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


<div on:click={()=>{eth=false;bnb=true;}}>
<img in:scale src="https://cryptologos.cc/logos/bnb-bnb-logo.svg?v=024" alt="BNB Logo" class="bnb-logo" />
<h1>$BNB DEFI VALUE</h1>
</div>

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
    {#each wbnb_pools as pool}
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

<br>
<div on:click={()=>{eth=true;bnb=false;}}>
<img in:scale src="https://cryptologos.cc/logos/ethereum-eth-logo.svg?v=024" alt="ETH Logo" class="bnb-logo" />
<h1>$ETH DEFI VALUE</h1>
</div>

<table>
  <thead in:scale>
    <tr>
      <th class="eth">POOL LP</th>
      <th class="eth">SWAPPING POOL ADDRESS</th>
      <th class="eth">WETH VALUE（USD）</th>
      <th class="eth">TOKEN VALUE（USD）</th>
    </tr>
  </thead>
  <tbody in:scale>
    {#each weth_pools as pool}
      {#if pool.attributes.name.split(' / ')[1] === 'WETH'}
        <tr>
          <td>{pool.attributes.name.split('/')[1]}/{pool.attributes.name.split('/')[0]}</td>
          <td><a href="https://etherscan.io/address/{pool.attributes.address}" target="_blank">Etherscan</a></td>
          <td>{pool.attributes.quote_token_price_usd}</td>
          <td>{pool.attributes.base_token_price_usd}</td>
        </tr>
      {:else if pool.attributes.name.split(' / ')[0] === 'WETH'}
        <tr>
          <td>{pool.attributes.name.split('/')[0]}/{pool.attributes.name.split('/')[1]}</td>
          <td><a href="https://etherscan.io/address/{pool.attributes.address}" target="_blank">Etherscan</a></td>
          <td>{pool.attributes.base_token_price_usd}</td>
          <td>{pool.attributes.quote_token_price_usd}</td>
        </tr>
      {/if}
    {/each}
  </tbody>
</table>


