# 📋 Sistema de Gerenciamento de Tarefas

Um sistema para gerenciamento de tarefas, construído com Laravel 10 e Vue.js 3, utilizando as melhores práticas de desenvolvimento e arquitetura de software.

## 🚀 Funcionalidades

- ✨ Interface moderna e responsiva
- 🔐 Autenticação segura com JWT
- 📱 CRUD completo de tarefas
- 🎯 Gestão de status das tarefas
- 👥 Multi-usuário com isolamento de dados
- 🔄 Atualizações em tempo real
- 🎨 Design moderno com Tailwind CSS

## 🛠️ Tecnologias Utilizadas

### Backend
- Laravel 10
- PHP 8.2
- MySQL 8.0
- JWT Authentication
- RESTful API

### Frontend
- Vue.js 3
- JWT.
- Tailwind CSS
- Axios
- Vue Router

### DevOps
- Docker
- Docker Compose
- Nginx
- Git

## 📦 Pré-requisitos

- Docker
- Docker Compose
- Git

## 🚀 Como Rodar o Projeto

1. Clone o repositório:
```bash
git clone <seu-repositorio>
cd desafio-tasks
```

2. Configure as variáveis de ambiente:
```bash
cp backend/.env.example backend/.env
```

3. Inicie os containers:
```bash
docker compose up -d
```

4. Instale as dependências e execute as migrações:
```bash
docker compose exec app composer install
docker compose exec app php artisan migrate
docker compose exec app php artisan key:generate
```

5. Acesse a aplicação:
- Frontend: http://localhost:3000
- Backend: http://localhost:8000
- API: http://localhost:8000/api

## 📚 Estrutura da API

### Autenticação
- POST `/api/register` - Registro de novo usuário
- POST `/api/login` - Login de usuário
- POST `/api/logout` - Logout de usuário

### Tarefas
- GET `/api/tasks` - Lista todas as tarefas do usuário
- POST `/api/tasks` - Cria uma nova tarefa
- PUT `/api/tasks/{id}` - Atualiza uma tarefa existente
- DELETE `/api/tasks/{id}` - Remove uma tarefa

### Formato das Tarefas
```json
{
    "title": "Título da Tarefa",
    "description": "Descrição detalhada",
    "status": "pendente|em andamento|concluída",
    "due_date": "2024-02-25"
}
```

## 🔐 Autenticação

O sistema utiliza JWT (JSON Web Tokens) para autenticação. Para fazer requisições autenticadas, inclua o token no header:

```
Authorization: Bearer <seu-token>
```

## 🎨 Interface

### Telas Principais
- Login/Registro
- Dashboard de Tarefas
- Formulário de Criação/Edição
- Visualização Detalhada

### Recursos da Interface
- Drag and Drop para atualização de status
- Filtros e Ordenação
- Temas Light/Dark
- Responsividade

## 📁 Estrutura do Projeto
```
desafio-tasks/
├── backend/               # API Laravel
│   ├── app/
│   ├── config/
│   ├── database/
│   ├── routes/
│   └── tests/
├── frontend/             # Aplicação Vue.js
│   ├── src/
│   ├── public/
│   └── components/
├── docker/              # Configurações Docker
│   ├── nginx/
│   └── php/
└── docker-compose.yml
```

## 🔧 Configuração de Desenvolvimento

### Backend
```bash
# Instalar dependências
docker compose exec app composer install

# Rodar migrations
docker compose exec app php artisan migrate

# Gerar chave da aplicação
docker compose exec app php artisan key:generate
```

### Frontend
```bash
# Instalar dependências
docker compose exec frontend npm install

# Rodar em modo desenvolvimento
docker compose exec frontend npm run dev
```

## 🧪 Testes

### Backend
```bash
docker compose exec app php artisan test
```

### Frontend
```bash
docker compose exec frontend npm run test
```

## 📈 Performance

O sistema foi otimizado para:
- Carregamento rápido
- Cache eficiente
- Queries otimizadas
- Baixo consumo de recursos

## 🔒 Segurança

- Proteção contra CSRF
- Sanitização de inputs
- Rate limiting
- Validação de dados
- Proteção contra XSS

## 🤝 Contribuindo

1. Faça o fork do projeto
2. Crie sua feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📝 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.



