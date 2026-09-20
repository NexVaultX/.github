# NexVaultX Architecture

## Overview

NexVaultX separates the frontend (web application and user experience) from the backend (Rust daemon handling all infrastructure-heavy operations). The backend is split into two logical layers: the **API** serving client requests and **Jobs** handling asynchronous background work.

## System Architecture

```text
NexVaultX Frontend
        │
        ▼
   Rust Backend
        │
        ├── API
        │     ├── Authentication
        │     ├── Assets
        │     ├── Search
        │     └── Database (queries)
        │
        └── Jobs
              ├── Malware Scanner
              ├── Asset Processor
              └── Search Indexer
```

## API Layer

The API layer serves all synchronous client requests. It should never run long-running or resource-intensive operations directly — those are delegated to the job system.

**Responsibilities:**
- Serve the HTTP API consumed by the frontend
- Handle authentication and authorization
- Accept asset uploads and return download URLs
- Query the database for projects, versions, users, and statistics
- Query the search index for results
- Enqueue background jobs

**Constraints:**
- Requests must complete quickly (no blocking on I/O-heavy work)
- Malware scanning, metadata extraction, and index updates happen asynchronously

## Job Layer

The job layer handles all expensive and asynchronous operations. Jobs run in isolated workers to prevent resource contention with the API.

**Job types:**
- **Malware Scanner** — validates files, detects threats, runs static inspection
- **Asset Processor** — extracts metadata, verifies content hashes, processes uploaded files
- **Search Indexer** — updates the search index after assets are registered or modified

**Job design principles:**
- Jobs have explicit states (queued, running, completed, failed)
- Failed jobs are retryable with configurable backoff
- Suspicious assets enter a quarantine state for manual review
- Partially processed assets never appear as published

## Storage

All storage operations go through an S3-compatible abstraction. The frontend never sees storage credentials or bucket internals — it receives presigned upload and download URLs.

```text
Frontend ──presigned URL──► S3-compatible storage
              (direct upload/download, bypasses backend)
```

**Supported providers:**
- AWS S3
- Cloudflare R2
- MinIO
- Other S3-compatible services

The storage provider must remain swappable without changing application logic.

## Database

The database is the **source of truth** for all asset, version, user, and statistics data. The search index is a derived, rebuildable structure — not a primary store.

**Key requirements:**
- Connection pooling and proper cleanup
- Transactions for multi-step writes
- Safe failure handling (no partial state visible to users)
- Database migrations for schema evolution
