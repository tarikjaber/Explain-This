<script lang="ts">
    import { createEventDispatcher } from 'svelte'

    const dispatch = createEventDispatcher()

    function handleKeyDown(event: KeyboardEvent) {
        if (event.key === 'Enter') {
            dispatch('entered', (event.target as HTMLInputElement).value)
        }
    }
</script>

<div>
    <input type="text" id="fname" name="fname" on:keydown={handleKeyDown} />
</div>

<style>
    div {
        width: 100vw;
        position: relative;

        display: flex;
        flex-flow: column wrap;
        align-items: center;
    }

    input {
        font-size: 18px;
        width: 400px;
        height: 50px;
        border-radius: 10px;
    }
</style>
