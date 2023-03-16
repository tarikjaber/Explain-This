<script>
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    function handleClick() {
        dispatch('clicked');
    }
</script>

<div class="navbar">
    <h1>Explain This</h1>
    <button on:click={handleClick}>API Key</button>
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
    }

    button:hover {
        cursor: pointer;
        background-color: #0059b2;
    }

    .navbar {
        background-color: #222831;
        color: white;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
</style>
