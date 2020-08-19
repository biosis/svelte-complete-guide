<script>
  import { createEventDispatcher } from "svelte";

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
      value={title}
      on:input={(event) => (title = event.target.value)} />
    <TextInput
      id="subtitle"
      label="Subtitle"
      value={subtitle}
      on:input={(event) => (subtitle = event.target.value)} />
    <TextInput
      id="imageUrl"
      label="Image URL"
      value={imageUrl}
      on:input={(event) => (imageUrl = event.target.value)} />
    <TextInput
      id="address"
      label="Address"
      value={address}
      on:input={(event) => (address = event.target.value)} />
    <TextInput
      type="email"
      id="email"
      label="E-mail"
      value={email}
      on:input={(event) => (email = event.target.value)} />
    <TextInput
      controlType="textarea"
      id="description"
      label="Description"
      rows="3"
      value={description}
      on:input={(event) => (description = event.target.value)} />
  </form>

  <div slot="footer">
    <Button mode="outline" on:click={cancel}>Close</Button>
    <Button on:click={submitForm}>Save</Button>
  </div>
</Modal>
