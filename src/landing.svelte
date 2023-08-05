<script>
  import { fade } from 'svelte/transition';
  import { onMount } from 'svelte';
  import Ani_text from './assets/typing.svelte';
  import Home from './home.svelte';

  const T = 'Greetings, esteemed pioneer.';
  const T1 = 'Welcome to our world of innovation and possibilities.';
  const T2 = 'We are not ordinary blockchain development team; we are pioneers of a new economy that is shaping the future of decentralized technology.';
  const T3 = "With expertise in cryptocurrency algorithm research, we're not just a team - we're visionaries who believe in the power of innovation.";
  const T4 = "Our passion lies in creating cutting-edge solutions that can revolutionize the cryptocurrency landscape.";
  const T5 = "Whether you're an experienced trader or a curious novice, our advanced trading robot L2G0 will provide you with efficient and stable trading.";
  const T6 = "Join us on this adventure and explore the new horizons of the crypto world together.";
  const T7 = "Embrace the future of trading with our friendly and professional approach as we embark on a journey of endless possibilities in the world of blockchain and cryptocurrencies.";
  const T8 = "Together, let's navigate the ever-changing cryptocurrency market with confidence and success.";
  const T9 = "Welcome to our innovative community. Let's revolutionize the cryptocurrency landscape together!";


  let showT1 = false;
  let showT2 = false;
  let showT3 = false;
  let showT4 = false;
  let showT5 = false;
  let showT6 = false; // Add new state variables for T6 to T10
  let showT7 = false;
  let showT8 = false;
  let showT9 = false;
  let showT10 = false;

  onMount(() => {
    setTimeout(() => {
      showT1 = true;
    }, T.length * 110);

    setTimeout(() => {
      showT2 = true;
    }, (T.length + T1.length) * 41);

    setTimeout(() => {
      showT3 = true;
    }, (T.length + T1.length + T2.length) * 41);

    setTimeout(() => {
      showT4 = true;
    }, (T.length + T1.length + T2.length + T3.length) * 41);

    setTimeout(() => {
      showT5 = true;
    }, (T.length + T1.length + T2.length + T3.length + T4.length) * 41);

    // Set the timings for showT6 to showT10 using the same pattern as above
    setTimeout(() => {
      showT6 = true;
    }, (T.length + T1.length + T2.length + T3.length + T4.length + T5.length) * 41);

    setTimeout(() => {
      showT7 = true;
    }, (T.length + T1.length + T2.length + T3.length + T4.length + T5.length + T6.length) * 41);

    setTimeout(() => {
      showT8 = true;
    }, (T.length + T1.length + T2.length + T3.length + T4.length + T5.length + T6.length + T7.length) * 41);

    setTimeout(() => {
      showT9 = true;
    }, (T.length + T1.length + T2.length + T3.length + T4.length + T5.length + T6.length + T7.length + T8.length) * 41);

    setTimeout(() => {
      showT10 = true;
    }, (T.length + T1.length + T2.length + T3.length + T4.length + T5.length + T6.length + T7.length + T8.length + T9.length) * 41);
  });
</script>

<html in:fade on:click={() => { showT9 = true; }}>
  {#if showT10}
    <Home />
  {:else}

{#if !showT1 & !showT2 & !showT3 & !showT4 & !showT5 & !showT6 & !showT7 & !showT8 & !showT9}
  <div class="container"> <h2 class="fancy" in:fade><Ani_text text={T} /></h2></div>
  <p>(tap anywhere to skip)</p>
{:else if showT1 & !showT2 & !showT3 & !showT4 & !showT5 & !showT6 & !showT7 & !showT8 & !showT9}
  <p in:fade>{T}</p>
  <div class="container"><h2 class="fancy" in:fade><Ani_text text={T1} /></h2></div>
  <p>(tap anywhere to skip)</p>
{:else if showT2 & !showT3 & !showT4 & !showT5 & !showT6 & !showT7 & !showT8 & !showT9}
  <p>{T}</p>
  <p in:fade>{T1}</p>
  <div class="container"><h2 class="fancy" in:fade><Ani_text text={T2} /></h2></div>
  <p>(tap anywhere to skip)</p>
{:else if showT3 & !showT4 & !showT5 & !showT6 & !showT7 & !showT8 & !showT9}
  <p>{T}</p>
  <p>{T1}</p>
  <p in:fade>{T2}</p>
  <div class="container"><h2 class="fancy" in:fade><Ani_text text={T3} /></h2></div>
  <p>(tap anywhere to skip)</p>
{:else if showT4 & !showT5 & !showT6 & !showT7 & !showT8 & !showT9}
  <p>{T}</p>
  <p>{T1}</p>
  <p>{T2}</p>
  <p in:fade>{T3}</p>
  <div class="container"><h2 class="fancy" in:fade><Ani_text text={T4} /></h2></div>
  <p>(tap anywhere to skip)</p>
{:else if showT5 & !showT6 & !showT7 & !showT8 & !showT9}
  <p>{T}</p>
  <p>{T1}</p>
  <p>{T2}</p>
  <p>{T3}</p>
  <p in:fade>{T4}</p>
  <div class="container"><h2 class="fancy" in:fade><Ani_text text={T5} /></h2></div>
  <p>(tap anywhere to skip)</p>
{:else if showT6 & !showT7 & !showT8 & !showT9}
  <p>{T}</p>
  <p>{T1}</p>
  <p>{T2}</p>
  <p>{T3}</p>
  <p>{T4}</p>
  <p in:fade>{T5}</p>
  <div class="container"><h2 class="fancy" in:fade><Ani_text text={T6} /></h2></div>
  <p>(tap anywhere to skip)</p>
{:else if showT7 & !showT8 & !showT9} <!-- Added new condition for T7 -->
  <p>{T}</p>
  <p>{T1}</p>
  <p>{T2}</p>
  <p>{T3}</p>
  <p>{T4}</p>
  <p>{T5}</p>
  <p in:fade>{T6}</p>
  <div class="container"><h2 class="fancy" in:fade><Ani_text text={T7} /></h2></div>
  <p>(tap anywhere to skip)</p>
{:else if showT8 & !showT9} <!-- Added new condition for T8 -->
  <p>{T}</p>
  <p>{T1}</p>
  <p>{T2}</p>
  <p>{T3}</p>
  <p>{T4}</p>
  <p>{T5}</p>
  <p>{T6}</p>
  <p in:fade>{T7}</p>
  <div class="container"><h2 class="fancy" in:fade><Ani_text text={T8} /></h2></div>
  <p>(tap anywhere to skip)</p>
{:else if showT9} <!-- Added new condition for T9 -->
  <p>{T}</p>
  <p>{T1}</p>
  <p>{T2}</p>
  <p>{T3}</p>
  <p>{T4}</p>
  <p>{T5}</p>
  <p>{T6}</p>
  <p>{T7}</p>
  <p in:fade>{T8}</p>
  <div class="container"><h2 class="fancy" in:fade><Ani_text text={T9} /></h2></div>
  <p>(tap anywhere to skip)</p>
{/if}


  {/if}
</html>

<style>
  .container {
    display: flex;
    justify-content: center; /* Center horizontally */
    width: 100%;
  }
  html {
    height: 100vh;
  }
</style>
