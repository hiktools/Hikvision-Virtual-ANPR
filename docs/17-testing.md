# 17 — Testing strategy

## 1. Unit tests

Test:
- Device registry and ID stability.
- Camera-to-device mapping.
- Authentication.
- XML parser and generator.
- Event canonicalisation.
- UK plate normalisation.
- Deduplication.
- Retention calculations.
- Retry/backoff logic.
- Secret redaction.

## 2. Integration tests

Use local mock RTSP sources and a mock NVR client. Test:
- Source connect/disconnect.
- Stream remux/forward.
- Snapshot generation.
- API authentication.
- Event encoding.
- Event delivery retry.
- Database restart and migration.
- Worker crash recovery.

## 3. Protocol tests

Keep a version-controlled, sanitised corpus of request/response fixtures. Each fixture must identify its origin and whether it is official documentation, a lab capture, or a synthetic test.

Do not store production credentials, public addresses, real plate data, or private footage in fixtures.

## 4. Hardware-in-the-loop tests

The target NVR test is mandatory for compatibility claims. Use one test channel and a rollback plan. Record exact NVR firmware and configuration.

## 5. Performance tests

Benchmark:
- 1, 2, 4, 8, 16, and 32 RTSP inputs.
- Proxy-only CPU and memory.
- Inference at multiple sampling rates.
- Day/night representative clips.
- Database write rate.
- Image storage throughput.
- Event delivery latency.

Report throughput, p50/p95/p99 latency, dropped frames, queue age, CPU, RAM, and thermal throttling if available.

## 6. Reliability tests

- Kill inference worker.
- Restart media worker.
- Restart application.
- Restart LXC.
- Interrupt network.
- Reboot one camera.
- Stop PostgreSQL.
- Fill image storage to configured threshold in a disposable test.
- Reconnect NVR.

Original CCTV recording must remain available during all sidecar failure tests.

## 7. CI

Every pull request should run:
- `gofmt`
- `go vet`
- Go unit tests
- static analysis
- frontend typecheck/lint/tests
- migration validation
- dependency and license checks
- build for target architecture
