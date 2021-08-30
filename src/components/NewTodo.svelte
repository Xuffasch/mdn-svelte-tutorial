<script>
  import { createEventDispatcher } from "svelte";
  let dispatch = createEventDispatcher();

  let newTodoName;

  const addTodo = () => {
    dispatch("addTodo", newTodoName);
    newTodoName = "";
  };

  const onCancel = () => {
    newTodoName = "";
  };
</script>

<form
  on:submit|preventDefault={addTodo}
  on:keydown={(e) => {
    console.log("pressed key : ", e.key);
    e.key == "Escape" && onCancel();
  }}
>
  <h2 class="label-wrapper">
    <label for="todo-0" class="label__lg">What needs to be done ?</label>
  </h2>
  <input
    type="text"
    id="todo-0"
    bind:value={newTodoName}
    autocomplete="off"
    class="input input__lg"
  />
  <button
    type="submit"
    disabled={!newTodoName}
    class="btn btn __primary btn__lg"
  >
    Add
  </button>
</form>
