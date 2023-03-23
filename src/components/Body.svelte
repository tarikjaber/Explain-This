<script lang="ts">
    import Input from './Input.svelte'
    import Pane from './Pane.svelte'

    export let apiKey: string
    let topic: string
    let searched: boolean = false

    let descriptions: string[] = []
    let groups = ['Child', 'Teen', 'College Student', 'Grad Student', 'Expert']
    const url = 'https://api.openai.com/v1/chat/completions'

    async function fetchDescription() {
        const data = {
            model: 'gpt-3.5-turbo',
            messages: [
                {
                    role: 'user',
                    content: `Explain ${topic} to a child, teen, college student, grad student, and expert. Separate each of the 5 paragraphs with a newline. Format the response like "Child: <description> Teen: <description> ..."`,
                },
            ],
            stream: true,
        }

        const response = await fetch(url, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                Authorization: `Bearer ${apiKey}`,
            },
            body: JSON.stringify(data),
        })

        if (!response.ok || response.body === null) {
            if (response.status === 401) {
                alert('Your API Key is invalid. Please enter a valid API Key.')
                searched = false
            } else {
                alert('ChatGPT API is overloaded. Please try again later.')
                searched = false
            }
            console.error(`API request failed with status ${response.status}`)
            return
        }

        const reader = response.body.getReader()

        let res = ''

        while (true) {
            const { done, value } = await reader.read()
            let decoded = new TextDecoder().decode(value).split('data: ')[1]
            let responseJson = JSON.parse(decoded)

            let content = responseJson.choices[0].delta.content
            let finish = responseJson.choices[0].finish_reason

            if (finish === 'stop') {
                break
            }

            if (!content) {
                continue
            }

            res += content
            descriptions = res.trim().split('\n\n').map((description) => {
                if (description.length < 17) {
                    return " "
                }
                const colonIndex = description.indexOf(':')
                return description.substring(colonIndex + 1)
            })
        }
    }

    function handleSearch(event: CustomEvent) {
        topic = event.detail
        searched = true
        fetchDescription()
    }
</script>

<div class="body">
    <Input on:entered={handleSearch} on:clicked={handleSearch} />
    <div class="panes">
        {#if searched}
            {#each groups as group, i}
                <Pane name={group} {topic} description={descriptions[i] ?? ""} buffering={i > descriptions.length - 1}/>
            {/each}
        {/if}
    </div>
</div>

<style>
    .body {
        margin-top: 30px;
    }

    .panes {
        margin-top: 10px;
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
    }

    p {
        padding: 0 20%;
        font-size: 18px;
    }
</style>
