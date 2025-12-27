# TaskFlow Constitution

## Project Vision

TaskFlow is an AI-powered todo application that evolves from a simple console app to a fully-featured, cloud-native AI chatbot deployed on Kubernetes. This project demonstrates mastery of Spec-Driven Development, AI-native architecture, and modern cloud technologies—all using **free-tier services**.

## Core Principles

### I. Spec-Driven Development (Non-Negotiable)

Every feature MUST begin with a specification before implementation.

**Guidelines:**
- Write `spec.md` before any code using the `/sp.specify` workflow
- Specifications define WHAT, not HOW
- All acceptance criteria must be testable and verifiable
- No manual coding allowed—refine specs until AI generates correct output

**Rationale:** Spec-driven development ensures clear requirements, reproducible AI-generated code, and traceable implementation decisions. This is the foundation of the hackathon methodology.

### II. AI-First Development

Leverage AI assistance (Claude Code) throughout the entire development lifecycle.

**Guidelines:**
- Use Claude Code for spec generation, planning, task breakdown, and implementation
- Maintain clear context through CLAUDE.md files at each project level
- All AI interactions must be recorded in Prompt History Records (PHRs)
- Verify AI outputs against specifications before acceptance

**Rationale:** AI-first development maximizes productivity and demonstrates the "Architecture of Intelligence" theme. The developer acts as System Architect, not syntax writer.

### III. Test-First (TDD Mandatory)

Tests MUST be written before implementation code.

**Guidelines:**
- Red-Green-Refactor cycle strictly enforced
- Tests must fail before implementation (red phase)
- User approves test cases before implementation begins
- Minimum 80% code coverage for core business logic

**Rationale:** TDD ensures correctness, prevents regressions, and produces self-documenting code. Critical for multi-phase evolution where each phase builds on previous work.

### IV. Free-Tier First

All external services MUST use free-tier offerings that do not require payment.

**Guidelines:**
- Prefer services with generous or always-free tiers
- Document all service limits and plan for graceful degradation
- No credit card charges—free credits or truly free tiers only
- Maintain list of alternative free services as fallbacks

**Rationale:** Demonstrates practical skills accessible to all developers. Ensures the project can be reproduced without financial barriers.

### V. Progressive Architecture

The application MUST evolve through 5 defined phases without breaking previous functionality.

**Guidelines:**
- Each phase builds incrementally on the previous
- Backward compatibility maintained throughout evolution
- Clean separation of concerns enables phase transitions
- Monorepo structure keeps frontend/backend in single context

**Rationale:** Simulates real-world software evolution from MVP to production-grade distributed system.

### VI. Stateless & Cloud-Native Design

All services MUST be designed for horizontal scalability and resilience.

**Guidelines:**
- Server holds NO state between requests (database is source of truth)
- Use JWT for stateless authentication
- Design for Kubernetes deployment from Phase II onward
- Support graceful degradation when external services are unavailable

**Rationale:** Cloud-native design enables the Kubernetes deployment phases and teaches production-ready architecture patterns.

### VII. Simplicity & YAGNI

Implement only what is specified—no premature optimization or feature creep.

**Guidelines:**
- Start simple, add complexity only when required by specs
- No organizational-only abstractions; every abstraction must serve a purpose
- Prefer standard library solutions over external dependencies
- Remove unused code immediately

**Rationale:** Complexity must be justified. The hackathon evaluates spec compliance, not additional features.

## Technology Stack

### Phase I: Console Application

| Layer | Technology | Justification |
|-------|------------|---------------|
| Language | Python 3.13+ | Modern Python with latest features |
| Package Manager | UV | Fast, modern Python package management |
| Data Storage | In-Memory Dict | Simplest solution for Phase I requirements |
| CLI Framework | Rich | Beautiful terminal output, tables, prompts |
| Validation | Pydantic | Type-safe data validation |

### Phase II: Full-Stack Web Application

| Layer | Technology | Justification | Free Tier |
|-------|------------|---------------|-----------|
| Frontend | Next.js 16+ (App Router) | Modern React with Server Components | Vercel (100GB bandwidth) |
| Backend | FastAPI | High-performance async Python API | Render/Railway free tier |
| ORM | SQLModel | SQLAlchemy + Pydantic integration | N/A |
| Database | Neon PostgreSQL | Serverless Postgres | 0.5GB, 190 compute hrs/mo |
| Authentication | Better Auth | Modern auth with JWT support | Self-hosted |

### Phase III: AI Chatbot

| Layer | Technology | Justification | Free Tier |
|-------|------------|---------------|-----------|
| LLM Provider | Groq | Fast inference, completely free tier | Generous rate limits |
| AI Framework | OpenAI Agents SDK | Official SDK for agent development | N/A (uses Groq backend) |
| MCP Server | Official MCP SDK | Model Context Protocol for tools | Open source |
| Chat UI | OpenAI ChatKit | Standard chat interface | Domain allowlist required |
| State Storage | Neon PostgreSQL | Conversation/message persistence | Same as Phase II |

### Phase IV: Local Kubernetes

| Layer | Technology | Justification | Free Tier |
|-------|------------|---------------|-----------|
| Containerization | Docker | Industry standard | Docker Desktop free |
| Local K8s | Minikube | Full K8s locally | Open source |
| Package Manager | Helm Charts | K8s application packaging | Open source |
| AI DevOps | kubectl-ai, kagent | AI-assisted K8s operations | Open source |
| Docker AI | Gordon (if available) | AI-assisted Docker operations | Docker Desktop feature |

