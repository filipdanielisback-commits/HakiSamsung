# Moto G60 vs Android 17 FCM — HAL compatibility matrix

**Device under study:** Motorola Moto G60 XT2135-2 (Europe / RETEU; hanoip family)  
**User-reported build:** Android 12, `S2RIS32.32-20-9-9-2-3`  
**Motorola exact kernel source tag:** `MMI-S2RIS32.32-20-9-9-2-3` — Linux 4.14.190  
**Framework target:** Android 17 (Samsung One UI 9 base, verified separately from Samsung's official announcement)  
**AOSP reference branch:** `platform/hardware/interfaces`, `android17-release`  
**Scope:** Public-source comparison only. No firmware extraction, device modification, flashing, or proprietary binary redistribution.

## 1. Important correction: use the Android 17 release branch

The earlier preliminary report used the Android 16 QPR2 development branch's Android 17 build configuration. That was useful as an early signal, but it is not the best final reference now that the official AOSP `android17-release` branch is available.

The Android 17 release branch's `compatibility_matrices/Android.bp` defines the matrices installed for Android 17 as:
- FCM 7
- FCM 8
- FCM 202404
- FCM 202504
- FCM 202604

It does **not** include FCM 6 in the Android 17 release matrix dependency list. AOSP VINTF matching rules require the device manifest's `target-level` to match the framework matrix level exactly. The FCM 7 matrix contains a comment describing special extended support for the HIDL `IRadio@1.6` interface for devices launching with FCM 6; that narrow radio note should not be interpreted as restoring the complete FCM 6 matrix.

**Consequence:** A device whose stock manifest actually declares `target-level="6"` appears to face a major Android 17 VINTF compatibility blocker because the Android 17 release matrix set omits FCM 6. We have not verified the Moto G60 stock manifest, so its actual target level remains unknown. Do not infer the manifest level solely from the installed Android version.

Official AOSP sources:
- Android 17 release branch matrix build rules: https://android.googlesource.com/platform/hardware/interfaces/+/refs/heads/android17-release/compatibility_matrices/Android.bp
- FCM 7 matrix: https://android.googlesource.com/platform/hardware/interfaces/+/refs/heads/android17-release/compatibility_matrices/compatibility_matrix.7.xml
- FCM 8 matrix: https://android.googlesource.com/platform/hardware/interfaces/+/refs/heads/android17-release/compatibility_matrices/compatibility_matrix.8.xml
- VINTF matching rules: https://source.android.com/docs/core/architecture/vintf/match-rules
- FCM lifecycle concepts: https://source.android.com/docs/core/architecture/vintf/fcm

## 2. Kernel profile comparison

The exact Motorola kernel tag's top-level Makefile identifies Linux **4.14.190**.

The Android 17 release branch's `Android.bp` associates the supported FCM kernel profiles as follows:

| Android 17 framework matrix | Kernel config profiles in AOSP build rules |
|---|---|
| FCM 7 | `kernel_config_t_5.10`, `kernel_config_t_5.15` |
| FCM 8 | `kernel_config_u_5.15`, `kernel_config_u_6.1` |
| FCM 202404 | `kernel_config_v_6.6` |
| FCM 202504 | `kernel_config_b_6.12` |
| FCM 202604 | `kernel_config_c_6.18` |

No 4.14 profile appears in the Android 17 release branch's listed supported FCM kernel profiles. The 4.14.190 baseline therefore does not line up with the supported Android 17 FCM 7+ kernel profiles.

This is a major technical blocker signal. It is not a complete kernel configuration test: the exact stock kernel config and manifest kernel target level have not been established. The Android kernel configuration documentation explains that kernel config fragments are version-specific and are checked by VTS and VINTF-related validation:
- https://android.googlesource.com/kernel/configs/+/refs/heads/main/README.md
- https://source.android.com/docs/core/architecture/vintf/match-rules

## 3. Public Hanoip source evidence

Reviewed community branch:
- https://github.com/motorola-sm6150-devs/device_motorola_hanoip/tree/lineage-20

Files:
- [BoardConfig.mk](https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/BoardConfig.mk)
- [device.mk](https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/device.mk)
- [proprietary-files.txt](https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/proprietary-files.txt)

Evidence found:
- `BoardConfig.mk` sets board platform `sm6150`, bootloader board name `hanoip`, and community kernel source path `kernel/motorola/sm6150`.
- It maps audio DSP/kernel module names and maps `wlan.ko` to `qca_cld3_wlan.ko`.
- It includes proprietary vendor and firmware configuration from separate vendor directories.
- `device.mk` installs `android.hardware.boot@1.1-impl-qti`, `android.hardware.boot@1.1-impl-qti.recovery`, and `android.hardware.boot@1.1-service`.
- `device.mk` includes Qualcomm telephony HIDL wrappers and IMS-related packages.
- `proprietary-files.txt` lists camera-related libraries, Qualcomm IMS/radio libraries, and vendor display color libraries (versions 1.0 through 1.3).
- The header of `proprietary-files.txt` says its default source set is `hanoip (S2RI32.32-20-9-9-2)`, which is not the exact user-reported `S2RIS32.32-20-9-9-2-3` build.

These are **community build configuration and dependency clues**, not the stock Android 12 device manifest. They do not establish the actual installed HAL service versions or prove that the exact user's vendor image is identical.

## 4. Subsystem comparison

Status meanings:
- **Public clue:** The community tree documents a related dependency or package.
- **Unknown:** The exact stock manifest or interface declaration has not been verified.
- **Potential mismatch:** A public build declaration differs from a requirement visible in the Android 17 FCM matrix, but stock equivalence is unproven.
- **Not assessed:** Public evidence is insufficient to compare the interface.

| Subsystem | Android 17 FCM 7 framework requirement | Hanoip public-source evidence | Current assessment |
|---|---|---|---|
| Audio | HIDL `android.hardware.audio@6.0` or `7.0-1`; audio effect HIDL 6.0 or 7.0 | Audio DSP kernel module aliases and audio-related vendor dependencies | Public clue; exact HAL versions unknown |
| Boot control | HIDL `android.hardware.boot@1.2` | Community `device.mk` installs QTI boot implementation/service labelled `@1.1` | Potential mismatch; not stock manifest proof |
| Camera | HIDL camera provider 2.4–7 or AIDL camera provider 1 | Camera libraries and Motorola camera packages in proprietary-file manifest | Public clue; provider HAL version/instance unknown |
| Bluetooth | HIDL Bluetooth HCI 1.0–1; AIDL Bluetooth audio v2 | No exact public Hanoip VINTF declaration located in reviewed files | Unknown |
| Graphics/display | Allocator HIDL 2.0/3.0/4.0 or AIDL 1; composer HIDL 2.1–4 or AIDL composer3; mapper HIDL 2.1/3.0/4.0 | Adreno 618 device-family evidence; vendor display color libraries listed | Public clue; graphics HAL versions unknown |
| GNSS/location | HIDL GNSS 2.0–1 or AIDL GNSS 2 | No exact manifest declaration located | Unknown |
| Key storage/security | HIDL Keymaster 3.0/4.0–1 or AIDL KeyMint 1–2; Gatekeeper HIDL 1.0 | No exact manifest declaration located | Unknown |
| Media | HIDL Codec2 1.0–2 and OMX 1.0 appear in FCM 7 | Qualcomm media libraries listed in proprietary-file manifest | Public clue; service declarations unknown |
| Power/health | AIDL Power 2–3; AIDL Health 1 | No exact manifest declaration located | Unknown |
| Radio/IMS | AIDL radio config/data/messaging/modem/network/SIM/voice v1; FCM 7 also contains HIDL radio compatibility entries including IRadio 1.6 | Qualcomm telephony wrappers, IMS packages and Qualcomm radio/IMS libraries | Strong subsystem dependency evidence; exact platform HAL interfaces unknown |
| Sensors | HIDL Sensors 1.0/2.0–1 or AIDL Sensors | No exact manifest declaration located | Unknown |
| Thermal | HIDL Thermal 2.0 | No exact manifest declaration located | Unknown |
| USB | HIDL USB 1.0–3 or AIDL USB | No exact manifest declaration located | Unknown |
| Vibrator | AIDL Vibrator 1–2 | No exact manifest declaration located | Unknown |
| Wi-Fi | HIDL Wi-Fi 1.3–6; AIDL Hostapd v1 and AIDL Supplicant | WLAN kernel module mapping to Qualcomm `qca_cld3_wlan.ko` | Public clue; exact HAL versions/instances unknown |

The Android 17 FCM 7 matrix includes the interface requirements above. A framework compatibility matrix describes what the framework expects from the vendor/device side; it is not evidence that the Moto G60 actually implements a particular version.

## 5. FCM 7 details worth highlighting

The Android 17 release branch's FCM 7 matrix is still a mixed HIDL/AIDL contract:
- Audio and audio-effect HIDL versions are specified.
- Boot control HIDL 1.2 is listed.
- Camera provider accepts HIDL 2.4–7 or AIDL 1.
- Graphics allocator/composer/mapper have HIDL and AIDL alternatives.
- Keymaster/KeyMint and Gatekeeper requirements remain represented.
- Radio has AIDL service requirements, while the matrix also documents special HIDL `IRadio@1.6` extended support for devices launching with FCM 6.
- Sensors, thermal, USB, vibrator and Wi-Fi requirements are present.

The exact instance names matter as well as interface versions. A name/version-only comparison is not enough; the complete device manifest must be reconciled against the framework matrix. AOSP documents this matching behavior here:
https://source.android.com/docs/core/architecture/vintf/match-rules

## 6. Current assessment

### Confirmed
- Android 17 is the verified Android base for One UI 9 from Samsung's official announcement (see the main VINTF comparison report).
- Motorola's exact kernel source tag for the user's reported build identifies Linux 4.14.190.
- The Android 17 release branch's supported FCM dependency list contains FCM 7, 8, 202404, 202504 and 202604; it omits FCM 6.
- The Android 17 release branch's FCM 7+ kernel profiles start at 5.10/5.15 and move newer; no 4.14 profile is listed.
- The community Hanoip tree installs a boot-control package labelled HIDL 1.1 and has Qualcomm audio, WLAN and telephony dependency clues.
- The public proprietary-file manifest references an older Hanoip build string than the user's exact build.

### Unknown
- The exact stock Moto G60 device manifest and its `target-level`.
- The exact stock device compatibility matrix.
- The stock HAL versions, instances, transport formats and vendor service names.
- The stock kernel manifest target level and exact kernel config.
- Whether any legitimate, separately maintained newer kernel/vendor interface implementation exists for this device.
- One UI/Samsung-specific service requirements and device authentication/feature dependencies.

### Potential blockers
1. If the stock manifest target level is 6, the Android 17 release branch's omission of FCM 6 is a major VINTF matching problem. The narrow FCM 7 HIDL radio compatibility comment does not establish whole-device FCM 6 support.
2. The exact 4.14.190 kernel does not match the listed Android 17 FCM 7+ kernel profile families.
3. The community build tree's boot-control package is labelled 1.1, while FCM 7 asks for HIDL boot control 1.2. This is a lead to investigate, not proof of a stock mismatch.
4. Graphics, camera, radio/IMS and audio are highly device-specific and currently lack exact stock manifest evidence.
5. Samsung-specific services are an independent compatibility layer beyond ordinary AOSP VINTF matching.

## 7. Decision and next research task

**Decision:** Android 17 is the correct framework target, but the Moto G60 currently has multiple serious compatibility risks. The project remains research-only and feasibility is not established.

The next safe research task is to look for an exact-build stock VINTF manifest or trustworthy manufacturer-published metadata. If the exact stock manifest is not publicly available, record it as unavailable; do not extract proprietary firmware or request private device dumps. We should also compare the public community kernel source to the Motorola 4.14.190 tag at a source-metadata level, without attempting device modification.

No port, working build, or successful compatibility result is claimed.
