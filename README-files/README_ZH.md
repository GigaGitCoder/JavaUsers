<div align="center">
  
![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white) ![Spring](https://img.shields.io/badge/spring-%236DB33F.svg?style=for-the-badge&logo=spring&logoColor=white) ![Java](https://img.shields.io/badge/java-%23ED8B00.svg?style=for-the-badge&logo=openjdk&logoColor=white) ![Docker](https://img.shields.io/badge/docker-%232496ED.svg?style=for-the-badge&logo=docker&logoColor=white) ![JWT](https://img.shields.io/badge/JWT-black?style=for-the-badge&logo=JSON%20web%20tokens)


[![License](https://img.shields.io/badge/license-GNU%20GPL%20v3.0-brightgreen)](LICENSE) ![Stars](https://img.shields.io/github/stars/GigaGitCoder/JavaUserService)
</div>

JavaUserService 是一个用于用户管理和身份验证的微服务。该服务提供注册、基于 JWT 的身份验证和管理功能。

## 服务端口

| 服务 | 端口 |
|------|------|
| Users Service API | 8092 |
| PostgreSQL 数据库 | 5433 |

## API 端点

### 用户身份验证

**基础 URL:** `http://localhost:8092/api/auth/`

#### 1. 登录
```http
POST /login
Content-Type: application/json
```

**请求体：**
```json
{
    "email": "user@example.com",
    "password": "your_password"
}
```

**响应：**
```json
{
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "type": "Bearer"
}
```

**重要：** 保存收到的 JWT 令牌，并在所有需要身份验证的后续请求的 `Authorization: Bearer <token>` 标头中使用它。

---

#### 2. 注册用户
```http
POST /register
Content-Type: application/json
```

**请求体：**
```json
{
    "nickname": "john_doe",
    "email": "john@example.com",
    "password": "secure_password123"
}
```

---

#### 3. 登出
```http
POST /logout
Authorization: Bearer <your_jwt_token>
```

**标头：**
- `Authorization: Bearer <your_jwt_token>` — 登录时收到的 JWT 令牌

---

### 管理功能

**基础 URL:** `http://localhost:8092/api/admin/`

**要求：** 所有管理端点都需要在 `Authorization` 标头中提供管理员 JWT 令牌。

#### 1. 获取所有用户
```http
GET /getAll
Authorization: Bearer <admin_jwt_token>
```

---

#### 2. 根据 ID 获取用户
```http
GET /getById/{id}
Authorization: Bearer <admin_jwt_token>
```

**路径参数：**
- `id` — 用户的唯一标识符

---

#### 3. 根据邮箱获取用户
```http
GET /getByEmail/{email}
Authorization: Bearer <admin_jwt_token>
```

**路径参数：**
- `email` — 用户的电子邮件地址

---

#### 4. 根据名称获取用户
```http
GET /getByName/{name}
Authorization: Bearer <admin_jwt_token>
```

**路径参数：**
- `name` — 用户的昵称

---

#### 5. 搜索用户
```http
GET /search?name=value&email=value&role=value
Authorization: Bearer <admin_jwt_token>
```

**查询参数：**
- `name` — 按昵称搜索（部分匹配）
- `email` — 按电子邮件搜索（部分匹配）
- `role` — 按角色搜索（部分匹配）

**示例：**
```
GET /search?role=ADMIN&name=john
```

---

#### 6. 注册为管理员
```http
POST /registerAsAdmin
Content-Type: application/json
```

**请求体：**
```json
{
    "nickname": "admin_user",
    "email": "admin@example.com",
    "password": "admin_secure_password"
}
```

**注意：** 此端点用于在初始系统设置期间创建第一个管理员。

---

#### 7. 更新用户
```http
PUT /update/{id}
Authorization: Bearer <admin_jwt_token>
Content-Type: application/json
```

**路径参数：**
- `id` — 要更新的用户标识符

**请求体：**
```json
{
    "nickname": "updated_nickname",
    "email": "updated@example.com",
    "password": "new_password"
}
```

---

#### 8. 删除用户
```http
DELETE /delete/{id}
Authorization: Bearer <admin_jwt_token>
```

**路径参数：**
- `id` — 要删除的用户标识符

---

## 数据格式

### 用户对象（User）
| 字段 | 类型 | 描述 |
|------|------|------|
| `id` | Long | 唯一标识符 |
| `nickname` | String | 用户名 |
| `email` | String | 电子邮件地址 |
| `password` | String | 密码（已加密） |
| `role` | String | 角色（USER、ADMIN） |

### JWT 令牌响应
| 字段 | 类型 | 描述 |
|------|------|------|
| `token` | String | 用于身份验证的 JWT 令牌 |
| `type` | String | 令牌类型（通常为 "Bearer"） |

## 安全性

- 所有密码在存储到数据库之前都会进行加密
- JWT 令牌用于身份验证和授权
- 管理端点受角色验证保护
- 令牌具有有限的有效期

## 注意事项

- 首次启动时应通过 `/api/admin/registerAsAdmin/` 端点创建第一个管理员
- 受保护端点必须在 `Authorization: Bearer <token>` 标头中传递 JWT 令牌
- 搜索字段支持部分匹配（不区分大小写）