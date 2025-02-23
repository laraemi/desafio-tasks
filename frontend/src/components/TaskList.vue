<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const tasks = ref([]);
const newTask = ref({ title: '', description: '', status: 'pendente', due_date: '' });
const editingTask = ref(null);
const errorMessage = ref('');
const successMessage = ref('');

// Função para limpar mensagens
const clearMessages = () => {
    errorMessage.value = '';
    successMessage.value = '';
};

// Função para mostrar mensagem de sucesso temporariamente
const showSuccess = (message) => {
    successMessage.value = message;
    setTimeout(() => {
        successMessage.value = '';
    }, 3000);
};

const fetchTasks = async () => {
    try {
        clearMessages();
        const token = localStorage.getItem('token');
        if (!token) {
            window.location.href = '/login';
            return;
        }

        const response = await axios.get('http://localhost:8000/api/tasks', {
            headers: { 
                'Authorization': `Bearer ${token}`,
                'Accept': 'application/json'
            }
        });
        tasks.value = response.data.data || response.data;
    } catch (error) {
        if (error.response?.status === 401) {
            localStorage.removeItem('token');
            window.location.href = '/login';
        }
        console.error('Erro ao buscar tarefas:', error);
        errorMessage.value = 'Erro ao carregar as tarefas. Por favor, verifique sua conexão.';
    }
};

const addTask = async () => {
    try {
        clearMessages();
        if (!newTask.value.title) {
            errorMessage.value = 'Por favor, preencha o título da tarefa';
            return;
        }

        const token = localStorage.getItem('token');
        if (!token) {
            window.location.href = '/login';
            return;
        }

        const response = await axios.post('http://localhost:8000/api/tasks', newTask.value, {
            headers: { 
                'Authorization': `Bearer ${token}`,
                'Accept': 'application/json',
                'Content-Type': 'application/json'
            }
        });

        if (response.data) {
            newTask.value = { title: '', description: '', status: 'pendente', due_date: '' };
            await fetchTasks();
            showSuccess('Tarefa adicionada com sucesso!');
        }
    } catch (error) {
        if (error.response?.status === 401) {
            localStorage.removeItem('token');
            window.location.href = '/login';
        }
        console.error('Erro ao adicionar tarefa:', error);
        errorMessage.value = 'Erro ao adicionar a tarefa. Por favor, tente novamente.';
    }
};

const startEditing = (task) => {
    editingTask.value = { ...task };
};

const cancelEditing = () => {
    editingTask.value = null;
};

const updateTask = async (task) => {
    try {
        clearMessages();
        const token = localStorage.getItem('token');
        if (!token) {
            window.location.href = '/login';
            return;
        }

        await axios.put(`http://localhost:8000/api/tasks/${task.id}`, task, {
            headers: { 
                'Authorization': `Bearer ${token}`,
                'Accept': 'application/json',
                'Content-Type': 'application/json'
            }
        });
        editingTask.value = null;
        await fetchTasks();
        showSuccess('Tarefa atualizada com sucesso!');
    } catch (error) {
        if (error.response?.status === 401) {
            localStorage.removeItem('token');
            window.location.href = '/login';
        }
        console.error('Erro ao atualizar tarefa:', error);
        errorMessage.value = 'Erro ao atualizar a tarefa. Por favor, tente novamente.';
    }
};

const deleteTask = async (id) => {
    try {
        clearMessages();
        if (!confirm('Tem certeza que deseja excluir esta tarefa?')) return;
        
        const token = localStorage.getItem('token');
        if (!token) {
            window.location.href = '/login';
            return;
        }

        await axios.delete(`http://localhost:8000/api/tasks/${id}`, {
            headers: { 
                'Authorization': `Bearer ${token}`,
                'Accept': 'application/json'
            }
        });
        await fetchTasks();
        showSuccess('Tarefa excluída com sucesso!');
    } catch (error) {
        if (error.response?.status === 401) {
            localStorage.removeItem('token');
            window.location.href = '/login';
        }
        console.error('Erro ao excluir tarefa:', error);
        errorMessage.value = 'Erro ao excluir a tarefa. Por favor, tente novamente.';
    }
};

onMounted(async () => {
    await fetchTasks();
});
</script>

