# Multi-Tenant SaaS Platform – API Documentation

## Authentication & Security

- **Authentication Method:** Bearer Token (JWT)
- **Header Format:**  
  `Authorization: Bearer <your_jwt_token>`
- **Token Expiry:** 24 hours
- **Base URL (Local):**  
  `http://localhost:5000/api`


---

## System

### 1. Health Check

Checks whether the API server and database connection are healthy.

- **Endpoint:** `GET /health`
- **Access:** Public

#### Response (200 OK)
```json
{
  "status": "ok",
  "database": "connected"
}
```

## 2. Authentication Module

### 2.1 Register Tenant (Sign Up)

Registers a new **Organization (Tenant)** along with its first **Admin user**.

- **Endpoint:** `POST /auth/register-tenant`
- **Access:** Public

#### Request Body (JSON)

```json
{
  "tenantName": "NovaTech Solutions",
  "subdomain": "novatech",
  "adminEmail": "admin@novatech.com",
  "password": "SecurePassword123"
}
```

#### Response (201 Created)

```json
{
  "message": "Tenant registered successfully",
  "tenantId": "tenant-uuid"
}

```

### 2.2 Login

Authenticates a user and returns a **JWT access token**.

- **Endpoint:** `POST /auth/login`
- **Access:** Public

#### Request Body (JSON)

```json
{
  "email": "admin@novatech.com",
  "password": "SecurePassword123"
}

```

#### Response (200 OK)

```json
{
  "token": "jwt-token",
  "user": {
    "id": "user-uuid",
    "email": "admin@novatech.com",
    "role": "tenant_admin",
    "tenantId": "tenant-uuid"
  }
}

```

### 2.3 Get Current User

Retrieves the profile of the currently logged-in user.

- **Endpoint:** `GET /auth/me`
- **Access:** Protected (All Roles)

#### Response (200 OK)

```json
{
  "id": "user-uuid",
  "fullName": "Sam",
  "email": "sam@novatech.com",
  "role": "user"
}

```

## 3. Tenant Management (Super Admin)


### 3.1 List All Tenants

- **Endpoint:** `GET /tenants`
- **Access:** Super Admin

#### Response (200 OK)

```json
{
  "results": 2,
  "tenants": [
    { "id": "t1", "name": "NovaTech Solutions", "subdomain": "novatech" },
    { "id": "t2", "name": "BlueWave Systems", "subdomain": "bluewave" }
  ]
}

```

### 3.2 Get Tenant Details

- **Endpoint:** `GET /tenants/:tenantId`
- **Access:** Super Admin


#### Response (200 OK)

```json
{
  "id": "tenant-uuid",
  "name": "NovaTech Solutions",
  "status": "active"
}
```

### 3.3 Update Tenant

- **Endpoint:** `PUT /tenants/:tenantId`
- **Access:** Super Admin


#### Request Body (JSON)

```json
{
  "status": "inactive"
}
```

## 4️. User Management (Tenant Admin)

### 4.1 List Users

- **Endpoint:** `GET /tenants/:tenantId/users`
- **Access:** Tenant Admin

#### Response (200 OK)

```json
{
  "users": [
    { "id": "u1", "fullName": "Ananya Rao", "role": "user" }
  ]
}

```

### 4.2 Create User


- **Endpoint:** `POST /tenants/:tenantId/users`
- **Access:** Tenant Admin

#### Request Body (JSON)

```json
{
  "email": "ananya@novatech.com",
  "password": "UserPassword123",
  "fullName": "Ananya Rao",
  "role": "user"
}

```

### 4.3 Update User

- **Endpoint:** `PUT /users/:id`
- **Access:** Tenant Admin

#### Request Body (JSON)

```json
{
  "fullName": "Ananya Sharma",
  "role": "tenant_admin"
}

```

### 4.4 Delete User

- **Endpoint:** `DELETE /users/:id`
- **Access:** Tenant Admin

---

## 5️. Project Management

### 5.1 List Projects

Lists all projects belonging to the requester’s tenant.

- **Endpoint:** `GET /projects`
- **Access:** User / Admin

#### Response (200 OK)

```json
{
  "projects": [
    { "id": "p1", "title": "Farmer Dashboard", "status": "active" }
  ]
}

```

### 5.2 Create Project

Creates a new project within the tenant.

- **Endpoint:** `POST /projects`
- **Access:** Admin

#### Request Body (JSON)

```json
{
  "title": "Agri Analytics Platform",
  "description": "Analytics system for crop monitoring",
  "status": "active"
}
```

### 5.3 Get Project Details

Retrieves detailed information for a specific project.

- **Endpoint:** `GET /projects/:id`
- **Access:** User / Admin

---

### 5.4 Update Project

Updates an existing project’s details.

- **Endpoint:** `PUT /projects/:id`
- **Access:** Admin

#### Request Body (JSON)

```json
{
  "status": "completed"
}
```

> **Note:**  
> Project deletion functionality is typically mapped to  
> `DELETE /projects/:id`

---

## 6️. Task Management

### 6.1 List Tasks

Retrieves all tasks associated with a specific project.

- **Endpoint:** `GET /projects/:projectId/tasks`
- **Access:** User / Admin

#### Response (200 OK)

```json
{
  "tasks": [
    { "id": "t1", "title": "Design Dashboard UI", "status": "TODO" }
  ]
}

```

### 6.2 Create Task

Creates a new task within a specific project.

- **Endpoint:** `POST /projects/:projectId/tasks`
- **Access:** Admin

#### Request Body (JSON)

```json
{
  "title": "Implement Authentication",
  "description": "JWT based login system",
  "priority": "HIGH",
  "dueDate": "2024-03-01"
}

```

### 6.3 Update Task Status

- **Endpoint:** `PATCH /tasks/:id/status`
- **Access:** User / Admin

#### Request Body (JSON)

```json
{
  "status": "IN_PROGRESS"
}
```

### 6.4 Update Task Details


- **Endpoint:** `PUT /tasks/:id`
- **Access:** Admin

#### Request Body (JSON)

```json
{
  "title": "Implement Authentication (Updated)",
  "priority": "MEDIUM"
}

