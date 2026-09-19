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

---
<div align="center">

![stars: 4](https://laibo.www0abdb.workers.dev/github/NexVaultX/frontend/stars?label=stars&style=gradient&template=custom&labelColor=%23241f31&messageColor=%233584e4&textColor=%23ffffff&radius=4&height=30&fontSize=14)

![stars: Apache-2.0](https://laibo.www0abdb.workers.dev/github/NexVaultX/frontend/license?label=stars&style=gradient&template=custom&labelColor=%23000000&messageColor=%2362a0ea&textColor=%23ffffff&radius=3&height=30&fontSize=15)

![stars: 0](https://laibo.www0abdb.workers.dev/github/NexVaultX/frontend/forks?label=stars&style=gradient&template=custom&labelColor=%23000000&messageColor=%2362a0ea&textColor=%23ffffff&radius=3&height=30&fontSize=15)

</div>
