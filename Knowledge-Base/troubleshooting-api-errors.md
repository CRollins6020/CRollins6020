# 🛠️ Troubleshooting API Errors

*Identify, interpret, and resolve common API error responses. This article walks through structured error handling, decoding status codes, and investigating failed requests using real-world examples.*

| **Field**       | **Value**                    |
|------------------|------------------------------|
| **Version**      | 1.2                          |
| **Author**       | Corey Rollins               |
| **Last Updated** | May 21, 2025                |
| **Status**       | Draft                        |
| **Source**       | GitHub Repository (TBD)      |

---

## Table of Contents

1. [Overview](#1-overview)  
2. [Interpreting HTTP Status Codes](#2-interpreting-http-status-codes)  
3. [Common Errors and Fixes](#3-common-errors-and-fixes)  
4. [Advanced Errors and Edge Cases](#4-advanced-errors-and-edge-cases)  
5. [Investigating Failed Requests](#5-investigating-failed-requests)  
6. [Reusable Code & Templates](#6-reusable-code--templates)  

---

## 1. Overview

APIs are central to many systems—but when something goes wrong, it’s essential to understand the error and resolve it quickly. This article outlines a structured approach to interpreting and troubleshooting API errors.

---

## 2. Interpreting HTTP Status Codes

Understanding the category of the status code is your first step in diagnosing issues.

| **Status Code Range** | **Meaning**      | **Typical Cause**                     |
|------------------------|------------------|----------------------------------------|
| 2xx                    | Success          | No issue—operation completed as expected |
| 4xx                    | Client Error     | Invalid input, authentication failure |
| 5xx                    | Server Error     | Backend or infrastructure failure     |

💡 **Tip**: Always check the `message` or `error` field in the response body for more context.

---

## 3. Common Errors and Fixes

### 3.1 400 Bad Request

- **What it means**: The request was malformed or missing required parameters.
- **How to fix**: Validate all input parameters and headers. Check content type (e.g., `application/json`).

### 3.2 401 Unauthorized

- **What it means**: Missing or invalid authentication credentials.
- **How to fix**: Verify access tokens or API keys. Ensure your credentials are current and correctly scoped.

### 3.3 403 Forbidden

- **What it means**: Authenticated but not authorized to perform the action.
- **How to fix**: Check user roles and permissions. Ensure the target resource allows the requested action.

### 3.4 404 Not Found

- **What it means**: The requested endpoint or resource does not exist.
- **How to fix**: Double-check URL paths and resource IDs. Confirm the resource is available and accessible.

### 3.5 429 Too Many Requests

- **What it means**: Rate limit exceeded.
- **How to fix**: Back off and retry after the `Retry-After` header value. Implement request throttling in your client.

### 3.6 500 Internal Server Error

- **What it means**: Something failed on the server.
- **How to fix**: Retry after a short delay. If persistent, report the issue with the request ID (if available).

---

## 4. Advanced Errors and Edge Cases

### 4.1 422 Unprocessable Entity

- **What it means**: The server understands the request but cannot process the contained instructions.
- **How to fix**: Validate the JSON structure, field types, and required values. Remove nulls or improperly formatted nested objects.

### 4.2 503 Service Unavailable

- **What it means**: The server is temporarily overloaded or down for maintenance.
- **How to fix**: Retry using exponential backoff. Monitor platform status pages if available.

### 4.3 408 Request Timeout

- **What it means**: The client took too long to send a request.
- **How to fix**: Optimize payloads, adjust timeout settings, or investigate network conditions.

---

## 5. Investigating Failed Requests

### 5.1 Enable Debug Logging

Capture full request/response pairs including:

- Method, URL, and query parameters  
- Headers (`Authorization`, `Content-Type`, etc.)  
- Payloads  
- Response body and status codes  
- Request ID (if available)

### 5.2 Use a Tool Like Postman or cURL

Reproduce the request manually:

```bash
curl -X POST https://api.example.com/v1/users \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{"email": "user@example.com"}'
```

Use the results to validate your assumptions and compare against expected responses. This helps isolate client-side vs. server-side issues. Share the full trace and request ID with engineering if escalation is needed.

---

### 5.3 Additional Example: GET Request with Invalid Token

```bash
curl -X GET https://api.example.com/v1/profile   -H "Authorization: Bearer INVALID_OR_EXPIRED_TOKEN"
```

Expected output:

```json
{
  "error": {
    "code": 401,
    "message": "Unauthorized: Invalid or expired token.",
    "request_id": "abc123"
  }
}
```

---

## 6. Reusable Code & Templates

> 🧩 Copy-paste ready snippets for documentation reuse

### 6.1 Standard Retry Logic (Python)

```python
import time
import requests

for attempt in range(3):
    response = requests.get(url)
    if response.status_code == 200:
        break
    elif response.status_code in [429, 503, 408]:
        time.sleep(2 ** attempt)
    else:
        raise Exception(response.text)
```

---

### 6.2 Authorization Header Example

```http
Authorization: Bearer <access_token>
```

Ensure your token is active, correctly scoped, and not expired.

---

### 6.3 Error Response Template

```json
{
  "error": {
    "code": 403,
    "message": "Access denied: insufficient permissions.",
    "request_id": "xyz789",
    "timestamp": "2025-05-21T12:00:00Z"
  }
}
```

---

[🔝 Back to top](#-troubleshooting-api-errors)
