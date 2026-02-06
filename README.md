
---

# 1) Пользователь / Профиль (tgId)

## 1.1 Получить профиль по tgId

**Method:** GET
**URL:** `/api/v1/users/{tgId}` (пример: `/api/v1/users/7947922450`)

**Заголовки запроса:**

```http
Host: localhost:8080
Connection: keep-alive
User-Agent: Sovokumus-WebApp/1.0
Accept: application/json
Accept-Language: ru-RU, ru;q=0.9, en;q=0.8
Accept-Encoding: gzip, deflate
Accept-Charset: utf-8
```

**Тело запроса:** отсутствует

**Заголовки ответов:**

```http
Content-Type: application/json; charset=utf-8
Content-Encoding: gzip
```

**Тела ответов:**

✅ **200 OK**

```json
{
  "tgId": 7947922450,
  "name": "Алексей",
  "city": "Казань",
  "bio": "Ищу команду на олимпиады",
  "skills": ["graphs", "dp"],
  "subjects": ["informatics"],
  "experienceLevel": "advanced",
  "createdAt": "2026-02-06T09:00:00Z",
  "updatedAt": "2026-02-06T09:10:00Z"
}
```

❌ **400 Bad Request**

```json
{
  "error": {
    "code": "INVALID_TG_ID",
    "message": "Некорректный Telegram ID",
    "details": [{ "field": "tgId", "issue": "must_be_number" }]
  }
}
```

❌ **404 Not Found**

```json
{ "error": { "code": "USER_NOT_FOUND", "message": "Пользователь не найден" } }
```

❌ **500 Internal Server Error**

```json
{ "error": { "code": "INTERNAL_ERROR", "message": "Ошибка сервера" } }
```

**Пояснение:** tgId передаётся в URL как идентификатор ресурса профиля. `GET` используется для чтения и не меняет данные.

---

## 1.2 Создать профиль

**Method:** POST
**URL:** `/api/v1/users`

**Заголовки запроса:**

```http
Host: localhost:8080
Connection: keep-alive
User-Agent: Sovokumus-WebApp/1.0
Accept: application/json
Accept-Language: ru-RU, ru;q=0.9, en;q=0.8
Accept-Encoding: gzip, deflate
Accept-Charset: utf-8
Content-Type: application/json; charset=utf-8
Content-Length: [автоматически]
```

**Тело запроса:**

```json
{
  "tgId": 7947922450,
  "name": "Алексей",
  "city": "Казань",
  "bio": "Ищу команду на олимпиады",
  "skills": ["graphs"],
  "subjects": ["informatics"],
  "experienceLevel": "middle"
}
```

**Заголовки ответов:**

```http
Content-Type: application/json; charset=utf-8
Content-Encoding: gzip
```

**Тела ответов:**

✅ **201 Created**

```json
{
  "tgId": 7947922450,
  "name": "Алексей",
  "city": "Казань",
  "bio": "Ищу команду на олимпиады",
  "skills": ["graphs"],
  "subjects": ["informatics"],
  "experienceLevel": "middle",
  "createdAt": "2026-02-06T09:00:00Z"
}
```

