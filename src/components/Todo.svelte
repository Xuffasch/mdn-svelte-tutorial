<script>
  import { tick, createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  import { selectOnFocus } from "../lib/actions";

  export let todo;

  // Initialize internal state variable to manage passed in todo
  let editing = false;

  let name = todo.name;
  let nameEl;

  // edition functions on existing todo

  async function onEdit() {
    editing = true;
  }

  function onToggle() {
    update({ completed: !todo.completed });
  }

  function update(updatedTodo) {
    todo = { ...todo, ...updatedTodo };
    dispatch("update", todo);
  }

  function onRemove() {
    dispatch("remove", todo);
  }

  function onCancel() {
    name = todo.name;
    editing = false;
  }

  function onSave() {
    update({ name: name });
    editing = false;
  }

  const focusOnInit = (node) =>
    node && typeof node.focus === "function" && node.focus();
</script>

<div class="stack-small">
  {#if editing}
    <div class="c-cb">
      <form
        on:submit|preventDefault={onSave}
        class="stack-small"
        on:keydown={(e) => editing.key === "Escape" && onCancel()}
      >
        <div class="form-group">
          <label for="todo-{todo.id}" class="todo-label"
            >New name for `{todo.name}`</label
          >
          <input
            bind:value={name}
            bind:this={nameEl}
            use:selectOnFocus
            use:focusOnInit
            id="todo-{todo.id}"
            type="text"
            autoComplete="off"
            class="todo-text"
          />
        </div>
        <div class="btn-group">
          <button type="button" on:click={onCancel} class="btn todo-cancel">
            Cancel
            <span class="visually-hidden">rename {todo.name}</span>
          </button>
          <button
            type="submit"
            disabled={!name}
            on:click={onSave}
            class="btn btn__danger"
          >
            Save
            <span class="visually-hidden">new name for {todo.name}</span>
          </button>
        </div>
      </form>
    </div>
  {:else}
    <div class="c-cb">
      <input
        type="checkbox"
        id="todo-{todo.id}"
        on:click={onToggle}
        checked={todo.completed}
      />
      <label for="todo-{todo.id}" class="todo-label">
        {todo.name}
      </label>
    </div>
    <div class="btn-group">
      <button type="button" on:click={onEdit} class="btn">
        Edit
        <span class="visually-hidden">{todo.name}</span>
      </button>
      <button type="button" on:click={onRemove} class="btn btn__danger">
        Delete
        <span class="visually-hidden">{todo.name}</span>
      </button>
    </div>
  {/if}
</div>
