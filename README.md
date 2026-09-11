<div align="center">

# NexVaultX

### Open-source asset management, built for security and scale

NexVaultX is an open-source organization building a platform for hosting and distributing assets — from mods and plugins to any binary artifact.

</div>

---

## About NexVaultX

NexVaultX is a platform for secure, scalable asset management, split across focused repositories:

- **Frontend** — the web application and user experience
- **Rust backend daemon** — the central backend service for asset handling, search, security, storage, and database operations

Every uploaded asset is treated as **untrusted input**: files are validated, hashed, scanned for malware, and inspected before they can be published.

## Platform Features

- **Native Rust search** — fast, typo-tolerant fuzzy search with relevance scoring, filtering, sorting, and pagination. No external search engine dependency.
- **Malware & security scanning** — file type validation, archive inspection, path traversal and archive-bomb detection, static analysis, and a quarantine state for suspicious assets.
- **S3-compatible storage** — AWS S3, Cloudflare R2, MinIO, and other providers via presigned uploads and downloads. Storage credentials never reach the frontend.
- **Asset versioning** — full lifecycle management for assets and their versions.
- **Background processing** — an asynchronous asset pipeline with job states, retries, and failure handling that keeps expensive work off the API hot path.
- **Database-first design** — the database is the source of truth; search indexes are derived, rebuildable structures.

## Architecture

```text
NexVaultX Frontend (web app)
        │
        ▼
   Rust Backend Daemon
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

## Repositories

| Repository | Description |
|------------|-------------|
| `.github` | Organization profile and meta |

*More repositories coming soon.*

## Roadmap

NexVaultX is in early development. The current milestone is establishing the Rust backend daemon as the foundation for the API, asset management, storage, database access, native search, malware scanning, and background processing.

📋 [View the full roadmap & TODO →](assets/TODO.md)

---

*NexVaultX — secure asset management, built in the open.*