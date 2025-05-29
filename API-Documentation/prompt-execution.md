# Prompt Execution API Reference

**Document Type:** API Reference  
**Intended Audience:** Developers, System Integrators  
**Format:** Markdown with GitHub-compatible HTML

---

## 📘 Metadata

| Field              | Value                                      |
|-------------------|--------------------------------------------|
| **Version**       | v1.0.0                                     |
| **Base URL**      | `https://api.promptengine.io/v1`           |
| **Authentication**| Bearer Token                               |
| **Last Updated**  | 2025-05-29                             |
| **OpenAPI Spec**  | [openapi.yaml](https://example.com/spec)   |
| **Support Contact**| dev-support@promptengine.io               |

---

## 📚 Table of Contents

1. [Overview](#1-overview)  
2. [Authentication](#2-authentication)  
3. [Rate Limits](#3-rate-limits)  
4. [Error Codes](#4-error-codes)  
5. [Endpoints](#5-endpoints)  
6. [Common Use Cases](#6-common-use-cases)  
7. [Data Models](#7-data-models)  
8. [Changelog](#8-changelog)

---

## 1. Overview

The Prompt Execution API allows developers to send prompt payloads to a hosted large language model (LLM), receive structured responses, and manage prompt execution lifecycles for integrations, internal tools, or user-facing applications.

### 🔍 How It Works

1. **Prepare a Prompt Payload**  
   Structure your prompt using the required fields such as model, input, and configuration parameters (e.g., temperature, max_tokens).

2. **POST to the Execution Endpoint**  
   Submit the prompt to `/v1/prompt/execute` with appropriate headers and a JSON body.

3. **Handle the Response**  
   The API returns an output including generated text, usage metrics, and execution metadata.

4. **Monitor and Optimize**  
   Use response metadata for logging, debugging, and cost tracking.

---

## 2. Authentication

### 🔍 How It Works

The API requires a valid access token to authorize each request. This token is included in the `Authorization` header using the Bearer scheme. Tokens are typically generated via a web interface or developer console, and they help ensure secure, auditable access to the system.

Authentication is required to access the API securely.  
Include a valid token in the `Authorization` header:

```http
Authorization: Bearer YOUR_ACCESS_TOKEN
```

Tokens can be generated via the developer dashboard or an admin endpoint.

---

## 3. Rate Limits

### 🔍 How It Works

Each plan tier defines how many requests a client can make per minute. When you exceed your rate limit, the server returns a `429` status. You can reduce request frequency, retry with a delay, or upgrade your plan to continue uninterrupted.

Specify your plan's rate limits to ensure fair usage.  

| Plan         | Requests per Minute |
|--------------|---------------------|
| Free         | 60                  |
| Pro          | 1,000               |
| Enterprise   | 5,000               |

A `429 Too Many Requests` error will occur if the limit is exceeded.

---

## 4. Error Codes

### 🔍 How It Works

The API uses standard HTTP status codes to report the result of your request. These responses allow you to programmatically handle errors (e.g., retry a 500 error, prompt login on 401). The response body often includes an `error` object with a code and a message for troubleshooting.

These are the standard responses you can expect:

| Code | Meaning             | Description                           |
|------|---------------------|---------------------------------------|
| 200  | OK                  | Request succeeded                     |
| 202  | Accepted            | Request accepted, processing queued   |
| 400  | Bad Request         | Invalid or missing parameters         |
| 401  | Unauthorized        | Missing or invalid authentication     |
| 403  | Forbidden           | Insufficient permissions              |
| 404  | Not Found           | Requested resource doesn't exist      |
| 429  | Too Many Requests   | Rate limit exceeded                   |
| 500  | Server Error        | Internal server issue                 |

### Example Error Response

```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Error message description."
  }
}
```

---

## 5. Endpoints

### 🔍 How It Works

Each endpoint represents a specific API action or resource. Use HTTP methods (`GET`, `POST`, `PUT`, `DELETE`) to perform operations. Combine endpoints with the correct URL paths, headers, and request bodies to interact with the API programmatically.

### POST /prompt/execute

Execute a new prompt.

```http
POST /v1/prompt/execute
Content-Type: application/json
```

**Request Body:**

| Field         | Type     | Required | Description                      |
|---------------|----------|----------|----------------------------------|
| `model`       | string   | Yes      | LLM to use (e.g., "gpt-4")       |
| `input`       | string   | Yes      | The prompt text                  |
| `temperature` | float    | No       | Randomness control (0.0–1.0)     |
| `max_tokens`  | integer  | No       | Limit on generated tokens        |

**Example Response:**

```json
{
  "id": "exec_abc123",
  "output": "The result of your prompt.",
  "usage": {
    "input_tokens": 25,
    "output_tokens": 100
  },
  "status": "completed"
}
```

---

## 6. Common Use Cases

Use these examples to illustrate typical usage:

```http
POST /v1/prompt/execute
Content-Type: application/json

{
  "model": "gpt-4",
  "input": "Explain quantum entanglement.",
  "temperature": 0.7,
  "max_tokens": 250
}
```

```http
GET /v1/executions/exec_abc123/status
```

---

## 7. Data Models

### 🔍 How It Works

Data models define the expected structure of inputs and outputs for the API. Knowing the schema ahead of time allows you to validate requests, anticipate fields in responses, and build integrations that won’t break with changing data.

### Example Request

```json
{
  "model": "gpt-4",
  "input": "Write a poem about the ocean.",
  "temperature": 0.5,
  "max_tokens": 100
}
```

| Field         | Type     | Description                            |
|---------------|----------|----------------------------------------|
| `model`       | string   | Identifier for the LLM                 |
| `input`       | string   | Prompt text to send                    |
| `temperature` | float    | Controls randomness of output          |
| `max_tokens`  | integer  | Maximum number of tokens in response   |

### Example Response

```json
{
  "id": "exec_def456",
  "output": "The sea was calm and blue...",
  "status": "completed"
}
```

| Field     | Type   | Description               |
|-----------|--------|---------------------------|
| `id`      | string | Execution identifier      |
| `output`  | string | Model's response text     |
| `status`  | string | Execution state           |

---

## 8. Changelog

| Version | Date       | Notes                        |
|---------|------------|------------------------------|
| v1.0.0  | 2025-05-29 | Initial release              |

---

[🔝 Back to Top](#prompt-execution-api-reference)
