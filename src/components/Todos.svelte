<script>
  import NewTodo from "./NewTodo.svelte";
  import FilterButton from "./FilterButton.svelte";
  import Todo from "./Todo.svelte";
  import MoreActions from "./MoreActions.svelte";
  import TodoStatus from "./TodoStatus.svelte";

  export let todos;

  // $: totalTodos = todos.length;

  // let newTodoId;
  // $: {
  //   if (todos.length == 0) {
  //     newTodoId = 1;
  //   } else {
  //     newTodoId = Math.max(...todos.map((t) => t.id)) + 1;
  //   }
  // }

  let newTodoId = todos.length ? Math.max(...todos.map((t) => t.id)) + 1 : 1;

  // $: completedTodos = todos.filter((todo) => todo.completed).length;

  let newTodoName = "";
  $: console.log(newTodoName);

  let filter = "all";
  const filterTodos = (filter, todos) =>
    filter === "active"
      ? todos.filter((t) => !t.completed)
      : filter == "completed"
      ? todos.filter((t) => t.completed)
      : todos;

  function addTodo(newTodoName) {
    todos = [...todos, { id: newTodoId, name: newTodoName, completed: false }];
    newTodoName = "";
  }

  function removeTodo(todo) {
    todos = todos.filter((t) => t.id != todo.id);
  }

  function updateTodo(todo) {
    // search for the updated todo in the list of todos
    const i = todos.findIndex((t) => t.id === todo.id);
    // get the todo old values then apply the todo new values received by the update event
    todos[i] = { ...todos[i], ...todo };
  }

  function checkAllTodos(completed) {
    todos.forEach((t, i) => (todos[i].completed = completed));
  }

  function removeCompletedTodos() {
    todos = todos.filter((t) => !t.completed);
  }
</script>

<h1>Svelte To-do list</h1>

<!-- Form  -->
<div class="todoapp  stack-large">
  <!-- <form on:submit|preventDefault={addTodo}>
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
    <button type="submit" disabled="" class="btn btn __primary btn__lg">
      Add
    </button>
  </form> -->
  <NewTodo autofocus on:addTodo={(e) => addTodo(e.detail)} />

  <!-- Filter on items to display -->
  <!-- <div class="filters btn-group stack-exception">
    <button
      class="btn toggle-btn"
      class:btn__primary={filter === "all"}
      aria-pressed={filter === "all"}
      on:click={() => (filter = "all")}
    >
      <span class="visually-hidden">Show</span>
      <span>All</span>
      <span class="visually-hidden">tasks</span>
    </button>
    <button
      class="btn toggle-btn"
      class:btn__primary={filter === "active"}
      aria-pressed={filter === "active"}
      on:click={() => (filter = "active")}
    >
      <span class="visually-hidden">Show</span>
      <span>Active</span>
      <span class="visually-hidden">tasks</span>
    </button>
    <button
      class="btn toggle-btn"
      class:btn__primary={filter === "completed"}
      aria-pressed={filter === "completed"}
      on:click={() => (filter = "completed")}
    >
      <span class="visually-hidden">Show</span>
      <span>Completed</span>
      <span class="visually-hidden">tasks</span>
    </button>
  </div> -->

  <FilterButton bind:myfilter={filter} />

  <!-- Infos on displayed items -->
  <!-- <h2 id="list-heading">
    {completedTodos} out of {totalTodos} itemps completed
  </h2> -->

  <TodoStatus {todos} />

  <!-- Todo with #each -->
  <ul role="list" class="todo-list stack-large" aria-labelledby="list-heading">
    {#each filterTodos(filter, todos) as todo (todo.id)}
      <li class="todo">
        <!-- <div class="stack-small">
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
              on:click={() => removeTodo(todo)}
              class="btn btn__danger"
            >
              Delete
              <span class="visually-hidden">{todo.name}</span>
            </button>
          </div>
        </div> -->
        <Todo
          {todo}
          on:update={(e) => updateTodo(e.detail)}
          on:remove={(e) => removeTodo(e.detail)}
        />
      </li>
    {:else}
      <li>Nothing to display</li>
    {/each}
  </ul>

  <!-- <div class="btn-group">
    <button type="button" class="btn btn__primary">Check all</button>
    <button type="button" class="btn btn__primary">Remove completed</button>
  </div> -->

  <MoreActions
    {todos}
    on:checkAll={(e) => checkAllTodos(e.detail)}
    on:removeCompleted={removeCompletedTodos}
  />
</div>
