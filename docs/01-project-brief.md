# 01 — Project brief

## 1. Vision

Build an original software system that sits logically alongside existing IP cameras and a Hikvision NVR. It ingests camera video, performs local ANPR, and presents virtual camera endpoints that the NVR can be configured to use.

The system is a sidecar, not an inline network bridge. Physical cameras remain connected to the external PoE switch. The ANPR service reads their streams independently. During migration, the NVR's existing camera channels should remain available until each replacement virtual endpoint has passed testing.

## 2. Target user outcome

An operator should be able to:

1. Configure a physical camera source.
2. Assign it a virtual camera identity.
3. View the forwarded stream.
4. Enable ANPR processing.
5. See recognised plate events in the application's own event history.
6. Have the target NVR receive and index the event as a native ANPR record, if and only if the NVR's supported protocol workflow permits it.
7. Search records and inspect associated plate and vehicle images.

## 3. Target environment

The first integration target is the user's Hikvision DS-7732NI-I4/16P(B), firmware V4.61.025 build 220905. Cameras connect through an external TP-Link PoE switch. The user reports that camera and NVR addresses are on the same subnet. The deployment target is a Debian LXC on Proxmox VE.

The system should be designed for up to 32 logical virtual camera identities. This is a capacity target, not a performance promise. Actual supported stream count must be measured on the host CPU and with the selected models.

## 4. Design principles

- Greenfield implementation: all project code is written independently.
- Protocol-first: observe and test device behaviour before implementing assumptions.
- Safe by default: no camera or NVR configuration changes during discovery.
- Fail-safe: ANPR failure must not interrupt original camera-to-NVR recording.
- Stream-efficient: avoid transcoding unless required.
- CPU-aware: schedule inference based on measured throughput.
- Modular: media ingest, inference, virtual-device protocol, and UI are separate packages/services.
- Auditable: every event and delivery attempt has traceable identifiers and timestamps.
- Honest compatibility: distinguish basic ONVIF/RTSP connectivity, generic event delivery, and native NVR ANPR indexing.

## 5. Scope boundaries

### Included in initial scope

- Camera source configuration.
- RTSP ingest and stream health.
- Virtual device registry.
- HTTP device-information endpoints.
- Authentication framework.
- ANPR inference pipeline.
- Internal event model.
- Snapshot and image handling.
- NVR compatibility test harness.
- Operator UI.
- Debian LXC deployment.

### Deferred until evidence exists

- Full SADP discovery implementation.
- Hikvision SDK emulation.
- Proprietary device registration or activation flows.
- Native NVR event-search integration.
- Multi-NVR support.
- GPU acceleration.
- Multi-site federation.

## 6. Success definition

The project is not considered complete merely because OCR recognises plates. The primary acceptance outcome is a repeatable end-to-end test in which the target NVR displays and searches a software-generated ANPR event in its native vehicle/plate search workflow. If that is not technically supported, document the limitation and provide a clearly labelled alternative integration mode; do not misrepresent generic alarms as native ANPR.
