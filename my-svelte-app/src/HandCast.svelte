<script>
  import HealthStatsPage from "./HealthStatsPage.svelte";
  import SignaturePage from "./SignaturePage.svelte";
  import UVXray from "./UVXray.svelte";
  import ScreenSaver from "./ScreenSaver.svelte";
  import { onMount, onDestroy } from 'svelte';
  import Pressure from "./Pressure.svelte";

  let currentPage = 'health'; // 'health', 'signature', 'uvxray', 'pressure'
  let bpm = 72; // Manage BPM state in parent
  let isLocked = false;
  let idleTimeout;
  const IDLE_TIME = 3000; // 1 minute in milliseconds

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

<div class="hand-cast-container">
    <!-- Using emoji hand as the cast -->
    <div class="cast-hand">
      <span class="hand-emoji">🤚</span>
      <div class="display-screen">
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
            {#if isLocked}
              <ScreenSaver />
            {:else if currentPage === 'health'}
              <HealthStatsPage bind:bpm={bpm} />
            {:else if currentPage === 'signature'}
              <SignaturePage />
            {:else if currentPage === 'uvxray'}
              <UVXray />
            {:else if currentPage === 'pressure'}
              <Pressure />
            {/if}
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="button-div">
    {#if currentPage === 'health'}
    <div class="bottom-controls">
      <button class="control-btn" on:click={decreaseBPM}>-</button>
      <button class="control-btn reset" on:click={resetBPM}>Reset BPM</button>
      <button class="control-btn" on:click={increaseBPM}>+</button>
    </div>
    {:else}
    <div class="bottom-controls">
    </div>
    {/if}
  </div>

<style>x1

.button-div {
  display: flex;
  justify-content: center;
  }
.screensaver-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    z-index: 9999;
    background: black;
  }

  .unlock-hint {
    position: fixed;
    bottom: 2rem;
    left: 50%;
    transform: translateX(-50%);
    color: rgba(0, 255, 255, 0.5);
    font-family: 'Courier New', monospace;
    font-size: 0.9rem;
    z-index: 10000;
    animation: pulse 2s ease-in-out infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 0.3; }
    50% { opacity: 0.8; }
  }

  button {
    margin: .3rem;
  }
  .hand-cast-container {
    position: relative;
    width: 100vw;
    height: 100vh;
    margin: 0;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .cast-hand {
    position: relative;
    transform: translateX(10%) rotate(-270deg);
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  .hand-emoji {
    font-size: 60vmin;
    filter: grayscale(100%) brightness(1.2);
    display: block;
    transform: translate(10%, -10%) scaleX(-1);
  }
  .cast-hand::before {
    content: '';
    position: absolute;
    bottom: -15vmin;
    left: 55%;
    transform: translateX(-50%) translateY(45%);
    width: 38vmin;
    height: 85vmin;
    background: #c9c9c9;
    border-radius: 6vmin;
    box-shadow:
      inset -2px 0 8px rgba(0,0,0,0.2),
      inset 2px 0 8px rgba(255,255,255,0.1);
    z-index: 2;
  }
  .display-screen {
    position: absolute;
    bottom: 5vmin;
    left: 50%;
    transform: translateX(-50%) translateY(70%);
    width: 40vmin;
    height: 22vmin;
    z-index: 10;
  }
  
  .screen-content {
    width: 180%;
    height: 142%;
    background: black;
    border-radius: 8px;
    padding: 2vmin;
    overflow-y: auto;
    transform: rotate(-90deg);
    color: #00ff88;
    font-family: 'Courier New', monospace;
    font-size: clamp(12px, 1.5vmin, 24px);
    box-shadow: inset 0 0 20px rgba(0,255,136,0.1);
    position: relative;
    left: -35%;
    top: 20%;
  }
  .nav-menu {
    display: flex;
    gap: 0;
    margin-bottom: 1rem;
    border-radius: 4px;
    overflow: hidden;
    background: transparent;
  }
  .nav-button {
    flex: 1;
    padding: 0.5rem;
    background: rgba(0, 0, 0, 0.6);
    border: 1px solid #00ffff;
    color: #00ffff;
    font-family: 'Courier New', monospace;
    font-size: 0.8rem;
    cursor: pointer;
    transition: all 0.3s ease;
  }
  .nav-button:not(:last-child) {
    border-right: none;
  }
  .nav-button.active {
    background: #00ffff;
    color: #000;
    box-shadow: 0 0 10px rgba(0, 255, 255, 0.5);
  }
  .nav-button:hover:not(.active) {
    background: rgba(0, 255, 255, 0.1);
  }
  .page-content {
    height: calc(100% - 2.5rem);
    overflow-y: auto;
  }
  
  .bottom-controls {
    position: fixed;
    bottom: 2rem;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    gap: 1rem;
    z-index: 1000;
    pointer-events: all;
  }

  .control-btn {
    padding: 1rem 2rem;
    background: rgba(0, 0, 0, 0.8);
    border: 2px solid #00ffff;
    color: #00ffff;
    font-family: 'Courier New', monospace;
    font-size: 1.2rem;
    font-weight: bold;
    cursor: pointer;
    border-radius: 8px;
    transition: all 0.2s ease;
    min-width: 60px;
  }

  .control-btn:hover {
    background: rgba(0, 255, 255, 0.2);
    box-shadow: 0 0 15px rgba(0, 255, 255, 0.5);
  }

  .control-btn:active {
    transform: scale(0.95);
    box-shadow: 0 0 20px rgba(0, 255, 255, 0.7);
  }

  .control-btn.reset {
    min-width: 150px;
  }

  .control-btn.lock-btn {
    min-width: 150px;
    background: rgba(0, 0, 0, 0.9);
    border-color: #ff9900;
    color: #ff9900;
  }

  .control-btn.lock-btn:hover {
    background: rgba(255, 153, 0, 0.2);
    box-shadow: 0 0 15px rgba(255, 153, 0, 0.5);
  }

  /* Scrollbar styling */
  .screen-content::-webkit-scrollbar,
  .page-content::-webkit-scrollbar {
    width: 4px;
  }
  
  .screen-content::-webkit-scrollbar-track,
  .page-content::-webkit-scrollbar-track {
    background: rgba(0, 0, 0, 0.2);
  }
  
  .screen-content::-webkit-scrollbar-thumb,
  .page-content::-webkit-scrollbar-thumb {
    background: #00ff88;
    border-radius: 2px;
  }
</style>