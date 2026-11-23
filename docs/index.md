---
title: Documentation
layout: default
nav_order: 1
has_children: true
permalink: /docs/
---

# Documentation

Technical specifications, architecture decisions, deployment instructions and implementation guidance for **Hikvision Virtual ANPR**.

This documentation serves as the project's engineering reference and implementation specification. It is intended for developers, contributors and AI coding agents working on the platform.

## Project overview

Understand the goals, requirements and overall design before implementing individual components.

| Document | Description |
|---|---|
| [Project brief](01-project-brief.md) | Project goals, scope and objectives. |
| [Requirements](02-requirements.md) | Functional and non-functional requirements. |
| [Architecture](03-architecture.md) | System architecture and component responsibilities. |
| [Network design](04-network-design.md) | Network topology, addressing and connectivity. |
| [Development milestones](18-milestones.md) | Implementation phases and delivery priorities. |

## Infrastructure and deployment

Deployment, device management and operational security.

| Document | Description |
|---|---|
| [Proxmox LXC deployment](05-proxmox-lxc.md) | Debian container deployment and configuration. |
| [Virtual device model](06-virtual-device-model.md) | Virtual camera identities and device behaviour. |
| [Security](14-security.md) | Authentication, access control and security requirements. |

## Hikvision integration

Research and implementation requirements for communicating with Hikvision equipment.

| Document | Description |
|---|---|
| [ISAPI compatibility](07-isapi-compatibility.md) | Device-facing HTTP interfaces and protocol compatibility. |
| [SADP discovery](08-sadp-discovery.md) | Device discovery and network identification. |
| [NVR integration test plan](12-nvr-integration-test-plan.md) | Verification of video, events and native ANPR functionality. |
| [Reference research](21-reference-research.md) | External technical references and research findings. |

## Video processing and ANPR

The media pipeline, recognition engine and event handling.

| Document | Description |
|---|---|
| [Media pipeline](09-media-pipeline.md) | RTSP ingestion, forwarding and stream management. |
| [ANPR engine](10-anpr-engine.md) | Vehicle detection, plate recognition and OCR. |
| [ANPR events](11-anpr-events.md) | Recognition events, images and event processing. |
| [Data model](13-data-model.md) | Database entities and relationships. |

## Application and operations

The management interface, monitoring and quality assurance.

| Document | Description |
|---|---|
| [Web interface](15-web-ui.md) | Frontend design and management functionality. |
| [Observability](16-observability.md) | Logging, monitoring and diagnostics. |
| [Testing strategy](17-testing.md) | Unit, integration and compatibility testing. |

## Development guidance

Instructions for contributors and automated development agents.

| Document | Description |
|---|---|
| [AI coding-agent instructions](19-ai-agent-build-instructions.md) | Implementation rules, development workflow and engineering constraints. |
| [Open questions](20-open-questions.md) | Unresolved decisions and research requirements. |
| [Glossary](22-glossary.md) | Technical terminology used throughout the project. |

---

## Important compatibility note

The project distinguishes between three separate capabilities:

1. Video streaming through a virtual camera.
2. Generic event or alarm delivery.
3. Native ANPR event indexing and plate searching through the NVR's own interface.

These are not interchangeable. Native ANPR compatibility must be demonstrated through repeatable testing against the target recorder and firmware before it is considered supported.

## Project repository

[Hikvision Virtual ANPR on GitHub](https://github.com/hiktools/Hikvision-Virtual-ANPR)
