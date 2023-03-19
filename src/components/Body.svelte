<script lang="ts">
    import Input from './Input.svelte'
    import Pane from './Pane.svelte'

    export let apiKey: string
    let topic: string
    let searched: boolean = false
    let buffering: boolean = true

    let placeholder =
        'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.'
    let descriptions = [
        placeholder,
        placeholder,
        placeholder,
        placeholder,
        placeholder,
    ]
    let groups = ['Child', 'Teen', 'College Student', 'Grad Student', 'Expert']
    const url = 'https://api.openai.com/v1/chat/completions'

    async function fetchDescription() {
        console.log('fetch description')
        console.log('api key: ' + apiKey)
        const data = {
            model: 'gpt-3.5-turbo',
            messages: [
                {
                    role: 'user',
                    content: `Explain ${topic} to a child, teen, college student, grad student, and expert. Separate each of the 5 paragraphs with a newline. Format the response like "Child: <description> Teen: <description> ..."`,
                },
            ],
        }

        buffering = true

        const response = await fetch(url, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                Authorization: `Bearer ${apiKey}`,
            },
            body: JSON.stringify(data),
        })

        console.log(response)

        if (!response.ok || response.body === null) {
            if (response.status === 401) {
                alert('Your API Key is invalid. Please enter a valid API Key.')
            } else {
                alert('ChatGPT API is overloaded. Please try again later.')
            }
            console.error(`API request failed with status ${response.status}`)
            return
        }

        let responseJson = await response.json()
        let content: string = responseJson.choices[0].message.content.trim()
        let rawDescriptions = content.split('\n\n')
        descriptions = rawDescriptions.map((description) => {
            const colonIndex = description.indexOf(':')
            return description.substring(colonIndex + 1)
        })

        buffering = false
    }

    function handleSearch(event: CustomEvent) {
        topic = event.detail
        searched = true
        fetchDescription()
    }
</script>

<div class="flex-container">
    <Input on:entered={handleSearch} on:clicked={handleSearch} />
    {#if searched}
        {#each descriptions as description, i}
            <Pane name={groups[i]} {topic} bind:description {buffering} />
        {/each}
    {/if}
</div>

<style>
    .flex-container {
        margin-top: 30px;
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
    }


    p {
        padding: 0 20%;
        font-size: 18px;
    }
</style>
