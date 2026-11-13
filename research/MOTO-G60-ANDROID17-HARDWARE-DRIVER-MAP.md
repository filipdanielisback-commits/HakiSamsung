# Moto G60 (hanoip): Android 17 hardware and vendor dependency map

**Status:** public-source research only  
**Target:** Motorola Moto G60 XT2135-2, Europe RETEU, user-reported Android 12 build S2RIS32.32-20-9-9-2-3

## Executive summary

The public Hanoip Android 13-era device tree shows a Qualcomm SM6150 platform with several hardware stacks that depend on device-specific kernel modules and Qualcomm/Motorola userspace components.

- **Graphics/display:** Adreno EGL/GLES/Vulkan userspace libraries, Qualcomm display post-processing services and calibration components.
- **Camera:** a HIDL camera provider declaration in the shared Motorola SM6150 manifest, plus Motorola/ArcSoft camera extensions and calibration libraries.
- **Audio:** a substantial Qualcomm audio-DSP/kernel-module family, codec-related modules and calibration dependencies.
- **Radio/IMS:** Qualcomm radio interfaces, IMS services and Motorola telephony framework components.
- **Connectivity/location:** Qualcomm WLAN module mapping, Bluetooth service components and Qualcomm GNSS/location libraries.

This is evidence of the **community Android configuration**, not a verified inventory of the user's exact stock Android 12 installation. The public Hanoip proprietary list says its unpinned blobs come from S2RI32.32-20-9-9-2; the shared SM6150 list identifies S2RI32.32-20-7. Neither is the exact user-reported build. The actual stock vendor manifest, exact HAL versions, and installed firmware remain unverified.

## Sources and scope

Reviewed public source files:

1. Hanoip device tree, LineageOS 20 branch:
   - [BoardConfig.mk](https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/BoardConfig.mk)
   - [device.mk](https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/device.mk)
   - [modules.load](https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/modules.load)
   - [proprietary-files.txt](https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/proprietary-files.txt)
   - [lineage.dependencies](https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/lineage.dependencies)
2. Shared Motorola SM6150 device configuration, LineageOS 20 branch:
   - [manifest.xml](https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/manifest.xml)
   - [compatibility_matrix.xml](https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/compatibility_matrix.xml)
   - [proprietary-files.txt](https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/proprietary-files.txt)
