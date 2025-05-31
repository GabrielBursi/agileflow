# Requisitos do Sistema - AgileFlow

## Visão Geral

O AgileFlow resolve o problema de ferramentas que são muito simples (sem controle adequado) ou muito complexas (difíceis de usar), oferecendo:
- Controle de acesso granular sem complexidade desnecessária
- Workflows ágeis padronizados mas flexíveis  
- Autenticação segura com experiência fluida
- Gerenciamento de múltiplos projetos com contextos isolados

## Requisitos Funcionais

### 🔐 Gestão de Usuários e Autenticação
- **RF001**: Registro de usuários com validação de email
- **RF002**: Autenticação JWT com refresh tokens
- **RF003**: Login/logout seguro
- **RF004**: Renovação automática de tokens
- **RF005**: Diferentes roles por projeto para mesmo usuário

### 🏢 Gestão de Organizações e Projetos
- **RF006**: Criação de organizações (workspaces)
- **RF007**: Criação de projetos dentro de organizações
- **RF008**: Convites com roles específicos
- **RF009**: Alternância entre projetos mantendo contexto
- **RF010**: Soft delete para projetos e organizações

### 👥 Gestão de Roles e Permissões
- **RF011**: Roles: Organization Admin, Product Owner, Scrum Master, Developer Senior, Developer Junior
- **RF012**: Permissões granulares baseadas em recursos e contexto
- **RF013**: Validação de permissões no backend e frontend
- **RF014**: Organization Admins gerenciam roles de usuários

### 📊 Gestão de Sprints e Backlog
- **RF015**: Product Owners criam e gerenciam product backlog
- **RF016**: Scrum Masters criam e gerenciam sprints
- **RF017**: Workflow: Backlog → To Do → In Progress → In Review → Done
- **RF018**: Estimativas de story points
- **RF019**: Cálculo automático de velocity

### ✅ Gestão de Tasks
- **RF020**: Tasks com tipos: Story, Bug, Epic, Task
- **RF021**: Developers auto-atribuem tasks disponíveis
- **RF022**: Comentários e anexos em tasks
- **RF023**: Controle de transições baseado em roles
- **RF024**: Time tracking opcional

### 📈 Relatórios e Métricas
- **RF025**: Burndown charts por sprint
- **RF026**: Métricas de produtividade por developer
- **RF027**: Métricas de entrega para Product Owners
- **RF028**: Relatórios de distribuição de trabalho

## Requisitos Não Funcionais

### 🔒 Segurança
- **RNF001**: Senhas hasheadas com BCrypt
- **RNF002**: JWTs com expiração de 15 minutos
- **RNF003**: Refresh tokens com 7 dias e rotação automática
- **RNF004**: Todas as rotas protegidas por autenticação
- **RNF005**: Validação de permissões em cada request

### ⚡ Performance
- **RNF006**: API responde em <200ms para CRUD simples
- **RNF007**: Consultas complexas em <1 segundo
- **RNF008**: Suporte a 1000+ usuários concorrentes
- **RNF009**: Frontend carrega em <3 segundos

### 🎨 Usabilidade
- **RNF010**: Interface responsiva (mobile-first)
- **RNF011**: Funcionamento offline para dados carregados
- **RNF012**: Feedback visual para todas as ações
- **RNF013**: Loading states e error boundaries

### 📈 Escalabilidade
- **RNF014**: Arquitetura permite novos módulos sem refatoração
- **RNF015**: Banco suporta crescimento horizontal
- **RNF016**: Deploy em containers
- **RNF017**: Logs estruturados para observabilidade

## Matriz de Permissões por Role

| Ação | Org Admin | Product Owner | Scrum Master | Dev Senior | Dev Junior |
|------|-----------|---------------|--------------|------------|------------|
| Gerenciar usuários do projeto | ✅ | ❌ | ❌ | ❌ | ❌ |
| Criar/editar projetos | ✅ | ❌ | ❌ | ❌ | ❌ |
| Gerenciar backlog | ✅ | ✅ | ❌ | ❌ | ❌ |
| Criar/gerenciar sprints | ✅ | ✅ | ✅ | ❌ | ❌ |
| Criar tasks | ✅ | ✅ | ✅ | ✅ | ✅ |
| Atribuir tasks | ✅ | ✅ | ✅ | ✅ | ❌ |
| Mover tasks para Done | ✅ | ✅ | ✅ | ✅ | ❌ |
| Ver relatórios completos | ✅ | ✅ | ✅ | ❌ | ❌ |

## Casos de Uso Principais

### 1. Criação de Projeto
**Ator**: Organization Admin  
**Fluxo**:
1. Admin acessa dashboard
2. Clica em "Novo Projeto"
3. Preenche nome, chave e descrição
4. Convida membros com roles específicos
5. Projeto é criado e membros recebem notificação

### 2. Planejamento de Sprint
**Ator**: Scrum Master  
**Fluxo**:
1. Acessa backlog do projeto
2. Seleciona tasks prioritizadas pelo PO
3. Define datas e goal do sprint
4. Tasks movem para sprint backlog
5. Sprint inicia automaticamente na data definida

### 3. Desenvolvimento de Task
**Ator**: Developer  
**Fluxo**:
1. Acessa board do sprint
2. Seleciona task disponível (auto-atribuição)
3. Move task: To Do → In Progress
4. Desenvolve e comenta progresso
5. Move para In Review
6. Após aprovação, move para Done

### 4. Acompanhamento de Progresso
**Ator**: Product Owner  
**Fluxo**:
1. Acessa dashboard do projeto
2. Visualiza burndown chart
3. Verifica tasks completadas/pendentes
4. Analisa velocity da equipe
5. Reprioritiza backlog se necessário