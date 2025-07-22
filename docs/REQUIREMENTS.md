# Requisitos do Sistema - AgileFlow (Atualizado)

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
- **RF006**: Verificação de email após registro
- **RF007**: Recovery de senha via email

### 🏢 Gestão de Organizações e Projetos

- **RF008**: Criação de organizações (workspaces)
- **RF009**: Criação de projetos dentro de organizações
- **RF010**: Convites com roles específicos via email
- **RF011**: Alternância entre projetos mantendo contexto
- **RF012**: Soft delete para projetos e organizações
- **RF013**: Dashboard organizacional com métricas gerais

### 👥 Gestão de Roles e Permissões

- **RF014**: Roles: Organization Admin, Product Owner, Scrum Master, Developer Senior, Developer Junior
- **RF015**: Permissões granulares baseadas em recursos e contexto
- **RF016**: Validação de permissões no backend e frontend
- **RF017**: Organization Admins gerenciam roles de usuários
- **RF018**: Histórico de mudanças de roles

### 📊 Gestão de Sprints e Backlog

- **RF019**: Product Owners criam e gerenciam product backlog
- **RF020**: Scrum Masters criam e gerenciam sprints
- **RF021**: Workflow: Backlog → To Do → In Progress → In Review → Done
- **RF022**: Estimativas de story points
- **RF023**: Cálculo automático de velocity
- **RF024**: Planning poker para estimativas colaborativas
- **RF025**: Retrospectivas de sprint com ações de melhoria
- **RF026**: Sprint reviews com demo e feedback
- **RF027**: Definição de critérios de aceite por story

### ✅ Gestão de Tasks

- **RF028**: Tasks com tipos: Story, Bug, Epic, Task, Spike
- **RF029**: Developers auto-atribuem tasks disponíveis
- **RF030**: Comentários e anexos em tasks
- **RF031**: Controle de transições baseado em roles
- **RF032**: Time tracking opcional
- **RF033**: Subtasks e dependências entre tasks
- **RF034**: Tags customizáveis por projeto
- **RF035**: Histórico completo de mudanças
- **RF036**: Notificações em tempo real para mudanças

### 📈 Relatórios e Métricas

- **RF037**: Burndown charts por sprint
- **RF038**: Métricas de produtividade por developer
- **RF039**: Métricas de entrega para Product Owners
- **RF040**: Relatórios de distribuição de trabalho
- **RF041**: Velocity tracking histórico
- **RF042**: Lead time e cycle time por task
- **RF043**: Cumulative flow diagram
- **RF044**: Relatórios de qualidade (bugs vs features)

### 🔔 Sistema de Notificações

- **RF045**: Notificações in-app em tempo real
- **RF046**: Notificações por email configuráveis
- **RF047**: Digest semanal de atividades
- **RF048**: Alertas para deadlines e bloqueios
- **RF049**: Preferências de notificação por usuário

### 📱 Features Colaborativas

- **RF050**: Comentários com menções (@user)
- **RF051**: Chat integrado por task
- **RF052**: Quadro Kanban interativo
- **RF053**: Calendário de eventos e deadlines
- **RF054**: Wiki/documentação por projeto

## Requisitos Não Funcionais

### 🔒 Segurança

- **RNF001**: Senhas hasheadas com BCrypt
- **RNF002**: JWTs com expiração de 15 minutos
- **RNF003**: Refresh tokens com 7 dias e rotação automática
- **RNF004**: Todas as rotas protegidas por autenticação
- **RNF005**: Validação de permissões em cada request
- **RNF006**: Rate limiting para APIs
- **RNF007**: Logs de auditoria para ações sensíveis
- **RNF008**: Criptografia de dados sensíveis

### ⚡ Performance

- **RNF009**: API responde em <200ms para CRUD simples
- **RNF010**: Consultas complexas em <1 segundo
- **RNF011**: Suporte a 1000+ usuários concorrentes
- **RNF012**: Frontend carrega em <3 segundos
- **RNF013**: WebSocket para updates em tempo real
- **RNF014**: Cache Redis para dados frequentes
- **RNF015**: Lazy loading para listas grandes

### 🎨 Usabilidade

- **RNF016**: Interface responsiva (mobile-first)
- **RNF017**: Funcionamento offline para dados carregados
- **RNF018**: Feedback visual para todas as ações
- **RNF019**: Loading states e error boundaries
- **RNF020**: Drag & drop para Kanban board
- **RNF021**: Atalhos de teclado para ações frequentes
- **RNF022**: Dark mode e temas personalizáveis

### 📈 Escalabilidade

