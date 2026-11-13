# Moto G60 hanoip — vendor interface and HAL research audit

**Reference device:** Moto G60 XT2135-2, Europe / RETEU, 6 GB RAM  
**Installed stock software:** Android 12, build `S2RIS32.32-20-9-9-2-3` (owner-confirmed)  
**Scope:** Public community device-tree configuration and Android platform documentation. This is a source-level audit, not a test of the user's phone.

## 1. What the public hanoip configuration tells us

Reviewed repository:
- https://github.com/motorola-sm6150-devs/device_motorola_hanoip/tree/lineage-20
- Branch head: `514e0261a117a0c7e7fa3cc3cd44f80819b0b779`

The `lineage-20` device configuration declares:

- Board platform: `sm6150`
- Bootloader board name / OTA assert: `hanoip`
- Architecture: `arm64`
- Kernel source path expected by this tree: `kernel/motorola/sm6150`
- Product and board shipping API level: `30`
- Product target VNDK version: `30`
- Additional VNDK version: `30`
- SELinux policy directories and a Qualcomm vendor policy include
- Vendor-specific kernel module mappings for audio and WLAN
- Product packages and wrappers for Qualcomm telephony/IMS
- Inherited proprietary configuration under `vendor/motorola/hanoip`, `vendor/motorola/hanoip-firmware`, and `vendor/motorola/hanoip-vendor`

The branch's dependency manifest also references `android_hardware_motorola`.

Primary configuration files:
- [BoardConfig.mk](https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/BoardConfig.mk)
- [device.mk](https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/device.mk)
- [lineage.dependencies](https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/lineage.dependencies)
- [proprietary-files.txt](https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/proprietary-files.txt)

## 2. What this does — and does not — establish

**Established from public build configuration:**
- The community tree expects a device-specific vendor layer and firmware layer.
- It uses a Qualcomm-oriented device configuration with audio and WLAN kernel-module mappings.
- It declares VNDK 30 and shipping API level 30 for that community build target.
- It includes device-specific SELinux policy and Qualcomm telephony integration.

**Not established:**
- The exact HAL versions and service instances on the user's stock Android 12 installation.
- Whether the community vendor configuration is identical to the user's RETEU stock vendor image.
- Whether a complete device VINTF manifest and compatibility matrix for the exact installed build match a newer framework.
- Whether every camera, audio, graphics, sensor, radio, fingerprint or other hardware feature works on a later Android release.
- Whether Samsung One UI services can run on this device.

The public device tree contains a proprietary-files manifest, but the actual vendor and firmware files are separate inputs. This project records their existence and dependencies only; it does not copy proprietary binaries or firmware into HakiSamsung.

## 3. Android interface concepts relevant to this project

Android's official documentation defines the key checks:

- A HAL is the standard interface between Android framework code and hardware-specific implementation. The target framework's required HALs must be supplied by the device vendor layer.
- VINTF combines device manifests, framework manifests and compatibility matrices. Manifests describe what a side provides; compatibility matrices describe what the other side requires.
- VNDK version matching is part of the vendor/framework compatibility evaluation. A declared VNDK version is not, by itself, proof of compatibility with a newer framework.
- HIDL is deprecated for new HAL work in favor of stable AIDL, but existing HIDL interfaces remain supported and must be assessed according to the target framework's compatibility rules.

Official Android references:
- HAL overview: https://source.android.com/docs/core/architecture/hal
- VINTF design: https://source.android.com/docs/core/architecture/vintf
- VINTF manifests: https://source.android.com/docs/core/architecture/vintf/objects
- Compatibility matrices: https://source.android.com/docs/core/architecture/vintf/comp-matrices
- VINTF matching rules: https://source.android.com/docs/core/architecture/vintf/match-rules
- AIDL HALs: https://source.android.com/docs/core/architecture/aidl/aidl-hals
- HIDL: https://source.android.com/docs/core/architecture/hidl

## 4. Preliminary risk map

| Area | Evidence from public tree | Research status |
|---|---|---|
| Graphics / display | Qualcomm SM6150 board target and device overlays are declared | Actual stock HAL and newer-framework compatibility unknown |
| Camera | Device tree contains camera-related configuration and proprietary-file references | Camera HAL version and compatibility unknown |
| Audio | Numerous Qualcomm audio module aliases and device-specific configuration are declared | Exact stock audio HAL inventory and compatibility unknown |
| Sensors | Device-specific vendor layer is required | Exact HAL inventory unknown |
| Wi-Fi / Bluetooth | WLAN module mapping and vendor dependencies are present | Exact firmware/HAL versions unknown |
| Telephony / IMS | Qualcomm telephony HIDL wrapper packages are declared | Exact service versions and framework compatibility unknown |
| Biometrics | No sufficient public evidence reviewed in this pass | Unknown |
| VINTF | Exact stock manifest and matrices were not available in the reviewed public device tree | Unknown |
| VNDK | Community Lineage configuration declares version 30 | Stock image and future framework match not established |

## 5. Implications for a future One UI 9 research target

A newer Android framework cannot be considered compatible just because the kernel source and device tree exist. The project needs the target framework's compatibility requirements and a reliable inventory of the device's vendor interfaces.

Samsung-specific services and framework extensions add another independent compatibility layer. AOSP VINTF compatibility alone would not prove that One UI services can operate on non-Samsung hardware.

The Android base version of One UI 9 must be supported by a dated official Samsung source before it is treated as a fixed engineering target in this repository.

## 6. Next research tasks

1. Find publicly available VINTF manifest and compatibility-matrix metadata for the exact XT2135-2 RETEU Android 12 build, without extracting or uploading proprietary images.
2. Map the public device-tree HAL dependencies by subsystem and identify which claims are confirmed versus inferred.
3. Verify the Android base and framework compatibility target for One UI 9 from Samsung's official documentation.
4. Build a public-source comparison table between the device's declared vendor interfaces and the target Android framework requirements.
5. Keep Samsung-only service dependencies separate from standard Android HAL compatibility.

**Current conclusion:** The public hanoip tree reveals a substantial Qualcomm/Motorola vendor dependency layer and a VNDK 30 community build configuration. This is useful evidence of the integration work required, but the exact stock HAL inventory and newer-framework compatibility remain unknown. No One UI 9 port is claimed.

No firmware extraction, boot modification, flashing, or proprietary binary redistribution is part of this research milestone.
