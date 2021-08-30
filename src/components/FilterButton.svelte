<script>
  export let myfilter = "all";
</script>

<div class="filters btn-group stack-exception">
  <button
    class="btn toggle-btn"
    class:btn__primary={myfilter === "all"}
    aria-pressed={myfilter === "all"}
    on:click={() => (myfilter = "all")}
  >
    <span class="visually-hidden">Show</span>
    <span>All</span>
    <span class="visually-hidden">tasks</span>
  </button>
  <button
    class="btn toggle-btn"
    class:btn__primary={myfilter === "active"}
    aria-pressed={myfilter === "active"}
    on:click={() => (myfilter = "active")}
  >
    <span class="visually-hidden">Show</span>
    <span>Active</span>
    <span class="visually-hidden">tasks</span>
  </button>
  <button
    class="btn toggle-btn"
    class:btn__primary={myfilter === "completed"}
    aria-pressed={myfilter === "completed"}
    on:click={() => (myfilter = "completed")}
  >
    <span class="visually-hidden">Show</span>
    <span>Completed</span>
    <span class="visually-hidden">tasks</span>
  </button>
</div>
