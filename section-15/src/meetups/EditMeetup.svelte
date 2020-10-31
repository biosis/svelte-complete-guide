<script>
    import meetups from "./meetups-store";
    import { createEventDispatcher } from "svelte";
    import { isEmpty, isValidEmail } from "../helpers/validation";

    import Button from "../shared/Button.svelte";
    import Modal from "../shared/Modal.svelte";
    import TextInput from "../shared/TextInput.svelte";

    export let id = null;

    let title = "";
    let subtitle = "";
    let imageUrl = "";
    let address = "";
    let email = "";
    let description = "";
    let selectedMeetup;

    if (id) {
        const unsubcribe = meetups.subscribe((items) => {
            selectedMeetup = items.find((i) => i.id === id);
            title = selectedMeetup.title;
            subtitle = selectedMeetup.subtitle;
            imageUrl = selectedMeetup.imageUrl;
            address = selectedMeetup.address;
            email = selectedMeetup.contactEmail;
            description = selectedMeetup.description;
        });
        unsubcribe();
    }

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
        const meetupData = {
            title: title,
            subtitle: subtitle,
            description: description,
            imageUrl: imageUrl,
            address: address,
            contactEmail: email,
        };

        if (id) {
            fetch(
                `https://svelte-course-753b5.firebaseio.com/meetups/${id}.json`,
                {
                    method: "PATCH",
                    body: JSON.stringify(meetupData),
                    headers: { "Content-Type": "application/json" },
                }
            )
                .then((res) => {
                    if (!res.ok) {
                        throw new Error("An error occured, please try again!");
                    }
                    meetups.updateMeetup(id, meetupData);
                })
                .catch((err) => console.log(err));
        } else {
            fetch("https://svelte-course-753b5.firebaseio.com/meetups.json", {
                method: "POST",
                body: JSON.stringify({ ...meetupData, isFavorite: false }),
                headers: { "Content-Type": "application/json" },
            })
                .then((res) => {
                    if (!res.ok) {
                        throw new Error("An error occured, please try again!");
                    }

                    return res.json();
                })
                .then((data) => {
                    meetups.addMeetup({
                        ...meetupData,
                        isFavorite: false,
                        id: data.name,
                    });
                })
                .catch((err) => console.log(err));
        }
        dispatch("save");
    }

    function deleteMeetup() {
        fetch(`https://svelte-course-753b5.firebaseio.com/meetups/${id}.json`, {
            method: "DELETE",
        })
            .then((res) => {
                if (!res.ok) {
                    throw new Error("An error occured, please try again!");
                }
                meetups.removeMeetup(id);
            })
            .catch((err) => console.log(err));
        dispatch("save");
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
        {#if id}
            <Button type="button" on:click={deleteMeetup}>Delete</Button>
        {/if}
    </div>
</Modal>
