<script>
  import HealthStatsPage from "./HealthStatsPage.svelte";
  import SignaturePage from "./SignaturePage.svelte";
  import UVXray from "./UVXray.svelte";
  import ScreenSaver from "./ScreenSaver.svelte";
  import { onMount, onDestroy } from 'svelte';
  import Pressure from "./Pressure.svelte";

  let currentPage = 'health'; // 'health', 'signature', 'uvxray', 'pressure'
  let bpm = 72; // Manage BPM state in parent
  let hydration = 50;
  let isLocked = false;
  let idleTimeout;
  const IDLE_TIME = 10000; // 1 minute in milliseconds

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

  function resetIdleTimer() {
    clearTimeout(idleTimeout);
    if (!isLocked) {
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
    }, 500); // every half second (adjust as you like)
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
    </div>
    <button class="control-btn" on:click={startHydration}>Start Shower</button>
    <button class="control-btn" on:click={stopHydration}>Stop</button>
    <button class="control-btn reset" on:click={resetHydration}>Reset Humidity</button>
    </div>
</div>

<style>
  .main-container {
    display: flex;
    flex-direction: row;
    min-height: 100vh;
  }

  /* make main container align items center so controls vertically center next to screen */
  .main-container {
    align-items: center;
    gap: 1rem;
  }
  @keyframes pulse {
    0%, 100% { opacity: 0.3; }
    50% { opacity: 0.8; }
  }

  button {
    margin: .3rem;
  }

  .bpm-controls {
    width: 100%;
    justify-content: center;
  }
  
  .screen-content {
    background: var(--background-color);
    border-radius: 8px;
    padding: 10px;
    /* Fixed size so it remains the same across scenes */
    width: 720px; /* desktop/tablet fixed width - change as needed */
    height: 310px; /* fixed height - change as needed */
    overflow: hidden; /* prevent outer scroll, use inner scrolling for page-content */
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
    margin-right: 1rem; /* small gap from right edge */
  }

  .control-btn {
    padding: 1rem 2rem;
    background: var(--background-color);
    border: 2px solid #00ffff;
    color: var(--font-color);
    font-family: 'Courier New', monospace;
    font-size: 1.2rem;
    font-weight: bold;
    cursor: pointer;
    border-radius: 8px;
    transition: all 0.2s ease;
    min-width: 60px;
  }

  .control-btn:hover {
    background: var(--background-color);
    box-shadow: 0 0 15px rgba(0, 255, 255, 0.5);
  }

  .control-btn:active {
    transform: scale(0.95);
    box-shadow: 0 0 20px rgba(0, 255, 255, 0.7);
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