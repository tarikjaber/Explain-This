<script lang="ts">
    import ProgressBar from './ProgressBar.svelte'

    export let buffering: boolean
    export let name: string
    export let topic: string
    export let description: string
</script>

<div class="card">
    <h1>{name.toLowerCase()}</h1>
    <p class="topic">{topic.toLowerCase()}</p>
    {#if buffering}
        <p> </p>
    {:else}
        <p>{description}</p>
    {/if}
</div>

<style lang="scss">
    $large-screen: 920px;

    .card {
        outline: solid black;
        border-radius: 10px;
        margin: 20px;

        @media (min-width: #{$large-screen}) {
            width: 400px;
        }
        @media (max-width: #{$large-screen - 1px}) {
            flex-grow: 1;
            flex-basis: 300px;
        }
    }

    .topic {
        color: grey;
        padding: 0px 20px;
    }

    h1,
    p {
        margin: 0px;
    }

    h1 {
        padding: 20px 20px 0px;
    }

    p {
        padding: 20px;
    }
</style>