### Phase V: Cloud Deployment

| Layer | Technology | Justification | Free Tier |
|-------|------------|---------------|-----------|
| Cloud K8s | Oracle Cloud OKE | Always Free tier (no expiration) | 4 OCPUs, 24GB RAM |
| Event Streaming | Redpanda Cloud | Kafka-compatible, serverless | Free serverless tier |
| Alternative Kafka | Strimzi (self-hosted) | K8s-native Kafka operator | Runs on OKE free tier |
| Runtime | Dapr | Distributed app building blocks | Open source |
| CI/CD | GitHub Actions | Automated deployment pipeline | 2000 mins/mo free |

## Development Constraints

### Mandatory Tooling

- **Claude Code**: All implementation via AI assistant
- **Spec-Kit Plus**: Specification management and workflow
- **UV**: Python package management (not pip)
- **Git**: Version control with meaningful commits

### Code Organization

```
taskflow/
├── .specify/                 # Spec-Kit configuration
│   └── memory/constitution.md
├── specs/                    # Feature specifications
│   ├── features/
│   ├── api/
│   └── database/
├── history/                  # Prompt History Records
│   └── prompts/
├── CLAUDE.md                 # Root AI instructions
├── frontend/                 # Next.js application
│   └── CLAUDE.md
├── backend/                  # FastAPI application
│   └── CLAUDE.md
├── helm/                     # Kubernetes charts
└── dapr/                     # Dapr components
```

### API Design Standards

- RESTful endpoints under `/api/`
- All routes require JWT authentication (Phase II+)
- User isolation: `GET /api/{user_id}/tasks`
- JSON request/response bodies
- Proper HTTP status codes (200, 201, 400, 401, 404, 500)

### Security Requirements

- Never hardcode secrets or tokens
- Use `.env` files for local development
- Kubernetes Secrets or Dapr Secrets for production
- JWT tokens with expiration (7 days default)
- HTTPS only in production

## Quality Standards

### Code Quality

- All code must pass linting (ruff for Python, eslint for TypeScript)
- Type hints required for all Python functions
- TypeScript strict mode enabled
- Functions must have single responsibility
- No commented-out code in commits

### Testing Requirements

| Test Type | Requirement |
|-----------|-------------|
| Unit Tests | 80% coverage minimum for business logic |
| Integration Tests | All API endpoints covered |
| E2E Tests | Critical user journeys (Phase II+) |
| Contract Tests | MCP tool interfaces (Phase III+) |

### Documentation Requirements

- README.md with setup instructions per phase
- CLAUDE.md files at project, frontend, and backend levels
- API documentation via OpenAPI/Swagger
- Spec files for every feature

### Review Process

- All implementations verified against specifications
- PHR created for every significant interaction
- ADR suggested for architectural decisions
- No merging without passing CI checks

## Feature Requirements by Phase

### Phase I: Basic Level (100 points)

- [ ] Add Task (title, description)
- [ ] Delete Task (by ID)
- [ ] Update Task (modify details)
- [ ] View Task List (all tasks with status)
- [ ] Mark as Complete (toggle status)

### Phase II: Basic Level Web (150 points)

- [ ] All Phase I features as web app
- [ ] User authentication (signup/signin)
- [ ] RESTful API endpoints
- [ ] Responsive frontend UI
- [ ] Persistent database storage

### Phase III: AI Chatbot (200 points)

- [ ] Conversational interface for all features
- [ ] MCP Server with 5 tools (add, list, complete, delete, update)
- [ ] Conversation state persistence
- [ ] Natural language understanding
- [ ] Stateless server architecture

### Phase IV: Local K8s (250 points)

- [ ] Containerized frontend and backend
- [ ] Helm charts for deployment
- [ ] Minikube deployment working
- [ ] AI-assisted operations (kubectl-ai)

### Phase V: Cloud + Advanced (300 points)

- [ ] Oracle OKE deployment
- [ ] Kafka/Redpanda event streaming
- [ ] Dapr integration (Pub/Sub, State, Bindings)
- [ ] Intermediate features (priorities, tags, search, filter, sort)
- [ ] Advanced features (recurring tasks, due dates, reminders)
- [ ] CI/CD pipeline with GitHub Actions

## Governance

- This Constitution supersedes all other project documentation
- Amendments require: documented rationale in ADR, version bump, migration plan
- All implementations must verify compliance with Core Principles
- Exceptions require explicit documentation with justification
- Use CLAUDE.md for runtime development guidance

### Version Control

- MAJOR: Breaking changes to principles or architecture
- MINOR: New principles, sections, or significant expansions
- PATCH: Clarifications, typos, non-semantic changes

### Compliance Verification

Every PR/implementation must answer:
1. Does this follow Spec-Driven Development? (Principle I)
2. Was this generated via AI assistance? (Principle II)
3. Were tests written first? (Principle III)
4. Does this use free-tier services only? (Principle IV)
5. Does this maintain backward compatibility? (Principle V)

**Version**: 1.0.0 | **Ratified**: 2025-12-27 | **Last Amended**: 2025-12-27
