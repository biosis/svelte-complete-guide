<script>
  import { bind } from "svelte/internal";
  import CustomInput from "./CustomInput.svelte";
  import Toggle from "./Toggle.svelte";
  import { isValidEmail } from "./validation";

  let value = "Max";
  let selectedOption = 1;
  let price = 0;
  let agreed;
  let favColor = "blue";
  let favColorChk = ["blue"];
  let singleFavColor = "red";
  let someInputData;
  let someDiv;
  let customInput;
  let enteredEmail = "";
  let formIsValid = false;

  $: console.log(value);
  $: console.log(price);
  $: console.log(agreed);
  $: console.log(favColor);
  $: console.log(favColorChk);
  $: console.log(singleFavColor);
  $: formIsValid = isValidEmail(enteredEmail);

  function setValue(event) {
    value = event.target.value;
  }
  function saveData() {
    //console.log(document.querySelector("#someInputData").value);
    console.log(someInputData.value);
    console.dir(someInputData);
    console.dir(someDiv);
    customInput.empty();
  }
</script>

<style>
  .invalid {
    border: 1px solid red;
  }
</style>

<main>
  <h1>Bindings & Forms</h1>

  <!-- <input type="text" {value} on:input={setValue} /> -->
  <!-- <input type="text" bind:value /> -->

  <CustomInput bind:value bind:this={customInput} />
  <Toggle bind:chosenOption={selectedOption} />

  <input type="number" bind:value={price} />

  <label>
    <input type="checkbox" bind:checked={agreed} /> Agree to terms ?
  </label>

  <h1>Favorite Color</h1>
  <label>
    <input type="radio" name="color" value="green" bind:group={favColor} /> Green
  </label>
  <label>
    <input type="radio" name="color" value="blue" bind:group={favColor} /> Blue
  </label>
  <label>
    <input type="radio" name="color" value="red" bind:group={favColor} /> Red
  </label>

  <h1>Checkbox Group</h1>
  <label>
    <input
      type="checkbox"
      name="colorChk"
      value="green"
      bind:group={favColorChk} /> Green
  </label>
  <label>
    <input
      type="checkbox"
      name="colorChk"
      value="blue"
      bind:group={favColorChk} /> Blue
  </label>
  <label>
    <input
      type="checkbox"
      name="colorChk"
      value="red"
      bind:group={favColorChk} /> Red
  </label>

  <select bind:value={singleFavColor}>
    <option value="green">Green</option>
    <option value="red">Red</option>
    <option value="blue">Blue</option>
  </select>

  <hr />

  <!-- <input type="text" id="someInputData" /> -->
  <input type="text" bind:this={someInputData} />
  <button on:click={saveData}>Save</button>

  <div bind:this={someDiv} />

  <hr />
  <form on:submit|preventDefault>
    <input
      type="email"
      bind:value={enteredEmail}
      class={isValidEmail(enteredEmail) ? '' : 'invalid'} />
    <button type="submit" disabled={!formIsValid}>Save</button>
  </form>
</main>
