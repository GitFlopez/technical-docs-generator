# TasQueue — Example Output: Prompt 4 (Release Notes & Changelog)

**Input used:**
- Current version: v2.3.0 (minor release)
- Changes: new webhook retry logic, TypeScript 5.4 support, dashboard dark mode, fixed stalled job detection bug, deprecated `queue.process()` in favor of `new Worker()`

---

## SECTION 1: CHANGELOG.md

# Changelog

All notable changes to TasQueue are documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
TasQueue adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

---

## [2.3.0] - 2026-04-14

### Added
- Webhook retry logic: failed webhook deliveries now retry up to 5 times with exponential backoff (500ms base delay, doubles each attempt)
- Dashboard dark mode: auto-detects system preference; manual toggle in Settings → Appearance
- TypeScript 5.4 support: updated type definitions to leverage new infer variance annotations for stricter generics

### Changed
- `Worker` class now accepts an optional `concurrency` config at construction time, overriding queue-level defaults
- Dashboard job detail page now shows webhook delivery history (URL, status code, attempts)

### Deprecated
- `queue.process(name, fn)` is deprecated in favor of `new Worker(name, fn)`. Will be removed in v3.0.0. Migration: replace `queue.process('job-name', handler)` with `const w = new Worker('job-name', handler); w.start()`

### Fixed
- Fixed a bug where stalled job detection fired prematurely on jobs with `delay > stalledInterval`, causing healthy delayed jobs to be requeued as stalled
- Fixed dashboard queue depth graph incorrectly showing active jobs as waiting when concurrency limit was reached

---

## [2.2.1] - 2026-03-28

### Fixed
- Fixed Redis connection leak when `queue.close()` was called before all workers finished
- Fixed TypeScript error when passing `undefined` to optional `data` field in `queue.add()`
- Fixed dashboard 500 error when viewing jobs with null `completedAt` timestamps

---

## [2.2.0] - 2026-03-10

### Added
- Job progress streaming: workers can now call `job.progress(pct, message)` and callers can subscribe with `job.onProgress(callback)`
- `queue.schedule(cron, name, data)` for recurring jobs using standard cron expressions
- New `drain()` method: waits for all current jobs to complete before resolving

### Changed
- Minimum Redis version bumped from 5.0 to 6.0 (required for Streams support used in progress tracking)
- Worker error events now include the full job payload in the error callback for easier debugging

[2.3.0]: https://github.com/yourorg/tasqueue/compare/v2.2.1...v2.3.0
[2.2.1]: https://github.com/yourorg/tasqueue/compare/v2.2.0...v2.2.1
[2.2.0]: https://github.com/yourorg/tasqueue/compare/v2.1.0...v2.2.0
[Unreleased]: https://github.com/yourorg/tasqueue/compare/v2.3.0...HEAD

---

## SECTION 2: Release Announcement Blog Post

### TasQueue 2.3.0 is here — webhook retries, dark mode, and TypeScript 5.4

Shipping reliable background jobs is hard enough without worrying about whether your downstream webhook actually received the job completion event. TasQueue 2.3.0 fixes that with automatic webhook retry logic — plus a long-requested dashboard dark mode and TypeScript 5.4 support.

Here's what's new:

#### Webhook retry logic

Previously, if a webhook delivery failed (network error, 5xx from your server), TasQueue logged the failure and moved on. Starting in 2.3.0, failed webhooks automatically retry up to 5 times with exponential backoff (500ms, 1s, 2s, 4s, 8s). Each attempt is logged in the dashboard job detail view.

No configuration needed — retries are enabled by default for all webhook-configured jobs.

#### Dashboard dark mode

The dashboard now auto-detects your OS preference (`prefers-color-scheme`). You can also toggle manually under Settings → Appearance. Looks great on an external monitor at 2 AM.

#### TypeScript 5.4 support

Updated type definitions to use TypeScript 5.4's new infer variance annotations, giving you stricter generics and better IDE completion when working with typed job payloads.

#### Bug fix: stalled job false positives

If you configured a `delay` longer than your `stalledInterval`, jobs were incorrectly being requeued as stalled before they even started. This is fixed in 2.3.0.

**Upgrade:**

```bash
npm install tasqueue@2.3.0
```

Full release notes: [CHANGELOG.md](https://github.com/yourorg/tasqueue/blob/main/CHANGELOG.md) | [Docs](https://tasqueue.dev/docs) | [GitHub](https://github.com/yourorg/tasqueue)

---

## SECTION 3: Twitter/X Thread

**Tweet 1:**
🚀 TasQueue 2.3.0 is out! Webhook retries, dark mode, TS 5.4 support, and a stalled-job bug fix. Thread: 🧵

**Tweet 2:**
Webhook retry logic is now built in. Failed deliveries retry 5x with exponential backoff (500ms → 8s). Every attempt logged in the dashboard.

Zero config. Enabled by default for all webhook jobs.

**Tweet 3:**
Dashboard dark mode — finally. Auto-detects system pref, manual toggle in Settings. Full job detail now shows webhook delivery history per job.

**Tweet 4:**
Bug fix: if your `delay` was longer than `stalledInterval`, TasQueue was requeuing healthy delayed jobs as stalled. Fixed. Your delayed jobs are safe now.

Also: `queue.process()` is deprecated — use `new Worker()` instead (v3 removal).

**Tweet 5:**
Upgrade:
```
npm install tasqueue@2.3.0
```

Full changelog: github.com/yourorg/tasqueue/blob/main/CHANGELOG.md
Docs: tasqueue.dev/docs

Thanks for the bug reports — keep them coming.

---

## SECTION 4: Migration Notes

No breaking changes in 2.3.0.

**Deprecation warning:** `queue.process()` is deprecated. Before v3.0.0, migrate:

```typescript
// Before (deprecated in 2.3.0, removed in 3.0.0)
queue.process('send-email', async (job) => {
  await sendEmail(job.data);
});

// After (use Worker class)
import { Worker } from 'tasqueue';

const worker = new Worker('send-email', async (job) => {
  await sendEmail(job.data);
});
worker.start();
```

The new `Worker` class gives you explicit `start()` / `stop()` control and supports per-worker concurrency overrides — reasons the old `queue.process()` API couldn't support cleanly.
