# 09 — Media pipeline

## 1. Goals

The media subsystem must ingest existing camera video, provide a stable virtual stream, and supply selected frames to the inference subsystem without disrupting the NVR's original recording path during development.

## 2. RTSP input

Per source configure:
- URL or host/path components.
- Username and secret reference.
- TCP or UDP transport preference.
- Main/substream selection.
- Connection timeout.
- Read timeout.
- Reconnect policy.
- Optional expected codec/resolution/frame rate.

Do not expose a raw URL containing credentials in UI responses or logs.

## 3. Passthrough modes

Preferred order:
1. **Packet/stream passthrough or remux:** preserve encoded video if supported and compatible.
2. **Decode for analysis plus original stream forwarding:** decode a parallel low-cost path or sample frames.
3. **Transcode:** only when target NVR cannot consume source codec/profile/container.

Avoid transcoding the full stream solely for ANPR. The inference path may decode at a reduced rate/resolution independently of the forwarded stream.

## 4. Main and substreams

Use the camera substream for inference where it contains sufficient plate detail; otherwise use the main stream or configurable region of interest. Measure plate pixel width at the expected detection distance. Do not assume a lower-resolution substream is adequate.

## 5. Frame sampling

Sampling policy should be per camera:
- Maximum inference frames per second.
- Motion/vehicle-triggered sampling if supported.
- Queue depth.
- Frame freshness limit.
- Maximum inference latency.
- Drop policy for stale frames.

For real-time systems, dropping an old frame is generally preferable to allowing an unbounded queue to grow.

## 6. Timestamps

Preserve source timestamps where reliable. Also record local receive time and processing time. Monitor clock skew and discontinuities. Store all canonical timestamps in UTC; display local time in the UI.

## 7. RTSP server

Use a maintained media server or a carefully tested in-process implementation. Evaluate:
- Per-device paths.
- Authentication.
- RTSP-over-TCP support.
- Codec support.
- Session limits.
- Connection lifecycle.
- Recording compatibility with the target NVR.

The chosen component is an implementation dependency, not a source of Hikvision protocol truth.

## 8. Health metrics

Per stream:
- Connected/disconnected.
- Last packet and last decoded frame.
- Input bitrate.
- Resolution and codec.
- Reconnect count.
- Packet loss or decode errors where measurable.
- Proxy client count.
- Forwarding latency.

## 9. Acceptance tests

- 1 stream ingest and forward.
- Main and substream.
- TCP and UDP modes where supported.
- Camera reboot recovery.
- Network interruption recovery.
- Bad credentials.
- Unsupported codec.
- NVR reconnect.
- 4/8/16/32 stream load tests.
