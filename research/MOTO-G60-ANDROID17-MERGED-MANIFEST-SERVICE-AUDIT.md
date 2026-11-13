# Moto G60 — Android 17 merged-manifest and HAL service source audit

**Target:** Moto G60 XT2135-2 / hanoip, RETEU  
**Source baseline:** public Hanoip device tree `lineage-20` and LineageOS SM6150 common device tree `lineage-20`  
**Purpose:** clarify what public source says about FCM 7 graphics mapper, health, power and thermal interfaces  
**Scope:** static inspection of public source metadata and build declarations only

## Executive summary

This audit corrects an overly broad interpretation in the earlier FCM 7 comparison. The shared SM6150 source tree contains more relevant HAL packages than its shared `manifest.xml` alone reveals:

- Graphics: common product configuration lists Qualcomm graphics mapper implementation libraries, including HIDL mapper 3.0 and 4.0 implementations.
- Health: common product configuration lists the HIDL `android.hardware.health@2.1-service` and related implementation packages.
- Power: common product configuration lists an SM6150 AIDL power service. Its Soong module explicitly attaches a VINTF fragment declaring `android.hardware.power` AIDL version 2, `IPower/default`.
- Thermal: common product configuration lists `android.hardware.thermal@2.0-service.qti`.

However, **this does not establish that these packages are selected in the exact Hanoip product build**, nor does it establish that the final merged manifest declares all required interfaces. The public Hanoip product file does not directly inherit the shared `common.mk`; it inherits `device.mk`, which in turn inherits a generated `vendor/motorola/hanoip/hanoip-vendor.mk` not present in the public source repository. The public Hanoip BoardConfig also includes generated vendor BoardConfig files that are not in that repository. Those missing generated files prevent a definitive reconstruction of the final product inheritance and merged manifest.

The earlier FCM 7 concern therefore needs to be refined:
- **Power AIDL 2:** a concrete candidate implementation and VINTF fragment exist in the shared SM6150 source.
- **Graphics mapper:** candidate HIDL 3.0/4.0 implementations are listed in shared product configuration, but exact declaration and inclusion in the Hanoip product remain unconfirmed.
- **Health:** the shared product configuration lists HIDL health 2.1, not the FCM 7 required Android health AIDL 1 contract. This does not satisfy that AIDL requirement by itself.
- **Thermal:** a HIDL 2.0 QTI service package is listed, but its final manifest declaration and Hanoip inclusion remain unconfirmed.

This is a source-level refinement, not proof of Android 17 compatibility.

## 1. Hanoip product inheritance evidence

Public Hanoip `lineage_hanoip.mk`:
- Inherits `device/motorola/hanoip/device.mk`.
- Does not directly inherit `device/motorola/sm6150-common/common.mk`.
- Sets product name/device to `lineage_hanoip` / `hanoip`.

Public Hanoip `device.mk`:
- Inherits `vendor/motorola/hanoip/hanoip-vendor.mk`.
- Declares device-specific packages and properties.
- Does not directly inherit the public shared SM6150 `common.mk`.

Public Hanoip `BoardConfig.mk`:
- Defines its own SM6150 board and kernel configuration.
- Includes `vendor/motorola/hanoip/BoardConfigVendor.mk`, `vendor/motorola/hanoip-firmware/BoardConfigVendor.mk`, and `vendor/motorola/hanoip-vendor/BoardConfigVendor.mk`.
- Does not directly include the shared tree's `BoardConfigCommon.mk`.

Public Hanoip `lineage.dependencies` lists `android_hardware_motorola` at `hardware/motorola`; it does not list the shared SM6150 device tree as a direct dependency.

The generated vendor makefiles and BoardConfig files are not present in the public Hanoip source inspected. They may include shared configuration or add manifest fragments, but that cannot be confirmed from the public Hanoip repository alone.

## 2. Shared SM6150 manifest versus product packages

The shared SM6150 `manifest.xml` is a device manifest with target-level 4. In the inspected file, the relevant standard HAL entries are not present:
- no `android.hardware.graphics.mapper` entry;
- no `android.hardware.health` AIDL 1 entry;
- no `android.hardware.power` AIDL entry;
- no `android.hardware.thermal` HIDL 2.0 entry.

It does declare `motorola.hardware.health` HIDL 1.0. That is a Motorola-specific interface and is not interchangeable with the Android health AIDL 1 interface required by the FCM 7 matrix.

The shared `BoardConfigCommon.mk` points `DEVICE_MANIFEST_FILE` to the shared `manifest.xml`. It also points `DEVICE_MATRIX_FILE` to the shared `compatibility_matrix.xml`. This confirms how that common board configuration would select those files, but does not prove the Hanoip product uses this common BoardConfig.

## 3. HAL-by-HAL findings

### Graphics mapper

Shared SM6150 `common.mk` lists:
- `android.hardware.graphics.mapper@3.0-impl-qti-display`
- `android.hardware.graphics.mapper@4.0-impl-qti-display`
- `gralloc.sm6150`
- Qualcomm display allocator and mapper vendor packages, including mapper versions 1.1 through 4.0.

**Assessment:** this is positive source evidence that the SM6150 common configuration has candidate graphics allocation/mapper implementation components. The earlier statement that mapper support was not evidenced anywhere in public shared source was too strong.

