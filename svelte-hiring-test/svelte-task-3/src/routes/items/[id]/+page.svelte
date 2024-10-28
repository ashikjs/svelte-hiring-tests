<script>
    import {page} from '$app/stores';
    import {onMount} from "svelte";
    import {items} from "$lib/data.ts";

    // Get the ID from the URL parameter
    $: id = $page.params.id;
    let item;

    onMount(() => {
        item = items.find((i) => i.id?.toString() == id)
    })

    function goBack() {
        history.back()
    }
</script>

<div class="container">
    {#if item}
        <div class="detail-card">
            <h1>{item.title}</h1>
            <p>{item.description}</p>
            <button class="back-button" on:click={goBack}>
                Back to Home
            </button>
        </div>
    {:else}
        <p>Item not found.</p>
    {/if}
</div>

<style>
    .container {
        max-width: 800px;
        margin: 0 auto;
        padding: 20px;
    }

    .detail-card {
        background: #fff;
        padding: 20px;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }

    h1 {
        font-size: 1.8rem;
        margin-bottom: 1rem;
    }

    p {
        color: #666;
        line-height: 1.6;
        margin-bottom: 1.5rem;
    }

    .back-button {
        padding: 8px 16px;
        background: #007bff;
        color: white;
        border: none;
        border-radius: 4px;
        cursor: pointer;
    }

    .back-button:hover {
        background: #0056b3;
    }
</style>
