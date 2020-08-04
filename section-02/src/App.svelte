<script>
  import ContactCard from "./ContactCard.svelte";

  let name = "Max";
  let age = 31;

  let title = "";
  let description = "";
  let image = "";

  // let uppercasename; not required!
  $: uppercaseName = name.toUpperCase();

  $: console.log(name);

  $: if (name === "Maximilian") {
    console.log("It runs!");
    age = 30;
  }

  function incrementAge() {
    age = age + 1;
  }

  function changeName() {
    name = "Maximilian";
  }

  function nameInput(event) {
    const enteredValue = event.target.value;
    name = enteredValue;
  }
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: purple;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <h1>Hello {uppercaseName}, my age is {age}!</h1>
  <button on:click={incrementAge}>Change Age</button>
  <button on:click={changeName}>Change Name</button>
  <!-- <input type="text" value={name} on:input={nameInput} /> -->
  <input type="text" bind:value={name} />
  <input type="text" bind:value={title} />
  <input type="text" bind:value={image} />
  <textarea rows="3" bind:value={description} />

  <ContactCard userName={name} jobTitle={title} {description} {image} />
</main>
