<script lang="ts">
    import Navbar from '../components/Navbar.svelte'
    import Body from '../components/Body.svelte'
    import { onMount } from 'svelte'

    let apiKey: string | null

    function getApiKey(reset: boolean = false) {
        apiKey = localStorage.getItem('apiKey')

        if (reset || !apiKey) {
            const goToApiKeysUrl = confirm("Do you want to go to the OpenAI API keys URL to get an API Key?");

            if (goToApiKeysUrl) {
                window.open("https://platform.openai.com/account/api-keys");
            }
            apiKey = prompt(
                'Please enter your OpenAI API key. The API key is stored locally.'
            )
            localStorage.setItem('apiKey', apiKey ?? "")
        }

        return apiKey
    }

    function handleApi(event: CustomEvent) {
        getApiKey(true)
    }

    onMount(() => {
        getApiKey()
    })
</script>

<svelte:head>
    <title>Explain This</title>
</svelte:head>
<Navbar on:clicked={handleApi}/>
<Body apiKey={apiKey ?? ""}/>
