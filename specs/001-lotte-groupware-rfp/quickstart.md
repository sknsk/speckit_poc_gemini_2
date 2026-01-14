# Quickstart

This guide provides instructions for setting up the local development environment for the Lotte Groupware System.

## Prerequisites

-   **Node.js**: Version 20.x or higher
-   **npm** or **yarn**: Package manager
-   **Docker**: For running PostgreSQL and Elasticsearch
-   **Git**: For version control

## Backend Setup

1.  **Navigate to the backend directory**:
    ```bash
    cd backend
    ```

2.  **Install dependencies**:
    ```bash
    npm install
    ```

3.  **Setup Environment Variables**:
    Create a `.env` file in the `backend` directory and populate it with the necessary variables. Start with the template:
    ```bash
    cp .env.example .env
    ```
    Update the `.env` file with your database connection string, JWT secret, etc.

4.  **Start Services**:
    A `docker-compose.yml` file will be provided to easily start the required services (PostgreSQL, Elasticsearch).
    ```bash
    docker-compose up -d
    ```

5.  **Run Database Migrations**:
    Prisma will be used for database management.
    ```bash
    npx prisma migrate dev
    ```

6.  **Run the development server**:
    ```bash
    npm run dev
    ```
    The backend server will be running on `http://localhost:8080`.

## Frontend Setup

1.  **Navigate to the frontend directory**:
    ```bash
    cd frontend
    ```

2.  **Install dependencies**:
    ```bash
    npm install
    ```

3.  **Run the development server**:
    ```bash
    npm start
    ```
    The frontend application will be running on `http://localhost:3000` and will connect to the backend API.

## Running Tests

-   **Backend Tests**:
    ```bash
    cd backend
    npm test
    ```

-   **Frontend Tests**:
    ```bash
    cd frontend
    npm test
    ```
