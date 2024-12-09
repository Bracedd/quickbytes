<script>
  import { onMount } from 'svelte';
  
  let goals = [];
  let completed = [];

  onMount(() => {
    const savedGoals = localStorage.getItem('dailyGoals');
    const savedCompleted = localStorage.getItem('completedGoals');
    
    goals = savedGoals ? JSON.parse(savedGoals) : ["", "", ""];
    completed = savedCompleted ? JSON.parse(savedCompleted) : Array(goals.length).fill(false);
  });

  function updateGoals() {
    localStorage.setItem('dailyGoals', JSON.stringify(goals));
  }

  function toggleComplete(index) {
    completed[index] = !completed[index];
    localStorage.setItem('completedGoals', JSON.stringify(completed));
  }

  function addGoal() {
    goals = [...goals, ""];
    completed = [...completed, false];
    updateGoals();
  }

  function removeGoal(index) {
    goals = goals.filter((_, i) => i !== index);
    completed = completed.filter((_, i) => i !== index);
    updateGoals();
    localStorage.setItem('completedGoals', JSON.stringify(completed));
  }
</script>

<div class="daily-goals">
  <div class="content">
    <div class="header">
      <h2>Today's Goals</h2>
      <button class="add-btn" on:click={addGoal}>
        <i class='bx bx-plus'></i>
      </button>
    </div>
    <div class="goals">
      {#each goals as goal, index}
        <div class="goal-item" class:completed={completed[index]}>
          <button 
            class="complete-btn" 
            on:click={() => toggleComplete(index)}
            class:done={completed[index]}
          >
            <i class='bx bx-check'></i>
          </button>
          <input
            type="text"
            placeholder={`Goal ${index + 1}`}
            bind:value={goals[index]}
            on:input={updateGoals}
          />
          {#if goals.length > 1}
            <button 
              class="remove-btn"
              on:click={() => removeGoal(index)}
            >
              <i class='bx bx-x'></i>
            </button>
          {/if}
        </div>
      {/each}
    </div>
  </div>
</div>

<style>
  .daily-goals {
    background: white;
    border-radius: 1rem;
    padding: 2rem;
    height: 100%;
    transition: all 0.2s ease;
    border: 1px solid #eee;
  }

  .daily-goals:hover {
    box-shadow: 0 4px 20px -8px rgba(0,0,0,0.1);
  }

  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
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

  .goals {
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
  }

  .goal-item {
    display: flex;
    gap: 0.5rem;
    align-items: center;
  }

  input {
    padding: 0.75rem 1rem;
    border-radius: 0.5rem;
    border: 1px solid #eee;
    font-size: 0.875rem;
    font-weight: 500;
    width: 100%;
    transition: all 0.2s ease;
  }

  input::placeholder {
    color: #999;
  }

  input:hover {
    border-color: #000;
  }

  input:focus {
    outline: none;
    border-color: #000;
  }

  .complete-btn {
    width: 28px;
    height: 28px;
    border-radius: 4px;
    border: 1px solid #eee;
    background: white;
    cursor: pointer;
    padding: 0;
    display: grid;
    place-items: center;
    transition: all 0.2s ease;
  }

  .complete-btn i {
    font-size: 1.2rem;
    opacity: 0;
    transition: opacity 0.2s ease;
  }

  .complete-btn.done i {
    opacity: 1;
  }

  .complete-btn.done {
    background: #000;
    color: white;
    border-color: #000;
  }

  .add-btn, .remove-btn {
    background: none;
    border: none;
    color: #666;
    cursor: pointer;
    padding: 0.25rem;
    transition: all 0.2s ease;
  }

  .add-btn {
    font-size: 1.25rem;
    padding: 0.5rem;
  }

  .add-btn:hover, .remove-btn:hover {
    color: #000;
  }

  .remove-btn {
    opacity: 0;
  }

  .goal-item:hover .remove-btn {
    opacity: 1;
  }
</style>