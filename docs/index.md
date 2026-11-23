# Hikvision Virtual ANPR

**A self-hosted virtual camera platform bringing local UK number plate recognition to existing CCTV infrastructure.**

Hikvision Virtual ANPR is an independent, greenfield project designed to receive existing IP camera streams, analyse video locally, and expose software-defined camera identities and event interfaces for carefully tested integration with Hikvision network video recorders.

<div class="grid cards" markdown>

-   :material-video-wireless:{ .lg .middle } **RTSP ingestion**

    Receive and manage streams from existing IP cameras without inserting the service into their recording path.

-   :material-car-search:{ .lg .middle } **Local ANPR**

    Detect vehicles and recognise UK registration plates using locally processed video.

-   :material-camera-switch:{ .lg .middle } **Virtual cameras**

    Present independently configured software-defined camera identities.

-   :material-api:{ .lg .middle } **NVR integration**

    Implement and test device-facing interfaces, event delivery and native ANPR indexing.

</div>

[Explore the architecture](03-architecture.md){ .md-button .md-button--primary }
[View the GitHub repository](https://github.com/hiktools/Hikvision-Virtual-ANPR){ .md-button }

## Project status

!!! info "Design baseline — protocol compatibility unverified"
    The project is currently at the design and protocol-research stage. Native ANPR compatibility must not be claimed until software-generated plate events are indexed and searchable through the target NVR's own vehicle or plate search interface.

| Component | Initial target |
|---|---|
| NVR | Hikvision DS-7732NI-I4/16P(B) |
| Firmware | V4.61.025 build 220905 |
| Deployment | Debian LXC on Proxmox |
| Processing | Local CPU-based inference |
| Virtual camera capacity | Target of up to 32 identities |
| ANPR profile | United Kingdom |

## How it works

1. Existing IP cameras provide RTSP video streams.
2. The platform ingests and forwards the streams.
3. The inference pipeline detects vehicles and reads registration plates.
4. Recognition results are stored as structured ANPR events with associated metadata and images.
5. Virtual camera endpoints expose independently configured device identities.
6. Compatibility tests establish which video, event and native ANPR features the target NVR actually accepts.

## Documentation

<div class="grid cards" markdown>

-   **Project and architecture**

    Understand scope, requirements, system boundaries and implementation milestones.

    - [Project brief](01-project-brief.md)
    - [Requirements](02-requirements.md)
    - [Architecture](03-architecture.md)
    - [Milestones](18-milestones.md)

-   **Infrastructure**

    Plan networking, deployment, virtual devices and security.

    - [Network design](04-network-design.md)
    - [Proxmox LXC deployment](05-proxmox-lxc.md)
    - [Virtual device model](06-virtual-device-model.md)
    - [Security](14-security.md)

-   **Hikvision integration**

    Research device discovery, ISAPI behaviour and NVR compatibility.

    - [ISAPI compatibility](07-isapi-compatibility.md)
    - [SADP discovery](08-sadp-discovery.md)
    - [NVR integration test plan](12-nvr-integration-test-plan.md)
    - [Reference research](21-reference-research.md)

-   **Video and ANPR**

    Explore media handling, recognition, event processing and storage.

    - [Media pipeline](09-media-pipeline.md)
    - [ANPR engine](10-anpr-engine.md)
    - [ANPR events](11-anpr-events.md)
    - [Data model](13-data-model.md)

-   **Application and operations**

    Review the operator interface, observability and testing.

    - [Web interface](15-web-ui.md)
    - [Observability](16-observability.md)
    - [Testing strategy](17-testing.md)

-   **Development resources**

    Guidance for contributors and AI coding agents.

    - [AI coding-agent instructions](19-ai-agent-build-instructions.md)
    - [Open questions](20-open-questions.md)
    - [Glossary](22-glossary.md)

</div>

## Engineering principles

- **Greenfield development:** All software is independently implemented; external repositories are research references only.
- **Local processing:** Video and recognition data remain on infrastructure controlled by the operator.
- **Recording preservation:** Existing camera-to-NVR recording paths must remain operational during development.
- **Protocol accuracy:** Undocumented behaviour is experimentally verified, never assumed.
- **Failure isolation:** ANPR processing failures must not interrupt video forwarding.
- **Honest compatibility:** Generic alarms, video streaming and native searchable ANPR records are treated as separate capabilities.

!!! warning "Independent project"
    Hikvision Virtual ANPR is not an official Hikvision product and is not affiliated with or certified by Hikvision. Compatibility depends on the exact recorder model and firmware and must be demonstrated through repeatable testing.
