{#if todos.length === 0}
  <p>No todos yet! Add one below.</p>
{:else}
  <ul>
    {#each todos as todo (todo.id)}
      <li class:completed={todo.completed}>
        <input
          type="checkbox"
          bind:checked={todo.completed}
          on:change={saveTodos}
        />
        <span>{todo.text}</span>
        <button on:click={() => deleteTodo(todo.id)}>Delete</button>
      </li>
    {/each}
  </ul>
{/if}

<form on:submit|preventDefault={addTodo}>
  <input
    bind:value={newTodo}
    placeholder="What needs to be done?"
  />
  <button type="submit">Add Todo</button>
</form>

<script>
  import { onMount } from 'svelte';

  let todos = [];
  let newTodo = '';

  // Load todos from localStorage on component mount
  onMount(() => {
    const savedTodos = localStorage.getItem('svelte-todos');
    if (savedTodos) {
      todos = JSON.parse(savedTodos);
    }
  });

  // Save todos to localStorage
  function saveTodos() {
    localStorage.setItem('svelte-todos', JSON.stringify(todos));
  }

  function addTodo() {
    if (newTodo.trim()) {
      todos = [...todos, {
        id: Date.now(),
        text: newTodo,
        completed: false
      }];
      newTodo = '';
      saveTodos();
    }
  }

  function deleteTodo(id) {
    todos = todos.filter(todo => todo.id !== id);
    saveTodos();
  }
</script>

<style>
  ul {
    list-style: none;
    padding: 0;
  }

  li {
    display: flex;
    align-items: center;
    gap: 1rem;
    padding: 0.5rem;
    border-bottom: 1px solid #eee;
  }

  .completed span {
    text-decoration: line-through;
    color: #888;
  }

  form {
    margin-top: 2rem;
    display: flex;
    gap: 1rem;
  }

  input[type="text"] {
    flex: 1;
    padding: 0.5rem;
    font-size: 1rem;
  }

  button {
    padding: 0.5rem 1rem;
    background-color: #0066cc;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  button:hover {
    background-color: #0052a3;
  }
</style> 