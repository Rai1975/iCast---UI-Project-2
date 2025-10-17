<script>
  export let displayContent = "App Content";
</script>

<div class="hand-cast-container">
  <!-- Using emoji hand as the cast -->
  <div class="cast-hand">
    <span class="hand-emoji">🤚</span>
    <div class="display-screen">
      <div class="screen-bezel">
        <div class="screen-content">
          <slot>
            <p>{displayContent}</p>
          </slot>
        </div>
      </div>
    </div>
  </div>
  
</div>

<style>
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
    transform: rotate(-270deg);
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  .hand-emoji {
    font-size: 60vmin;
    filter: grayscale(100%) brightness(1.2);
    display: block;
  }

  .cast-hand::before {
    content: '';
    position: absolute;
    bottom: -15vmin;
    left: 55%;
    transform: translateX(-50%) translateY(30%);
    width: 30vmin;
    height: 50vmin;
    background: linear-gradient(180deg, #d4c5b0 0%, #c4b5a0 100%);
    border-radius: 6vmin;
    box-shadow:
      inset -2px 0 8px rgba(0,0,0,0.2),
      inset 2px 0 8px rgba(255,255,255,0.1);
    z-index: -1;
  }

  .display-screen {
    position: absolute;
    bottom: 5vmin;
    left: 50%;
    transform: translateX(-40%) translateY(100%);
    width: 25vmin;
    height: 18vmin;
    z-index: 10;
  }
  
  .screen-bezel {
    width: 100%;
    height: 100%;
    background: linear-gradient(145deg, #2a2a2a, #1a1a1a);
    border-radius: 12px;
    transform: rotate(-90deg);
    padding: 1vmin;
    box-shadow: 
      inset 0 2px 4px rgba(0,0,0,0.5),
      0 8px 16px rgba(0,0,0,0.6);
  }
  
  .screen-content {
    width: 100%;
    height: 100%;
    background: linear-gradient(145deg, #1e3a5f, #0f2540);
    border-radius: 8px;
    padding: 2vmin;
    overflow-y: auto;
    color: #00ff88;
    font-family: 'Courier New', monospace;
    font-size: clamp(12px, 1.5vmin, 24px);
    box-shadow: inset 0 0 20px rgba(0,255,136,0.1);
  }
  
  .screen-content p {
    margin: 0;
    text-align: center;
  }
  
  /* Scrollbar styling */
  .screen-content::-webkit-scrollbar {
    width: 4px;
  }
  
  .screen-content::-webkit-scrollbar-track {
    background: rgba(0,0,0,0.2);
  }
  
  .screen-content::-webkit-scrollbar-thumb {
    background: #00ff88;
    border-radius: 2px;
  }


</style>