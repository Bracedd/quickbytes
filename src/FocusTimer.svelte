<script>
  import { onMount } from 'svelte';
  
  let minutes = 25;
  let seconds = 0;
  let timer;
  let isActive = false;
  let isEditing = false;

  onMount(() => {
    const savedMinutes = localStorage.getItem('timerMinutes');
    if (savedMinutes) {
      minutes = parseInt(savedMinutes);
    }
  });

  const startTimer = () => {
    if (isActive) return;
    
    isActive = true;
    timer = setInterval(() => {
      if (seconds === 0) {
        if (minutes === 0) {
          clearInterval(timer);
          isActive = false;
          return;
        }
        minutes--;
        seconds = 59;
      } else {
        seconds--;
      }
    }, 1000);
  };

  const pauseTimer = () => {
    clearInterval(timer);
    isActive = false;
  };

  const resetTimer = () => {
    clearInterval(timer);
    isActive = false;
    minutes = parseInt(localStorage.getItem('timerMinutes')) || 25;
    seconds = 0;
  };

  const handleTimeClick = () => {
    if (!isActive) {
      isEditing = true;
    }
  };

  const handleTimeInput = (event) => {
    const newMinutes = parseInt(event.target.value) || 25;
    minutes = Math.min(Math.max(1, newMinutes), 60); // Limit between 1-60 minutes
    localStorage.setItem('timerMinutes', minutes.toString());
    isEditing = false;
  };
</script>

<div class="focus-timer" class:active={isActive}>
  <div class="content">
    <h2>Focus Timer</h2>
    <div class="timer-controls">
      {#if isEditing}
        <input
          type="number"
          class="time-input"
          value={minutes}
          min="1"
          max="60"
          on:blur={handleTimeInput}
          on:keydown={(e) => e.key === 'Enter' && handleTimeInput(e)}
          autofocus
        />
      {:else}
        <div class="time-display" on:click={handleTimeClick}>
          {String(minutes).padStart(2, "0")}:{String(seconds).padStart(2, "0")}
        </div>
      {/if}
    </div>
    <div class="actions">
      {#if isActive}
        <button class="pause" on:click={pauseTimer}>Pause</button>
      {:else}
        <button class="start" on:click={startTimer}>Start</button>
      {/if}
      <button class="reset" on:click={resetTimer}>Reset</button>
    </div>
  </div>
</div>

<style>
  .focus-timer {
    background: white;
    border-radius: 1rem;
    padding: 2rem;
    height: 100%;
    transition: all 0.2s ease;
    border: 1px solid #eee;
  }

  .focus-timer:hover {
    box-shadow: 0 4px 20px -8px rgba(0,0,0,0.1);
  }

  .active {
    border-color: #000;
  }

  .content {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
  }

  h2 {
    font-size: 1.25rem;
    font-weight: 600;
    letter-spacing: -0.02em;
  }

  .time-display {
    font-size: 3.5rem;
    font-weight: 700;
    font-variant-numeric: tabular-nums;
    letter-spacing: -0.03em;
    cursor: pointer;
    transition: opacity 0.2s ease;
  }

  .time-display:hover {
    opacity: 0.8;
  }

  .time-input {
    font-size: 3.5rem;
    font-weight: 700;
    width: 120px;
    text-align: center;
    border: none;
    border-bottom: 2px solid #000;
    background: transparent;
    padding: 0;
    margin: 0;
  }

  .time-input:focus {
    outline: none;
  }

  /* Remove spinner buttons from number input */
  .time-input::-webkit-inner-spin-button,
  .time-input::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
  
  .time-input[type=number] {
    -moz-appearance: textfield;
  }

  .actions {
    display: flex;
    gap: 0.75rem;
  }

  button {
    flex: 1;
    padding: 0.75rem 1.5rem;
    border-radius: 0.5rem;
    font-size: 0.875rem;
    font-weight: 500;
    transition: all 0.2s ease;
    border: 1px solid #eee;
  }

  .start {
    background: #000;
    color: white;
    border-color: #000;
  }

  .start:hover {
    background: #222;
  }

  .pause {
    background: #fff;
    color: #000;
    border-color: #000;
  }

  .pause:hover {
    background: #f5f5f5;
  }

  .reset {
    background: #fff;
    color: #666;
  }

  .reset:hover {
    background: #f5f5f5;
  }

  .timer-controls {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1rem;
  }
</style>