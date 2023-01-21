<script>
  export let id = "";
  export let name = "";
  export let checked = false;
  export let removeTodo;
  export let editTodo;

  let todoSpanRef;
  let todoInputRef;

  function onNameChange() {
    todoSpanRef.classList.toggle("hidden");
    todoInputRef.classList.toggle("hidden");
    todoInputRef.disabled = !todoInputRef.disabled;
    todoInputRef.focus();
  }
</script>

<main>
  <div class="card w-96 bg-base-100 shadow-xl my-2 mx-auto">
    <div class="card-body flex-row items-center">
      <div class="form-control">
        <label class="cursor-pointer label justify-start">
          <input
            type="checkbox"
            class="checkbox checkbox-success"
            checked={checked}
            on:change={() => {
              checked = !checked;
            }}
          />
          <span bind:this={todoSpanRef} class="label-text pl-4"> {name} </span>
          <input
            disabled
            bind:this={todoInputRef}
            type="text"
            class="input input-sm w-full max-w-xs ml-2 hidden !bg-transparent !border-transparent"
            on:change={(e) => editTodo(e, id)}
            on:blur={onNameChange}
          />
        </label>
      </div>
      <div class="card-actions ml-auto">
        <button
          class="btn btn-square btn-sm btn-warning"
          on:click={onNameChange}
        >
          <i class="fa-solid fa-pen-to-square" />
        </button>
        <button
          class="btn btn-square btn-sm btn-error"
          on:click={removeTodo(id)}
        >
          <i class="fa-solid fa-trash" />
        </button>
      </div>
    </div>
  </div>
</main>
