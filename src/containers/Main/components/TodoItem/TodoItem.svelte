<script>

  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();
  
  function remove() {
    dispatch('remove', { id });
	}

	function toggleStatus() {
    let newStatus = !complete;
    dispatch('toggle', {
      id,
      newStatus
    });
  }
    
  // #Reactive
  export let id; // document ID
  export let text;
  export let complete;
</script>

<style>
  .text {
    word-wrap: break-word;
  }
  .text.is-complete {
    text-decoration: line-through;
    color: rgb(114, 116, 114);
  }
  .icon {
    background-color: transparent;
    border-width: 0px;
    width: 20px;
  }
  button:not(:disabled):active {
      background-color: transparent;
  }
  button:focus {
    border-color: transparent;
  }
</style>

<li>

<!-- #controlFlow -->
{#if complete}
  <span class="text is-complete">{ text }</span>
  <!-- #EventHandler -->
  <button class="icon" on:click={toggleStatus}>❎</button>
{:else}
  <span class="text">{ text }</span>
  <!-- #EventHandler -->
  <button class="icon" on:click={toggleStatus}>✅</button>
{/if}
<!-- #EventHandler -->
<button class="icon" on:click={remove}>🗑</button>

</li>