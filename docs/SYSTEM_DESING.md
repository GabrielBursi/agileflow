# System Design - AgileFlow

## Bounded Contexts (DDD)

### Identity & Access Management
- **Responsabilidade**: Autenticação, autorização e gestão de usuários
- **Entities**: User, Role, Permission, RefreshToken
- **Value Objects**: Email, Password, JWT

### Organization Management
- **Responsabilidade**: Organizações e projetos
- **Entities**: Organization, Project, ProjectMember
- **Value Objects**: ProjectStatus, MembershipRole

### Sprint Management
- **Responsabilidade**: Sprints e planejamento
- **Entities**: Sprint, Backlog
- **Value Objects**: SprintStatus, Velocity

### Task Management
- **Responsabilidade**: Tasks e workflow
- **Entities**: Task, Comment, Attachment
- **Value Objects**: TaskStatus, StoryPoints, TaskType
