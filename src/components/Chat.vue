<script setup>
import { ref } from 'vue'

const username = ref('')
const newMessage = ref('')
const ws = ref(null)
const isConnected = ref(false)

const messages = ref([
    {
        user: 'Bot',
        text: 'test'
    },
    {
        user: 'Bot2',
        text: 'asassdsd'
    },
    {
        user: 'Bot3',
        text: 'teafasfasfst'
    }
])

const connectToChat = () => {
    if (username.value.trim()) {
        ws.value = new WebSocket('ws://localhost:8080/ws')

        ws.value.onopen = () => {
            isConnected.value = true
        }

        ws.value.onmessage = (event) => {
            const data = JSON.parse(event.data)
            messages.value.push(data)
        }

        ws.value.onclose = () => {
            isConnected.value = false
        }
    }
}
const sendMessage = () => {
    if (newMessage.value.trim() && ws.value && isConnected) {
        const data = { user: username.value, text: newMessage.value }
        ws.value.send(JSON.stringify(data))
        newMessage.value = ''
    }
}
</script>

<template>
    <div v-if="!isConnected">
        <input v-model="username" placeholder="Enter your Username" />
        <button @click="connectToChat">Join Chat</button>

    </div>
    <div v-else>
        <div>
            <div>
                <div v-for="(msg, index) in messages" :key="index">
                    <span>{{ msg.user }}</span>
                    <p>{{ msg.text }}</p>
                </div>
            </div>
            <div>
                <input v-model="newMessage" placeholder="Enter your message" />
                <button @click="sendMessage">Send</button>
            </div>
        </div>
    </div>
</template>
