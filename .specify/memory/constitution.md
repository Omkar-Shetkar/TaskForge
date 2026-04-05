<!--
Sync Impact Report:
- Version change: [PROJECT_SETUP] → 1.0.0
- List of modified principles:
  - [PRINCIPLE_1_NAME] → I. Library-First
  - [PRINCIPLE_2_NAME] → II. Test-Driven Development (TDD)
  - [PRINCIPLE_3_NAME] → III. Domain-Driven Design (DDD)
  - [PRINCIPLE_4_NAME] → IV. Real-time Collaboration
  - [PRINCIPLE_5_NAME] → V. AI-Assisted Orchestration
- Added sections: Additional Constraints, Development Workflow
- Removed sections: None
- Templates requiring updates:
  - .specify/templates/plan-template.md (✅ updated)
  - .specify/templates/spec-template.md (✅ updated)
  - .specify/templates/tasks-template.md (✅ updated)
- Follow-up TODOs: None
-->

# TaskForge Constitution

## Core Principles

### I. Library-First
Every feature starts as a standalone library or module. Logic must be self-contained, independently 
testable, and documented. This reduces coordination overhead by ensuring clear boundaries between 
components and promoting reuse across the orchestrator.

### II. Test-Driven Development (TDD)
We follow a strict TDD approach. Tests MUST be written and approved before implementation starts. 
The Red-Green-Refactor cycle is non-negotiable for all core business logic, API contracts, and 
AI integrations.

### III. Domain-Driven Design (DDD)
We employ DDD and standard design patterns to model the task orchestration domain. Design MUST be 
pragmatic—aiming for clarity and maintainability without over-engineering. Core entities and 
services MUST reflect the ubiquitous language of the project.

### IV. Real-time Collaboration
Real-time synchronization is a first-class citizen. All collaborative features (task updates, 
comments, mentions) MUST prioritize low-latency sync (<500ms) to ensure a seamless "live" 
experience for teams.

### V. AI-Assisted Orchestration
AI is integrated to reduce cognitive load and coordination overhead. AI features (subtask 
generation, risk detection) MUST be verifiable, provide interactive feedback, and strictly 
align with user intent.

## Additional Constraints

- **Tech Stack**: The project uses Next.js + Tailwind for the frontend and Spring Boot (Java) 
  for the backend. Gemini is the primary AI provider.
- **Concurrency**: A "Last Write Wins" strategy is used for simple, predictable conflict 
  resolution during the MVP phase.
- **Privacy**: Project-level RBAC is mandatory; tasks and project data MUST only be visible 
  to assigned project members.

## Development Workflow

- **Sequence**: Every feature MUST follow the Sequential Execution flow: Specification 
  → Implementation Planning → Task Decomposition → Implementation → Validation.
- **Quality Gates**: All pull requests MUST pass the TDD test suite, linting, and a peer 
  review that explicitly verifies compliance with these principles.
- **Documentation**: Specifications and implementation plans are living documents and 
  MUST be updated in lockstep with code changes.

## Governance

- This constitution supersedes all other local practices or patterns.
- Amendments require a formal update via the `/speckit.constitution` command and 
  consensus among core maintainers.
- Versioning follows Semantic Versioning (SemVer) rules: MAJOR for principle changes, 
  MINOR for additions, PATCH for clarifications.
- Compliance reviews are expected during every planning and implementation phase.

**Version**: 1.0.0 | **Ratified**: 2026-04-05 | **Last Amended**: 2026-04-05
