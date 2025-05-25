# Prompt Execution API v2.1 Documentation

**API Metadata:**
- **Version:** v2.1
- **Base URL:** `https://api.promptexecution.com/v2`
- **Authentication:** Bearer Token (OAuth 2.0)
- **Content Type:** `application/json`
- **Rate Limit:** 1,000 requests/hour (burst: 100/minute)
- **OpenAPI Spec:** [Download OpenAPI 3.0 Schema](https://api.promptexecution.com/v2/openapi.json)

---

## **SDK Installation & Quick Start**

### **Official SDK Support**

<details>
<summary>Node.js SDK Installation</summary>

```bash
npm install @promptexecution/node-sdk
# or
yarn add @promptexecution/node-sdk
```

**Quick Start:**
```javascript
import { PromptExecutionClient } from '@promptexecution/node-sdk';

const client = new PromptExecutionClient({
  apiKey: 'pe_sk_live_abc123def456ghi789',
  timeout: 30000,
  retryAttempts: 3
});

// Simple execution
const result = await client.execute({
  templateId: "greeting_template",
  variables: { name: "Alice" }
});
console.log(result.content);
```

</details>

<details>
<summary>Python SDK Installation</summary>

```bash
pip install promptexecution
# or
poetry add promptexecution
```

**Quick Start:**
```python
from promptexecution import PromptExecutionClient

client = PromptExecutionClient(
    api_key='pe_sk_live_abc123def456ghi789',
    timeout=30,
    max_retries=3
)

# Simple execution
result = client.execute(
    template_id="greeting_template",
    variables={"name": "Alice"}
)
print(result.content)
```

</details>

<details>
<summary>Go SDK Installation</summary>

```bash
go get github.com/promptexecution/go-sdk
```

**Quick Start:**
```go
package main

import (
    "context"
    "fmt"
    "log"
    
    "github.com/promptexecution/go-sdk"
)

func main() {
    client := promptexecution.NewClient("pe_sk_live_abc123def456ghi789")
    
    result, err := client.Execute(context.Background(), &promptexecution.ExecuteRequest{
        TemplateID: "greeting_template",
        Variables: map[string]interface{}{
            "name": "Alice",
        },
    })
    
    if err != nil {
        log.Fatal(err)
    }
    
    fmt.Println(result.Content)
}
```

</details>

<details>
<summary>Ruby SDK Installation</summary>

```bash
gem install promptexecution
# or add to Gemfile
gem 'promptexecution'
```

**Quick Start:**
```ruby
require 'promptexecution'

client = PromptExecution::Client.new(
  api_key: 'pe_sk_live_abc123def456ghi789'
)

result = client.execute(
  template_id: 'greeting_template',
  variables: { name: 'Alice' }
)

puts result.content
```

</details>

<details>
<summary>PHP SDK Installation</summary>

```bash
composer require promptexecution/php-sdk
```

**Quick Start:**
```php
<?php
require_once 'vendor/autoload.php';

use PromptExecution\Client;

$client = new Client([
    'api_key' => 'pe_sk_live_abc123def456ghi789'
]);

$result = $client->execute([
    'template_id' => 'greeting_template',
    'variables' => ['name' => 'Alice']
]);

echo $result->content;
?>
```

</details>

### **Framework Integration Examples**

<details>
<summary>React/Next.js Integration</summary>

```javascript
// hooks/usePromptExecution.js
import { useState, useCallback } from 'react';
import { PromptExecutionClient } from '@promptexecution/node-sdk';

const client = new PromptExecutionClient({
  apiKey: process.env.NEXT_PUBLIC_PROMPT_EXECUTION_API_KEY
});

export function usePromptExecution() {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  
  const execute = useCallback(async (templateId, variables) => {
    setLoading(true);
    setError(null);
    
    try {
      const result = await client.execute({
        templateId,
        variables
      });
      return result;
    } catch (err) {
      setError(err.message);
      throw err;
    } finally {
      setLoading(false);
    }
  }, []);
  
  return { execute, loading, error };
}

// components/ContentGenerator.jsx
import { useState } from 'react';
import { usePromptExecution } from '../hooks/usePromptExecution';

export function ContentGenerator() {
  const [topic, setTopic] = useState('');
  const [content, setContent] = useState('');
  const { execute, loading, error } = usePromptExecution();
  
  const generateContent = async () => {
    try {
      const result = await execute('blog_post_generator', {
        topic,
        tone: 'professional'
      });
      setContent(result.content);
    } catch (err) {
      console.error('Generation failed:', err);
    }
  };
  
  return (
    <div>
      <input 
        value={topic}
        onChange={(e) => setTopic(e.target.value)}
        placeholder="Enter topic..."
      />
      <button onClick={generateContent} disabled={loading}>
        {loading ? 'Generating...' : 'Generate Content'}
      </button>
      {error && <div className="error">{error}</div>}
      {content && <div className="content">{content}</div>}
    </div>
  );
}
```

</details>

<details>
<summary>Express.js Middleware</summary>

```javascript
// middleware/promptExecution.js
import { PromptExecutionClient } from '@promptexecution/node-sdk';

const client = new PromptExecutionClient({
  apiKey: process.env.PROMPT_EXECUTION_API_KEY
});

export const promptExecutionMiddleware = (req, res, next) => {
  req.promptExecution = {
    execute: async (templateId, variables) => {
      return await client.execute({ templateId, variables });
    },
    
    executeBatch: async (templateId, inputs) => {
      return await client.batch.submit({ templateId, inputs });
    }
  };
  
  next();
};

// routes/content.js
import express from 'express';

const router = express.Router();

router.post('/generate', async (req, res) => {
  try {
    const { templateId, variables } = req.body;
    
    const result = await req.promptExecution.execute(templateId, variables);
    
    res.json({
      success: true,
      content: result.content,
      metadata: result.metadata
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: error.message
    });
  }
});

export default router;
```

</details>

<details>
<summary>Django Integration</summary>

```python
# services/prompt_execution.py
from django.conf import settings
from promptexecution import PromptExecutionClient
from promptexecution.exceptions import APIError

class PromptExecutionService:
    def __init__(self):
        self.client = PromptExecutionClient(
            api_key=settings.PROMPT_EXECUTION_API_KEY
        )
    
    async def generate_content(self, template_id: str, variables: dict):
        """Generate content using prompt execution"""
        try:
            result = await self.client.execute_async(
                template_id=template_id,
                variables=variables
            )
            return {
                'success': True,
                'content': result.content,
                'metadata': result.metadata
            }
        except APIError as e:
            return {
                'success': False,
                'error': str(e)
            }

# views.py
from django.http import JsonResponse
from django.views.decorators.csrf import csrf_exempt
from django.views.decorators.http import require_http_methods
from .services.prompt_execution import PromptExecutionService
import json

@csrf_exempt
@require_http_methods(["POST"])
async def generate_content(request):
    try:
        data = json.loads(request.body)
        template_id = data.get('template_id')
        variables = data.get('variables', {})
        
        service = PromptExecutionService()
        result = await service.generate_content(template_id, variables)
        
        return JsonResponse(result)
    except Exception as e:
        return JsonResponse({
            'success': False,
            'error': str(e)
        }, status=500)
```

</details>

### **SDK Feature Comparison**

| Feature | Node.js | Python | Go | Ruby | PHP |
|---------|---------|--------|----|----- |-----|
| Async/Await | ✅ | ✅ | ✅ | ✅ | ❌ |
| Batch Processing | ✅ | ✅ | ✅ | ✅ | ✅ |
| Streaming | ✅ | ✅ | ✅ | ❌ | ❌ |
| Progress Tracking | ✅ | ✅ | ✅ | ✅ | ✅ |
| Auto Retry | ✅ | ✅ | ✅ | ✅ | ✅ |
| Rate Limit Handling | ✅ | ✅ | ✅ | ✅ | ✅ |
| TypeScript Support | ✅ | N/A | N/A | N/A | N/A |

---

## Table of Contents

1. [Overview](#1-overview)
2. [Authentication](#2-authentication)
3. [Common use cases](#3-common-use-cases)
4. [Prompt execution endpoints](#4-prompt-execution-endpoints)
5. [Error handling](#5-error-handling)
6. [Rate limiting](#6-rate-limiting)
7. [Batch processing and workflows](#7-batch-processing-and-workflows)
8. [Performance and scaling](#8-performance-and-scaling)

---

## 1. Overview

The Prompt Execution API enables developers to execute AI prompts at scale with enterprise-grade reliability, monitoring, and optimization features. This RESTful API supports synchronous and asynchronous execution, batch processing, and real-time result streaming.

### API capabilities

- **Prompt Execution:** Run single or batch AI prompts with customizable parameters
- **Template Management:** Create, version, and deploy reusable prompt templates
- **Result Processing:** Stream, cache, and transform execution results
- **Monitoring & Analytics:** Track performance, costs, and usage patterns
- **Workflow Automation:** Chain prompts and integrate with external systems

### Feature comparison by plan

| Feature | Basic | Professional | Enterprise |
|---------|--------|-------------|------------|
| Requests/hour | 1,000 | 10,000 | Unlimited |
| Concurrent executions | 5 | 50 | 500 |
| Batch processing | ❌ | ✅ | ✅ |
| Custom models | ❌ | ✅ | ✅ |
| Workflow automation | ❌ | ❌ | ✅ |
| Priority support | ❌ | ✅ | ✅ |

**🔗 Next Steps:** Ready to start? Jump to [Authentication](#2-authentication) or explore [Common use cases](#3-common-use-cases).

---

## 2. Authentication

The Prompt Execution API uses OAuth 2.0 Bearer tokens for secure authentication. All requests must include a valid API key in the Authorization header.

### Obtaining an API key

1. **Sign up** for an account at [https://promptexecution.com/signup](https://promptexecution.com/signup)
2. **Navigate** to API Settings in your dashboard
3. **Generate** a new API key with appropriate permissions
4. **Copy** your API key and store it securely

### Authentication headers

Include your API key in every request:

```bash
Authorization: Bearer pe_sk_live_abc123def456ghi789
Content-Type: application/json
```

### Security best practices

<div class="warning">
⚠️ **Security Note:** Never include API keys in client-side code, version control, or public repositories
</div>

- **Environment variables:** Store API keys in environment variables
- **Key rotation:** Regenerate keys quarterly or after security incidents
- **Scope limitation:** Use role-based keys with minimal required permissions
- **Network security:** Always use HTTPS for API requests

### Testing authentication

```bash
curl -X GET "https://api.promptexecution.com/v2/auth/verify" \
  -H "Authorization: Bearer pe_sk_live_abc123def456ghi789" \
  -H "Content-Type: application/json"
```

**✅ Success Response:**
```json
{
  "valid": true,
  "account_id": "acc_123456789",
  "plan": "professional",
  "permissions": ["execute", "batch", "templates"],
  "rate_limit": {
    "requests_per_hour": 10000,
    "remaining": 9987
  }
}
```

**🚨 Troubleshooting:** If you receive a 401 error, verify your API key format and ensure it starts with `pe_sk_live_` for production or `pe_sk_test_` for sandbox.

---

## 3. Common use cases

This section demonstrates three real-world implementation patterns that showcase practical API integration approaches for different business scenarios.

### Use case 1: Content generation pipeline

**Scenario:** Automated blog post creation with SEO optimization

<details>
<summary>Node.js SDK Example</summary>

```javascript
import { PromptExecutionClient } from '@promptexecution/node-sdk';

const client = new PromptExecutionClient({
  apiKey: 'pe_sk_live_abc123def456ghi789'
});

// Content generation with template variables
const generateBlogPost = async (topic, keywords) => {
  try {
    const result = await client.execute({
      templateId: "blog_post_seo_v2",
      variables: {
        topic: topic,
        keywords: keywords,
        tone: "professional",
        wordCount: 1500
      },
      modelConfig: {
        temperature: 0.7,
        maxTokens: 2000
      }
    });
    
    return result;
  } catch (error) {
    console.error('Blog generation failed:', error.message);
    throw error;
  }
};

// Usage example
const result = await generateBlogPost(
  "AI in Healthcare", 
  ["artificial intelligence", "medical diagnosis", "patient care"]
);
console.log(result.content);
```

</details>

<details>
<summary>Python SDK Example</summary>

```python
from promptexecution import PromptExecutionClient
from promptexecution.exceptions import APIError

client = PromptExecutionClient(api_key='pe_sk_live_abc123def456ghi789')

def generate_blog_post(topic: str, keywords: list) -> dict:
    """Generate SEO-optimized blog post using template"""
    try:
        result = client.execute(
            template_id="blog_post_seo_v2",
            variables={
                "topic": topic,
                "keywords": keywords,
                "tone": "professional",
                "word_count": 1500
            },
            model_config={
                "temperature": 0.7,
                "max_tokens": 2000
            }
        )
        return result
    except APIError as e:
        print(f"Blog generation failed: {e}")
        raise

# Usage example
result = generate_blog_post(
    "AI in Healthcare",
    ["artificial intelligence", "medical diagnosis", "patient care"]
)
print(result.content)
```

</details>

<details>
<summary>Raw HTTP Example</summary>

```javascript
// Raw fetch implementation for comparison
const generateBlogPost = async (topic, keywords) => {
  const response = await fetch('https://api.promptexecution.com/v2/execute', {
    method: 'POST',
    headers: {
      'Authorization': 'Bearer pe_sk_live_abc123def456ghi789',
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      template_id: "blog_post_seo_v2",
      variables: {
        topic: topic,
        keywords: keywords,
        tone: "professional",
        word_count: 1500
      },
      model_config: {
        temperature: 0.7,
        max_tokens: 2000
      }
    })
  });
  
  return await response.json();
};
```

</details>

### Use case 2: Customer support automation

**Scenario:** Real-time customer query classification and response generation

<details>
<summary>Python SDK Example</summary>

```python
from promptexecution import PromptExecutionClient
from promptexecution.exceptions import APIError, RateLimitError
import asyncio

class SupportAutomation:
    def __init__(self, api_key):
        self.client = PromptExecutionClient(api_key=api_key)
        
    async def classify_and_respond(self, customer_message, context):
        """Classify customer query and generate appropriate response"""
        
        try:
            # Step 1: Classify the query using async execution
            classification = await self.client.execute_async(
                template_id="support_classification_v3",
                variables={
                    "message": customer_message,
                    "customer_tier": context.get("tier", "standard")
                },
                model_config={
                    "temperature": 0.1  # Low temperature for consistent classification
                }
            )
            
            # Step 2: Generate response based on classification
            response = await self.client.execute_async(
                template_id=f"support_response_{classification['category']}",
                variables={
                    "message": customer_message,
                    "urgency": classification["urgency"],
                    "customer_name": context.get("name", "valued customer")
                }
            )
            
            return {
                "classification": classification,
                "response": response.content,
                "escalate": classification["urgency"] == "high"
            }
            
        except RateLimitError as e:
            # Handle rate limiting gracefully
            await asyncio.sleep(e.retry_after)
            return await self.classify_and_respond(customer_message, context)
        except APIError as e:
            print(f"Support automation error: {e}")
            return self._fallback_response(customer_message)
    
    def _fallback_response(self, message):
        """Fallback for API failures"""
        return {
            "classification": {"category": "general", "urgency": "medium"},
            "response": "Thank you for contacting us. A support representative will respond within 2 hours.",
            "escalate": True
        }

# Usage example
support = SupportAutomation("pe_sk_live_abc123def456ghi789")
result = await support.classify_and_respond(
    "My order hasn't arrived and I need it urgently for tomorrow",
    {"tier": "premium", "name": "Sarah Johnson"}
)
```

</details>

<details>
<summary>Node.js SDK Example</summary>

```javascript
import { PromptExecutionClient } from '@promptexecution/node-sdk';

class SupportAutomation {
  constructor(apiKey) {
    this.client = new PromptExecutionClient({ 
      apiKey,
      retryAttempts: 3,
      timeout: 30000
    });
  }
  
  async classifyAndRespond(customerMessage, context) {
    try {
      // Step 1: Classify the query
      const classification = await this.client.execute({
        templateId: "support_classification_v3",
        variables: {
          message: customerMessage,
          customerTier: context.tier || "standard"
        },
        modelConfig: {
          temperature: 0.1
        }
      });
      
      // Step 2: Generate response based on classification
      const response = await this.client.execute({
        templateId: `support_response_${classification.category}`,
        variables: {
          message: customerMessage,
          urgency: classification.urgency,
          customerName: context.name || "valued customer"
        }
      });
      
      return {
        classification,
        response: response.content,
        escalate: classification.urgency === "high"
      };
      
    } catch (error) {
      if (error.code === 'RATE_LIMIT_EXCEEDED') {
        // Wait and retry
        await new Promise(resolve => setTimeout(resolve, error.retryAfter * 1000));
        return this.classifyAndRespond(customerMessage, context);
      }
      
      console.error('Support automation error:', error.message);
      return this.fallbackResponse(customerMessage);
    }
  }
  
  fallbackResponse(message) {
    return {
      classification: { category: "general", urgency: "medium" },
      response: "Thank you for contacting us. A support representative will respond within 2 hours.",
      escalate: true
    };
  }
}

// Usage example
const support = new SupportAutomation("pe_sk_live_abc123def456ghi789");
const result = await support.classifyAndRespond(
  "My order hasn't arrived and I need it urgently for tomorrow",
  { tier: "premium", name: "Sarah Johnson" }
);
```

</details>

<details>
<summary>Raw HTTP Example</summary>

```python
import requests
import json

class SupportAutomation:
    def __init__(self, api_key):
        self.api_key = api_key
        self.base_url = "https://api.promptexecution.com/v2"
        
    def classify_and_respond(self, customer_message, context):
        """Classify customer query and generate appropriate response"""
        
        # Step 1: Classify the query
        classification = self.execute_prompt({
            "template_id": "support_classification_v3",
            "variables": {
                "message": customer_message,
                "customer_tier": context.get("tier", "standard")
            },
            "model_config": {
                "temperature": 0.1  # Low temperature for consistent classification
            }
        })
        
        # Step 2: Generate response based on classification
        response = self.execute_prompt({
            "template_id": f"support_response_{classification['category']}",
            "variables": {
                "message": customer_message,
                "urgency": classification["urgency"],
                "customer_name": context.get("name", "valued customer")
            }
        })
        
        return {
            "classification": classification,
            "response": response["content"],
            "escalate": classification["urgency"] == "high"
        }
    
    def execute_prompt(self, payload):
        """Helper method for API calls"""
        response = requests.post(
            f"{self.base_url}/execute",
            headers={
                "Authorization": f"Bearer {self.api_key}",
                "Content-Type": "application/json"
            },
            json=payload
        )
        return response.json()

# Usage example
support = SupportAutomation("pe_sk_live_abc123def456ghi789")
result = support.classify_and_respond(
    "My order hasn't arrived and I need it urgently for tomorrow",
    {"tier": "premium", "name": "Sarah Johnson"}
)
```

</details>

### Use case 3: Batch data processing

**Scenario:** Processing customer feedback surveys at scale

<details>
<summary>Python SDK Example</summary>

```python
from promptexecution import PromptExecutionClient
from promptexecution.batch import BatchProcessor
import asyncio

# Initialize client with batch processing capabilities
client = PromptExecutionClient(api_key='pe_sk_live_abc123def456ghi789')
batch_processor = BatchProcessor(client)

async def process_customer_feedback(reviews_data):
    """Process large volume of customer reviews with sentiment analysis"""
    
    # Prepare batch inputs
    batch_inputs = [
        {
            "id": f"review_{review['id']}",
            "variables": {
                "review_text": review["text"],
                "product_category": review["category"]
            }
        }
        for review in reviews_data
    ]
    
    # Execute batch processing with progress tracking
    batch_result = await batch_processor.execute_batch(
        template_id="sentiment_analysis_detailed_v1",
        inputs=batch_inputs,
        model_config={
            "temperature": 0.3,
            "response_format": "structured"
        },
        options={
            "callback_url": "https://your-app.com/webhooks/batch-complete",
            "max_concurrent": 50,
            "retry_failed": True
        }
    )
    
    print(f"Batch submitted: {batch_result.batch_id}")
    print(f"Estimated completion: {batch_result.estimated_completion}")
    
    # Monitor progress
    async for progress in batch_processor.track_progress(batch_result.batch_id):
        print(f"Progress: {progress.completed}/{progress.total} completed")
        if progress.status == "completed":
            break
    
    # Retrieve results
    results = await batch_processor.get_results(batch_result.batch_id)
    return results

# Usage example
reviews = [
    {"id": 1, "text": "Great product, fast shipping!", "category": "electronics"},
    {"id": 2, "text": "Poor quality, disappointed", "category": "electronics"},
    # ... more reviews
]

results = await process_customer_feedback(reviews)
print(f"Processed {len(results.successful)} reviews successfully")
```

</details>

<details>
<summary>Node.js SDK Example</summary>

```javascript
import { PromptExecutionClient, BatchProcessor } from '@promptexecution/node-sdk';

const client = new PromptExecutionClient({
  apiKey: 'pe_sk_live_abc123def456ghi789'
});

const batchProcessor = new BatchProcessor(client);

async function processCustomerFeedback(reviewsData) {
  // Prepare batch inputs
  const batchInputs = reviewsData.map(review => ({
    id: `review_${review.id}`,
    variables: {
      reviewText: review.text,
      productCategory: review.category
    }
  }));
  
  try {
    // Submit batch for processing
    const batchResult = await batchProcessor.submit({
      templateId: "sentiment_analysis_detailed_v1",
      inputs: batchInputs,
      modelConfig: {
        temperature: 0.3,
        responseFormat: "structured"
      },
      options: {
        callbackUrl: "https://your-app.com/webhooks/batch-complete",
        maxConcurrent: 50,
        retryFailed: true
      }
    });
    
    console.log(`Batch submitted: ${batchResult.batchId}`);
    
    // Set up progress monitoring
    const progressStream = batchProcessor.trackProgress(batchResult.batchId);
    
    progressStream.on('progress', (progress) => {
      console.log(`Progress: ${progress.completed}/${progress.total} completed`);
    });
    
    progressStream.on('completed', async () => {
      const results = await batchProcessor.getResults(batchResult.batchId);
      console.log(`Successfully processed ${results.successful.length} reviews`);
      
      // Process results
      results.successful.forEach(result => {
        console.log(`Review ${result.itemId}: ${result.content}`);
      });
    });
    
    progressStream.on('error', (error) => {
      console.error('Batch processing error:', error);
    });
    
  } catch (error) {
    console.error('Failed to submit batch:', error.message);
    throw error;
  }
}

// Usage example
const reviews = [
  { id: 1, text: "Great product, fast shipping!", category: "electronics" },
  { id: 2, text: "Poor quality, disappointed", category: "electronics" },
  // ... more reviews
];

await processCustomerFeedback(reviews);
```

</details>

<details>
<summary>Raw HTTP Example</summary>

```bash
# Batch processing for sentiment analysis of 1000 customer reviews
curl -X POST "https://api.promptexecution.com/v2/batch" \
  -H "Authorization: Bearer pe_sk_live_abc123def456ghi789" \
  -H "Content-Type: application/json" \
  -d '{
    "template_id": "sentiment_analysis_detailed_v1",
    "inputs": [
      {
        "id": "review_001",
        "variables": {
          "review_text": "Great product, fast shipping!",
          "product_category": "electronics"
        }
      },
      {
        "id": "review_002", 
        "variables": {
          "review_text": "Poor quality, disappointed with purchase",
          "product_category": "electronics"
        }
      }
    ],
    "model_config": {
      "temperature": 0.3,
      "response_format": "structured"
    },
    "callback_url": "https://your-app.com/webhooks/batch-complete"
  }'
```

</details>

**✅ Batch Response:**
```json
{
  "batch_id": "batch_789xyz",
  "status": "processing",
  "total_items": 1000,
  "estimated_completion": "2024-01-15T10:30:00Z",
  "webhook_url": "https://your-app.com/webhooks/batch-complete"
}
```

---

## 4. Prompt execution endpoints

### 4.1 Execute single prompt

Execute a prompt immediately with synchronous response.

**`POST https://api.promptexecution.com/v2/execute`**

**Purpose:** Execute a single prompt with custom variables and model configuration for immediate results.

**Parameters:**

| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `template_id` | string | ⚠️ Optional | Pre-configured prompt template ID | `"blog_post_seo_v2"` |
| `prompt` | string | ⚠️ Optional | Raw prompt text (required if no template_id) | `"Summarize this article: {content}"` |
| `variables` | object | ⚠️ Optional | Key-value pairs for template substitution | `{"topic": "AI", "tone": "casual"}` |
| `model_config` | object | ⚠️ Optional | Model execution parameters | `{"temperature": 0.7, "max_tokens": 1000}` |
| `response_format` | string | ⚠️ Optional | Output format preference | `"text"`, `"json"`, `"structured"` |
| `stream` | boolean | ⚠️ Optional | Enable real-time streaming | `false` |

**Request Example:**

<details>
<summary>Complete Request Example</summary>

```bash
curl -X POST "https://api.promptexecution.com/v2/execute" \
  -H "Authorization: Bearer pe_sk_live_abc123def456ghi789" \
  -H "Content-Type: application/json" \
  -d '{
    "template_id": "product_description_v3",
    "variables": {
      "product_name": "Wireless Noise-Canceling Headphones",
      "key_features": ["Active noise cancellation", "30-hour battery", "Premium sound quality"],
      "target_audience": "music enthusiasts",
      "tone": "professional yet engaging"
    },
    "model_config": {
      "temperature": 0.8,
      "max_tokens": 500,
      "top_p": 0.9
    },
    "response_format": "structured"
  }'
```

</details>

**Response Example:**

```json
{
  "execution_id": "exec_456def789",
  "status": "completed",
  "content": "Experience premium audio with our Wireless Noise-Canceling Headphones...",
  "metadata": {
    "tokens_used": 487,
    "execution_time": 2.3,
    "model": "gpt-4-turbo",
    "cost": 0.024
  },
  "created_at": "2024-01-15T09:15:30Z"
}
```

**Error Responses:**

- **400 Bad Request:** Missing required parameters or invalid template ID
- **401 Unauthorized:** Invalid or expired API key
- **403 Forbidden:** Insufficient permissions for template access
- **429 Too Many Requests:** Rate limit exceeded
- **500 Internal Server Error:** Temporary service unavailability

**Rate Limiting:**

This endpoint counts against your hourly request limit. Check the `X-RateLimit-Remaining` header for current usage.

### 4.2 Create prompt template

Create reusable prompt templates with versioning support.

**`POST https://api.promptexecution.com/v2/templates`**

**Purpose:** Create a versioned prompt template for consistent execution across your applications.

**Parameters:**

| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `name` | string | ✅ Required | Unique template identifier | `"customer_email_response"` |
| `description` | string | ⚠️ Optional | Template purpose and usage notes | `"Generates professional customer service responses"` |
| `prompt` | string | ✅ Required | Template with variable placeholders | `"Reply to customer: {{message}} in {{tone}} tone"` |
| `variables` | array | ⚠️ Optional | Variable definitions with validation | `[{"name": "tone", "type": "string", "required": true}]` |
| `default_config` | object | ⚠️ Optional | Default model configuration | `{"temperature": 0.7, "max_tokens": 1000}` |
| `tags` | array | ⚠️ Optional | Organizational tags | `["customer-service", "email", "v2"]` |

**Request Example:**

```bash
curl -X POST "https://api.promptexecution.com/v2/templates" \
  -H "Authorization: Bearer pe_sk_live_abc123def456ghi789" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "product_review_summary",
    "description": "Summarizes product reviews with sentiment analysis",
    "prompt": "Analyze these product reviews for {{product_name}}:\n\n{{reviews}}\n\nProvide:\n1. Overall sentiment (positive/negative/neutral)\n2. Key themes\n3. Improvement suggestions\n4. Rating prediction (1-5 stars)",
    "variables": [
      {
        "name": "product_name",
        "type": "string",
        "required": true,
        "description": "Name of the product being reviewed"
      },
      {
        "name": "reviews", 
        "type": "string",
        "required": true,
        "description": "Concatenated customer reviews"
      }
    ],
    "default_config": {
      "temperature": 0.3,
      "max_tokens": 800,
      "response_format": "structured"
    },
    "tags": ["analytics", "reviews", "sentiment"]
  }'
```

**✅ Success Response:**
```json
{
  "template_id": "tmpl_abc123xyz789",
  "name": "product_review_summary",
  "version": "1.0",
  "status": "active",
  "created_at": "2024-01-15T09:20:45Z",
  "url": "https://api.promptexecution.com/v2/templates/tmpl_abc123xyz789"
}
```

### 4.3 Get execution status

Monitor the status and results of prompt executions.

**`GET https://api.promptexecution.com/v2/executions/{execution_id}`**

**Purpose:** Retrieve detailed information about a specific prompt execution including results, metadata, and performance metrics.

**Path Parameters:**

| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `execution_id` | string | ✅ Required | Unique execution identifier | `"exec_456def789"` |

**Query Parameters:**

| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `include_content` | boolean | ⚠️ Optional | Include full execution results | `true` |
| `include_metadata` | boolean | ⚠️ Optional | Include performance and cost data | `true` |

**Request Example:**

```bash
curl -X GET "https://api.promptexecution.com/v2/executions/exec_456def789?include_content=true&include_metadata=true" \
  -H "Authorization: Bearer pe_sk_live_abc123def456ghi789"
```

**Response Example:**

```json
{
  "execution_id": "exec_456def789",
  "template_id": "tmpl_abc123xyz789",
  "status": "completed",
  "content": "Based on 247 customer reviews for the Wireless Headphones:\n\n1. **Overall Sentiment:** Positive (83% satisfaction)\n2. **Key Themes:**\n   - Sound quality praised by 91% of reviewers\n   - Battery life concerns mentioned by 23%\n   - Comfort during extended use highlighted positively\n3. **Improvement Suggestions:**\n   - Enhanced battery optimization\n   - Additional size options for better fit\n4. **Rating Prediction:** 4.2/5 stars",
  "metadata": {
    "tokens_used": 682,
    "execution_time": 3.7,
    "model": "gpt-4-turbo",
    "cost": 0.034,
    "variables_used": {
      "product_name": "Wireless Noise-Canceling Headphones",
      "reviews": "[247 reviews processed]"
    }
  },
  "created_at": "2024-01-15T09:15:30Z",
  "completed_at": "2024-01-15T09:15:34Z"
}
```

### 4.4 List execution history

Retrieve paginated history of prompt executions with filtering options.

**`GET https://api.promptexecution.com/v2/executions`**

**Purpose:** Access historical execution data for monitoring, analytics, and debugging purposes.

**Query Parameters:**

| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `limit` | integer | ⚠️ Optional | Results per page (1-100) | `25` |
| `offset` | integer | ⚠️ Optional | Pagination offset | `50` |
| `template_id` | string | ⚠️ Optional | Filter by template | `"tmpl_abc123xyz789"` |
| `status` | string | ⚠️ Optional | Filter by execution status | `"completed"`, `"failed"`, `"processing"` |
| `date_from` | string | ⚠️ Optional | Start date filter (ISO 8601) | `"2024-01-01T00:00:00Z"` |
| `date_to` | string | ⚠️ Optional | End date filter (ISO 8601) | `"2024-01-15T23:59:59Z"` |

**Request Example:**

```bash
curl -X GET "https://api.promptexecution.com/v2/executions?limit=10&status=completed&template_id=tmpl_abc123xyz789" \
  -H "Authorization: Bearer pe_sk_live_abc123def456ghi789"
```

**Response Example:**

```json
{
  "executions": [
    {
      "execution_id": "exec_456def789",
      "template_id": "tmpl_abc123xyz789",
      "status": "completed",
      "tokens_used": 682,
      "cost": 0.034,
      "created_at": "2024-01-15T09:15:30Z"
    },
    {
      "execution_id": "exec_123abc456", 
      "template_id": "tmpl_abc123xyz789",
      "status": "completed",
      "tokens_used": 445,
      "cost": 0.022,
      "created_at": "2024-01-15T08:42:15Z"
    }
  ],
  "pagination": {
    "total": 1247,
    "limit": 10,
    "offset": 0,
    "has_more": true
  }
}
```

[Back to top](#prompt-execution-api-v21-documentation)

---

## 5. Error handling

The Prompt Execution API uses standard HTTP status codes and provides detailed error information to help you troubleshoot integration issues effectively.

### Standard error responses

All error responses follow this consistent format:

```json
{
  "error": {
    "type": "validation_error",
    "code": "MISSING_REQUIRED_FIELD",
    "message": "The 'template_id' or 'prompt' field is required",
    "details": {
      "field": "template_id",
      "provided_value": null,
      "expected_type": "string"
    },
    "request_id": "req_789xyz123",
    "timestamp": "2024-01-15T09:25:42Z"
  }
}
```

### HTTP status codes

| Code | Status | Description | Common Causes |
|------|--------|-------------|---------------|
| 200 | OK | Request successful | - |
| 400 | Bad Request | Invalid request parameters | Missing required fields, invalid JSON format, template not found |
| 401 | Unauthorized | Authentication failed | Invalid API key, expired token, malformed Authorization header |
| 403 | Forbidden | Insufficient permissions | Template access denied, account suspended, feature not available in plan |
| 404 | Not Found | Resource not found | Execution ID doesn't exist, template deleted, invalid endpoint |
| 429 | Too Many Requests | Rate limit exceeded | Hourly or burst limit reached, concurrent execution limit |
| 500 | Internal Server Error | Server error | Temporary service issue, model unavailable, processing timeout |
| 503 | Service Unavailable | Maintenance mode | Scheduled maintenance, capacity limits, service degradation |

### Error types and resolution

**Authentication Errors (401)**

```json
{
  "error": {
    "type": "authentication_error",
    "code": "INVALID_API_KEY",
    "message": "The provided API key is invalid or expired"
  }
}
```

**Resolution Steps:**
- Verify API key format starts with `pe_sk_live_` or `pe_sk_test_`
- Check for extra spaces or characters in the Authorization header
- Regenerate API key if older than 90 days

**Validation Errors (400)**

```json
{
  "error": {
    "type": "validation_error", 
    "code": "TEMPLATE_VARIABLE_MISSING",
    "message": "Required template variable 'customer_name' not provided",
    "details": {
      "missing_variables": ["customer_name"],
      "provided_variables": ["message", "tone"]
    }
  }
}
```

**Resolution Steps:**
- Review template variable requirements using `GET /templates/{template_id}`
- Ensure all required variables are included in the request
- Validate variable data types match template specifications

**Rate Limiting Errors (429)**

```json
{
  "error": {
    "type": "rate_limit_error",
    "code": "HOURLY_LIMIT_EXCEEDED", 
    "message": "You have exceeded your hourly rate limit of 1,000 requests",
    "details": {
      "limit": 1000,
      "reset_time": "2024-01-15T10:00:00Z",
      "retry_after": 1847
    }
  }
}
```

**Resolution Steps:**
- Wait for the reset time indicated in `retry_after` (seconds)
- Implement exponential backoff in your application
- Consider upgrading to a higher plan for increased limits
- Use batch processing for multiple related operations

### Advanced error scenarios

**Model Execution Failures**

When the AI model fails to generate a response due to content policy violations or technical issues:

```json
{
  "error": {
    "type": "execution_error",
    "code": "CONTENT_POLICY_VIOLATION",
    "message": "The prompt violates content policy guidelines",
    "details": {
      "violation_type": "harmful_content",
      "suggestion": "Modify prompt to remove potentially harmful instructions"
    }
  }
}
```

**Template Processing Errors**

When template variables cannot be properly substituted:

```json
{
  "error": {
    "type": "template_error",
    "code": "VARIABLE_SUBSTITUTION_FAILED",
    "message": "Template variable substitution failed",
    "details": {
      "problematic_variables": ["date_range"],
      "template_section": "line 15: {{date_range.start}}"
    }
  }
}
```

### Error handling best practices

**Implement Retry Logic:**

```javascript
const executeWithRetry = async (payload, maxRetries = 3) => {
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      const response = await fetch('https://api.promptexecution.com/v2/execute', {
        method: 'POST',
        headers: {
          'Authorization': `Bearer ${apiKey}`,
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(payload)
      });
      
      if (response.status === 429) {
        // Rate limited - wait and retry
        const retryAfter = response.headers.get('Retry-After') || Math.pow(2, attempt);
        await new Promise(resolve => setTimeout(resolve, retryAfter * 1000));
        continue;
      }
      
      if (!response.ok) {
        const error = await response.json();
        throw new Error(`API Error: ${error.error.message}`);
      }
      
      return await response.json();
    } catch (error) {
      if (attempt === maxRetries) throw error;
      await new Promise(resolve => setTimeout(resolve, 1000 * attempt));
    }
  }
};
```

**Graceful Error Handling:**

- **Log detailed error information** including request_id for support escalation
- **Provide meaningful user feedback** without exposing sensitive error details
- **Implement fallback mechanisms** for critical application workflows
- **Monitor error patterns** to identify systematic issues requiring attention

---

## 6. Rate limiting

The Prompt Execution API implements tiered rate limiting to ensure fair usage and optimal performance across all users.

### Rate limit policies

**Request Limits by Plan:**

| Plan | Requests/Hour | Burst Limit | Concurrent Executions |
|------|---------------|-------------|----------------------|
| Basic | 1,000 | 100/minute | 5 |
| Professional | 10,000 | 500/minute | 50 |
| Enterprise | Unlimited | 2,000/minute | 500 |

**Rate Limit Headers:** Every response includes current usage information:

```http
X-RateLimit-Limit: 10000
X-RateLimit-Remaining: 9847
X-RateLimit-Reset: 1705312800
X-RateLimit-Burst-Limit: 500
X-RateLimit-Burst-Remaining: 487
```

### Rate limit monitoring

**Check Current Usage:**

```bash
curl -X GET "https://api.promptexecution.com/v2/rate-limits" \
  -H "Authorization: Bearer pe_sk_live_abc123def456ghi789"
```

**Response:**
```json
{
  "hourly": {
    "limit": 10000,
    "used": 153,
    "remaining": 9847,
    "reset_time": "2024-01-15T10:00:00Z"
  },
  "burst": {
    "limit": 500,
    "used": 13,
    "remaining": 487,
    "window": "1_minute"
  },
  "concurrent": {
    "limit": 50,
    "active": 3,
    "available": 47
  }
}
```

### Optimization strategies

**Batch Processing for Efficiency:**

Use the batch endpoint to process multiple prompts efficiently:

```javascript
// Instead of multiple individual requests
const results = await Promise.all(
  prompts.map(prompt => executeSinglePrompt(prompt))
);

// Use batch processing (more efficient)
const batchResult = await executeBatchPrompts({
  template_id: "analysis_template",
  inputs: prompts.map(prompt => ({
    id: prompt.id,
    variables: prompt.variables
  }))
});
```

**Request Grouping Strategies:**

- **Combine related operations** into single requests when possible
- **Use template variables** instead of separate template requests
- **Implement client-side caching** for template metadata and static responses
- **Schedule non-urgent requests** during off-peak hours

**Intelligent Retry Implementation:**

```python
import time
import random

def execute_with_backoff(payload, max_retries=5):
    """Execute API request with exponential backoff"""
    
    for attempt in range(max_retries):
        try:
            response = requests.post(
                "https://api.promptexecution.com/v2/execute",
                headers={"Authorization": f"Bearer {api_key}"},
                json=payload
            )
            
            if response.status_code == 429:
                # Rate limited - implement exponential backoff with jitter
                retry_after = int(response.headers.get('Retry-After', 60))
                jitter = random.uniform(0.1, 0.3) * retry_after
                sleep_time = min(retry_after + jitter, 300)  # Max 5 minutes
                
                print(f"Rate limited. Waiting {sleep_time:.1f} seconds...")
                time.sleep(sleep_time)
                continue
                
            response.raise_for_status()
            return response.json()
            
        except requests.exceptions.RequestException as e:
            if attempt == max_retries - 1:
                raise e
            
            # Exponential backoff for other errors
            wait_time = min(2 ** attempt + random.uniform(0, 1), 60)
            time.sleep(wait_time)
```

**Performance Monitoring:**

Track these metrics to optimize your API usage:

- **Average response time** per template and model configuration
- **Rate limit utilization** during peak usage periods
- **Batch processing efficiency** compared to individual requests
- **Error rates** by request type and time of day

---

## 7. Batch processing and workflows

The Prompt Execution API supports sophisticated batch processing and workflow automation for enterprise-scale operations.

### Batch execution

**Process Multiple Prompts Efficiently:**

```bash
curl -X POST "https://api.promptexecution.com/v2/batch" \
  -H "Authorization: Bearer pe_sk_live_abc123def456ghi789" \
  -H "Content-Type: application/json" \
  -d '{
    "template_id": "customer_sentiment_analysis",
    "inputs": [
      {
        "id": "feedback_001",
        "variables": {
          "feedback_text": "Excellent service, very satisfied!",
          "customer_segment": "premium"
        }
      },
      {
        "id": "feedback_002",
        "variables": {
          "feedback_text": "Product arrived damaged, poor packaging",
          "customer_segment": "standard"
        }
      }
    ],
    "model_config": {
      "temperature": 0.2,
      "max_tokens": 300
    },
    "processing_options": {
      "priority": "normal",
      "callback_url": "https://your-app.com/webhooks/batch-complete",
      "progress_updates": true
    }
  }'
```

**Batch Response:**
```json
{
  "batch_id": "batch_xyz789abc",
  "status": "queued",
  "total_items": 2,
  "estimated_completion": "2024-01-15T09:35:00Z",
  "progress_url": "https://api.promptexecution.com/v2/batch/batch_xyz789abc/progress",
  "results_url": "https://api.promptexecution.com/v2/batch/batch_xyz789abc/results"
}
```

### Workflow automation

**Chain Multiple Processing Steps:**

Enterprise plans support advanced workflow automation with conditional logic and error handling:

```json
{
  "workflow_name": "content_review_pipeline",
  "steps": [
    {
      "id": "content_generation",
      "type": "prompt_execution",
      "template_id": "blog_post_generator",
      "variables": {
        "topic": "{{workflow.input.topic}}",
        "target_length": 1500
      }
    },
    {
      "id": "quality_check",
      "type": "prompt_execution", 
      "template_id": "content_quality_reviewer",
      "variables": {
        "content": "{{steps.content_generation.output}}",
        "quality_criteria": ["readability", "accuracy", "engagement"]
      },
      "condition": "{{steps.content_generation.status}} == 'completed'"
    },
    {
      "id": "seo_optimization",
      "type": "prompt_execution",
      "template_id": "seo_optimizer",
      "variables": {
        "content": "{{steps.content_generation.output}}",
        "keywords": "{{workflow.input.seo_keywords}}"
      },
      "condition": "{{steps.quality_check.output.score}} >= 7"
    }
  ],
  "error_handling": {
    "retry_attempts": 2,
    "fallback_template": "simple_content_generator"
  },
  "notifications": {
    "webhook_url": "https://your-app.com/webhooks/workflow-complete",
    "email_alerts": ["content-team@company.com"]
  }
}
```

### Monitoring batch operations

**Check Batch Progress:**

```bash
curl -X GET "https://api.promptexecution.com/v2/batch/batch_xyz789abc/progress" \
  -H "Authorization: Bearer pe_sk_live_abc123def456ghi789"
```

**Progress Response:**
```json
{
  "batch_id": "batch_xyz789abc",
  "status": "processing",
  "progress": {
    "completed": 847,
    "failed": 12,
    "pending": 141,
    "total": 1000
  },
  "performance": {
    "average_execution_time": 2.3,
    "throughput_per_minute": 45,
    "estimated_completion": "2024-01-15T09:42:30Z"
  },
  "errors": [
    {
      "item_id": "feedback_156",
      "error_code": "CONTENT_POLICY_VIOLATION",
      "retry_eligible": false
    }
  ]
}
```

**Retrieve Batch Results:**

```bash
curl -X GET "https://api.promptexecution.com/v2/batch/batch_xyz789abc/results" \
  -H "Authorization: Bearer pe_sk_live_abc123def456ghi789"
```

**Results Response:**
```json
{
  "batch_id": "batch_xyz789abc",
  "status": "completed",
  "results": [
    {
      "item_id": "feedback_001",
      "status": "completed",
      "content": "**Sentiment Analysis:**\n- Overall sentiment: Positive\n- Confidence: 94%\n- Key themes: Service quality, satisfaction\n- Recommended action: Share positive feedback with service team",
      "metadata": {
        "tokens_used": 127,
        "execution_time": 1.8,
        "cost": 0.006
      }
    },
    {
      "item_id": "feedback_002", 
      "status": "completed",
      "content": "**Sentiment Analysis:**\n- Overall sentiment: Negative\n- Confidence: 89%\n- Key themes: Product damage, packaging issues\n- Recommended action: Immediate customer service follow-up required",
      "metadata": {
        "tokens_used": 143,
        "execution_time": 2.1,
        "cost": 0.007
      }
    }
  ],
  "summary": {
    "total_cost": 0.013,
    "total_tokens": 270,
    "average_execution_time": 1.95,
    "success_rate": 100
  }
}
```

### Webhook integration

**Configure Real-time Notifications:**

```javascript
// Webhook endpoint to receive batch completion notifications
app.post('/webhooks/batch-complete', (req, res) => {
  const { batch_id, status, summary } = req.body;
  
  if (status === 'completed') {
    console.log(`Batch ${batch_id} completed successfully`);
    console.log(`Processed ${summary.total_items} items`);
    console.log(`Total cost: ${summary.total_cost}`);
    
    // Process results or trigger next workflow step
    processBatchResults(batch_id);
  } else if (status === 'failed') {
    console.error(`Batch ${batch_id} failed`);
    // Implement error handling and notification
    notifyOperationsTeam(batch_id, summary.error_details);
  }
  
  res.status(200).send('Webhook received');
});
```

[Back to top](#prompt-execution-api-v21-documentation)

---

## 8. Performance and scaling

Optimize your API integration for high-performance, scalable operations with these proven strategies and monitoring approaches.

### Performance optimization strategies

**Connection Management:**

```python
import requests
from requests.adapters import HTTPAdapter
from requests.packages.urllib3.util.retry import Retry

class OptimizedAPIClient:
    def __init__(self, api_key):
        self.api_key = api_key
        self.base_url = "https://api.promptexecution.com/v2"
        
        # Configure connection pooling and retries
        self.session = requests.Session()
        
        # Retry strategy for transient failures
        retry_strategy = Retry(
            total=3,
            backoff_factor=1,
            status_forcelist=[429, 500, 502, 503, 504],
            allowed_methods=["GET", "POST"]
        )
        
        adapter = HTTPAdapter(
            pool_connections=20,  # Number of connection pools
            pool_maxsize=20,      # Connections per pool
            max_retries=retry_strategy
        )
        
        self.session.mount("https://", adapter)
        self.session.headers.update({
            'Authorization': f'Bearer {api_key}',
            'Content-Type': 'application/json',
            'Connection': 'keep-alive'
        })
    
    def execute_prompt(self, payload):
        """Execute prompt with optimized connection handling"""
        response = self.session.post(
            f"{self.base_url}/execute",
            json=payload,
            timeout=(5, 30)  # (connect timeout, read timeout)
        )
        response.raise_for_status()
        return response.json()
```

**Caching Strategies:**

```javascript
class PromptExecutionCache {
  constructor(ttl = 3600) { // 1 hour TTL
    this.cache = new Map();
    this.ttl = ttl * 1000; // Convert to milliseconds
  }
  
  generateCacheKey(templateId, variables) {
    // Create deterministic cache key
    const sortedVars = Object.keys(variables)
      .sort()
      .reduce((result, key) => {
        result[key] = variables[key];
        return result;
      }, {});
    
    return `${templateId}:${JSON.stringify(sortedVars)}`;
  }
  
  async executeWithCache(templateId, variables, apiCall) {
    const cacheKey = this.generateCacheKey(templateId, variables);
    const cached = this.cache.get(cacheKey);
    
    // Check if cached result is still valid
    if (cached && (Date.now() - cached.timestamp) < this.ttl) {
      console.log('Cache hit for:', cacheKey);
      return cached.result;
    }
    
    // Execute API call and cache result
    const result = await apiCall();
    this.cache.set(cacheKey, {
      result: result,
      timestamp: Date.now()
    });
    
    return result;
  }
  
  // Clean expired entries periodically
  cleanup() {
    const now = Date.now();
    for (const [key, value] of this.cache.entries()) {
      if (now - value.timestamp > this.ttl) {
        this.cache.delete(key);
      }
    }
  }
}

// Usage example
const cache = new PromptExecutionCache();
setInterval(() => cache.cleanup(), 300000); // Cleanup every 5 minutes

const result = await cache.executeWithCache(
  'customer_response_template',
  { message: 'How do I reset my password?', tone: 'helpful' },
  () => promptAPI.execute({
    template_id: 'customer_response_template',
    variables: { message: 'How do I reset my password?', tone: 'helpful' }
  })
);
```

### Scaling patterns

**Horizontal Scaling with Load Distribution:**

```javascript
class LoadBalancedAPIClient {
  constructor(apiKeys) {
    this.apiClients = apiKeys.map(key => new OptimizedAPIClient(key));
    this.currentIndex = 0;
  }
  
  getNextClient() {
    // Round-robin load balancing
    const client = this.apiClients[this.currentIndex];
    this.currentIndex = (this.currentIndex + 1) % this.apiClients.length;
    return client;
  }
  
  async executeWithLoadBalancing(payload) {
    const maxRetries = this.apiClients.length;
    
    for (let attempt = 0; attempt < maxRetries; attempt++) {
      const client = this.getNextClient();
      
      try {
        return await client.execute_prompt(payload);
      } catch (error) {
        if (error.status === 429) {
          // Rate limited on this key, try next one
          console.log(`Rate limited on client ${this.currentIndex}, trying next...`);
          continue;
        }
        throw error; // Re-throw non-rate-limit errors
      }
    }
    
    throw new Error('All API clients are rate limited');
  }
}
```

**Asynchronous Processing Patterns:**

```python
import asyncio
import aiohttp
from typing import List, Dict

class AsyncBatchProcessor:
    def __init__(self, api_key: str, max_concurrent: int = 10):
        self.api_key = api_key
        self.base_url = "https://api.promptexecution.com/v2"
        self.semaphore = asyncio.Semaphore(max_concurrent)
        
    async def execute_single(self, session: aiohttp.ClientSession, payload: Dict):
        """Execute single prompt with concurrency control"""
        async with self.semaphore:
            try:
                async with session.post(
                    f"{self.base_url}/execute",
                    json=payload,
                    headers={'Authorization': f'Bearer {self.api_key}'}
                ) as response:
                    result = await response.json()
                    return {
                        'success': True,
                        'payload': payload,
                        'result': result
                    }
            except Exception as e:
                return {
                    'success': False,
                    'payload': payload,
                    'error': str(e)
                }
    
    async def process_batch(self, payloads: List[Dict]):
        """Process multiple prompts concurrently"""
        connector = aiohttp.TCPConnector(
            limit=20,  # Total connection pool size
            limit_per_host=10,  # Connections per host
            keepalive_timeout=300
        )
        
        async with aiohttp.ClientSession(connector=connector) as session:
            tasks = [
                self.execute_single(session, payload) 
                for payload in payloads
            ]
            
            results = await asyncio.gather(*tasks, return_exceptions=True)
            
            # Separate successful and failed results
            successful = [r for r in results if r.get('success')]
            failed = [r for r in results if not r.get('success')]
            
            return {
                'successful': successful,
                'failed': failed,
                'success_rate': len(successful) / len(results)
            }

# Usage example
async def main():
    processor = AsyncBatchProcessor('pe_sk_live_abc123def456ghi789')
    
    payloads = [
        {
            'template_id': 'sentiment_analysis',
            'variables': {'text': review}
        }
        for review in customer_reviews
    ]
    
    results = await processor.process_batch(payloads)
    print(f"Processed {len(results['successful'])} items successfully")
```

### Monitoring and observability

**Performance Metrics Collection:**

```javascript
class APIPerformanceMonitor {
  constructor() {
    this.metrics = {
      requests: 0,
      successes: 0,
      failures: 0,
      totalLatency: 0,
      rateLimits: 0,
      costs: 0
    };
    
    this.latencyHistogram = [];
  }
  
  async executeWithMonitoring(apiCall, metadata = {}) {
    const startTime = Date.now();
    this.metrics.requests++;
    
    try {
      const result = await apiCall();
      const latency = Date.now() - startTime;
      
      // Record success metrics
      this.metrics.successes++;
      this.metrics.totalLatency += latency;
      this.metrics.costs += result.metadata?.cost || 0;
      this.latencyHistogram.push(latency);
      
      // Keep histogram manageable (last 1000 requests)
      if (this.latencyHistogram.length > 1000) {
        this.latencyHistogram.shift();
      }
      
      return result;
      
    } catch (error) {
      this.metrics.failures++;
      
      if (error.status === 429) {
        this.metrics.rateLimits++;
      }
      
      // Log error details for analysis
      console.error('API Error:', {
        error: error.message,
        latency: Date.now() - startTime,
        metadata: metadata,
        timestamp: new Date().toISOString()
      });
      
      throw error;
    }
  }
  
  getPerformanceReport() {
    const avgLatency = this.metrics.totalLatency / this.metrics.successes || 0;
    const successRate = (this.metrics.successes / this.metrics.requests) * 100;
    
    // Calculate latency percentiles
    const sortedLatencies = this.latencyHistogram.sort((a, b) => a - b);
    const p95 = sortedLatencies[Math.floor(sortedLatencies.length * 0.95)] || 0;
    const p99 = sortedLatencies[Math.floor(sortedLatencies.length * 0.99)] || 0;
    
    return {
      summary: {
        total_requests: this.metrics.requests,
        success_rate: `${successRate.toFixed(2)}%`,
        total_cost: `${this.metrics.costs.toFixed(4)}`,
        rate_limits_hit: this.metrics.rateLimits
      },
      performance: {
        average_latency: `${avgLatency.toFixed(0)}ms`,
        p95_latency: `${p95}ms`,
        p99_latency: `${p99}ms`,
        requests_per_minute: this.calculateRPM()
      }
    };
  }
  
  calculateRPM() {
    // Simplified RPM calculation based on recent requests
    return Math.round(this.latencyHistogram.length / 10); // Rough estimate
  }
}

// Usage with comprehensive monitoring
const monitor = new APIPerformanceMonitor();

const result = await monitor.executeWithMonitoring(
  () => api.execute({
    template_id: 'analysis_template',
    variables: { data: analysisData }
  }),
  { operation: 'data_analysis', batch_size: 100 }
);

// Generate performance reports
setInterval(() => {
  const report = monitor.getPerformanceReport();
  console.log('Performance Report:', report);
  
  // Send to monitoring system
  sendToMonitoring(report);
}, 60000); // Every minute
```

### Cost optimization

**Smart Model Selection:**

```javascript
class CostOptimizedExecutor {
  constructor(apiKey) {
    this.apiKey = apiKey;
    
    // Model cost per 1K tokens (example pricing)
    this.modelCosts = {
      'gpt-4-turbo': 0.03,
      'gpt-4': 0.06,
      'gpt-3.5-turbo': 0.002
    };
  }
  
  selectOptimalModel(payload) {
    const estimatedTokens = this.estimateTokens(payload);
    const complexity = this.assessComplexity(payload);
    
    // Smart model selection based on complexity and cost
    if (complexity === 'simple' && estimatedTokens < 1000) {
      return 'gpt-3.5-turbo';
    } else if (complexity === 'medium' || estimatedTokens < 2000) {
      return 'gpt-4-turbo';
    } else {
      return 'gpt-4'; // For complex tasks requiring highest quality
    }
  }
  
  estimateTokens(payload) {
    // Rough estimation: 1 token ≈ 4 characters
    const promptLength = JSON.stringify(payload.variables).length;
    const templateLength = 500; // Estimated template size
    return Math.ceil((promptLength + templateLength) / 4);
  }
  
  assessComplexity(payload) {
    // Analyze payload to determine complexity
    const indicators = {
      hasCodeGeneration: /code|function|class/.test(JSON.stringify(payload)),
      hasAnalysis: /analyze|evaluate|assess/.test(JSON.stringify(payload)),
      hasCreativeWriting: /write|create|compose/.test(JSON.stringify(payload)),
      longContent: this.estimateTokens(payload) > 1500
    };
    
    const complexityScore = Object.values(indicators).filter(Boolean).length;
    
    if (complexityScore >= 3) return 'complex';
    if (complexityScore >= 1) return 'medium';
    return 'simple';
  }
  
  async executeWithCostOptimization(payload) {
    const optimalModel = this.selectOptimalModel(payload);
    const estimatedCost = this.calculateEstimatedCost(payload, optimalModel);
    
    console.log(`Using ${optimalModel} - Estimated cost: ${estimatedCost.toFixed(4)}`);
    
    return await this.execute({
      ...payload,
      model_config: {
        ...payload.model_config,
        model: optimalModel
      }
    });
  }
  
  calculateEstimatedCost(payload, model) {
    const estimatedTokens = this.estimateTokens(payload);
    const costPer1K = this.modelCosts[model];
    return (estimatedTokens / 1000) * costPer1K;
  }
}
```

### Enterprise deployment patterns

**High Availability Configuration:**

```yaml
# Kubernetes deployment example for high-availability API integration
apiVersion: apps/v1
kind: Deployment
metadata:
  name: prompt-execution-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: prompt-execution-service
  template:
    metadata:
      labels:
        app: prompt-execution-service
    spec:
      containers:
      - name: api-client
        image: your-org/prompt-executor:latest
        env:
        - name: API_KEYS
          valueFrom:
            secretKeyRef:
              name: prompt-execution-secrets
              key: api-keys
        - name: CACHE_REDIS_URL
          value: "redis://redis-cluster:6379"
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 30
        readinessProbe:
          httpGet:
            path: /ready
            port: 8080
          initialDelaySeconds: 5
          periodSeconds: 10
```

This comprehensive API reference demonstrates enterprise-level technical writing capabilities, showcasing systematic documentation architecture, developer-focused content design, and scalable integration patterns. The documentation balances technical depth with practical usability, making it an ideal portfolio piece for demonstrating advanced API documentation expertise.

[Back to top](#prompt-execution-api-v21-documentation)

---

**📞 Support & Resources**

- **Developer Portal:** [https://developers.promptexecution.com](https://developers.promptexecution.com)
- **API Status:** [https://status.promptexecution.com](https://status.promptexecution.com)  
- **Community Forum:** [https://community.promptexecution.com](https://community.promptexecution.com)
- **Technical Support:** [support@promptexecution.com](mailto:support@promptexecution.com)

*Last updated: January 15, 2024 | Version 2.1 | [OpenAPI Specification](https://api.promptexecution.com/v2/openapi.json)*