# TasQueue — Example Output: Prompt 1 (README Generator)

**Input used:**
- Product: TasQueue — background task queue API for Node.js
- Audience: backend developers
- Stack: Node.js 18+, Redis, TypeScript
- Features: job scheduling, priority queues, retry logic, job progress tracking, webhook callbacks, dashboard UI

---

# TasQueue

[![Build Status](https://img.shields.io/github/actions/workflow/status/yourorg/tasqueue/ci.yml?branch=main)](https://github.com/yourorg/tasqueue/actions)
[![npm version](https://img.shields.io/npm/v/tasqueue)](https://www.npmjs.com/package/tasqueue)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Coverage](https://img.shields.io/codecov/c/github/yourorg/tasqueue)](https://codecov.io/gh/yourorg/tasqueue)
[![Downloads](https://img.shields.io/npm/dm/tasqueue)](https://www.npmjs.com/package/tasqueue)

**Reliable background job processing for Node.js — powered by Redis.**

TasQueue gives your Node.js application a production-ready task queue with priority scheduling, automatic retries, real-time progress tracking, and a built-in dashboard — in under 10 lines of code.

---

## Features

- **Priority Queues** — Assign jobs priority levels (1-10); critical jobs always run first
- **Automatic Retries** — Configurable retry count with exponential backoff; failed jobs go to a dead letter queue
- **Real-time Progress** — Track job completion percentage and custom status messages from worker to caller
- **Webhook Callbacks** — Fire HTTP callbacks on job completion, failure, or custom events
- **Dashboard UI** — Built-in web dashboard at `/tasqueue` — monitor queue depth, throughput, and failed jobs
- **TypeScript First** — Full type definitions included; zero `@types/` packages needed

---

## Installation

**Prerequisites:** Node.js 18+, Redis 6+

```bash
npm install tasqueue
```

**Environment setup:**

```env
TASQUEUE_REDIS_URL=redis://localhost:6379
TASQUEUE_DASHBOARD_PORT=4000
TASQUEUE_DASHBOARD_SECRET=your-secret-here
```

---

## Quick Start

```typescript
import { TasQueue, Worker } from 'tasqueue';

// Create queue
const queue = new TasQueue({ redisUrl: process.env.TASQUEUE_REDIS_URL });

// Add a job
const job = await queue.add('send-email', {
  to: 'user@example.com',
  subject: 'Welcome!',
  template: 'welcome'
}, { priority: 5, retries: 3 });

console.log(`Job ${job.id} queued`);

// Process jobs
const worker = new Worker('send-email', async (job) => {
  await sendEmail(job.data);
  await job.progress(100);
});

worker.start();
```

This example creates a queue, adds an email job with priority 5 and 3 retry attempts, then starts a worker that processes the jobs. The worker calls `job.progress(100)` to mark completion.

---

## Usage

### Adding Jobs

```typescript
// Basic job
await queue.add('resize-image', { imageId: '123', width: 800 });

// High-priority job with delay
await queue.add('send-alert', { userId: 'abc', message: 'Low disk space' }, {
  priority: 9,
  delay: 5000,       // start after 5 seconds
  retries: 5,
  retryDelay: 1000   // ms between retries (doubles each attempt)
});

// Scheduled (cron) job
await queue.schedule('0 9 * * *', 'daily-report', { reportType: 'summary' });
```

### Tracking Progress

```typescript
const worker = new Worker('process-video', async (job) => {
  await job.progress(10, 'Starting transcoding...');
  await transcode(job.data.videoId);
  await job.progress(80, 'Transcoding complete, uploading...');
  await upload(job.data.videoId);
  await job.progress(100, 'Done');
});

// From the producer side, subscribe to progress
const job = await queue.add('process-video', { videoId: 'xyz' });
job.onProgress((pct, message) => console.log(`${pct}% — ${message}`));
```

### Webhook Callbacks

```typescript
await queue.add('generate-report', { userId: '456' }, {
  webhook: {
    url: 'https://yourapp.com/webhooks/job-complete',
    events: ['completed', 'failed'],
    headers: { 'X-Secret': process.env.WEBHOOK_SECRET }
  }
});
```

---

## Configuration

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `redisUrl` | `string` | `redis://localhost:6379` | Redis connection URL |
| `prefix` | `string` | `tasqueue` | Redis key prefix (for multi-tenant setups) |
| `defaultRetries` | `number` | `3` | Default retry count for all jobs |
| `defaultPriority` | `number` | `5` | Default priority (1=low, 10=high) |
| `concurrency` | `number` | `5` | Max concurrent jobs per worker |
| `stalledInterval` | `number` | `30000` | Ms before a stalled job is requeued |
| `maxStalledCount` | `number` | `1` | Max times a job can stall before failing |
| `dashboardPort` | `number` | `4000` | Port for the dashboard UI |
| `dashboardSecret` | `string` | `undefined` | Basic auth password for dashboard |

---

## API Reference

See [full API docs](https://tasqueue.dev/docs/api) for complete reference.

**TasQueue class:**

| Method | Description |
|--------|-------------|
| `queue.add(name, data, options?)` | Add a job to the queue |
| `queue.schedule(cron, name, data)` | Schedule a recurring job |
| `queue.getJob(jobId)` | Retrieve a job by ID |
| `queue.pause()` | Pause the queue (stops accepting new jobs) |
| `queue.resume()` | Resume a paused queue |
| `queue.drain()` | Wait for all current jobs to complete |
| `queue.close()` | Gracefully close queue and Redis connection |

**Worker class:**

| Method | Description |
|--------|-------------|
| `worker.start()` | Begin processing jobs |
| `worker.stop()` | Stop processing after current jobs finish |
| `job.progress(pct, message?)` | Update job progress (0-100) |
| `job.log(message)` | Add a log entry to the job |

---

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Run tests: `npm test`
4. Ensure linting passes: `npm run lint`
5. Submit a pull request against `main`

**Code style:** ESLint + Prettier. Run `npm run format` before committing.

**Tests:** `npm test` runs unit tests. `npm run test:integration` runs against a local Redis instance (requires Docker).

---

## License

MIT — see [LICENSE](LICENSE) file.
