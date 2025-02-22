<script setup>
import { ref } from 'vue';
import axios from 'axios';

const email = ref('');
const password = ref('');
const error = ref('');
const showRegister = ref(false);

const login = async () => {
    try {
        const response = await axios.post('http://localhost:8000/api/login', {
            email: email.value,
            password: password.value
        });

        // Armazena o token JWT
        localStorage.setItem('token', response.data.access_token);
        // Configura o token no cabeçalho padrão do axios
        axios.defaults.headers.common['Authorization'] = `Bearer ${response.data.access_token}`;
        window.location.reload();
    } catch (err) {
        console.error('Erro de login:', err.response?.data);
        error.value = err.response?.data?.message || 'Email ou senha incorretos';
    }
};
</script>

<template>
    <div class="min-h-screen flex items-center justify-center bg-gray-50 py-12 px-4 sm:px-6 lg:px-8">
        <div class="max-w-md w-full space-y-8">
            <div>
                <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">
                    Login
                </h2>
                <p class="mt-2 text-center text-sm text-gray-600">
                    Ou
                    <a href="#" @click.prevent="$emit('show-register')" class="font-medium text-blue-600 hover:text-blue-500">
                        crie sua conta aqui
                    </a>
                </p>
            </div>
            <form class="mt-8 space-y-6" @submit.prevent="login">
                <div class="rounded-md shadow-sm -space-y-px">
                    <div>
                        <label for="email-address" class="sr-only">Email</label>
                        <input 
                            v-model="email"
                            id="email-address" 
                            name="email" 
                            type="email" 
                            required 
                            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-t-md focus:outline-none focus:ring-blue-500 focus:border-blue-500 focus:z-10 sm:text-sm" 
                            placeholder="Email"
                        >
                    </div>
                    <div>
                        <label for="password" class="sr-only">Senha</label>
                        <input 
                            v-model="password"
                            id="password" 
                            name="password" 
                            type="password" 
                            required 
                            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-blue-500 focus:border-blue-500 focus:z-10 sm:text-sm" 
                            placeholder="Senha"
                        >
                    </div>
                </div>

                <div v-if="error" class="text-red-500 text-sm text-center">
                    {{ error }}
                </div>

                <div>
                    <button 
                        type="submit"
                        class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500"
                    >
                        Entrar
                    </button>
                </div>
            </form>
        </div>
    </div>
</template> 