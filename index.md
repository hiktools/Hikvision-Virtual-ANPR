---
title: Home
layout: home
nav_exclude: true
---

# Hikvision Virtual ANPR

**An independent, AI-powered virtual camera platform with local UK Automatic Number Plate Recognition (ANPR).**

Hikvision Virtual ANPR is a greenfield software project designed to bring intelligent number plate recognition to existing CCTV infrastructure.

The system receives RTSP streams from existing IP cameras, analyses video locally using computer vision and OCR, and exposes software-defined virtual camera endpoints for integration with Hikvision network video recorders.

The goal is to provide a bridge between existing CCTV infrastructure and modern, locally processed ANPR capabilities.

[View on GitHub](https://github.com/hiktools/Hikvision-Virtual-ANPR)

---

## What is Hikvision Virtual ANPR?

The software operates alongside existing CCTV infrastructure rather than replacing it.

- **RTSP ingestion** — Connect to existing IP cameras and receive their video streams.
- **AI-powered ANPR** — Detect vehicles and recognise UK registration plates locally.
- **Virtual cameras** — Expose independently configurable software-defined camera identities.
- **Hikvision integration** — Implement and validate device-facing interfaces for compatible NVRs.
- **Event processing** — Store recognised plates, confidence scores, timestamps and associated images.
- **Web management** — Configure cameras, monitor streams and search recognition events.
- **Self-hosted** — Run locally on your own infrastructure without requiring cloud-based video processing.

## How it works

1. Existing IP cameras provide RTSP video streams.
2. The system receives and forwards those streams.
3. An inference pipeline detects vehicles and reads registration plates.
4. Recognition results become structured ANPR events.
5. Virtual camera endpoints expose the configured device interfaces.
6. Compatible NVRs can be tested for video, event and native ANPR integration.

## Project status

<escape>**Current stage:** Design baseline and protocol research.</escape>

The initial integration target is:

| Component | Target |
|---|---|
| NVR | Hikvision DS-7732NI-I4/16P(B) |
| Firmware | V4.61.025 build 220905 |
| Deployment | Debian LXC on Proxmox |
| Processing | Local CPU-based inference |
| Virtual camera target | Up to 32 identities |
| Initial ANPR profile | United Kingdom |

Native ANPR compatibility is not yet verified. A generic alarm appearing on an NVR is not sufficient proof: the NVR must successfully index and expose software-generated plate records through its own native vehicle or plate search interface.

## Documentation

The documentation is organised into technical specifications intended for developers and AI coding agents.

### Getting started

- [Project brief](docs/01-project-brief.md)
- [Requirements](docs/02-requirements.md)
- [Architecture](docs/03-architecture.md)
- [Development milestones](docs/18-milestones.md)

### Infrastructure and devices

- [Network design](docs/04-network-design.md)
- [Proxmox LXC deployment](docs/05-proxmox-lxc.md)
- [Virtual device model](docs/06-virtual-device-model.md)
- [Security](docs/14-security.md)

### Video and ANPR

- [RTSP media pipeline](docs/09-media-pipeline.md)
- [ANPR engine](docs/10-anpr-engine.md)
- [ANPR events](docs/11-anpr-events.md)
- [Data model](docs/13-data-model.md)

### Hikvision compatibility

- [ISAPI compatibility](docs/07-isapi-compatibility.md)
- [SADP discovery](docs/08-sadp-discovery.md)
- [NVR integration test plan](docs/12-nvr-integration-test-plan.md)
- [Reference research](docs/21-reference-research.md)

### Development and operations

- [Web interface](docs/15-web-ui.md)
- [Observability](docs/16-observability.md)
- [Testing strategy](docs/17-testing.md)
- [Open questions](docs/20-open-questions.md)
- [Glossary](docs/22-glossary.md)

### AI-assisted development

- [AI coding-agent instructions](docs/19-ai-agent-build-instructions.md)

These instructions define the engineering constraints, implementation expectations and verification requirements for agents contributing to the project.

---

## Engineering principles

- **Greenfield development:** All software is independently implemented. External repositories are research references only.
- **Local processing:** Video and recognition data remain on infrastructure controlled by the operator.
- **Recording preservation:** Existing camera-to-NVR recording paths must remain operational during development.
- **Protocol accuracy:** Undocumented behaviour must be experimentally verified rather than assumed.
- **Failure isolation:** ANPR processing failures must not interrupt video forwarding.
- **Honest compatibility:** Native ANPR support is only claimed after successful testing against the target NVR and firmware.

## Important

Hikvision Virtual ANPR is an independent project and is not an official Hikvision product. Compatibility depends on the capabilities and firmware of the target recorder. No manufacturer affiliation or certification is implied.

---

**Your cameras. Your infrastructure. Your recognition system.**
