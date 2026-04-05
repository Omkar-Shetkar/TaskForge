# Implementation Plan: TaskForge MVP

**Branch**: `001-taskforge-mvp` | **Date**: 2026-04-05 | **Spec**: [specs/001-taskforge-mvp/spec.md](spec.md)
**Input**: Feature specification from `/specs/001-taskforge-mvp/spec.md`

## Summary

TaskForge is a collaborative task orchestrator web app for small-to-medium teams. The MVP focuses on real-time task management, AI-assisted planning (subtask generation, risk detection), and project visualization (Kanban, List, Calendar). The technical approach uses a Next.js + Tailwind frontend with a Spring Boot backend, leveraging Gemini for AI features and WebSockets for real-time synchronization.

## Technical Context

**Language/Version**: Java 21 (Spring Boot 3.2), TypeScript (Next.js 14)
**Primary Dependencies**: Spring Boot, Spring Security, Spring WebFlux (for real-time), Next.js, Tailwind CSS, Gemini API, JDBC/JPA
**Storage**: PostgreSQL (Relational data), Redis (Real-time state/caching), S3-compatible storage (Attachments)
**Testing**: JUnit 5, Mockito, Playwright (E2E)
**Target Platform**: Web (Responsive Desktop & Mobile)
**Project Type**: web-service + web-app
**Performance Goals**: <500ms real-time sync latency, <1s dashboard load time
**Constraints**: <10MB file attachments, project-level RBAC, "Last Write Wins" concurrency
**Scale/Scope**: 2-20 users per team, up to 10k tasks per workspace

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

- [x] **I. Library-First**: Core logic (AI, task management) will be isolated into independent modules/packages.
- [x] **II. CLI Interface**: (Note: While primarily a web app, core logic should be testable via CLI/scripts).
- [x] **III. Test-First**: TDD will be followed for core services and AI integrations.
- [x] **IV. Integration Testing**: Contract tests for Spring Boot APIs and inter-service communication.
- [x] **V. Simplicity**: YAGNI applied to out-of-scope features (Gantt, Time tracking).

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
    └── requirements.md  # Specification quality check
```

### Source Code (repository root)

```text
backend/
├── src/
│   ├── main/
│   │   ├── java/com/taskforge/
│   │   │   ├── core/           # Business logic (Library-First)
│   │   │   ├── api/            # REST Controllers
│   │   │   ├── security/       # Auth & RBAC
│   │   │   └── ai/              # Gemini Integration
│   │   └── resources/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   ├── services/
│   └── hooks/          # Real-time/AI hooks
└── tests/
```

**Structure Decision**: Web application (Next.js + Spring Boot) with a modular backend to satisfy the Library-First principle.

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

*No violations identified.*
