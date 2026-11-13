# Moto G60 Android 12 VINTF baseline vs One UI 9 / Android 17

**Device:** Motorola Moto G60 XT2135-2, Europe / RETEU  
**User-reported installed build:** `S2RIS32.32-20-9-9-2-3` (Android 12)  
**Exact Motorola kernel source tag:** `MMI-S2RIS32.32-20-9-9-2-3`  
**Research target:** Samsung One UI 9, officially based on Android 17  
**Status:** Preliminary public-source comparison; exact stock VINTF manifest is not yet publicly verified  
**Scope:** Public source metadata and documentation only. No firmware extraction, device modification, flashing, or proprietary binary redistribution.

## 1. Executive findings

1. Samsung's official May 12, 2026 One UI 9 beta announcement explicitly says One UI 9 is built on Android 17. Samsung's September 16, 2026 announcement confirms the stable One UI 9 rollout. Android 17 is therefore the verified Android-base target for this research.
2. The exact Motorola kernel tag for the user's build has a top-level kernel version of **Linux 4.14.190** (from the public kernel `Makefile`).
3. The public Hanoip `lineage-20` device tree sets `BOARD_SHIPPING_API_LEVEL := 30`, `PRODUCT_SHIPPING_API_LEVEL := 30`, and VNDK target/extra versions of 30. API level 30 corresponds to Android 11. These are community build configuration declarations, not a dump of the user's stock Android 12 VINTF manifest.
4. The public Hanoip tree does not provide an identifiable stock device VINTF manifest or device compatibility matrix in the reviewed branch. Its proprietary vendor configuration is inherited from separate vendor directories. Therefore the stock `manifest.target-level`, kernel target level, HAL list, and device compatibility matrix remain unverified.
5. AOSP's Android 17 development build configuration lists FCM levels 6, 7, 8, 202404, 202504, and 202604 for Android 17 devices; FCM 5 is omitted. AOSP's FCM documentation associates FCM 5 with Android 11 and FCM 6 with Android 12.
6. The Android 17 FCM 6 matrix build configuration lists kernel profiles for 4.19, 5.4, and 5.10. It does not list a 4.14 profile. The exact Motorola kernel tag is 4.14.190. This is a significant preliminary kernel/VINTF compatibility warning, but the exact stock manifest and full kernel match data are not available, so this report does not declare a final VINTF failure.

## 2. One UI 9 Android base: verified

Samsung Newsroom, May 12, 2026, “Samsung Launches One UI 9 Beta for Galaxy S26 Series Users”:
- https://news.samsung.com/us/samsung-launches-one-ui-9-beta-galaxy-s26-series-users/
- Samsung explicitly describes the beta as “Built on the new Android 17”.

Samsung Newsroom, September 16, 2026, “Samsung Begins Official Rollout of One UI 9, Bringing the Latest Galaxy Experiences to More Devices”:
- https://news.samsung.com/us/samsung-begins-official-rollout-one-ui-9-latest-galaxy-experiences-devices/
- Primary source for the stable rollout announcement.

**Engineering target decision:** Use Android 17 as the One UI 9 Android framework target. This verifies the Android base only; it does not imply One UI 9 can run on non-Samsung hardware or that Samsung-specific services are portable.

## 3. Motorola kernel baseline: exact tag inspected

Motorola's exact tag:
- https://github.com/MotorolaMobilityLLC/kernel-msm/tree/MMI-S2RIS32.32-20-9-9-2-3
- Tag target commit: https://github.com/MotorolaMobilityLLC/kernel-msm/commit/f6673a1302e3b9ca84c54e956ad4075d676fdf59
- Raw kernel Makefile: https://raw.githubusercontent.com/MotorolaMobilityLLC/kernel-msm/MMI-S2RIS32.32-20-9-9-2-3/Makefile

The Makefile begins with:
- `VERSION = 4`
- `PATCHLEVEL = 14`
- `SUBLEVEL = 190`

