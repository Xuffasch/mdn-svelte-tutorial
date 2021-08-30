<script>
  export let todos;

  $: totalTodos = todos.length;
  $: completedTodos = todos.filter((todo) => todo.completed).length;
</script>

<h2 id="list-heading">{completedTodos} out of {totalTodos} items completed</h2>
