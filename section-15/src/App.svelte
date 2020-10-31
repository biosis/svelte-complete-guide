<script>
    import meetups from "./meetups/meetups-store";

    import Button from "./shared/Button.svelte";
    import LoadingSpinner from "./shared/LoadingSpinner.svelte";
    import Header from "./shared/Header.svelte";
    import MeetupGrid from "./meetups/MeetupGrid.svelte";
    import EditMeetup from "./meetups/EditMeetup.svelte";
    import MeetupDetail from "./meetups/MeetupDetail.svelte";
    import Error from "./shared/Error.svelte";

    let isLoading = true;
    let error;

    fetch("https://svelte-course-753b5.firebaseio.com/meetups.json")
        .then((res) => {
            if (!res.ok) {
                throw new Error(
                    "Fetching meetups error occured, please try again later!"
                );
            }
            return res.json();
        })
        .then((data) => {
            const loadedMeetups = [];
            for (const key in data) {
                loadedMeetups.push({
                    ...data[key],
                    id: key,
                });
            }
            meetups.setMeetups(loadedMeetups.reverse());
            isLoading = false;
        })
        .catch((err) => {
            isLoading = false;
            error = err;
            console.log(err);
        });

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

    function clearError() {
        error = null;
    }
</script>

<style>
    main {
        margin-top: 5rem;
    }
</style>

{#if error}
    <Error message={error.message} on:cancel={clearError} />
{/if}

<Header />

<main>
    {#if page === 'overview'}
        {#if editMode === 'edit'}
            <EditMeetup
                id={editedId}
                on:save={savedMeetup}
                on:cancel={cancelEdit} />
        {/if}
        {#if isLoading}
            <LoadingSpinner />
        {:else}
            <MeetupGrid
                meetups={$meetups}
                on:showdetails={showDetails}
                on:edit={startEdit}
                on:add={() => {
                    editMode = 'edit';
                }} />
        {/if}
    {:else}
        <MeetupDetail id={pageData.id} on:close={closeDetails} />
    {/if}
</main>
