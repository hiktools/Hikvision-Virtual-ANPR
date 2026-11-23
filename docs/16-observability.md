# 16 — Observability

## 1. Structured logging

Use JSON logs with:
- timestamp UTC
- severity
- service
- component
- camera/device ID
- trace/request ID
- event ID where relevant
- message
- error code

Never include credentials, full RTSP URLs with secrets, authentication headers, or unredacted tokens.

## 2. Metrics

### Stream
- `stream_connected`
- `stream_last_frame_timestamp`
- `stream_reconnect_total`
- `stream_input_bitrate`
- `stream_decode_errors_total`
- `stream_proxy_clients`

### Inference
- `inference_frames_total`
- `inference_queue_depth`
- `inference_latency_seconds`
- `vehicle_detections_total`
- `plate_candidates_total`
- `ocr_latency_seconds`
- `ocr_low_confidence_total`

### Events
- `anpr_events_created_total`
- `anpr_events_deduplicated_total`
- `event_delivery_attempts_total`
- `event_delivery_failures_total`
- `event_delivery_queue_depth`

### System
- CPU, memory, disk usage, database pool, open file descriptors, process restarts.

## 3. Health endpoints

Separate:
- Liveness: process is running.
- Readiness: database and essential services available.
- Per-camera health: source stream and virtual endpoint status.
- Integration health: NVR reachability and last successful compatibility test.

## 4. Diagnostics bundle

Allow an administrator to export a redacted diagnostic bundle containing configuration with secrets removed, software versions, health snapshots, relevant logs, and test results. Never include plate images or credentials unless explicitly selected with a warning.
