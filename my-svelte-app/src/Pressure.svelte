<script>
  import { onMount } from "svelte";

  // Current pressure reading
  let currentPressure = 45;

  // Pressure control settings
  let pressureSetting = 45;
  let targetPressure = 45;
  const minPressure = 20;
  const maxPressure = 80;

  // Mock pressure history data
  let pressureHistory = [];

  // Generate mock data
  function generateMockData() {
    const data = [];
    const now = new Date();

    for (let i = 95; i >= 0; i--) {
      const timestamp = new Date(now.getTime() - i * 15 * 60 * 1000); // 15 minutes ago
      const basePressure = 40 + Math.sin(i * 0.1) * 10;
      const randomVariation = (Math.random() - 0.5) * 8; // Random variation
      const pressure = Math.max(
        minPressure,
        Math.min(maxPressure, basePressure + randomVariation)
      );

      data.push({
        time: timestamp,
        pressure: Math.round(pressure * 10) / 10,
      });
    }

    return data;
  }

  // Update current pressure based on setting
  function updatePressure() {
    const diff = pressureSetting - currentPressure;
    const step = diff * 0.1; // Gradual change
    currentPressure = Math.round((currentPressure + step) * 10) / 10;

    // Add new data point to history
    if (pressureHistory.length > 0) {
      pressureHistory.push({
        time: new Date(),
        pressure: currentPressure,
      });

      // Keep only last 96 points
      if (pressureHistory.length > 96) {
        pressureHistory = pressureHistory.slice(-96);
      }
    }
  }

  // Handle pressure adjustments
  function increasePressure() {
    if (targetPressure < maxPressure) {
      targetPressure = Math.min(maxPressure, targetPressure + 5);
    }
  }

  function decreasePressure() {
    if (targetPressure > minPressure) {
      targetPressure = Math.max(minPressure, targetPressure - 5);
    }
  }

  // Apply pressure changes
  function applyPressureChange() {
    pressureSetting = targetPressure;
  }

  // Check if update button should be enabled
  $: canUpdate = targetPressure !== pressureSetting;

  onMount(() => {
    pressureHistory = generateMockData();
    // Simulate real-time pressure updates
    const interval = setInterval(updatePressure, 2000);
    return () => clearInterval(interval);
  });

  // SVG graph dimensions
  const graphHeight = 60;
  const padding = 8;

  // Calculate SVG path for pressure history
  function getPressurePath() {
    if (pressureHistory.length < 2) return "";

    const dataPoints = pressureHistory.slice(-20); // Last 5 hours
    const minPressure = Math.min(...dataPoints.map((d) => d.pressure));
    const maxPressure = Math.max(...dataPoints.map((d) => d.pressure));
    const pressureRange = maxPressure - minPressure || 1;

    // Use percentage-based positioning for responsive width
    const stepX = (100 - (2 * padding)) / (dataPoints.length - 1);

    return dataPoints
      .map((point, index) => {
        const x = padding + index * stepX;
        const y =
          padding +
          ((maxPressure - point.pressure) / pressureRange) *
            (graphHeight - 2 * padding);
        return `${index === 0 ? "M" : "L"} ${x}% ${y}`;
      })
      .join(" ");
  }
</script>

