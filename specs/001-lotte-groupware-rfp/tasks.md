# Task Breakdown: 롯데손해보험 그룹웨어 시스템 고도화

This document breaks down the implementation of the Lotte Groupware System into actionable, dependency-ordered tasks.

## Phase 1: Project Setup

**Goal**: Initialize the project structure, install dependencies, and set up the containerized development environment.

- [ ] T001 Initialize Node.js project in `backend/` (`npm init -y`) and `frontend/` (`npm init -y`).
- [ ] T002 Install base dependencies for the backend in `backend/package.json` (express, typescript, prisma, passport, etc.).
- [ ] T003 Install base dependencies for the frontend in `frontend/package.json` (react, react-dom, typescript, material-ui, etc.).
- [ ] T004 Create project structure with directories for `src/` and `tests/` in both `backend/` and `frontend/`.
- [ ] T005 [P] Configure TypeScript (`tsconfig.json`) for both `backend/` and `frontend/`.
- [ ] T006 Create a `docker-compose.yml` file at the root to manage PostgreSQL and Elasticsearch services.
- [ ] T007 Create a `.env.example` file in `backend/` for environment variable management.

## Phase 2: Foundational & Core Services

**Goal**: Establish the database schema and implement core services like authentication middleware that are prerequisites for all user stories.

- [ ] T008 Define the complete database schema in `backend/prisma/schema.prisma` based on `data-model.md`.
- [ ] T009 Run the initial database migration to create all tables (`npx prisma migrate dev --name init`).
- [ ] T010 [P] Implement a basic Express server setup in `backend/src/index.ts`.
- [ ] T011 [P] Implement the core React application entrypoint in `frontend/src/index.tsx`.
- [ ] T012 Implement JWT-based authentication strategy using Passport.js in `backend/src/lib/auth.ts`.
- [ ] T013 Implement authentication middleware to protect routes in `backend/src/api/middleware/auth.ts`.
- [ ] T014 [P] Set up global state management (e.g., React Context or Zustand) for user authentication in `frontend/src/state/auth.ts`.

## Phase 3: User Story 1 - 통합 로그인 및 개인화 포탈 (Login & Portal)

**Goal**: Users can log in, view, and customize a personal dashboard.
**Independent Test**: A user can log in via the UI, see a personalized portal, and their session is persisted. The dashboard displays placeholder portlets.

- [ ] T015 [US1] Implement the `/auth/login` and `/auth/me` endpoints in `backend/src/api/auth.ts` based on `contracts/auth.yaml`.
- [ ] T016 [US1] Create the `UserService` to handle user-related business logic in `backend/src/services/userService.ts`.
- [ ] T017 [P] [US1] Create the Login page UI component in `frontend/src/pages/LoginPage.tsx`.
- [ ] T018 [P] [US1] Create the main Dashboard UI component in `frontend/src/pages/DashboardPage.tsx`.
- [ ] T019 [P] [US1] Implement a `Portlet` component placeholder in `frontend/src/components/Portlet.tsx`.
- [ ] T020 [US1] Connect the frontend login form to the backend API, handling token storage and state updates.
- [ ] T021 [US1] Implement a protected route mechanism in `frontend/` that redirects unauthenticated users to the login page.

## Phase 4: User Story 2 - 전자결재 문서 기안 및 처리 (Electronic Approvals)

**Goal**: Users can draft, submit, and process approval documents.
**Independent Test**: A logged-in user can create a new approval document from a form, submit it, and another user designated as the approver can view and approve/reject it.

- [ ] T022 [P] [US2] Implement API endpoints for listing, creating, and retrieving approvals in `backend/src/api/approvals.ts` per `contracts/approvals.yaml`.
- [ ] T023 [P] [US2] Implement the `ApprovalService` for handling approval logic in `backend/src/services/approvalService.ts`.
- [ ] T024 [P] [US2] Create the "New Approval" form component in `frontend/src/components/approvals/NewApprovalForm.tsx`.
- [ ] T025 [P] [US2] Create the "Approval List" view component in `frontend/src/pages/ApprovalListPage.tsx`.
- [ ] T026 [P] [US2] Create the "Approval Detail" view component for processing in `frontend/src/pages/ApprovalDetailPage.tsx`.
- [ ] T027 [US2] Implement the API endpoint for processing an approval step (approve/reject) in `backend/src/api/approvals.ts`.
- [ ] T028 [US2] Connect the frontend components to the approval API endpoints.

## Phase 5: User Story 3 - 협업을 위한 게시판 활용 (Bulletin Boards)

**Goal**: Users can create and interact with posts on bulletin boards.
**Independent Test**: A logged-in user can navigate to a board, create a new post with a title and content, and other users can view the post and add comments.

- [ ] T029 [P] [US3] Implement API endpoints for posts and comments in `backend/src/api/posts.ts` as defined in `contracts/posts.yaml`.
- [ ] T030 [P] [US3] Implement the `PostService` for post and comment business logic in `backend/src/services/postService.ts`.
- [ ] T031 [P] [US3] Create the "Post List" view component in `frontend/src/components/board/PostList.tsx`.
- [ ] T032 [P] [US3] Create the "Post Detail" view including a comment section in `frontend/src/components/board/PostDetail.tsx`.
- [ ] T033 [P] [US3] Create a "New Post" form component in `frontend/src/components/board/NewPostForm.tsx`.
- [ ] T034 [US3] Connect the frontend board components to the corresponding backend APIs.

## Phase 6: Polish & Cross-Cutting Concerns

**Goal**: Finalize the application by implementing system-wide features like logging, error handling, and search integration.

- [ ] T035 [P] Implement a structured logging solution (e.g., Winston or Pino) throughout the `backend/`.
- [ ] T036 [P] Implement standardized error handling middleware in `backend/src/api/middleware/error.ts`.
- [ ] T037 Set up Elasticsearch indexing for all major data models (`User`, `Approval`, `Post`).
- [ ] T038 Implement the AI-powered search service using `pgvector` and the chosen LLM in `backend/src/services/searchService.ts`.
- [ ] T039 [P] Create a unified search bar component in the `frontend/` that consumes the search service.
- [ ] T040 Write comprehensive end-to-end tests for the primary user flows.

## Dependencies & Implementation Strategy

### Dependency Graph

-   **User Story 1 (Login/Portal)** is the foundational user-facing feature.
-   **User Story 2 (Approvals)** depends on User Story 1 (requires authenticated users).
-   **User Story 3 (Boards)** depends on User Story 1 (requires authenticated users).

```
        +---------+
        | US1     |
        | (Login) |
        +---------+
         /       \
        /         \
+---------+   +---------+
| US2     |   | US3     |
| (Appr.) |   | (Boards)|
+---------+   +---------+
```

### Implementation Strategy

The project will be delivered incrementally based on the user stories:

1.  **MVP (Minimum Viable Product)**: Complete **Phase 1, 2, and 3** to deliver the core login and portal functionality. This provides immediate value by establishing the system's entry point and framework.
2.  **Incremental Features**: Once the MVP is stable, **Phase 4 (Approvals)** and **Phase 5 (Boards)** can be developed **in parallel** as they do not depend on each other.
3.  **Finalization**: **Phase 6** will be addressed last, as it polishes and enhances the already functional application.

### Parallel Execution

-   Within each user story phase, backend API tasks (`[P]`) and frontend UI component tasks (`[P]`) can often be developed in parallel, as their interaction is defined by the API contracts in the `contracts/` directory. For example, in Phase 4, `T022` (backend) can be worked on at the same time as `T024`, `T025`, and `T026` (frontend).

```
