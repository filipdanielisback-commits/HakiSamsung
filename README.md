# HakiSamsung

**Research project exploring the compatibility of Samsung One UI 9 with non-Samsung Android devices.**

HakiSamsung is an independent community research project. It studies Android 17, Samsung's One UI software architecture, device-specific hardware support, and the technical and licensing constraints involved in adapting a Samsung-based experience to devices from other manufacturers.

> **Status: Research only — no working port or supported device is claimed.**
>
> One UI 9 is Samsung proprietary software. This repository does not distribute Samsung firmware, proprietary applications, vendor blobs, boot images, or other copyrighted binaries. References to Samsung software are for identification and compatibility research only.

## Goals

- Document the architecture and publicly documented features of One UI 9.
- Evaluate hardware and software compatibility one device at a time.
- Maintain evidence-based device reports, with unknowns clearly marked.
- Explore original, independently written compatibility components where legally and technically appropriate.
- Keep research reproducible through source links, version notes, and test reports.

## Initial device targets

| Device | Initial research focus | Status |
|---|---|---|
| Motorola Moto G60 (hanoip) | Android base, Qualcomm platform, device tree and vendor-interface research | Not evaluated |
| Xiaomi 15T | MediaTek platform, Android base, device tree and vendor-interface research | Not evaluated |
| Other devices | Added after basic hardware and software information is verified | Not evaluated |

A device appearing in this table is **not** a claim that One UI 9 can boot on it.

## Repository map

- docs/ — project goals, architecture notes and research requirements
- devices/ — per-device compatibility reports
- samsung-firmware/ — firmware research policy and metadata templates; no firmware binaries
- research/ — sources, terminology and investigation notes
- LICENSES/ — third-party notices and licensing records

## Research principles

1. **No unsupported claims.** Distinguish verified facts, hypotheses and unknowns.
2. **Respect licensing.** Do not upload or redistribute Samsung proprietary files without explicit permission.
3. **Protect devices and data.** Early work is documentation and static analysis; do not treat experimental builds as safe for daily use.
4. **Preserve provenance.** Record the source, version, date and checksum for any lawfully obtained research artifact, without publishing restricted files.
5. **Be transparent.** Clearly label any future build as experimental and list what has and has not been tested.

## Current milestone

**Milestone 0 — Documentation and feasibility research.**

No ROM, installer, kernel, recovery image or flashable package is provided by this repository.

## Contributing

Please read CONTRIBUTING.md before opening an issue or pull request. Device reports should include reliable sources and label unknown information explicitly.

## Disclaimer

HakiSamsung is an independent, unofficial project and is not affiliated with, endorsed by, or sponsored by Samsung Electronics, Motorola, Xiaomi, Google, or their affiliates. Product and company names are used only to identify the software and hardware being researched.