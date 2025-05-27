# User Management API Documentation

**Version:** v2.1 | **Base URL:** `https://api.example.com/v2` | **Updated:** May 26, 2025  
**Authentication:** Bearer Token | **Status:** Stable | **Support:** api-support@example.com

---

## Table of contents

1. [Overview](#1-overview)  
2. [Authentication](#2-authentication)  
3. [Common use cases](#3-common-use-cases)  
4. [Endpoints](#4-endpoints)  
   - [Create a new user](#41-create-a-new-user)  
   - [Retrieve user info](#42-retrieve-user-info)  
   - [Update user details](#43-update-user-details)  
   - [Delete user](#44-delete-user)  
5. [Error handling](#5-error-handling)  
6. [Rate limiting](#6-rate-limiting)  

---

## 1. Overview

The User Management API allows you to create, retrieve, update, and delete user accounts with specific role assignments. It supports common account lifecycle operations required for enterprise environments.

**Base URL:** `https://api.example.com/v2`

---

## 2. Authentication

All API calls require a Bearer Token provided in the `Authorization` header.

**Example:**
```http
Authorization: Bearer sk_live_yourtoken
```

🚨 **Important:** Never expose your token in frontend code or public repositories.

---

## 3. Common use cases

### 3.1 Create a user

```bash
curl -X POST "https://api.example.com/v2/users" \
  -H "Authorization: Bearer sk_live_yourtoken" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "jane@example.com",
    "name": "Jane Doe",
    "role": "admin"
}'
```

🎯 **You should see:**
```json
{
  "id": "usr_001122",
  "email": "jane@example.com",
  "name": "Jane Doe",
  "role": "admin",
  "status": "invited",
  "created_at": "2025-05-26T10:00:00Z"
}
```

---

### 3.2 Retrieve user info

```bash
curl -X GET "https://api.example.com/v2/users/usr_001122" \
  -H "Authorization: Bearer sk_live_yourtoken"
```

🎯 **You should see:**
```json
{
  "id": "usr_001122",
  "email": "jane@example.com",
  "name": "Jane Doe",
  "role": "admin",
  "status": "active"
}
```

---

### 3.3 Update user role

```bash
curl -X PATCH "https://api.example.com/v2/users/usr_001122" \
  -H "Authorization: Bearer sk_live_yourtoken" \
  -H "Content-Type: application/json" \
  -d '{
    "role": "member"
}'
```

🎯 **You should see:**
```json
{
  "id": "usr_001122",
  "role": "member",
  "updated_at": "2025-05-26T12:30:00Z"
}
```

---

### 3.4 Delete a user

```bash
curl -X DELETE "https://api.example.com/v2/users/usr_001122" \
  -H "Authorization: Bearer sk_live_yourtoken"
```

🎯 **You should see:**
```json
{
  "id": "usr_001122",
  "deleted": true
}
```

---

## 4. Endpoints

---

### 4.1 Create a new user

**Endpoint:** `POST /v2/users`  
**Purpose:** Adds a new user with specified permissions.

| Parameter | Type   | Required | Description              | Example               |
|-----------|--------|----------|--------------------------|-----------------------|
| email     | string | ✅       | User's email address     | `jane@example.com`    |
| name      | string | ✅       | Full name of the user    | `Jane Doe`            |
| role      | string | ⚠️       | Role assigned to user    | `admin`, `member`     |

---

### 4.2 Retrieve user info

**Endpoint:** `GET /v2/users/{user_id}`  
**Purpose:** Retrieve user details by ID.  
🚨 **Include your authentication token.**

---

### 4.3 Update user details

**Endpoint:** `PATCH /v2/users/{user_id}`  
**Purpose:** Modify role or name of a specific user.  
⚠️ You can update one or more fields.

---

### 4.4 Delete user

**Endpoint:** `DELETE /v2/users/{user_id}`  
**Purpose:** Permanently removes a user from your organization.

---

## 5. Error handling

| Code | Type        | Message                | Explanation                                    |
|------|-------------|------------------------|------------------------------------------------|
| 400  | Client Error| Bad Request            | Required field missing or invalid format       |
| 401  | Auth Error  | Unauthorized           | API token missing or incorrect                 |
| 403  | Auth Error  | Forbidden              | Token lacks required permissions               |
| 404  | Client Error| Not Found              | User ID not found                              |
| 409  | Conflict    | Email Already Exists   | Email is already in use                        |

🚨 **Common error:** `403 Forbidden` means your token lacks user management permissions.

---

## 6. Rate limiting

| Plan       | Requests/Minute |
|------------|------------------|
| Free       | 60               |
| Pro        | 250              |
| Enterprise | Custom           |

Exceeding this limit returns a `429 Too Many Requests` response. Retry after the interval provided in the `Retry-After` header.

---

**Need help?** [Support Portal](https://example.com/support) | [Developer Forum](https://forum.example.com) | [GitHub Issues](https://github.com/example/issues)  
**Resources:** [Code Examples](https://github.com/example/snippets) | [SDKs](https://example.com/sdk) | [Changelog](https://example.com/changelog)

*Last updated: May 26, 2025 | Next review: June 2025*
