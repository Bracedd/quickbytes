<script>
  import { onMount } from 'svelte';
  
  let mood = "happy";
  let moodNote = "";
  
  const moods = [
    { value: "happy", label: "Happy", emoji: "😊" },
    { value: "neutral", label: "Neutral", emoji: "😐" },
    { value: "sad", label: "Sad", emoji: "😔" }
  ];

  onMount(() => {
    const savedMood = localStorage.getItem('currentMood');
    const savedNote = localStorage.getItem('moodNote');
    
    if (savedMood) mood = savedMood;
    if (savedNote) moodNote = savedNote;
  });

  function updateMood() {
    localStorage.setItem('currentMood', mood);
  }

  function updateNote() {
    localStorage.setItem('moodNote', moodNote);
  }
</script>

<div class="mood-check">
  <div class="content">
    <h2>How are you feeling?</h2>
    <div class="mood-selector">
      <select bind:value={mood} on:change={updateMood}>
        {#each moods as { value, label, emoji }}
          <option {value}>{emoji} {label}</option>
        {/each}
      </select>
    </div>
    <textarea
      placeholder="Add a note about your mood..."
      bind:value={moodNote}
      on:input={updateNote}
    ></textarea>
  </div>
</div>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
  
  .mood-check {
    background: white;
    border-radius: 1rem;
    padding: 2rem;
    height: 100%;
    transition: all 0.2s ease;
    border: 1px solid #eee;
  }
  
  .mood-check:hover {
    box-shadow: 0 4px 20px -8px rgba(0,0,0,0.1);
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
  
  select {
    padding: 0.75rem 1rem;
    border-radius: 0.5rem;
    border: 1px solid #eee;
    font-size: 0.875rem;
    font-weight: 500;
    width: 100%;
    background: white;
    cursor: pointer;
    transition: all 0.2s ease;
    font-family: "Inter", sans-serif;
  }
  
  select:hover {
    border-color: #000;
  }
  
  select:focus {
    outline: none;
    border-color: #000;
  }
  
  textarea {
    width: 100%;
    min-height: 80px;
    padding: 0.75rem 1rem;
    border-radius: 0.5rem;
    border: 1px solid #eee;
    resize: none;
    font-family: inherit;
    font-size: 0.875rem;
    transition: all 0.2s ease;
  }
  
  textarea:hover {
    border-color: #000;
  }
  
  textarea:focus {
    outline: none;
    border-color: #000;
  }
</style>