❌ **400 Bad Request**

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Некорректные данные",
    "details": [{ "field": "tgId", "issue": "required" }]
  }
}
```

❌ **409 Conflict**

```json
{
  "error": {
    "code": "USER_ALREADY_EXISTS",
    "message": "Профиль уже создан"
  }
}
```

❌ **413 Payload Too Large**

```json
{ "error": { "code": "PAYLOAD_TOO_LARGE", "message": "Слишком большой запрос" } }
```

❌ **500 Internal Server Error**

```json
{ "error": { "code": "INTERNAL_ERROR", "message": "Ошибка сервера" } }
```

**Пояснение:** `POST` создаёт новый профиль, поэтому `201`. `409` — если профиль с этим tgId уже существует.

---

## 1.3 Обновить профиль по tgId

**Method:** PUT
**URL:** `/api/v1/users/{tgId}` (пример: `/api/v1/users/7947922450`)

**Заголовки запроса:**

```http
Host: localhost:8080
Connection: keep-alive
User-Agent: Sovokumus-WebApp/1.0
Accept: application/json
Accept-Language: ru-RU, ru;q=0.9, en;q=0.8
Accept-Encoding: gzip, deflate
Accept-Charset: utf-8
Content-Type: application/json; charset=utf-8
Content-Length: [автоматически]
```

**Тело запроса:**

```json
{
  "name": "Алексей",
  "city": "Казань",
  "bio": "Сильный в алгоритмах",
  "skills": ["graphs", "dp"],
  "subjects": ["informatics"],
  "experienceLevel": "advanced"
}
```

**Заголовки ответов:**

```http
Content-Type: application/json; charset=utf-8
Content-Encoding: gzip
```

**Тела ответов:**

✅ **200 OK**

```json
{
  "tgId": 7947922450,
  "name": "Алексей",
  "city": "Казань",
  "bio": "Сильный в алгоритмах",
  "skills": ["graphs", "dp"],
  "subjects": ["informatics"],
  "experienceLevel": "advanced",
  "updatedAt": "2026-02-06T09:20:00Z"
}
```

❌ **400 Bad Request**

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Некорректные данные",
    "details": [{ "field": "name", "issue": "too_short" }]
  }
}
```

❌ **404 Not Found**

```json
{ "error": { "code": "USER_NOT_FOUND", "message": "Пользователь не найден" } }
```

❌ **409 Conflict**

```json
{ "error": { "code": "PROFILE_LOCKED", "message": "Профиль нельзя изменить сейчас" } }
```

❌ **413 Payload Too Large**

```json
{ "error": { "code": "PAYLOAD_TOO_LARGE", "message": "Слишком большой запрос" } }
```

❌ **500 Internal Server Error**

```json
{ "error": { "code": "INTERNAL_ERROR", "message": "Ошибка сервера" } }
```

**Пояснение:** `PUT` обновляет ресурс профиля по tgId. `409` — когда запрос нормальный, но бизнес-правило запрещает изменение.

---

# 2) Олимпиады

## 2.1 Получить список олимпиад (поиск/фильтры)

**Method:** GET
**URL:** `/api/v1/olympiads?query=инф&level=regional&subject=informatics&page=1&limit=20`

**Заголовки запроса:**

```http
Host: localhost:8080
Connection: keep-alive
User-Agent: Sovokumus-WebApp/1.0
Accept: application/json
Accept-Language: ru-RU, ru;q=0.9, en;q=0.8
Accept-Encoding: gzip, deflate
Accept-Charset: utf-8
```

**Тело запроса:** отсутствует

**Заголовки ответов:**

```http
Content-Type: application/json; charset=utf-8
Content-Encoding: gzip
```

**Тела ответов:**

✅ **200 OK**

```json
{
  "items": [
    {
      "id": "ol_10",
      "title": "Олимпиада по информатике",
      "level": "regional",
      "subject": "informatics",
      "teamSize": { "min": 2, "max": 3 }
    }
  ],
  "page": 1,
  "limit": 20,
  "total": 1
}
```

❌ **400 Bad Request**

```json
{
  "error": {
    "code": "INVALID_QUERY",
    "message": "Некорректные параметры запроса",
    "details": [{ "field": "limit", "issue": "must_be_1_100" }]
  }
}
```

❌ **500 Internal Server Error**

```json
{ "error": { "code": "INTERNAL_ERROR", "message": "Ошибка сервера" } }
```

**Пояснение:** список/поиск — это чтение, значит `GET`, а фильтры идут через query-параметры.

---

## 2.2 Получить олимпиаду по id

**Method:** GET
**URL:** `/api/v1/olympiads/{olympiadId}` (пример: `/api/v1/olympiads/ol_10`)

