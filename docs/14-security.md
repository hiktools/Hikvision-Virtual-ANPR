# 14 — Security and privacy

## 1. Threat model

Assets include live video, recorded snapshots, number plates, camera credentials, NVR credentials, network configuration, and event history.

Threats include:
- Unauthorised LAN access.
- Compromised camera credentials.
- Credential leakage in logs or URLs.
- Malicious HTTP/XML/RTSP inputs.
- Denial of service via stream or event flooding.
- Cross-camera data access.
- Unauthorised access to plate history.
- Unintended public exposure.

## 2. Authentication and authorisation

- Require an administrator password during first-run setup.
- Store password hashes using a modern password hashing algorithm.
- Use role-based access control for administrator and read-only operator.
- Apply separate credentials to virtual-device endpoints.
- Rate-limit login and authentication failures.
- Use TLS for management interfaces.
- Avoid shared default credentials.

## 3. Secret handling

- Encrypt source-camera credentials at rest using a key stored separately from the database.
- Never log RTSP URLs with credentials.
- Redact Authorization headers, cookies, tokens, and passwords.
- Do not return secrets in API responses.
- Document key rotation and backup implications.

## 4. Network

- No WAN exposure by default.
- Firewall management UI to trusted admin networks.
- Bind database and worker RPC to loopback or private interfaces.
- Restrict virtual-device listeners to the CCTV/NVR network.
- Do not expose camera or NVR interfaces through public reverse proxies.
- Use least-privilege service accounts.

## 5. Input validation

- Bound HTTP body size and XML depth.
- Validate all paths and device identifiers.
- Prevent SSRF by restricting configurable RTSP targets to approved camera subnets or explicit allowlists.
- Prevent arbitrary file writes through snapshot names or event paths.
- Validate media type, dimensions, and decoded image size.
- Apply timeouts to all network operations.

## 6. Privacy and lawful operation

ANPR and vehicle registration data can be personal data depending on the context. The operator must determine the applicable legal basis, transparency obligations, retention, access controls, and data-subject handling under relevant law, including UK GDPR and the Data Protection Act 2018 where applicable.

Provide configurable retention, audit trails, access controls, and deletion/export workflows. Do not add face recognition or unrelated profiling features by default.

## 7. Security testing

- Dependency and container image scanning.
- Static analysis.
- Fuzz XML and HTTP parsers.
- Authentication tests.
- Cross-device authorisation tests.
- SSRF and path traversal tests.
- Rate-limit tests.
- Secret-redaction tests.
- Backup encryption and restore tests.
