<script lang="ts">  
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
  const handleDeleteItem = (id) => {
    todos = todos.filter(todo => todo.id !== id)
    todos = todos
  }
</script>

<main>
  <div>
    <input type="text" bind:value={inputValue} placeholder="Add item to the to do">
    <button on:click={saveInputValue}>Submit</button>
  </div>
  <ul>
    {#each todos as todo}
      <li id={String(todo.id)}>
        <span class="text-item">{todo.text}</span>
        <button on:click={() => handleDeleteItem(todo.id)}>Delete</button>
      </li>
      
    {/each}
  </ul>
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
</style>
