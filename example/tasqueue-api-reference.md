# TasQueue — Example Output: Prompt 2 (API Reference Docs)

**Input used:**
- API: TasQueue REST Management API
- Base URL: https://api.tasqueue.dev/v1
- Auth: Bearer token (API key)
- Endpoints: POST /jobs, GET /jobs/:id, DELETE /jobs/:id, GET /queues, POST /queues/:name/pause

---

# TasQueue API Reference

TasQueue provides a REST API for managing jobs, queues, and workers programmatically — useful for dashboards, monitoring integrations, and cross-service job management.

**Base URL:** `https://api.tasqueue.dev/v1`  
**API Version:** `2.3`  
**Protocol:** HTTPS only

---

## Authentication

All API requests require a Bearer token in the `Authorization` header.

**Get your API key:** Dashboard → Settings → API Keys → Generate New Key

```bash
curl https://api.tasqueue.dev/v1/queues \
  -H "Authorization: Bearer sk_live_your_api_key_here"
```

> **Warning:** Never commit API keys to source control. Use environment variables (`TASQUEUE_API_KEY`).

**API keys are scoped:**
- `sk_live_` — production keys (full access)
- `sk_test_` — sandbox keys (isolated test environment)

---

## Request Format

All requests and responses use `application/json`.

**Required headers:**

| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | Yes | `Bearer sk_[live\|test]_your_key` |
| `Content-Type` | Yes (POST/PATCH) | `application/json` |
| `Idempotency-Key` | No | UUID to safely retry POST requests |

---

## Endpoints

### POST /jobs

Enqueue a new background job.

**Request Body:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `queue` | string | Yes | Queue name to add the job to |
| `name` | string | Yes | Job type identifier (must match a registered worker) |
| `data` | object | Yes | Arbitrary JSON payload passed to the worker |
| `priority` | integer | No | 1 (low) to 10 (high). Default: 5 |
| `delay` | integer | No | Milliseconds to wait before processing. Default: 0 |
| `retries` | integer | No | Max retry attempts on failure. Default: 3 |
| `retryDelay` | integer | No | Base ms between retries (doubles each attempt). Default: 1000 |
| `webhook` | object | No | Webhook config (see Webhook Object below) |
| `ttl` | integer | No | Job expires after this many ms if not started. Default: none |

**Webhook Object:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `url` | string | Yes | HTTPS URL to POST job events to |
| `events` | string[] | Yes | `["completed", "failed", "progress"]` |
| `headers` | object | No | Custom headers sent with webhook requests |

**Request Example:**

```json
POST /v1/jobs
{
  "queue": "email",
  "name": "send-welcome",
  "data": {
    "userId": "usr_abc123",
    "email": "user@example.com",
    "template": "welcome-v2"
  },
  "priority": 7,
  "retries": 3,
  "webhook": {
    "url": "https://yourapp.com/webhooks/tasqueue",
    "events": ["completed", "failed"],
    "headers": {
      "X-Webhook-Secret": "whsec_your_secret"
    }
  }
}
```

**Response (201 Created):**

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Unique job ID (`job_` prefix) |
| `queue` | string | Queue name |
| `name` | string | Job type |
| `status` | string | `waiting` \| `active` \| `completed` \| `failed` \| `delayed` |
| `priority` | integer | Job priority |
| `createdAt` | string | ISO 8601 timestamp |
| `processAt` | string | ISO 8601 timestamp when job will be processed |

```json
{
  "id": "job_7k3m2nxp",
  "queue": "email",
  "name": "send-welcome",
  "status": "waiting",
  "priority": 7,
  "createdAt": "2026-04-14T11:00:00.000Z",
  "processAt": "2026-04-14T11:00:00.000Z"
}
```

**cURL Example:**

```bash
curl -X POST https://api.tasqueue.dev/v1/jobs \
  -H "Authorization: Bearer sk_test_example" \
  -H "Content-Type: application/json" \
  -d '{
    "queue": "email",
    "name": "send-welcome",
    "data": { "userId": "usr_abc123" },
    "priority": 7
  }'
```

**JavaScript SDK Example:**

```javascript
import { TasQueueClient } from 'tasqueue/client';

const client = new TasQueueClient({ apiKey: process.env.TASQUEUE_API_KEY });

const job = await client.jobs.create({
  queue: 'email',
  name: 'send-welcome',
  data: { userId: 'usr_abc123' },
  priority: 7
});

console.log(`Created job: ${job.id}`);
```

---

### GET /jobs/:id

Retrieve a job's current status and metadata.

**Path Parameters:**

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string | Yes | Job ID (e.g., `job_7k3m2nxp`) |

**Response (200 OK):**

