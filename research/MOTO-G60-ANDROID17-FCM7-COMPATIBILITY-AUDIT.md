# Moto G60: Android 17 FCM 7 and legacy-manifest compatibility audit

**Status:** public-source comparison; no device modification or firmware extraction  
**Device under study:** Moto G60 XT2135-2 / hanoip  
**Community baseline:** LineageOS 20 shared SM6150 manifest and Hanoip device tree  
**Framework target:** Android 17 release branch / One UI 9 Android 17 base

## Executive result

The public SM6150 shared device manifest declares **target-level 4**. The Android 17 framework build rules reviewed here install an FCM 6 Android 17 compatibility matrix and FCM 7, 8, 202404, 202504 and 202604 matrices; they do not install FCM 4. Android's VINTF rules require the device manifest target level to match a framework matrix level. Therefore, the existing public community manifest is not a drop-in VINTF match for Android 17.

This does **not** prove a Moto G60 could never be adapted. It means the current manifest cannot simply be carried forward unchanged. A hypothetical newer target level would need a newly compatible device-side HAL contract and must meet the selected Android 17 matrix. The exact stock RETEU manifest remains unknown.

There is a second major concern: the Android 17 build rules associate FCM 6's Android 17 matrix with kernel configuration profiles 4.19, 5.4 and 5.10; FCM 7 profiles are 5.10 and 5.15. The documented Motorola kernel baseline for the user's exact build is Linux 4.14.190. That is below the oldest listed profile. This is a serious kernel compatibility concern, not a complete proof of impossibility or a statement that a kernel version number alone determines all compatibility.

## Sources checked

