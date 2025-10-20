<script>
  export let recovery = 87;
  export let boneHealth = 92;
  export let bpm = 72;
  export let temperature = 36.8;
  export let hydration = 78;

  let selectedMetric = null;

  function selectMetric(metric) {
    selectedMetric = selectedMetric === metric ? null : metric;
  }
</script>

<div class="health-display">
  <div class="grid">
    <button
      class="metric-card"
      class:selected={selectedMetric === 'recovery'}
      on:click={() => selectMetric('recovery')}
    >
      <div class="icon">↻</div>
      <div class="value">{recovery}<span class="unit">%</span></div>
      <div class="label">Recovery</div>
    </button>

    <button
      class="metric-card"
      class:selected={selectedMetric === 'bpm'}
      on:click={() => selectMetric('bpm')}
    >
      <div class="icon pulse">♥</div>
      <div class="value">{bpm}</div>
      <div class="label">BPM</div>
    </button>

    <button
      class="metric-card"
      class:selected={selectedMetric === 'bone'}
      on:click={() => selectMetric('bone')}
    >
      <div class="icon">🦴</div>
      <div class="value">{boneHealth}<span class="unit">%</span></div>
      <div class="label">Bone</div>
    </button>

    <button
      class="metric-card"
      class:selected={selectedMetric === 'hydration'}
      on:click={() => selectMetric('hydration')}
    >
      <div class="icon">💧</div>
      <div class="value">{hydration}<span class="unit">%</span></div>
      <div class="label">Humidity</div>
    </button>

    <button
      class="metric-card wide"
      class:selected={selectedMetric === 'temp'}
      on:click={() => selectMetric('temp')}
    >
      <div class="icon">🌡️</div>
      <div class="value">{temperature}<span class="unit">°C</span></div>
      <div class="label">Temperature</div>
    </button>
  </div>
</div>

<style>
  .health-display {
    min-width: 100%;
    min-height: 100%;
    display: flex;
    flex-direction: column;
  }

  .grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 0.8vmin;
    width: 100%;
    height: 100%;
  }
  
  .metric-card {
    background: var(--secondary-color);
    border: 1px solid #00ffff;
    border-radius: 1vmin;
    padding: 1vmin;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: all 0.3s ease;
    position: relative;
    overflow: hidden;
  }
  
  .metric-card:hover {
    background: var(--secondary-color);
    border-color: #00ffff;
    box-shadow: 0 0 10px rgba(0, 255, 255, 0.3);
    transform: scale(1.05);
  }

  .metric-card.wide {
    grid-column: 1 / -1;
  }

  .icon {
    font-size: clamp(12px, 2.5vmin, 24px);
    margin-bottom: 0.5vmin;
    filter: grayscale(100%) brightness(0) invert(1);
    opacity: 0.9;
  }

  .value {
    font-size: clamp(14px, 2.2vmin, 26px);
    color: var(--font-color);
    font-weight: bold;
    margin-bottom: 0.3vmin;
    text-shadow: 0 0 5px rgba(0, 255, 255, 0.5);
  }
  
  .unit {
    font-size: clamp(9px, 1.4vmin, 16px);
    opacity: 0.7;
    margin-left: 0.2vmin;
  }
  
  .label {
    font-size: clamp(8px, 1.2vmin, 14px);
    color: var(--font-color);
    opacity: 0.7;
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  .pulse {
    animation: pulse 1.2s ease-in-out infinite;
    filter: none !important;
  }
  
  @keyframes pulse {
    0%, 100% {
      opacity: 1;
      transform: scale(1);
    }
    50% {
      opacity: 0.6;
      transform: scale(0.9);
    }
  }

  @keyframes scan {
    0% { transform: translateY(0); }
    100% { transform: translateY(100%); }
  }
</style>