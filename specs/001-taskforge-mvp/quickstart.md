# Quickstart: TaskForge MVP Development

## Environment Setup

### Prerequisites
- Java 21 JDK
- Node.js 20+
- PostgreSQL 15+
- Redis 7+
- Docker (optional, for dependencies)

### Database Configuration
1. Create a PostgreSQL database named `taskforge`.
2. Configure credentials in `backend/src/main/resources/application.properties`.

## Building the Project

### Backend
```bash
cd backend
./mvnw clean install
./mvnw spring-boot:run
```

### Frontend
```bash
cd frontend
npm install
npm run dev
```

## Running Tests
- **Backend Unit/Integration Tests**: `./mvnw test`
- **Frontend Unit Tests**: `npm run test`
- **E2E Tests**: `npm run test:e2e` (Ensure both frontend and backend are running)

## AI Feature Integration
To enable AI features, set the `GEMINI_API_KEY` environment variable in the backend environment.
