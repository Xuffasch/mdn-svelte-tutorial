<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  export let todos;
  $: completedTodos = todos.filter((t) => t.completed).length;

  // follows what check state we want to apply
  let completed = true;

  const checkAll = () => {
    // send the check value we agreed on to apply
    dispatch("checkAll", completed);
    // reverse the state that checkAll function can send the next time;
    completed = !completed;
  };

  const removeCompleted = () => {
    dispatch("removeCompleted");
  };
</script>

<div class="btn-group">
  <button
    type="button"
    disabled={todos.length == 0}
    on:click={checkAll}
    class="btn btn__primary">{completed ? "Check" : "Uncheck"} all</button
  >
  <button
    type="button"
    disabled={completedTodos == 0}
    on:click={removeCompleted}
    class="btn btn__primary">Remove completed</button
  >
</div>
