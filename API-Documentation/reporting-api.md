
# 📊 Reporting & Analytics API

*Access detailed usage metrics and system logs. This reference supports filtering, pagination, and report exports—ideal for building dashboards and automating internal reporting.*

| **Field**       | **Value**                    |
|------------------|------------------------------|
| **Version**      | 1.0                          |
| **Author**       | Corey Rollins               |
| **Last Updated** | May 21, 2025                |
| **Status**       | Draft                        |
| **Source**       | GitHub Repository (TBD)      |

---

## 1. Overview

The Reporting & Analytics API enables internal teams to track platform usage, monitor system activity, and export key metrics. Designed for seamless integration into dashboards and automation tools, it supports:

- Usage summaries by user, team, or service
- System log access for diagnostics and auditing
- Export jobs for historical or large-scale reporting
- Filtering, pagination, and scoped access

**Example Use Cases**:
- Visualize monthly active users in Power BI
- Alert IT on failed login attempts using log severity filters
- Automate monthly CSV reports for executive stakeholders

🔗 [Back to top](#-reporting--analytics-api)

---

## 2. Authentication

### 2.1 Supported Methods

- **OAuth 2.0 Bearer Token** – Required for user and application-level access  
- **API Key** – Reserved for backend service-to-service integrations

### 2.2 Token Issuance

Tokens are issued by the internal Identity Provider (IdP) or OAuth2-compliant authorization server. Tokens must include the `read:analytics` scope.

💡 **Tip**: Use short-lived access tokens with refresh tokens to maintain security.

### 2.3 Example Header

```http
Authorization: Bearer <access_token>
```

---

## 3. Endpoints

### 3.1 `GET /reports/usage-summary`

Returns usage statistics over a defined date range, optionally filtered by user or team.

**Query Parameters:**

| Parameter     | Type     | Required | Description                          |
|---------------|----------|----------|--------------------------------------|
| `start_date`  | `string` | Yes      | ISO 8601 format (e.g., `2025-01-01`) |
| `end_date`    | `string` | Yes      | ISO 8601 format                      |
| `user_id`     | `string` | No       | Filter by user ID                    |
| `team_id`     | `string` | No       | Filter by team ID                    |

**Example Request:**

```bash
curl -H "Authorization: Bearer <access_token>" \
  "https://api.example.com/reports/usage-summary?start_date=2025-01-01&end_date=2025-01-31"
```

**Example Response:**

```json
{
  "total_users": 148,         // Total registered users during the time range
  "active_users": 97,         // Users with at least one session/activity
  "total_sessions": 642       // All user sessions during the date range
}
```

___

### 3.2 `GET /logs/system-events`

Retrieves system logs in a paginated format. Useful for audits and troubleshooting.

**Query Parameters:**

| Parameter     | Type     | Required | Description                              |
|---------------|----------|----------|------------------------------------------|
| `limit`       | `int`    | No       | Items per page (default: 100)            |
| `page`        | `int`    | No       | Page number                              |
| `severity`    | `string` | No       | Filter by severity (`info`, `warning`, `error`) |

**Example Response:**

```json
{
  "events": [
    {
      "timestamp": "2025-05-21T10:15:30Z",  // UTC ISO timestamp of the event
      "severity": "warning",                // One of: info, warning, error
      "message": "Report export failed due to timeout" // Human-readable description
    }
  ],
  "page": 1,             // Current results page
  "total_pages": 12      // Total pages available
}
```

___

### 3.3 `POST /reports/export`

Initiates an asynchronous report export job.

**Request Body:**

```json
{
  "report_type": "usage-summary",
  "format": "csv",
  "start_date": "2025-01-01",
  "end_date": "2025-01-31"
}
```

**Example Response:**

```json
{
  "job_id": "abc123",    // Unique export job identifier
  "status": "queued"     // Initial status: queued, processing, completed, or failed
}
```

⚠️ **Warning**: Only users with `report_admin` privileges may run export jobs.

---

### 3.4 `GET /reports/export/:job_id`

Checks the status of an export job.

**Example Response:**

```json
{
  "job_id": "abc123",    // Identifier used to query export status
  "status": "completed", // Final job status
  "download_url": "https://api.example.com/downloads/export-abc123.csv" // File URL
}
```

Statuses: `queued`, `processing`, `completed`, `failed`

---

## 4. Error Handling

| Code | Message               | Description                       |
|------|------------------------|-----------------------------------|
| 401  | Unauthorized           | Token missing, expired, or invalid |
| 403  | Forbidden              | User lacks required permissions   |
| 404  | Not Found              | Resource does not exist           |
| 429  | Too Many Requests      | Rate limit exceeded               |
| 500  | Internal Server Error  | General system failure            |

**Example 403 Response:**

```json
{
  "error": "Forbidden",     // Standardized error name
  "message": "You do not have access to this resource." // User-facing message
}
```

💡 **Tip**: Handle 429 errors using exponential backoff and retry headers.

---

## 5. Rate Limits

| Limit Type         | Value             |
|--------------------|-------------------|
| API Requests/min   | 100 per token     |
| Export Jobs/day    | 10 per user       |

To request a rate limit increase, contact the API admin team.

---

## 6. Changelog

| Version | Date       | Changes                            |
|---------|------------|-------------------------------------|
| 1.0     | 2025-05-21 | Initial draft published             |
