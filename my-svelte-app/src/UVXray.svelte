<script>
  import { fade } from 'svelte/transition';
  import { onDestroy } from 'svelte';
  
  // Mode state
  let currentMode = 'uv'; // 'uv' or 'xray'
  
  // UV Disinfection state
  let isUVActive = false;
  let isUVCompleted = false;
  let uvTimer = 180; // 3 minutes in seconds
  let uvInterval;
  let uvProgress = 0;
  
  // X-Ray state
  let isXrayActive = false;
  let xrayProgress = 0;
  let xrayInterval;
  let healingProgress = 78; // Simulated healing progress
  
  // Switch between UV and X-ray modes
  function switchMode(mode) {
    if (currentMode !== mode) {
      stopUV();
      stopXray();
      currentMode = mode;
    }
  }
  
  // UV Functions
  function toggleUV() {
    if (isUVActive) {
      stopUV();
    } else {
      startUV();
    }
  }
  
  function startUV() {
    isUVActive = true;
    isUVCompleted = false;
    uvProgress = 0;
    uvTimer = 180;
    
    uvInterval = setInterval(() => {
      uvTimer--;
      uvProgress = ((180 - uvTimer) / 180) * 100;
      
      if (uvTimer <= 0) {
        completeUV();
      }
    }, 1000);
  }
  
  function completeUV() {
    clearInterval(uvInterval);
    uvTimer = 0;
    uvProgress = 100;
    isUVCompleted = true;
    
    // Show completion state for 2 seconds before stopping
    setTimeout(() => {
      stopUV();
    }, 2000);
  }
  
  function stopUV() {
    isUVActive = false;
    isUVCompleted = false;
    clearInterval(uvInterval);
  }
  
  // X-ray Functions
  function toggleXray() {
    if (isXrayActive) {
      stopXray();
    } else {
      startXray();
    }
  }
  
  function startXray() {
    isXrayActive = true;
    xrayProgress = 0;
    
    xrayInterval = setInterval(() => {
      xrayProgress += 2;
      if (xrayProgress >= 100) {
        stopXray();
        // Keep xrayProgress at 100 to show healing status
        xrayProgress = 100;
      }
    }, 50);
  }
  
  function stopXray() {
    isXrayActive = false;
    clearInterval(xrayInterval);
  }
  
  // Format time from seconds to MM:SS
  function formatTime(seconds) {
    const mins = Math.floor(seconds / 60);
    const secs = seconds % 60;
    return `${mins}:${secs.toString().padStart(2, '0')}`;
  }
  
  // Cleanup on component destroy
  onDestroy(() => {
    stopUV();
    stopXray();
  });
</script>

