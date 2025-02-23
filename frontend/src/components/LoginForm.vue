const handleSubmit = async () => {
    try {
        clearMessages();
        const response = await axios.post('http://localhost:8000/api/login', {
            email: email.value,
            password: password.value
        }, {
            headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json'
            }
        });

        if (response.data.token) {
            localStorage.setItem('token', response.data.token);
            router.push('/tasks');
        }
    } catch (error) {
        console.error('Erro no login:', error);
        errorMessage.value = error.response?.data?.message || 'Erro ao fazer login. Verifique suas credenciais.';
    }
}; 