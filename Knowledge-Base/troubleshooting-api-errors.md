# 🛠️ Troubleshooting API Errors

*Identify, interpret, and resolve common API error responses. This article walks through structured error handling, decoding status codes, and investigating failed requests using real-world examples.*

| **Field**       | **Value**                    |
|------------------|------------------------------|
| **Version**      | 1.0                          |
| **Author**       | Corey Rollins               |
| **Last Updated** | May 21, 2025                |
| **Status**       | Draft                        |
| **Source**       | GitHub Repository (TBD)      |

---

## 1. Overview

APIs are central to many systems—but when something goes wrong, it’s essential to understand the error and resolve it quickly. This article outlines a structured approach to interpreting and troubleshooting API errors.

---

## 2. Interpreting HTTP Status Codes

Understanding the category of the status code is your first step in diagnosing issues.

| **Status Code Range** | **Meaning**                    | **Typical Cause**                          |
|------------------------|--------------------------------|--------------------------------------------|
| 2xx                    | Success                        | No issue—operation completed as expected   |
| 4xx                    | Client Error                   | Invalid input, authentication failure      |
| 5xx                    | Server Error                   | Backend or infrastructure failure          |

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

## 4. Investigating Failed Requests

### 4.1 Enable Debug Logging

Capture full request/response pairs including headers and payloads.

### 4.2 Use a Tool Like Postman or cURL

Reproduce the request manually to isolate client vs. server issues.

```bash
curl -X POST https://api.example.com/v1/users \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{"email": "user@example.com"}'