<template>
    <div class="max-w-4xl mx-auto p-6 bg-white shadow-lg rounded-xl">
        <h1 class="text-3xl font-bold mb-6 text-center text-gray-800">Gerenciador de Tarefas</h1>
        
        <!-- Mensagens de Erro e Sucesso -->
        <div v-if="errorMessage" class="mb-4 p-4 bg-red-100 text-red-700 rounded-lg">
            {{ errorMessage }}
        </div>
        <div v-if="successMessage" class="mb-4 p-4 bg-green-100 text-green-700 rounded-lg">
            {{ successMessage }}
        </div>
        
        <!-- Formulário de Nova Tarefa -->
        <div class="mb-8 bg-gray-50 p-4 rounded-lg">
            <h2 class="text-xl font-semibold mb-4 text-gray-700">Nova Tarefa</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <input 
                    v-model="newTask.title" 
                    type="text" 
                    placeholder="Título" 
                    class="border rounded-lg p-2 w-full focus:outline-none focus:ring-2 focus:ring-blue-400"
                    required
                >
                <input 
                    v-model="newTask.description" 
                    type="text" 
                    placeholder="Descrição" 
                    class="border rounded-lg p-2 w-full focus:outline-none focus:ring-2 focus:ring-blue-400"
                >
                <select 
                    v-model="newTask.status" 
                    class="border rounded-lg p-2 w-full focus:outline-none focus:ring-2 focus:ring-blue-400"
                >
                    <option value="pendente">Pendente</option>
                    <option value="em andamento">Em Andamento</option>
                    <option value="concluída">Concluída</option>
                </select>
                <input 
                    v-model="newTask.due_date" 
                    type="date" 
                    class="border rounded-lg p-2 w-full focus:outline-none focus:ring-2 focus:ring-blue-400"
                    required
                >
            </div>
            <button 
                @click="addTask" 
                class="mt-4 bg-blue-500 hover:bg-blue-600 text-white px-6 py-2 rounded-lg transition duration-200"
            >
                Adicionar Tarefa
            </button>
        </div>
        
        <!-- Lista de Tarefas -->
        <div v-if="tasks.length === 0" class="text-center text-gray-500 py-8">
            Nenhuma tarefa encontrada. Adicione uma nova tarefa!
        </div>
        
        <div v-else class="space-y-4">
            <div v-for="task in tasks" :key="task.id" 
                class="p-4 border rounded-lg hover:shadow-md transition duration-200"
                :class="{
                    'bg-yellow-50': task.status === 'pendente',
                    'bg-blue-50': task.status === 'em andamento',
                    'bg-green-50': task.status === 'concluída'
                }"
            >
                <!-- Modo Visualização -->
                <div v-if="editingTask?.id !== task.id">
                    <div class="flex justify-between items-start">
                        <div>
                            <h3 class="text-xl font-semibold">{{ task.title }}</h3>
                            <p class="text-gray-600">{{ task.description }}</p>
                            <div class="mt-2 space-x-2">
                                <span class="text-sm px-3 py-1 rounded-full" 
                                    :class="{
                                        'bg-yellow-200 text-yellow-800': task.status === 'pendente',
                                        'bg-blue-200 text-blue-800': task.status === 'em andamento',
                                        'bg-green-200 text-green-800': task.status === 'concluída'
                                    }"
                                >
                                    {{ task.status }}
                                </span>
                                <span class="text-sm text-gray-500">
                                    Data: {{ new Date(task.due_date).toLocaleDateString() }}
                                </span>
                            </div>
                        </div>
                        <div class="space-x-2">
                            <button 
                                @click="startEditing(task)"
                                class="bg-gray-500 hover:bg-gray-600 text-white px-3 py-1 rounded transition duration-200"
                            >
                                Editar
                            </button>
                            <button 
                                @click="deleteTask(task.id)"
                                class="bg-red-500 hover:bg-red-600 text-white px-3 py-1 rounded transition duration-200"
                            >
                                Excluir
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Modo Edição -->
                <div v-else class="space-y-4">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <input 
                            v-model="editingTask.title" 
                            type="text" 
                            class="border rounded p-2 w-full"
                            required
                        >
                        <input 
                            v-model="editingTask.description" 
                            type="text" 
                            class="border rounded p-2 w-full"
                        >
                        <select 
                            v-model="editingTask.status" 
                            class="border rounded p-2 w-full"
                        >
                            <option value="pendente">Pendente</option>
                            <option value="em andamento">Em Andamento</option>
                            <option value="concluída">Concluída</option>
                        </select>
                        <input 
                            v-model="editingTask.due_date" 
                            type="date" 
                            class="border rounded p-2 w-full"
                            required
                        >
                    </div>
                    <div class="flex justify-end space-x-2">
                        <button 
                            @click="updateTask(editingTask)"
                            class="bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded transition duration-200"
                        >
                            Salvar
                        </button>
                        <button 
                            @click="cancelEditing"
                            class="bg-gray-500 hover:bg-gray-600 text-white px-4 py-2 rounded transition duration-200"
                        >
                            Cancelar
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}
</style>
