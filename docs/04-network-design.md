# 04 — Network design

## 1. Known topology

The user's physical cameras connect through an external TP-Link PoE switch. The NVR and cameras are reported to share the same subnet. The PoE switch and core switch are TP-Link T1600G-series devices in the user's rack/network.

The exact switch port map, VLAN configuration, subnet, DHCP range, gateway, NVR IP, camera IPs, and Proxmox bridge are unknown. Discover them before assigning addresses.

## 2. Do not apply example addresses blindly

Earlier planning used `192.168.0.0/24` and suggested `.159`–`.191`. These are placeholders only. They must not be configured until:
- Current subnet and mask are confirmed.
- DHCP pool and reservations are inspected.
- Existing static IPs are inventoried.
- Proxmox host and bridge addresses are confirmed.
- NVR and camera addresses are recorded.
- Duplicate-address checks are performed.

Prefer DHCP reservations or a documented static allocation outside the dynamic pool.

## 3. Recommended initial network mode

Start with one LXC management/data IP on the existing CCTV/LAN subnet. Implement virtual camera identities as logical endpoints behind the LXC, using distinct TCP ports or host-based routing for the first prototype.

Why: this is simple to route, observe, firewall, and debug. It avoids macvlan parent-host reachability issues and avoids assuming that the NVR requires one MAC address per virtual camera.

Only introduce per-device secondary IP addresses after a compatibility test proves they are needed. Multiple IP aliases on one interface normally share the interface MAC; do not claim they are equivalent to independent physical devices.

## 4. Alternative: multiple secondary IPs

If testing proves that the NVR must connect to each virtual device by a separate IP:
- Assign secondary IPs to the LXC interface or a dedicated bridge.
- Ensure the switch and upstream network permit those addresses.
- Check ARP behaviour and duplicate IP conflicts.
- Confirm whether NVR device management accepts multiple endpoints sharing one MAC.
- Test HTTP, RTSP, and any required discovery separately.
- Document host networking and firewall rules.

Do not use macvlan merely to create a cosmetic illusion of separate hardware. Use it only if a separate L2 identity is technically required and supported by the host network.

## 5. Ports

Potential listeners, to be enabled only when implemented and required:

| Port | Transport | Purpose |
|---|---|---|
| 80 | TCP | HTTP management/API compatibility |
| 443 | TCP | HTTPS management/API compatibility |
| 554 | TCP/UDP as tested | RTSP |
| 8000 | TCP | Only if a compatible SDK service is implemented and required |
| 8554 | TCP | Optional internal RTSP listener |
| 8080 | TCP | Internal/admin API only; not exposed to CCTV devices |
| 5432 | TCP | Database; bind locally, never expose to camera/NVR LAN |
| 50051 | TCP | Optional internal inference RPC; restrict to loopback/private interface |

These are candidate ports, not a statement that the NVR uses all of them. Determine actual requirements using documentation and packet capture.

## 6. Firewall policy

Permit only:
- LXC to physical cameras: configured RTSP and any explicitly needed management endpoints.
- NVR to virtual endpoints: tested HTTP/HTTPS and RTSP endpoints.
- Admin workstation to web UI: HTTPS.
- LXC to approved NTP/DNS.
- Internal process-to-process inference communication.

Deny unsolicited access from WAN. Do not port-forward camera or ANPR management interfaces to the public internet.

## 7. Discovery commands

Run read-only discovery on Proxmox:

```bash
ip -br addr
ip route
bridge link
bridge vlan show
cat /etc/network/interfaces
```

On the router/DHCP server, export current leases and reservations. On the NVR, record its IP, mask, gateway, firmware, channel count, and camera connection mode. Record camera IPs and RTSP availability without changing configuration.

## 8. Rollback

Keep each original NVR camera configuration documented. For a test camera:
1. Export or photograph its original NVR settings.
2. Add and test virtual endpoint only if a spare channel exists.
3. If replacing a channel, change one channel only.
4. Confirm video and recording.
5. If failure occurs, restore the original camera IP/credentials and channel settings.
6. Never migrate all cameras in one operation.
