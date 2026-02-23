<script lang="ts">
  import { onMount } from "svelte";

    let socket:WebSocket
    let messages:Array<string> = $state([])

    let socketFuncs = () => {
        socket = new WebSocket('ws://localhost:8250/wss')
        socket.onopen = () => {
            console.log("WS connection established") // look what a cool word I have learned
        }

        socket.onmessage = (e) => {
            console.log("got message! ", e.data)
            messages = [...messages, e.data]
        }
        
        socket.onclose = () => {
            console.log("WS connection terminated")
        }

        socket.onerror = (err) => {
            console.log("connection error: ", err)
        }

        return () => {
            socket.close()
        }
    }

    onMount(socketFuncs)
</script>

<button type="button" onclick={socketFuncs}>
    Update connection to WebSocket
</button>
<p>messages:</p>
{#each messages as message }
    <p>{message}</p>
{/each}

<style lang="scss">
    p {
        font-weight: 1000;
        font-size: 2em;
    }
</style>