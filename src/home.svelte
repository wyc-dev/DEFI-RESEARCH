<script>

import { fade } from 'svelte/transition';
import Logo from './assets/logo.svelte';
import Twitter from './assets/twitter_logo.svelte';
import Linkedin from './assets/linkedin_logo.svelte';


const schema = {
"@context": "https://schema.org",
"@type": "WebSite",
"name": "L2 Research",
"url": "https://l2-research.vercel.app/",
"description": "Real-time cryptocurrency trend analysis with decentralized and centralized data.",
"publisher": {
"@type": "Organization",
"name": "L2 Research",
"logo": {
"@type": "ImageObject",
"url": "https://i.ibb.co/k0MG7Xs/L2R.png" 
}
}
};


</script>







<html in:fade>

<head>
  <title>L2 Research</title>
  <meta name="description" content="L2 Research provides real-time cryptocurrency trend analysis with decentralized and centralized data. Stay updated with the latest market trends and make informed decisions.">
</head>

<div class="flash"><Logo/></div>

<h2>L2 Research</h2>

<p>We are a company specializing in researching cryptocurrency market arbitrage strategies, focus on analyzing trends in the cryptocurrency market and conducting practical studies through trading on various exchanges. L2 Research aims to provide effective strategies that supporters and interested individuals can follow to place orders on the Bybit exchange conveniently and securely.</p>

<br>

<div><a data-sveltekit-reload href="https://twitter.com/l2research"><Twitter/></a>
<a data-sveltekit-reload href="https://www.linkedin.com/company/91319068/"><Linkedin/></a>
<br>
</div> <br> <h4>🖤 WAGMI ♠️</h4> 


</html>


<style>
.flash{
  animation: flashing 4s infinite;
  filter: drop-shadow(1 1 3em #ffffff);
}
</style>