<div class="pressure-container">
  <div class="pressure-control-container">
    <div class="pressure-control">
      <div class="control-header">
        <div class="control-label">Pressure Control</div>
      </div>

      <div class="pressure-adjustment-reading-container">
        <div class="sensor-reading">
          <div class="pressure-value">
            {currentPressure}<span class="unit">kPa</span>
          </div>
          <div class="sensor-label">Current Pressure</div>
        </div>
        <div class="pressure-adjustment">
          <div class="pressure-adjustment-buttons-container">
          <button
            class="arrow-btn up"
            disabled={targetPressure >= maxPressure}
            on:click={increasePressure}
          >
            ▲
          </button>

          <div class="target-pressure">
            <div class="target-value">
              {targetPressure}<span class="unit">kPa</span>
            </div>
            <div class="target-label">Target</div>
          </div>

          <button
            class="arrow-btn down"
            disabled={targetPressure <= minPressure}
            on:click={decreasePressure}
          >
              ▼
            </button>
          </div>
          <button
          class="update-btn"
          class:enabled={canUpdate}
          disabled={!canUpdate}
          on:click={applyPressureChange}
        >
          {canUpdate ? "Update Pressure" : "Current Setting"}
        </button>
        </div>
        
      </div>

      <div class="pressure-graph-container">
        <div class="history-header">
          <div class="history-label">Pressure History</div>
          <div class="time-range">Last 5 Hours</div>
        </div>

         <div class="graph-container">
           <svg height={graphHeight} class="pressure-graph">
            <defs>
              <pattern
                id="grid"
                width="15"
                height="15"
                patternUnits="userSpaceOnUse"
              >
                <path
                  d="M 15 0 L 0 0 0 15"
                  fill="none"
                  stroke="rgba(0,255,255,0.1)"
                  stroke-width="0.5"
                />
              </pattern>
            </defs>
            <rect width="100%" height="100%" fill="url(#grid)" />
             <path
               d={getPressurePath()}
               fill="none"
               stroke="#00ffff"
               stroke-width="2.5"
               stroke-linecap="round"
               stroke-linejoin="round"
               opacity="0.9"
             />

             {#each pressureHistory.slice(-20) as point, index}
               <circle
                 cx={`${padding + index * ((100 - 2 * padding) / 19)}%`}
                 cy={padding +
                   ((Math.max(
                     ...pressureHistory.slice(-20).map((d) => d.pressure)
                   ) -
                     point.pressure) /
                     (Math.max(
                       ...pressureHistory.slice(-20).map((d) => d.pressure)
                     ) -
                       Math.min(
                         ...pressureHistory.slice(-20).map((d) => d.pressure)
                       ) || 1)) *
                     (graphHeight - 2 * padding)}
                 r="2"
                 fill="#00ffff"
                 opacity="0.9"
                 stroke="rgba(0, 255, 255, 0.3)"
                 stroke-width="0.5"
               />
             {/each}
          </svg>

          <div class="graph-info">
            <div class="min-pressure">
              Min: {Math.min(
                ...pressureHistory.slice(-20).map((d) => d.pressure)
              ).toFixed(1)} kPa
            </div>
            <div class="max-pressure">
              Max: {Math.max(
                ...pressureHistory.slice(-20).map((d) => d.pressure)
              ).toFixed(1)} kPa
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  .pressure-container {
    width: 99%;
    height: 100%;
    display: flex;
    flex-direction: column;
    gap: 0.8vmin;
    padding: 0.5vmin;
  }

  .sensor-reading {
    background: var(--secondary-color);
    padding: 1vmin;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    position: relative;
    overflow: hidden;
  }

  .pressure-value {
    font-size: clamp(14px, 2.8vmin, 28px);
    color: var(--font-color);
    font-weight: bold;
    margin-bottom: 0.2vmin;
    text-shadow: 0 0 6px rgba(0, 255, 255, 0.6);
  }

  .unit {
    font-size: clamp(8px, 1.4vmin, 16px);
    opacity: 0.7;
    margin-left: 0.2vmin;
  }

  .sensor-label {
    font-size: clamp(7px, 1.1vmin, 12px);
    color: var(--font-color);
    opacity: 0.7;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    margin-bottom: 0.5vmin;
  }

  .pressure-control {
    background: var(--secondary-color);
    border: 1px solid #00ffff;
    border-radius: 1vmin;
    padding: 1vmin;
    display: flex;
    flex-direction: column;
    gap: 0.8vmin;
  }

  .control-header {
    display: flex;
    align-items: center;
    gap: 0.4vmin;
    margin-bottom: 0.3vmin;
  }

  .control-label {
    font-size: clamp(8px, 1.3vmin, 14px);
    color: var(--font-color);
    font-weight: bold;
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  .pressure-adjustment {
    margin-left: 20%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .pressure-adjustment-buttons-container {
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 1vmin;
  }

  .pressure-adjustment-reading-container {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1vmin;
    margin-bottom: 0.5vmin;
  }

  .arrow-btn {
    width: 3vmin;
    height: 3vmin;
    background: var(--secondary-color);
    border: 1px solid #00ffff;
    border-radius: 50%;
    color: var(--font-color);
    font-size: clamp(10px, 1.8vmin, 18px);
    cursor: pointer;
    transition: all 0.3s ease;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: "Courier New", monospace;
  }

  .arrow-btn:hover:not(:disabled) {
    box-shadow: var(--secondary-color);
    transform: scale(1.1);
  }

  .arrow-btn:disabled {
    opacity: 0.3;
    cursor: not-allowed;
  }

  .target-pressure {
    display: flex;
    flex-direction: column;
    align-items: center;
    min-width: 4vmin;
  }

  .target-value {
    font-size: clamp(12px, 2.2vmin, 22px);
    color: var(--font-color);
    font-weight: bold;
    text-shadow: 0 0 5px rgba(0, 255, 255, 0.5);
  }

  .target-label {
    font-size: clamp(6px, 1vmin, 10px);
    color: var(--font-color);
    opacity: 0.7;
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  .update-btn {
    width: 100%;
    padding: 0.6vmin;
    background: var(--secondary-color);
    border: 1px solid #00ffff;
    border-radius: 0.5vmin;
    color: var(--font-color);
    font-size: clamp(8px, 1.2vmin, 14px);
    cursor: pointer;
    transition: all 0.3s ease;
    font-family: "Courier New", monospace;
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  .update-btn.enabled {
    background: var(--secondary-color);
    box-shadow: var(--secondary-color);
  }

  .update-btn:hover:not(:disabled) {
    box-shadow: var(--secondary-color);
    transform: translateY(-1px);
  }

  .update-btn:disabled {
    opacity: 0.4;
    cursor: not-allowed;
  }

  /* removed unused .pressure-history block */

  .history-header {
    display: flex;
    align-items: center;
    gap: 0.4vmin;
    margin-bottom: 0.3vmin;
  }

  .history-label {
    font-size: clamp(8px, 1.3vmin, 14px);
    color: var(--font-color);
    font-weight: bold;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    flex: 1;
    text-align: left;
  }

  .time-range {
    font-size: clamp(6px, 1vmin, 10px);
    color: var(--font-color);
    opacity: 0.7;
  }

  .graph-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.3vmin;
  }

  .pressure-graph {
    width: 100%;
    border: 1px solid rgba(0, 255, 255, 0.3);
    border-radius: 0.4vmin;
    background: var(--secondary-color);
  }

  .graph-info {
    display: flex;
    justify-content: space-between;
    width: 100%;
    font-size: clamp(8px, 1.2vmin, 14px);
    color: var(--font-color);
    opacity: 0.8;
    font-weight: bold;
  }
</style>
