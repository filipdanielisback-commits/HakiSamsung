# Moto G60 XT2135-2 RETEU — kernel and device-tree source audit

**Audit scope:** Public source metadata and build-configuration review only. No firmware images, proprietary vendor files, device modifications or flashing.

## Target identity

- Device: Motorola Moto G60, XT2135-2
- Region/channel: Europe / RETEU
- RAM: 6 GB (reported by device owner)
- Installed Android: Android 12 (confirmed by device owner)
- Build: `S2RIS32.32-20-9-9-2-3`
- Platform codename: hanoip

## 1. Motorola's matching kernel-source tag

Motorola's kernel repository contains the annotated Git tag:

- Tag: `MMI-S2RIS32.32-20-9-9-2-3`
- Tag object: `f3db6b21a89490d61f530d40584af8c6c9221cae`
- Target commit: `f6673a1302e3b9ca84c54e956ad4075d676fdf59`
- Tag message: `Hanoip push for Android 12`
- Tagger date: 2023-12-07
- Tag signature status: unsigned

The matching Motorola issue #574 names the exact XT2135-2 RETEU Android 12 firmware package and is closed with the `published` label. A Motorola contributor's comment links to this tag.

References:
- Issue: https://github.com/MotorolaMobilityLLC/kernel-msm/issues/574
- Exact Git tag: https://github.com/MotorolaMobilityLLC/kernel-msm/tree/MMI-S2RIS32.32-20-9-9-2-3
- Tag object: https://api.github.com/repos/MotorolaMobilityLLC/kernel-msm/git/tags/f3db6b21a89490d61f530d40584af8c6c9221cae
- Target commit: https://github.com/MotorolaMobilityLLC/kernel-msm/commit/f6673a1302e3b9ca84c54e956ad4075d676fdf59

Important distinction: GitHub's Releases API currently returns no release entries for this repository, but the exact Git tag itself exists. The earlier not-found result was from querying the Releases endpoint, not evidence that the Git tag was missing.

**Finding:** A manufacturer-published kernel source tag exists for the exact build family and Android 12. The tag is a strong provenance match. A complete source build has not been attempted or validated.

## 2. Public hanoip device tree

Repository: https://github.com/motorola-sm6150-devs/device_motorola_hanoip

Branch reviewed: `lineage-20`

Branch head:
- Commit: `514e0261a117a0c7e7fa3cc3cd44f80819b0b779`
- Commit date: 2023-10-21
- Commit message: `hanoip: Set default QR code scanner`

Relevant configuration findings from this branch:

- `TARGET_BOARD_PLATFORM := sm6150`
- `TARGET_BOOTLOADER_BOARD_NAME := hanoip`
- `TARGET_KERNEL_SOURCE := kernel/motorola/sm6150`
- Kernel config fragments include `vendor/sdmsteppe-perf_defconfig`, `vendor/ext_config/moto-sdmsteppe.config`, and `vendor/ext_config/moto-sdmmagpie-hanoip.config`.
- `device.mk` inherits proprietary vendor files under `vendor/motorola/hanoip`.
- `BoardConfig.mk` also expects separate `hanoip-firmware` and `hanoip-vendor` trees.
- The device configuration sets shipping API level 30 and target VNDK version 30.

References:
- Device tree: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/tree/lineage-20
- Board configuration: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/BoardConfig.mk
- Product configuration: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/device.mk
- Dependencies: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/lineage.dependencies

## 3. Community kernel tree expected by the device configuration

The device tree's kernel path corresponds to the public repository:

- Repository: https://github.com/motorola-sm6150-devs/android_kernel_motorola_sm6150
- Branch: `lineage-20`
- The repository is a fork of `sm6150-motorola/android_kernel_motorola_sm6150`.
- LineageOS also has a public SM6150 kernel repository on a `lineage-20` branch: https://github.com/LineageOS/android_kernel_motorola_sm6150/tree/lineage-20

This establishes a plausible community source pairing at the repository/branch level. It does **not** prove that the community branch is byte-for-byte or commit-for-commit equivalent to Motorola's exact Android 12 tag. The branch histories and source contents need a controlled comparison before any compatibility claim.

## 4. Compatibility implications

1. The exact Motorola Android 12 kernel tag is now identified.
2. The public hanoip device tree and its expected kernel source path are identified.
3. The reviewed community device tree is on a `lineage-20` branch, while the user's installed stock build is Android 12. Treat the community tree as a separate ROM development baseline, not as proof that its proprietary vendor assumptions match the user's installed build.
4. The device configuration depends on proprietary vendor and firmware trees. Those are not included in this research repository and must not be copied here without appropriate rights.
5. Shipping API level 30 / VNDK 30 are recorded as build-configuration values. They are not evidence of One UI 9 compatibility.

## 5. Current assessment

**Kernel source provenance: strong match at tag level.**  
**Community device-tree/kernel pairing: identified, exact source equivalence not established.**  
**One UI 9 compatibility: unknown; no port or build is claimed.**

The next research task is to compare public source metadata and documented interfaces, then map Samsung-specific framework and service dependencies against AOSP/Android requirements. No firmware extraction, boot modification, or flashing is part of this milestone.
