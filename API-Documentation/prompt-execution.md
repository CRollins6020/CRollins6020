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
| **Last Updated**  | 2025-05-29                                 |
| **OpenAPI Spec**  | [openapi.yaml](https://example.com/spec)   |
| **Support Contact**| dev-support@promptengine.io               |

---

## 📚 Table of Contents

1. [Overview](#1-overview)  
2. [Authentication](#2-authentication)  
3. [Rate Limits](#3-rate-limits)  
4. [Error Codes](#4-error-codes)  
5. [Endpoints](#5-endpoints)  
    - [POST /prompts/execute](#post-promptsexecute)  
    - [GET /prompts/{id}/status](#get-promptsidstatus)  
6. [Common Use Cases](#6-common-use-cases)  
7. [Data Models](#7-data-models)  
8. [Changelog](#8-changelog)

---

## 1. Overview

The Prompt Execution API lets you send text prompts to a language model and receive generated responses.
It’s designed to integrate AI completions into your products, workflows, or internal systems.
Whether you're building a chatbot, content generator, or automated QA system, this API gives you the power to scale language-based tasks easily.

### 🔍 How It Works

1. **Submit a prompt**  
   Send a `POST` request to `/prompts/execute` with the prompt text and optional configuration parameters (e.g., model name, streaming mode).

2. **Receive an execution ID**  
   The API responds with a unique `execution_id`, which tracks the lifecycle of the prompt execution.

3. **Check execution status**  
   Use a `GET` request to `/prompts/{id}/status` to retrieve the current status (queued, processing, completed, or failed).

4. **Retrieve the result**  
   Once processing is complete, the final output is returned in the `output` field of the response JSON.

This architecture allows for flexible and scalable use of AI models in any system that can make secure HTTPS requests.


The Prompt Execution API allows you to programmatically execute prompts, retrieve results, and track execution status.  
Ideal for integrating AI-driven completion tasks into your own applications, workflows, or backend systems.

---

## 2. Authentication

Authentication is required to access the API securely. Each request must include a valid access token in the `Authorization` header. These tokens help us verify your identity and ensure your usage is authorized and tracked appropriately.


This API uses **Bearer Token Authentication**.  
Include the token in the `Authorization` header:

```http
Authorization: Bearer YOUR_ACCESS_TOKEN
```

Tokens are issued via the developer dashboard or admin endpoint.

---

## 3. Rate Limits

To ensure fair usage and system stability, the API enforces rate limits based on your subscription tier. If you exceed the allowed number of requests per minute, the API will return a `429 Too Many Requests` error. Consider upgrading your plan or spacing out your requests if you regularly hit the limit.


| Plan         | Requests per Minute |
|--------------|---------------------|
| Free         | 30                  |
| Pro          | 500                 |
| Enterprise   | 2,000               |

A `429 Too Many Requests` error is returned when limits are exceeded.

---

## 4. Error Codes

The API responds with standard HTTP status codes to indicate the outcome of your request. Understanding these codes can help you quickly diagnose and fix issues during development and integration.


| Code | Meaning             | Description                               |
|------|---------------------|-------------------------------------------|
| 200  | OK                  | Request succeeded                         |
| 202  | Accepted            | Execution request received and queued     |
| 400  | Bad Request         | Invalid input format or parameters        |
| 401  | Unauthorized        | Token is invalid or missing               |
| 403  | Forbidden           | Access denied due to permissions          |
| 404  | Not Found           | Resource does not exist                   |
| 429  | Too Many Requests   | Rate limit exceeded                       |
| 500  | Server Error        | Internal server error                     |

### Example Error Response

```json
{
  "error": {
    "code": "INVALID_PROMPT",
    "message": "The provided prompt format is invalid."
  }
}
```

---

## 5. Endpoints

### POST /prompts/execute

Submit a prompt for execution.

```http
POST /v1/prompts/execute
Content-Type: application/json
```

**Body Parameters:**

| Name         | Type     | Required | Description                             |
|--------------|----------|----------|-----------------------------------------|
| `prompt`     | string   | Yes      | The text or structured prompt to execute |
| `model`      | string   | No       | Model to use (default: `gpt-4`)         |
| `stream`     | boolean  | No       | If true, enables streaming response     |
| `metadata`   | object   | No       | Optional metadata to tag the request    |

**Response:**

```json
{
  "execution_id": "abc123",
  "status": "queued"
}
```

---

### GET /prompts/{id}/status

Check the status or result of a previously submitted execution.

```http
GET /v1/prompts/{id}/status
```

**Path Parameters:**

| Name      | Type   | Required | Description                  |
|-----------|--------|----------|------------------------------|
| `id`      | string | Yes      | ID of the prompt execution   |

**Response:**

```json
{
  "execution_id": "abc123",
  "status": "completed",
  "output": "Here is your generated result."
}
```

---

## 6. Common Use Cases

These examples show how to use the API in real-world scenarios. They demonstrate the basic request and response patterns for executing prompts and retrieving their results.


- Execute a basic prompt:

```http
POST /v1/prompts/execute
Content-Type: application/json

{
  "prompt": "Summarize the history of AI in 3 sentences."
}
```

- Retrieve output after submission:

```http
GET /v1/prompts/abc123/status
```

- Execute with metadata and a custom model:

```http
POST /v1/prompts/execute
Content-Type: application/json

{
  "prompt": "Generate a list of technical documentation best practices.",
  "model": "gpt-4-turbo",
  "metadata": {
    "project_id": "doc-suite",
    "user": "writer-corey"
  }
}
```

---

## 7. Data Models

The following JSON models describe the structure of request and response payloads. They provide a clear view of what data to send and expect when interacting with the API.


### Execution Request

```json
{
  "prompt": "string",
  "model": "gpt-4",
  "stream": false,
  "metadata": {
    "key": "value"
  }
}
```

| Field      | Type     | Description                                   |
|------------|----------|-----------------------------------------------|
| `prompt`   | string   | Prompt text to be executed                    |
| `model`    | string   | Optional model name (e.g., gpt-4, gpt-3.5)    |
| `stream`   | boolean  | Whether to receive results as a stream        |
| `metadata` | object   | Optional key-value pairs for internal tagging |

### Execution Response

```json
{
  "execution_id": "string",
  "status": "queued"
}
```

| Field          | Type   | Description                                 |
|----------------|--------|---------------------------------------------|
| `execution_id` | string | Unique ID used to retrieve execution status |
| `status`       | string | One of: `queued`, `processing`, `completed`, `failed` |

### Status Response

```json
{
  "execution_id": "string",
  "status": "completed",
  "output": "string"
}
```

| Field     | Type   | Description                  |
|-----------|--------|------------------------------|
| `status`  | string | Current execution status     |
| `output`  | string | Final response text          |

---

## 8. Changelog

| Version | Date       | Notes                        |
|---------|------------|------------------------------|
| v1.0.0  | 2025-05-29 | Initial version of the API   |

---

[🔝 Back to Top](#prompt-execution-api-reference)
