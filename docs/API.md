# API Documentation

## Overview

This document provides comprehensive API documentation for all public functions, classes, and methods in the codebase.

## Table of Contents

- [Authentication](#authentication)
- [Core API](#core-api)
- [Data Management](#data-management)
- [Utility Functions](#utility-functions)
- [Error Handling](#error-handling)
- [Rate Limiting](#rate-limiting)
- [Webhooks](#webhooks)

## Authentication

### API Keys

All API requests require authentication using an API key passed in the request header:

```bash
Authorization: Bearer YOUR_API_KEY
```

### JWT Tokens

For user-specific operations, JWT tokens are required:

```bash
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

## Core API

### Base URL

```
Production: https://api.example.com/v1
Staging: https://staging-api.example.com/v1
Development: http://localhost:3000/v1
```

### HTTP Status Codes

- `200` - Success
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `429` - Rate Limited
- `500` - Internal Server Error

## Data Management

### Create Resource

**Endpoint:** `POST /resources`

**Request Body:**
```json
{
  "name": "Example Resource",
  "description": "A sample resource",
  "metadata": {
    "category": "sample",
    "tags": ["example", "demo"]
  }
}
```

**Response:**
```json
{
  "id": "res_123456789",
  "name": "Example Resource",
  "description": "A sample resource",
  "metadata": {
    "category": "sample",
    "tags": ["example", "demo"]
  },
  "created_at": "2024-01-15T10:30:00Z",
  "updated_at": "2024-01-15T10:30:00Z"
}
```

**Example Usage:**
```javascript
const response = await fetch('/v1/resources', {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${apiKey}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    name: 'Example Resource',
    description: 'A sample resource',
    metadata: {
      category: 'sample',
      tags: ['example', 'demo']
    }
  })
});

const resource = await response.json();
```

### Get Resource

**Endpoint:** `GET /resources/{id}`

**Response:**
```json
{
  "id": "res_123456789",
  "name": "Example Resource",
  "description": "A sample resource",
  "metadata": {
    "category": "sample",
    "tags": ["example", "demo"]
  },
  "created_at": "2024-01-15T10:30:00Z",
  "updated_at": "2024-01-15T10:30:00Z"
}
```

**Example Usage:**
```javascript
const response = await fetch(`/v1/resources/${resourceId}`, {
  headers: {
    'Authorization': `Bearer ${apiKey}`
  }
});

const resource = await response.json();
```

### List Resources

**Endpoint:** `GET /resources`

**Query Parameters:**
- `page` (optional): Page number (default: 1)
- `limit` (optional): Items per page (default: 20, max: 100)
- `sort` (optional): Sort field (default: created_at)
- `order` (optional): Sort order (asc/desc, default: desc)
- `filter` (optional): JSON filter object

**Response:**
```json
{
  "data": [
    {
      "id": "res_123456789",
      "name": "Example Resource",
      "description": "A sample resource",
      "created_at": "2024-01-15T10:30:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 1,
    "pages": 1
  }
}
```

**Example Usage:**
```javascript
const params = new URLSearchParams({
  page: '1',
  limit: '20',
  sort: 'created_at',
  order: 'desc'
});

const response = await fetch(`/v1/resources?${params}`, {
  headers: {
    'Authorization': `Bearer ${apiKey}`
  }
});

const result = await response.json();
```

### Update Resource

**Endpoint:** `PUT /resources/{id}`

**Request Body:**
```json
{
  "name": "Updated Resource Name",
  "description": "Updated description"
}
```

**Response:**
```json
{
  "id": "res_123456789",
  "name": "Updated Resource Name",
  "description": "Updated description",
  "metadata": {
    "category": "sample",
    "tags": ["example", "demo"]
  },
  "created_at": "2024-01-15T10:30:00Z",
  "updated_at": "2024-01-15T11:00:00Z"
}
```

**Example Usage:**
```javascript
const response = await fetch(`/v1/resources/${resourceId}`, {
  method: 'PUT',
  headers: {
    'Authorization': `Bearer ${apiKey}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    name: 'Updated Resource Name',
    description: 'Updated description'
  })
});

const updatedResource = await response.json();
```

### Delete Resource

**Endpoint:** `DELETE /resources/{id}`

**Response:** `204 No Content`

**Example Usage:**
```javascript
const response = await fetch(`/v1/resources/${resourceId}`, {
  method: 'DELETE',
  headers: {
    'Authorization': `Bearer ${apiKey}`
  }
});

if (response.status === 204) {
  console.log('Resource deleted successfully');
}
```

## Utility Functions

### Data Validation

#### `validateEmail(email: string): boolean`

Validates email format.

**Parameters:**
- `email`: Email string to validate

**Returns:** `boolean` - True if valid email format

**Example:**
```javascript
import { validateEmail } from './utils/validation';

const isValid = validateEmail('user@example.com'); // true
const isInvalid = validateEmail('invalid-email'); // false
```

#### `validatePhone(phone: string): boolean`

Validates phone number format.

**Parameters:**
- `phone`: Phone string to validate

**Returns:** `boolean` - True if valid phone format

**Example:**
```javascript
import { validatePhone } from './utils/validation';

const isValid = validatePhone('+1-555-123-4567'); // true
const isInvalid = validatePhone('123'); // false
```

### Data Formatting

#### `formatCurrency(amount: number, currency: string): string`

Formats number as currency.

**Parameters:**
- `amount`: Numeric amount
- `currency`: Currency code (USD, EUR, etc.)

**Returns:** `string` - Formatted currency string

**Example:**
```javascript
import { formatCurrency } from './utils/formatting';

const formatted = formatCurrency(1234.56, 'USD'); // "$1,234.56"
const euroFormatted = formatCurrency(1234.56, 'EUR'); // "€1.234,56"
```

#### `formatDate(date: Date, format: string): string`

Formats date according to specified format.

**Parameters:**
- `date`: Date object or date string
- `format`: Format string (e.g., 'YYYY-MM-DD', 'MM/DD/YYYY')

**Returns:** `string` - Formatted date string

**Example:**
```javascript
import { formatDate } from './utils/formatting';

const date = new Date('2024-01-15');
const formatted = formatDate(date, 'YYYY-MM-DD'); // "2024-01-15"
const usFormatted = formatDate(date, 'MM/DD/YYYY'); // "01/15/2024"
```

### Data Transformation

#### `transformData(data: any, schema: Schema): any`

Transforms data according to schema definition.

**Parameters:**
- `data`: Raw data to transform
- `schema`: Transformation schema

**Returns:** `any` - Transformed data

**Example:**
```javascript
import { transformData } from './utils/transformation';

const schema = {
  id: 'user_id',
  name: 'full_name',
  email: 'email_address'
};

const rawData = {
  user_id: 123,
  full_name: 'John Doe',
  email_address: 'john@example.com'
};

const transformed = transformData(rawData, schema);
// Result: { id: 123, name: 'John Doe', email: 'john@example.com' }
```

## Error Handling

### Error Response Format

All API errors follow a consistent format:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input provided",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      }
    ],
    "request_id": "req_123456789"
  }
}
```

### Common Error Codes

- `VALIDATION_ERROR` - Input validation failed
- `AUTHENTICATION_ERROR` - Invalid or missing authentication
- `AUTHORIZATION_ERROR` - Insufficient permissions
- `NOT_FOUND_ERROR` - Resource not found
- `RATE_LIMIT_ERROR` - Too many requests
- `INTERNAL_ERROR` - Server error

### Error Handling Example

```javascript
async function handleApiRequest(url, options) {
  try {
    const response = await fetch(url, options);
    
    if (!response.ok) {
      const errorData = await response.json();
      throw new ApiError(errorData.error);
    }
    
    return await response.json();
  } catch (error) {
    if (error instanceof ApiError) {
      console.error(`API Error: ${error.code} - ${error.message}`);
      
      if (error.code === 'RATE_LIMIT_ERROR') {
        // Implement retry logic with exponential backoff
        await delay(1000);
        return handleApiRequest(url, options);
      }
    }
    
    throw error;
  }
}
```

## Rate Limiting

### Rate Limit Headers

API responses include rate limit information in headers:

```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1642248000
```

### Rate Limit Exceeded

When rate limit is exceeded, the API returns `429 Too Many Requests`:

```json
{
  "error": {
    "code": "RATE_LIMIT_ERROR",
    "message": "Rate limit exceeded",
    "retry_after": 60
  }
}
```

### Rate Limit Handling

```javascript
class RateLimitHandler {
  constructor() {
    this.requests = [];
  }
  
  async makeRequest(fn) {
    const now = Date.now();
    
    // Clean old requests
    this.requests = this.requests.filter(time => now - time < 60000);
    
    if (this.requests.length >= 1000) {
      const oldestRequest = Math.min(...this.requests);
      const waitTime = 60000 - (now - oldestRequest);
      await this.delay(waitTime);
    }
    
    this.requests.push(now);
    return fn();
  }
  
  delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }
}
```

## Webhooks

### Webhook Configuration

Configure webhooks to receive real-time updates:

**Endpoint:** `POST /webhooks`

**Request Body:**
```json
{
  "url": "https://your-app.com/webhook",
  "events": ["resource.created", "resource.updated"],
  "secret": "your_webhook_secret"
}
```

### Webhook Payload

Webhook payloads include event information:

```json
{
  "event": "resource.created",
  "timestamp": "2024-01-15T10:30:00Z",
  "data": {
    "id": "res_123456789",
    "name": "Example Resource"
  },
  "signature": "sha256=..."
}
```

### Webhook Verification

Verify webhook authenticity using the signature:

```javascript
import crypto from 'crypto';

function verifyWebhook(payload, signature, secret) {
  const expectedSignature = `sha256=${crypto
    .createHmac('sha256', secret)
    .update(JSON.stringify(payload))
    .digest('hex')}`;
    
  return crypto.timingSafeEqual(
    Buffer.from(signature),
    Buffer.from(expectedSignature)
  );
}
```

## SDK Examples

### JavaScript/Node.js

```javascript
import { ApiClient } from '@example/sdk';

const client = new ApiClient({
  apiKey: 'your_api_key',
  baseUrl: 'https://api.example.com/v1'
});

// Create resource
const resource = await client.resources.create({
  name: 'Example',
  description: 'Description'
});

// List resources
const resources = await client.resources.list({
  page: 1,
  limit: 20
});

// Update resource
const updated = await client.resources.update(resource.id, {
  name: 'Updated Name'
});

// Delete resource
await client.resources.delete(resource.id);
```

### Python

```python
from example_sdk import ApiClient

client = ApiClient(
    api_key='your_api_key',
    base_url='https://api.example.com/v1'
)

# Create resource
resource = client.resources.create(
    name='Example',
    description='Description'
)

# List resources
resources = client.resources.list(
    page=1,
    limit=20
)

# Update resource
updated = client.resources.update(
    resource.id,
    name='Updated Name'
)

# Delete resource
client.resources.delete(resource.id)
```

### PHP

```php
use Example\Sdk\ApiClient;

$client = new ApiClient([
    'api_key' => 'your_api_key',
    'base_url' => 'https://api.example.com/v1'
]);

// Create resource
$resource = $client->resources->create([
    'name' => 'Example',
    'description' => 'Description'
]);

// List resources
$resources = $client->resources->list([
    'page' => 1,
    'limit' => 20
]);

// Update resource
$updated = $client->resources->update($resource->id, [
    'name' => 'Updated Name'
]);

// Delete resource
$client->resources->delete($resource->id);
```