<div class="uvxray-container">
  <div class="status-display">
    <!-- UV Display -->
    {#if !isXrayActive}
      <div class="circle-progress uv-display" class:active={isUVActive} in:fade={{duration: 300}}>
        <div class="progress-circle">
          <svg viewBox="0 0 100 100">
            <circle class="progress-bg" cx="50" cy="50" r="45"/>
            <circle 
              class="progress-bar" 
              cx="50" 
              cy="50" 
              r="45"
              style="stroke-dashoffset: {283 - (uvProgress / 100) * 283}px"
            />
          </svg>
          <div class="timer-display">
            <span class="time">{formatTime(uvTimer)}</span>
            <span class="label">
              {#if isUVCompleted}
                COMPLETE
              {:else if isUVActive}
                CLEANING
              {:else}
                READY
              {/if}
            </span>
          </div>
        </div>
      </div>
    {:else}
      <div class="circle-progress xray-display" class:active={isXrayActive} in:fade={{duration: 300}}>
        <div class="xray-content">
          <div class="xray-image" style="opacity: {xrayProgress/100}">
            <svg class="xray-bone" viewBox="0 0 100 300" fill="none">
              <path d="M40 20C40 8.954 48.954 0 60 0h20c11.046 0 20 8.954 20 20v60c0 11.046-8.954 20-20 20H60c-11.046 0-20-8.954-20-20V20z" fill="#fff"/>
              <rect x="40" y="100" width="60" height="100" fill="#fff"/>
              <path d="M40 280c0 11.046 8.954 20 20 20h20c11.046 0 20-8.954 20-20v-60c0-11.046-8.954-20-20-20H60c-11.046 0-20 8.954-20 20v60z" fill="#fff"/>
            </svg>
          </div>
          {#if isXrayActive}
            <div class="scan-line" style="top: {xrayProgress}%"></div>
          {/if}
          {#if xrayProgress >= 100}
            <div class="healing-status" in:fade={{duration: 300}} out:fade={{duration: 300}}>
              <div class="healing-bar">
                <div class="healing-progress" style="width: {healingProgress}%"></div>
              </div>
              <div class="healing-text">Healing Progress: {healingProgress}%</div>
            </div>
          {/if}
        </div>
      </div>
    {/if}
    
  </div>

  <div class="controls">
    <button 
      class="action-button"
      class:active={isUVActive}
      on:click={toggleUV}
      disabled={isXrayActive || isUVCompleted}
    >
      {#if isUVCompleted}
        COMPLETED
      {:else if isUVActive}
        STOP UV CLEAN
      {:else}
        START UV CLEAN
      {/if}
    </button>
    <button 
      class="action-button"
      class:active={isXrayActive}
      on:click={toggleXray}
      disabled={isUVActive}
    >
      {isXrayActive ? 'STOP' : 'START'} X-RAY
    </button>
  </div>
</div>

<style>
  button {
    margin: .3rem;
  }

  .uvxray-container {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
    padding: 1rem;
    height: 100%;
    color: var(--font-color);
    align-items: center;
    justify-content: center;
  }

  .status-display {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    flex: 1;
    overflow: hidden;
  }

  .circle-progress {
    position: relative;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 1rem;
  }

  .progress-circle {
    position: relative;
    width: 160px;
    height: 160px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  /* UV Display Styles */
  .uv-display {
    background: radial-gradient(circle at center, rgba(0, 255, 255, 0.05), transparent 70%);
  }

  .uv-display svg {
    position: absolute;
    top: 0;
    left: 0;
    transform: rotate(-90deg);
    width: 100%;
    height: 100%;
  }

  .progress-bg, .progress-bar {
    fill: none;
    stroke-width: 4;
  }

  .progress-bg {
    stroke: rgba(0, 255, 255, 0.1);
  }

  .progress-bar {
    stroke: #00ffff;
    stroke-linecap: round;
    stroke-dasharray: 283px;
    transition: stroke-dashoffset 0.3s ease;
  }

  .timer-display {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: center;
    z-index: 2;
  }

  .timer-display .time {
    display: block;
    font-size: 2.5rem;
    font-weight: bold;
    font-family: 'Courier New', monospace;
    letter-spacing: 2px;
    line-height: 1;
    margin-bottom: 0.5rem;
  }

  .timer-display .label {
    display: block;
    font-size: 0.8rem;
    opacity: 0.8;
    letter-spacing: 1px;
    font-family: 'Courier New', monospace;
    text-transform: uppercase;
  }

  /* X-Ray Display Styles */
  .xray-display {
    background: radial-gradient(circle at center, rgba(0, 255, 255, 0.05), transparent 70%);
    border: 1px solid rgba(0, 255, 255, 0.1);
    border-radius: 8px;
  }

  .xray-content {
    position: relative;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .xray-image {
    position: relative;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: opacity 0.3s ease;
  }

  .xray-bone {
    height: 90%;
    width: auto;
    filter: brightness(0.8) contrast(1.2) drop-shadow(0 0 10px rgba(0, 255, 255, 0.3));
  }

  .scan-line {
    position: absolute;
    left: 0;
    width: 100%;
    height: 2px;
    background: #00ffff;
    box-shadow: 
      0 0 10px #00ffff,
      0 0 20px rgba(0, 255, 255, 0.5);
    z-index: 3;
  }

  .healing-status {
    position: absolute;
    bottom: 1rem;
    left: 50%;
    transform: translateX(-50%);
    width: 90%;
    text-align: center;
    background: rgba(0, 0, 0, 0.8);
    padding: 0.75rem;
    border-radius: 4px;
    border: 1px solid rgba(0, 255, 255, 0.5);
    box-shadow: 0 0 15px rgba(0, 255, 255, 0.2);
    z-index: 4;
  }

  .healing-bar {
    width: 100%;
    height: 4px;
    background: rgba(0, 255, 255, 0.2);
    border-radius: 2px;
    margin: 0 auto 0.5rem;
    overflow: hidden;
  }

  .healing-progress {
    height: 100%;
    background: #00ffff;
    box-shadow: 0 0 10px #00ffff;
    transition: width 0.5s ease;
  }

  .healing-text {
    font-size: 0.9rem;
    opacity: 0.9;
    font-family: 'Courier New', monospace;
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  .controls {
    display: flex;
    gap: 0;
    width: 100%;
    border-radius: 4px;
    overflow: hidden;
  }

  .action-button {
    flex: 1;
    padding: 0.5rem;
    background: rgba(0, 0, 0, 0.6);
    border: 1px solid #00ffff;
    color: var(--font-color);
    font-family: 'Courier New', monospace;
    font-size: 0.8rem;
    cursor: pointer;
    transition: all 0.3s ease;
  }

  .action-button:not(:last-child) {
    border-right: none;
  }

  .action-button:hover:not(:disabled):not(.active) {
    background: rgba(0, 255, 255, 0.1);
  }

  .action-button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }

  .action-button.active {
    background: #00ffff;
    color: var(--font-color);
    box-shadow: 0 0 10px rgba(0, 255, 255, 0.5);
  }

  .circle-progress.active {
    animation: glow 2s infinite;
  }

  @keyframes glow {
    0% {
      box-shadow: 0 0 0 0 rgba(0, 255, 255, 0.2);
    }
    70% {
      box-shadow: 0 0 20px 10px rgba(0, 255, 255, 0);
    }
    100% {
      box-shadow: 0 0 0 0 rgba(0, 255, 255, 0);
    }
  }
</style>