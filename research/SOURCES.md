# Research source register

Sources below are starting points. Each technical claim in a device report should cite the exact source that supports it.

## Samsung One UI 9

- Samsung Mobile Press, One UI 9 official rollout announcement (September 16, 2026): https://api.samsungmobilepress.com/press-releases/one-ui-9-official-rollout-latest-galaxy-experiences
  - Primary source for the official rollout announcement and feature descriptions.
- Samsung Newsroom, One UI 9 beta announcement (May 12, 2026): https://news.samsung.com/us/samsung-launches-one-ui-9-beta-galaxy-s26-series-users/
  - Primary source explicitly stating One UI 9 beta is built on Android 17.

## Android platform

- Android Open Source Project: https://source.android.com/
- Android Compatibility Definition Document: https://source.android.com/docs/compatibility/cdd
- Android Compatibility Test Suite: https://source.android.com/docs/compatibility/cts
- Android HAL overview: https://source.android.com/docs/core/architecture/hal
- Android VINTF design: https://source.android.com/docs/core/architecture/vintf
- Android VINTF manifests: https://source.android.com/docs/core/architecture/vintf/objects
- Android VINTF compatibility matrices: https://source.android.com/docs/core/architecture/vintf/comp-matrices
- Android VINTF matching rules: https://source.android.com/docs/core/architecture/vintf/match-rules
- Android VINTF FCM lifecycle: https://source.android.com/docs/core/architecture/vintf/fcm
- AOSP Android 17 release FCM build configuration: https://android.googlesource.com/platform/hardware/interfaces/+/refs/heads/android17-release/compatibility_matrices/Android.bp
- AOSP Android 17 release FCM 7 matrix: https://android.googlesource.com/platform/hardware/interfaces/+/refs/heads/android17-release/compatibility_matrices/compatibility_matrix.7.xml
- AOSP Android 17 release FCM 8 matrix: https://android.googlesource.com/platform/hardware/interfaces/+/refs/heads/android17-release/compatibility_matrices/compatibility_matrix.8.xml
- Android AIDL HAL guidance: https://source.android.com/docs/core/architecture/aidl/aidl-hals
- Android HIDL guidance: https://source.android.com/docs/core/architecture/hidl

## Motorola Moto G60 / hanoip

- Motorola Support, About your hardware — moto g60: https://en-us.support.motorola.com/app/answers/detail/a_id/159299/~/about-your-hardware---moto-g60
- Motorola Hanoip device tree: https://github.com/motorola-sm6150-devs/device_motorola_hanoip
- Hanoip device tree `lineage-20` branch: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/tree/lineage-20
- Hanoip community kernel `lineage-20` branch: https://github.com/motorola-sm6150-devs/android_kernel_motorola_sm6150/tree/lineage-20
- LineageOS SM6150 kernel `lineage-20` branch: https://github.com/LineageOS/android_kernel_motorola_sm6150/tree/lineage-20
- Motorola exact-build kernel-source request #574: https://github.com/MotorolaMobilityLLC/kernel-msm/issues/574
- Motorola exact-build kernel Git tag: https://github.com/MotorolaMobilityLLC/kernel-msm/tree/MMI-S2RIS32.32-20-9-9-2-3
- Motorola exact-build kernel tag target commit: https://github.com/MotorolaMobilityLLC/kernel-msm/commit/f6673a1302e3b9ca84c54e956ad4075d676fdf59
- Moto G60 vendor/HAL source audit: https://github.com/filipdanielisback-commits/HakiSamsung/blob/main/research/MOTO-G60-VENDOR-HAL-AUDIT.md
- Moto G60 Android 17 VINTF comparison: https://github.com/filipdanielisback-commits/HakiSamsung/blob/main/research/MOTO-G60-ANDROID17-VINTF-COMPATIBILITY.md
- Moto G60 Android 17 FCM/HAL matrix: https://github.com/filipdanielisback-commits/HakiSamsung/blob/main/research/MOTO-G60-ANDROID17-FCM-MATRIX.md
- TeamWin Hanoip device configuration: https://github.com/TeamWin/android_device_motorola_hanoip
- Motorola kernel source organization: https://github.com/MotorolaMobilityLLC/kernel-msm
- Motorola Moto-common target list: https://github.com/moto-common/android_device_motorola_targets
- Motorola Hanoip community source organization (archived): https://github.com/moto-hanoip/
- Independent Moto G60 build/version listing (corroboration only): https://www.phonecopy.com/en/phones/view/motorola_moto_g60

## Xiaomi devices

Add official Xiaomi specifications, public kernel sources and device configuration repositories only after the exact model and regional variant have been confirmed.

## Source quality

Prefer manufacturer documentation, official AOSP documentation, and source repositories maintained for the exact device. Secondary articles can help discover leads, but should not be the only evidence for a compatibility claim. Record the exact branch, build, model and date checked. A repository's existence does not prove that it is maintained or compatible with a particular software build.


