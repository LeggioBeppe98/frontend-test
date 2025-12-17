<script setup>
import { computed } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import Prove from '../components/Prove.vue'

const router = useRouter()
const route = useRoute()

const search = computed({
    get() {
        return route.query.search ?? ''
    },
    set(search) {
        router.replace({ query: { search } })
    }
})
</script>

<template>
    <h2>AboutView</h2>
    <label>
        Search: <input v-model.trim="search" maxlength="20">
    </label>
    <Prove></Prove>
</template>