# Data Model: TaskForge MVP

## Entities

### Workspace
- `id`: UUID (Primary Key)
- `name`: String (Not Null)
- `slug`: String (Unique, Not Null)
- `created_at`: Timestamp
- `updated_at`: Timestamp

### Project
- `id`: UUID (Primary Key)
- `workspace_id`: UUID (Foreign Key to Workspace)
- `name`: String (Not Null)
- `description`: Text
- `created_at`: Timestamp
- `updated_at`: Timestamp

### ProjectMember
- `id`: UUID (Primary Key)
- `project_id`: UUID (Foreign Key to Project)
- `user_id`: UUID (Foreign Key to User)
- `role`: Enum (Admin, Member)
- `joined_at`: Timestamp

### Task
- `id`: UUID (Primary Key)
- `project_id`: UUID (Foreign Key to Project)
- `parent_task_id`: UUID (Self-referential, nullable)
- `title`: String (Not Null)
- `description`: Text
- `assignee_id`: UUID (Foreign Key to User, nullable)
- `status`: Enum (Todo, In Progress, Done)
- `priority`: Enum (Low, Medium, High)
- `due_date`: Date (Nullable)
- `version`: Long (For Optimistic Locking)
- `created_at`: Timestamp
- `updated_at`: Timestamp

### Comment
- `id`: UUID (Primary Key)
- `task_id`: UUID (Foreign Key to Task)
- `author_id`: UUID (Foreign Key to User)
- `content`: Text (Supports @mentions)
- `created_at`: Timestamp

### Attachment
- `id`: UUID (Primary Key)
- `task_id`: UUID (Foreign Key to Task)
- `file_name`: String
- `file_path`: String (Storage reference)
- `file_size`: Long (Max 10MB)
- `mime_type`: String
- `created_at`: Timestamp

## Relationships
- A **Workspace** has many **Projects**.
- A **Project** has many **ProjectMembers**.
- A **Project** has many **Tasks**.
- A **Task** can have one **Parent Task** and many **Subtasks**.
- A **Task** has many **Comments** and **Attachments**.

## State Transitions (Task)
- `Todo` ↔ `In Progress` ↔ `Done`
- All status transitions are allowed.
- Subtask progress rolls up to the parent task (e.g., parent is Done only if all subtasks are Done).
