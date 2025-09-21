# Express JWT Auth API

Base URL:

```
https://express-jwt-auth-vtri.onrender.com
```

## Overview

This is a **Node.js + Express + MongoDB** backend with **JWT authentication**.
It supports:

* User signup
* User login
* Access token & refresh token
* JWT-protected routes

---

## 📂 Base Routes

### 1. Signup

**Endpoint:**

```
POST /auth/signup
```

**Request Body (JSON):**

```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

**Response:**

```json
{
  "message": "User created",
  "userId": "64xxxxxxx"
}
```

---

### 2. Login

**Endpoint:**

```
POST /auth/login
```

**Request Body (JSON):**

```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

**Response:**

```json
{
  "accessToken": "<JWT_ACCESS_TOKEN>"
}
```

**Notes:**

* Sets `refreshToken` as **httpOnly cookie**.
* `accessToken` expires in 15 minutes.

---

### 3. Refresh Access Token

**Endpoint:**

```
GET /auth/refresh-token
```

**Request:**

* Must send **refreshToken cookie**.

**Response:**

```json
{
  "accessToken": "<NEW_ACCESS_TOKEN>"
}
```
---

### 5. Protected Route (Test)

**Endpoint:**

```
GET /auth/profile
```

**Headers:**

```
Authorization: Bearer <ACCESS_TOKEN>
```

**Response:**

```json
{
  "user": {
    "_id": "64xxxxxx",
    "email": "user@example.com",
    "createdAt": "2025-09-21T06:00:00.000Z",
    "updatedAt": "2025-09-21T06:10:00.000Z"
  }
}
```

**Notes:**

* Requires a valid **access token**.
* Returns user info **excluding password and refresh token**.

---

## ⚙️ Environment Variables

```env
PORT=5000
MONGO_URI=<your_mongodb_connection_string>
JWT_SECRET=<your_jwt_secret>
JWT_REFRESH_SECRET=<your_jwt_refresh_secret>
```

---

## 🔒 Authentication Flow

1. User **signup** → hashed password stored in MongoDB.
2. User **login** → returns `accessToken` and sets `refreshToken` cookie.
3. **Protected routes** require `accessToken` in Authorization header.
4. When `accessToken` expires → use `/auth/refresh-token` to get a new one.

---

## ✅ Notes

* All request and response bodies are in **JSON format**.
* Refresh token is stored in **httpOnly cookie** for security.
* Use `Authorization: Bearer <ACCESS_TOKEN>` for all protected endpoints.
