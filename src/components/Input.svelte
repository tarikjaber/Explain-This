<script lang="ts">
    import { createEventDispatcher, onMount } from 'svelte'
    import Search from './Search.svelte'
    let text: string
    let ref:HTMLInputElement

    const dispatch = createEventDispatcher()

    function clearText() {
        ref.focus()
        ref.value = ""
    }

    function handleKeyDown(event: KeyboardEvent) {
        if (event.key === 'Enter') {
            dispatch('entered', (event.target as HTMLInputElement).value)
            clearText()
        }
    }

    function handleClick() {
        dispatch('clicked', text)
        clearText()
    }

    onMount(() => {
        ref.focus()
    })
</script>

<div>
    <input type="text" id="fname" name="fname" on:keydown={handleKeyDown} bind:value={text} bind:this={ref}/>
    <button on:click={handleClick}><Search /></button>
</div>

<style>
    div {
        width: 100vw;
        position: relative;
        display: flex;
        justify-content: center;
    }

    input {
        font-size: 18px;
        padding: 0px 20px;
        border: 3px solid black;
        height: 50px;
        border-style: solid none solid solid;
        border-radius: 8px 0 0 8px;
        flex-shrinK: 1;
    }

    input:focus {
        outline: none;
    }

    button {
        width: 55px;
        background-color: white;
        height: 55px;
        padding: 8px;
        font-size: medium;
        font-weight: bold;
        border-radius: 0 8px 8px 0;
        color: black;
        border: 3px black;
        border-style: solid solid solid none;
    }

    button:hover {
        cursor: pointer;
    }
</style>
