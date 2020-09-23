<script>
  export let value;

  export function empty() {
    value = "";
  }
</script>

<input type="text" bind:value />
