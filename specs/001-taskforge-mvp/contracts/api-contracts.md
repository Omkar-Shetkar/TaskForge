# API Contracts: TaskForge MVP

## Task API (REST)

### `POST /api/v1/projects/{projectId}/tasks`
Create a new task within a project.
- **Request Body**:
  ```json
  {
    "title": "String (Required)",
    "description": "String (Optional)",
    "assigneeId": "UUID (Optional)",
    "priority": "Enum[Low, Medium, High]",
    "status": "Enum[Todo, In Progress, Done]",
    "dueDate": "ISO Date String (Optional)",
    "parentTaskId": "UUID (Optional)"
  }
  ```
- **Response**: `201 Created` with Task entity and `Location` header.

### `PATCH /api/v1/tasks/{taskId}`
Update a task's attributes (e.g., status, assignee).
- **Request Body**: Partial task updates and `version` (for concurrency check).
- **Response**: `200 OK` or `409 Conflict` (if version mismatch).

## AI Service API (Internal/Library)

### `POST /api/v1/ai/tasks/{taskId}/breakdown`
Request AI-generated subtasks for a parent task.
- **Response**:
  ```json
  {
    "suggested_subtasks": [
      { "title": "String", "description": "String" }
    ]
  }
  ```

## Real-time Events (WebSockets/STOMP)

### `/topic/projects/{projectId}/updates`
Broadcasting project-wide changes.
- **Payload**:
  ```json
  {
    "type": "TASK_UPDATED | TASK_CREATED | COMMENT_ADDED",
    "timestamp": "ISO Date String",
    "data": { ... }
  }
  ```

### `/user/queue/notifications`
Personalized real-time notifications for @mentions or task assignments.
- **Payload**:
  ```json
  {
    "type": "MENTION | ASSIGNMENT",
    "message": "String",
    "taskId": "UUID"
  }
  ```
