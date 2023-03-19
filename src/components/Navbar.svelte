<script lang="ts">
    import { createEventDispatcher } from 'svelte';
    import Button from './Button.svelte'

    const dispatch = createEventDispatcher();

    function handleClick() {
        dispatch('clicked');
    }
</script>

<div class="navbar">
    <h1>Explain This</h1>
    <Button text={"API Key"} on:clicked={handleClick} />
    <a href="https://github.com/tarikjaber/Explain-This" target="_blank">
        <img src="/github-mark-white.png" alt="GitHub Contribution Icon" class="github-icon">
    </a>
</div>

<style>
    h1 {
        margin: 0;
        padding: 20px 30px;
    }

    img {
        width: 40px;
        margin-right: 30px;
    }

    .navbar {
        background-color: #222831;
        color: white;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
</style>

