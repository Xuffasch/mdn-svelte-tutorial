<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

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
  <button type="button" on:click={checkAll} class="btn btn__primary"
    >{completed ? "Check" : "Uncheck"} all</button
  >
  <button type="button" on:click={removeCompleted} class="btn btn__primary"
    >Remove completed</button
  >
</div>
