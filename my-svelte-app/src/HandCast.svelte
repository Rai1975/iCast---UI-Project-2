<script>
  import HealthStatsPage from "./HealthStatsPage.svelte";
  import SignaturePage from "./SignaturePage.svelte";
  import UVXray from "./UVXray.svelte";
  import ScreenSaver from "./ScreenSaver.svelte";
  import InfoModal from "./InfoModal.svelte";
  import { onMount, onDestroy } from 'svelte';
  import Pressure from "./Pressure.svelte";

  let currentPage = 'health'; // 'health', 'signature', 'uvxray', 'pressure'
  let bpm = 72; // Manage BPM state in parent
  let hydration = 50;
  let isLocked = false;
  let showInfoPopup = false;
  let idleTimeout;
  const IDLE_TIME = 10000;

 function changePage(page) {
    currentPage = page;
    resetIdleTimer();
  }

  function increaseBPM() {
    if (bpm < 200) bpm += 5;
    resetIdleTimer();
  }

  function decreaseBPM() {
    if (bpm > 40) bpm -= 5;
    resetIdleTimer();
  }

  function resetBPM() {
    bpm = 72;
    resetIdleTimer();
  }

  function toggleLock() {
    isLocked = !isLocked;
    if (!isLocked) {
      resetIdleTimer();
    } else {
      clearTimeout(idleTimeout);
    }
  }

  function showInfo() {
    showInfoPopup = true;
    resetIdleTimer();
  }

  function closeInfo() {
    showInfoPopup = false;
  }

  function resetIdleTimer() {
    clearTimeout(idleTimeout);
    if (!isLocked && currentPage != 'uvxray') {
      idleTimeout = setTimeout(() => {
        isLocked = true;
      }, IDLE_TIME);
    }
  }

  // Reset timer on any interaction
  function handleInteraction(e) {
    if (isLocked) {
      // Unlock on any interaction when locked
      isLocked = false;
      resetIdleTimer();
    } else {
      resetIdleTimer();
    }
  }

  let hydrationInterval = null;
  let isHydrating = false;
  let hydrationWarning = false;

  function startHydration() {
    if (isHydrating) return; // already running
    isHydrating = true;
    hydrationInterval = setInterval(() => {
      if (hydration < 100) {
        hydration += 1;
      } else {
        hydration = 100;
        hydrationWarning = true;
        stopHydration(); // auto-stop when full
      }
    }, 250); // every half second (adjust as you like)
  }

  function stopHydration() {
    isHydrating = false;
    clearInterval(hydrationInterval);
    hydrationInterval = null;
  }

  function resetHydration() {
    stopHydration();
    hydration = 50;
    hydrationWarning = false;
  }

  onMount(() => {
    resetIdleTimer();
    window.addEventListener('mousemove', handleInteraction);
    window.addEventListener('keydown', handleInteraction);
    window.addEventListener('click', handleInteraction);
    window.addEventListener('touchstart', handleInteraction);
  });

  onDestroy(() => {
    clearTimeout(idleTimeout);
    window.removeEventListener('mousemove', handleInteraction);
    window.removeEventListener('keydown', handleInteraction);
    window.removeEventListener('click', handleInteraction);
    window.removeEventListener('touchstart', handleInteraction);
  });
</script>

<header class="site-header">
  <div class="header-inner">
    <div class="brand">
      <span class="title">iCast</span>
    </div>
    <div class="tag">Medical Interface</div>
  </div>
</header>

