<script>
  import { createEventDispatcher } from "svelte";

  export let id;
  export let title;
  export let price;
  export let bestseller = true;

  const dispatch = createEventDispatcher();

  function addToCart() {
    dispatch("add-to-cart", { id: id });
  }
</script>

<style>

</style>

<article>
  <h1>{title}</h1>
  <h2>${price}</h2>
  {#if bestseller}
    <p>BESTSELLER</p>
  {/if}
  <button on:click={addToCart}>Add to cart</button>
  <button on:click={() => dispatch('delete', id)}>Delete</button>
</article>
