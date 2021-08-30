<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  export let todo;
</script>

<div class="stack-small">
  <div class="c-cb">
    <input
      type="checkbox"
      id="todo-{todo.id}"
      on:click={() => {
        todo.completed = !todo.completed;
      }}
      checked={todo.completed}
    />
    <label for="todo-{todo.id}" class="todo-label">
      {todo.name}
    </label>
  </div>
  <div class="btn-group">
    <button type="button" class="btn">
      Edit
      <span class="visually-hidden">{todo.name}</span>
    </button>
    <button
      type="button"
      on:click={() => dispatch("remove", todo)}
      class="btn btn__danger"
    >
      Delete
      <span class="visually-hidden">{todo.name}</span>
    </button>
  </div>
</div>
