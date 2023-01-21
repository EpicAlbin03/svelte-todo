<script>
  import Todo from "./Todo.svelte";
  import { onMount, afterUpdate } from 'svelte';
  import { v4 as uuidv4 } from "uuid";

  let todos = [
    {
      id: uuidv4(),
      name: "Todo 1",
      checked: false,
    },
  ];

  let inputRef;

  onMount(() => {
    const storedTodos = JSON.parse(localStorage.getItem("todos"));
    if (storedTodos) todos = storedTodos;
  });

  afterUpdate(() => {
    localStorage.setItem("todos", JSON.stringify(todos));
  });

  function addTodo() {
    if (inputRef.value === "") return;
    todos = [
      ...todos,
      { id: uuidv4(), name: inputRef.value, checked: false },
    ];
    inputRef.value = "";
  }

  function clearSelected() {
    todos = todos.filter((todo) => !todo.checked);
  }

  function clearAll() {
    todos = [];
  }

  function editTodo(e, id) {
    const newTodos = [...todos];
    const todo = newTodos.find((todo) => todo.id === id);
    todo.name = e.target.value;
    todos = newTodos;
  }

  function removeTodo(id) {
    todos = todos.filter((todo) => todo.id !== id);
  }
</script>

<main class="flex flex-col justify-center items-center gap-2 pt-32">
  <input
    type="text"
    placeholder="Todo name"
    class="input input-bordered w-full max-w-xs"
    bind:this={inputRef}
  />

  <div>
    <button on:click={addTodo} class="btn btn-success">Add</button>
    <button on:click={clearSelected} class="btn btn-warning"
      >Clear Selected</button
    >
    <button on:click={clearAll} class="btn btn-error">Clear All</button>
  </div>

  {#each todos as todo}
    <Todo
      id={todo.id}
      name={todo.name}
      bind:checked={todo.checked}
      {removeTodo}
      {editTodo}
    />
  {/each}
</main>