**Заголовки запроса:**

```http
Host: localhost:8080
Connection: keep-alive
User-Agent: Sovokumus-WebApp/1.0
Accept: application/json
Accept-Language: ru-RU, ru;q=0.9, en;q=0.8
Accept-Encoding: gzip, deflate
Accept-Charset: utf-8
```

**Тело запроса:** отсутствует

**Заголовки ответов:**

```http
Content-Type: application/json; charset=utf-8
Content-Encoding: gzip
```

**Тела ответов:**

✅ **200 OK**

```json
{
  "id": "ol_10",
  "title": "Олимпиада по информатике",
  "level": "regional",
  "subject": "informatics",
  "teamSize": { "min": 2, "max": 3 }
}
```

❌ **404 Not Found**

```json
{ "error": { "code": "OLYMPIAD_NOT_FOUND", "message": "Олимпиада не найдена" } }
```

❌ **500 Internal Server Error**
``

`json
{ "error": { "code": "INTERNAL_ERROR", "message": "Ошибка сервера" } }

````

**Пояснение:** получение одного ресурса по id — стандартный `GET /resource/{id}`.

---

# 3) Команды

## 3.1 Создать команду
**Method:** POST  
**URL:** `/api/v1/teams`

**Заголовки запроса:**
```http
Host: localhost:8080
Connection: keep-alive
User-Agent: Sovokumus-WebApp/1.0
Accept: application/json
Accept-Language: ru-RU, ru;q=0.9, en;q=0.8
Accept-Encoding: gzip, deflate
Accept-Charset: utf-8
Content-Type: application/json; charset=utf-8
Content-Length: [автоматически]
````

**Тело запроса:**

```json
{
  "ownerTgId": 7947922450,
  "title": "Кодим и побеждаем",
  "olympiadId": "ol_10",
  "description": "Ищем 1-2 сильных по графам",
  "requiredSkills": ["graphs", "dp"],
  "city": "Казань",
  "isRemoteAllowed": true
}
```

**Заголовки ответов:**

```http
Content-Type: application/json; charset=utf-8
Content-Encoding: gzip
```

**Тела ответов:**

✅ **201 Created**

```json
{
  "id": "t_501",
  "ownerTgId": 7947922450,
  "title": "Кодим и побеждаем",
  "olympiadId": "ol_10",
  "membersCount": 1,
  "status": "open",
  "createdAt": "2026-02-06T10:00:00Z"
}
```

