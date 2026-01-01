# Multi-Tenant SaaS Platform

## Description
This project is a cloud-ready **Multi-Tenant SaaS Project Management System** designed to demonstrate how a single application can securely serve multiple organizations while keeping their data strictly isolated.

Each organization (tenant) operates in its own logical workspace. Users can collaborate on projects and tasks, while administrators control access and resources. The system is built with scalability, security, and real-world SaaS practices in mind.

The platform supports three user roles:
- **Super Admin** – manages the entire system and all tenants
- **Tenant Admin** – manages users, projects, and tasks within an organization
- **User** – works on assigned projects and tasks

---

## Key Features
* **True Multi-Tenancy:** Logical data isolation using tenant identifiers at every layer
* **Secure Authentication:** Stateless JWT-based authentication with encrypted passwords
* **Role-Based Access Control (RBAC):** Granular permissions per role
* **Organization Management:** Independent workspaces for each tenant
* **Project Lifecycle Management:** Create, update, and track projects
* **Task Tracking:** Assign tasks with priorities, deadlines, and status
* **User Administration:** Invite, update, and remove users per tenant
* **Scalable Architecture:** Containerized services using Docker
* **Modern UI:** Clean and responsive frontend built with React

---

## Technology Stack

### **Frontend**
* **Framework:** React.js
* **Build Tool:** Vite
* **State Handling:** Context API
* **Routing:** React Router
* **API Communication:** Axios
* **UI Styling:** Responsive CSS (Flexbox & Grid)

### **Backend**
* **Runtime:** Node.js
* **Framework:** Express.js
* **ORM:** Prisma
* **Authentication:** JWT
* **Security:** Bcrypt, CORS, Helmet
* **Validation:** Express Middleware

### **Database & DevOps**
* **Database:** PostgreSQL
* **Containers:** Docker
* **Orchestration:** Docker Compose
* **Environment:** Linux-based containers

---

## Architecture Overview
The system follows a **client–server architecture**:

- The **frontend** handles user interaction and communicates with the backend using REST APIs.
- The **backend** enforces authentication, authorization, and tenant isolation.
- The **database** stores all tenant data using a shared schema with strict `tenant_id` scoping.

All services are containerized and run inside a single Docker network, ensuring consistent environments across development and deployment.

---

## Installation & Setup

### **Prerequisites**
* Docker & Docker Compose
* Node.js (v18 or later) – optional for local development
* Git

---

### **Method 1: Docker (Recommended)**

This method runs the entire stack automatically.

#### 1. Clone the Repository
```bash
git clone <your-repository-url>
cd Multi-Tenant-SaaS-Platform
```
2.  **Configure Environment**
    Ensure the required environment variables are set (Docker defaults are sufficient for testing).
    ```properties
    POSTGRES_USER=postgres
    POSTGRES_PASSWORD=postgres
    POSTGRES_DB=saas_db
    ```

3.  **Start the Application**
    ```bash
    docker-compose up -d --build
    ```

4.  **Access the App**
    * **Frontend:** [http://localhost:3000](http://localhost:3000)
    * **Backend:** [http://localhost:5000/](http://localhost:5000/)
    * **Health Check:** [http://localhost:5000/api/health](http://localhost:5000/api/health)

    *Note: The backend automatically runs database migrations and seeds initial data on startup..*

### **Method 2: Local Development (Manual)**

<details>
<summary>Expand manual setup steps</summary>

1.  **Database Setup**
    Ensure PostgreSQL is running locally on port 5432.

2.  **Backend Setup**
    ```bash
    cd backend
    npm install
    cp .env.example .env
    npx prisma migrate dev 
    npm run seed
    npm start

    ```

3.  **Frontend Setup**
    ```bash
    cd frontend
    npm install
    npm run dev

    ```
</details>

## Environment Variables

   The backend relies on the following configuration values:

| Variable         | Purpose                      |
| ---------------- | ---------------------------- |
| `PORT`           | Backend server port          |
| `DATABASE_URL`   | PostgreSQL connection string |
| `JWT_SECRET`     | Token signing key            |
| `JWT_EXPIRES_IN` | Token validity duration      |
| `FRONTEND_URL`   | Allowed CORS origin          |

## API Documentation

### **Authentication**
* `POST /api/auth/register-tenant` Create a new organization 
* `POST /api/auth/login`– Authenticate user

### **Projects & Tasks**
* `GET /api/projects` - List all projects for current tenant
* `POST /api/projects` - Create a new project
* `POST /api/projects/:id/tasks` - Add a task to a project 
* `PATCH /api/tasks/:id/status` - Update task status

### **Tenant & User Management**
* `GET /api/tenants` - (Super Admin) List all tenants
* `GET /api/tenants/:id`-Update tenant status
* `GET /api/tenants/:id/users` - List tenant users
* `POST /api/tenants/:id/users` - Add new users

## Testing Credentials (Seed Data)

**System Admin:**
* Email: `superadmin@system.com`
* Password: `Admin@123`

**Tenant Admin (Demo Company):**
* Email: `admin@demo.com`
* Password: `Admin@123`
* Subdomain: `demo`