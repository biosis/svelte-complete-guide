<script>
  import { createEventDispatcher } from "svelte";
  import { isEmpty, isValidEmail } from "../helpers/validation";

  import Button from "../shared/Button.svelte";
  import Modal from "../shared/Modal.svelte";
  import TextInput from "../shared/TextInput.svelte";

  let title = "";
  let subtitle = "";
  let imageUrl = "";
  let address = "";
  let email = "";
  let description = "";

  const dispatch = createEventDispatcher();

  $: titleValid = !isEmpty(title);
  $: subtitleValid = !isEmpty(subtitle);
  $: imageUrlValid = !isEmpty(imageUrl);
  $: addressValid = !isEmpty(address);
  $: emailValid = isValidEmail(email);
  $: descriptionValid = !isEmpty(description);
  $: formIsValid =
    titleValid &&
    subtitleValid &&
    imageUrlValid &&
    addressValid &&
    emailValid &&
    descriptionValid;

  function submitForm() {
    dispatch("save", {
      title: title,
      subtitle: subtitle,
      description: description,
      imageUrl: imageUrl,
      address: address,
      contactEmail: email,
    });

    title = "";
    subtitle = "";
    description = "";
    imageUrl = "";
    address = "";
    email = "";
  }

  function cancel() {
    dispatch("cancel");
  }
</script>

<style>
  form {
    width: 100%;
  }
</style>

<Modal title="Edit Meetup Data" on:cancel>
  <form on:submit|preventDefault={submitForm}>
    <TextInput
      id="title"
      label="Title"
      valid={titleValid}
      validityMessage="Please enter a valid title."
      value={title}
      on:input={(event) => (title = event.target.value)} />
    <TextInput
      id="subtitle"
      label="Subtitle"
      valid={subtitleValid}
      validityMessage="Please enter a valid subtitle."
      value={subtitle}
      on:input={(event) => (subtitle = event.target.value)} />
    <TextInput
      id="imageUrl"
      label="Image URL"
      valid={imageUrlValid}
      validityMessage="Please enter a valid image URL."
      value={imageUrl}
      on:input={(event) => (imageUrl = event.target.value)} />
    <TextInput
      id="address"
      label="Address"
      valid={addressValid}
      validityMessage="Please enter a valid address."
      value={address}
      on:input={(event) => (address = event.target.value)} />
    <TextInput
      type="email"
      id="email"
      label="E-mail"
      valid={emailValid}
      validityMessage="Please enter a valid email address."
      value={email}
      on:input={(event) => (email = event.target.value)} />
    <TextInput
      controlType="textarea"
      id="description"
      label="Description"
      valid={descriptionValid}
      validityMessage="Please enter a valid descritpion."
      rows="3"
      value={description}
      on:input={(event) => (description = event.target.value)} />
  </form>

  <div slot="footer">
    <Button mode="outline" on:click={cancel}>Close</Button>
    <Button on:click={submitForm} disabled={!formIsValid}>Save</Button>
  </div>
</Modal>
