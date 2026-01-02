# Technical Specification

**Project Name:** Multi-Tenant SaaS Project Management System  
**Date:** October 26, 2025  
**Version:** 1.0  
**Author:** Sam  

---

## 1. Project Structure

The application is organized as a **single repository (monorepo)** that contains both backend and frontend services.  
This structure simplifies version control, ensures consistent configuration, and allows Docker Compose to manage the complete system from a single entry point.

---

### 1.1 Root Directory Structure

```text
/Multi-Tenant-SaaS-Platform
├── docker-compose.yml       # Service definitions and networking
├── submission.json          # Evaluation and login credentials
├── README.md                # Project overview and usage guide
├── .gitignore               # Files excluded from version control
├── docs/                    # Technical and functional documentation
├── backend/                 # API service (Node.js / Express)
└── frontend/                # Client application (React)
```

## 1.2 Backend Structure (`/backend`)

The backend service exposes RESTful APIs and handles authentication, authorization, and tenant isolation.
It is implemented using Node.js, Express, and Prisma ORM, following a layered architecture for maintainability.

```text
backend/
├── .env.example             # Sample environment configuration
├── Dockerfile               # Backend container definition
├── package.json             # Backend dependencies and scripts
├── prisma/
│   ├── schema.prisma        # Data models and relations
│   └── migrations/          # Versioned database migrations
├── seeds/
│   └── seed.js              # Initial data population
└── src/
    ├── controllers/         # Request handlers
    ├── middleware/          # Auth, RBAC, error handling
    ├── routes/              # API route mapping
    └── utils/               # Utility helpers (JWT, hashing)
```

## 1.3 Frontend Structure (`/frontend`)

The frontend provides the user interface for managing tenants, projects, and tasks.
It is built using React and optimized with Vite for faster builds and hot reloading.

```text
frontend/
├── Dockerfile               # Frontend container setup
├── package.json             # Frontend dependencies
├── public/                  # Static resources
└── src/
    ├── context/             # Global state management
    ├── pages/               # Screen-level components
    ├── App.js               # Route configuration
    └── main.js              # Application bootstrap
```

## 2️. Development Setup Guide
This section describes how developers can configure and run the system locally for development or testing.
---

## 2.1 Prerequisites

Ensure the following software is installed before proceeding:

- **Docker Desktop** — Version **4.0+**  
  _(Required to run all services in containers)_

- **Node.js** — Version **18 LTS**  
  _(RUseful for local development and debuggin)_

- **Git** — Version **2.0+**
  _(Required for source control)_

---

## 2.2 Environment Variables

Backend configuration is managed using environment variables.
Create a .env file inside the backend/ directory or rely on Docker defaults.

### Required Variables

```ini
# Server Configuration
PORT=5000
NODE_ENV=development

DATABASE_URL="postgresql://postgres:postgres@database:5432/saas_db"

JWT_SECRET="replace_with_secure_secret_key"
JWT_EXPIRES_IN="24h"

FRONTEND_URL="http://localhost:3000"

```

## 2.3 Installation Steps

### Clone the Repository

```bash
git clone <repository_url>
cd Multi-Tenant-SaaS-Platform
```

### Install Dependencies (Optional for Local Development)

If you want to edit the code locally with autocomplete and IntelliSense support, install dependencies manually.

#### Backend Dependencies

```bash
cd backend
npm install
```

#### Frontend Dependencies

```bash
cd ../frontend
npm install
```

## 2.4 How to Run Locally (Docker — Recommended)

Docker Compose is the preferred way to run the full system.

### Build and Start Containers

Run the following command from the **root directory**:

```bash
docker-compose up -d --build
```

### Verify Services

Ensure that all three containers — **database**, **backend**, and **frontend** — are running:

```bash
docker-compose ps
```

### Automatic Initialization

The **backend container** is configured to automatically run the following on startup:

- `prisma migrate deploy`
- `node seeds/seed.js`

Wait approximately **30–60 seconds** for the database to initialize and seed data to be populated.

---

## Access the Application

- **Frontend:** http://localhost:3000  
- **Backend API:** http://localhost:5000  
- **Health Check:** http://localhost:5000/api/health  

---

## 2.5 How to Run Tests

Since this project relies on **Docker** for the runtime environment, testing is performed against the **running containers**.

### Manual Verification (Postman / Curl)

- Use the credentials provided in `submission.json` to test authentication endpoints.
- Verify system health using the health check endpoint.

#### Example: Health Check

```bash
curl http://localhost:5000/api/health
```

**Expected Output:**

```json
{
  "status": "ok",
  "database": "connected"
}
```

### Database Inspection

To verify seeded data or inspect database tables, connect to the PostgreSQL container:

```bash
docker exec -it database psql -U postgres -d saas_db
```

Then run SQL queries, for example:

```sql
SELECT * FROM tenants;
```