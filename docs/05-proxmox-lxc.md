# 05 — Proxmox LXC deployment

## 1. Deployment target

Preferred initial deployment: one Debian LXC on Proxmox VE. The host's CPU model, core count, memory, storage availability, and network bridge must be audited before final resource allocation.

Do not hard-code a container ID, storage name, bridge, IP, or resource size until verified.

## 2. OS choice

Use a currently supported Debian release that is compatible with the selected Go toolchain, FFmpeg/GStreamer packages, and inference runtime. Debian 13 is the proposed target; validate all required packages and libraries before locking it. Debian 12 may be used if a dependency requires it, but document the reason and support lifecycle.

## 3. Initial sizing

These are provisional starting points, not guaranteed requirements:

| Resource | Initial target |
|---|---|
| vCPU | 8 cores, benchmark before increasing |
| RAM | 16 GB, increase if measured need |
| Swap | 2–4 GB, not a substitute for RAM |
| Root disk | 32–64 GB |
| Event/image storage | Start at 100–200 GB, based on retention |
| Network | 1 Gbps minimum; 2.5 Gbps preferred if measured traffic warrants it |
| GPU | None initially |

The user wants 32 configured virtual camera identities, but 32 simultaneous full-rate ANPR inference streams are not guaranteed on CPU. Use configurable sampling and benchmark.

## 4. Container type

Prefer unprivileged LXC. Enable only required features. Avoid privileged mode as a convenience. If device passthrough, FUSE, or nested container runtime is later required, document the specific need and threat trade-off.

Do not install Docker-in-LXC by default. Run native systemd services in the LXC initially. A Docker deployment can be supported separately after the core application is stable.

## 5. Storage

Separate:
- OS and binaries.
- Configuration and secrets.
- PostgreSQL data.
- Event image storage.
- Logs.
- Backups.

Use a dedicated mount point for event images if available. Do not store continuous duplicate video by default. Calculate image storage using measured average image size × events/day × retention days × cameras, with headroom.

## 6. Systemd services

Suggested units:
- `hikvision-anprd.service`
- `hikvision-media-worker@.service` or a managed worker pool
- `hikvision-inference-worker.service`
- `hikvision-web.service` if separately served

Requirements:
- Dedicated unprivileged service user.
- `Restart=on-failure`.
- Resource limits where appropriate.
- Read-only application directories where possible.
- Writable state directories only.
- `NoNewPrivileges=true`, `PrivateTmp=true`, and suitable systemd hardening after testing.
- Secrets via protected environment file or credential mechanism, not command-line arguments.

## 7. Pre-deployment audit

On Proxmox host, collect:

```bash
pveversion -v
lscpu
free -h
df -hT
pvesm status
cat /etc/pve/storage.cfg
ip -br addr
ip route
pct list
qm list
lspci | grep -Ei 'vga|3d|display' || true
```

Check CPU flags and architecture for inference runtime compatibility. Check storage free space and I/O performance. Check whether the LXC can reach camera and NVR subnets.

## 8. Deployment sequence

1. Audit host and network.
2. Select supported Debian template.
3. Create unprivileged LXC with conservative resources.
4. Configure static DHCP reservation or verified static address.
5. Apply host and LXC firewall policy.
6. Install only required runtime dependencies.
7. Create service user and directory layout.
8. Install application from a versioned release artifact.
9. Configure PostgreSQL and migrations.
10. Start API and health checks with no camera sources.
11. Add one test source.
12. Verify proxy before enabling inference.
13. Enable inference and inspect resource usage.
14. Test one virtual endpoint against a spare NVR channel.

## 9. Backups

Back up configuration, database, event metadata, and selected images. Use consistent database backup procedures. Test restore into a disposable LXC. Proxmox snapshots are useful but do not replace application-consistent backups.
