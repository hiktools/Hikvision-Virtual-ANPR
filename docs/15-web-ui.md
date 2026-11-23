# 15 — Web UI

## 1. Technology

Vue 3 and TypeScript. The UI communicates with a versioned application API. It is not part of the RTSP or event hot path.

## 2. Main navigation

- Overview
- Cameras
- Virtual devices
- Live view
- ANPR events
- NVR integration
- System health
- Settings
- Audit log

## 3. Overview

Show:
- Physical source connectivity.
- Virtual device availability.
- Active RTSP sessions.
- Inference queue depth and throughput.
- Recent recognition events.
- Event delivery states.
- CPU, memory, storage, and temperature if available.
- NVR compatibility status and last verified test.

Never show "Native ANPR connected" merely because an HTTP endpoint is reachable.

## 4. Camera setup

Wizard:
1. Enter source name and RTSP details.
2. Test connectivity.
3. Select main/substream.
4. Preview frame and codec.
5. Create virtual device mapping.
6. Enable/disable ANPR.
7. Configure sampling and quality.
8. Save and show health.

Secrets should be write-only in the UI; editing a camera must not reveal stored credentials.

## 5. Event search

Filters:
- Plate text.
- Time range.
- Source camera.
- Confidence thresholds.
- Review status.
- NVR delivery state.

Display raw OCR and normalised plate separately. Display capture time, processing time, confidence breakdown, and image assets.

## 6. Event review

Operator may correct a plate. Preserve original OCR, correction actor, timestamp, and reason. Corrections must not silently rewrite previously delivered event payloads.

## 7. NVR integration page

Show capabilities independently:
- Reachable.
- Video tested.
- Device profile tested.
- Generic event tested.
- Native ANPR indexing verified.

Display NVR model, firmware, last test date, evidence reference, and compatibility profile.

## 8. Accessibility and usability

Use keyboard-accessible controls, clear status text, responsive layouts, and non-colour-only status indicators. Confirm destructive actions. Provide explicit warnings before changing live camera mappings.
