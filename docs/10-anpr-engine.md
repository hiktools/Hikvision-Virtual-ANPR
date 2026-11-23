# 10 — ANPR engine

## 1. Pipeline

Separate the following stages:

1. Frame sampling.
2. Vehicle detection.
3. Plate-region detection.
4. Vehicle/plate association.
5. Tracking across frames.
6. Plate crop quality assessment.
7. OCR.
8. Country-format validation.
9. Temporal consensus.
10. Event candidate generation.

Each stage must expose timing and confidence metrics.

## 2. Model runtime

Evaluate CPU-compatible ONNX Runtime and other actively maintained runtimes. Verify licensing, supported operators, architecture, and model redistribution rights before bundling any model.

Do not assume PaddleOCR is automatically the best CPU option. Benchmark alternatives using representative camera footage and the target host CPU.

## 3. UK plate profile

The first country profile is Great Britain/UK registration formats. Implement validation as a configurable ruleset, not a hard-coded universal regex. Historic, personalised, diplomatic, military, and non-standard plates may not follow the common current format.

Validation should adjust confidence or flag a candidate; it must not fabricate or force a string into a valid-looking plate.

## 4. Confidence

Keep separate:
- Vehicle detection confidence.
- Plate detection confidence.
- OCR character/token confidence.
- Temporal consensus confidence.
- Country-format validation result.

Do not combine these into an unexplained single number. If a vendor event format requires a single confidence field, define and document a deterministic mapping.

## 5. Temporal consensus and deduplication

Associate plate observations to a tracked vehicle using time, camera, bounding boxes, and track ID. Use a configurable event grouping window. Keep all raw observations for audit and debugging. A repeated sighting after a configurable cooldown may be a new event.

## 6. CPU strategy

No dedicated GPU is currently available. Use:
- Substream sampling where plate pixels remain sufficient.
- Per-camera inference rate limits.
- Vehicle-triggered plate OCR.
- Bounded queues.
- Model warm-up.
- CPU thread limits.
- Optional batch inference if benchmarked beneficial.
- Per-camera schedules and priorities.

Do not promise 32 simultaneous real-time streams until load tests establish achievable throughput.

## 7. Quality and test data

Build a test corpus with lawful access and appropriate privacy controls. Include:
- Daylight and night.
- Headlights and glare.
- Motion blur.
- Rain and reflections.
- Angled plates.
- Different distances.
- Dirty/obscured plates.
- UK plate variants.
- Empty scenes and false-positive cases.

Measure plate exact-match accuracy, character error rate, false positive rate, missed plate rate, and end-to-end latency. Report sample size and conditions.

## 8. Privacy

Process locally by default. Avoid uploading footage or plate numbers to third-party APIs. Define access controls and retention periods. Plate data can be personal data depending on context and use; the operator must assess applicable data protection obligations.
