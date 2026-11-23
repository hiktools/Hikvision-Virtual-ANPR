# 21 — Reference research

The following links were supplied by the project owner. They are research references, not project dependencies or code bases to fork.

## 1. Hikvision SDK viewer

https://github.com/mykhi/hikvision-sdk-viewer

Research questions:
- Which HCNetSDK functions are used?
- Does the viewer connect to the NVR or camera?
- What authentication and transport are required?
- Which behaviour is SDK-specific rather than ISAPI?
- Does the target NVR require SDK interaction for native ANPR records?

Do not assume an SDK viewer demonstrates a camera-to-NVR event protocol.

## 2. go2rtc ISAPI documentation

https://github.com/AlexxIT/go2rtc/blob/master/internal/isapi/README.md

Research questions:
- Which ISAPI interactions are implemented?
- Is the functionality a camera source/backchannel client or a server-side emulator?
- What authentication and event behaviour are covered?
- Which parts are unrelated to ANPR?

Do not treat a client implementation as proof of a server emulator.

## 3. Hikvision best practices

https://github.com/uchkunr/hikvision-best-practices

Research questions:
- Which ISAPI endpoint examples are documented?
- How is Digest authentication handled?
- How are multipart event streams parsed?
- Which examples are based on actual devices?
- Are versions and firmware assumptions stated?

Use it as a learning aid. Independently verify endpoint contracts.

## 4. Hikvision virtual cam

https://github.com/shranet/hikvision-virtual-cam

Research questions:
- How are logical devices represented?
- What discovery behaviour is implemented?
- How are RTSP endpoints served?
- What device-information and snapshot endpoints exist?
- What limitations are documented?

Do not copy or import source code. Build an independent implementation from documented requirements and observed protocol behaviour.

## 5. Research record format

For each finding record:
- URL and retrieval date.
- Repository commit/tag or documentation version.
- Exact file/section.
- Behaviour described.
- Whether it is client-side or server-side.
- Whether it is generic or model-specific.
- Independent verification method.
- Licence implications if any code is considered.

## 6. Official documentation

Prioritise current official Hikvision ISAPI and target NVR documentation where available. Record document revision and model applicability. Public documentation may not cover proprietary or firmware-specific behaviour; use authorised lab testing to close gaps.
