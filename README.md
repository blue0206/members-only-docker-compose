# Members Only - Development Environment

This repository contains the Docker Compose configuration to orchestrate the local development environment for the **Members Only** project. It simplifies the process of running all required services (backend, frontend, database, etc.) with a single command.

> **Note:** This setup corresponds to the **monolithic deployment** versions of the frontend and backend services, which can be found on their respective `monolith-deployment` branches.

---

## 🚀 Overview

This Docker Compose setup provides a consistent, containerized environment for full-stack development. It includes:

- **Backend Service:** A Node.js container running the Express.
- **Frontend Service:** A Node.js container running the **Vite dev server** for a fast, Hot Module Replacement (HMR) powered frontend development experience.
- **Database Service:** A dedicated PostgreSQL container to act as the development database.
- **Shared Network:** All services are connected to a shared bridge network, allowing them to communicate using their service names.

---

## Prerequisites

- [Docker](https://www.docker.com/products/docker-desktop/) installed and running.
- A code editor like VS Code.
- Git installed.

---

## ⚙️ Local Setup Instructions

This setup assumes a **sibling directory structure**.

1.  **Create a parent directory** for the entire project workspace.

    ```bash
    mkdir members-only
    cd members-only
    ```

2.  **Clone all required repositories** into this parent directory so they are siblings.

    ```bash
    # Clone this dev-env repository
    git clone https://github.com/blue0206/members-only-docker-compose.git

    # Clone the backend repository
    git clone https://github.com/blue0206/members-only-backend.git

    # Clone the frontend repository
    git clone https://github.com/blue0206/members-only-frontend.git
    ```

    After this step, your directory should look like this:

    ```
    members-only/
    ├── members-only-docker-compose/   (contains the docker-compose.yml)
    ├── members-only-backend/
    └── members-only-frontend/
    ```

3.  **Set up Environment Variables:**
    - Navigate into `backend-repo` and create a `.env.development` file. Copy the contents from `.env.example` and fill in the values. **Important:** The `DATABASE_URL` must point to the Docker service name:
      ```env
      # members-only-backend/.env
      DATABASE_URL="postgresql://your_db_user:your_db_password@db:5432/your_dev_db_name"
      # ... other backend variables
      ```
    - Navigate into `frontend-repo` and create a `.env.development` file. Copy the contents from `.env.example`.
    - The database credentials used in the backend's `DATABASE_URL` must match the `POSTGRES_*` environment variables defined for the `db` service in `docker-compose.yml`.

---

## 🚀 Running the Application

1.  **Navigate to this directory:**

    ```bash
    cd members-only-docker-compose
    ```

2.  **Start all services:**

    ```bash
    docker-compose up --build
    ```

    - The `--build` flag must be set on first run or after changing `package.json` in either service to rebuild the images with new dependencies.

3.  **Accessing the Services:**
    - **Frontend:** Open your browser to `http://localhost:5173`
    - **Backend API:** Accessible at `http://localhost:8000` (for Postman testing).

---

## Useful Docker Compose Commands

- **Start in background (detached mode):** `docker-compose up -d`
- **Stop services:** `docker-compose down`
- **Stop and remove volumes (clears DB data):** `docker-compose down -v`
- **View logs for all services:** `docker-compose logs -f`