**Still unknown:** whether these packages are included in the exact Hanoip product build, which mapper interface is declared in its final merged manifest, and whether the relevant implementation is compatible with the Android 17 framework matrix and kernel/driver stack. A product package list is not runtime validation.

### Health

Shared SM6150 `common.mk` lists:
- `android.hardware.health@2.1-impl:64`
- `android.hardware.health@2.1-impl.recovery`
- `android.hardware.health@2.1-service`

The shared manifest inspected declares `motorola.hardware.health` HIDL 1.0, not `android.hardware.health` AIDL 1.

**Assessment:** there is evidence of a standard Android HIDL health 2.1 service in the shared product configuration. It is not the AIDL 1 interface listed as mandatory by the FCM 7 matrix. The existence of a HIDL 2.1 package does not establish the required AIDL health contract.

**Still unknown:** whether a separate AIDL health service exists in another product/vendor source, whether a suitable service is supplied by a different manifest fragment, and what is actually present in the final Hanoip product.

### Power

Shared SM6150 `common.mk` lists `android.hardware.power-service.sm6150-libperfmgr`.

Its `power-libperfmgr/Android.bp` declares a vendor binary with:
- shared library `android.hardware.power-V2-ndk`;
- VINTF fragment `aidl/android.hardware.power-service.sm6150.xml`.

That fragment explicitly declares:
- HAL name `android.hardware.power`;
- AIDL version 2;
- `IPower/default`.

**Assessment:** this is concrete evidence of an AIDL Power 2 implementation and an associated VINTF fragment in the shared SM6150 source. Power AIDL 2 is within the FCM 7 matrix's stated AIDL 2–3 alternatives.

**Still unknown:** whether the Hanoip product includes this shared service package, whether the fragment is present in its final merged vendor manifest, and whether the implementation is operational and compatible with the target framework.

### Thermal

Shared SM6150 `common.mk` lists `android.hardware.thermal@2.0-service.qti`.

**Assessment:** a candidate HIDL thermal 2.0 service package is present in the shared product configuration, matching the interface family/version named by the FCM 7 matrix.

**Still unknown:** the service's final VINTF declaration, the source and dependencies of the QTI package, whether the package is selected in the Hanoip product, and whether it works with the target framework and device sensors/thermal zones.

## 4. Evidence classification

| Interface | Shared manifest declaration | Shared product/service evidence | Hanoip final product status |
|---|---|---|---|
| Graphics mapper | Not found in shared manifest | HIDL mapper 3.0/4.0 implementation packages and Qualcomm mapper components listed in `common.mk` | Unknown |
| Health | Motorola health HIDL 1.0 only; no Android health AIDL 1 entry | Android health HIDL 2.1 service and implementation packages listed | Unknown; FCM 7 AIDL health requirement not evidenced |
| Power | No AIDL power entry in shared manifest | AIDL Power 2 service and explicit VINTF fragment found | Unknown whether included in final Hanoip product |
| Thermal | No thermal 2.0 entry in shared manifest | QTI thermal HIDL 2.0 service package listed | Unknown |

## 5. Updated FCM 7 interpretation

The earlier FCM 7 audit correctly described what was and was not declared in the shared `manifest.xml`, but its follow-up wording could be read as though no implementation evidence existed elsewhere. This new audit shows that the shared product configuration contains candidate implementation packages for all four areas, and an explicit AIDL Power 2 VINTF fragment.

The most important remaining gaps are:
1. the public Hanoip product's relationship to the shared SM6150 `common.mk` and common BoardConfig is not proven by the public device files;
2. the final merged Hanoip manifest is unavailable in the inspected public source;
3. the Android health AIDL 1 contract required by FCM 7 is not evidenced by the shared health HIDL 2.1 package;
4. package declarations do not prove service availability, interface registration, or runtime compatibility.

The FCM 4 target-level mismatch and the Linux 4.14.190 kernel-profile concern remain unchanged.

## 6. Next evidence to seek

The next useful source-only task is to inspect public generated vendor makefile references and related common/device source history where available, then map all VINTF fragments associated with the mapper, health, power and thermal packages. If the generated vendor source is not public, record the final merged manifest and exact stock HAL inventory as unknown rather than inferring them.

## Sources

- Hanoip `lineage_hanoip.mk`: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/lineage_hanoip.mk
- Hanoip `device.mk`: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/device.mk
- Hanoip `BoardConfig.mk`: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/BoardConfig.mk
- Hanoip `lineage.dependencies`: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/lineage.dependencies
- Shared SM6150 `manifest.xml`: https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/manifest.xml
- Shared SM6150 `BoardConfigCommon.mk`: https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/BoardConfigCommon.mk
- Shared SM6150 `common.mk`: https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/common.mk
- Shared SM6150 Power AIDL module: https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/power-libperfmgr/Android.bp
- Shared SM6150 Power AIDL VINTF fragment: https://github.com/LineageOS/android_device_motorola_sm6150-common/blob/lineage-20/power-libperfmgr/aidl/android.hardware.power-service.sm6150.xml
- AOSP FCM 7 matrix: https://android.googlesource.com/platform/hardware/interfaces/+/07df4f22df/compatibility_matrices/compatibility_matrix.7.xml

## Scope and licensing

This audit uses public source text and metadata only. It does not extract or redistribute proprietary firmware or vendor binaries, access device partitions, flash software, or modify a device. Public source declarations are not runtime test results.
