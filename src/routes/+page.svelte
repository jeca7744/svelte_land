<script lang="ts">
  import { fly, scale } from 'svelte/transition';
  import { cubicOut, backOut } from 'svelte/easing';
  import { onMount } from 'svelte';
  import { fade, fly, scale } from 'svelte/transition';


  let showLogo = false;
  let showWelcomeText = false;
  let showScrollText = false;

  // Trigger initial animations after a short delay
  onMount(() => {
    setTimeout(() => {
      showLogo = true;
    }, 2000); // Delay of 2 seconds for logo

    setTimeout(() => {
      showWelcomeText = true;
    }, 4000); // Delay for text after logo appears
  });

  // Handle scroll event to trigger next set of animations
  function handleScroll() {
    const scrollY = window.scrollY;
    if (scrollY > 100) {
      showWelcomeText = false;
      showScrollText = true;
    }
  }

  // Attach scroll listener when component mounts
  onMount(() => {
    window.addEventListener('scroll', handleScroll);
    return () => {
      window.removeEventListener('scroll', handleScroll);
    };
  });
</script>

<!-- Main Logo and Text Animations -->
<section class="hero">
  {#if showLogo}
    <img
      src="/logo.png"
      alt="Telepath Logo"
      class="logo"
      in:fly={{ x: 0, y: 0, duration: 1000, easing: cubicOut }} 
    />
  {/if}

  {#if showWelcomeText}
    <h1
      class="welcome-text"
      in:fly={{ x: -200, duration: 1500, easing: backOut }}
      out:fly={{ x: 200, duration: 800 }}
    >
      Welcome to Telepath
    </h1>
  {/if}
</section>

<!-- Disappearing text and new text that appears on scroll -->
<section class="scroll-text">
  {#if showScrollText}
    <h2 class="scrolling-message" in:fly={{ y: 200, duration: 1500 }} out:fade={{ duration: 800 }}>
      The magic behind your digital clinic
    </h2>
  {/if}
</section>

<style>
  /* General body styles */
  body {
    margin: 0;
    padding: 0;
    font-family: 'Arial', sans-serif;
    overflow-x: hidden;
    background-color: #f0f0f0;
  }

  .hero {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
    text-align: center;
  }

  .logo {
    width: 200px;
    height: 200px;
    transform: rotateZ(360deg); /* Initial rotation for the effect */
  }

  .welcome-text {
    font-size: 4rem;
    font-weight: 900;
    margin-top: 2rem;
  }

  .scroll-text {
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    background-color: #ffffff;
  }

  .scrolling-message {
    font-size: 3.5rem;
    font-weight: bold;
  }
</style>