- Shared SM6150 [device manifest](https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/manifest.xml), branch `lineage-20`.
- Hanoip [BoardConfig.mk](https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/BoardConfig.mk).
- AOSP [Android 17 FCM build rules](https://android.googlesource.com/platform/hardware/interfaces/+/android16-qpr2-release/compatibility_matrices/Android.bp), including the Android 17 matrix selection and kernel profile declarations.
- AOSP [FCM 7 matrix](https://android.googlesource.com/platform/hardware/interfaces/+/07df4f22df/compatibility_matrices/compatibility_matrix.7.xml).
- AOSP [VINTF match rules](https://source.android.com/docs/core/architecture/vintf/match-rules).
- AOSP [FCM lifecycle](https://source.android.com/docs/core/architecture/vintf/fcm).
- Motorola exact-build kernel tag [MMI-S2RIS32.32-20-9-9-2-3](https://github.com/MotorolaMobilityLLC/kernel-msm/tree/MMI-S2RIS32.32-20-9-9-2-3), whose Makefile identifies Linux 4.14.190.

## 1. Target-level mismatch

The shared SM6150 community manifest starts with:

- `type="device"`
- `target-level="4"`

AOSP's Android 17 build rules select these framework matrices for Android 17 builds:

- FCM 6 Android 17 variant
- FCM 7
- FCM 8
- FCM 202404
- FCM 202504
- FCM 202604

FCM 4 is not in that list. The Android VINTF matching rules state that the manifest's target FCM level must exactly match the framework compatibility matrix level selected for it.

**Finding:** the shared community manifest at target-level 4 cannot match an Android 17 framework matrix as-is.

**Important qualification:** the shared community manifest is not proven to be the final merged stock manifest for XT2135-2 RETEU. Also, a future device-side compatibility implementation could in principle target a newer FCM, but it would have to meet that newer matrix. That would be a substantial compatibility project, not a metadata-only edit.

## 2. FCM 7 mandatory HAL comparison

The AOSP FCM 7 matrix marks the following HALs as required (not optional):

| FCM 7 required interface | Community SM6150 manifest evidence | Preliminary assessment |
|---|---|---|
| `android.hardware.audio` HIDL 6.0 or 7.0-1 | HIDL 6.0 declared | Version appears within the FCM 7 accepted alternatives |
| `android.hardware.audio.effect` HIDL 6.0 or 7.0 | HIDL 6.0 declared | Version appears within the FCM 7 accepted alternatives |
| `android.hardware.gatekeeper` HIDL 1.0 | HIDL 1.0 declared | Interface is present in this manifest |
| `android.hardware.graphics.mapper` HIDL 2.1, 3.0 or 4.0, or AIDL 1 | No matching mapper entry found in this shared manifest | Not evidenced here; may be declared by another merged manifest or supplied by another component |
| `android.hardware.health` AIDL 1 | Shared manifest declares `motorola.hardware.health` HIDL 1.0, not this AIDL interface | Not evidenced here; a compatible AIDL implementation or another declaration would be needed |
| `android.hardware.power` AIDL 2-3 | No matching AIDL power entry found in this shared manifest | Not evidenced here; may be supplied elsewhere |
| `android.hardware.thermal` HIDL 2.0 | No matching thermal entry found in this shared manifest | Not evidenced here; must verify merged vendor/ODM manifests and service inventory |

The FCM 7 matrix lists camera provider HIDL 2.4-7 and AIDL 1 as optional alternatives. The community manifest's camera provider HIDL 2.4 is therefore within the matrix's stated version range. The matrix also lists optional Bluetooth HIDL 1.0-1, matching the community manifest's Bluetooth 1.0 declaration.

This comparison only tests declarations visible in one shared manifest against the FCM 7 XML. It does not prove that all required services exist, that the interfaces are operational, or that the final merged manifest is complete. Some HAL declarations can live in vendor or ODM manifests outside this shared source file.

## 3. Kernel-profile concern

The Android 17 AOSP build rules associate these profiles with the matrices:

| Matrix | Kernel configuration profiles listed by AOSP |
|---|---|
| FCM 6 Android 17 variant | 4.19, 5.4, 5.10 |
| FCM 7 | 5.10, 5.15 |
| FCM 8 | 5.15, 6.1 |
| FCM 202404 | 6.1, 6.6 |
| FCM 202504 | 6.12 |
| FCM 202604 | 6.12 |

The Motorola exact-build kernel source tag is Linux 4.14.190. That kernel version is older than every profile listed for the Android 17 FCM 6 variant. The existing Hanoip community kernel is also based on 4.14.190.

**Finding:** the existing 4.14.190 baseline is a major obstacle to matching the Android 17 kernel profiles. Clang LTO or build-system changes do not change the underlying Linux kernel version.

This profile comparison is not a complete kernel configuration audit and does not establish whether any carefully engineered alternative kernel path exists. It does show that simply reusing the published kernel baseline is not supported by the profiles listed in the Android 17 build rules.

## 4. What currently looks promising versus difficult

**Some legacy HAL versions remain within FCM 7's accepted ranges:**
- Audio HIDL 6.0
- Audio effects HIDL 6.0
- Camera provider HIDL 2.4
- Bluetooth HIDL 1.0

**The shared manifest does not currently evidence several FCM 7 mandatory contracts:**
- Graphics mapper
- AIDL health
- AIDL power
- Thermal HIDL 2.0

These may be provided in other manifests or through components not represented in this file. The next evidence needed is a source-level audit of the complete merged manifest and service declarations, not assumptions based on this one XML file.

**The largest architectural concern remains the kernel baseline:** Linux 4.14.190 is older than the Android 17 kernel profiles listed for even its retained FCM 6 Android 17 matrix.

## 5. Revised project assessment

- **Current public community manifest unchanged:** does not match an Android 17 FCM level.
- **Newer FCM target:** theoretically conceivable only with a compatible set of device-side HAL contracts; not demonstrated.
- **Existing 4.14.190 kernel:** major concern against Android 17's published kernel profiles.
- **Graphics, camera, audio and radio vendor stack:** still needs exact-build and service-level validation.
- **Genuine One UI 9:** no working port exists and feasibility remains unproven.

The research does not support declaring the project impossible. It does support saying that this is substantially more than adapting Samsung's interface layer: the current kernel and vendor interface baseline would need major compatibility work.

## 6. Next research task

Audit all public Hanoip and shared SM6150 source declarations that could supply the FCM 7 required mapper, health, power and thermal interfaces. Distinguish:
1. declarations in the shared manifest;
2. declarations in device-specific/vendor manifest XML files referenced by source;
3. service packages or implementations mentioned by build files;
4. unknown items that cannot be confirmed from public source.

Do not treat a missing declaration in one XML file as proof the HAL is absent from the final device image.

## Scope and licensing boundary

This report is based on public source and documentation only. It does not involve flashing, bootloader changes, firmware extraction, partition access, or proprietary binary redistribution. Proprietary firmware and vendor binaries are not treated as freely redistributable.
