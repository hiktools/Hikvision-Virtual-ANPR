# 11 — ANPR events and images

## 1. Canonical event

The internal event is vendor-neutral. Example:

```json
{
  "event_id": "uuid",
  "camera_id": "uuid",
  "virtual_device_id": "uuid",
  "captured_at": "2026-01-01T12:00:00Z",
  "received_at": "2026-01-01T12:00:00Z",
  "plate_raw": "AB12CDE",
  "plate_normalized": "AB12CDE",
  "country_profile": "GB",
  "vehicle_type": "unknown",
  "direction": "unknown",
  "confidence": {
    "vehicle_detection": 0.98,
    "plate_detection": 0.96,
    "ocr": 0.93,
    "temporal_consensus": 0.97
  },
  "model_versions": {
    "vehicle_detector": "version",
    "plate_detector": "version",
    "ocr": "version"
  },
  "images": {
    "plate_crop": "asset-id",
    "vehicle_context": "asset-id"
  },
  "delivery": []
}
```

This is an application schema, not a Hikvision wire payload.

## 2. Event lifecycle

Candidate → validated → deduplicated/grouped → persisted → adapter encoded → delivery pending → delivered/failed/retrying.

Persist before attempting network delivery. Use idempotency keys so retries do not create multiple internal records.

## 3. Image assets

Store at least:
- Plate crop, with enough context for review.
- Vehicle/context image where available.
- Optional full frame only if justified by storage and privacy policy.

Record MIME type, dimensions, checksum, creation time, camera ID, and retention expiry. Validate image size and content. Never trust user-supplied filenames or MIME headers.

## 4. Vendor adapter

The adapter converts canonical events into a tested protocol representation. It owns:
- XML schema/version.
- Event type and state fields.
- Channel mapping.
- Date/time format.
- Confidence mapping.
- Image attachment encoding/transport.
- Delivery direction and acknowledgement.
- Retry policy.

Do not hard-code a sample XML payload as a final protocol implementation. Obtain exact schema and transaction sequence from authoritative documentation or captured target-device traffic.

## 5. Event state and retries

Store each delivery attempt with:
- Adapter profile and version.
- Destination device.
- Request/response metadata with secrets redacted.
- Attempt timestamp.
- Result status.
- Error category.
- Retry-after time.

Use bounded exponential backoff and a dead-letter state. Do not retry permanent schema/authentication failures indefinitely.

## 6. Native versus generic events

Expose explicit status:
- `internal_only`
- `generic_alarm_delivered`
- `nvr_event_received`
- `native_anpr_index_verified`

Only the final state means the plate is confirmed in native NVR search. A successful HTTP response alone is not sufficient.
