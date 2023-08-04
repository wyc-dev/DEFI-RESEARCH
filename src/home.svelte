<script>

import { fade } from 'svelte/transition';
import Logo from './assets/logo.svelte';
import Twitter from './assets/twitter_logo.svelte';
import Linkedin from './assets/linkedin_logo.svelte';
import Linkedin_words from './assets/linkedin.svelte';
import Telegram from './assets/telegram_logo.svelte';
import Bot from './assets/robot_pic.svelte';
import Table from './assets/table.svelte';
import Defi from './assets/defi.svelte';
import Bybit from './assets/bybit_logo.svelte';
import Gitbook from './assets/gitbook.svelte';
import Graph from './assets/graph.svelte';
import Back from './assets/background.svelte';
import Team from './assets/teamwork.svelte';


let page = 1;
const schema = {
"@context": "https://schema.org",
"@type": "WebSite",
"name": "L2 Research",
"url": "https://l2r.xyz//",
"description": "L2 Research provides real-time cryptocurrency trend analysis with decentralized and centralized data. Stay updated with the latest market trends and make informed decisions.",
"publisher": {
"@type": "Organization",
"name": "L2 Research",
"logo": {
"@type": "ImageObject",
"url": "https://i.ibb.co/k0MG7Xs/L2R.png" 
}
}
};

function navigateToBybit() {
  window.location.href = "https://i.bybit.com/1LOab1mc?action=inviteToCopy";
}

function navigateToLinkedin() {
  window.location.href = "https://www.linkedin.com/company/91319068/";
}

function navigateToGitbook() {
  window.location.href = "https://web3msg.gitbook.io/l2g0/";
}
</script>



<html lang="ts" in:fade>

<head>
  <title>L2 Research</title>
  <meta name="description" content="L2 Research provides real-time cryptocurrency trend analysis with decentralized and centralized data. Stay updated with the latest market trends and make informed decisions.">
</head>


{#if page === 1}

<div class="flash" in:fade><Logo/></div>

<h2 in:fade>Bybit x "L2G0"</h2>
<h5 in:fade>Layer 2 research market making Grid bot 0</h5>

<div class="center" in:fade><Bot/></div><br>

<div class="center" in:fade><button on:click={()=>{navigateToBybit();}}>Trade with us on <Bybit/></button></div><br>
<div class="center" in:fade><button on:click={()=>{navigateToGitbook();}}>Study on <black><Gitbook/></black></button></div><br>
<div class="center" in:fade><button on:click={()=>{page = 2;}}>More about L2 Research</button></div>

<br><br>

<div in:fade>
<a  class="bor" data-sveltekit-reload href="https://twitter.com/l2research"><Twitter/></a>
<a  class="bor" data-sveltekit-reload href="https://www.linkedin.com/company/91319068/"><Linkedin/></a>
<a  class="bor" data-sveltekit-reload href="https://t.me/l2research"><Telegram/></a>
<br>
</div> 

<br>

<div class="center" in:fade><p>L2 Research © founded by <a class="bor" data-sveltekit-reload href="https://www.linkedin.com/in/y-c-wong/">YC</a> in 2020</p></div>

<br>

{:else}

<br>

<h2 in:fade>L2 DeFi Research</h2>

<div class="center" in:fade><p>"L2 Research" specializing in researching cryptocurrency market arbitrage strategies, focus on analyzing trends with AI in the cryptocurrency market and conducting practical studies through trading on various exchanges. We aims to provide effective strategies that supporters and interested individuals can follow to place orders on the Bybit exchange conveniently and securely.</p>
</div>

<br>

<div class="center" in:fade><Defi/></div>

<br><br>
<h2 in:fade>CEX & DEX Data Analysis</h2>

<div class="center" in:fade><p>We offers real-time analysis of cryptocurrency trends from top DEX and CEX, enabling users to make informed decisions based on up-to-date data from both decentralized and centralized sources around the verse.</p></div>

<div class="center" in:fade><Graph/></div>


<h2 in:fade>Stay Stable & Sustainable</h2>

<br>

<div class="center" in:fade><p>With years of meticulous research, extensive backtesting, and rigorous real-world testing, our robot utilizes sophisticated Market Making and Market Neutral Strategies commonly employed by hedge funds. "L2G0" designed to offer remarkable stability, has consistently delivered monthly returns ranging from 2% to 13%. We are confident that our product will revolutionize your trading experience, providing you with unparalleled advantages in the dynamic world of digital assets.</p></div>

<div class="center" in:fade><Team/></div>

<div class="center" in:fade><p>With a shared passion for technology, finance, and innovation, our team is a perfect fusion of expertise and talent. Our developers bring years of experience in creating sophisticated trading algorithms, ensuring that our Automated Cryptocurrency Trading Robot remains at the forefront of technological advancements.</p></div>

<br><br><br>

<div class="center" in:fade><button on:click={()=>{navigateToLinkedin();}}>Follow us on <Linkedin_words/></button></div>

<br><br><br><br>

<div class="center"><Table/></div>


<br><br><br>
<div class="center" in:fade><button on:click={()=>{page = 1;}}>Back to "L2G0" page</button></div>

<br><br>

<div>
<a  class="bor" data-sveltekit-reload href="https://twitter.com/l2research"><Twitter/></a>
<a  class="bor" data-sveltekit-reload href="https://www.linkedin.com/company/91319068/"><Linkedin/></a>
<a  class="bor" data-sveltekit-reload href="https://t.me/l2research"><Telegram/></a>
<br>
</div> <br>

<h4>🖤 WAGMI ♠️</h4> 
{/if}

</html>


<style>
h5{
  font-size: 0.68em;
}
.bor{
  margin : 4px;
}
.flash{
  animation: flashing 4s infinite;
  filter: drop-shadow(0 0 3em #06cca9);
}
.center{
  display: flex;
  align-items: center;
  justify-content: center;
  max-width: 420px;
  filter: drop-shadow(0 0 1em #ffffff33);
}

black{
  display: flex;
  align-items: center;
  justify-content: center;
  max-width: 420px;
  filter: drop-shadow(0 0 0.1em #000000);
}
button{
  display: flex;
  align-items: center; /* Vertically center the content */
  justify-content: center; /* Horizontally center the content */
  gap: 0px;
  font-size: 0.8em;
  max-width: 290px;
  min-width: 200px;
  height: 45px;
  border: 1px solid #06cca9;
}
p{
  max-width: 60vw;
  min-width: 200px;
}
img{
  height:250px;
}
.body {
  opacity: 0.4;
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: 9999;
  filter: drop-shadow(0 0 4em #444444);
}
html{
}
</style>