Therefore the exact public source tag identifies kernel **4.14.190**. This is stronger evidence than inferring a kernel version from the device family or a community kernel repository.

## 4. What the public Hanoip community tree declares

Reviewed branch:
- https://github.com/motorola-sm6150-devs/device_motorola_hanoip/tree/lineage-20

Relevant files:
- [BoardConfig.mk](https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/BoardConfig.mk)
- [device.mk](https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/device.mk)
- [lineage.dependencies](https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/lineage.dependencies)
- [proprietary-files.txt](https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/proprietary-files.txt)

Observed declarations:
- `TARGET_BOARD_PLATFORM := sm6150`
- `TARGET_BOOTLOADER_BOARD_NAME := hanoip`
- `TARGET_KERNEL_SOURCE := kernel/motorola/sm6150`
- `BOARD_SHIPPING_API_LEVEL := 30`
- `PRODUCT_SHIPPING_API_LEVEL := 30`
- `PRODUCT_EXTRA_VNDK_VERSIONS := 30`
- `PRODUCT_TARGET_VNDK_VERSION := 30`
- The build inherits Motorola vendor configuration from separate `vendor/motorola/hanoip`, `hanoip-firmware`, and `hanoip-vendor` directories.

The `proprietary-files.txt` header says its listed files are from `hanoip (S2RI32.32-20-9-9-2)`. That is not the exact user-reported `S2RIS32.32-20-9-9-2-3` build. The community proprietary-file manifest must not be treated as a proven exact stock-build match.

A review of the public device-tree branch did not locate a device `manifest.xml`, `compatibility_matrix.device.xml`, or an explicit VINTF manifest declaration. This does not prove that the stock firmware lacks those objects; they may be part of the separate proprietary vendor/firmware contents that are not represented by the public device-tree repository.

## 5. Android 17 FCM comparison

AOSP VINTF documentation explains:
- Device manifest describes the HALs and other vendor-side interfaces the device provides.
- Framework compatibility matrix describes what the framework requires from a device.
- A successful match is required between the framework compatibility matrix and device manifest, and between the framework manifest and device compatibility matrix.
- The manifest's target FCM level is an explicit compatibility contract; it must not be guessed from the currently installed Android version alone.

Sources:
- https://source.android.com/docs/core/architecture/vintf
- https://source.android.com/docs/core/architecture/vintf/objects
- https://source.android.com/docs/core/architecture/vintf/comp-matrices
- https://source.android.com/docs/core/architecture/vintf/match-rules

AOSP FCM lifecycle documentation associates Android 11 with FCM 5 and Android 12 with FCM 6:
- https://source.android.com/docs/core/architecture/vintf/fcm

AOSP Android 17 development build configuration:
- https://android.googlesource.com/platform/hardware/interfaces/+/android16-qpr2-release/compatibility_matrices/Android.bp

That source labels its Android 17 matrix set as:
- `framework_compatibility_matrix.6.android17.xml`
- `framework_compatibility_matrix.7.xml`
- `framework_compatibility_matrix.8.xml`
- `framework_compatibility_matrix.202404.xml`
- `framework_compatibility_matrix.202504.xml`
- `framework_compatibility_matrix.202604.xml`

FCM 5 is not in that Android 17 matrix set. This is a development-branch build configuration, so it is a strong AOSP direction signal, not a substitute for checking the final released Android 17 source tree at the time of an implementation decision.

### Preliminary kernel profile comparison

The same Android 17 build configuration registers the Android 17 FCM 6 matrix with kernel configuration profiles:
- `kernel_config_s_4.19`
- `kernel_config_s_5.4`
- `kernel_config_s_5.10`

The Motorola kernel source tag is **4.14.190**, not one of those three listed profiles.

AOSP's kernel matching rules state that the framework selects kernel requirements based on the device's declared kernel FCM level and kernel version, then checks required kernel configuration values. A matrix with no matching kernel version/branch does not match:
- https://source.android.com/docs/core/architecture/vintf/match-rules