## Motorola Moto G60 kernel source comparison
- Motorola exact firmware source issue #574: https://github.com/MotorolaMobilityLLC/kernel-msm/issues/574
- Motorola exact kernel tag: https://github.com/MotorolaMobilityLLC/kernel-msm/tree/MMI-S2RIS32.32-20-9-9-2-3
- Motorola exact tag target commit: https://github.com/MotorolaMobilityLLC/kernel-msm/commit/f6673a1302e3b9ca84c54e956ad4075d676fdf59
- Hanoip community kernel branch: https://github.com/motorola-sm6150-devs/android_kernel_motorola_sm6150/tree/lineage-20
- Community Hanoip kernel commit enabling Clang LTO: https://github.com/motorola-sm6150-devs/android_kernel_motorola_sm6150/commit/5b04fac0cb2a8311ae69c8644384426e838d91fe
- LineageOS SM6150 device-family README and supported-device information: https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/README.md
- Detailed Moto G60 kernel source comparison: https://github.com/filipdanielisback-commits/HakiSamsung/blob/main/research/MOTO-G60-KERNEL-SOURCE-COMPARISON.md


## Motorola Moto G60 Android 17 hardware map
- Hanoip device tree BoardConfig.mk: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/BoardConfig.mk
- Hanoip device tree device.mk: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/device.mk
- Hanoip kernel module list: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/modules.load
- Hanoip proprietary file inventory: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/proprietary-files.txt
- Shared Motorola SM6150 VINTF manifest: https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/manifest.xml
- Shared Motorola SM6150 compatibility matrix: https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/compatibility_matrix.xml
- Shared Motorola SM6150 proprietary file inventory: https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/proprietary-files.txt
- Detailed Moto G60 Android 17 hardware and vendor dependency map: https://github.com/filipdanielisback-commits/HakiSamsung/blob/main/research/MOTO-G60-ANDROID17-HARDWARE-DRIVER-MAP.md


## Motorola Moto G60 Android 17 FCM 7 compatibility audit
- Shared Motorola SM6150 device manifest: https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/manifest.xml
- Hanoip BoardConfig.mk: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/BoardConfig.mk
- AOSP Android 17 FCM selection and kernel profile build rules: https://android.googlesource.com/platform/hardware/interfaces/+/android16-qpr2-release/compatibility_matrices/Android.bp
- AOSP FCM 7 framework compatibility matrix: https://android.googlesource.com/platform/hardware/interfaces/+/07df4f22df/compatibility_matrices/compatibility_matrix.7.xml
- AOSP VINTF match rules: https://source.android.com/docs/core/architecture/vintf/match-rules
- AOSP FCM lifecycle: https://source.android.com/docs/core/architecture/vintf/fcm
- Moto G60 Android 17 FCM 7 compatibility audit: https://github.com/filipdanielisback-commits/HakiSamsung/blob/main/research/MOTO-G60-ANDROID17-FCM7-COMPATIBILITY-AUDIT.md

## Motorola Moto G60 One UI 9 requirements
- Samsung One UI 9 beta announcement (Android 17 base): https://news.samsung.com/us/samsung-launches-one-ui-9-beta-galaxy-s26-series-users/
- AOSP Android 17 FCM selection and kernel profile build rules: https://android.googlesource.com/platform/hardware/interfaces/+/android16-qpr2-release/compatibility_matrices/Android.bp
- AOSP VINTF matching rules: https://source.android.com/docs/core/architecture/vintf/match-rules
- AOSP compatibility definition document: https://source.android.com/docs/compatibility/cdd
- AOSP compatibility test suite documentation: https://source.android.com/docs/compatibility/cts
- Moto G60 One UI 9 research requirements checklist: https://github.com/filipdanielisback-commits/HakiSamsung/blob/main/research/MOTO-G60-ONE-UI-9-REQUIREMENTS.md


## Motorola Moto G60 Android 17 merged-manifest and HAL service audit
- Hanoip product makefile: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/lineage_hanoip.mk
- Hanoip device makefile: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/device.mk
- Hanoip BoardConfig: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/BoardConfig.mk
- Hanoip dependencies: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/lineage.dependencies
- Shared SM6150 product configuration: https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/common.mk
- Shared SM6150 BoardConfig: https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/BoardConfigCommon.mk
- Shared SM6150 manifest: https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/manifest.xml
- Shared SM6150 AIDL Power module: https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/power-libperfmgr/Android.bp
- Shared SM6150 AIDL Power VINTF fragment: https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/power-libperfmgr/aidl/android.hardware.power-service.sm6150.xml
- Detailed Moto G60 merged-manifest and HAL service audit: https://github.com/filipdanielisback-commits/HakiSamsung/blob/main/research/MOTO-G60-ANDROID17-MERGED-MANIFEST-SERVICE-AUDIT.md


## Motorola Moto G60 public device/vendor inheritance audit
- Hanoip `lineage-20` device.mk: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/device.mk
- Hanoip `lineage-20` BoardConfig.mk: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/BoardConfig.mk
- Independent Hanoip device-tree fork: https://github.com/bhawesh2002/android_device_motorola_hanoip
- Independent fork device manifest: https://github.com/bhawesh2002/android_device_motorola_hanoip/blob/default/manifest.xml
- Independent fork additional Hanoip manifest: https://github.com/bhawesh2002/android_device_motorola_hanoip/blob/default/manifest_hanoip.xml
- Independent fork BoardConfig: https://github.com/bhawesh2002/android_device_motorola_hanoip/blob/default/BoardConfig.mk
- Independent fork device.mk: https://github.com/bhawesh2002/android_device_motorola_hanoip/blob/default/device.mk
- AOSP VINTF manifest composition rules: https://source.android.com/docs/core/architecture/vintf/objects
- Detailed public device/vendor inheritance audit: https://github.com/filipdanielisback-commits/HakiSamsung/blob/main/research/MOTO-G60-PUBLIC-DEVICE-VENDOR-INHERITANCE-AUDIT.md
