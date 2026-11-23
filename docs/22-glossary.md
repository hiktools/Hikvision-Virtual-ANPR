# 22 — Glossary

- **ANPR:** Automatic Number Plate Recognition.
- **LPR:** License Plate Recognition; often used interchangeably with ANPR.
- **ISAPI:** Hikvision's IP-device API family. Exact endpoint behaviour varies by device and firmware.
- **SADP:** Hikvision device discovery/configuration mechanism.
- **HCNetSDK:** Hikvision client SDK used by some applications to communicate with devices.
- **RTSP:** Real Time Streaming Protocol used to establish media sessions.
- **ONVIF:** Open network interface standards for IP-based physical security products.
- **Virtual device:** A software-defined endpoint presented to an NVR/client.
- **Physical source:** Existing camera supplying the upstream video.
- **Canonical event:** Vendor-neutral internal representation of a recognition.
- **Native ANPR indexing:** NVR stores a recognition in its own vehicle/plate search database and exposes it through its native search interface.
- **Generic alarm:** An event visible in a general alarm/event log but not necessarily searchable as a vehicle plate.
- **Remux:** Repackage an encoded stream without decoding/re-encoding.
- **Transcode:** Decode and re-encode media, consuming more CPU and potentially changing quality.
- **Temporal consensus:** Combining repeated observations of one plate over time to improve recognition confidence.
- **Idempotency:** Repeating a delivery operation without creating duplicate logical results.
