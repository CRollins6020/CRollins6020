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

The Prompt Execution API allows developers to send prompts to a hosted large language model (LLM) and receive generated responses. It supports custom configurations such as model selection, temperature, and max token limits. Responses include output text, usage statistics, and execution metadata, enabling integration into applications or internal tools.

---

## 2. Authentication

The API uses Bearer tokens for authorization. Include your token in the `Authorization` header of each request. Tokens are typically generated from a developer dashboard or admin portal.

```http
Authorization: Bearer YOUR_ACCESS_TOKEN
```

---

## 3. Rate Limits

Rate limits vary by plan tier. Exceeding the limit returns a `429 Too Many Requests` response. Below are the default limits:

| Plan         | Requests per Minute |
|--------------|---------------------|
| Free         | 60                  |
| Pro          | 1,000               |
| Enterprise   | 5,000               |

To avoid interruptions, manage request frequency or upgrade your plan as needed.

---

## 4. Error Codes

The API uses standard HTTP status codes to report the result of a request. Error responses include a JSON object with a code and message to help you debug.

| Code | Meaning             | Description                           |
|------|---------------------|---------------------------------------|
| 200  | OK                  | Request succeeded                     |
| 202  | Accepted            | Request accepted, processing queued   |
| 400  | Bad Request         | Invalid or missing parameters         |
| 401  | Unauthorized        | Missing or invalid authentication     |
| 403  | Forbidden           | Insufficient permissions              |
| 404  | Not Found           | Resource doesn't exist                |
| 429  | Too Many Requests   | Rate limit exceeded                   |
| 500  | Server Error        | Internal server error                 |

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

### POST /prompt/execute

Submits a prompt for LLM processing.

```http
POST /v1/prompt/execute
Content-Type: application/json
```

**Request Body:**

| Field         | Type     | Required | Description                      |
|---------------|----------|----------|----------------------------------|
| `model`       | string   | Yes      | LLM to use (e.g., "gpt-4")       |
| `input`       | string   | Yes      | Prompt to execute                |
| `temperature` | float    | No       | Controls randomness (0.0–1.0)    |
| `max_tokens`  | integer  | No       | Max tokens in response           |

**Example Response:**

```json
{
  "id": "exec_abc123",
  "output": "Generated response here.",
  "usage": {
    "input_tokens": 25,
    "output_tokens": 100
  },
  "status": "completed"
}
```

---

## 6. Common Use Cases

These examples demonstrate typical prompt submissions:

```http
POST /v1/prompt/execute
Content-Type: application/json

{
  "model": "gpt-4",
  "input": "Summarize the plot of Hamlet.",
  "temperature": 0.5,
  "max_tokens": 200
}
```

```http
GET /v1/executions/exec_abc123/status
```

---

## 7. Data Models

Request and response formats follow standard JSON schemas. Define these clearly in your integration layer.

### Request

```json
{
  "model": "gpt-4",
  "input": "Write a haiku about rain.",
  "temperature": 0.7,
  "max_tokens": 50
}
```

| Field         | Type     | Description                            |
|---------------|----------|----------------------------------------|
| `model`       | string   | LLM to use                             |
| `input`       | string   | Prompt content                         |
| `temperature` | float    | Randomness control                     |
| `max_tokens`  | integer  | Token limit for output                 |

### Response

```json
{
  "id": "exec_xyz789",
  "output": "Rain drips from branches / Silence stirs the waking ground / Clouds begin to clear",
  "status": "completed"
}
```

| Field     | Type   | Description             |
|-----------|--------|-------------------------|
| `id`      | string | Execution ID            |
| `output`  | string | LLM-generated response  |
| `status`  | string | Execution status        |

---

## 8. Changelog

| Version | Date       | Notes                        |
|---------|------------|------------------------------|
| v1.0.0  | 2025-05-29 | Initial release              |

---

[🔝 Back to Top](#prompt-execution-api-reference)
