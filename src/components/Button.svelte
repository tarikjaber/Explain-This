<script lang="ts">
    import { createEventDispatcher } from 'svelte';

    export let text: string;

    const dispatch = createEventDispatcher();

    function handleClick() {
        dispatch('clicked');
    }
</script>

<button on:click={handleClick}>{text}</button>

<style>
    button {
        margin-left: auto;
        min-width: 85px;
        margin-right: 30px;
        background-color: #007fff;
        padding: 12px;
        font-size: medium;
        font-weight: bold;
        border-radius: 8px;
        color: white;
        border: none;
        transition: all 0.1s ease-in-out;
    }

    button:hover {
        cursor: pointer;
        background-color: #0059b2;
    }
</style>
