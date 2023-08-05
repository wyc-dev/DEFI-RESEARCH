<!-- AnimatedText.svelte -->
<script>
  import { onMount } from 'svelte';

  export let text = '';
  let currentIndex = 0;

  let typedText = '';

  function typeNextLetter() {
    if (currentIndex < text.length) {
      typedText += text[currentIndex];
      currentIndex += 1;
      setTimeout(typeNextLetter, 41); // Adjust the typing speed here (e.g., 100ms for 0.1 second delay)
    }
  }

  // Use onMount to start typing animation after the component is mounted
  onMount(() => {
    typeNextLetter();
  });
</script>

<div>
  {typedText}
</div>

<style>
div{
  width:68vw;
  font-family: 'Nunito Sans', sans-serif;  line-height: 1.4;
}
</style>
