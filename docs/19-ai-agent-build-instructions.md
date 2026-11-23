# 19 — Instructions for AI coding agents

You are implementing Hikvision Virtual ANPR, an original greenfield project. Read this file and the relevant architecture pages before changing code.

## 1. Absolute rules

1. Write original implementation code. The supplied repositories are research references only; do not copy their source files, vendor code, or large code fragments.
2. Do not fork or import a reference repository as a runtime dependency.
3. Do not invent Hikvision endpoint paths, XML schemas, SADP packet structures, SDK behaviour, or NVR capabilities.
4. Label unknown protocol behaviour `UNVERIFIED`.
5. Never claim native ANPR compatibility until the target NVR's native plate-search UI retrieves the event and associated images.
6. Do not alter the user's live NVR/camera configuration without explicit approval and a rollback plan.
7. Keep the physical camera-to-NVR recording path operational during development.
8. Never log passwords, tokens, RTSP credential URLs, or unredacted authentication headers.
9. Keep queues and buffers bounded.
10. Add tests and documentation with every feature.

## 2. Before coding

- Inspect repository state and existing code.
- Read README and relevant docs.
- Identify current milestone and acceptance criteria.
- Search official protocol documentation and record sources.
- Ask for missing information if implementation depends on an unknown protocol or network fact.
- Propose a small plan before broad refactors.

## 3. Coding standards

- Go code must be formatted with `gofmt`.
- Use context cancellation for long-running operations.
- Use explicit timeouts for network calls.
- Avoid global mutable state.
- Wrap errors with useful context.
- Use typed domain models.
- Keep vendor-specific structures in adapter packages.
- Validate all external input.
- Do not use unbounded goroutines, channels, or queues.
- Keep configuration backwards-compatible where practical.
- Write migrations for database schema changes.

## 4. Protocol implementation workflow

For each endpoint or event:
1. State what evidence supports it.
2. Define request and response contract.
3. Implement parser/serializer.
4. Add synthetic fixture tests.
5. Add integration test.
6. Test against target NVR where possible.
7. Update compatibility matrix with exact model/firmware.
8. State limitations.

Do not infer a complete camera protocol from a successful snapshot endpoint.

## 5. Media implementation workflow

- Start with one stream.
- Verify passthrough before adding inference.
- Avoid transcoding unless required.
- Keep inference sampling independent from stream forwarding.
- Drop stale frames instead of allowing unbounded latency.
- Add reconnection and health tests.

## 6. ANPR implementation workflow

- Separate detection and OCR.
- Preserve raw OCR.
- Keep confidence components separate.
- Do not force low-confidence text into a valid plate.
- Test day/night, motion, angle, blur, glare, and non-plate negatives.
- Record model versions and benchmark conditions.
- Keep inference local by default.

## 7. Change management

For every milestone:
- Create a short implementation plan.
- Make small commits.
- Run tests.
- Report changed files.
- Report known gaps.
- Update relevant documentation.
- Do not silently change the network plan or target platform.

## 8. Definition of done

A task is done only when:
- Acceptance criteria pass.
- Tests are added and pass.
- Errors and edge cases are handled.
- Security implications are considered.
- Documentation is updated.
- Compatibility claims match actual evidence.
