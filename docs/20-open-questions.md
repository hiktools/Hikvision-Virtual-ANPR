# 20 — Open questions and decisions

This page is the source of truth for unresolved project assumptions.

## Network

- [ ] Actual LAN subnet and mask.
- [ ] Default gateway.
- [ ] DHCP range and reservations.
- [ ] NVR IP and interface configuration.
- [ ] Physical camera IP inventory.
- [ ] Proxmox host IP and bridge.
- [ ] VLAN configuration and switch uplinks.
- [ ] Whether NVR can route to LXC if VLANs differ.

## Proxmox host

- [ ] CPU model, sockets, cores, threads, flags.
- [ ] Total and available RAM.
- [ ] Storage pools, free capacity, and performance.
- [ ] Proxmox version and kernel.
- [ ] LXC template availability.
- [ ] Network link speed.
- [ ] Whether any future accelerator is possible.

## NVR

- [ ] Confirm exact model suffix and firmware.
- [ ] Current channel count in use.
- [ ] Available test channel or safe maintenance window.
- [ ] Supported camera protocols and addition modes.
- [ ] Native ANPR menu availability and current functionality.
- [ ] Whether ANPR support is restricted to specific camera models.
- [ ] Exact event subscription and image transport behaviour.
- [ ] Whether native plate search can index third-party events.

## Cameras

- [ ] Camera models and firmware.
- [ ] Main/substream codecs, resolution, and frame rate.
- [ ] RTSP credentials and access policy.
- [ ] Camera field of view, plate distance, and lighting.
- [ ] Which cameras are suitable for plate capture.

## Performance

- [ ] Desired recognition latency.
- [ ] Expected vehicle events per camera/day.
- [ ] Retention period for event metadata and images.
- [ ] Whether 32 streams require inference simultaneously or only selected cameras.
- [ ] CPU benchmark results.

## Product

- [ ] Repository URL and licence.
- [ ] Public/private repository decision.
- [ ] UI authentication and operator roles.
- [ ] Backup target.
- [ ] Release/update strategy.

## Decisions already made

- Greenfield project; no fork.
- Reference repositories are research material only.
- Go is the primary backend language.
- Vue 3 + TypeScript is the planned UI.
- Debian LXC on Proxmox is the preferred deployment.
- No dedicated GPU is currently available.
- Target is up to 32 virtual camera identities.
- External PoE switch connects physical cameras.
- NVR and cameras are reported to share a subnet.
- NVR target is DS-7732NI-I4/16P(B), firmware V4.61.025 build 220905.
- Preserve existing recording and migrate one channel at a time.