- **RNF023**: Arquitetura permite novos módulos sem refatoração
- **RNF024**: Banco suporta crescimento horizontal
- **RNF025**: Deploy em containers
- **RNF026**: Logs estruturados para observabilidade
- **RNF027**: Monitoramento de saúde da aplicação
- **RNF028**: Backup automático diário
- **RNF029**: CDN para assets estáticos

### 🔄 Integrações

- **RNF030**: API REST documentada (OpenAPI)
- **RNF031**: Webhooks para eventos importantes
- **RNF032**: Integração com Git (GitHub/GitLab)
- **RNF033**: Importação/exportação de dados
- **RNF034**: Single Sign-On (SSO) opcional

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
| Conduzir retrospectivas | ✅ | ❌ | ✅ | ❌ | ❌ |
| Planning poker | ✅ | ✅ | ✅ | ✅ | ✅ |
| Ver relatórios completos | ✅ | ✅ | ✅ | ❌ | ❌ |
| Configurar integrações | ✅ | ❌ | ❌ | ❌ | ❌ |

## Casos de Uso Principais (Expandidos)

### 1. Onboarding de Usuário
**Ator**: Novo usuário  
**Fluxo**:
1. Usuário se registra com email
2. Recebe email de verificação
3. Confirma email e é redirecionado
4. Completa perfil básico
5. Recebe tour da aplicação

### 2. Criação de Projeto
**Ator**: Organization Admin  
**Fluxo**:
1. Admin acessa dashboard
2. Clica em "Novo Projeto"
3. Preenche nome, chave e descrição
4. Configura workflow e tipos de task
5. Convida membros com roles específicos
6. Projeto é criado e membros recebem notificação

### 3. Planning Poker
**Ator**: Equipe de desenvolvimento  
**Fluxo**:
1. Scrum Master inicia sessão de planning
2. Seleciona story do backlog
3. Equipe discute requisitos
4. Cada membro vota secretamente
5. Votos são revelados simultaneamente
6. Em caso de divergência, nova discussão
7. Story recebe estimativa final

### 4. Sprint Planning
**Ator**: Scrum Master + Product Owner  
**Fluxo**:
1. PO prioriza backlog
2. SM cria nova sprint
3. Equipe estima capacidade
4. Selecionam stories baseado na velocity
5. Definem sprint goal
6. Tasks são criadas e estimadas
7. Sprint é iniciada

### 5. Daily Workflow
**Ator**: Developer  
**Fluxo**:
1. Acessa board do sprint
2. Vê tasks atribuídas e impedimentos
3. Atualiza status das tasks
4. Adiciona comentários sobre progresso
5. Identifica e reporta bloqueios
6. Auto-atribui nova task se disponível

### 6. Code Review Process
**Ator**: Developer Senior  
**Fluxo**:
1. Developer move task para "In Review"
2. Notificação é enviada para reviewers
3. Senior acessa task e vê detalhes
4. Faz review do código (integração Git)
5. Aprova ou solicita mudanças
6. Task move para Done ou volta para In Progress

### 7. Sprint Review & Retrospective
**Ator**: Scrum Master  
**Fluxo**:
1. SM encerra sprint automaticamente
2. Gera relatório de conclusão
3. Conduz demo com stakeholders
4. Coleta feedback sobre entrega
5. Facilita retrospectiva da equipe
6. Define ações de melhoria
7. Inicia planejamento da próxima sprint

### 8. Monitoramento de Progresso
**Ator**: Product Owner  
**Fluxo**:
1. Acessa dashboard do projeto
2. Visualiza burndown chart atualizado
3. Analisa velocity e tendências
4. Revisa tasks em andamento
5. Identifica riscos e impedimentos
6. Reprioriza backlog conforme necessário
7. Comunica status para stakeholders

## Eventos do Sistema (Event Storming)

### Eventos de Usuário
- Usuário registrado
- Email verificado
- Usuário logado/deslogado
- Perfil atualizado
- Senha alterada

### Eventos de Projeto
- Organização criada
- Projeto criado
- Membro convidado
- Convite aceito/rejeitado
- Role atualizada

### Eventos de Sprint
- Backlog priorizado
- Sprint criada
- Sprint iniciada
- Sprint finalizada
- Retrospectiva realizada

### Eventos de Task
- Task criada
- Task atribuída
- Status atualizado
- Comentário adicionado
- Task completada
- Tempo registrado

### Eventos de Colaboração
- Planning poker iniciado
- Voto registrado
- Estimativa definida
- Review solicitada
- Aprovação dada
- Bloqueio reportado

## Integrações Planejadas

### Repositórios de Código
- GitHub
- GitLab
- Bitbucket

### Comunicação
- Slack
- Microsoft Teams
- Discord

### Ferramentas de Design
- Figma
- Adobe XD

### Monitoramento
- Sentry (error tracking)
- Analytics próprio