```json
{
  "id": "job_7k3m2nxp",
  "queue": "email",
  "name": "send-welcome",
  "status": "completed",
  "priority": 7,
  "progress": 100,
  "data": { "userId": "usr_abc123" },
  "result": { "messageId": "msg_xyz789", "deliveredAt": "2026-04-14T11:00:03.442Z" },
  "attempts": 1,
  "createdAt": "2026-04-14T11:00:00.000Z",
  "startedAt": "2026-04-14T11:00:00.812Z",
  "completedAt": "2026-04-14T11:00:03.445Z",
  "logs": [
    { "ts": "2026-04-14T11:00:01.200Z", "message": "Rendering template welcome-v2" },
    { "ts": "2026-04-14T11:00:02.800Z", "message": "Email queued with provider" }
  ]
}
```

**cURL Example:**

```bash
curl https://api.tasqueue.dev/v1/jobs/job_7k3m2nxp \
  -H "Authorization: Bearer sk_test_example"
```

---

### DELETE /jobs/:id

Cancel a waiting or delayed job. Cannot cancel active (in-progress) jobs.

**Path Parameters:**

| Param | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string | Yes | Job ID to cancel |

**Response (200 OK):**

```json
{ "id": "job_7k3m2nxp", "cancelled": true }
```

**Error (409):** Returns `{"error": "job_not_cancellable", "message": "Job is already active or completed"}` if job cannot be cancelled.

---

### GET /queues

List all queues with current depth and throughput stats.

**Query Parameters:**

| Param | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `limit` | integer | No | 20 | Max queues to return (1-100) |
| `cursor` | string | No | — | Pagination cursor from previous response |

**Response (200 OK):**

```json
{
  "queues": [
    {
      "name": "email",
      "status": "active",
      "waiting": 4,
      "active": 2,
      "completed": 15420,
      "failed": 12,
      "throughput": { "last1m": 18, "last5m": 92, "last1h": 1104 }
    },
    {
      "name": "video-processing",
      "status": "paused",
      "waiting": 0,
      "active": 0,
      "completed": 330,
      "failed": 7,
      "throughput": { "last1m": 0, "last5m": 0, "last1h": 14 }
    }
  ],
  "nextCursor": null,
  "hasMore": false
}
```

---

### POST /queues/:name/pause

Pause a queue. Workers stop picking up new jobs; in-progress jobs complete normally.

**Response (200 OK):**

```json
{ "name": "email", "status": "paused", "pausedAt": "2026-04-14T11:15:00.000Z" }
```

Use `POST /queues/:name/resume` to unpause.

---

## Error Codes

| Code | HTTP Status | Meaning | Resolution |
|------|-------------|---------|------------|
| `unauthorized` | 401 | Invalid or missing API key | Check your `Authorization` header |
| `forbidden` | 403 | API key doesn't have permission for this action | Use a key with the correct scope |
| `not_found` | 404 | Job or queue not found | Verify the ID/name exists |
| `conflict` | 409 | Action not allowed in current state | Check job/queue status before retrying |
| `validation_error` | 422 | Request body failed validation | Check the `errors` array in the response |
| `rate_limited` | 429 | Too many requests | Retry after `Retry-After` header value (seconds) |
| `internal_error` | 500 | Server error | Retry with exponential backoff; contact support if persists |

**Error response format:**

```json
{
  "error": "validation_error",
  "message": "Request validation failed",
  "errors": [
    { "field": "priority", "message": "Must be between 1 and 10" }
  ]
}
```

---

## Rate Limiting

| Plan | Limit |
|------|-------|
| Free | 60 requests/minute |
| Growth | 500 requests/minute |
| Enterprise | Custom |

**Rate limit headers on every response:**

```
X-RateLimit-Limit: 500
X-RateLimit-Remaining: 487
X-RateLimit-Reset: 1713094860
```

**Retry with exponential backoff (JavaScript):**

```javascript
async function apiWithRetry(fn, maxAttempts = 4) {
  for (let attempt = 0; attempt < maxAttempts; attempt++) {
    try {
      return await fn();
    } catch (err) {
      if (err.status !== 429 || attempt === maxAttempts - 1) throw err;
      const delay = Math.pow(2, attempt) * 1000 + Math.random() * 500;
      await new Promise(resolve => setTimeout(resolve, delay));
    }
  }
}
```

---

## SDK Quick-Start

**JavaScript:**

```javascript
import { TasQueueClient } from 'tasqueue/client';

const client = new TasQueueClient({ apiKey: process.env.TASQUEUE_API_KEY });
const job = await client.jobs.create({ queue: 'email', name: 'send-welcome', data: { userId: '123' } });
console.log(job.id); // job_7k3m2nxp
```

**Python:**

```python
from tasqueue import TasQueueClient
import os

client = TasQueueClient(api_key=os.environ['TASQUEUE_API_KEY'])
job = client.jobs.create(queue='email', name='send-welcome', data={'user_id': '123'})
print(job.id)  # job_7k3m2nxp
```

**cURL:**

```bash
curl -X POST https://api.tasqueue.dev/v1/jobs \
  -H "Authorization: Bearer $TASQUEUE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"queue":"email","name":"send-welcome","data":{"userId":"123"}}'
```
