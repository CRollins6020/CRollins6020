# 🛠️ Troubleshooting API Errors

*Identify, interpret, and resolve common API error responses. This article walks through structured error handling, decoding status codes, and investigating failed requests using real-world examples.*

| **Field**       | **Value**                    |
|------------------|------------------------------|
| **Version**      | 1.1                          |
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
6. [Reusable Components](#6-reusable-components)  

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

> 📦 _This section can be reused as a standard troubleshooting module in API user guides or internal runbooks._

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

❓ **Note**: If your platform supports request IDs, include them in any support tickets to accelerate investigation.

---

## 4. Advanced Errors and Edge Cases

> 🧱 _Designed as a future extension or standalone module._

### 4.1 422 Unprocessable Entity  
- **What it means**: The server understands the request but cannot process the contained instructions.  
- **How to fix**: Confirm that your JSON body adheres to schema requirements and data types. Double-check for null values, unexpected object nesting, or unsupported formats.

### 4.2 503 Service Unavailable  
- **What it means**: The service is temporarily down or overloaded.  
- **How to fix**: Retry using exponential backoff. Monitor platform status dashboards or alerting systems for known issues or maintenance windows.

### 4.3 408 Request Timeout  
- **What it means**: The client did not produce a request within the server’s timeout window.  
- **How to fix**: Review client connection and read timeout settings. Optimize request payloads and reduce server-side latency where possible.

💡 **Tip**: Consider adding automated retries for transient 408/429/503 errors in your integration logic.

---

## 5. Investigating Failed Requests

### 5.1 Enable Debug Logging

Enable debug logging in your application or middleware. Capture full request/response pairs, including:

- Request method and URL
- Headers (especially `Authorization`, `Content-Type`)
- Payload/body
- Timestamps
- Response status and error message

This provides critical visibility into malformed input or unauthorized requests.

### 5.2 Use a Tool Like Postman or cURL

Use these tools to recreate failing requests manually and isolate the issue:

\`\`\`bash
curl -X POST https://api.example.com/v1/users \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{"email": "user@example.com"}'
\`\`\`

Doing this removes variables introduced by frontend or SDK logic and helps determine whether the problem is client-side or server-side.

Once the issue is reproduced and isolated, retest using corrected inputs. If unresolved, escalate using logs and request IDs.

---

## 6. Reusable Components

> 🧩 _These modular assets support reuse across KBs, onboarding guides, and developer documentation._

### 6.1 API Error Message Template

\`\`\`json
{
  "error": {
    "code": 403,
    "message": "Access denied: insufficient permissions.",
    "request_id": "abc123",
    "timestamp": "2025-05-21T14:34:09Z"
  }
}
\`\`\`

Use consistent structure across services for better parsing and logging.

---

### 6.2 Standard Retry Logic Snippet

\`\`\`python
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
\`\`\`

💡 **Tip**: Apply exponential backoff for transient errors.

---

### 6.3 Authentication Header Example

\`\`\`http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
\`\`\`

Confirm the token is correctly scoped and unexpired.

---

### 6.4 Token Expiry Troubleshooting Guide

1. Decode the token with a tool like jwt.io.  
2. Look for `exp` (expiration timestamp).  
3. Compare it to current UTC time.  
4. If expired, re-authenticate via SSO or OAuth flow.

---

### 6.5 Rate Limiting Best Practices

- Implement client-side request throttling  
- Respect `Retry-After` headers  
- Cache repetitive read calls  
- Use batch endpoints where available  

---

[🔝 Back to top](#-troubleshooting-api-errors)
