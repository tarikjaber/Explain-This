<script lang="ts">
    export let name: string;
    export let topic: string;
    export let description: string;
    export let buffering: boolean;
</script>

<div class="card">
    <h1>{ name }</h1>
    <p class="topic">{ topic.toLowerCase() }</p>
    {#if buffering}
        <div class="indeterminate-progress-bar">
            <div class="indeterminate-progress-bar__progress"></div>
        </div>
    {:else}
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

    .indeterminate-progress-bar {
        /* Color */
        background-color: #d1d5db;

        /* Rounded border */
        border-radius: 9999px;

        /* Size */
        margin: 20px;
        height: 0.5rem;

        position: relative;
        overflow: hidden;
    }

    .indeterminate-progress-bar__progress {
        /* Color */
        background-color: #3b82f6;

        /* Rounded border */
        border-radius: 9999px;

        /* Absolute position */
        position: absolute;
        bottom: 0;
        top: 0;
        width: 50%;

        /* Move the bar infinitely */
        animation-duration: 2s;
        animation-iteration-count: infinite;
        animation-name: indeterminate-progress-bar;
    }

    @keyframes indeterminate-progress-bar {
        from {
            left: -50%;
        }
        to {
            left: 100%;
        }
}
</style>
