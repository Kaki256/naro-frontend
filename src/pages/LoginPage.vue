<script setup lang="ts">
import { ref } from 'vue'

const username = ref<string>('')
const password = ref<string>('')

const login = () =>
    fetch('/api/login', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({ username: username.value, password: password.value })
    })

</script>

<template>
    <div class="login">
        <h1>ログイン画面</h1>
        <label>
            名前: 
            <input type="text" v-model="username">
        </label>
        <label>
            パスワード: 
            <input type="password" v-model="password">
        </label>
        <button @click="login">ログイン</button>
    </div>
</template>