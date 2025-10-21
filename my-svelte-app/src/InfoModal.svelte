<script>
  export let onClose;
</script>

<div class="popup-overlay" on:click={onClose}>
  <div class="popup-content" on:click|stopPropagation>
    <button class="close-btn" on:click={onClose}>×</button>
    <h2>Control Panel Guide</h2>
    
    <div class="info-section">
      <h3>BPM Controls</h3>
      <ul>
        <li><strong>- Button:</strong> Decrease heart rate by 5 BPM (minimum 40)</li>
        <li><strong>+ Button:</strong> Increase heart rate by 5 BPM (maximum 200)</li>
        <li><strong>Reset BPM:</strong> Reset heart rate to default 72 BPM</li>
        <li><strong>i Button:</strong> Show this information popup</li>
      </ul>
    </div>

    <div class="info-section">
      <h3>Hydration Controls</h3>
      <ul>
        <li><strong>Start Shower:</strong> Begin increasing device humidity level</li>
        <li><strong>Stop:</strong> Stop the humidity increase</li>
        <li><strong>Reset Humidity:</strong> Reset humidity to 50% and clear warnings</li>
      </ul>
    </div>

    <div class="info-section">
      <h3>Auto-Lock Feature</h3>
      <p>The device automatically locks after 10 seconds of inactivity. Any interaction (mouse movement, click, or key press) will unlock the device and reset the timer.</p>
    </div>
  </div>
</div>

<style>
  .popup-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.7);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 10001;
    backdrop-filter: blur(4px);
  }

  .popup-content {
    background: rgba(0, 40, 35, 0.95);
    border: 2px solid rgba(0, 255, 255, 0.5);
    border-radius: 12px;
    padding: 2rem;
    max-width: 600px;
    max-height: 80vh;
    overflow-y: auto;
    color: #00ffaa;
    font-family: 'Courier New', monospace;
    box-shadow: 0 0 30px rgba(0, 255, 255, 0.3);
    position: relative;
  }

  .close-btn {
    position: absolute;
    top: 0.5rem;
    right: 0.5rem;
    background: transparent;
    border: none;
    color: #00ffaa;
    font-size: 2rem;
    cursor: pointer;
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s ease;
  }

  .close-btn:hover {
    color: #00ffff;
    transform: scale(1.1);
  }

  h2 {
    color: #00ffff;
    margin: 0 0 1.5rem 0;
    font-size: 1.5rem;
    text-align: center;
    text-shadow: 0 0 10px rgba(0, 255, 255, 0.5);
  }

  h3 {
    color: #00ffcc;
    margin: 0 0 0.75rem 0;
    font-size: 1.1rem;
  }

  .info-section {
    margin-bottom: 1.5rem;
    padding: 1rem;
    background: rgba(0, 255, 255, 0.05);
    border-radius: 8px;
    border: 1px solid rgba(0, 255, 255, 0.2);
  }

  .info-section:last-child {
    margin-bottom: 0;
  }

  ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  li {
    margin-bottom: 0.5rem;
    padding-left: 1rem;
    position: relative;
  }

  li:before {
    content: "▸";
    position: absolute;
    left: 0;
    color: #00ffff;
  }

  strong {
    color: #00ffff;
  }

  p {
    margin: 0;
    line-height: 1.6;
  }

  /* Scrollbar styling */
  .popup-content::-webkit-scrollbar {
    width: 6px;
  }

  .popup-content::-webkit-scrollbar-track {
    background: rgba(0, 255, 255, 0.1);
    border-radius: 3px;
  }

  .popup-content::-webkit-scrollbar-thumb {
    background: rgba(0, 255, 255, 0.3);
    border-radius: 3px;
  }

  .popup-content::-webkit-scrollbar-thumb:hover {
    background: rgba(0, 255, 255, 0.5);
  }
</style>