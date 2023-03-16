<script lang="ts">
    export let name: string;
    export let topic: string;
    export let description: string;
    export let buffering: boolean;
</script>

<div class="card">
    <h1>{ name }</h1>
    {#if buffering}
        <div class="loader"></div>

    {:else}
        <p class="topic">{ topic.toLowerCase() }</p>
        <p>{ description }</p>
    {/if}
</div>

<style>
    .card {
        width: 400px;
        outline: solid black;
        border-radius: 10px;
        margin: 30px;
    }

    .topic {
        color: grey;
        padding: 0px 20px;
    }

    h1, p {
        margin: 0px;
    }

    h1 {
        padding: 20px 20px 0px;
    }

    p {
        padding: 20px;
    }

    .loader {
        border: 16px solid #f3f3f3; /* Light grey */
        border-top: 16px solid #3498db; /* Blue */
        border-radius: 50%;
        width: 80px;
        margin: 10px auto 10px;
        height: 80px;
        animation: spin 2s linear infinite;
    }

    @keyframes spin {
        0% { transform: rotate(0deg); }
        100% { transform: rotate(360deg); }
    }
</style>
