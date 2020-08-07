<script>
  import Button from "./shared/Button.svelte";
  import Header from "./shared/Header.svelte";
  import TextInput from "./shared/TextInput.svelte";
  import MeetupGrid from "./meetups/MeetupGrid.svelte";

  let title = "";
  let subtitle = "";
  let imageUrl = "";
  let address = "";
  let email = "";
  let description = "";

  let meetups = [
    {
      id: "m1",
      title: "Coding Bootcamp",
      subtitle: "Learn to code in 2 hours",
      description:
        "In this meetup, we will have some experts that teach you how to code!",
      imageUrl:
        "https://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Caffe_Nero_coffee_bar%2C_High_St%2C_Sutton%2C_Surrey%2C_Greater_London.JPG/800px-Caffe_Nero_coffee_bar%2C_High_St%2C_Sutton%2C_Surrey%2C_Greater_London.JPG",
      address:
        "2 Carshalton Road Unit 1, Old House Inn, Sutton SM1 4RA England",
      contactEmail: "code@test.com",
    },
    {
      id: "m2",
      title: "Swim Together",
      subtitle: "Let's go for some swimming",
      description: "We will simply swim some rounds!",
      imageUrl:
        "https://upload.wikimedia.org/wikipedia/commons/thumb/6/69/Olympic_swimming_pool_%28Tbilisi%29.jpg/800px-Olympic_swimming_pool_%28Tbilisi%29.jpg",
      address:
        "2 Carshalton Road Unit 1, Old House Inn, Sutton SM1 4RA England",
      contactEmail: "swim@test.com",
    },
  ];

  function addMeetup() {
    const newMeetup = {
      id: Math.random().toString(),
      title: title,
      subtitle: subtitle,
      description: description,
      imageUrl: imageUrl,
      address: address,
      contactEmail: email,
    };

    title = "";
    subtitle = "";
    description = "";
    imageUrl = "";
    address = "";
    email = "";

    meetups = [newMeetup, ...meetups];
  }
</script>

<style>
  main {
    margin-top: 5rem;
  }
  form {
    width: 30rem;
    max-width: 90%;
    margin: auto;
  }
</style>

<Header />

<main>
  <form on:submit|preventDefault={addMeetup}>
    <TextInput
      type="text"
      id="title"
      label="Title"
      value={title}
      on:input={(event) => (title = event.target.value)} />
    <TextInput
      type="text"
      id="subtitle"
      label="Subtitle"
      value={subtitle}
      on:input={(event) => (subtitle = event.target.value)} />
    <TextInput
      type="text"
      id="imageUrl"
      label="Image URL"
      value={imageUrl}
      on:input={(event) => (imageUrl = event.target.value)} />
    <TextInput
      type="text"
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

    <Button type="submit" caption="Save" />
  </form>
  <MeetupGrid {meetups} />
</main>
