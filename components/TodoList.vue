<template>
    <form @submit.prevent="handleSubmit">
        <input v-model="newTodo" placeholder="Add a todo" />
        <button type="submit">Add</button>
    </form>

    <div>
        <ul>
            <li v-for="(todo, index) in todos" :key="index">
                {{ todo }}
            </li>
        </ul>
    </div>
</template>

<script setup lang="ts">
import { reactive, ref } from 'vue';

const newTodo = ref<string>('')
const todos = reactive<string[]>([])

function handleSubmit() {
    todos.push(newTodo.value)
    newTodo.value = ''
}
</script>