<div class="main-container">
  <div class="screen-content">
    <nav class="nav-menu">
      <button
        class="nav-button"
        class:active={currentPage === 'health'}
        on:click={() => changePage('health')}
      >
        Health
      </button>
      <button
        class="nav-button"
        class:active={currentPage === 'signature'}
        on:click={() => changePage('signature')}
      >
        Sign
      </button>
      <button
        class="nav-button"
        class:active={currentPage === 'uvxray'}
        on:click={() => changePage('uvxray')}
      >
        UV/X-Ray
      </button>
      <button
        class="nav-button"
        class:active={currentPage === 'pressure'}
        on:click={() => changePage('pressure')}
      >
        Pressure
      </button>
    </nav>
    <div class="page-content">
        {#if hydrationWarning}
        <div class="hydration-warning">
          🚨 STOP! DEVICE IS TOO WET 🚨
        </div>
        {:else if isLocked}
        <ScreenSaver />
        {:else if currentPage === 'health'}
        <HealthStatsPage bind:bpm={bpm} bind:hydration={hydration} />
        {:else if currentPage === 'signature'}
        <SignaturePage />
        {:else if currentPage === 'uvxray'}
        <UVXray />
        {:else if currentPage === 'pressure'}
        <Pressure />
      {/if}
    </div>
  </div>
  <div class="bottom-controls">
    <div class="bpm-controls">
      <button class="control-btn" on:click={decreaseBPM}>-</button>
      <button class="control-btn reset" on:click={resetBPM}>Reset BPM</button>
      <button class="control-btn" on:click={increaseBPM}>+</button>
      <button class="control-btn" on:click={showInfo}>i</button>
    </div>
    <button class="control-btn" on:click={startHydration}>Start Shower</button>
    <button class="control-btn" on:click={stopHydration}>Stop</button>
    <button class="control-btn reset" on:click={resetHydration}>Reset Humidity</button>
  </div>
</div>

{#if showInfoPopup}
  <InfoModal onClose={closeInfo} />
{/if}

<style>
  .main-container {
    display: flex;
    flex-direction: row;
    min-height: calc(100vh - 64px);
    padding-top: 64px;
    align-items: center;
    gap: 1rem;
    background-image: url('/assets/images/arm.jpg');
    background-size: 125%;
    background-position: center;
    background-repeat: no-repeat;
  }

  /* Site header */
  .site-header {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    height: 64px;
    /* Lighter, semi-transparent teal for a less dark header */
    background: rgba(0, 105, 92, 0.65); /* teal with 65% opacity */
    color: rgb(255, 255, 255);
    display: flex;
    align-items: center;
    box-shadow: 0 1px 4px rgba(0,0,0,0.12);
    z-index: 2000;
    font-family: 'Courier New', monospace;
    backdrop-filter: blur(4px); /* subtle glass effect */
    text-shadow: rgba(0, 0, 0, 0.503) 0.1em 0.1em .2em;
  }

  .site-header .header-inner {
    width: 100%;
    max-width: 1200px;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 1rem;
  }

  .site-header .brand {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    font-weight: bold;
    font-size: 1.5rem;
  }

  .site-header .tag {
    font-size: 1.5rem;
    font-family: 'Courier New', monospace;
    font-weight: bold;
    opacity: 0.9;
  }


  button {
    margin: .3rem;
  }

  .bpm-controls {
    width: 100%;
    justify-content: center;
  }
  
  .screen-content {
    transform: rotate(-7deg);
    background: var(--background-color);
    border-radius: 8px;
    padding: 10px;
    margin-left: 180px;
    margin-bottom: 55px;
    width: 720px;
    height: 310px;
    overflow: hidden;
    color: var(--font-color);
    font-family: 'Courier New', monospace;
    font-size: clamp(12px, 1.5vmin, 24px);
    box-shadow: inset 0 0 20px rgba(0,255,136,0.1);
    position: relative;
    flex: 0 0 auto;
    display: flex;
    flex-direction: column;
  }
  .nav-menu {
    display: flex;
    gap: 0;
    margin-bottom: 1rem;
    border-radius: 4px;
    overflow: hidden;
    background: var(--background-color);
  }
  .nav-button {
    flex: 1;
    padding: 0.5rem;
    background: var(--background-color);
    border: 1px solid #00ffff;
    border-radius: 4px;
    color: var(--font-color);
    font-family: 'Courier New', monospace;
    font-size: 0.8rem;
    cursor: pointer;
    transition: all 0.3s ease;
  }
  .nav-button.active {
    background: var(--background-color);
    color: var(--font-color);
    box-shadow: 0 0 10px rgba(0, 255, 255, 0.5);
  }
  .nav-button:hover:not(.active) {
    background: var(--background-color);
  }
  .page-content {
    /* make page content scroll internally while parent stays fixed */
    flex: 1 1 auto;
    overflow-y: auto;
    height: calc(100% - 2.5rem);
  }
  
  .bottom-controls {
    display: flex;
    flex-direction: column;
    justify-content: center; /* center vertically alongside screen */
    gap: 1rem;
    z-index: 1000;
    pointer-events: all;
    margin-left: auto; /* push controls to the far right of the main container */
    margin-right: 6rem; /* small gap from right edge */
    margin-bottom: 5rem;
    background: rgba(0, 105, 92, 0.60); /* subtle teal tint */
    border-radius: 8px;
    padding: 0.6rem;
    box-shadow: 0 4px 12px rgba(0,0,0,0.08);
    backdrop-filter: blur(6px);
  }

  .control-btn {
    padding: 0.8rem 1.6rem;
    background: rgba(0, 64, 54, 0.3); /* light translucent background */
    border: 1px solid rgba(0, 200, 170, 0.3);
    color: #ffffff;
    font-family: 'Courier New', monospace;
    font-size: 1.05rem;
    font-weight: 700;
    cursor: pointer;
    border-radius: 8px;
    transition: all 0.18s ease;
    min-width: 64px;
    backdrop-filter: blur(3px);
    text-shadow: rgba(0, 0, 0, 0.503) 0.1em 0.1em .2em;
  }

  .control-btn:hover {
    background: rgba(230, 255, 250, 0.18);
    box-shadow: 0 6px 18px rgba(0, 105, 92, 0.12);
  }

  .control-btn:active {
    transform: scale(0.98);
    box-shadow: 0 4px 10px rgba(0, 105, 92, 0.12);
  }

  .control-btn.reset {
    min-width: 150px;
  }
  /* Scrollbar styling */
  .screen-content::-webkit-scrollbar,
  .page-content::-webkit-scrollbar {
    width: 4px;
  }
  
  .screen-content::-webkit-scrollbar-track,
  .page-content::-webkit-scrollbar-track {
    background: var(--background-color); 
  }
  
  .screen-content::-webkit-scrollbar-thumb,
  .page-content::-webkit-scrollbar-thumb {
    background: var(--background-color);
    border-radius: 2px;
  }

    .hydration-warning {
    position: fixed;
    background: red;
    color: white;
    font-family: 'Courier New', monospace;
    font-size: 2rem;
    font-weight: bold;
    padding: 1.5rem 2rem;
    border-radius: 1rem;
    box-shadow: 0 0 20px rgba(255,0,0,0.8);
    z-index: 10000;
    animation: flash 1s infinite;
  }

  @keyframes flash {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.4; }
  }
</style>