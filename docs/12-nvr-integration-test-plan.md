# 12 — Target NVR integration test plan

## 1. Target

Hikvision DS-7732NI-I4/16P(B), firmware V4.61.025 build 220905, as supplied by the project owner. Reconfirm exact model suffix and firmware from the device before testing.

The recorder is reported to have approximately 25–32 camera channels occupied. Do not assume spare channels exist.

## 2. Critical distinction

A camera being accepted for ordinary video recording does not prove ANPR compatibility. A generic alarm appearing in the event log does not prove the plate is indexed in the native vehicle search interface.

## 3. Test prerequisites

- Export NVR configuration or document rollback settings.
- Record current channel mapping and camera IPs.
- Confirm time/NTP configuration.
- Identify one spare channel or arrange a controlled test window to replace one channel temporarily.
- Have one physical camera stream available to the virtual service.
- Ensure original camera recording can be restored quickly.
- Capture NVR-to-camera traffic only on an authorised network and redact secrets.

## 4. Test stages

### Stage A — Reachability

- NVR can reach virtual service.
- Authentication challenge is understood.
- HTTP and RTSP ports respond.
- No false manufacturer or certification claims.

### Stage B — Video

- Add one virtual endpoint manually if supported.
- NVR displays live video.
- NVR records video.
- Reconnect after virtual service restart.
- Reconnect after camera source restart.
- Check audio/substream requirements if applicable.

### Stage C — Device behaviour

- Record all NVR requests.
- Identify required device-information and capability endpoints.
- Verify channel mapping.
- Test NVR reboot and virtual-service reboot.
- Confirm no repeated authentication lockouts.

### Stage D — Event delivery

- Determine whether NVR polls, subscribes, or expects device-initiated event delivery.
- Deliver a synthetic test event with clearly labelled test plate data.
- Confirm event appears in ordinary event/alarm log.
- Verify image transport and association.
- Record exact request/response and timing.

### Stage E — Native ANPR

- Use a controlled test event.
- Search by exact plate and time.
- Confirm the NVR displays plate, time, camera/channel, and expected images.
- Confirm export/playback behaviour.
- Repeat after NVR restart.
- Repeat at least 20 times and check for missing/duplicate records.

### Stage F — Scale

Only after one device passes:
- 2 devices.
- 4 devices.
- 8 devices.
- Then larger batches based on channel capacity and measured resources.

## 5. Results format

For every test record:
- Date/time.
- NVR model and firmware.
- Virtual service version.
- Camera source and virtual device IDs.
- Configuration.
- Packet capture reference.
- Expected result.
- Actual result.
- Pass/fail.
- Notes and reproducibility.

## 6. Rollback

If video, authentication, or NVR stability fails:
1. Remove/disable virtual endpoint.
2. Restore original camera channel configuration.
3. Verify original live view and recording.
4. Preserve logs and captures.
5. Do not proceed to more channels until cause is understood.

## 7. Exit criteria

Native ANPR milestone passes only when the NVR's own vehicle/plate search interface retrieves software-generated events with correct plate and associated images, after a restart, in repeated tests.
