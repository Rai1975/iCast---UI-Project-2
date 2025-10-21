<script>
  import { onMount, onDestroy } from 'svelte';
  
  export let signatures = [
    'sig1.png',
    'sig2.svg',
    'sig3.png',
    'sig4.png'
  ];
  
  const quotes = [
    "Every step forward is a step toward healing.",
    "Your strength is greater than any challenge.",
    "Recovery is not a race, it's a journey.",
    "Believe in your body's power to heal.",
    "Each day brings new hope and progress.",
    "You are stronger than you think.",
    "Healing takes time, and that's okay.",
    "Your comeback story starts today.",
    "Progress, not perfection.",
    "Tomorrow is a new beginning."
  ];
  
  let currentTime = new Date();
  let currentQuote = quotes[Math.floor(Math.random() * quotes.length)];
  let currentSigIndex = 0;
  let timeInterval;
  let quoteInterval;
  let sigInterval;
  
  function updateTime() {
    currentTime = new Date();
  }
  
  function changeQuote() {
    currentQuote = quotes[Math.floor(Math.random() * quotes.length)];
  }
  
  function nextSignature() {
    currentSigIndex = (currentSigIndex + 1) % signatures.length;
  }
  
  function formatTime(date) {
    let hours = date.getHours();
    let minutes = date.getMinutes();
    let ampm = hours >= 12 ? 'PM' : 'AM';
    hours = hours % 12;
    hours = hours ? hours : 12;
    minutes = minutes < 10 ? '0' + minutes : minutes;
    return `${hours}:${minutes} ${ampm}`;
  }
  
  function formatDate(date) {
    const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
    const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
    return `${days[date.getDay()]}, ${months[date.getMonth()]} ${date.getDate()}`;
  }
  
  onMount(() => {
    updateTime();
    timeInterval = setInterval(updateTime, 1000);
    quoteInterval = setInterval(changeQuote, 15000); // Change quote every 15 seconds
    sigInterval = setInterval(nextSignature, 5000); // Change signature every 5 seconds
  });
  
  onDestroy(() => {
    if (timeInterval) clearInterval(timeInterval);
    if (quoteInterval) clearInterval(quoteInterval);
    if (sigInterval) clearInterval(sigInterval);
  });
</script>

<div class="screensaver">
  <div class="quote-section">
    <p class="quote">{currentQuote}</p>
  </div>
  
  <div class="time-section">
    <div class="time">{formatTime(currentTime)}</div>
    <div class="date">{formatDate(currentTime)}</div>
  </div>
  
  <div class="signature-section">
    <img src="/assets/signatures/{signatures[currentSigIndex]}" alt="Signature" class="signature-img" />
  </div>
</div>

<style>
  .screensaver {
    min-width: 100%;
    min-height: 100%;
    background: var(--secondary-color);
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    position: relative;
    overflow: hidden;
  }
  
  .screensaver::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 1px;
    background: var(--secondary-color);
    animation: scan 4s linear infinite;
    opacity: 0.3;
  }
  
  @keyframes scan {
    0% { transform: translateY(0); }
    100% { transform: translateY(20vmin); }
  }
  
  .quote-section {
    text-align: center;
    padding: 1vmin;
  }
  
  .quote {
    color: var(--font-color);
    font-size: clamp(10px, 1.5vmin, 18px);
    font-style: italic;
    margin: 0;
    opacity: 0.9;
    line-height: 1.6;
    text-shadow: 0 0 10px rgba(0, 255, 255, 0.3);
    animation: fadeIn 1s ease-in;
  }
  
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(-10px); }
    to { opacity: 0.9; transform: translateY(0); }
  }
  
  .time-section {
    text-align: center;
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  
  .time {
    font-size: clamp(28px, 8vmin, 64px);
    color: var(--font-color);
    font-weight: bold;
    margin-bottom: 1vmin;
    text-shadow: 0 0 20px rgba(0, 255, 255, 0.5);
    font-family: 'Courier New', monospace;
    letter-spacing: 0.1em;
  }
  
  .date {
    font-size: clamp(10px, 1.8vmin, 20px);
    color: var(--font-color);
    opacity: 0.7;
    font-family: 'Courier New', monospace;
  }
  
  .signature-section {
    text-align: center;
    padding: 1vmin;
    min-height: 6vmin;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .signature-img {
    max-width: 50%;
    max-height: 6vmin;
    object-fit: contain;
    filter: invert(1) drop-shadow(0 0 8px rgba(0, 255, 255, 0.3));
    opacity: 0.8;
    animation: sigFade 0.5s ease-in;
  }
  
  @keyframes sigFade {
    from { opacity: 0; }
    to { opacity: 0.8; }
  }
</style>