❌ **400 Bad Request**

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Некорректные данные",
    "details": [{ "field": "title", "issue": "required" }]
  }
}
```

❌ **404 Not Found**

```json
{ "error": { "code": "OLYMPIAD_NOT_FOUND", "message": "Олимпиада не найдена" } }
```

❌ **409 Conflict**

```json
{
  "error": {
    "code": "TEAM_CONFLICT",
    "message": "Нельзя создать команду: конфликт состояния"
  }
}
```

❌ **413 Payload Too Large**

```json
{ "error": { "code": "PAYLOAD_TOO_LARGE", "message": "Слишком большой запрос" } }
```

❌ **500 Internal Server Error**

```json
{ "error": { "code": "INTERNAL_ERROR", "message": "Ошибка сервера" } }
```

**Пояснение:** `POST` создаёт команду → `201`. `ownerTgId` нужен, раз у вас идентификация через tgId.

---

## 3.2 Получить список команд (фильтры)

**Method:** GET
**URL:** `/api/v1/teams?olympiadId=ol_10&city=Казань&remote=true&skill=graphs&page=1&limit=20`

**Заголовки запроса:**

```http
Host: localhost:8080
Connection: keep-alive
User-Agent: Sovokumus-WebApp/1.0
Accept: application/json
Accept-Language: ru-RU, ru;q=0.9, en;q=0.8
Accept-Encoding: gzip, deflate
Accept-Charset: utf-8
```

**Тело запроса:** отсутствует

**Заголовки ответов:**

```http
Content-Type: application/json; charset=utf-8
Content-Encoding: gzip
```

**Тела ответов:**

✅ **200 OK**

```json
{
  "items": [
    {
      "id": "t_501",
      "title": "Кодим и побеждаем",
      "olympiadId": "ol_10",
      "status": "open",
      "city": "Казань",
      "isRemoteAllowed": true,
      "requiredSkills": ["graphs", "dp"]
    }
  ],
  "page": 1,
  "limit": 20,
  "total": 1
}
```

❌ **400 Bad Request**

```json
{
  "error": {
    "code": "INVALID_QUERY",
    "message": "Некорректные параметры запроса",
    "details": [{ "field": "page", "issue": "must_be_positive" }]
  }
}
```

❌ **500 Internal Server Error**

```json
{ "error": { "code": "INTERNAL_ERROR", "message": "Ошибка сервера" } }
```

**Пояснение:** список — `GET`, фильтры в query, чтобы менять выборку без body.

---

## 3.3 Получить команду по id

**Method:** GET
**URL:** `/api/v1/teams/{teamId}` (пример: `/api/v1/teams/t_501`)

**Заголовки запроса:**

```http
Host: localhost:8080
Connection: keep-alive
User-Agent: Sovokumus-WebApp/1.0
Accept: application/json
Accept-Language: ru-RU, ru;q=0.9, en;q=0.8
Accept-Encoding: gzip, deflate
Accept-Charset: utf-8
```

**Тело запроса:** отсутствует

**Заголовки ответов:**

```http
Content-Type: application/json; charset=utf-8
Content-Encoding: gzip
```

**Тела ответов:**

✅ **200 OK**

```json
{
  "id": "t_501",
  "ownerTgId": 7947922450,
  "title": "Кодим и побеждаем",
  "olympiadId": "ol_10",
  "description": "Ищем 1-2 сильных по графам",
  "requiredSkills": ["graphs", "dp"],
  "city": "Казань",
  "isRemoteAllowed": true,
  "members": [{ "tgId": 7947922450, "name": "Алексей" }],
  "status": "open"
}
```

❌ **404 Not Found**

```json
{ "error": { "code": "TEAM_NOT_FOUND", "message": "Команда не найдена" } }
```

❌ **500 Internal Server Error**

```json
{ "error": { "code": "INTERNAL_ERROR", "message": "Ошибка сервера" } }
```

**Пояснение:** ресурс “команда” по id — `GET /teams/{teamId}`.

---

## 3.4 Обновить команду

**Method:** PUT
**URL:** `/api/v1/teams/{teamId}` (пример: `/api/v1/teams/t_501`)

**Заголовки запроса:**

```http
Host: localhost:8080
Connection: keep-alive
User-Agent: Sovokumus-WebApp/1.0
Accept: application/json
Accept-Language: ru-RU, ru;q=0.9, en;q=0.8
Accept-Encoding: gzip, deflate
Accept-Charset: utf-8
Content-Type: application/json; charset=utf-8
Content-Length: [автоматически]
```

**Тело запроса:**

```json
{
  "title": "Кодим и побеждаем (обновлено)",
  "description": "Теперь ищем 1 человека по DP",
  "requiredSkills": ["dp"],
  "isRemoteAllowed": true,
  "city": "Казань"
}
```

**Заголовки ответов:**

```http
Content-Type: application/json; charset=utf-8
Content-Encoding: gzip
```

**Тела ответов:**

✅ **200 OK**

```json
{
  "id": "t_501",
  "title": "Кодим и побеждаем (обновлено)",
  "description": "Теперь ищем 1 человека по DP",
  "requiredSkills": ["dp"],
  "isRemoteAllowed": true,
  "city": "Казань",
  "updatedAt": "2026-02-06T10:30:00Z"
}
```

❌ **400 Bad Request**

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Некорректные данные",
    "details": [{ "field": "title", "issue": "too_long" }]
  }
}
```

