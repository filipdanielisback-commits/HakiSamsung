# Project goals

## Mission

Investigate, with evidence, whether and how parts of the Samsung One UI 9 experience could be made compatible with non-Samsung Android devices. This project starts with research, not a promise of a bootable ROM.

## Scope

- Android 17 platform concepts relevant to One UI 9.
- Publicly documented One UI features and high-level architecture.
- Hardware and vendor-interface differences between target devices.
- Compatibility boundaries between Android framework code, Samsung-specific components, and device-specific implementations.
- Original research tools and independently written code, subject to appropriate licenses.

## Out of scope for the initial milestone

- Distributing Samsung firmware or proprietary binaries.
- Claiming that a target device is supported without reproducible test evidence.
- Publishing a flashable image or asking contributors to test an unverified build on a daily-use device.
- Circumventing security controls or redistributing files contrary to their license.

## Milestones

### M0 — Research foundation
- Create project documentation and contribution rules.
- Establish source-quality and evidence labels.
- Create initial device research records.

### M1 — Device feasibility
- Verify each device's exact model, chipset, Android version and public kernel/device-tree status.
- Identify relevant Android compatibility requirements.
- Record unknowns and blockers.

### M2 — Architecture study
- Map publicly documented Android and One UI layers.
- Separate generic Android components from Samsung-specific components.
- Review licensing before considering any code reuse.

### M3 — Prototype planning
- Only after M0–M2, define a lawful, device-specific prototype scope.
- Require review, backups, recovery planning and appropriate adult/experienced developer supervision before any hardware testing.

## Evidence labels

- **Verified:** supported by a primary source or reproducible test.
- **Reported:** found in a secondary source; not independently confirmed.
- **Hypothesis:** plausible, but not yet demonstrated.
- **Unknown:** evidence is not yet available.
- **Blocked:** a known technical, legal or licensing obstacle exists.
