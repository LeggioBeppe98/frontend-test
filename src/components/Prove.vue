<script setup>
import { ref, computed, watch } from 'vue'
import { onMounted } from 'vue'
import ChildComp from './ChildComp.vue'

const count = ref(0)

const text = ref('')

const titleClass = ref('title')

const awesome = ref(true)

const hideCompleted = ref(false)

let id = 0
const newTodo = ref('')
const todos = ref([
    { id: id++, text: 'Learn HTML', done: true },
    { id: id++, text: 'Learn JavaScript', done: true },
    { id: id++, text: 'Learn Vue', done: false }
])

const pElementRef = ref(null)

const filteredTodos = computed(() => {
    return hideCompleted.value ? todos.value.filter((t) => !t.done) : todos.value
})

const childMsg = ref('No child msg yet')

function increment() {
    count.value++;
}
function onInput(e) {
    text.value = e.target.value
}

function changeStatus() {
    awesome.value = !awesome.value
}

function addTodo() {
    todos.value.push({
        id: id++,
        text: newTodo.value
    })

    newTodo.value = '' // Reset dell'input
}

function removeTodo(todo) {
    let newArray = [];
    todos.value.forEach(element => {
        if (element.id != todo.id) {
            newArray.push(element)
        }
    });
    todos.value = newArray
}

onMounted(() => {
    // component is now mounted.
    pElementRef.value.textContent = "ciao";
})

watch(count, (newCount) => {
    console.log(`new count is: ${newCount}`)
})

watch(count)
</script>

<template>
    <h1>{{ count }}</h1>
    <button @click="increment" class="btn">Incrementa</button>


    <h1 :class="titleClass">Make me red</h1>

    <input v-model="text" @input="onInput">
    <p>{{ text }}</p>

    <button @click="changeStatus" class="btn">Change</button>
    <h1 v-if="awesome">Vue is awesome!</h1>
    <h1 v-else>Oh no 😢</h1>

    <form @submit.prevent="addTodo">
        <input v-model="newTodo" required placeholder="new todo">
        <button>Add Todo</button>
    </form>
    <ul>
        <li v-for="todo in filteredTodos" :key="todo.id">
            <input type="checkbox" v-model="todo.done">
            <span :class="{ barrato: todo.done }">{{ todo.text }}</span>
            <button @click="removeTodo(todo)">X</button>
        </li>
        <button @click="hideCompleted = !hideCompleted">
            {{ hideCompleted ? 'Show all' : 'Hide completed' }}
        </button>
    </ul>

    <p ref="pElementRef">Hello</p>


    <ChildComp @response="(msg) => childMsg = msg" />
    <p>{{ childMsg }}</p>
</template>

<style scoped>
.btn {
    background-color: #42b883;
    color: white;
    padding: 12px 24px;
    border: none;
    border-radius: 6px;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    margin-top: 10px;
    transition: background-color 0.3s;
}

.title {
    color: red;
}

.barrato {
    text-decoration: line-through;
}
</style>
