# 03 — System architecture

## 1. Topology

Use a sidecar topology:

- Physical cameras remain on the existing PoE switch and LAN.
- The LXC connects to the same routed/L2 network as permitted by the final network audit.
- The LXC pulls RTSP from each physical camera.
- The LXC exposes virtual-device services to the NVR.
- The NVR is configured to use a virtual endpoint only after that endpoint passes staged tests.
- Keep the original direct camera path available during rollout and rollback.

Do not insert the software inline between switch and NVR. A sidecar avoids making recording dependent on the ANPR service.

## 2. Logical components

### Control plane

Owns camera configuration, virtual-device identity, protocol profiles, credentials, health, and operator API.

### Media plane

Owns RTSP input sessions, packet/frame handling, optional remux/transcode, and virtual RTSP output.

### Inference plane

Consumes sampled frames, detects vehicles and plates, tracks objects, runs OCR, and produces canonical recognition candidates.

### Event plane

Validates candidates, applies deduplication and confidence policy, stores events and images, and dispatches adapter-specific payloads.

### Compatibility plane

Implements tested device-facing HTTP/ISAPI-like endpoints and any separately proven discovery or event workflows. Each endpoint must be backed by protocol evidence and a compatibility test.

### Presentation plane

Vue 3 web UI and operator API. It must not be in the media hot path.

## 3. Suggested process boundaries

Start with a modular Go application plus a separate inference worker process. Avoid premature microservices.

- `anprd`: API, device registry, virtual HTTP endpoints, event orchestration, database access.
- `media-worker`: stream ingest/forwarding and frame sampling; can initially use FFmpeg/GStreamer subprocesses.
- `inference-worker`: model runtime and OCR; communicate over a versioned local gRPC or Unix-socket interface.
- `web`: Vue 3 static assets served by `anprd` or a local reverse proxy.

A future deployment may split these processes into containers, but the initial Proxmox target is one LXC.

## 4. Data flow

1. Source manager validates RTSP configuration.
2. Media worker opens input stream and publishes health.
3. Media worker forwards/remuxes the source when possible.
4. Sampler selects frames according to per-camera policy.
5. Detector identifies vehicles and candidate plate regions.
6. Tracker associates observations across frames.
7. OCR recognises text and returns raw candidates plus confidence.
8. Plate normaliser applies country profile without overwriting raw text.
9. Event policy groups repeated observations and determines whether to create an event.
10. Event store persists canonical event and image assets.
11. Adapter creates protocol-specific payload from canonical event.
12. Delivery worker attempts NVR communication using the proven workflow.
13. Delivery result is recorded; retry is idempotent.

## 5. Canonical interfaces

Define Go interfaces around behaviour, not vendor names:

```go
type StreamSource interface {
    Start(ctx context.Context) error
    Stop(ctx context.Context) error
    Health() StreamHealth
    Frames() <-chan FrameRef
}

type Detector interface {
    Detect(ctx context.Context, frame Frame) ([]Detection, error)
}

type PlateRecognizer interface {
    Recognize(ctx context.Context, crop Image) (OCRResult, error)
}

type EventAdapter interface {
    Capabilities(ctx context.Context, device Device) (AdapterCapabilities, error)
    Encode(ctx context.Context, event ANPREvent) (EncodedEvent, error)
    Deliver(ctx context.Context, event EncodedEvent) (DeliveryResult, error)
}
```

The exact interfaces can evolve, but protocol adapters must not leak vendor-specific fields into the canonical domain model.

## 6. Failure isolation

- Stream proxy continues if inference is stopped.
- Inference queue is bounded; stale frames are dropped rather than accumulating unbounded latency.
- Event persistence happens before external delivery.
- Delivery retries do not rerun OCR.
- Database outages must not cause unbounded in-memory event accumulation.
- Each camera has independent cancellation, backoff, and health state.
- A malformed external request must not crash the process.

## 7. Compatibility profiles

Represent device compatibility as explicit profiles, e.g. `generic-rtsp`, `hikvision-isapi-research`, and a target-specific profile named with model/firmware. Do not present a generic profile as a genuine Hikvision product identity.
