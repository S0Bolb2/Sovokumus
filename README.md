# Sovokumus API (v1)

REST API для приложения по поиску сокомандников на олимпиады (профили, олимпиады, команды, приглашения).  
Базовый URL: `http://localhost:8080` потом скорее всего будет домен sovokumus.ru

---

## Содержание

- [1. Пользователь / Профиль](#1-пользователь--профиль)
  - [1.1 Получить профиль по tgId](#11-получить-профиль-по-tgid)
  - [1.2 Создать профиль](#12-создать-профиль)
  - [1.3 Обновить профиль по tgId](#13-обновить-профиль-по-tgid)
- [2. Олимпиады](#2-олимпиады)
  - [2.1 Получить список олимпиад (поиск/фильтры)](#21-получить-список-олимпиад-поискфильтры)
  - [2.2 Получить олимпиаду по id](#22-получить-олимпиаду-по-id)
- [3. Команды](#3-команды)
  - [3.1 Создать команду](#31-создать-команду)
  - [3.2 Получить список команд (фильтры)](#32-получить-список-команд-фильтры)
  - [3.3 Получить команду по id](#33-получить-команду-по-id)
  - [3.4 Обновить команду](#34-обновить-команду)
  - [3.5 Закрыть набор в команду](#35-закрыть-набор-в-команду)
- [4. Поиск сокомандников (пользователи)](#4-поиск-сокомандников-пользователи)
  - [4.1 Поиск пользователей по навыкам/предметам](#41-поиск-пользователей-по-навыкампредметам)
- [5. Приглашения (invites)](#5-приглашения-invites)
  - [5.1 Отправить приглашение пользователю в команду](#51-отправить-приглашение-пользователю-в-команду)
  - [5.2 Получить входящие приглашения пользователя](#52-получить-входящие-приглашения-пользователя)
  - [5.3 Принять приглашение](#53-принять-приглашение)
  - [5.4 Отклонить приглашение](#54-отклонить-приглашение)



---

## 1. Пользователь / Профиль

### 1.1 Получить профиль по tgId

**GET** `/api/v1/users/{tgId}`  
Пример: `/api/v1/users/7947922450`

**Request:** тело отсутствует

**Responses:**

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

---

### 1.2 Создать профиль

**POST** `/api/v1/users`

**Request body:**
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

**Responses:**

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

---

### 1.3 Обновить профиль по tgId

**PUT** `/api/v1/users/{tgId}`  
Пример: `/api/v1/users/7947922450`

**Request body:**
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

**Responses:**

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

---

## 2. Олимпиады

### 2.1 Получить список олимпиад (поиск/фильтры)

**GET** `/api/v1/olympiads?query=инф&level=regional&subject=informatics&page=1&limit=20`

**Request:** тело отсутствует

**Responses:**

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

---

### 2.2 Получить олимпиаду по id

**GET** `/api/v1/olympiads/{olympiadId}`  
Пример: `/api/v1/olympiads/ol_10`

**Request:** тело отсутствует

**Responses:**

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
```json
{ "error": { "code": "INTERNAL_ERROR", "message": "Ошибка сервера" } }
```

---

## 3. Команды

### 3.1 Создать команду

**POST** `/api/v1/teams`

**Request body:**
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

**Responses:**

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

---

### 3.2 Получить список команд (фильтры)

**GET** `/api/v1/teams?olympiadId=ol_10&city=Казань&remote=true&skill=graphs&page=1&limit=20`

**Request:** тело отсутствует

**Responses:**

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

---

### 3.3 Получить команду по id

**GET** `/api/v1/teams/{teamId}`  
Пример: `/api/v1/teams/t_501`

**Request:** тело отсутствует

**Responses:**

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

---

### 3.4 Обновить команду

**PUT** `/api/v1/teams/{teamId}`  
Пример: `/api/v1/teams/t_501`

**Request body:**
```json
{
  "title": "Кодим и побеждаем (обновлено)",
  "description": "Теперь ищем 1 человека по DP",
  "requiredSkills": ["dp"],
  "isRemoteAllowed": true,
  "city": "Казань"
}
```

**Responses:**

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

---

### 3.5 Закрыть набор в команду

**PATCH** `/api/v1/teams/{teamId}/close`  
Пример: `/api/v1/teams/t_501/close`

**Request:** тело отсутствует

**Responses:**

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

---

## 4. Поиск сокомандников (пользователи)

### 4.1 Поиск пользователей по навыкам/предметам

**GET** `/api/v1/users?skill=graphs&subject=informatics&level=advanced&city=Казань&remote=true&page=1&limit=20`

**Request:** тело отсутствует

**Responses:**

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

---

## 5. Приглашения (invites)

### 5.1 Отправить приглашение пользователю в команду

**POST** `/api/v1/teams/{teamId}/invites`  
Пример: `/api/v1/teams/t_501/invites`

**Request body:**
```json
{
  "fromTgId": 7947922450,
  "toTgId": 8195976349,
  "message": "Привет! Ищем сильного по графам, го к нам?"
}
```

**Responses:**

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

❌ **404 Not Found** (команда или пользователь)
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

---

### 5.2 Получить входящие приглашения пользователя

**GET** `/api/v1/users/{tgId}/invites?status=pending&page=1&limit=20`  
Пример: `/api/v1/users/8195976349/invites?status=pending&page=1&limit=20`

**Request:** тело отсутствует

**Responses:**

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

---

### 5.3 Принять приглашение

**POST** `/api/v1/invites/{inviteId}/accept`  
Пример: `/api/v1/invites/inv_9001/accept`

**Request body:** (можно пустое, но явнее указать, кто принимает)
```json
{
  "byTgId": 8195976349
}
```

**Responses:**

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

---

### 5.4 Отклонить приглашение

**POST** `/api/v1/invites/{inviteId}/reject`  
Пример: `/api/v1/invites/inv_9001/reject`

**Request body:**
```json
{
  "byTgId": 8195976349,
  "reason": "Уже есть команда"
}
```

**Responses:**

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

❌ **413 Payload Too Large**
```json
{ "error": { "code": "PAYLOAD_TOO_LARGE", "message": "Слишком большой запрос" } }
```

❌ **500 Internal Server Error**
```json
{ "error": { "code": "INTERNAL_ERROR", "message": "Ошибка сервера" } }
```

---

### Быстрые примеры (curl)

```bash
# Получить профиль
curl -i "http://localhost:8080/api/v1/users/7947922450"

# Создать профиль
curl -i -X POST "http://localhost:8080/api/v1/users" \
  -H "Content-Type: application/json; charset=utf-8" \
  -d '{"tgId":7947922450,"name":"Алексей","city":"Казань","bio":"Ищу команду на олимпиады","skills":["graphs"],"subjects":["informatics"],"experienceLevel":"middle"}'

# Создать команду
curl -i -X POST "http://localhost:8080/api/v1/teams" \
  -H "Content-Type: application/json; charset=utf-8" \
  -d '{"ownerTgId":7947922450,"title":"Кодим и побеждаем","olympiadId":"ol_10","description":"Ищем 1-2 сильных по графам","requiredSkills":["graphs","dp"],"city":"Казань","isRemoteAllowed":true}'

# Отправить приглашение
curl -i -X POST "http://localhost:8080/api/v1/teams/t_501/invites" \
  -H "Content-Type: application/json; charset=utf-8" \
  -d '{"fromTgId":7947922450,"toTgId":8195976349,"message":"Привет! Ищем сильного по графам, го к нам?"}'
```

---
