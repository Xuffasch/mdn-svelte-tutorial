<script>
  import Todos from "./components/Todos.svelte";
  import "./assets/global.css";
  import Alert from "./components/Alert.svelte";

  let todos = [
    { id: 1, name: "Create a Svelte starter app", completed: true },
    { id: 2, name: "Create your first component", completed: true },
    { id: 3, name: "Complete the rest of the tutorial", completed: false },
  ];
</script>

<Alert />
<Todos {todos} />
