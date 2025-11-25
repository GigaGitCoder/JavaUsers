<div align="center">
  
![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white) ![Spring](https://img.shields.io/badge/spring-%236DB33F.svg?style=for-the-badge&logo=spring&logoColor=white) ![Java](https://img.shields.io/badge/java-%23ED8B00.svg?style=for-the-badge&logo=openjdk&logoColor=white) ![Docker](https://img.shields.io/badge/docker-%232496ED.svg?style=for-the-badge&logo=docker&logoColor=white) ![JWT](https://img.shields.io/badge/JWT-black?style=for-the-badge&logo=JSON%20web%20tokens)


[![License](https://img.shields.io/badge/license-GNU%20GPL%20v3.0-brightgreen)](LICENSE) ![Stars](https://img.shields.io/github/stars/GigaGitCoder/JavaUserService)
</div>

JavaUserService is a microservice for user management and authentication. The service provides registration, JWT-based authentication, and administrative functions.

## Service Ports

| Service | Port |
|---------|------|
| Users Service API | 8092 |
| PostgreSQL Database | 5433 |

## API Endpoints

### User Authentication

**Base URL:** `http://localhost:8092/api/auth/`

#### 1. Login
```http
POST /login
Content-Type: application/json
```

**Request Body:**
```json
{
    "email": "user@example.com",
    "password": "your_password"
}
```

**Response:**
```json
{
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "type": "Bearer"
}
```

**Important:** Save the received JWT token and use it in the `Authorization: Bearer <token>` header for all subsequent requests requiring authentication.

---

#### 2. Register User
```http
POST /register
Content-Type: application/json
```

**Request Body:**
```json
{
    "nickname": "john_doe",
    "email": "john@example.com",
    "password": "secure_password123"
}
```

---

#### 3. Logout
```http
POST /logout
Authorization: Bearer <your_jwt_token>
```

**Headers:**
- `Authorization: Bearer <your_jwt_token>` — JWT token received during login

---

### Administrative Functions

**Base URL:** `http://localhost:8092/api/admin/`

**Requirements:** All administrative endpoints require an administrator JWT token in the `Authorization` header.

#### 1. Get All Users
```http
GET /getAll
Authorization: Bearer <admin_jwt_token>
```

---

#### 2. Get User by ID
```http
GET /getById/{id}
Authorization: Bearer <admin_jwt_token>
```

**Path Parameters:**
- `id` — unique user identifier

---

#### 3. Get User by Email
```http
GET /getByEmail/{email}
Authorization: Bearer <admin_jwt_token>
```

**Path Parameters:**
- `email` — user's email address

---

#### 4. Get User by Name
```http
GET /getByName/{name}
Authorization: Bearer <admin_jwt_token>
```

**Path Parameters:**
- `name` — user's nickname

---

#### 5. Search Users
```http
GET /search?name=value&email=value&role=value
Authorization: Bearer <admin_jwt_token>
```

**Query Parameters:**
- `name` — search by nickname (partial match)
- `email` — search by email (partial match)
- `role` — search by role (partial match)

**Example:**
```
GET /search?role=ADMIN&name=john
```

---

#### 6. Register as Administrator
```http
POST /registerAsAdmin
Content-Type: application/json
```

**Request Body:**
```json
{
    "nickname": "admin_user",
    "email": "admin@example.com",
    "password": "admin_secure_password"
}
```

**Note:** This endpoint is used to create the first administrator during initial system setup.

---

#### 7. Update User
```http
PUT /update/{id}
Authorization: Bearer <admin_jwt_token>
Content-Type: application/json
```

**Path Parameters:**
- `id` — identifier of the user to update

**Request Body:**
```json
{
    "nickname": "updated_nickname",
    "email": "updated@example.com",
    "password": "new_password"
}
```

---

#### 8. Delete User
```http
DELETE /delete/{id}
Authorization: Bearer <admin_jwt_token>
```

**Path Parameters:**
- `id` — identifier of the user to delete

---

## Data Format

### User Object
| Field | Type | Description |
|-------|------|-------------|
| `id` | Long | Unique identifier |
| `nickname` | String | Username |
| `email` | String | Email address |
| `password` | String | Password (hashed) |
| `role` | String | Role (USER, ADMIN) |

### JWT Token Response
| Field | Type | Description |
|-------|------|-------------|
| `token` | String | JWT token for authentication |
| `type` | String | Token type (usually "Bearer") |

## Security

- All passwords are hashed before being stored in the database
- JWT tokens are used for authentication and authorization
- Administrative endpoints are protected by role verification
- Tokens have a limited validity period

## Notes

- The first administrator should be created via the `/api/admin/registerAsAdmin/` endpoint on first launch
- JWT token must be passed in the `Authorization: Bearer <token>` header for protected endpoints
- Search fields support partial matching (case-insensitive)

# Localizations

- [Русский](./README-files/README_RU.md)
- [中文](./README-files/README_ZH.md)