❌ **404 Not Found**

```json
{ "error": { "code": "TEAM_NOT_FOUND", "message": "Команда не найдена" } }
```

❌ **409 Conflict**

```json
{ "error": { "code": "TEAM_CLOSED", "message": "Нельзя изменить команду: набор закрыт" } }
```

❌ **413 Payload Too Large**

```json
{ "error": { "code": "PAYLOAD_TOO_LARGE", "message": "Слишком большой запрос" } }
```

❌ **500 Internal Server Error**

```json
{ "error": { "code": "INTERNAL_ERROR", "message": "Ошибка сервера" } }
```

**Пояснение:** `PUT` меняет данные команды. `409` — если команда уже в таком состоянии, что менять нельзя.

---

## 3.5 Закрыть набор в команду

**Method:** PATCH
**URL:** `/api/v1/teams/{teamId}/close` (пример: `/api/v1/teams/t_501/close`)

**Заголовки запроса:**

```http
Host: localhost:8080
Connection: keep-alive
User-Agent: Sovokumus-WebApp/1.0
Accept: application/json
Accept-Language: ru-RU, ru;q=0.9, en;q=0.8
Accept-Encoding: gzip, deflate
Accept-Charset: utf-8
```

**Тело запроса:** отсутствует

**Заголовки ответов:**

```http
Content-Type: application/json; charset=utf-8
Content-Encoding: gzip
```

**Тела ответов:**

✅ **200 OK**

```json
{ "id": "t_501", "status": "closed" }
```

❌ **404 Not Found**

```json
{ "error": { "code": "TEAM_NOT_FOUND", "message": "Команда не найдена" } }
```

❌ **409 Conflict**

```json
{ "error": { "code": "ALREADY_CLOSED", "message": "Команда уже закрыта для набора" } }
```

❌ **500 Internal Server Error**

```json
{ "error": { "code": "INTERNAL_ERROR", "message": "Ошибка сервера" } }
```

**Пояснение:** это изменение одного поля-состояния, поэтому удобнее отдельное действие `PATCH /close`.

---

# 4) Поиск сокомандников (пользователи)

## 4.1 Поиск пользователей по навыкам/предметам

**Method:** GET
**URL:** `/api/v1/users?skill=graphs&subject=informatics&level=advanced&city=Казань&remote=true&page=1&limit=20`

**Заголовки запроса:**

```http
Host: localhost:8080
Connection: keep-alive
User-Agent: Sovokumus-WebApp/1.0
Accept: application/json
Accept-Language: ru-RU, ru;q=0.9, en;q=0.8
Accept-Encoding: gzip, deflate
Accept-Charset: utf-8
```

**Тело запроса:** отсутствует

**Заголовки ответов:**

```http
Content-Type: application/json; charset=utf-8
Content-Encoding: gzip
```

**Тела ответов:**

✅ **200 OK**

```json
{
  "items": [
    {
      "tgId": 8195976349,
      "name": "Мария",
      "city": "Казань",
      "skills": ["graphs", "dp"],
      "subjects": ["informatics"],
      "experienceLevel": "advanced",
      "isRemoteAllowed": true
    }
  ],
  "page": 1,
  "limit": 20,
  "total": 1
}
```

❌ **400 Bad Request**

```json
{
  "error": {
    "code": "INVALID_QUERY",
    "message": "Некорректные параметры запроса",
    "details": [{ "field": "level", "issue": "unknown_value" }]
  }
}
```

❌ **500 Internal Server Error**

```json
{ "error": { "code": "INTERNAL_ERROR", "message": "Ошибка сервера" } }
```

**Пояснение:** поиск — это получение списка по критериям, поэтому `GET` + query-параметры.

---

# 5) Приглашения (invites)

## 5.1 Отправить приглашение пользователю в команду

**Method:** POST
**URL:** `/api/v1/teams/{teamId}/invites` (пример: `/api/v1/teams/t_501/invites`)

