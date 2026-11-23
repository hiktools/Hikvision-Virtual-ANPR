# 18 — Development milestones

## M0 — Repository and engineering foundation

Deliver:
- Go module and project layout.
- Contribution and coding standards.
- CI pipeline.
- Configuration loader.
- Structured logging.
- Health endpoints.
- Threat model.

Exit: clean build, tests, and documented local development setup.

## M1 — Camera source and stream proxy

Deliver:
- Add one RTSP source.
- Health monitoring and reconnect.
- RTSP forwarding/remux.
- Snapshot preview.
- No ANPR or NVR changes yet.

Exit: stable proxy for one source and proof that original NVR recording remains unaffected.

## M2 — Virtual device foundation

Deliver:
- Virtual-device registry.
- Device identity model.
- HTTP routing and authentication.
- Basic device-information endpoints based on documented/tested contracts.
- Per-device RTSP paths.
- Compatibility test harness.

Exit: one virtual endpoint works with a generic client; no claim of native Hikvision compatibility.

## M3 — NVR video compatibility

Deliver:
- Capture and document target NVR request sequence.
- Test manual addition on spare/test channel.
- Verify live view, recording, reconnect, and restart.

Exit: Level A/B compatibility evidence.

## M4 — ANPR inference

Deliver:
- Frame sampler.
- Detector and OCR adapter.
- UK plate profile.
- Temporal consensus and deduplication.
- Event review UI.

Exit: documented benchmark and accuracy report on representative lawful test data.

## M5 — Event protocol research and implementation

Deliver:
- Identify NVR event subscription/delivery sequence.
- Canonical event model.
- Tested event adapter.
- Image association and retry handling.

Exit: generic event or NVR event receipt proven; clearly state if native indexing remains unresolved.

## M6 — Native ANPR proof

Deliver:
- Plate appears in NVR native search.
- Associated images visible.
- Correct camera/channel and timestamp.
- Repeatability after restart.

Exit: Level D compatibility on exact target firmware, or a documented technical limitation.

## M7 — Multi-camera scale

Deliver:
- 2/4/8/16/32 device testing.
- Resource controls.
- Storage retention.
- Recovery and monitoring.

Exit: published capacity envelope based on actual hardware.

## M8 — Production hardening

Deliver:
- Secure installer.
- Backup/restore.
- Upgrade/rollback.
- Documentation.
- Security review.
- Release process.

## Rule

Do not skip M3 and M5/M6. ANPR OCR success does not solve NVR protocol compatibility.
