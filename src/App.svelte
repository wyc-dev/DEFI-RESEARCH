<script>
  import { onDestroy } from 'svelte';
  import Web3 from 'web3';

  const web3 = new Web3('https://bsc-dataseed1.binance.org:443');
  let latestBlock = null;
  let transactions = [];

  async function fetchLatestBlock() {
    latestBlock = await web3.eth.getBlock('latest');
    transactions = latestBlock.transactions;
  }

  async function fetchTransactionStatus(txHash) {
    const url = `https://mempool.space/api/tx/${txHash}/hex`;
    const res = await fetch(url);
    const data = await res.json();
    return data;
  }

  fetchLatestBlock(); // 頁面載入時獲取最新區塊

  const interval = setInterval(fetchLatestBlock, 1000000); // 每1秒更新一次最新區塊

  onDestroy(() => {
    clearInterval(interval); // 清除定時器
  });
</script>

<h1>Binance Smart Chain最新交易列表</h1>

{#if latestBlock === null}
  <p>正在獲取最新區塊...</p>
{:else}
  <ul>
    {#each transactions as t}
      <li>{t}</li>
      {#await fetchTransactionStatus(t) then status}
        <li>狀態: {status}</li>
      {:catch error}
        <li>獲取交易狀態時出錯: {error.message}</li>
      {/await}
    {/each}
  </ul>
{/if}
