# Motorola Moto G60 — initial compatibility record

**Codename commonly associated with this device:** hanoip  
**Research status:** Initial source review completed; deeper platform analysis pending  
**Port status:** No One UI 9 port exists or is claimed by this repository.

## Hardware and software baseline

| Item | Current finding | Evidence status |
|---|---|---|
| Device family | Moto G40 Fusion / Moto G60 share the hanoip device family in public device trees | Verified in cited device-tree documentation |
| SoC | Qualcomm Snapdragon 732G (SM7150 family) | Verified in multiple public device-tree specifications |
| CPU | Octa-core: 2x Kryo 470 Gold up to 2.3 GHz + 6x Kryo 470 Silver up to 1.8 GHz | Reported by device-tree documentation |
| GPU | Adreno 618 | Reported by device-tree documentation |
| RAM | 4 GB / 6 GB configurations are listed | Reported; exact retail variant must be confirmed |
| Display | Approximately 6.8-inch, 1080 x 2460, 120 Hz IPS LCD | Reported by public device-tree documentation |
| Shipped Android | Android 11 | Reported by public device-tree documentation |
| Current installed build | Not supplied or verified | Unknown |
| Kernel source status | Motorola's public kernel source organization has a Moto G60 source request marked published; exact matching source branch and build still need verification | Partially verified |
| Device tree | Public hanoip Android device trees exist | Verified |
| Current One UI 9 compatibility | No evidence | Unknown |

Specifications can differ by regional and retail variant. Do not treat a family-level device tree as proof that every G60 variant has identical hardware or software.

## Public development references

- Motorola support — Moto G60 hardware overview: https://en-us.support.motorola.com/app/answers/detail/a_id/159299/~/about-your-hardware---moto-g60
- Hanoip Android device tree: https://github.com/motorola-sm6150-devs/device_motorola_hanoip
- Hanoip TWRP device configuration: https://github.com/TeamWin/android_device_motorola_hanoip
- Motorola kernel source organization: https://github.com/MotorolaMobilityLLC/kernel-msm
- Moto-common target status table: https://github.com/moto-common/android_device_motorola_targets

The Moto-common target table currently lists hanoip as **Dropped**. This describes that project's target status, not whether the phone itself works or whether other community sources exist.

## Research questions still open

1. Which exact Moto G60 model and region are in scope?
2. Which Android release, vendor build and security patch level are installed on the target variant?
3. Which published Motorola kernel branch matches that exact build?
4. Which device-tree branch and common device dependencies match the same software base?
5. What vendor-interface and HAL versions are present?
6. Which Android 17 framework requirements differ from the device's available platform?
7. Which One UI components require Samsung-specific services, permissions or hardware?

## Initial feasibility assessment

The existence of public Android device trees is useful groundwork, but it does not demonstrate compatibility with Samsung One UI 9. The Snapdragon platform alone is not enough to establish feasibility.

**Assessment: Research candidate; feasibility undetermined.**

## Next evidence to collect

- Exact retail model and region, without publishing serial numbers or IMEI.
- Publicly documented software build and Android version.
- Matching kernel source release and license.
- Device-tree branch provenance and supported Android base.
- Documented HAL and vendor-interface requirements.

No firmware extraction, boot modification or flashing is part of this research milestone.
