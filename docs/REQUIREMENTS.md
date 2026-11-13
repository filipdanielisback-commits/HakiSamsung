# Research requirements

This file describes what must be understood before any device-specific prototype can be responsibly proposed. It is not a flashing guide.

## Information required per device

- Exact commercial model and regional variant.
- SoC and GPU, verified from manufacturer or reliable technical documentation.
- Current Android version and build fingerprint, when voluntarily supplied by a device owner.
- Public kernel source availability and applicable license.
- Public device-tree and vendor-interface documentation, if available.
- Partition and boot architecture facts from reputable public documentation.
- Known hardware differences between variants.

## Platform requirements to investigate

- Android 17 platform compatibility.
- Framework and system-service dependencies.
- Hardware abstraction layer (HAL) interfaces.
- Kernel and driver compatibility.
- Graphics, audio, camera, sensors, modem and connectivity support.
- Verified boot, signing and security model.
- Device-specific power management and thermal behaviour.
- Samsung account/service dependencies and region limitations.

## Evidence required before changing a status

A device may only be marked **Prototype tested** or **Boot verified** when a report includes:
- Exact device model and hardware variant.
- Exact software/build identifiers.
- Test environment and date.
- What was tested and what was not tested.
- Logs or measurements that can be shared lawfully, with personal data removed.
- Independent review where practical.

## Safety and data protection

- Early work should remain documentation, source review and static analysis.
- Do not use an everyday or essential phone as an experimental test device.
- Do not publish instructions that encourage unreviewed modification of a device's boot or security configuration.
- Keep personal identifiers, account data, serial numbers and private logs out of public issues.
- Any later physical-device testing must be planned and supervised by an experienced adult developer, with an appropriate recovery plan.

## Legal and licensing requirements

- Do not commit Samsung firmware, proprietary apps, vendor blobs, keys or other restricted binaries.
- Check the license of every external source file before reuse.
- Preserve required copyright and license notices.
- Prefer links, metadata and hashes over copying third-party artifacts.
- Ask the rights holder when redistribution permissions are unclear.
