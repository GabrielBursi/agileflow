# AgileFlow - Sistema de Gestão de Projetos Ágeis

![Java](https://img.shields.io/badge/java-%23ED8B00.svg?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring](https://img.shields.io/badge/spring-%236DB33F.svg?style=for-the-badge&logo=spring&logoColor=white)
![Next JS](https://img.shields.io/badge/Next-black?style=for-the-badge&logo=next.js&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white)

Uma plataforma de gestão de projetos ágeis moderna implementando metodologias Scrum e Kanban, com foco em segurança e controle granular de acesso baseado em roles (RBAC).

## 🚀 Características Principais

- **Controle de Acesso Granular**: Sistema RBAC com roles específicos por projeto
- **Autenticação Robusta**: JWT com refresh tokens e renovação automática
- **Multi-Projeto**: Gerenciamento de múltiplos projetos com contextos isolados
- **Metodologias Ágeis**: Suporte completo para Scrum e Kanban
- **Arquitetura Escalável**: DDD + Clean Architecture + Hexagonal Architecture

## 🏗️ Arquitetura

```
Frontend (Next + TypeScript)
         ↕ HTTP/REST
Backend (Java + Spring Boot)
         ↕ SQL
Database (PostgreSQL)
```

O sistema segue os princípios de **Domain-Driven Design (DDD)** com **Clean Architecture**, organizando o código em camadas bem definidas onde as regras de negócio ficam no centro, independentes de frameworks e tecnologias externas.

## 📋 Funcionalidades

### 🔐 Gestão de Identidade
- Registro e autenticação de usuários
- Sistema JWT com refresh tokens
- Controle de acesso baseado em roles

### 🏢 Gestão Organizacional
- Criação de organizações (workspaces)
- Gerenciamento de projetos
- Convites e permissões por projeto

### 📊 Gestão de Sprints
- Criação e gerenciamento de sprints
- Product backlog
- Planejamento e estimativas

### ✅ Gestão de Tasks
- Tasks com diferentes tipos (Story, Bug, Epic, Task)
- Workflow de status configurável
- Comentários e anexos
- Time tracking

### 📈 Relatórios e Métricas
- Burndown charts
- Métricas de produtividade
- Relatórios de entrega
- Distribuição de trabalho

## 🛠️ Stack Tecnológica

### Backend
- **Java 17+** - Linguagem principal
- **Spring Boot 3.x** - Framework web
- **Spring Security 6.x** - Segurança e autenticação
- **Spring Data JPA** - Persistência de dados
- **PostgreSQL** - Banco de dados
- **JWT** - Tokens de autenticação

### Frontend
- **Next** - Framework UI
- **TypeScript** - Tipagem estática
- **Tailwind CSS** - Estilização
- **Next Router** - Roteamento
- **Zustand** - Gerenciamento de estado

### DevOps
- **Docker** - Containerização
- **Docker Compose** - Orquestração local
- **GitHub Actions** - CI/CD

## 🚦 Getting Started

### Pré-requisitos
- Java 17+
- Node.js 18+
- Docker e Docker Compose
- PostgreSQL (ou use Docker)

### Instalação e Execução

1. **Clone o repositório**
```bash
git clone https://github.com/seu-usuario/agileflow.git
cd agileflow
```

2. **Execute com Docker Compose**
```bash
docker-compose up -d
```

3. **Ou execute localmente**
```bash
# Backend
cd backend
./mvnw spring-boot:run

# Frontend
cd frontend
npm install
npm run dev
```

A aplicação estará disponível em:
- Frontend: http://localhost:3000
- Backend API: http://localhost:8080
- Swagger UI: http://localhost:8080/swagger-ui.html

## 📚 Documentação

- [📋 Requisitos](docs/REQUIREMENTS.md) - Requisitos funcionais e não funcionais
- [🏗️ System Design](docs/SYSTEM_DESIGN.md) - Arquitetura e design do sistema *(em construção 🚧)*
- [🗃️ Database Design](docs/DATABASE.md) - Modelo de dados e otimizações *(em construção 🚧)*
- [🔧 API Documentation](docs/API.md) - Documentação da API REST *(em construção 🚧)*
- [🏛️ Architecture](docs/ARCHITECTURE.md) - Detalhes da arquitetura DDD *(em construção 🚧)*
- [🚀 Deployment](docs/DEPLOYMENT.md) - Guia de deploy e configuração *(em construção 🚧)*


