<script>
    import meetups from "./meetups/meetups-store";

    import Button from "./shared/Button.svelte";
    import Header from "./shared/Header.svelte";
    import MeetupGrid from "./meetups/MeetupGrid.svelte";
    import EditMeetup from "./meetups/EditMeetup.svelte";
    import MeetupDetail from "./meetups/MeetupDetail.svelte";

    /* 
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
      isFavorite: false,
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
      isFavorite: false,
    },
  ]; */

    let editMode;
    let editedId;
    let page = "overview";
    let pageData = {};

    function savedMeetup(event) {
        editMode = null;
        editedId = null;
    }

    function cancelEdit() {
        editMode = null;
        editedId = null;
    }

    function startEdit(event) {
        editMode = "edit";
        editedId = event.detail;
    }

    function showDetails(event) {
        page = "details";
        pageData.id = event.detail;
    }

    function closeDetails() {
        page = "overview";
        pageData = {};
    }
</script>

<style>
    main {
        margin-top: 5rem;
    }
</style>

<Header />

<main>
    {#if page === 'overview'}
        {#if editMode === 'edit'}
            <EditMeetup
                id={editedId}
                on:save={savedMeetup}
                on:cancel={cancelEdit} />
        {/if}
        <MeetupGrid
            meetups={$meetups}
            on:showdetails={showDetails}
            on:edit={startEdit}
            on:add={() => {
                editMode = 'edit';
            }} />
    {:else}
        <MeetupDetail id={pageData.id} on:close={closeDetails} />
    {/if}
</main>
