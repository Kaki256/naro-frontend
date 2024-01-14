<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const inputCityName = ref<string>('')
const router = useRouter()

const showCityInfo = () => {
    if (inputCityName.value) {
        router.push(`/city/${encodeURIComponent(inputCityName.value)}`)
    }
}
</script>

<template>
    <div>
        <label>
            都市の名前
            <input type="text" v-model="inputCityName">
        </label>
        <button @click="showCityInfo">表示する</button>
    </div>
</template>