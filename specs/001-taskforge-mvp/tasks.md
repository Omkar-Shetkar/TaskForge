# Tasks: TaskForge MVP

**Input**: Design documents from `/specs/001-taskforge-mvp/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: TDD approach is MANDATORY per the project constitution. Write and FAIL tests before implementation.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Parallelizable (different files, no dependencies)
- **[Story]**: User story label (e.g., [US1], [US2])
- Paths assume `backend/src/` and `frontend/src/` per plan.md

---

## Phase 1: Setup

**Purpose**: Project initialization and basic structure

- [x] T001 Create project structure for `backend/` and `frontend/` per implementation plan
- [x] T002 [P] Initialize Spring Boot 3.2 project in `backend/` with Java 21
- [x] T003 [P] Initialize Next.js 14 project in `frontend/` with TypeScript and Tailwind CSS
- [x] T004 [P] Configure shared linting, formatting, and CI rules for both projects

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure for authentication, database, and real-time sync

**⚠️ CRITICAL**: Must complete before starting user stories

- [ ] T005 Setup PostgreSQL database schema and migrations in `backend/src/main/resources/db/migration/`
- [ ] T006 Implement base security framework with Spring Security in `backend/src/main/java/com/taskforge/auth/`
- [ ] T007 [P] Implement S3-compatible attachment storage with signed URLs in `backend/src/main/java/com/taskforge/core/service/FileStorageService.java`
- [ ] T008 [P] Configure Redis for real-time state management in `backend/src/main/resources/application.properties`
- [ ] T009 [P] Setup WebSocket and STOMP configuration in `backend/src/main/java/com/taskforge/collab/WebSocketConfig.java`
- [ ] T010 Implement generic Error Handling and Logging infrastructure in `backend/src/main/java/com/taskforge/core/error/`

**Checkpoint**: Foundation ready - US implementation can begin

---

## Phase 3: User Story 1 - Project Setup & Team Invites (Priority: P1) 🎯 MVP

**Goal**: Enable workspace creation and team collaboration foundation

**Independent Test**: Verify workspace creation and successful team invite acceptance flow

### Tests for User Story 1 (MANDATORY) ⚠️

- [ ] T011 [P] [US1] Write failing integration tests for Workspace creation in `backend/src/test/java/com/taskforge/project/WorkspaceControllerIT.java`
- [ ] T012 [P] [US1] Write failing contract tests for Team Invitation API in `backend/src/test/java/com/taskforge/project/InvitationContractTest.java`

### Implementation for User Story 1

- [ ] T013 [P] [US1] Implement `Workspace` and `ProjectMember` entities in `backend/src/main/java/com/taskforge/project/model/`
- [ ] T014 [US1] Implement Workspace and Invitation services with TDD in `backend/src/main/java/com/taskforge/project/service/`
- [ ] T015 [US1] Implement project-level RBAC evaluation logic in `backend/src/main/java/com/taskforge/auth/ProjectSecurityEvaluator.java`
- [ ] T016 [US1] Create Onboarding and Team Management UI in `frontend/src/pages/onboarding/` and `frontend/src/components/team/`
- [ ] T017 [US1] Integrate frontend with Workspace and Invitation APIs in `frontend/src/services/projectService.ts`

**Checkpoint**: US1 fully functional and testable independently

---

## Phase 4: User Story 2 - Task Management (Priority: P1) 🎯 MVP

**Goal**: Core task and subtask management with due dates and assignments

**Independent Test**: Create a task with subtasks, assign it, and verify subtask progress rollup

### Tests for User Story 2 (MANDATORY) ⚠️

- [ ] T018 [P] [US2] Write failing unit tests for Task entity and subtask rollup logic in `backend/src/test/java/com/taskforge/task/model/TaskTest.java`
- [ ] T019 [P] [US2] Write failing integration tests for Task CRUD API in `backend/src/test/java/com/taskforge/task/TaskControllerIT.java`

### Implementation for User Story 2

- [ ] T020 [P] [US2] Implement `Task` and `Label` entities with `@Version` for optimistic locking in `backend/src/main/java/com/taskforge/task/model/`
- [ ] T021 [US2] Implement Task and Label services with subtask management and progress rollup in `backend/src/main/java/com/taskforge/task/service/TaskService.java`
- [ ] T022 [US2] Create Task Creation and Detail UI components in `frontend/src/components/tasks/`
- [ ] T022 [US2] Implement Task Management service in `frontend/src/services/taskService.ts`
- [ ] T024 [US2] Implement "Last Write Wins" conflict handling logic in `frontend/src/hooks/useTaskUpdates.ts`

**Checkpoint**: Task management functional; US1 + US2 form core MVP

---

## Phase 5: User Story 3 - Real-time Collaboration (Priority: P2)

**Goal**: Contextual communication with comments and @mentions

**Independent Test**: Verify instant comment appearance for multiple users and @mention notifications

### Tests for User Story 3 (MANDATORY) ⚠️

- [ ] T025 [P] [US3] Write failing integration tests for Comment API and @mention detection in `backend/src/test/java/com/taskforge/collab/CommentControllerIT.java`
- [ ] T026 [P] [US3] Write failing WebSocket event delivery tests in `backend/src/test/java/com/taskforge/collab/RealTimeUpdateTest.java`

### Implementation for User Story 3

- [ ] T027 [P] [US3] Implement `Comment` and `Notification` entities in `backend/src/main/java/com/taskforge/collab/model/`
- [ ] T028 [US3] Implement Comment Service with @mention parsing and event broadcasting in `backend/src/main/java/com/taskforge/collab/service/CommentService.java`
- [ ] T029 [US3] Create Comment Feed and Notification Bell UI in `frontend/src/components/collaboration/`
- [ ] T030 [US3] Setup WebSocket client and STOMP subscriptions in `frontend/src/services/realtimeService.ts`

**Checkpoint**: US1, US2, and US3 now working together with real-time sync

---

## Phase 6: User Story 4 - AI-Assisted Task Breakdown (Priority: P2)

**Goal**: Reduce planning overhead using Gemini AI for subtask generation

**Independent Test**: Trigger AI breakdown on a task and verify relevant subtasks are suggested

### Tests for User Story 4 (MANDATORY) ⚠️

- [ ] T031 [P] [US4] Write failing unit tests for Gemini AI service integration in `backend/src/test/java/com/taskforge/ai/GeminiServiceTest.java`
- [ ] T032 [P] [US4] Write failing contract tests for AI breakdown endpoint in `backend/src/test/java/com/taskforge/ai/AIContractTest.java`

### Implementation for User Story 4

- [ ] T033 [US4] Implement `GeminiService` for backend-orchestrated AI calls in `backend/src/main/java/com/taskforge/ai/service/GeminiService.java`
- [ ] T034 [US4] Implement AI breakdown endpoint with structured prompt management in `backend/src/main/java/com/taskforge/ai/api/AIController.java`
- [ ] T035 [US4] Create AI Suggestion UI components (interactive subtask selection) in `frontend/src/components/ai/`
- [ ] T036 [US4] Implement risk detection AI analysis logic in `backend/src/main/java/com/taskforge/ai/service/RiskAnalyzer.java`
- [ ] T037 [US4] Implement "Helpful/Not Helpful" feedback UI and tracking for AI suggestions in `frontend/src/components/ai/Feedback.tsx`

**Checkpoint**: AI features integrated and functional

---

## Phase 7: User Story 5 - Multi-view Visualization (Priority: P2)

**Goal**: Visualize tasks across Kanban, List, and Calendar views

**Independent Test**: Verify task consistency across all three view toggles

### Implementation for User Story 5

- [ ] T038 [P] [US5] Implement Kanban Board UI with drag-and-drop in `frontend/src/components/views/KanbanBoard.tsx`
- [ ] T039 [P] [US5] Implement sortable List View in `frontend/src/components/views/ListView.tsx`
- [ ] T040 [P] [US5] Implement Calendar View using a date-based layout in `frontend/src/components/views/CalendarView.tsx`
- [ ] T041 [US5] Implement View Toggling and preference persistence in `frontend/src/hooks/useViewManager.ts`

**Checkpoint**: Visualization views complete and synced

---

## Phase 8: User Story 6 - Progress Tracking & Reporting (Priority: P3)

**Goal**: Dashboards and data export for project health visibility

**Independent Test**: Generate a project CSV export and verify dashboard charts update on task completion

### Tests for User Story 6 (MANDATORY) ⚠️

- [ ] T042 [P] [US6] Write failing tests for Report Generation (CSV/PDF) in `backend/src/test/java/com/taskforge/report/ReportServiceTest.java`
- [ ] T043 [P] [US6] Write failing integration tests for Dashboard stats API in `backend/src/test/java/com/taskforge/report/DashboardControllerIT.java`

### Implementation for User Story 6

- [ ] T044 [US6] Implement Dashboard Service for progress chart data in `backend/src/main/java/com/taskforge/report/service/DashboardService.java`
- [ ] T045 [US6] Implement CSV and PDF Export Service in `backend/src/main/java/com/taskforge/report/service/ExportService.java`
- [ ] T046 [US6] Create Dashboard UI with progress charts in `frontend/src/pages/dashboard/`
- [ ] T047 [US6] Add Export buttons and file download handling in `frontend/src/components/common/ExportActions.tsx`

**Checkpoint**: All user stories complete

---

## Phase 9: Polish & Cross-Cutting Concerns

**Purpose**: Final hardening and optimization

- [ ] T048 Performance optimization for dashboard queries and WebSocket message volume
- [ ] T049 Security audit of project-level RBAC and AI endpoint rate limiting
- [ ] T050 [P] Update `README.md` and API documentation in `docs/`
- [ ] T051 Run final `quickstart.md` validation across clean environments

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: Initial project structure and dependencies.
- **Foundational (Phase 2)**: Depends on Phase 1. BLOCKS all user stories.
- **User Stories (Phase 3+)**: Depend on Phase 2.
  - US1 (P1) and US2 (P1) are the MVP core and should be prioritized.
  - US3, US4, US5 (P2) can proceed in parallel once core task entities exist.
  - US6 (P3) depends on completion of task data.

### Parallel Opportunities

- Backend and Frontend project initialization (T002, T003) can run in parallel.
- Entity implementation (T013, T020, T027) can run in parallel.
- Independent UI views (T038, T039, T040) can run in parallel.
- Tests and Models within the same story can often run in parallel.

---

## Implementation Strategy

### MVP First (US1 & US2)

1. Complete Setup + Foundational (Phases 1-2).
2. Complete US1 (Project/Team) and US2 (Task Management).
3. **VALIDATE**: Core "Task Orchestrator" functionality is ready.

### Incremental Delivery

1. Add Real-time (US3) to enhance collaboration.
2. Add AI (US4) to provide intelligent assistance.
3. Add Visualization (US5) for flexible project views.
4. Add Reporting (US6) for management oversight.
