<script lang="ts">
    import Input from "../components/Input.svelte"
    import Navbar from "../components/Navbar.svelte"
    import Pane from "../components/Pane.svelte";
    import { onMount } from "svelte";

    let topic: string = "Quantum Physics"
    let buffering: boolean = true

    let placeholder = "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."
    let individuals = {
        child: placeholder,
        teen: placeholder,
        "college student": placeholder,
        "grad student": placeholder,
        expert: placeholder
    }

    const url = 'https://api.openai.com/v1/chat/completions'

    async function fetchDescription() {
        let apiKey = localStorage.getItem("apiKey")
        if (apiKey === null) {
            while (apiKey === null || apiKey === "") {
                apiKey = prompt("Please enter your OpenAI API key")
            }
            localStorage.setItem("apiKey", apiKey)
        }

        const data = {
            model: 'gpt-3.5-turbo',
            messages: [
                {
                    role: 'user',
                    content: `Explain ${topic} to a child, teen, college student, grad student, and expert. Keep it concise and informative. Make your response in json format like this {
                        "child": "...",
                        "teen": "...",
                        "college student": "...",
                        "grad student": "...",
                        "expert": "..."
                    }`,
                },
            ],
        }

        buffering = true

        fetch(url, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                Authorization: `Bearer ${apiKey}`,
            },
            body: JSON.stringify(data),
        })
            .then((res) => res.json())
            .then((json) => {
                console.log(json);
                let jsonString: string = json.choices[0].message.content;
                return JSON.parse(jsonString);
            })
            .then((json) => {
                individuals = json;
                buffering = false;
            })
            .catch((err) => console.error(err))
    }

    onMount(fetchDescription)

    function handleEntered(event: CustomEvent) {
        topic = event.detail
        fetchDescription()
    }
</script>

<Navbar />
<div class="flex-container">
    <Input on:entered={handleEntered}/>
    {#each Object.entries(individuals) as [individual, description]}
        <Pane name={individual} {topic} description={description} {buffering}/>
    {/each}
</div>

<style>



    .flex-container {
        margin-top: 30px;
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
    }
</style>
