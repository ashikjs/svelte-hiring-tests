<script>
  import ItemCard from '$lib/components/ItemCard.svelte';
  import {items} from "$lib/data.ts";
</script>

<div class="container">
  <h1>Items List</h1>
  <p class="task">Task: Make these items navigate to their detail pages</p>
  
  <div class="items-grid">
    {#each items as item}
      <ItemCard {item} />
    {/each}
  </div>
</div>

<style>
  .container {
    padding: 20px;
    max-width: 1200px;
    margin: 0 auto;
  }
  
  h1 {
    font-size: 2rem;
    margin-bottom: 1rem;
  }
  
  .task {
    color: #666;
    margin-bottom: 1.5rem;
  }
  
  .items-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 20px;
  }
</style>
