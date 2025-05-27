# Reporting & Analytics API Documentation

**Version:** v2.0 | **Base URL:** `https://api.exampleai.com/v2` | **Updated:** May 26, 2025  
**Authentication:** Bearer Token | **Status:** Stable | **Support:** api-support@exampleai.com

---

## Table of contents

1. [Overview](#1-overview)  
2. [Authentication](#2-authentication)  
3. [Common use cases](#3-common-use-cases)  
4. [Endpoints](#4-endpoints)  
   - [List reports](#41-list-reports)  
   - [Run a report](#42-run-a-report)  
   - [Download report results](#43-download-report-results)  
5. [Error handling](#5-error-handling)  
6. [Rate limiting](#6-rate-limiting)  

---

## 1. Overview

The Reporting & Analytics API allows users to programmatically access pre-defined or custom reports, execute them on demand, and download the results in CSV or JSON formats.

**Base URL:** `https://api.exampleai.com/v2`

This API is ideal for dashboard integration, operational tracking, and scheduled data exports.

---

## 2. Authentication

All requests require a Bearer Token in the `Authorization` header.

**Example:**
```http
Authorization: Bearer sk_test_reportingapikey123
```

🚨 **Important:** Use environment variables to secure API keys in server-side code.

---

## 3. Common use cases

### 3.1 List available reports

```bash
curl -X GET https://api.exampleai.com/v2/reports \
  -H "Authorization: Bearer sk_test_reportingapikey123"
```

🎯 **You should see:** A list of available reports with metadata like `id`, `name`, and `frequency`.

---

### 3.2 Run a report

```bash
curl -X POST https://api.exampleai.com/v2/reports/run \
  -H "Authorization: Bearer sk_test_reportingapikey123" \
  -H "Content-Type: application/json" \
  -d '{
    "report_id": "rpt_monthly_summary",
    "format": "csv"
}'
```

🎯 **You should see:** An execution ID you can use to retrieve results.

---

### 3.3 Download report results

```bash
curl -X GET https://api.exampleai.com/v2/reports/results/exec_123456 \
  -H "Authorization: Bearer sk_test_reportingapikey123"
```

🎯 **You should see:** A file stream or JSON response with the report data.

---

## 4. Endpoints

---

### 4.1 List reports

**Endpoint:** `GET /v2/reports`  
**Purpose:** Retrieve all available reports and their metadata.

---

### 4.2 Run a report

**Endpoint:** `POST /v2/reports/run`  
**Purpose:** Execute a report on demand.

**Parameters:**

| Parameter     | Type   | Required   | Description                      | Example                |
|--------------|--------|------------|----------------------------------|------------------------|
| `report_id`  | string | ✅ Required| Identifier of the report to run  | `"rpt_monthly_summary"`|
| `format`     | string | ⚠️ Optional| Output format: `csv` or `json`   | `"csv"`                |

---

### 4.3 Download report results

**Endpoint:** `GET /v2/reports/results/{execution_id}`  
**Purpose:** Retrieve the output file for a completed report execution.

---

## 5. Error handling

| Status Code | Meaning              | Description                                  |
|-------------|----------------------|----------------------------------------------|
| 400         | Bad Request          | Invalid or missing parameters                |
| 401         | Unauthorized         | Invalid or missing API key                   |
| 404         | Not Found            | Report or execution ID not found             |
| 429         | Too Many Requests    | Rate limit exceeded                          |
| 500         | Internal Server Error| Something went wrong on the server side      |

---

## 6. Rate limiting

Rate limits vary by plan:

| Plan Type | Max Requests per Minute |
|-----------|-------------------------|
| Free      | 20                      |
| Pro       | 100                     |
| Enterprise| Custom (contact support)|

Exceeding the limit returns HTTP `429 Too Many Requests`.

---

**Need help?** [Support Portal](https://exampleai.com/support) | [Developer Forum](https://forum.exampleai.com) | [GitHub Issues](https://github.com/exampleai/issues)  
**Resources:** [Code Examples](https://github.com/exampleai/snippets) | [SDKs](https://exampleai.com/sdk) | [Changelog](https://exampleai.com/changelog)

*Last updated: May 26, 2025 | Next review: June 2025*
