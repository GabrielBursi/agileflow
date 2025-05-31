# System Design - AgileFlow

## Arquitetura de Alto Nível

O sistema segue Domain-Driven Design (DDD), Clean Architecture e Hexagonal Architecture com camadas bem definidas:

```
┌─────────────────────────────────────────────────────────────┐
│                    Frontend (React)                         │
│  ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐│
│  │   Auth Module   │ │ Project Module  │ │   Task Module   ││
│  └─────────────────┘ └─────────────────┘ └─────────────────┘│
└─────────────────────────────────────────────────────────────┘
                              │ HTTP/REST
┌─────────────────────────────────────────────────────────────┐
│                Backend (Java/Spring Boot)                  │
│  ┌─────────────────────────────────────────────────────────┐│
│  │               Infrastructure Layer                      ││
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────────────┐││
│  │  │ Spring Web  │ │ Spring Sec  │ │    JPA/Hibernate    │││
│  │  │ Controllers │ │   + JWT     │ │    Repositories     │││
│  │  └─────────────┘ └─────────────┘ └─────────────────────┘││
│  └─────────────────────────────────────────────────────────┘│
│  ┌─────────────────────────────────────────────────────────┐│
│  │                Application Layer                        ││
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────────────┐││
│  │  │   Use Cases │ │    DTOs     │ │     Services        │││
│  │  │  @Service   │ │             │ │    @Component       │││
│  │  └─────────────┘ └─────────────┘ └─────────────────────┘││
│  └─────────────────────────────────────────────────────────┘│
│  ┌─────────────────────────────────────────────────────────┐│
│  │                  Domain Layer                           ││
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────────────┐││
│  │  │  Entities   │ │ Value Objs  │ │  Domain Services    │││
│  │  │   @Entity   │ │  @Embeddable│ │      @Service       │││
│  │  └─────────────┘ └─────────────┘ └─────────────────────┘││
│  └─────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────┘
                              │
┌─────────────────────────────────────────────────────────────┐
│                    Database (PostgreSQL)                   │
└─────────────────────────────────────────────────────────────┘
```

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

## Comunicação Entre Contextos

Os contextos se comunicam através de:
- **Domain Events** (Spring Application Events)
- **Application Services** com baixo acoplamento

**Exemplos:**
- Usuário removido → Evento → Remove suas tasks
- Sprint finalizado → Evento → Calcula métricas
- Mudança de permissão → Evento → Invalida tokens

## Estrutura de Diretórios

```
src/main/java/com/agileflow/
├── domain/                 # Domain Layer (Core Business Logic)
│   ├── shared/            # Shared kernel
│   │   ├── entity/        # Base entity classes
│   │   ├── valueobject/   # Common value objects  
│   │   └── event/         # Domain events
│   ├── identity/          # Identity & Access Management BC
│   │   ├── entity/        # User, Role, etc.
│   │   ├── valueobject/   # Email, Password, etc.
│   │   ├── repository/    # Repository interfaces
│   │   └── service/       # Domain services
│   ├── organization/      # Organization Management BC
│   ├── sprint/           # Sprint Management BC
│   └── task/             # Task Management BC
├── application/           # Application Layer
│   ├── shared/           # Shared application services
│   ├── identity/         # Auth use cases
│   │   ├── usecase/      # Use case implementations
│   │   ├── dto/          # DTOs
│   │   └── service/      # Application services
│   ├── organization/     # Org use cases
│   ├── sprint/          # Sprint use cases
│   └── task/            # Task use cases
├── infrastructure/       # Infrastructure Layer
│   ├── persistence/     # JPA entities and repositories
│   │   ├── entity/      # JPA entities
│   │   ├── repository/  # JPA repository implementations
│   │   └── config/     # Database configuration
│   ├── security/       # Security implementations
│   │   ├── jwt/        # JWT utilities
│   │   ├── config/     # Security configuration
│   │   └── filter/     # Security filters
│   └── external/       # External services
└── interfaces/          # Interface Adapters
    ├── rest/           # REST controllers
    │   ├── controller/ # Controllers
    │   ├── dto/       # Request/Response DTOs
    │   └── mapper/    # DTO mappers
    ├── config/        # Spring configuration
    └── exception/     # Global exception handling
```

## Princípios Arquiteturais

### 1. Separação de Responsabilidades
- **Domain Layer**: Regras de negócio puras
- **Application Layer**: Orquestração de use cases
- **Infrastructure Layer**: Detalhes técnicos
- **Interface Layer**: Adaptadores externos

### 2. Inversão de Dependência
- Domain define interfaces
- Infrastructure implementa interfaces
- Application usa abstrações

### 3. Independência de Framework
- Domain não conhece Spring
- Lógica de negócio testável isoladamente
- Flexibilidade para mudanças tecnológicas

## Padrões de Design Utilizados

### Repository Pattern
```java
// Domain
public interface TaskRepository {
    Optional<Task> findById(TaskId id);
    void save(Task task);
}

// Infrastructure
@Repository
public class JpaTaskRepository implements TaskRepository {
    // Implementação com Spring Data JPA
}
```

### Use Case Pattern
```java
@Service
@Transactional
public class AssignTaskUseCase {
    public Result<Void> execute(AssignTaskCommand command) {
        // 1. Validar
        // 2. Buscar entidades
        // 3. Verificar permissões
        // 4. Executar lógica de domínio
        // 5. Persistir
    }
}
```

### Domain Events
```java
// Domain Entity
public class Task {
    public void assignTo(UserId assignee) {
        // Lógica de negócio
        DomainEventPublisher.publish(new TaskAssignedEvent(this.id, assignee));
    }
}

// Event Handler
@EventListener
public void handleTaskAssigned(TaskAssignedEvent event) {
    // Lógica complementar
}
```

## Segurança

### JWT + Refresh Token Flow
```
1. Login → JWT (15min) + Refresh Token (7 dias)
2. Request → JWT no header Authorization
3. JWT expira → Renovação automática com Refresh Token
4. Refresh Token expira → Novo login necessário
```

### RBAC (Role-Based Access Control)
```java
@PreAuthorize("hasPermission(#projectId, 'PROJECT', 'MANAGE_TASKS')")
public void createTask(UUID projectId, CreateTaskRequest request) {
    // Lógica do método
}
```

## Observabilidade

### Logging Estruturado
```java
@Slf4j
public class TaskService {
    public void createTask(CreateTaskCommand command) {
        log.info("Creating task", 
            kv("projectId", command.projectId()),
            kv("userId", command.userId()),
            kv("taskType", command.type())
        );
    }
}
```

### Métricas com Micrometer
- Contadores de requests por endpoint
- Timers de performance
- Gauges de recursos do sistema
- Métricas customizadas de negócio

### Health Checks
- Database connectivity
- External services status
- Memory/CPU usage
- Custom business metrics