**Interpretation:** The exact 4.14.190 kernel is not a straightforward fit for the Android 17 FCM 6 kernel profiles visible in the AOSP development build configuration. The device's actual stock `manifest.target-level` and `manifest.kernel.target-level` are still unknown. The device may also have vendor-specific compatibility constraints beyond the AOSP matrix. Treat this as a high-priority compatibility blocker to investigate, not a final VINTF test result.

## 6. Baseline comparison table

| Area | Moto G60 public evidence | Android 17 target evidence | Assessment |
|---|---|---|---|
| Android base | User reports stock Android 12 | Samsung officially says One UI 9 is based on Android 17 | Android base target verified |
| Shipping API | Community tree declares API 30 / Android 11 | Android 17 FCM set omits FCM 5 | Community metadata is not stock manifest proof |
| VNDK | Community tree declares target VNDK 30 | VINTF/FCM uses explicit interface requirements, not just a VNDK label | Needs exact vendor/framework metadata |
| Device manifest | Not found in public device-tree branch reviewed | Android 17 requires a compatible device manifest for the selected FCM | Unknown; blocking evidence gap |
| Device compatibility matrix | Not found in public device-tree branch reviewed | Must match framework manifest | Unknown; blocking evidence gap |
| HAL versions and instances | Proprietary vendor config is referenced but not fully represented in public tree | Android 17 FCM lists required HALs | Cannot compare fully yet |
| Kernel | Exact source tag says Linux 4.14.190 | Android 17 FCM 6 dev matrix lists 4.19 / 5.4 / 5.10 profiles | Significant mismatch warning |
| Samsung-specific services | No evidence of Samsung service compatibility on hanoip | One UI includes Samsung-specific framework/services | Separate unverified layer |

## 7. What is confirmed vs unknown

### Confirmed
- One UI 9's Android base is Android 17, based on Samsung's official beta announcement.
- The stable One UI 9 rollout was officially announced by Samsung.
- Motorola published an exact kernel source tag matching the user's reported build string.
- That tag's kernel source identifies Linux 4.14.190.
- The public Hanoip community tree declares shipping API 30 and VNDK 30.
- The reviewed Android 17 AOSP development build configuration omits FCM 5 and lists Android 17 FCM 6+ matrices.
- Its FCM 6 kernel profiles do not list 4.14.

### Unknown
- Exact stock `/vendor/etc/vintf/manifest.xml` contents for XT2135-2 RETEU build `S2RIS32.32-20-9-9-2-3`.
- Exact stock device `target-level` and kernel `target-level`.
- Exact stock device compatibility matrix.
- Exact HAL versions/instances from the stock build and their complete Android 17 FCM match results.
- Whether a vendor implementation or kernel branch different from the currently identified stock kernel exists that could satisfy the Android 17 requirements.
- Samsung proprietary service, framework-extension, and feature dependencies on non-Samsung hardware.

## 8. Research conclusion

**One UI 9 / Android 17 is now a verified research target. The Moto G60 Android 12 VINTF baseline is not yet verified from stock artifacts.**

The exact 4.14.190 kernel source and AOSP Android 17 FCM 6 kernel-profile list reveal a substantial compatibility concern. The community tree's API 30/VNDK 30 values also indicate an Android 11-era vendor baseline, but they are not sufficient to establish the stock manifest's target FCM level.

Do not describe the Moto G60 as Android 17 VINTF-compatible, and do not claim a One UI 9 port is feasible. The next valid research step is to locate an independently published, exact-build stock VINTF manifest or authoritative Motorola metadata. If none is publicly available, record the stock manifest and target FCM as unavailable rather than attempting to extract proprietary firmware. Then compare each declared HAL and kernel requirement against the final released Android 17 FCM files and keep Samsung-specific service dependencies as a separate audit.

No device modification, firmware extraction, or flashing is required for this research stage.
