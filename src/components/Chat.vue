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
    <div v-if="!isConnected" class="login">
        <input v-model="username" placeholder="Enter your Username" v-on:keyup.enter="connectToChat" />
        <button @click="connectToChat">Join Chat</button>
    </div>

    <template v-else>
        <div class="chat-container">
            <div class="messages">
                <div v-for="(msg, index) in messages" :key="index"
                    :class="['message', { 'message-right': msg.user === username }]">
                    <span class="username">{{ msg.user }}</span>
                    <p class="text-bubble">{{ msg.text }}</p>
                </div>
            </div>
            <div class="input-box">
                <input v-model="newMessage" placeholder="Enter your message" v-on:keyup.enter="sendMessage" />
                <button @click="sendMessage">Send</button>
            </div>
        </div>
    </template>
</template>

<style scoped>
.username {
    display: block;
    margin-bottom: 5px;
    color: #666;
}

.message {
    padding: 10px;
    text-align: left;
    color: black;
    margin: 10px;
    max-width: 70%;
}

.message-right {
    text-align: right;
    margin-left: auto;
}

.text-bubble {
    display: inline-block;
    padding: 8px 12px;
    background: #f0f0f0;
    border-radius: 10px;
    margin: 0;
}

.message-right .text-bubble {
    background: #25d366;
    color: white;
}

.input-box {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    padding: 10px;
    background: white;
    display: flex;
    gap: 10px;
    max-width: 100%;
    box-sizing: border-box;
}

.input-box input {
    flex: 1;
}

.chat-container {
    width: 100%;
    height: 100vh;
    display: flex;
    flex-direction: column;
    overflow: auto;
}

.login {
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
}
</style>