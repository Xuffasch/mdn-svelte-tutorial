<script>
  export let todos;
  let headingEl;

  export const focus = () => {
    headingEl.focus();
  };

  $: totalTodos = todos.length;
  $: completedTodos = todos.filter((todo) => todo.completed).length;
</script>

<!-- tabIndex = -1 means that the element can be focus but cannot be navigated to by tabs -->
<h2 bind:this={headingEl} id="list-heading" tabIndex="-1">
  {completedTodos} out of {totalTodos} items completed
</h2>
