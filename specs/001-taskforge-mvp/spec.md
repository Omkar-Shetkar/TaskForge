# Feature Specification: TaskForge MVP

**Feature Branch**: `001-taskforge-mvp`  
**Created**: 2026-04-05  
**Status**: Draft  
**Input**: User description: "Create a comprehensive living specification for TaskForge — a modern collaborative task orchestrator web app that helps small-to-medium teams manage projects with real-time collaboration and AI assistance powered by Gemini. Core Purpose: Reduce coordination overhead by providing clean task management, real-time updates, and intelligent AI features. Target Users: Team leads, project managers, and individual contributors in teams of 2–20 people. Key Features (MVP): Project & workspace management with team invites and roles Task creation, assignment, subtasks, labels, due dates, and attachments Multiple views: Kanban board, list, and calendar Real-time collaboration (comments, @mentions, activity feed) AI capabilities: auto-generate subtasks, suggest priorities, detect risks, and create progress summaries/reports Personal & team dashboard with progress charts Export tasks/reports to PDF/CSV Non-Functional Requirements: Modern, responsive, and delightful UI (Next.js + Tailwind) Fast performance and real-time sync Secure authentication and data privacy Mobile-friendly design Success Criteria: Quick onboarding (<5 mins) Create and assign tasks in under 60 seconds AI suggestions are useful ≥80% of the time Always up-to-date task status Out of Scope: Time tracking, Gantt charts, invoicing, advanced workflows, offline mode, enterprise SSO Instructions: Use user stories format (“As a [role], I want… so that…”) Define core data models/entities Keep the spec clear, structured, and maintainable as a single source of truth Generate the SPEC.md file accordingly."

## Clarifications

### Session 2026-04-05
- Q: Task Status Workflow → A: Fixed: Todo, In Progress, Done
- Q: Real-time Concurrency Strategy → A: Last Write Wins (Simple)
- Q: Task Assignment Visibility → A: Project-level: Visible only to members of that project
- Q: Attachment Handling → A: Standard: 10MB limit per file
- Q: Notification Frequency → A: Targeted: Real-time for @mentions/assignments only

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Project Setup & Team Invites (Priority: P1)

As a team lead, I want to create a workspace and invite my team so that we can start collaborating on projects.

**Why this priority**: Workspace and team members are foundational for all collaborative features. Without a team, the core value proposition of "collaborative task orchestrator" is not met.

**Independent Test**: Can be fully tested by creating a workspace and successfully sending/accepting a team invite. Delivers the foundational capability for team collaboration.

**Acceptance Scenarios**:

1. **Given** a new user on the onboarding page, **When** they enter workspace details and click "Create", **Then** a new workspace is created and they are redirected to the dashboard.
2. **Given** a workspace admin, **When** they enter a teammate's email and select a role, **Then** an invitation is sent and the teammate appears as "Pending" in the team list.

---

### User Story 2 - Task Management (Priority: P1)

As an individual contributor, I want to create, assign, and manage tasks with subtasks and due dates so that I can stay organized and meet deadlines.

**Why this priority**: Task management is the primary function of the application. Everything else (views, AI, reports) depends on tasks existing and being manageable.

**Independent Test**: Can be fully tested by creating a task, adding subtasks, setting a due date, and assigning it to a teammate. Delivers core utility of managing work.

**Acceptance Scenarios**:

1. **Given** a project, **When** a user clicks "Add Task" and fills in title and assignee, **Then** the task is created and appears in the task list.
2. **Given** an existing task, **When** a user adds a subtask, **Then** the subtask is associated with the parent task and progress is updated.

---

### User Story 3 - Real-time Collaboration (Priority: P2)

As a team member, I want to comment on tasks and @mention colleagues so that we can communicate effectively in context.

**Why this priority**: Essential for reducing "coordination overhead," but depends on the existence of tasks and projects.

**Independent Test**: Can be fully tested by two users commenting on the same task simultaneously and seeing updates without refresh. Delivers contextual communication.

**Acceptance Scenarios**:

1. **Given** a task detail view, **When** User A posts a comment, **Then** User B (viewing the same task) sees the comment appear instantly.
2. **Given** a comment field, **When** a user types "@" followed by a teammate's name, **Then** a dropdown of team members appears for selection.

---

### User Story 4 - AI-Assisted Task Breakdown (Priority: P2)

As a project manager, I want the AI to suggest subtasks and priorities so that I can plan projects more efficiently.

**Why this priority**: Differentiator for TaskForge. Provides significant value in reducing cognitive load during planning.

**Independent Test**: Can be fully tested by requesting subtask generation for a task with a descriptive title and receiving relevant suggestions. Delivers intelligent planning assistance.

**Acceptance Scenarios**:

