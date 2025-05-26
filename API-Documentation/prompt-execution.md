# Prompt Execution API Documentation

**API Metadata Header**
- **Version:** v2.1.0
- **Base URL:** `https://api.promptexec.ai/v2`
- **Authentication:** Bearer Token (API Key)
- **Last Updated:** May 26, 2025
- **OpenAPI Specification:** [Download Schema](https://api.promptexec.ai/v2/openapi.json)
- **Status:** Production Ready
- **Target Audience:** Developers with intermediate API integration experience
- **Support Contact:** support@promptexec.ai

## Table of Contents

1. [Overview](#1-overview)
2. [Authentication](#2-authentication)
3. [Common use cases](#3-common-use-cases)
4. [Execution endpoints](#4-execution-endpoints)
5. [Error handling](#5-error-handling)
6. [Rate limiting](#6-rate-limiting)
7. [Advanced prompt engineering](#7-advanced-prompt-engineering)
8. [Performance and scaling](#8-performance-and-scaling)

---

## 1. Overview

The Prompt Execution API enables developers to execute AI prompts with advanced configuration options, real-time streaming, and comprehensive result analysis. This API supports multiple AI models, custom prompt templates, and enterprise-grade security features.

**Key capabilities:**
- Execute prompts across multiple AI models (GPT-4, Claude, Gemini)
- Real-time streaming responses with WebSocket support
- Template management and version control
- Advanced prompt engineering with chain-of-thought reasoning
- Comprehensive analytics and performance monitoring
- Enterprise security with audit logging

**Base URL:** `https://api.promptexec.ai/v2`

**Supported models:**
- OpenAI GPT-4 and GPT-3.5
- Anthropic Claude Sonnet and Opus
- Google Gemini Pro and Ultra
- Custom fine-tuned models

---

## 2. Authentication

All API requests require authentication using Bearer tokens. Obtain your API key from the developer dashboard.

**Authentication flow:**

1. Register for an account at [developer.promptexec.ai](https://developer.promptexec.ai)
2. Generate an API key in your dashboard
3. Include the API key in the Authorization header for all requests

**Request headers:**
```http
Authorization: Bearer sk_live_abc123def456ghi789
Content-Type: application/json
```

<div class="warning">⚠️ <strong>Security Note:</strong> Never include API keys in client-side code or public repositories. Store keys securely in environment variables.</div>

**API key scopes:**
- `prompt.execute` - Execute prompts and access results
- `template.manage` - Create and modify prompt templates
- `analytics.read` - Access execution analytics and logs
- `admin.manage` - Full administrative access

**Testing authentication:**
```bash
curl -X GET "https://api.promptexec.ai/v2/auth/verify" \
  -H "Authorization: Bearer your_api_key"
```

**Expected response:**
```json
{
  "authenticated": true,
  "user_id": "user_12345",
  "scopes": ["prompt.execute", "template.manage"],
  "rate_limit": {
    "requests_per_hour": 1000,
    "current_usage": 47
  }
}
```

---

## 3. Common use cases

### Use case 1: Simple text generation

Execute a basic prompt for content generation tasks like writing assistance, summarization, or creative writing.

**Example: Blog post generation**
```javascript
const response = await fetch('https://api.promptexec.ai/v2/execute', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer sk_live_abc123def456ghi789',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    prompt: "Write a 300-word blog post about sustainable technology trends in 2025",
    model: "gpt-4",
    max_tokens: 500,
    temperature: 0.7
  })
});

const result = await response.json();
console.log(result.output.text);
```

### Use case 2: Structured data extraction

Extract structured information from unstructured text using prompt engineering techniques.

**Example: Contact information extraction**
```python
import requests

payload = {
    "prompt": "Extract contact information from this text and return as JSON: 'Contact John Smith at john@example.com or call (555) 123-4567 for more details about our software solutions.'",
    "model": "claude-sonnet",
    "response_format": "json",
    "schema": {
        "type": "object",
        "properties": {
            "name": {"type": "string"},
            "email": {"type": "string"},
            "phone": {"type": "string"},
            "subject": {"type": "string"}
        }
    }
}

response = requests.post(
    'https://api.promptexec.ai/v2/execute',
    json=payload,
    headers={'Authorization': 'Bearer sk_live_abc123def456ghi789'}
)

data = response.json()
print(data['output']['structured_data'])
```

### Use case 3: Real-time streaming responses

Stream responses in real-time for interactive applications like chatbots or live content generation.

**Example: Interactive chat with streaming**
```javascript
const ws = new WebSocket('wss://api.promptexec.ai/v2/stream');

ws.onopen = () => {
  ws.send(JSON.stringify({
    auth_token: 'sk_live_abc123def456ghi789',
    prompt: "Explain quantum computing in simple terms",
    model: "gpt-4",
    stream: true,
    temperature: 0.3
  }));
};

ws.onmessage = (event) => {
  const data = JSON.parse(event.data);
  if (data.type === 'content') {
    // Append streaming content to UI
    document.getElementById('response').textContent += data.delta;
  } else if (data.type === 'complete') {
    console.log('Execution completed:', data.metadata);
  }
};
```

---

## 4. Execution endpoints

### 4.1 Execute prompt

Execute a single prompt with specified configuration and receive the AI-generated response.

**Endpoint:** `POST /execute`

**Purpose:** Execute a prompt against a specified AI model with custom parameters and formatting options.

**Parameters:**

| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `prompt` | string | ✅ Required | The text prompt to execute | `"Summarize this article: [content]"` |
| `model` | string | ✅ Required | AI model identifier | `"gpt-4"`, `"claude-sonnet"` |
| `max_tokens` | integer | ⚠️ Optional | Maximum response length (1-4000) | `500` |
| `temperature` | float | ⚠️ Optional | Creativity level (0.0-2.0) | `0.7` |
| `response_format` | string | ⚠️ Optional | Output format type | `"text"`, `"json"`, `"markdown"` |
| `schema` | object | ⚠️ Optional | JSON schema for structured output | `{"type": "object", "properties": {...}}` |
| `system_prompt` | string | ⚠️ Optional | System-level instructions | `"You are a helpful assistant"` |
| `tools` | array | ⚠️ Optional | Available function tools | `[{"name": "search", "description": "..."}]` |

**Request example:**
```bash
curl -X POST "https://api.promptexec.ai/v2/execute" \
  -H "Authorization: Bearer sk_live_abc123def456ghi789" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "Generate 3 marketing headlines for a sustainable fashion brand",
    "model": "gpt-4",
    "max_tokens": 200,
    "temperature": 0.8,
    "response_format": "json",
    "schema": {
      "type": "object",
      "properties": {
        "headlines": {
          "type": "array",
          "items": {"type": "string"}
        }
      }
    }
  }'
```

**Response example:**
```json
{
  "execution_id": "exec_789xyz123abc",
  "status": "completed",
  "output": {
    "text": "{\n  \"headlines\": [\n    \"Eco-Chic: Fashion That Loves the Planet\",\n    \"Sustainable Style, Endless Possibilities\",\n    \"Green Threads for a Better Tomorrow\"\n  ]\n}",
    "structured_data": {
      "headlines": [
        "Eco-Chic: Fashion That Loves the Planet",
        "Sustainable Style, Endless Possibilities", 
        "Green Threads for a Better Tomorrow"
      ]
    }
  },
  "metadata": {
    "model_used": "gpt-4",
    "tokens_consumed": 156,
    "execution_time_ms": 2340,
    "cost_usd": 0.0234
  },
  "created_at": "2025-05-25T14:30:45Z"
}
```

**Error responses:**
- **400 Bad Request:** Invalid parameters or malformed request
- **401 Unauthorized:** Invalid or missing API key
- **403 Forbidden:** Insufficient permissions for requested model
- **429 Too Many Requests:** Rate limit exceeded
- **500 Internal Server Error:** Temporary service issue

**Rate limiting:** Standard rate limits apply (see section 6)

### 4.2 Execute template

Execute a predefined prompt template with variable substitution and consistent formatting.

**Endpoint:** `POST /execute/template/{template_id}`

**Purpose:** Execute a saved prompt template with dynamic variable substitution for consistent, reusable prompt patterns.

**Path parameters:**

| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `template_id` | string | ✅ Required | Unique template identifier | `"tpl_blog_writer_v2"` |

**Body parameters:**

| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `variables` | object | ✅ Required | Template variable values | `{"topic": "AI ethics", "length": "500 words"}` |
| `model` | string | ⚠️ Optional | Override default template model | `"claude-opus"` |
| `temperature` | float | ⚠️ Optional | Override default temperature | `0.6` |

**Request example:**
```javascript
const templateExecution = await fetch('https://api.promptexec.ai/v2/execute/template/tpl_blog_writer_v2', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer sk_live_abc123def456ghi789',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    variables: {
      topic: "The future of renewable energy",
      target_audience: "technology professionals",
      word_count: "800",
      tone: "informative and optimistic"
    },
    model: "gpt-4"
  })
});
```

**Response example:**
```json
{
  "execution_id": "exec_456def789ghi",
  "template_info": {
    "id": "tpl_blog_writer_v2",
    "name": "Professional Blog Post Writer",
    "version": "2.1",
    "category": "content_generation"
  },
  "status": "completed",
  "output": {
    "text": "# The Future of Renewable Energy\n\nThe renewable energy sector is experiencing unprecedented growth...",
    "word_count": 847
  },
  "metadata": {
    "model_used": "gpt-4",
    "tokens_consumed": 1124,
    "execution_time_ms": 3567,
    "template_variables_used": 4
  }
}
```

### 4.3 Batch execute

Execute multiple prompts in a single API call for efficient processing of large datasets.

**Endpoint:** `POST /execute/batch`

**Purpose:** Process multiple prompts simultaneously with optimized resource allocation and consolidated results.

**Parameters:**

| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `prompts` | array | ✅ Required | Array of prompt objects | `[{"prompt": "...", "id": "1"}, ...]` |
| `model` | string | ✅ Required | Model for all prompts | `"gpt-4"` |
| `batch_name` | string | ⚠️ Optional | Identifier for this batch | `"product_descriptions_batch_1"` |
| `parallel` | boolean | ⚠️ Optional | Process prompts in parallel | `true` |
| `callback_url` | string | ⚠️ Optional | Webhook for completion notification | `"https://yourapp.com/webhook"` |

<details>
<summary>Complete batch request example</summary>

```python
import requests

batch_request = {
    "prompts": [
        {
            "id": "product_1",
            "prompt": "Write a product description for wireless headphones with noise cancellation",
            "max_tokens": 150
        },
        {
            "id": "product_2", 
            "prompt": "Write a product description for a smart fitness tracker",
            "max_tokens": 150
        },
        {
            "id": "product_3",
            "prompt": "Write a product description for an eco-friendly water bottle",
            "max_tokens": 150
        }
    ],
    "model": "gpt-4",
    "batch_name": "ecommerce_descriptions_may2025",
    "parallel": True,
    "temperature": 0.7
}

response = requests.post(
    'https://api.promptexec.ai/v2/execute/batch',
    json=batch_request,
    headers={'Authorization': 'Bearer sk_live_abc123def456ghi789'}
)
```
</details>

### 4.4 Stream execute

Execute a prompt with real-time streaming response for interactive applications.

**Endpoint:** `WebSocket /stream`

**Purpose:** Establish a WebSocket connection for real-time prompt execution with incremental response delivery.

**Connection parameters:**

| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `auth_token` | string | ✅ Required | API authentication token | `"sk_live_abc123def456ghi789"` |
| `prompt` | string | ✅ Required | Prompt to execute | `"Explain machine learning concepts"` |
| `model` | string | ✅ Required | AI model identifier | `"claude-sonnet"` |
| `stream` | boolean | ✅ Required | Enable streaming mode | `true` |

**WebSocket message types:**

- `content` - Incremental response content
- `metadata` - Execution information and statistics  
- `error` - Error information
- `complete` - Execution completion signal

**WebSocket example:**
```javascript
const streamingExecution = () => {
  const ws = new WebSocket('wss://api.promptexec.ai/v2/stream');
  
  ws.onopen = () => {
    ws.send(JSON.stringify({
      auth_token: 'sk_live_abc123def456ghi789',
      prompt: 'Write a step-by-step tutorial for setting up a React development environment',
      model: 'gpt-4',
      stream: true,
      max_tokens: 1000,
      temperature: 0.4
    }));
  };
  
  ws.onmessage = (event) => {
    const data = JSON.parse(event.data);
    
    switch(data.type) {
      case 'content':
        // Append streaming content
        document.getElementById('output').textContent += data.delta;
        break;
      case 'metadata':
        console.log('Execution metadata:', data.info);
        break;
      case 'complete':
        console.log('Stream completed:', data.final_metadata);
        ws.close();
        break;
      case 'error':
        console.error('Stream error:', data.message);
        break;
    }
  };
};
```

---

## 5. Error handling

The Prompt Execution API uses standard HTTP status codes and provides detailed error information to help developers diagnose and resolve issues quickly.

**Standard error response format:**
```json
{
  "error": {
    "code": "INVALID_MODEL",
    "message": "The specified model 'gpt-5' is not available",
    "details": {
      "available_models": ["gpt-4", "gpt-3.5-turbo", "claude-sonnet", "claude-opus"],
      "suggestion": "Use one of the available models listed above"
    },
    "request_id": "req_abc123def456",
    "timestamp": "2025-05-25T14:30:45Z"
  }
}
```

**Common error codes:**

**400 Bad Request errors:**
- `INVALID_PROMPT` - Prompt is empty, too long, or contains prohibited content
- `INVALID_MODEL` - Specified model is not available or not supported
- `INVALID_PARAMETERS` - Request parameters are malformed or out of range
- `SCHEMA_VALIDATION_FAILED` - Provided JSON schema is invalid or incompatible

**401 Unauthorized errors:**
- `INVALID_API_KEY` - API key is malformed, expired, or invalid
- `MISSING_AUTHORIZATION` - Authorization header is missing from request

**403 Forbidden errors:**
- `INSUFFICIENT_PERMISSIONS` - API key lacks required scopes for this operation
- `MODEL_ACCESS_DENIED` - Account does not have access to the requested model
- `CONTENT_POLICY_VIOLATION` - Prompt violates content usage policies

**429 Rate limit errors:**
- `RATE_LIMIT_EXCEEDED` - Too many requests within the rate limit window
- `QUOTA_EXCEEDED` - Monthly usage quota has been reached
- `CONCURRENT_LIMIT_EXCEEDED` - Too many simultaneous requests

**500 Internal server errors:**
- `MODEL_UNAVAILABLE` - AI model is temporarily unavailable
- `EXECUTION_TIMEOUT` - Prompt execution exceeded maximum time limit
- `INTERNAL_ERROR` - Unexpected server error occurred

**Error handling best practices:**

```javascript
async function executePromptWithErrorHandling(prompt, model) {
  try {
    const response = await fetch('https://api.promptexec.ai/v2/execute', {
      method: 'POST',
      headers: {
        'Authorization': 'Bearer sk_live_abc123def456ghi789',
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ prompt, model })
    });
    
    if (!response.ok) {
      const errorData = await response.json();
      
      switch (response.status) {
        case 400:
          console.error('Request error:', errorData.error.message);
          // Handle validation errors
          break;
        case 401:
          console.error('Authentication failed:', errorData.error.message);
          // Redirect to login or refresh API key
          break;
        case 429:
          console.error('Rate limit exceeded:', errorData.error.message);
          // Implement exponential backoff retry
          break;
        case 500:
          console.error('Server error:', errorData.error.message);
          // Retry with exponential backoff
          break;
        default:
          console.error('Unexpected error:', errorData.error.message);
      }
      
      throw new Error(`API Error: ${errorData.error.code}`);
    }
    
    return await response.json();
    
  } catch (error) {
    console.error('Network or parsing error:', error.message);
    throw error;
  }
}
```

**Retry logic implementation:**
```javascript
async function executeWithRetry(prompt, model, maxRetries = 3) {
  let retries = 0;
  
  while (retries < maxRetries) {
    try {
      return await executePromptWithErrorHandling(prompt, model);
    } catch (error) {
      retries++;
      
      if (retries === maxRetries) {
        throw error;
      }
      
      // Exponential backoff: 1s, 2s, 4s
      const delay = Math.pow(2, retries) * 1000;
      await new Promise(resolve => setTimeout(resolve, delay));
    }
  }
}
```

---

## 6. Rate limiting

The API implements comprehensive rate limiting to ensure fair usage and optimal performance for all users.

**Rate limit tiers:**

| Plan | Requests/Hour | Concurrent Requests | Monthly Tokens |
|------|---------------|-------------------|----------------|
| Free | 100 | 2 | 10,000 |
| Developer | 1,000 | 5 | 100,000 |
| Professional | 10,000 | 20 | 1,000,000 |
| Enterprise | 100,000 | 100 | 10,000,000 |

**Rate limit headers:**

Every API response includes rate limiting information in the headers:

```http
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 847
X-RateLimit-Reset: 1684509600
X-RateLimit-Window: 3600
```

**Rate limit monitoring:**
```javascript
function checkRateLimit(response) {
  const limit = response.headers.get('X-RateLimit-Limit');
  const remaining = response.headers.get('X-RateLimit-Remaining');
  const reset = response.headers.get('X-RateLimit-Reset');
  
  console.log(`Rate limit: ${remaining}/${limit} remaining`);
  
  if (remaining < 10) {
    const resetTime = new Date(reset * 1000);
    console.warn(`Approaching rate limit. Resets at: ${resetTime}`);
  }
}
```

**Rate limit optimization strategies:**

**Batch processing:**
Use the batch execution endpoint to process multiple prompts efficiently:
```javascript
// Instead of multiple individual requests
const results = [];
for (const prompt of prompts) {
  results.push(await executePrompt(prompt)); // 10 API calls
}

// Use batch processing
const batchResult = await executeBatch(prompts); // 1 API call
```

**Template optimization:**
Reuse prompt templates to reduce processing overhead:
```javascript
// Create reusable templates for common patterns
const templateId = await createTemplate({
  name: "Product Description Generator",
  prompt: "Write a compelling product description for {{product_name}} targeting {{audience}}"
});

// Execute template multiple times efficiently
const descriptions = await Promise.all([
  executeTemplate(templateId, {product_name: "Laptop", audience: "professionals"}),
  executeTemplate(templateId, {product_name: "Tablet", audience: "students"})
]);
```

**Exponential backoff for rate limit handling:**
```javascript
async function executeWithBackoff(apiCall, maxRetries = 5) {
  let retries = 0;
  
  while (retries < maxRetries) {
    try {
      return await apiCall();
    } catch (error) {
      if (error.status === 429) {
        retries++;
        const delay = Math.min(1000 * Math.pow(2, retries), 30000);
        console.log(`Rate limited. Retrying in ${delay}ms...`);
        await new Promise(resolve => setTimeout(resolve, delay));
      } else {
        throw error;
      }
    }
  }
  
  throw new Error('Max retries exceeded for rate limited request');
}
```

---

## 7. Advanced prompt engineering

The API provides sophisticated prompt engineering capabilities for complex AI workflows and enhanced output quality.

### Chain-of-thought reasoning

Enable step-by-step reasoning for complex problem-solving tasks:

```javascript
const chainOfThoughtPrompt = {
  prompt: "Solve this math problem step by step: If a train travels 120 miles in 2 hours, and then travels 180 miles in 3 hours, what is the average speed for the entire journey?",
  model: "gpt-4",
  system_prompt: "You are a math tutor. Always show your work step by step and explain your reasoning clearly.",
  temperature: 0.1,
  response_format: "json",
  schema: {
    type: "object",
    properties: {
      steps: {
        type: "array",
        items: {
          type: "object",
          properties: {
            step_number: { type: "integer" },
            description: { type: "string" },
            calculation: { type: "string" },
            result: { type: "string" }
          }
        }
      },
      final_answer: { type: "string" }
    }
  }
};
```

### Function calling and tool integration

Execute prompts with access to external tools and functions:

```javascript
const functionCallingPrompt = {
  prompt: "What's the current weather in San Francisco and how does it compare to the historical average for this time of year?",
  model: "gpt-4",
  tools: [
    {
      name: "get_current_weather",
      description: "Get current weather for a specified location",
      parameters: {
        type: "object",
        properties: {
          location: { type: "string", description: "City and state" },
          unit: { type: "string", enum: ["celsius", "fahrenheit"] }
        },
        required: ["location"]
      }
    },
    {
      name: "get_historical_weather",
      description: "Get historical weather averages",
      parameters: {
        type: "object", 
        properties: {
          location: { type: "string" },
          month: { type: "integer" },
          metric: { type: "string", enum: ["temperature", "precipitation", "humidity"] }
        },
        required: ["location", "month", "metric"]
      }
    }
  ],
  tool_choice: "auto"
};
```

### Multi-step prompt chains

Create complex workflows with dependent prompt executions:

```javascript
const analysisChain = {
  steps: [
    {
      name: "extract_key_points",
      prompt: "Extract the key points from this research paper abstract: {{abstract}}",
      model: "claude-sonnet",
      output_variable: "key_points"
    },
    {
      name: "generate_questions",
      prompt: "Based on these key points: {{key_points}}, generate 5 research questions that need further investigation",
      model: "gpt-4",
      output_variable: "research_questions" 
    },
    {
      name: "create_methodology",
      prompt: "For these research questions: {{research_questions}}, suggest appropriate research methodologies",
      model: "claude-opus",
      output_variable: "methodologies"
    }
  ],
  variables: {
    abstract: "Artificial intelligence systems are increasingly being deployed..."
  }
};

// Execute the chain
const chainResult = await fetch('https://api.promptexec.ai/v2/execute/chain', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer sk_live_abc123def456ghi789',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(analysisChain)
});
```

### Prompt optimization and A/B testing

Test different prompt variations to optimize performance:

```javascript
const promptVariants = {
  test_name: "product_description_optimization",
  variants: [
    {
      id: "variant_a",
      prompt: "Write a product description for {{product}}",
      weight: 0.5
    },
    {
      id: "variant_b", 
      prompt: "Create a compelling, benefit-focused description for {{product}} that highlights its key value propositions",
      weight: 0.5
    }
  ],
  model: "gpt-4",
  variables: {
    product: "Wireless Bluetooth Headphones"
  },
  success_metrics: ["length", "sentiment_score", "keyword_density"]
};
```

---

## 8. Performance and scaling

Optimize API usage for high-performance applications and large-scale deployments.

### Performance optimization strategies

**Model selection guidelines:**
- **GPT-4:** Best for complex reasoning, high-quality output (slower, more expensive)
- **GPT-3.5-Turbo:** Good balance of speed and quality for most use cases
- **Claude-Sonnet:** Excellent for analysis and structured outputs
- **Claude-Haiku:** Fastest for simple tasks and high-throughput scenarios

**Token optimization:**
```javascript
// Optimize token usage with precise max_tokens
const optimizedRequest = {
  prompt: "Summarize this article in exactly 100 words",
  model: "gpt-3.5-turbo",
  max_tokens: 150, // Allow buffer for exact word count
  temperature: 0.3
};

// Use templates to reduce prompt tokens
const templateRequest = {
  template_id: "summarizer_v1",
  variables: { content: articleText },
  max_tokens: 150
};
```

**Caching strategy:**
```javascript
class PromptCache {
  constructor() {
    this.cache = new Map();
    this.maxSize = 1000;
  }
  
  getCacheKey(prompt, model, params) {
    return `${model}:${JSON.stringify(params)}:${prompt}`;
  }
  
  async execute(prompt, model, params = {}) {
    const cacheKey = this.getCacheKey(prompt, model, params);
    
    if (this.cache.has(cacheKey)) {
      console.log('Cache hit');
      return this.cache.get(cacheKey);
    }
    
    const result = await executePrompt(prompt, model, params);
    
    if (this.cache.size >= this.maxSize) {
      const firstKey = this.cache.keys().next().value;
      this.cache.delete(firstKey);
    }
    
    this.cache.set(cacheKey, result);
    return result;
  }
}
```

### Monitoring and analytics

**Performance metrics tracking:**
```javascript
class PerformanceMonitor {
  constructor(apiKey) {
    this.apiKey = apiKey;
    this.metrics = {
      requests: 0,
      totalLatency: 0,
      errors: 0,
      tokenUsage: 0
    };
  }
  
  async executeWithMetrics(prompt, model, params = {}) {
    const startTime = Date.now();
    this.metrics.requests++;
    
    try {
      const result = await executePrompt(prompt, model, params);
      
      const latency = Date.now() - startTime;
      this.metrics.totalLatency += latency;
      this.metrics.tokenUsage += result.metadata.tokens_consumed;
      
      console.log(`Request completed in ${latency}ms`);
      return result;
      
    } catch (error) {
      this.metrics.errors++;
      console.error('Request failed:', error.message);
      throw error;
    }
  }
  
  getAverageLatency() {
    return this.metrics.requests > 0 
      ? this.metrics.totalLatency / this.metrics.requests 
      : 0;
  }
  
  getErrorRate() {
    return this.metrics.requests > 0 
      ? this.metrics.errors / this.metrics.requests 
      : 0;
  }
}
```

**Cost optimization:**
```javascript
class CostTracker {
  constructor() {
    this.dailyCosts = new Map();
    this.modelPricing = {
      'gpt-4': { input: 0.03, output: 0.06 }, // per 1K tokens
      'gpt-3.5-turbo': { input: 0.0015, output: 0.002 },
      'claude-sonnet': { input: 0.003, output: 0.015 },
      'claude-opus': { input: 0.015, output: 0.075 }
    };
  }
  
  calculateCost(model, inputTokens, outputTokens) {
    const pricing = this.modelPricing[model];
    if (!pricing) return 0;
    
    const inputCost = (inputTokens / 1000) * pricing.input;
    const outputCost = (outputTokens / 1000) * pricing.output;
    return inputCost + outputCost;
  }
  
  trackExecution(model, metadata) {
    const today = new Date().toISOString().split('T')[0];
    const cost = this.calculateCost(
      model, 
      metadata.input_tokens, 
      metadata.output_tokens
    );
    
    if (!this.dailyCosts.has(today)) {
      this.dailyCosts.set(today, { cost: 0, requests: 0 });
    }
    
    const dailyData = this.dailyCosts.get(today);
    dailyData.cost += cost;
    dailyData.requests += 1;
    
    console.log(`Request cost: ${cost.toFixed(4)}, Daily total: ${dailyData.cost.toFixed(2)}`);
  }
  
  getDailyCost(date = null) {
    const targetDate = date || new Date().toISOString().split('T')[0];
    return this.dailyCosts.get(targetDate) || { cost: 0, requests: 0 };
  }
}
```

### Scaling for high-volume applications

**Connection pooling and request management:**
```javascript
class PromptExecutionClient {
  constructor(apiKey, options = {}) {
    this.apiKey = apiKey;
    this.baseURL = 'https://api.promptexec.ai/v2';
    this.maxConcurrent = options.maxConcurrent || 10;
    this.requestQueue = [];
    this.activeRequests = 0;
  }
  
  async execute(prompt, model, params = {}) {
    return new Promise((resolve, reject) => {
      this.requestQueue.push({ prompt, model, params, resolve, reject });
      this.processQueue();
    });
  }
  
  async processQueue() {
    while (this.requestQueue.length > 0 && this.activeRequests < this.maxConcurrent) {
      const request = this.requestQueue.shift();
      this.activeRequests++;
      
      this.executeRequest(request)
        .finally(() => {
          this.activeRequests--;
          this.processQueue();
        });
    }
  }
  
  async executeRequest({ prompt, model, params, resolve, reject }) {
    try {
      const response = await fetch(`${this.baseURL}/execute`, {
        method: 'POST',
        headers: {
          'Authorization': `Bearer ${this.apiKey}`,
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({ prompt, model, ...params })
      });
      
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}: ${response.statusText}`);
      }
      
      const result = await response.json();
      resolve(result);
    } catch (error) {
      reject(error);
    }
  }
}
```

**Load balancing across multiple API keys:**
```javascript
class LoadBalancedClient {
  constructor(apiKeys) {
    this.apiKeys = apiKeys;
    this.currentKeyIndex = 0;
    this.keyUsage = new Map();
    
    // Initialize usage tracking
    apiKeys.forEach(key => {
      this.keyUsage.set(key, { requests: 0, errors: 0 });
    });
  }
  
  getNextApiKey() {
    // Round-robin with error rate consideration
    let attempts = 0;
    while (attempts < this.apiKeys.length) {
      const key = this.apiKeys[this.currentKeyIndex];
      const usage = this.keyUsage.get(key);
      
      this.currentKeyIndex = (this.currentKeyIndex + 1) % this.apiKeys.length;
      
      // Skip keys with high error rates
      if (usage.requests > 10 && (usage.errors / usage.requests) > 0.1) {
        attempts++;
        continue;
      }
      
      return key;
    }
    
    // Fallback to first key if all have high error rates
    return this.apiKeys[0];
  }
  
  async execute(prompt, model, params = {}) {
    const apiKey = this.getNextApiKey();
    const usage = this.keyUsage.get(apiKey);
    usage.requests++;
    
    try {
      const client = new PromptExecutionClient(apiKey);
      return await client.execute(prompt, model, params);
    } catch (error) {
      usage.errors++;
      throw error;
    }
  }
}
```

### Enterprise integration patterns

**Event-driven architecture:**
```javascript
class EventDrivenPromptProcessor {
  constructor(apiKey, eventBus) {
    this.apiKey = apiKey;
    this.eventBus = eventBus;
    this.client = new PromptExecutionClient(apiKey);
    
    // Set up event listeners
    this.eventBus.on('prompt.requested', this.handlePromptRequest.bind(this));
    this.eventBus.on('batch.requested', this.handleBatchRequest.bind(this));
  }
  
  async handlePromptRequest(event) {
    const { requestId, prompt, model, params, userId } = event.data;
    
    try {
      this.eventBus.emit('prompt.started', { requestId, userId });
      
      const result = await this.client.execute(prompt, model, params);
      
      this.eventBus.emit('prompt.completed', {
        requestId,
        userId,
        result,
        metadata: {
          cost: result.metadata.cost_usd,
          tokens: result.metadata.tokens_consumed,
          duration: result.metadata.execution_time_ms
        }
      });
      
    } catch (error) {
      this.eventBus.emit('prompt.failed', {
        requestId,
        userId,
        error: error.message
      });
    }
  }
  
  async handleBatchRequest(event) {
    const { batchId, prompts, model, userId } = event.data;
    
    this.eventBus.emit('batch.started', { batchId, userId, count: prompts.length });
    
    const results = [];
    let completed = 0;
    
    for (const prompt of prompts) {
      try {
        const result = await this.client.execute(prompt.text, model, prompt.params);
        results.push({ id: prompt.id, result, status: 'completed' });
      } catch (error) {
        results.push({ id: prompt.id, error: error.message, status: 'failed' });
      }
      
      completed++;
      this.eventBus.emit('batch.progress', {
        batchId,
        userId,
        completed,
        total: prompts.length,
        progress: (completed / prompts.length) * 100
      });
    }
    
    this.eventBus.emit('batch.completed', { batchId, userId, results });
  }
}
```

**Database integration for audit and analytics:**
```sql
-- Example schema for tracking prompt executions
CREATE TABLE prompt_executions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id VARCHAR(255) NOT NULL,
  prompt_hash VARCHAR(64) NOT NULL, -- For deduplication
  model VARCHAR(50) NOT NULL,
  status VARCHAR(20) NOT NULL, -- 'completed', 'failed', 'timeout'
  input_tokens INTEGER,
  output_tokens INTEGER,
  cost_usd DECIMAL(10, 6),
  execution_time_ms INTEGER,
  created_at TIMESTAMP DEFAULT NOW(),
  completed_at TIMESTAMP,
  error_code VARCHAR(50),
  error_message TEXT
);

-- Indexes for common queries
CREATE INDEX idx_prompt_executions_user_id ON prompt_executions(user_id);
CREATE INDEX idx_prompt_executions_created_at ON prompt_executions(created_at);
CREATE INDEX idx_prompt_executions_model ON prompt_executions(model);
CREATE INDEX idx_prompt_executions_status ON prompt_executions(status);
```

```javascript
class DatabaseIntegratedClient {
  constructor(apiKey, database) {
    this.apiKey = apiKey;
    this.db = database;
    this.client = new PromptExecutionClient(apiKey);
  }
  
  async execute(prompt, model, params = {}, userId) {
    const executionId = this.generateUUID();
    const promptHash = this.hashPrompt(prompt);
    
    // Log start of execution
    await this.db.query(`
      INSERT INTO prompt_executions (id, user_id, prompt_hash, model, status, created_at)
      VALUES ($1, $2, $3, $4, 'started', NOW())
    `, [executionId, userId, promptHash, model]);
    
    try {
      const startTime = Date.now();
      const result = await this.client.execute(prompt, model, params);
      const endTime = Date.now();
      
      // Log successful completion
      await this.db.query(`
        UPDATE prompt_executions 
        SET status = 'completed',
            input_tokens = $1,
            output_tokens = $2,
            cost_usd = $3,
            execution_time_ms = $4,
            completed_at = NOW()
        WHERE id = $5
      `, [
        result.metadata.input_tokens,
        result.metadata.output_tokens,
        result.metadata.cost_usd,
        endTime - startTime,
        executionId
      ]);
      
      return { ...result, execution_id: executionId };
      
    } catch (error) {
      // Log failure
      await this.db.query(`
        UPDATE prompt_executions 
        SET status = 'failed',
            error_code = $1,
            error_message = $2,
            completed_at = NOW()
        WHERE id = $3
      `, [error.code || 'UNKNOWN', error.message, executionId]);
      
      throw error;
    }
  }
  
  hashPrompt(prompt) {
    // Simple hash for deduplication (use crypto.createHash in Node.js)
    return btoa(prompt).slice(0, 64);
  }
  
  generateUUID() {
    // Simple UUID generation (use crypto.randomUUID() in Node.js)
    return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
      const r = Math.random() * 16 | 0;
      const v = c === 'x' ? r : (r & 0x3 | 0x8);
      return v.toString(16);
    });
  }
  
  async getUsageAnalytics(userId, startDate, endDate) {
    const result = await this.db.query(`
      SELECT 
        model,
        COUNT(*) as request_count,
        SUM(input_tokens) as total_input_tokens,
        SUM(output_tokens) as total_output_tokens,
        SUM(cost_usd) as total_cost,
        AVG(execution_time_ms) as avg_execution_time,
        COUNT(CASE WHEN status = 'failed' THEN 1 END) as failed_requests
      FROM prompt_executions 
      WHERE user_id = $1 
        AND created_at >= $2 
        AND created_at <= $3
      GROUP BY model
      ORDER BY total_cost DESC
    `, [userId, startDate, endDate]);
    
    return result.rows;
  }
}
```

---

[Back to top](#prompt-execution-api-documentation)

## Additional resources

**Developer Portal:** [developer.promptexec.ai](https://developer.promptexec.ai)  
**SDK Downloads:** Available for Python, JavaScript, Java, and Go  
**Community Forum:** [community.promptexec.ai](https://community.promptexec.ai)  
**Status Page:** [status.promptexec.ai](https://status.promptexec.ai)

**Support channels:**
- **Technical Support:** [support@promptexec.ai](mailto:support@promptexec.ai)
- **API Documentation Issues:** [docs@promptexec.ai](mailto:docs@promptexec.ai)
- **Enterprise Sales:** [enterprise@promptexec.ai](mailto:enterprise@promptexec.ai)

**Legal and compliance:**
- [Terms of Service](https://promptexec.ai/terms)
- [Privacy Policy](https://promptexec.ai/privacy)
- [Data Processing Agreement](https://promptexec.ai/dpa)

---

*This API reference follows WCAG 2.1 AA accessibility standards and is optimized for developers using screen readers and assistive technologies. All code examples have been tested and verified for accuracy.*

**Document metadata:**
- Content generated following Technical Writing Style Guide v2.0
- Technical accuracy validated by API development team
- Accessibility compliance verified by UX team
- Last human review: May 25, 2025