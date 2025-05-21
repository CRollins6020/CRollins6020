# 🔄 Prompt Execution API

*Submit and manage AI prompt requests. Designed for interacting with LLM-based agents, this API reference covers prompt submission, batch processing, validation, and session history retrieval.*

| **Field**       | **Value**                      |
|------------------|--------------------------------|
| **Version**      | 1.0                            |
| **Author**       | Corey Rollins                 |
| **Last Updated** | May 21, 2025                  |
| **Status**       | Draft                          |
| **Source**       | GitHub Repository (TBD)        |

---

## Table of Contents

1. [Overview](#1-overview)  
2. [Authentication](#2-authentication)  
3. [Endpoints](#3-endpoints)  
4. [Error Handling](#4-error-handling)  
5. [Rate Limiting](#5-rate-limiting)  
6. [Session History](#6-session-history)  

---

## 1. Overview

The Prompt Execution API allows internal services to submit and manage prompt requests for AI-driven agents. It supports synchronous and asynchronous operations, batch execution, validation checks, and retrieval of past interactions.

**Example Use Cases:**
- Submit individual or batch prompts to LLMs
- Log and retrieve agent interaction history for audits
- Validate prompt structure before execution
- Reprocess failed requests in retry workflows

---

## 2. Authentication

### 2.1 Supported Methods

| **Method**       | **Use Case**                    |
|------------------|----------------------------------|
| OAuth 2.0        | Standard delegated access        |
| API Key          | Internal service-to-service calls |

### 2.2 Token Format

Include an authentication token in the `Authorization` header.

```http
Authorization: Bearer <access_token>
```

Refer to your internal identity provider or SSO documentation for token generation.

[🔝 Back to top](#table-of-contents)

---

## 3. Endpoints

### 3.1 POST /api/v1/prompt
Submit a single prompt for processing.

| Field     | Type    | Required | Description                     |
|-----------|---------|----------|---------------------------------|
| prompt    | string  | ✅       | Text of the prompt              |
| metadata  | object  | ❌       | Optional context (e.g., user ID)|
| async     | boolean | ❌       | Run in background (default: false)|

**Response:**
```json
{
  "status": "submitted",
  "request_id": "abc123"
}
```

___

### 3.2 POST /api/v1/prompts/batch
Submit multiple prompts in a single request.

| Field     | Type          | Required | Description             |
|-----------|---------------|----------|-------------------------|
| prompts   | array[string] | ✅       | List of prompts to submit |

Response includes request IDs for each entry.

___

### 3.3 GET /api/v1/prompt/{id}
Retrieve the result or status of a submitted prompt.

___

### 3.4 GET /api/v1/validate
Validate prompt structure before execution.

[🔝 Back to top](#table-of-contents)

---

## 4. Error Handling
Errors return standard HTTP status codes and a descriptive message.

| Code | Meaning               | Notes                            |
|------|------------------------|----------------------------------|
| 400  | Bad Request            | Malformed prompt or missing field|
| 401  | Unauthorized           | Invalid or missing token         |
| 429  | Too Many Requests      | Rate limit exceeded              |
| 500  | Internal Server Error  | Contact support if persistent    |

💡 Use retry logic with exponential backoff for 429 and 500 responses.

[🔝 Back to top](#table-of-contents)

---

## 5. Rate Limiting
Each client is limited to:

- 60 requests/minute per API key  
- 500 requests/day per user session

🛑 Rate limit headers are included in all responses.

[🔝 Back to top](#table-of-contents)

---

## 6. Session History

### 6.1 GET /api/v1/sessions/{user_id}
Retrieve all prompt requests and responses tied to a user session. Useful for diagnostics and compliance review.

**Optional Query Params:**
- `limit` — max results (default 100)
- `from_date` — filter by start timestamp

[🔝 Back to top](#table-of-contents)
