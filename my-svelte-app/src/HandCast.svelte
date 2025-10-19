<script>
  export let displayContent = "App Content";
  import HealthStatsPage from "./HealthStatsPage.svelte";
    import SignaturePage from "./SignaturePage.svelte";
</script>

<div class="hand-cast-container">
  <!-- Using emoji hand as the cast -->
  <div class="cast-hand">
    <span class="hand-emoji">🤚</span>
    <div class="display-screen">
        <div class="screen-content">
          <slot>
            <SignaturePage />
          </slot>
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
    transform: translateX(-41.5%) translateY(70%);
    width: 25vmin;
    height: 18vmin;
    z-index: 10;
  }
  
  .screen-content {
    width: 100%;
    height: 145%;
    background: black;
    border-radius: 8px;
    padding: 2vmin;
    overflow-y: auto;
    transform: rotate(-90deg);
    color: #00ff88;
    font-family: 'Courier New', monospace;
    font-size: clamp(12px, 1.5vmin, 24px);
    box-shadow: inset 0 0 20px rgba(0,255,136,0.1);
  }
  
  /* Scrollbar styling */
  .screen-content::-webkit-scrollbar {
    width: 4px;
  }
  
  .screen-content::-webkit-scrollbar-track {
    background: rgba(0, 0, 0, 0.2);
  }
  
  .screen-content::-webkit-scrollbar-thumb {
    background: #00ff88;
    border-radius: 2px;
  }


</style>