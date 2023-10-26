<script lang="ts">
  import { onMount } from "svelte";

  let todos = [{
    id: 1,
    text: 'Learn Svelte'
  }];
  let inputValue: string;
  const saveInputValue = () => {
    todos = [...todos, {
      id: todos.length + 1,
      text: inputValue
    }]
    inputValue = ''
  }
  const saveRecommendationValue = () => {
    todos = [...todos, {
      id: todos.length + 1,
      text: recommendation
    }]
    handleRefetch()
    
  }
  const handleDeleteItem = (id: number) => {
    todos = todos.filter(todo => todo.id !== id)
    todos = todos
  }

  const handleRefetch = async () => {
    await fetch("https://www.boredapi.com/api/activity")
      .then(response => response.json())
      .then(data => recommendation = data.activity)
  }

  let recommendation: string; 
  onMount( async () => {
    const response = await fetch("https://www.boredapi.com/api/activity")
    const data = await response.json()
    recommendation = data.activity
  })
</script>

<main>
  <div>
    <input type="text" bind:value={inputValue} placeholder="Add item to the to do">
    <button class="saveToDo" on:click={saveInputValue}>Submit</button>
  </div>
  <ul>
    {#each todos as todo}
      <li id={String(todo.id)}>
        <span class="text-item">{todo.text}</span>
        <button on:click={() => handleDeleteItem(todo.id)}>Delete</button>
      </li>
      
    {/each}
  </ul>
  <b>Need help thinking new to do tasks? Here's one:</b>
  <br/> 
  { recommendation ? recommendation : "loading.." }
  { #if recommendation }
    <button class="saveToDo" on:click={saveRecommendationValue}>Save To Do</button>
  {/if}
</main>

<style>
  main {
    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: center;
    margin: 20px;
  } 
  input {
    width: 60vw;
  }
  ul {
    width: 60%;
    padding: 10px;
  }
  li {
    display: flex;
    justify-content: space-between;
    padding: 5px 15px;
  }
  .text-item {
    margin-right: 10px;
  }
  .saveToDo {
    margin-top: 10px;
  }
</style>
