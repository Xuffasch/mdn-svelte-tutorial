<script>
  import { alert } from "../stores/stores";
</script>

{#if $alert}
  <div role="alert" on:click={() => ($alert = "")}>
    <p>{$alert}</p>
  </div>
{/if}

<style>
  div {
    position: fixed;
    right: 0;
    margin-right: 1.5rem;
    margin-left: 1.5rem;
    margin-top: 1rem;
    border-radius: 0.2rem;
    padding: 0.5rem 1.4rem;
    z-index: 100;
    display: flex;
    align-items: center;
    background-color: #565656;
    color: #fff;
    cursor: pointer;
    font-size: 0.875rem;
    font-weight: 700;
    font-size: 1.5rem;
    opacity: 95%;
  }
  div p {
    color: #fff;
  }
  div svg {
    height: 1.6rem;
    fill: currentColor;
    width: 1.4rem;
    margin-right: 0.5rem;
  }
</style>
