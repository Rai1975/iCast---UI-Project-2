<script>
  import { onMount, onDestroy } from 'svelte';
  
  export let signatures = [
    'sig1.png',
    'sig2.svg',
    'sig3.png',
    'sig4.png'
  ];

  export let names = [
    'James',
    'Jacob',
    'Poppy',
    'Slim'
  ]
  
  export let intervalTime = 4000;
  
  let currentIndex = 0;
  let direction = 1; // 1 for left-to-right, -1 for right-to-left
  let isTransitioning = false;
  let interval;
  let loadedImages = {};
  let allImagesLoaded = false;
  
  function nextSignature() {
    if (isTransitioning) return;

    isTransitioning = true;
    
    setTimeout(() => {
      currentIndex = (currentIndex + 1) % signatures.length;
      isTransitioning = false;
    }, 800);
  }
  
  onMount(async () => {
    // Preload all images to check if they exist
    const loadPromises = signatures.map((sig, index) => {
      return new Promise((resolve) => {
        const img = new Image();
        img.onload = () => {
          loadedImages[index] = true;
          resolve(true);
        };
        img.onerror = () => {
          console.warn(`Failed to load signature: ${sig}`);
          loadedImages[index] = false;
          resolve(false);
        };
        img.src = `/assets/signatures/${sig}`;
      });
    });

    await Promise.all(loadPromises);
    allImagesLoaded = true;

    // Check if any images loaded successfully
    const hasLoadedImages = Object.values(loadedImages).some(loaded => loaded);
    if (!hasLoadedImages) {
      console.error('No signature images could be loaded. Check that files exist in /assets/signatures/');
    }

    interval = setInterval(nextSignature, intervalTime);
  });
  
  onDestroy(() => {
    if (interval) clearInterval(interval);
  });
  
  function getSignaturePath(filename) {
    return `/assets/signatures/${filename}`;
  }

  function getNextIndex() {
    return (currentIndex + 1) % signatures.length;
  }
</script>

<div class="screensaver">
  {#if !allImagesLoaded}
    <div class="loading">Loading signatures...</div>
  {:else if !Object.values(loadedImages).some(loaded => loaded)}
    <div class="error">
      <div class="error-title">No Signatures Found</div>
      <div class="error-text">Place signature files in:</div>
      <div class="error-path">/assets/signatures/</div>
      <div class="error-list">
        {#each signatures as sig}
          <div class="file-name">{sig}</div>
        {/each}
      </div>
    </div>
  {:else}
    <div class="signature-container">
      {#each signatures as sig, index}
        {#if loadedImages[index]}
          <div
            class="signature"
            class:active={index === currentIndex}
            class:exit={isTransitioning && index === currentIndex}
            class:enter={isTransitioning && index === getNextIndex()}
          >
            <img src={getSignaturePath(sig)} alt="Signature {index + 1}" />
            <div class="signature-label">{names[index]}</div>
          </div>
        {/if}
      {/each}
    </div>
    
    <div class="dots">
      {#each signatures as _, index}
        {#if loadedImages[index]}
          <div class="dot" class:active={index === currentIndex}></div>
        {/if}
      {/each}
    </div>
  {/if}
</div>

<style>
  .screensaver {
    width: 100%;
    height: 100%;
    background: #000;
    position: relative;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  
  .loading, .error {
    color: #00ffff;
    text-align: center;
    font-size: clamp(10px, 1.5vmin, 18px);
  }

  .error {
    padding: 2vmin;
  }

  .error-title {
    font-size: clamp(12px, 1.8vmin, 20px);
    margin-bottom: 1vmin;
    font-weight: bold;
  }

  .error-text {
    opacity: 0.7;
    margin-bottom: 0.5vmin;
  }

  .error-path {
    color: #00ff88;
    font-family: monospace;
    margin-bottom: 1vmin;
  }

  .error-list {
    margin-top: 1vmin;
    font-size: clamp(8px, 1.2vmin, 14px);
    opacity: 0.5;
  }

  .file-name {
    margin: 0.3vmin 0;
  }

  .signature-container {
    position: relative;
    width: 100%;
    height: 85%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .signature {
    position: absolute;
    width: 90%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 1vmin;
    opacity: 0;
    transform: translateX(100%);
  }
  
  .signature.active {
    opacity: 1;
    transform: translateX(0);
  }

  .signature.exit {
    animation: slideOut 0.8s cubic-bezier(0.4, 0, 0.2, 1) forwards;
  }

  .signature.enter {
    animation: slideIn 0.8s cubic-bezier(0.4, 0, 0.2, 1) forwards;
  }

  @keyframes slideOut {
    from {
      opacity: 1;
      transform: translateX(0);
    }
    to {
      opacity: 0;
      transform: translateX(-100%);
    }
  }

  @keyframes slideIn {
    from {
      opacity: 0;
      transform: translateX(100%);
    }
    to {
      opacity: 1;
      transform: translateX(0);
    }
  }

  .signature img {
    max-width: 100%;
    max-height: 80%;
    object-fit: contain;
    filter: invert(1) drop-shadow(0 0 8px rgba(0, 255, 255, 0.4));
  }
  
  .signature-label {
    color: #00ffff;
    font-size: clamp(8px, 1.2vmin, 14px);
    opacity: 0.6;
    text-transform: uppercase;
    letter-spacing: 0.1em;
  }
  
  .dots {
    display: flex;
    gap: 1vmin;
    padding: 1vmin;
    position: absolute;
    bottom: 1vmin;
  }
  
  .dot {
    width: 1vmin;
    height: 1vmin;
    min-width: 4px;
    min-height: 4px;
    border-radius: 50%;
    background: rgba(0, 255, 255, 0.3);
    transition: all 0.3s ease;
  }
  
  .dot.active {
    background: #00ffff;
    box-shadow: 0 0 8px rgba(0, 255, 255, 0.6);
    transform: scale(1.3);
  }
  
  .screensaver::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 2px;
    background: linear-gradient(90deg, transparent, #00ffff, transparent);
    animation: scan 3s linear infinite;
    opacity: 0.3;
  }
  
  @keyframes scan {
    0% { transform: translateY(0); }
    100% { transform: translateY(15vmin); }
  }
</style>