<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const tasks = ref([]);

onMounted(async () => {
    const response = await axios.get('http://localhost:8000/api/tasks', {
        headers: { Authorization: `Bearer ${localStorage.getItem('token')}` },
    });
    tasks.value = response.data;
});
</script>

<template>
    <div>
        <h1>Minhas Tarefas</h1>
        <ul>
            <li v-for="task in tasks" :key="task.id">
                {{ task.title }} - {{ task.status }}
            </li>
        </ul>
    </div>
</template>
