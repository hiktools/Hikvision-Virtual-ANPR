# 07 — ISAPI compatibility

## 1. Purpose

Implement only the subset of device API behaviour needed by the target NVR, based on public documentation, captured traffic from authorised devices, and repeatable tests. ISAPI is a family of interfaces; implementing a similarly named endpoint does not prove complete compatibility.

## 2. Research-first workflow

For each candidate endpoint:
1. Identify the NVR request or documented endpoint.
2. Record HTTP method, path, headers, authentication challenge, content type, body, response code, and timing.
3. Remove credentials, tokens, public IPs, and personal data from captures.
4. Identify required fields versus optional fields by controlled tests.
5. Implement an endpoint contract.
6. Add a golden request/response test.
7. Test against the exact target NVR firmware.
8. Record result as `UNVERIFIED`, `PARTIAL`, or `VERIFIED`.

## 3. Endpoint catalogue

Potential device endpoint families to research:
- Device information.
- Device capabilities.
- System time.
- Network information.
- Channel information.
- Video input and stream configuration.
- Snapshot.
- Event subscription.
- Event notification.
- Event search or metadata retrieval.
- ANPR-specific configuration and capabilities.

Do not assume exact paths or schemas. Derive them from current official documentation and captured traffic.

## 4. Authentication

Implement HTTP Digest authentication only according to the relevant standard and tested client behaviour. Requirements:
- Correct challenge and response validation.
- Nonce expiry and replay resistance.
- Configurable realm.
- Constant-time credential comparison where applicable.
- Rate limiting and lockout policy.
- TLS support.
- No credentials in logs.
- Separate accounts or credentials per virtual device where practical.

Avoid hard-coded default passwords. Require an administrator to set credentials on first setup.

## 5. XML handling

- Parse XML with a strict, bounded parser.
- Enforce request body size limits.
- Validate namespace and content type where required.
- Reject malformed or excessively nested input safely.
- Generate XML from typed structures, not string concatenation.
- Preserve unknown extension fields only when safe and required by tested interoperability.
- Include schema/golden-file tests for each supported payload.

## 6. Event subscription direction

The NVR may poll, subscribe to a long-lived event stream, or expect the device to send events to a configured destination. Do not choose one without evidence. Capture the NVR's behaviour with a genuine compatible device or consult exact-version documentation. Build an adapter interface so different delivery directions can be supported.

## 7. Compatibility statement

The UI and docs must distinguish:
- Endpoint implemented.
- Endpoint tested with a generic client.
- Endpoint tested with target NVR.
- Native ANPR indexing verified.

Never label an endpoint "Hikvision compatible" solely because it returns an XML document containing familiar field names.
