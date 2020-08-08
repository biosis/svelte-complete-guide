<script>
  import { tick } from "svelte";
  import Product from "./Product.svelte";
  import Modal from "./Modal.svelte";

  let products = [
    {
      id: "p1",
      title: "A book",
      price: 9.99,
    },
  ];

  let showModal = false;
  let closeable = false;

  let text = "This is some dummy text!";

  function addToCart(event) {
    console.log(event.detail);
  }
  function deleteProduct(event) {
    console.log(event.detail);
  }

  function transform(event) {
    if (event.which !== 9) {
      return;
    }
    event.preventDefault();

    const selectionStart = event.target.selectionStart;
    const selectionEnd = event.target.selectionEnd;

    text =
      text.slice(0, selectionStart) +
      text.slice(selectionStart, selectionEnd).toUpperCase() +
      text.slice(selectionEnd);

    tick().then(() => {
      event.target.selectionStart = selectionStart;
      event.target.selectionEnd = selectionEnd;
    });
  }
</script>

<style>

</style>

<main>
  {#each products as product}
    <Product
      {...product}
      on:add-to-cart={addToCart}
      on:delete={deleteProduct} />
  {/each}
  <button on:click={() => (showModal = true)}>Show Modal</button>

  {#if showModal}
    <Modal
      on:cancel={() => (showModal = false)}
      on:close={() => (showModal = false)}
      let:didAgree={closeable}>
      <h1 slot="header">Hello!</h1>
      <p>It works!</p>
      <button
        slot="footer"
        on:click={() => (showModal = false)}
        disabled={!closeable}>
        CONFIRM
      </button>
    </Modal>
  {/if}
</main>

<textarea rows="5" value={text} on:keydown={transform} />
