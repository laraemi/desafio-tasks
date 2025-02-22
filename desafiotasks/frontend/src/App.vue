<script setup>
import { ref, onMounted } from 'vue';
import TaskList from './components/TaskList.vue';
import Login from './components/Login.vue';
import Register from './components/Register.vue';

const isAuthenticated = ref(false);
const showRegister = ref(false);

onMounted(() => {
    const token = localStorage.getItem('token');
    isAuthenticated.value = !!token;
});

const logout = () => {
    localStorage.removeItem('token');
    isAuthenticated.value = false;
};

const toggleRegister = () => {
    showRegister.value = !showRegister.value;
};
</script>

<template>
    <div>
        <template v-if="isAuthenticated">
            <nav class="bg-gray-800 text-white p-4">
                <div class="max-w-7xl mx-auto flex justify-between items-center">
                    <h1 class="text-xl font-bold">Sistema de Tarefas</h1>
                    <button 
                        @click="logout"
                        class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded transition duration-200"
                    >
                        Sair
                    </button>
                </div>
            </nav>
            <div class="container mx-auto p-6">
                <TaskList />
            </div>
        </template>
        <template v-else>
            <Login v-if="!showRegister" @show-register="toggleRegister" />
            <Register v-else @show-login="toggleRegister" />
        </template>
    </div>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