3. Android 17 AOSP compatibility references:
   - [Android 17 release compatibility matrix build rules](https://android.googlesource.com/platform/hardware/interfaces/+/refs/heads/android17-release/compatibility_matrices/Android.bp)
   - [Android 17 FCM 7 matrix](https://android.googlesource.com/platform/hardware/interfaces/+/refs/heads/android17-release/compatibility_matrices/compatibility_matrix.7.xml)
   - [VINTF match rules](https://source.android.com/docs/core/architecture/vintf/match-rules)
   - [FCM lifecycle](https://source.android.com/docs/core/architecture/vintf/fcm)

The public Hanoip tree identifies board platform sm6150, bootloader board name hanoip, and OTA assert device hanoip. Its build configuration targets 64-bit ARM and declares Android shipping API level 30 / VNDK 30. These are community build declarations, not direct readings from the stock phone.

## Hardware and software dependency map

| Area | Public-source evidence | What it implies | Android 17 status |
|---|---|---|---|
| Graphics / GPU | Shared SM6150 blob list includes Adreno EGL, GLES and Vulkan userspace libraries, Adreno utility libraries, and Adreno GPU firmware filenames including a618_gmu.bin. Hanoip tree has Qualcomm display post-processing libraries and color HAL components. | Graphics is not just a kernel GPU driver; it also relies on userspace graphics libraries, firmware, display HAL/service interfaces and calibration/post-processing. | **High-risk / unverified.** Exact stock graphics HAL declarations and library compatibility with Android 17 are not established. |
| Camera | Shared SM6150 manifest declares android.hardware.camera.provider HIDL 2.4 (legacy/0). Hanoip blob list contains Motorola camera framework/extensions and ArcSoft calibration/verification libraries. | Camera depends on provider HAL, Qualcomm camera stack, sensor-specific configuration/calibration, firmware and Motorola/third-party userspace components. | **High-risk / unverified.** Public sources do not establish an Android 17-compatible camera provider or prove the listed camera blobs match the exact stock build. |
| Audio | Hanoip modules.load lists many audio modules: ADSP loader, APR, Q6, WCD codec/macro, soundwire, MBHC and smart amplifier modules. BoardConfig.mk maps renamed audio modules. Shared manifest declares HIDL audio 6.0 and audio-effect 6.0. | Audio spans kernel modules, DSP firmware, codec/amp drivers, ACDB calibration, audio HAL and policy/configuration. | **High-risk / unverified.** Android 17 framework requirements and the exact stock audio HAL/firmware pairing have not been validated. |
| Cellular radio / IMS | Shared SM6150 manifest declares Qualcomm radio-related HIDL interfaces including IMS 1.7, QTI radio, UIM/LPA and OEM hook interfaces. Hanoip tree includes QTI telephony wrappers, IMS packages and Motorola telephony framework components. | Modem operation requires matching kernel/firmware, RIL/QMI interfaces, radio HALs, IMS services and carrier/device configuration. | **High-risk / unverified.** No proof this legacy vendor interface set satisfies Android 17 requirements or that Samsung telephony/IMS services can use it. |
| Wi-Fi | Hanoip BoardConfig.mk maps wlan.ko to qca_cld3_wlan.ko; modules.load includes qca_cld3_wlan.ko. Shared manifest includes QTI hostapd/supplicant interfaces. | Wi-Fi needs a compatible kernel module, firmware, userspace supplicant/hostapd and vendor HAL/service interfaces. | **Unverified.** The kernel module name alone does not prove Android 17 interface compatibility. |
| Bluetooth | Shared SM6150 blob list includes Qualcomm Bluetooth service and implementation libraries; the shared manifest declares Bluetooth HIDL 1.0. | Bluetooth requires controller firmware, transport/driver support and a compatible Bluetooth HAL/service. | **Unverified.** Exact stock declarations and Android 17 compatibility are unknown. |
| GNSS / location | Shared SM6150 blob list includes Qualcomm GNSS service libraries and location stack components; a GNSS 4.0 service manifest file appears in the list with a removal marker. | Location depends on modem/location services, vendor libraries and the declared GNSS HAL. | **Unverified.** The removal marker is not proof of the final installed manifest; exact stock GNSS HAL is unknown. |
| Sensors | Hanoip module list includes sensors_class.ko; shared blob list includes Qualcomm sensor-related userspace components and configuration files. | Sensor support combines kernel drivers, sensor DSP/SSC services, calibration and Android sensor HAL. | **Unverified.** Exact sensor HAL version and Android 17 compatibility are not established. |
| Security / keystore | Shared SM6150 blob list includes Qualcomm Keymaster 4.1 service and QSEE-related libraries. | Secure key operations depend on TEE/firmware, vendor services and Android keystore interfaces. | **High-risk / unverified.** No verified stock KeyMint/Keymaster declaration for the exact build or Android 17 match. |

## Important VINTF observation

The shared Motorola SM6150 LineageOS 20 manifest begins with target-level 4. That is a **community shared-device manifest**, not the exact stock manifest from the user's XT2135-2 firmware.

The Android 17 release branch reviewed for this project installs framework compatibility matrices at FCM levels 7, 8, 202404, 202504 and 202604; it does not install FCM 4. Under Android's VINTF matching rules, a device manifest's target FCM level must match the selected framework compatibility matrix level. Therefore, if the exact stock Moto G60 manifest were also target-level 4, that would be a major Android 17 compatibility obstacle. We have **not** established that stock-manifest fact.

This is separate from the earlier kernel concern: the documented Motorola kernel source tag is Linux 4.14.190, while Android 17's current FCM profiles reviewed earlier start at much newer kernel generations. Neither public community configuration nor this hardware map proves a viable kernel or vendor path.

## What this research establishes

1. Hanoip has a substantial Qualcomm/Motorola hardware integration layer; it is not simply a generic Android build with a different interface.
2. The device's graphics, camera, audio and radio stacks all depend on combinations of kernel support, firmware, vendor HALs and userspace libraries.
3. The public shared manifest's FCM target-level 4 is a meaningful warning sign for an Android 17 investigation, but cannot be substituted for the exact stock manifest.
4. The Hanoip proprietary-file list is not an exact-build match for the user's reported firmware. The common SM6150 proprietary list is from an even older Hanoip build.
5. None of these sources demonstrates that One UI 9 can run on the Moto G60, and none demonstrates that it is impossible.

## Remaining research questions

- Can a trustworthy public source establish the exact stock Android 12 VINTF manifest and target FCM level for XT2135-2 RETEU build S2RIS32.32-20-9-9-2-3?
- Which Android 17 framework HAL requirements are directly incompatible with the community SM6150 manifest, versus merely absent from that older community configuration?
- Are there public source-level updates for the Qualcomm SM6150 graphics, camera, audio and radio interface layers that change the assessment? No binary extraction or device modification is part of this research.
- What additional Samsung-specific service dependencies would be required beyond ordinary Android VINTF compatibility?

## Safety and licensing boundary

This milestone uses public source metadata and text configuration only. It does not require flashing, bootloader changes, firmware extraction, partition access, or redistribution of proprietary binaries. Proprietary-file lists are treated as inventories/references, not as permission to copy or redistribute the listed files.
