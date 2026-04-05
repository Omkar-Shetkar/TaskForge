# Phase 0 Research: TaskForge MVP

## Decisions

### Real-time Synchronization Strategy
- **Decision**: Spring WebFlux with WebSockets (STOMP protocol)
- **Rationale**: Efficient for two-way communication required for task status updates, comments, and project notifications. Spring WebFlux integrates well with Java 21 and offers reactive patterns for scalability.
- **Alternatives Considered**: 
  - Server-Sent Events (SSE): Rejected as it's unidirectional; we need client-to-server updates for @mentions and task movements.
  - Periodic Polling: Rejected due to latency (SC-004 requires <500ms sync).

### AI Integration Pattern
- **Decision**: Backend-orchestrated Gemini API integration via dedicated `ai-service`
- **Rationale**: Centralizing AI calls in the backend allows for better prompt management, security (hiding API keys), and rate limiting. It also aligns with the Library-First principle (reusable AI module).
- **Alternatives Considered**:
  - Direct Frontend Gemini API calls: Rejected for security and lack of reusable logic.

### Concurrency Handling
- **Decision**: Optimistic Locking with "Last Write Wins" (standard database versioning)
- **Rationale**: Simple to implement and meets the MVP requirement for small teams. Using `@Version` in JPA/Hibernate will prevent accidental overwrites during race conditions while still favoring the latest update.
- **Alternatives Considered**:
  - Pessimistic Locking: Rejected as it negatively impacts user experience during collaboration.
  - Operational Transformation (OT): Rejected for MVP complexity (out of scope).

### Project-level RBAC
- **Decision**: Spring Security with custom ProjectMembership evaluation
- **Rationale**: Standard, well-tested security framework. Using `@PreAuthorize` at the service layer ensures that only members assigned to a project can view or modify associated tasks.
- **Alternatives Considered**: 
  - Workspace-wide roles: Rejected for failing the privacy requirement.

### Attachment Storage
- **Decision**: S3-compatible cloud storage (e.g., MinIO or AWS S3) with Signed URLs
- **Rationale**: Secure and scalable. Signed URLs ensure that only authorized project members can access file attachments (10MB limit).
- **Alternatives Considered**:
  - Database BLOBs: Rejected for performance and scalability reasons.
