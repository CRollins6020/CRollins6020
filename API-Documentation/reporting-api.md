# Reporting & Analytics API Reference

| **Metadata** | **Details** |
|--------------|-------------|
| **Author** | Corey Rollins |
| **Version** | 1.0.0 |
| **Last Updated** | May 23, 2025 |

## Table of Contents

1. [Overview](#1-overview)
2. [Authentication](#2-authentication)
3. [Rate Limiting](#3-rate-limiting)
4. [Reports Endpoints](#4-reports-endpoints)
5. [Analytics Endpoints](#5-analytics-endpoints)
6. [Dashboards Endpoints](#6-dashboards-endpoints)
7. [Data Export Endpoints](#7-data-export-endpoints)
8. [Error Handling](#8-error-handling)
9. [Troubleshooting](#9-troubleshooting)
10. [Advanced Integration Patterns](#10-advanced-integration-patterns)
11. [SDK and Examples](#11-sdk-and-examples)

---

## 1. Overview

The Reporting & Analytics API provides programmatic access to generate reports, retrieve analytics data, manage dashboards, and export data from your application. This RESTful API supports JSON request and response formats, enabling seamless integration with your existing systems and workflows.

**Base URL:** `https://api.yourplatform.com/v1`

**Supported Formats:** JSON only

**API Versioning:** All endpoints include version information in the URL path. The current stable version is `v1`.

The API is designed for high-volume enterprise use cases while maintaining simplicity for basic reporting needs. All endpoints support pagination, filtering, and sorting to help you retrieve exactly the data you need efficiently.

___

### Key Capabilities

This API enables you to programmatically access the same reporting and analytics features available in the web interface, including real-time metrics, historical trend analysis, custom report generation, and dashboard management. Whether you're building custom integrations, automating report delivery, or creating embedded analytics experiences, these endpoints provide the foundation for data-driven applications.

**[Back to top](#table-of-contents)**

---

## 2. Authentication

All API requests require authentication using API keys generated from your account settings.

### API Key Authentication

Include your API key in the request header:

```http
Authorization: Bearer YOUR_API_KEY
```

### Authentication Errors

| Status Code | Error Code | Description |
|-------------|------------|-------------|
| 401 | `invalid_api_key` | The provided API key is invalid or expired |
| 401 | `missing_authorization` | No authorization header provided |
| 403 | `insufficient_permissions` | API key lacks required permissions for this endpoint |

> ⚠️ **Security Note:** Never expose API keys in client-side code. Use environment variables or secure key management systems in production.

Authentication failures don't count toward rate limits, but repeated errors may trigger temporary access restrictions.

**[Back to top](#table-of-contents)**

---

## 3. Rate Limiting

The API implements rate limiting to ensure fair usage and system stability. Rate limits apply per API key and are tracked across rolling time windows.

### Rate Limit Headers

Every API response includes rate limiting information:

```http
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1621234567
```

### Rate Limit Tiers

| Endpoint Category | Requests per Hour | Burst Limit |
|------------------|-------------------|-------------|
| Reports | 500 | 10 per minute |
| Analytics | 1000 | 20 per minute |
| Dashboards | 200 | 5 per minute |
| Data Export | 50 | 2 per minute |

### Handling Rate Limits

When you exceed rate limits, the API returns a `429 Too Many Requests` status with a `Retry-After` header indicating when you can make your next request. Implement exponential backoff in your applications to handle rate limiting gracefully.

For high-volume use cases, contact support to discuss custom rate limit increases based on your specific requirements and usage patterns.

**[Back to top](#table-of-contents)**

---

## 4. Reports Endpoints

Reports endpoints allow you to create, retrieve, and manage custom reports. These endpoints support both predefined report templates and fully customizable report configurations.

___

### Create Report

**POST** `/reports`

Generate a new report with specified parameters and data sources.

#### Request Body

```json
{
  "name": "Q1 Sales Performance",
  "description": "Quarterly sales analysis with regional breakdown",
  "data_source": "sales_transactions",
  "date_range": {
    "start": "2025-01-01",
    "end": "2025-03-31"
  },
  "filters": {
    "region": ["north", "south"],
    "product_category": ["electronics", "software"]
  },
  "metrics": ["total_revenue", "transaction_count", "average_order_value"],
  "dimensions": ["region", "month"],
  "format": "json"
}
```

#### Response

```json
{
  "report_id": "rpt_1a2b3c4d5e6f",
  "status": "processing",
  "name": "Q1 Sales Performance",
  "created_at": "2025-05-23T10:30:00Z",
  "estimated_completion": "2025-05-23T10:35:00Z",
  "download_url": null
}
```

___

### Retrieve Report

**GET** `/reports/{report_id}`

Fetch details and results for a specific report.

#### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `report_id` | string | Unique identifier for the report |

#### Response

```json
{
  "report_id": "rpt_1a2b3c4d5e6f",
  "status": "completed",
  "name": "Q1 Sales Performance",
  "created_at": "2025-05-23T10:30:00Z",
  "completed_at": "2025-05-23T10:34:22Z",
  "download_url": "https://api.yourplatform.com/v1/reports/rpt_1a2b3c4d5e6f/download",
  "metadata": {
    "row_count": 1547,
    "file_size": "892KB",
    "columns": ["region", "month", "total_revenue", "transaction_count"]
  },
  "data": {
    "summary": {
      "total_revenue": 2847592.50,
      "total_transactions": 8432
    },
    "results": [
      {
        "region": "north",
        "month": "2025-01",
        "total_revenue": 245678.90,
        "transaction_count": 432
      }
    ]
  }
}
```

___

### List Reports

**GET** `/reports`

Retrieve a paginated list of reports with optional filtering.

#### Query Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `page` | integer | 1 | Page number for pagination |
| `limit` | integer | 20 | Number of reports per page (max 100) |
| `status` | string | all | Filter by status: `processing`, `completed`, `failed`, `all` |
| `created_after` | string | - | ISO 8601 date to filter reports created after |
| `data_source` | string | - | Filter by specific data source |

#### Response

```json
{
  "reports": [
    {
      "report_id": "rpt_1a2b3c4d5e6f",
      "name": "Q1 Sales Performance",
      "status": "completed",
      "created_at": "2025-05-23T10:30:00Z"
    }
  ],
  "pagination": {
    "current_page": 1,
    "total_pages": 15,
    "total_count": 287,
    "has_next": true
  }
}
```

**[Back to top](#table-of-contents)**

---

## 5. Analytics Endpoints

Analytics endpoints provide access to real-time and historical metrics, enabling you to build custom analytics experiences and integrate platform data into external systems.

___

### Get Metrics

**GET** `/analytics/metrics`

Retrieve specific metrics with time-based aggregation and filtering.

#### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `metrics` | array | Yes | Comma-separated list of metric names |
| `start_date` | string | Yes | Start date in ISO 8601 format |
| `end_date` | string | Yes | End date in ISO 8601 format |
| `granularity` | string | No | Time granularity: `hour`, `day`, `week`, `month` |
| `filters` | object | No | JSON object with filter conditions |

#### Example Request

```http
GET /analytics/metrics?metrics=page_views,unique_visitors&start_date=2025-05-01&end_date=2025-05-23&granularity=day&filters={"country":"US","device_type":"mobile"}
```

#### Response

```json
{
  "metrics": ["page_views", "unique_visitors"],
  "granularity": "day",
  "date_range": {
    "start": "2025-05-01",
    "end": "2025-05-23"
  },
  "data": [
    {
      "date": "2025-05-01",
      "page_views": 12457,
      "unique_visitors": 8932
    },
    {
      "date": "2025-05-02",
      "page_views": 13102,
      "unique_visitors": 9234
    }
  ],
  "summary": {
    "total_page_views": 287543,
    "average_daily_visitors": 9182
  }
}
```

___

### Get Real-time Metrics

**GET** `/analytics/realtime`

Access current real-time metrics for live monitoring and dashboards.

#### Query Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `metrics` | array | all | Specific metrics to retrieve |
| `refresh_interval` | integer | 30 | Data refresh interval in seconds |

#### Response

```json
{
  "timestamp": "2025-05-23T14:30:45Z",
  "refresh_interval": 30,
  "metrics": {
    "active_users": 1247,
    "current_page_views": 89,
    "bounce_rate": 0.34,
    "average_session_duration": 245.7
  },
  "trending": {
    "most_visited_pages": [
      "/dashboard",
      "/reports",
      "/analytics"
    ],
    "top_traffic_sources": [
      "direct",
      "google",
      "social_media"
    ]
  }
}
```

___

### Get Custom Analytics

**POST** `/analytics/custom`

Execute custom analytics queries with advanced filtering, nested aggregations, and complex calculations.

#### Request Body

```json
{
  "query": {
    "data_source": "user_events",
    "metrics": [
      {
        "name": "conversion_rate",
        "calculation": "count(completed_purchases) / count(unique_sessions)"
      },
      {
        "name": "revenue_per_visitor",
        "calculation": "sum(revenue) / count(distinct(user_id))"
      }
    ],
    "dimensions": ["traffic_source", "device_type", "geo_country"],
    "filters": [
      {
        "field": "event_date",
        "operator": "between",
        "value": ["2025-05-01", "2025-05-23"]
      },
      {
        "field": "user_type",
        "operator": "in",
        "value": ["premium", "enterprise"]
      },
      {
        "logical_operator": "OR",
        "conditions": [
          {"field": "revenue", "operator": "gte", "value": 100},
          {"field": "session_duration", "operator": "gte", "value": 300}
        ]
      }
    ],
    "having": [
      {
        "field": "conversion_rate",
        "operator": "gte", 
        "value": 0.05
      }
    ],
    "sort": [
      {"field": "revenue_per_visitor", "direction": "desc"},
      {"field": "conversion_rate", "direction": "desc"}
    ],
    "limit": 100
  }
}
```

#### Response

```json
{
  "query_id": "qry_abc123def456",
  "execution_time_ms": 1247,
  "total_rows": 847,
  "data": [
    {
      "traffic_source": "google",
      "device_type": "mobile",
      "geo_country": "US",
      "conversion_rate": 0.127,
      "revenue_per_visitor": 47.32,
      "sample_size": 15847
    }
  ],
  "metadata": {
    "cache_hit": false,
    "data_freshness": "2025-05-23T14:30:00Z",
    "sampling_rate": 1.0
  }
}
```

**[Back to top](#table-of-contents)**

---

## 6. Dashboards Endpoints

Dashboard endpoints enable programmatic management of dashboard configurations, widget layouts, and sharing permissions for embedded analytics scenarios.

___

### Create Dashboard

**POST** `/dashboards`

Create a new dashboard with specified widgets and layout configuration.

#### Request Body

```json
{
  "name": "Executive Summary",
  "description": "High-level KPIs and trends",
  "layout": "grid",
  "widgets": [
    {
      "type": "metric_card",
      "title": "Total Revenue",
      "data_source": "sales_transactions",
      "metric": "total_revenue",
      "time_range": "last_30_days",
      "position": {"row": 1, "col": 1, "width": 2, "height": 1}
    },
    {
      "type": "line_chart",
      "title": "Revenue Trend",
      "data_source": "sales_transactions",
      "metrics": ["daily_revenue"],
      "time_range": "last_90_days",
      "position": {"row": 2, "col": 1, "width": 4, "height": 2}
    }
  ],
  "sharing": {
    "public": false,
    "password_protected": true,
    "allowed_domains": ["yourcompany.com"]
  }
}
```

#### Response

```json
{
  "dashboard_id": "dash_9x8y7z6w5v4u",
  "name": "Executive Summary",
  "created_at": "2025-05-23T14:45:00Z",
  "share_url": "https://dashboard.yourplatform.com/shared/dash_9x8y7z6w5v4u",
  "embed_code": "<iframe src=\"https://dashboard.yourplatform.com/embed/dash_9x8y7z6w5v4u\" width=\"100%\" height=\"600\"></iframe>"
}
```

___

### Update Dashboard

**PUT** `/dashboards/{dashboard_id}`

Update an existing dashboard's configuration, including widgets, layout, and sharing settings.

#### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `dashboard_id` | string | Unique identifier for the dashboard |

#### Request Body

The request body follows the same structure as the create dashboard endpoint, with all fields optional for partial updates.

___

### Get Dashboard Data

**GET** `/dashboards/{dashboard_id}/data`

Retrieve current data for all widgets in a dashboard, optimized for display rendering.

#### Query Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `refresh` | boolean | false | Force refresh of cached data |
| `time_zone` | string | UTC | Time zone for date calculations |

#### Response

```json
{
  "dashboard_id": "dash_9x8y7z6w5v4u",
  "last_updated": "2025-05-23T14:50:30Z",
  "widgets": [
    {
      "widget_id": "widget_1",
      "type": "metric_card",
      "title": "Total Revenue",
      "data": {
        "current_value": 284759.25,
        "previous_value": 267432.10,
        "change_percent": 6.47,
        "trend": "up"
      }
    },
    {
      "widget_id": "widget_2",
      "type": "line_chart",
      "title": "Revenue Trend",
      "data": {
        "series": [
          {
            "name": "Daily Revenue",
            "data": [
              {"date": "2025-05-01", "value": 8457.30},
              {"date": "2025-05-02", "value": 9234.50}
            ]
          }
        ]
      }
    }
  ]
}
```

**[Back to top](#table-of-contents)**

---

## 7. Data Export Endpoints

Data export endpoints facilitate bulk data extraction in various formats for external analysis, backup purposes, and integration with third-party tools.

___

### Request Data Export

**POST** `/exports`

Initiate a data export job for large datasets with format and compression options.

#### Request Body

```json
{
  "export_type": "full_data",
  "data_sources": ["user_events", "sales_transactions"],
  "date_range": {
    "start": "2025-01-01",
    "end": "2025-05-23"
  },
  "format": "csv",
  "compression": "gzip",
  "filters": {
    "exclude_test_data": true,
    "include_deleted": false
  },
  "columns": ["timestamp", "user_id", "event_type", "revenue"],
  "callback_url": "https://yourapp.com/webhooks/export-complete"
}
```

#### Response

```json
{
  "export_id": "exp_a1b2c3d4e5f6",
  "status": "queued",
  "estimated_size": "2.4GB",
  "estimated_duration": "15-20 minutes",
  "created_at": "2025-05-23T15:00:00Z"
}
```

### Webhook Handling

When providing a `callback_url`, the API sends POST requests on status changes:

```json
{
  "export_id": "exp_a1b2c3d4e5f6",
  "status": "completed",
  "timestamp": "2025-05-23T15:18:32Z",
  "download_urls": ["https://exports.yourplatform.com/..."]
}
```

**Webhook Retry Policy:** Failed webhooks are retried up to 5 times with exponential backoff. Implement idempotent handlers and return 2xx status codes for successful processing.

### Bulk Export Limitations

| Export Type | Max Records | Max Duration | File Split Threshold |
|-------------|-------------|--------------|---------------------|
| Standard | 10M records | 2 hours | 1GB per file |
| Premium | 100M records | 8 hours | 5GB per file |
| Enterprise | Unlimited | 24 hours | 10GB per file |

Large exports automatically split into multiple files. Use the `Content-Range` header pattern for resumable downloads on connection failures.
```

___

### Get Export Status

**GET** `/exports/{export_id}`

Check the status and progress of a data export job.

#### Response

```json
{
  "export_id": "exp_a1b2c3d4e5f6",
  "status": "completed",
  "progress": 100,
  "created_at": "2025-05-23T15:00:00Z",
  "completed_at": "2025-05-23T15:18:32Z",
  "file_info": {
    "filename": "export_20250523_150000.csv.gz",
    "size": "2.1GB",
    "row_count": 5847293,
    "checksum": "sha256:a1b2c3d4..."
  },
  "download_urls": [
    {
      "url": "https://exports.yourplatform.com/exp_a1b2c3d4e5f6/part_001.csv.gz",
      "expires_at": "2025-05-30T15:18:32Z"
    }
  ]
}
```

> 📋 **Note:** Large exports are automatically split into multiple files for easier handling. Download URLs expire after 7 days for security purposes.

**[Back to top](#table-of-contents)**

---

## 8. Error Handling

The API uses standard HTTP status codes and provides detailed error information in JSON format to help you troubleshoot integration issues quickly.

### Error Response Format

```json
{
  "error": {
    "code": "validation_failed",
    "message": "The request contains invalid parameters",
    "details": [
      {
        "field": "date_range.start",
        "issue": "Date must be in ISO 8601 format (YYYY-MM-DD)"
      }
    ],
    "request_id": "req_1234567890abcdef"
  }
}
```

### Common Error Codes

| Status Code | Error Code | Description | Resolution |
|-------------|------------|-------------|------------|
| 400 | `validation_failed` | Request parameters are invalid | Check the `details` array for specific field errors |
| 401 | `invalid_api_key` | Authentication failed | Verify your API key is correct and active |
| 403 | `insufficient_permissions` | Access denied for this resource | Contact support to review API key permissions |
| 404 | `resource_not_found` | Requested resource doesn't exist | Verify the resource ID and your access permissions |
| 429 | `rate_limit_exceeded` | Too many requests | Implement exponential backoff and retry logic |
| 500 | `internal_error` | Server-side error occurred | Retry the request or contact support if persistent |
| 503 | `service_unavailable` | Temporary service disruption | Check status page and retry with exponential backoff |

### Error Handling Best Practices

Implement robust error handling in your applications by checking HTTP status codes, parsing error responses for specific details, and implementing appropriate retry logic for transient errors. Always log the `request_id` from error responses when contacting support for faster issue resolution.

For validation errors, the `details` array provides field-specific information to help users correct their input. Rate limiting errors include a `Retry-After` header indicating when the next request can be made.

**[Back to top](#table-of-contents)**

---

## 9. Troubleshooting

Common integration issues and their solutions to help you resolve problems quickly.

### Report Generation Issues

**Problem:** Reports fail with `data_too_large` error
**Solution:** Reduce date range or add filters to limit data volume. Consider using pagination or data exports for large datasets.

**Problem:** Report shows "No data found" despite existing records
**Solution:** Verify filter syntax and date range format. Check data source permissions and ensure the API key has access to the specified data source.

### Analytics Query Problems

**Problem:** Custom analytics queries timeout
**Solution:** Optimize queries by adding indexes on filter fields, reducing date ranges, or using pre-aggregated data sources. Complex calculations should use the `/analytics/custom` endpoint instead of real-time metrics.

**Problem:** Inconsistent metric values between API and dashboard
**Solution:** Check time zone settings and ensure both use the same calculation window. API responses use UTC by default; specify `time_zone` parameter to match dashboard settings.

### Dashboard and Export Failures

**Problem:** Dashboard widgets show "Loading" indefinitely
**Solution:** Verify widget data sources exist and API key permissions. Check for circular dependencies in calculated metrics.

**Problem:** Export webhooks not received
**Solution:** Ensure webhook URL accepts POST requests and returns 2xx status codes. Check firewall settings and implement retry logic for failed deliveries.

**Problem:** Large exports fail or produce corrupt files
**Solution:** Use compression to reduce file size, implement resumable download logic, and verify checksums. Consider splitting requests by date range for datasets exceeding 10GB.

### Performance Optimization

**Problem:** API responses are slow
**Solution:** Implement caching for frequently accessed data, use appropriate pagination limits, and leverage ETags for conditional requests. Consider using real-time endpoints only when necessary.

**[Back to top](#table-of-contents)**

---

## 10. Advanced Integration Patterns

While the API can be consumed directly via HTTP requests, official SDKs provide convenient wrapper methods and handle authentication, rate limiting, and error handling automatically.

### Available SDKs

| Language | Repository | Documentation |
|----------|------------|---------------|
| Python | github.com/yourplatform/python-sdk | docs.yourplatform.com/sdk/python |
| JavaScript/Node.js | github.com/yourplatform/js-sdk | docs.yourplatform.com/sdk/javascript |
| PHP | github.com/yourplatform/php-sdk | docs.yourplatform.com/sdk/php |
| Ruby | github.com/yourplatform/ruby-sdk | docs.yourplatform.com/sdk/ruby |

### cURL Examples

```bash
# Create a report
curl -X POST https://api.yourplatform.com/v1/reports \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Sales Report",
    "data_source": "sales_transactions",
    "date_range": {"start": "2025-05-01", "end": "2025-05-31"},
    "metrics": ["total_revenue", "transaction_count"]
  }'

# Get real-time metrics
curl -X GET "https://api.yourplatform.com/v1/analytics/realtime?metrics=active_users,page_views" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### Python Example

```python
from yourplatform import Client

client = Client(api_key='your_api_key_here')

# Create a report with complex filtering
report = client.reports.create(
    name='Monthly Sales Report',
    data_source='sales_transactions',
    date_range={'start': '2025-05-01', 'end': '2025-05-31'},
    metrics=['total_revenue', 'transaction_count'],
    filters={'region': ['north', 'south'], 'product_type': 'premium'}
)

# Get real-time metrics with error handling
try:
    metrics = client.analytics.get_realtime(['active_users', 'page_views'])
    print(f"Active users: {metrics['active_users']}")
except RateLimitError as e:
    print(f"Rate limited. Retry after: {e.retry_after} seconds")
```

### JavaScript Example

```javascript
const { YourPlatformClient } = require('@yourplatform/sdk');

const client = new YourPlatformClient('your_api_key_here');

// Create and manage a dashboard with error handling
async function createDashboard() {
  try {
    const dashboard = await client.dashboards.create({
      name: 'Sales Dashboard',
      widgets: [
        {
          type: 'metric_card',
          title: 'Total Revenue',
          metric: 'total_revenue',
          time_range: 'last_30_days'
        }
      ]
    });
    
    return dashboard;
  } catch (error) {
    if (error.code === 'rate_limit_exceeded') {
      await new Promise(resolve => setTimeout(resolve, error.retryAfter * 1000));
      return createDashboard(); // Retry
    }
    throw error;
  }
}
```

### PHP Example

```php
<?php
use YourPlatform\Client;

$client = new Client('your_api_key_here');

// Execute custom analytics query
try {
    $result = $client->analytics->custom([
        'query' => [
            'data_source' => 'user_events',
            'metrics' => [
                [
                    'name' => 'conversion_rate',
                    'calculation' => 'count(completed_purchases) / count(unique_sessions)'
                ]
            ],
            'dimensions' => ['traffic_source', 'device_type'],
            'filters' => [
                [
                    'field' => 'event_date',
                    'operator' => 'between',
                    'value' => ['2025-05-01', '2025-05-23']
                ]
            ]
        ]
    ]);
    
    echo "Conversion rate: " . $result['data'][0]['conversion_rate'];
} catch (YourPlatform\RateLimitException $e) {
    sleep($e->getRetryAfter());
    // Retry logic here
}
?>
```

### Ruby Example

```ruby
require 'yourplatform'

client = YourPlatform::Client.new(api_key: 'your_api_key_here')

# Create export with webhook notification
begin
  export = client.exports.create(
    export_type: 'full_data',
    data_sources: ['user_events', 'sales_transactions'],
    date_range: {
      start: '2025-01-01',
      end: '2025-05-23'
    },
    format: 'csv',
    compression: 'gzip',
    callback_url: 'https://yourapp.com/webhooks/export-complete'
  )
  
  puts "Export started: #{export.export_id}"
rescue YourPlatform::RateLimitError => e
  sleep(e.retry_after)
  retry
end
```

### Common Integration Patterns

Most applications follow these patterns: scheduled report generation with webhook notifications, real-time dashboard updates using WebSocket connections, data synchronization through periodic exports, and embedded analytics using iframe integration.

For high-frequency integrations, implement local caching and leverage ETags for efficient cache validation. The API supports conditional requests to minimize unnecessary data transfer.

**[Back to top](#table-of-contents)**

---

## Next Steps

Generate an API key from your dashboard, choose the appropriate SDK, and start with simple metric retrieval before building complex reporting workflows. Consult SDK documentation for language-specific examples and the webhook guide for asynchronous processing patterns.

Enterprise customers receive dedicated technical support for large-scale integrations.