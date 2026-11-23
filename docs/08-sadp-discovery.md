# 08 — SADP discovery

## 1. Purpose

SADP is a device discovery/configuration mechanism used in Hikvision environments. It may help a client discover a virtual device, but discovery is not equivalent to NVR acceptance or ANPR support.

## 2. Research requirements

Before implementation, establish:
- Broadcast and multicast behaviour on the target LAN.
- UDP ports and message formats from official documentation or authorised packet captures.
- Whether the NVR actually uses SADP during manual addition or discovery.
- Required response fields.
- Whether device discovery requires a unique MAC or serial.
- How multiple logical devices behind one host are represented.

Do not invent packet formats from memory.

## 3. Safe implementation

- Bind only to the intended LAN interface.
- Do not respond to requests outside the configured subnet.
- Use project-owned stable identifiers.
- Do not clone another physical camera's MAC or serial.
- Rate-limit responses.
- Avoid configuration-changing SADP commands in the first release.
- Make discovery optional and disable it by default until tested.

## 4. Test plan

1. Capture baseline discovery traffic on an isolated test network.
2. Run the virtual service with discovery disabled.
3. Enable passive packet logging.
4. Implement a read-only response only after format is understood.
5. Verify discovery with a dedicated client.
6. Verify NVR discovery separately.
7. Test 2, then 4, then 32 logical devices for collisions.
8. Confirm no duplicate identity or excessive broadcast traffic.

## 5. Fallback

If SADP cannot represent multiple logical devices reliably, support manual device addition using the tested IP/port mapping. Do not make SADP a hard dependency for the core service.
