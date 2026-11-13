# Motorola Moto G60 — initial compatibility record

**Exact reference device:** Motorola Moto G60 XT2135-2 (Europe / RETEU)  
**RAM configuration:** 6 GB (user-confirmed)  
**Codename commonly associated with this device:** hanoip  
**Installed build reported by user:** S2RIS32.32-20-9-9-2-3  
**Research status:** Initial source review completed; deeper platform analysis pending  
**Port status:** No One UI 9 port exists or is claimed by this repository.

## Hardware and software baseline

| Item | Current finding | Evidence status |
|---|---|---|
| Exact model | XT2135-2 | User-confirmed |
| Region / software channel | Europe / RETEU | User-confirmed; independently corroborated by Motorola kernel-source request's matching firmware package filename |
| RAM | 6 GB | User-confirmed |
| Device family | Moto G40 Fusion / Moto G60 share the hanoip device family in public device trees | Verified in cited device-tree documentation |
| SoC | Qualcomm Snapdragon 732G (SM7150 family) | Verified in multiple public device-tree specifications |
| CPU | Octa-core: 2x Kryo 470 Gold up to 2.3 GHz + 6x Kryo 470 Silver up to 1.8 GHz | Reported by device-tree documentation |
| GPU | Adreno 618 | Reported by device-tree documentation |
| Display | Approximately 6.8-inch, 1080 x 2460, 120 Hz IPS LCD | Reported by public device-tree documentation |
| Android release for reported build | Android 12 | Strongly corroborated: Motorola kernel-source issue names the exact XT2135-2 Europe firmware package with the Android 12 marker; independent device/version listing also classifies this build as Android 12 |
| Installed build | S2RIS32.32-20-9-9-2-3 | User-confirmed; exact build string appears in Motorola kernel-source request |
| Kernel source status | Motorola kernel-source request #574 for this exact firmware release is marked published OSS code | Verified at request metadata level; source branch contents and completeness still need review |
| Device tree | Public hanoip Android device trees exist | Verified |
| Current One UI 9 compatibility | No evidence | Unknown |

Specifications can differ by regional and retail variant. The exact XT2135-2 Europe identity is now the reference target; family-level device trees still do not prove that every G60 variant has identical hardware or software.

## Public development references

- Motorola kernel-source request for the exact build and Europe package: https://github.com/MotorolaMobilityLLC/kernel-msm/issues/574
- Motorola support — Moto G60 hardware overview: https://en-us.support.motorola.com/app/answers/detail/a_id/159299/~/about-your-hardware---moto-g60
- Hanoip Android device tree: https://github.com/motorola-sm6150-devs/device_motorola_hanoip
- Hanoip TWRP device configuration: https://github.com/TeamWin/android_device_motorola_hanoip
- Motorola kernel source organization: https://github.com/MotorolaMobilityLLC/kernel-msm
- Moto-common target status table: https://github.com/moto-common/android_device_motorola_targets
- Independent Moto G60 build/version listing (corroboration only): https://www.phonecopy.com/en/phones/view/motorola_moto_g60

The Motorola kernel-source issue identifies the matching package as `XT2135-2_HANOIP_RETEU_12_S2RIS32.32-20-9-9-2-3_subsidy-DEFAULT_regulatory-XT2135-2-EUROPE_CFC.xml.zip`. The issue is marked published OSS code. This is evidence that Motorola published a kernel-source response associated with this release; it is not yet proof that every required source component is present or that a compatible kernel can be built.

The Moto-common target table currently lists hanoip as **Dropped**. This describes that project's target status, not whether the phone itself works or whether other community sources exist.

## Research questions still open

1. What exact Android security patch level and vendor build are installed on the user's device?
2. Which published Motorola kernel branch and source commit match this exact XT2135-2 RETEU build?
3. Which device-tree branch and common device dependencies match the same Android 12 software base?
4. What vendor-interface and HAL versions are present in the documented Android 12 baseline?
5. Which Android 17 framework requirements differ from the device's available platform?
6. Which One UI components require Samsung-specific services, permissions or hardware?

## Initial feasibility assessment

The exact regional build and a matching Motorola kernel-source publication reference make the baseline substantially more specific. However, public Android device trees and a kernel-source publication do not demonstrate compatibility with Samsung One UI 9. The Snapdragon platform alone is not enough to establish feasibility.

**Assessment: Research candidate; feasibility undetermined.**

## Next evidence to collect

- Publicly documented Android security patch level and build properties for the exact XT2135-2 RETEU build.
- Matching kernel source release, commit provenance and license.
- Device-tree branch provenance and supported Android base.
- Documented HAL and vendor-interface requirements.

No firmware extraction, boot modification or flashing is part of this research milestone.