1. **Given** a task with a complex title (e.g., "Implement User Auth"), **When** the user clicks "AI Breakdown", **Then** the system suggests at least 3 relevant subtasks.
2. **Given** a list of new tasks, **When** the user clicks "AI Prioritize", **Then** the system assigns suggested priority levels (Low, Medium, High) based on task descriptions.

---

### User Story 5 - Multi-view Visualization (Priority: P2)

As a user, I want to toggle between Kanban, List, and Calendar views so that I can visualize work in the way that suits me best.

**Why this priority**: Improves usability and user experience for different types of project management workflows.

**Independent Test**: Can be fully tested by switching between views and seeing the same set of tasks represented correctly in each format. Delivers flexible work visualization.

**Acceptance Scenarios**:

1. **Given** the project dashboard, **When** the user clicks "Kanban", **Then** tasks are displayed as cards in status-based columns.
2. **Given** the project dashboard, **When** the user clicks "Calendar", **Then** tasks with due dates are displayed on their respective dates.

---

### User Story 6 - Progress Tracking & Reporting (Priority: P3)

As a stakeholder, I want to see a dashboard with progress charts and export reports so that I can understand project health.

**Why this priority**: Valuable for project oversight, but can be performed manually or via simple lists in early stages.

**Independent Test**: Can be fully tested by generating a PDF report for a project and verifying it contains accurate task status counts. Delivers project health transparency.

**Acceptance Scenarios**:

1. **Given** a team dashboard, **When** tasks are completed, **Then** the "Progress Chart" updates to reflect the change in project completion percentage.
2. **Given** a project view, **When** the user clicks "Export to CSV", **Then** a file is downloaded containing all project tasks and their current metadata.

---

### Edge Cases

- **Conflict Resolution**: When two users edit the same task title simultaneously, the system uses a "Last Write Wins" strategy.
- How does the system handle AI suggestions for very short or nonsensical task titles?
- What happens if a user is invited to a workspace but already has an account with that email?
- How are notifications handled when a task is deleted while a user is being @mentioned in a new comment?

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: System MUST support workspace creation and team member invitations (Admin/Member roles).
- **FR-002**: System MUST allow users to create, update, and delete tasks (CRUD) with title, description, assignee, due date, and labels.
- **FR-003**: System MUST support hierarchical tasks (subtasks) with progress rollup.
- **FR-004**: System MUST provide real-time updates for task changes, comments, and project status.
- **FR-005**: System MUST allow users to add comments to tasks and @mention workspace members.
- **FR-006**: System MUST integrate with an AI service to auto-generate subtasks based on the parent task's title and description.
- **FR-007**: System MUST use AI to analyze task lists for potential risks (e.g., overdue tasks, resource overallocation) and suggest priority adjustments.
- **FR-008**: System MUST implement Kanban (drag-and-drop), List (sortable), and Calendar (date-based) project views.
- **FR-009**: System MUST display personal and team dashboards with visual progress charts (e.g., burndown, status distribution).
- **FR-010**: System MUST allow exporting project data and progress summaries to PDF and CSV formats.
- **FR-011**: System MUST implement secure authentication and project-level role-based access control (only members assigned to a project can view/edit its tasks).

### Key Entities *(include if feature involves data)*

- **Workspace**: Root container for projects and team members.
- **Project**: Collection of tasks belonging to a workspace, accessible only by assigned members.
- **Task**: Single unit of work. Attributes: Title, Description, Assignee, Status (Todo, In Progress, Done), Due Date, Priority, Labels, Attachments (Max 10MB each).
- **Subtask**: Child task linked to a parent task.
- **Member**: User associated with a workspace with a specific role (Admin, Member).
- **Comment**: Contextual message on a task, supports @mentions.
- **Label**: Tag for categorizing tasks across projects.
- **Notification**: Real-time alert triggered by direct @mentions or task assignments only.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: New teams can complete workspace setup and invite members in under 5 minutes.
- **SC-002**: Users can create a task and assign it to a teammate in under 60 seconds from any project view.
- **SC-003**: AI-generated subtasks are accepted or used as-is by users ≥80% of the time.
- **SC-004**: Real-time sync latency for status changes (e.g., moving a card in Kanban) is under 500ms for all concurrent viewers.
- **SC-005**: 90% of active users visit the dashboard at least once per session to check progress.

## Assumptions

- **Target Users**: Users have stable internet connectivity (required for real-time and AI features).
- **Tech Stack**: Next.js + Tailwind for frontend/backend, Gemini for AI, and a real-time capable database/backend.
- **Mobile Support**: The UI will be responsive and mobile-friendly, but a native app is out of scope for MVP.
- **Language**: Initial release supports English only.
- **File Storage**: Attachments will be stored in a cloud bucket (e.g., AWS S3 or similar) with a 10MB per file limit.
