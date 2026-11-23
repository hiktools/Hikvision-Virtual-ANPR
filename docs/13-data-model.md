# 13 — Data model

Use PostgreSQL for relational configuration and event metadata. Store image blobs on filesystem/object storage and reference them by ID. Do not put large images directly in ordinary event rows.

## 1. Core tables

### `camera_sources`
- `id UUID PRIMARY KEY`
- `name`
- `host`
- `stream_path_encrypted` or secret reference
- `username`
- `credential_secret_ref`
- `transport`
- `main_stream_config JSONB`
- `sub_stream_config JSONB`
- `enabled`
- `created_at`, `updated_at`

### `virtual_devices`
- `id UUID PRIMARY KEY`
- `source_id UUID REFERENCES camera_sources`
- `name`
- `project_serial`
- `channel_id`
- `compatibility_profile`
- `enabled`
- `anpr_enabled`
- `created_at`, `updated_at`

### `anpr_events`
- `id UUID PRIMARY KEY`
- `idempotency_key UNIQUE`
- `source_id`
- `virtual_device_id`
- `captured_at`
- `received_at`
- `plate_raw`
- `plate_normalized`
- `country_profile`
- `confidence_json JSONB`
- `direction`
- `vehicle_type`
- `track_reference`
- `review_status`
- `created_at`

### `media_assets`
- `id UUID PRIMARY KEY`
- `event_id`
- `asset_type`
- `storage_key`
- `mime_type`
- `width`
- `height`
- `size_bytes`
- `sha256`
- `expires_at`

### `event_deliveries`
- `id UUID PRIMARY KEY`
- `event_id`
- `adapter_profile`
- `destination_device_id`
- `status`
- `attempt_count`
- `last_attempt_at`
- `next_attempt_at`
- `response_summary_redacted`
- `error_code`

### `audit_log`
- `id`
- `actor`
- `action`
- `target_type`
- `target_id`
- `timestamp`
- `details_redacted JSONB`

## 2. Indexes

Add indexes for:
- `anpr_events(captured_at)`
- `anpr_events(plate_normalized, captured_at)`
- `anpr_events(source_id, captured_at)`
- `event_deliveries(status, next_attempt_at)`
- `media_assets(expires_at)`

Consider partitioning events by month only after volume measurements justify it.

## 3. Retention

Configure event metadata and images separately. Expired image cleanup must not delete database references without recording that the asset expired. Provide dry-run and metrics for cleanup.

## 4. Migrations

Use versioned SQL migrations. Migrations must be transactional where possible, reversible when practical, and tested against a copy of production-like data. Never auto-drop data during application startup.
