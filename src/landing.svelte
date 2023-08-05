<script>
  import { fade } from 'svelte/transition';
  import { onMount } from 'svelte';
  import Ani_text from './assets/typing.svelte';
  import Home from './home.svelte';

  const T = 'Greetings, esteemed pioneer.';
  const T1 = 'We are evangelists of New Economy.';
  const T2 = 'Facilitate efficient and stable tradings.';
  const T3 = 'Welcome to join our adventures.';

  let showT1 = false;
  let showT2 = false;
  let showT3 = false;
  let showT4 = false;

  onMount(() => {
    setTimeout(() => {
      showT1 = true;
    }, T.length * 110);

    setTimeout(() => {
      showT2 = true;
    }, (T.length + T1.length) * 100);

    setTimeout(() => {
      showT3 = true;
    }, (T.length + T1.length + T2.length) * 100);

    setTimeout(() => {
      showT4 = true;
    }, (T.length + T1.length + T2.length + T3.length) * 100);
  });
</script>

<html in:fade on:click={()=>{showT4 = true;}}>
{#if showT4}

  <Home />

{:else}

  {#if !showT1 & !showT2 & !showT3}
    <Ani_text text={T} />
  {:else if showT1 & !showT2 & !showT3}
    <p in:fade>{T}</p>
    <Ani_text text={T1} />
  {:else if showT2 & !showT3}
    <p>{T}</p>
    <p in:fade>{T1}</p>
    <Ani_text text={T2} />
  {:else if showT3}
    <p>{T}</p>
    <p>{T1}</p>
    <p in:fade>{T2}</p>
    <Ani_text text={T3} />
  {/if}

{/if}
</html>