# NexVaultX TODO

> **Status:** Early development — Rust backend daemon foundation
> **Last updated:** 2026-09-11

## Overview

NexVaultX is an asset management platform. The **frontend** is responsible for the web application and user experience; the **Rust backend daemon** is the central backend service for asset handling, search, security, storage, and database operations.

**Progress: 0 / 62 tasks complete**

| Area | Tasks | Done |
|------|------:|-----:|
| Core Responsibilities | 11 | 0 |
| Search | 12 | 0 |
| Malware & Security Scanning | 14 | 0 |
| Storage | 9 | 0 |
| Database | 9 | 0 |
| Asset Pipeline | 7 | 0 |
| **Total** | **62** | **0** |

## Current Focus

The first milestone is **not** to rebuild every existing backend feature. It is to establish the Rust daemon as the foundation for:

1. API
2. Asset management
3. Storage
4. Database access
5. Native Rust search
6. Malware scanning
7. Background processing

Everything else builds on top of this foundation.

---

## Rust Backend Daemon

Create a Rust backend daemon for NexVaultX that becomes the central backend service for asset handling, search, security, storage, and database operations.

### Core Responsibilities

- [ ] Create the Rust backend repository/package
- [ ] Design the daemon architecture
- [ ] Create the HTTP API used by the frontend
- [ ] Implement authentication and authorization integration
- [ ] Implement asset upload handling
- [ ] Implement asset downloading
- [ ] Implement asset/version management
- [ ] Implement S3-compatible object storage management
- [ ] Implement database access and management
- [ ] Implement background jobs/workers
- [ ] Implement asset processing and metadata extraction

### Search

Replace the current Meilisearch dependency with a native Rust search system.

- [ ] Design the NexVaultX search architecture
- [ ] Implement project/resource indexing
- [ ] Implement fuzzy search
- [ ] Implement typo tolerance
- [ ] Implement relevance scoring
- [ ] Implement tokenization and normalization
- [ ] Implement filtering
- [ ] Implement sorting
- [ ] Implement pagination
- [ ] Implement search result ranking
- [ ] Benchmark search performance against the current Meilisearch implementation
- [ ] Make the search index rebuildable from the primary database

> The database remains the source of truth. The search index is a derived, rebuildable data structure.

### Malware & Security Scanning

Uploaded assets must be treated as untrusted input.

- [ ] Design the asset security pipeline
- [ ] Validate uploaded file types
- [ ] Validate archive structures
- [ ] Detect path traversal attempts
- [ ] Detect archive bombs / decompression attacks
- [ ] Calculate file hashes
- [ ] Integrate malware scanning
- [ ] Implement static asset inspection
- [ ] Inspect JAR/plugin/mod contents
- [ ] Detect suspicious files and patterns
- [ ] Isolate scanning from the main API process
- [ ] Add resource limits for scanning jobs
- [ ] Prevent uploaded code from being executed by the backend
- [ ] Add a review/quarantine state for suspicious assets

### Storage

Support S3-compatible object storage without exposing storage credentials or bucket internals to the frontend.

- [ ] Implement S3-compatible storage abstraction
- [ ] Support presigned uploads
- [ ] Support presigned downloads
- [ ] Implement asset deletion
- [ ] Implement asset/version storage
- [ ] Implement storage cleanup
- [ ] Verify uploaded objects
- [ ] Store and verify content hashes
- [ ] Keep storage providers replaceable

**Potential storage targets:**

- AWS S3
- Cloudflare R2
- MinIO
- Other S3-compatible providers

### Database

- [ ] Define backend database architecture
- [ ] Implement database connection management
- [ ] Implement project/resource queries
- [ ] Implement version queries
- [ ] Implement user-related queries
- [ ] Implement download/statistics queries
- [ ] Implement transactions where required
- [ ] Add database migrations
- [ ] Ensure database failures are handled safely

### Asset Pipeline

The intended asset lifecycle roughly follows:

```text
Upload
  ↓
Validation
  ↓
Hashing
  ↓
Storage
  ↓
Security Scan
  ↓
Metadata Extraction
  ↓
Database Registration
  ↓
Search Indexing
  ↓
Available
```

- [ ] Design the complete asset lifecycle
- [ ] Implement asynchronous processing
- [ ] Implement job states
- [ ] Implement retry handling
- [ ] Implement failure handling
- [ ] Implement quarantine handling
- [ ] Ensure partially processed assets cannot appear as published

### Architecture

Keep the API and background processing logically separated.

```text
NexVaultX Frontend
        │
        ▼
   Rust Backend
        │
        ├── API
        ├── Authentication
        ├── Assets
        ├── Search
        ├── Storage
        ├── Database
        └── Jobs
              │
              ├── Malware Scanner
              ├── Asset Processor
              └── Search Indexer
```

The backend should be designed so expensive operations do not block normal API requests.

## Definition of Done — Initial Milestone

The initial milestone is complete when the Rust daemon provides:

- [ ] A working HTTP API
- [ ] Asset upload and download handling
- [ ] S3-compatible storage integration
- [ ] Database access with migrations
- [ ] A native Rust search index
- [ ] A malware scanning pipeline
- [ ] Background job processing
