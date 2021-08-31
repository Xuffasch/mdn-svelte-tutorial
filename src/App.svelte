<script>
  import Todos from "./components/Todos.svelte";
  import "./assets/global.css";
  import Alert from "./components/Alert.svelte";

  import { todos } from "./stores/stores";
</script>

<Alert />
<Todos bind:todos={$todos} />
