# Implementation Plan: TaskForge MVP

**Branch**: `001-taskforge-mvp` | **Date**: 2026-04-05 | **Spec**: [specs/001-taskforge-mvp/spec.md](spec.md)
**Input**: Feature specification from `/specs/001-taskforge-mvp/spec.md`

## Summary

TaskForge is a modern collaborative task orchestrator web app. The implementation is divided into six logical modules:
1. **Auth & Workspace Security**: Core authentication and project-level RBAC.
2. **Project Management**: Foundational workspace/project structure and team member invitations.
3. **Task Core**: Hierarchical task management (subtasks) with progress rollup and status tracking.
4. **Collaboration (Real-time)**: WebSocket-driven updates, task comments, and @mention notifications.
5. **AI Features**: Gemini-powered subtask generation, risk analysis, and priority suggestions.
6. **Dashboard & Visualization**: Multi-view (Kanban, List, Calendar) displays and progress reporting (CSV/PDF).

The technical approach utilizes a **Next.js + Tailwind** frontend and a **Spring Boot (Java 21)** backend, with **WebSockets** for real-time sync and **Gemini API** for AI capabilities.

## Technical Context

**Language/Version**: Java 21 (Spring Boot 3.2), TypeScript (Next.js 14)
**Primary Dependencies**: Spring Boot, Spring Security, Spring WebFlux, Next.js, Tailwind CSS, Gemini API, JDBC/JPA
**Storage**: PostgreSQL (Relational), Redis (Real-time/Cache), S3-compatible (Attachments)
**Testing**: JUnit 5, Mockito, Playwright (E2E)
**Target Platform**: Web (Responsive)
**Project Type**: web-service + web-app
**Performance Goals**: <500ms sync latency, <1s dashboard load time
**Constraints**: <10MB attachments, project-level RBAC, "Last Write Wins" concurrency
**Scale/Scope**: 2-20 users per team, up to 10k tasks per workspace

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

- [x] **I. Library-First**: Core logic (AI services, Task domain) isolated into independent modules.
- [x] **II. CLI Interface**: Core logic testable via scripts/CLI tools.
- [x] **III. Test-First**: TDD followed for all core services and API contracts.
- [x] **IV. Integration Testing**: Contract tests for all internal and external APIs.
- [x] **V. Simplicity**: Pragmatic DDD approach, avoiding over-engineering for out-of-scope features.

## Project Structure

### Documentation (this feature)

```text
specs/001-taskforge-mvp/
├── plan.md              # This file
├── research.md          # Phase 0 output
├── data-model.md        # Phase 1 output
├── quickstart.md        # Phase 1 output
├── contracts/           # Phase 1 output
└── checklists/
    └── requirements.md  # Spec quality check
```

### Source Code (repository root)

```text
backend/
├── src/
│   ├── main/
│   │   ├── java/com/taskforge/
│   │   │   ├── auth/           # Module 1: Auth & Security
│   │   │   ├── project/        # Module 2: Project Management
│   │   │   ├── task/           # Module 3: Task Core
│   │   │   ├── collab/         # Module 4: Collaboration
│   │   │   ├── ai/             # Module 5: AI Features
│   │   │   └── report/         # Module 6: Dashboard & Reporting
│   │   └── resources/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   ├── services/       # Module-specific API clients
│   └── hooks/          # Real-time and AI-specific hooks
└── tests/
```

**Structure Decision**: Web application (Next.js + Spring Boot) with a modular backend reflecting the 6 logical implementation phases.

## Complexity Tracking

*No violations identified.*
