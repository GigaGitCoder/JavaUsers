<div align="center">
  
![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white) ![Spring](https://img.shields.io/badge/spring-%236DB33F.svg?style=for-the-badge&logo=spring&logoColor=white) ![Java](https://img.shields.io/badge/java-%23ED8B00.svg?style=for-the-badge&logo=openjdk&logoColor=white) ![Docker](https://img.shields.io/badge/docker-%232496ED.svg?style=for-the-badge&logo=docker&logoColor=white) ![JWT](https://img.shields.io/badge/JWT-black?style=for-the-badge&logo=JSON%20web%20tokens)


[![License](https://img.shields.io/badge/license-GNU%20GPL%20v3.0-brightgreen)](LICENSE) ![Stars](https://img.shields.io/github/stars/GigaGitCoder/JavaUserService)
</div>

JavaUserService — это микросервис для управления пользователями и аутентификации. Сервис обеспечивает регистрацию, авторизацию с использованием JWT токенов и административные функции.

## Используемые порты

| Сервис | Порт |
|--------|------|
| Users Service API | 8092 |
| PostgreSQL Database | 5433 |

## API Эндпоинты

### Аутентификация пользователей

**Базовый URL:** `http://localhost:8092/api/auth/`

#### 1. Вход в систему
```http
POST /login
Content-Type: application/json
```

**Тело запроса:**
```json
{
    "email": "user@example.com",
    "password": "your_password"
}
```

**Ответ:**
```json
{
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "type": "Bearer"
}
```

**Важно:** Сохраните полученный JWT токен и используйте его в заголовке `Authorization: Bearer <token>` для всех последующих запросов, требующих аутентификации.

---

#### 2. Регистрация пользователя
```http
POST /register
Content-Type: application/json
```

**Тело запроса:**
```json
{
    "nickname": "john_doe",
    "email": "john@example.com",
    "password": "secure_password123"
}
```

---

#### 3. Выход из системы
```http
POST /logout
Authorization: Bearer <your_jwt_token>
```

**Заголовки:**
- `Authorization: Bearer <your_jwt_token>` — JWT токен, полученный при входе

---

### Административные функции

**Базовый URL:** `http://localhost:8092/api/admin/`

**Требования:** Все административные эндпоинты требуют JWT токен администратора в заголовке `Authorization`.

#### 1. Получить всех пользователей
```http
GET /getAll
Authorization: Bearer <admin_jwt_token>
```

---

#### 2. Получить пользователя по ID
```http
GET /getById/{id}
Authorization: Bearer <admin_jwt_token>
```

**Параметры пути:**
- `id` — уникальный идентификатор пользователя

---

#### 3. Получить пользователя по Email
```http
GET /getByEmail/{email}
Authorization: Bearer <admin_jwt_token>
```

**Параметры пути:**
- `email` — адрес электронной почты пользователя

---

#### 4. Получить пользователя по имени
```http
GET /getByName/{name}
Authorization: Bearer <admin_jwt_token>
```

**Параметры пути:**
- `name` — никнейм пользователя

---

#### 5. Поиск пользователей
```http
GET /search?name=value&email=value&role=value
Authorization: Bearer <admin_jwt_token>
```

**Параметры запроса:**
- `name` — поиск по никнейму (частичное совпадение)
- `email` — поиск по электронной почте (частичное совпадение)
- `role` — поиск по роли (частичное совпадение)

**Пример:**
```
GET /search?role=ADMIN&name=john
```

---

#### 6. Регистрация администратора
```http
POST /registerAsAdmin
Content-Type: application/json
```

**Тело запроса:**
```json
{
    "nickname": "admin_user",
    "email": "admin@example.com",
    "password": "admin_secure_password"
}
```

**Примечание:** Этот эндпоинт используется для создания первого администратора при начальной настройке системы.

---

#### 7. Обновить пользователя
```http
PUT /update/{id}
Authorization: Bearer <admin_jwt_token>
Content-Type: application/json
```

**Параметры пути:**
- `id` — идентификатор пользователя для обновления

**Тело запроса:**
```json
{
    "nickname": "updated_nickname",
    "email": "updated@example.com",
    "password": "new_password"
}
```

---

#### 8. Удалить пользователя
```http
DELETE /delete/{id}
Authorization: Bearer <admin_jwt_token>
```

**Параметры пути:**
- `id` — идентификатор пользователя для удаления

---

## Формат данных

### Объект пользователя (User)
| Поле | Тип | Описание |
|------|-----|----------|
| `id` | Long | Уникальный идентификатор |
| `nickname` | String | Имя пользователя |
| `email` | String | Электронная почта |
| `password` | String | Пароль (хешируется) |
| `role` | String | Роль (USER, ADMIN) |

### JWT Token Response
| Поле | Тип | Описание |
|------|-----|----------|
| `token` | String | JWT токен для аутентификации |
| `type` | String | Тип токена (обычно "Bearer") |

## Безопасность

- Все пароли хешируются перед сохранением в базу данных
- JWT токены используются для аутентификации и авторизации
- Административные эндпоинты защищены проверкой роли
- Токены имеют ограниченный срок действия

## Примечания

- Первого администратора следует создать через эндпоинт `/api/admin/registerAsAdmin/` при первом запуске
- JWT токен необходимо передавать в заголовке `Authorization: Bearer <token>` для защищённых эндпоинтов
- Поля поиска поддерживают частичное совпадение (регистр не учитывается)