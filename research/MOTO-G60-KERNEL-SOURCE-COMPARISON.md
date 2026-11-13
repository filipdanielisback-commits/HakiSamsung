# Moto G60 kernel source comparison — Motorola vs Hanoip community tree

**Target:** Motorola Moto G60 XT2135-2 (hanoip), RETEU  
**Exact user build:** `S2RIS32.32-20-9-9-2-3`  
**Motorola source tag:** `MMI-S2RIS32.32-20-9-9-2-3`  
**Community source:** `motorola-sm6150-devs/android_kernel_motorola_sm6150`, branch `lineage-20`  
**Scope:** Read-only public-source comparison. No device modification, flashing, firmware extraction, or proprietary binary handling.

## 1. Result at a glance

The public Hanoip community kernel is **not a newer-kernel route** for this project. Its top-level Makefile reports Linux **4.14.190**, the same upstream kernel version as Motorola's exact-build source tag.

Several important shared build/configuration files are byte-for-byte identical between the Motorola tag and the community branch. This supports a common source lineage for those files, but it does not prove the entire trees are identical or that the community kernel is a complete substitute for the exact stock build.

## 2. Motorola exact-build source

Motorola issue #574 is explicitly tied to the firmware package for the exact user-reported build:
`XT2135-2_HANOIP_RETEU_12_S2RIS32.32-20-9-9-2-3_subsidy-DEFAULT_regulatory-XT2135-2-EUROPE_CFC.xml.zip`

- Issue: https://github.com/MotorolaMobilityLLC/kernel-msm/issues/574
- Exact tag: https://github.com/MotorolaMobilityLLC/kernel-msm/tree/MMI-S2RIS32.32-20-9-9-2-3
- Tag target commit: https://github.com/MotorolaMobilityLLC/kernel-msm/commit/f6673a1302e3b9ca84c54e956ad4075d676fdf59
- The tag target's top-level Makefile states `VERSION = 4`, `PATCHLEVEL = 14`, `SUBLEVEL = 190`.

The exact source is therefore Linux 4.14.190.

## 3. Community kernel version and history

Reviewed repository:
https://github.com/motorola-sm6150-devs/android_kernel_motorola_sm6150/tree/lineage-20

- The community branch's top-level Makefile also states Linux 4.14.190.
- The LineageOS SM6150 kernel repository uses branch `lineage-20` and describes the device-family kernel as the SM6150 kernel.
- The community kernel's relevant visible commit `5b04fac0cb2a8311ae69c8644384426e838d91fe` is dated 2023-10-24 and is titled `hanoip_defconfig: Enable Clang LTO`.
- That commit adds `CONFIG_LTO_CLANG=y` to `arch/arm64/configs/vendor/hanoip_defconfig`.

This is a configuration/build change, not a kernel version upgrade. The community branch does not move this device to Linux 4.19, 5.4, 5.10, or later.

## 4. Byte-for-byte file comparison

The following files have identical Git blob SHA values in the Motorola exact-build tag and the community `lineage-20` branch:

| File | Blob SHA | Result |
|---|---|---|
| `arch/arm64/configs/vendor/sdmsteppe-perf_defconfig` | `e0ba73e9372781c77aeb442effacff35158dea0a` | Identical (730 lines) |
| `arch/arm64/configs/vendor/ext_config/moto-sdmmagpie-hanoip.config` | `fd170fa7ecdf416f45701dff18521dd0f4717f97` | Identical (81 lines) |
| `arch/arm64/configs/vendor/debugfs.config` | same source blob in both trees | Identical |
| `drivers/soc/qcom/Kconfig` | `9aeab2cfac05278d88baf545b5b8102d5a8edb9b` | Identical |
| `build.config.common` | `11bc1025114269d0d933ffd741ae6db4eccd2955` | Identical |
| `AndroidKernel.mk` | `3ab7b9b23615d0ff93fc8f69a2c43e09fe0b2c59` | Identical |

The identical Hanoip-specific extension config includes `CONFIG_HANOIP_DTB=y`. The identical SM6150 performance defconfig establishes that the shared base configuration is not newer in the community tree.

### Important limitation

Identical selected files do not mean the full source trees are identical. A full source comparison would require a complete, correctly aligned tree comparison and build provenance. We should not infer binary equivalence or exact-stock equivalence from a handful of matching blobs.

## 5. Notable differences

### Top-level Makefile

The Makefile blobs differ. The reviewed diff includes changes to host compiler selection and LLVM tool handling (for example, selecting Clang/LLVM tools when `LLVM` is enabled). This is a build-system/toolchain difference, not evidence of a newer Linux kernel baseline.

### Community Hanoip defconfig

The community repository contains an additional `arch/arm64/configs/vendor/hanoip_defconfig` file. Its header sets `CONFIG_LOCALVERSION="-lineageos"`, and it includes `CONFIG_LTO_CLANG=y`. The October 24, 2023 community commit above enables that option.

The community device tree's `BoardConfig.mk` separately lists the kernel configuration inputs:
- `vendor/sdmsteppe-perf_defconfig`
- `vendor/ext_config/moto-sdmsteppe.config`
- `vendor/ext_config/moto-sdmmagpie-hanoip.config`
- `vendor/debugfs.config`
- `vendor/ext_config/lineage-moto-sdmsteppe.config`

Therefore, do not assume the standalone `hanoip_defconfig` is the configuration used by that device-tree build just because it exists in the kernel repository. The declared config list is the relevant evidence, and the exact generated build configuration is not available in this public source review.

## 6. Relevance to Android 17 / One UI 9

This comparison does not resolve the Android 17 kernel compatibility concern. It reinforces it:

1. Motorola's exact-build kernel source is 4.14.190.
2. The public Hanoip community kernel is also 4.14.190.
3. The key shared device/kernel configuration files are identical.
4. The visible community-specific changes are not a newer kernel baseline.
5. The Android 17 release FCM profiles reviewed in the previous report start at newer kernel families; no 4.14 profile was listed.

A newer Linux kernel would not automatically make the device compatible anyway. The device-specific drivers, DTB/DTBO, vendor modules, kernel interfaces, and VINTF declarations would still need to be supported and validated. This report does not propose a kernel upgrade or provide device-modification steps.

## 7. Assessment

<badge>Research conclusion</badge>

**No public evidence found in the reviewed Motorola and Hanoip community kernel repositories establishes a newer Android-17-ready kernel path for Moto G60.** The community source remains 4.14.190 and shares key configuration files with Motorola's exact-build source.

This is a negative finding about the two reviewed public source trees, not proof that no other source or engineering path exists anywhere. The next research gate is to map the device-specific kernel/vendor interface constraints at a documentation level and determine whether the Android 17 requirements can be met without assuming a kernel upgrade.

No port, successful build, or working compatibility result is claimed.
