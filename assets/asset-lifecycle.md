# Asset Lifecycle

The asset pipeline defines the stages an uploaded file passes through before becoming publicly available.

## Lifecycle Stages

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

## Stage Descriptions

### 1. Upload

The frontend uploads an asset file. The backend accepts the upload and places it in a staging area for processing.

**Requirements:**
- Accept file uploads via presigned URLs or direct upload
- Enforce file size limits
- Track upload state separately from published state

### 2. Validation

The backend validates the uploaded file before any further processing.

**Checks performed:**
- File type matches declared type (magic bytes, not just extension)
- Archive structure is valid (for ZIP, JAR, and similar formats)
- No path traversal attempts in file names
- No archive bombs or decompression attacks detected

**Failure behavior:** Invalid uploads are rejected immediately. No further processing occurs.

### 3. Hashing

Calculate cryptographic hashes for the uploaded file.

**Purpose:**
- Content integrity verification
- Deduplication detection
- Audit trail

**Implementation:**
- Compute a full-file hash (e.g., SHA-256)
- Store the hash alongside the asset record

### 4. Storage

Store the validated file in S3-compatible object storage.

**Requirements:**
- Upload to a non-public location
- Use presigned URLs for direct client access
- Verify the uploaded object matches the source hash
- No storage credentials exposed to the frontend

### 5. Security Scan

Scan the stored file for malware and suspicious content.

**Checks performed:**
- Malware signature scanning
- Static analysis of JAR/plugin/mod contents
- Detection of suspicious files and patterns
- Resource-intensive inspection isolated from the API process

**Failure behavior:** Suspicious assets enter a **quarantine** state. They are not published or searchable until reviewed.

**Resource limits:**
- Scanning jobs run in isolated workers
- Time and memory limits enforced per scan
- Uploaded code is never executed by the backend

### 6. Metadata Extraction

Extract metadata from the processed asset.

**Examples:**
- File type and size
- Archive contents (for ZIP/JAR files)
- Mod or plugin metadata (if applicable)
- Version information
- Dependencies (if detectable)

### 7. Database Registration

Register the asset in the database as a complete, verified record.

**Requirements:**
- Write asset metadata, file hash, and storage location to the database
- Link the asset to its project and version
- Ensure the asset is marked as **not yet available** until all subsequent steps complete

### 8. Search Indexing

Update the search index with the new or modified asset.

**Requirements:**
- Index is derived from the database (database is source of truth)
- Index is rebuildable from scratch if needed
- Index updates happen asynchronously and do not block the API
- Search is not available for assets that have not been indexed

### 9. Available

The asset is fully processed, registered, indexed, and publicly discoverable.

**Requirements:**
- All previous stages must have completed successfully
- Partially processed assets must never appear as published
- Quarantined assets must never reach this state

## Error Handling

| Scenario | Behavior |
|----------|----------|
| Invalid file | Reject upload, no further processing |
| Hash mismatch | Reject asset, flag for investigation |
| Storage failure | Retry upload, fail job after retries exhausted |
| Scan finds malware | Move to quarantine, notify administrators |
| Scan finds suspicious patterns | Move to quarantine for manual review |
| Metadata extraction fails | Log error, mark asset as incomplete |
| Database write fails | Retry transaction, fail job after retries exhausted |
| Index update fails | Retry indexing, asset remains searchable from database until rebuilt |

## Key Principle

**Partially processed assets must never appear as published.** An asset becomes available only after every stage in the pipeline has completed successfully.