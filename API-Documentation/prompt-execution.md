# Prompt Execution API Documentation

**Version:** v1.3 | **Base URL:** `https://api.exampleai.com/v1` | **Updated:** May 26, 2025  
**Authentication:** Bearer Token | **Status:** Stable | **Support:** api-support@exampleai.com

---

## Table of contents

1. [Overview](#1-overview)  
2. [Authentication](#2-authentication)  
3. [Common use cases](#3-common-use-cases)  
4. [Endpoints](#4-endpoints)  
   - [Submit a prompt](#41-submit-a-prompt)  
   - [Check prompt status](#42-check-prompt-status)  
   - [Retrieve prompt result](#43-retrieve-prompt-result)  
5. [Error handling](#5-error-handling)  
6. [Rate limiting](#6-rate-limiting)  

---

## 1. Overview

The Prompt Execution API allows your application to submit natural language prompts to a hosted LLM agent, check the status of the request, and retrieve results once the output is generated.

**Base URL:** `https://api.exampleai.com/v1`

This API supports synchronous and asynchronous execution for high-availability use cases such as task automation, real-time query handling, and agent workflows.

---

## 2. Authentication

All endpoints require a Bearer Token passed in the `Authorization` header.

**Example:**
```http
Authorization: Bearer sk_test_yourapikey123
```

🚨 **Important:** Never expose your token in frontend code or client applications.

---

## 3. Common use cases

### 3.1 Submit a prompt for execution

Use this to send input to the AI system and receive a response in real-time or asynchronously.

```bash
curl -X POST https://api.exampleai.com/v1/prompts \
  -H "Authorization: Bearer sk_test_yourapikey123" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "Summarize the key themes of Macbeth.",
    "temperature": 0.7,
    "mode": "sync"
}'
```

🎯 **You should see:** A JSON response with generated text or an execution ID for later retrieval.

---

### 3.2 Check the status of a prompt

Useful for asynchronous calls where you need to poll for completion.

```bash
curl -X GET https://api.exampleai.com/v1/prompts/exec_456789/status \
  -H "Authorization: Bearer sk_test_yourapikey123"
```

🎯 **You should see:** `"status": "completed"` or `"status": "processing"`

---

### 3.3 Retrieve completed output

Fetches the final result once the prompt has been processed.

```bash
curl -X GET https://api.exampleai.com/v1/prompts/exec_456789/result \
  -H "Authorization: Bearer sk_test_yourapikey123"
```

🎯 **You should see:** `"result": "Macbeth explores ambition, guilt, and fate..."`

---

## 4. Endpoints

---

### 4.1 Submit a prompt

**Endpoint:** `POST /v1/prompts`  
**Purpose:** Send a prompt for LLM execution, either synchronous or asynchronous.

**Parameters:**

| Parameter     | Type     | Required      | Description                             | Example                              |
|--------------|----------|---------------|-----------------------------------------|--------------------------------------|
| `prompt`     | string   | ✅ Required    | The natural language input to execute   | `"What's the weather in Boston?"`    |
| `temperature`| float    | ⚠️ Optional    | Controls randomness (0–1)               | `0.7`                                |
| `mode`       | string   | ⚠️ Optional    | Execution mode (`sync` or `async`)      | `"sync"`                             |

---

### 4.2 Check prompt status

**Endpoint:** `GET /v1/prompts/{execution_id}/status`  
**Purpose:** Poll the API to check if the async job is complete.

**Path parameter:**

| Parameter       | Type   | Required   | Description                     | Example         |
|----------------|--------|------------|---------------------------------|-----------------|
| `execution_id` | string | ✅ Required| Unique ID of the execution job  | `exec_abc123`   |

---

### 4.3 Retrieve prompt result

**Endpoint:** `GET /v1/prompts/{execution_id}/result`  
**Purpose:** Retrieve the completed output for the given prompt.

---

## 5. Error handling

| Status Code | Meaning              | Description                                         |
|-------------|----------------------|-----------------------------------------------------|
| 400         | Bad Request          | Invalid or missing parameters                       |
| 401         | Unauthorized         | Invalid or missing API key                          |
| 403         | Forbidden            | Token lacks permissions for prompt execution        |
| 429         | Too Many Requests    | Rate limit exceeded                                 |
| 500         | Internal Server Error| Unexpected error on the server side                 |

🚨 **Common error:** `401 Unauthorized` usually means your API key is incorrect or expired.

---

## 6. Rate limiting

This API enforces rate limits to ensure fair usage.

| Plan Type | Max Requests per Minute |
|-----------|-------------------------|
| Free      | 30                      |
| Pro       | 120                     |
| Enterprise| Custom (contact support)|

Exceeding the limit will return HTTP `429 Too Many Requests`.

---

**Need help?** [Support Portal](https://exampleai.com/support) | [Developer Forum](https://forum.exampleai.com) | [GitHub Issues](https://github.com/exampleai/issues)  
**Resources:** [Code Examples](https://github.com/exampleai/snippets) | [SDKs](https://exampleai.com/sdk) | [Changelog](https://exampleai.com/changelog)

*Last updated: May 26, 2025 | Next review: June 2025*
