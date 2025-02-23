const handleSubmit = async () => {
    try {
        clearMessages();
        const response = await axios.post('http://localhost:8000/api/register', {
            name: name.value,
            email: email.value,
            password: password.value,
            password_confirmation: passwordConfirmation.value
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
        console.error('Erro no registro:', error);
        errorMessage.value = error.response?.data?.message || 'Erro ao criar conta. Verifique os dados informados.';
    }
}; 