# Hikvision Virtual ANPR

Independent, greenfield software project for building a virtual network-camera platform with local ANPR processing and a carefully validated integration path to Hikvision NVRs.

> **Compatibility warning:** This project is not an official Hikvision product and must not claim native ANPR compatibility until tested against the exact target NVR model and firmware. An ISAPI-shaped endpoint or an XML event accepted by one device does not prove that an NVR will index it in its native vehicle-search interface.

## Project goals

- Ingest video from existing IP cameras without interrupting their direct recording path.
- Analyse selected frames locally to detect vehicles and read registration plates.
- Expose independently configurable virtual camera identities and video streams.
- Implement documented, testable device and event interfaces.
- Deliver ANPR metadata and associated images using a protocol workflow proven against the target NVR.
- Provide an operator UI, event history, health monitoring, and reproducible deployment.
- Support a target scale of 32 input cameras, subject to measured CPU throughput and NVR channel limits.

## Non-goals

- Bypassing authentication, access controls, licensing, or security features.
- Falsely representing the software as a genuine Hikvision-manufactured device.
- Guaranteeing support for every Hikvision model or firmware.
- Replacing the NVR's existing recording function.
- Recording 32 duplicate continuous video archives by default.
- Treating a generic HTTP alarm as equivalent to a native searchable ANPR record.

## Documentation map

| Page | Purpose |
|---|---|
| [Project brief](docs/01-project-brief.md) | Scope, goals, assumptions, and constraints |
| [Requirements](docs/02-requirements.md) | Functional and non-functional requirements |
| [Architecture](docs/03-architecture.md) | System boundaries and component design |
| [Network design](docs/04-network-design.md) | LAN topology, IP plan, routing, and ports |
| [Proxmox LXC deployment](docs/05-proxmox-lxc.md) | Container specification and deployment strategy |
| [Virtual device model](docs/06-virtual-device-model.md) | Device identity, channels, and per-camera isolation |
| [ISAPI compatibility](docs/07-isapi-compatibility.md) | API design and protocol research process |
| [SADP discovery](docs/08-sadp-discovery.md) | Device discovery research and safe implementation |
| [RTSP and media](docs/09-media-pipeline.md) | Stream ingest, proxying, and media handling |
| [ANPR engine](docs/10-anpr-engine.md) | Detection, OCR, tracking, and CPU optimisation |
| [ANPR events](docs/11-anpr-events.md) | Event model, images, delivery, and deduplication |
| [NVR integration](docs/12-nvr-integration-test-plan.md) | Staged compatibility test plan |
| [Data model](docs/13-data-model.md) | Database entities and retention |
| [Security](docs/14-security.md) | Authentication, network controls, privacy, and audit |
| [Web UI](docs/15-web-ui.md) | Administration and operator workflows |
| [Observability](docs/16-observability.md) | Logs, metrics, health, and troubleshooting |
| [Testing](docs/17-testing.md) | Unit, integration, performance, and acceptance tests |
| [Milestones](docs/18-milestones.md) | Build sequence and exit criteria |
| [AI coding-agent instructions](docs/19-ai-agent-build-instructions.md) | Rules for an AI agent implementing the project |
| [Open questions](docs/20-open-questions.md) | Decisions that require measurements or protocol evidence |
| [Reference research](docs/21-reference-research.md) | How the supplied repositories should be used |
| [Glossary](docs/22-glossary.md) | Shared terminology |

## Current target environment

Known from project planning:

- Target recorder: Hikvision DS-7732NI-I4/16P(B), firmware V4.61.025 build 220905.
- Cameras are connected through an external TP-Link PoE switch, not directly to the NVR's built-in PoE ports.
- The NVR and cameras are reported to be on the same IP subnet.
- Proxmox VE host is referred to as `pve3`.
- Deployment preference: Debian LXC.
- No dedicated GPU is currently available.
- Current NVR camera occupancy is approximately 25–32 channels.
- Desired virtual-camera capacity: 32 identities.

Unknown and must be discovered before deployment:

- Actual subnet, gateway, DHCP pool, NVR address, camera addresses, and Proxmox bridge.
- Proxmox CPU model, available cores, RAM, storage capacity, and network link speed.
- Exact NVR camera-addition and ANPR feature behaviour.
- Whether the target NVR accepts third-party or software-emulated devices for native ANPR indexing.
- Required event subscription direction, image transport, and any proprietary SDK interactions.

## Engineering rule

Never convert an assumption into a protocol fact. Mark unknown behaviour as `UNVERIFIED`, record the test and firmware used, and only promote it to `VERIFIED` when a repeatable test proves it.