**Заголовки запроса:**

```http
Host: localhost:8080
Connection: keep-alive
User-Agent: Sovokumus-WebApp/1.0
Accept: application/json
Accept-Language: ru-RU, ru;q=0.9, en;q=0.8
Accept-Encoding: gzip, deflate
Accept-Charset: utf-8
Content-Type: application/json; charset=utf-8
Content-Length: [автоматически]
```

**Тело запроса:**

```json
{
  "fromTgId": 7947922450,
  "toTgId": 8195976349,
  "message": "Привет! Ищем сильного по графам, го к нам?"
}
```

**Заголовки ответов:**

```http
Content-Type: application/json; charset=utf-8
Content-Encoding: gzip
```

**Тела ответов:**

✅ **201 Created**

```json
{
  "id": "inv_9001",
  "teamId": "t_501",
  "fromTgId": 7947922450,
  "toTgId": 8195976349,
  "status": "pending",
  "createdAt": "2026-02-06T10:10:00Z"
}
```

❌ **400 Bad Request**

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Некорректные данные",
    "details": [{ "field": "toTgId", "issue": "required" }]
  }
}
```

❌ **404 Not Found**

```json
{ "error": { "code": "TEAM_NOT_FOUND", "message": "Команда не найдена" } }
```

или

```json
{ "error": { "code": "USER_NOT_FOUND", "message": "Пользователь не найден" } }
```

❌ **409 Conflict**

```json
{
  "error": {
    "code": "INVITE_CONFLICT",
    "message": "Нельзя отправить приглашение: уже отправлено/уже в команде/набор закрыт"
  }
}
```

❌ **413 Payload Too Large**

```json
{ "error": { "code": "PAYLOAD_TOO_LARGE", "message": "Слишком большой запрос" } }
```

❌ **500 Internal Server Error**

```json
{ "error": { "code": "INTERNAL_ERROR", "message": "Ошибка сервера" } }
```

**Пояснение:** `POST` создаёт ресурс “приглашение”. `409` — если состояние не позволяет создать новое (дубликат/закрыто).

---

## 5.2 Получить входящие приглашения пользователя (по tgId)

**Method:** GET
**URL:** `/api/v1/users/{tgId}/invites?status=pending&page=1&limit=20`
Пример: `/api/v1/users/8195976349/invites?status=pending&page=1&limit=20`

**Заголовки запроса:**

```http
Host: localhost:8080
Connection: keep-alive
User-Agent: Sovokumus-WebApp/1.0
Accept: application/json
Accept-Language: ru-RU, ru;q=0.9, en;q=0.8
Accept-Encoding: gzip, deflate
Accept-Charset: utf-8
```

**Тело запроса:** отсутствует

**Заголовки ответов:**

```http
Content-Type: application/json; charset=utf-8
Content-Encoding: gzip
```

**Тела ответов:**

✅ **200 OK**

```json
{
  "items": [
    {
      "id": "inv_9001",
      "teamId": "t_501",
      "fromTgId": 7947922450,
      "toTgId": 8195976349,
      "status": "pending",
      "createdAt": "2026-02-06T10:10:00Z"
    }
  ],
  "page": 1,
  "limit": 20,
  "total": 1
}
```

❌ **400 Bad Request**

```json
{
  "error": {
    "code": "INVALID_QUERY",
    "message": "Некорректные параметры запроса",
    "details": [{ "field": "status", "issue": "unknown_value" }]
  }
}
```

❌ **404 Not Found**

```json
{ "error": { "code": "USER_NOT_FOUND", "message": "Пользователь не найден" } }
```

❌ **500 Internal Server Error**

```json
{ "error": { "code": "INTERNAL_ERROR", "message": "Ошибка сервера" } }
```

**Пояснение:** список приглашений — это чтение, поэтому `GET`. Статус/пагинация — через query.

---

## 5.3 Принять приглашение

**Method:** POST
**URL:** `/api/v1/invites/{inviteId}/accept` (пример: `/api/v1/invites/inv_9001/accept`)

**Заголовки запроса:**

```http
Host: localhost:8080
Connection: keep-alive
User-Agent: Sovokumus-WebApp/1.0
Accept: application/json
Accept-Language: ru-RU, ru;q=0.9, en;q=0.8
Accept-Encoding: gzip, deflate
Accept-Charset: utf-8
Content-Type: application/json; charset=utf-8
Content-Length: [автоматически]
```

**Тело запроса:** можно пустое, но чтобы было явно “кто принимает”:

```json
{
  "byTgId": 8195976349
}
```

**Заголовки ответов:**

```http
Content-Type: application/json; charset=utf-8
Content-Encoding: gzip
```

**Тела ответов:**

✅ **200 OK**

```json
{
  "id": "inv_9001",
  "status": "accepted",
  "acceptedAt": "2026-02-06T10:20:00Z"
}
```

❌ **400 Bad Request**

```json
{
  "error": {
    "code": "INVALID_ACTION",
    "message": "Некорректное действие или данные",
    "details": [{ "field": "byTgId", "issue": "required" }]
  }
}
```

❌ **404 Not Found**

```json
{ "error": { "code": "INVITE_NOT_FOUND", "message": "Приглашение не найдено" } }
```

❌ **409 Conflict**

```json
{
  "error": {
    "code": "INVITE_ALREADY_PROCESSED",
    "message": "Приглашение уже принято/отклонено/просрочено"
  }
}
```

❌ **500 Internal Server Error**

```json
{ "error": { "code": "INTERNAL_ERROR", "message": "Ошибка сервера" } }
```

**Пояснение:** это действие меняет состояние приглашения, поэтому `POST` на “action endpoint” (`/accept`) — понятно и наглядно для ДЗ.

---

## 5.4 Отклонить приглашение

**Method:** POST
**URL:** `/api/v1/invites/{inviteId}/reject` (пример: `/api/v1/invites/inv_9001/reject`)

**Заголовки запроса:**

```http
Host: localhost:8080
Connection: keep-alive
User-Agent: Sovokumus-WebApp/1.0
Accept: application/json
Accept-Language: ru-RU, ru;q=0.9, en;q=0.8
Accept-Encoding: gzip, deflate
Accept-Charset: utf-8
Content-Type: application/json; charset=utf-8
Content-Length: [автоматически]
```

**Тело запроса:**

```json
{
  "byTgId": 8195976349,
  "reason": "Уже есть команда"
}
```

**Заголовки ответов:**

```http
Content-Type: application/json; charset=utf-8
Content-Encoding: gzip
```

**Тела ответов:**

✅ **200 OK**

```json
{
  "id": "inv_9001",
  "status": "rejected",
  "rejectedAt": "2026-02-06T10:21:00Z"
}
```

❌ **400 Bad Request**

```json
{
  "error": {
    "code": "INVALID_ACTION",
    "message": "Некорректное действие или данные",
    "details": [{ "field": "byTgId", "issue": "required" }]
  }
}
```

❌ **404 Not Found**

```json
{ "error": { "code": "INVITE_NOT_FOUND", "message": "Приглашение не найдено" } }
```

❌ **409 Conflict**

```json
{
  "error": {
    "code": "INVITE_ALREADY_PROCESSED",
    "message": "Приглашение уже принято/отклонено/просрочено"
  }
}
```

❌ **413 Payload Too Large** (если reason слишком длинный)

```json
{ "error": { "code": "PAYLOAD_TOO_LARGE", "message": "Слишком большой запрос" } }
```

❌ **500 Internal Server Error**

```json
{ "error": { "code": "INTERNAL_ERROR", "message": "Ошибка сервера" } }
```

**Пояснение:** аналогично `/accept`, но меняем статус на `rejected`, а `reason` — просто доп. поле.

---
