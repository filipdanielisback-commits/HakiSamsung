# Motorola Moto G60 — initial compatibility record

**Exact reference device:** Motorola Moto G60 XT2135-2 (Europe / RETEU)  
**RAM configuration:** 6 GB (user-confirmed)  
**Codename commonly associated with this device:** hanoip  
**Installed build reported by user:** S2RIS32.32-20-9-9-2-3  
**Installed Android:** Android 12 (user-confirmed)  
**Research status:** Exact kernel-source tag identified; device-tree mapping in progress  
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
| Android release | Android 12 | User-confirmed; exact build appears in Motorola's matching source request and tag |
| Installed build | S2RIS32.32-20-9-9-2-3 | User-confirmed |
| Motorola kernel source | Exact annotated tag `MMI-S2RIS32.32-20-9-9-2-3` exists and points to commit `f6673a1302e3b9ca84c54e956ad4075d676fdf59` | Verified in Motorola's Git tag metadata |
| Community device tree | Public hanoip `lineage-20` branch exists | Verified; Android 12 stock equivalence not established |
| Community kernel path | Device tree expects `kernel/motorola/sm6150`; public SM6150 community kernel repositories have a `lineage-20` branch | Repository/branch mapping identified |
| Current One UI 9 compatibility | No evidence | Unknown |

Specifications can differ by regional and retail variant. The exact XT2135-2 Europe identity is now the reference target; family-level device trees still do not prove that every G60 variant has identical hardware or software.

## Public development references

- Motorola kernel-source request for the exact build and Europe package: https://github.com/MotorolaMobilityLLC/kernel-msm/issues/574
- Exact Motorola kernel Git tag: https://github.com/MotorolaMobilityLLC/kernel-msm/tree/MMI-S2RIS32.32-20-9-9-2-3
- Motorola kernel tag target commit: https://github.com/MotorolaMobilityLLC/kernel-msm/commit/f6673a1302e3b9ca84c54e956ad4075d676fdf59
- Hanoip Android device tree, `lineage-20`: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/tree/lineage-20
- Hanoip community kernel tree, `lineage-20`: https://github.com/motorola-sm6150-devs/android_kernel_motorola_sm6150/tree/lineage-20
- LineageOS SM6150 kernel tree, `lineage-20`: https://github.com/LineageOS/android_kernel_motorola_sm6150/tree/lineage-20
- TeamWin Hanoip device configuration: https://github.com/TeamWin/android_device_motorola_hanoip
- Motorola support — Moto G60 hardware overview: https://en-us.support.motorola.com/app/answers/detail/a_id/159299/~/about-your-hardware---moto-g60
- Moto-common target status table: https://github.com/moto-common/android_device_motorola_targets
- Independent Moto G60 build/version listing (corroboration only): https://www.phonecopy.com/en/phones/view/motorola_moto_g60

## Kernel and device-tree mapping

Motorola's issue #574 identifies the exact firmware package as `XT2135-2_HANOIP_RETEU_12_S2RIS32.32-20-9-9-2-3_subsidy-DEFAULT_regulatory-XT2135-2-EUROPE_CFC.xml.zip`. The issue is closed and carries the `published` label. A Motorola contributor links to the exact Git tag `MMI-S2RIS32.32-20-9-9-2-3`, whose annotated tag message is `Hanoip push for Android 12`. The tag points to commit `f6673a1302e3b9ca84c54e956ad4075d676fdf59`.

The public community device tree's `lineage-20` branch sets `TARGET_KERNEL_SOURCE := kernel/motorola/sm6150`. It expects proprietary vendor and firmware directories, including `vendor/motorola/hanoip`, `hanoip-firmware` and `hanoip-vendor`. Its configuration sets shipping API level 30 and target VNDK version 30.

The community tree's `lineage-20` branch and corresponding community kernel branch form a plausible build-source pairing, but their equivalence to Motorola's exact Android 12 source tag has not been demonstrated. Do not mix these branches or infer compatibility from matching codenames alone.

See [the detailed source audit](../../research/MOTO-G60-SOURCE-AUDIT.md) for tag IDs, branch commits and caveats.

## Initial feasibility assessment

The exact Motorola Android 12 kernel tag is now identified, and a public community device-tree/kernel pairing has been mapped. This is meaningful groundwork, but it does not demonstrate compatibility with Samsung One UI 9. Vendor interfaces, HALs, proprietary dependencies and Samsung-specific services remain unverified.

**Assessment: Research candidate; feasibility undetermined.**


## Android 17 / One UI 9 VINTF checkpoint

Samsung's official One UI 9 beta announcement explicitly identifies Android 17 as its base, and Samsung later announced the stable rollout. Android 17 is now the verified research target, not an assumption.

The exact Motorola kernel source tag matching the reported build has Linux version **4.14.190** in its top-level Makefile. The reviewed AOSP Android 17 development FCM configuration lists FCM levels 6, 7, 8, 202404, 202504 and 202604, omitting FCM 5. Its FCM 6 Android 17 matrix build entry lists kernel profiles 4.19, 5.4 and 5.10, not 4.14. This is a significant preliminary kernel/VINTF compatibility warning, not a completed device-side VINTF match test.

The public community device tree declares shipping API level 30 and VNDK 30, but it does not establish the stock Android 12 manifest's target FCM level. No exact stock device VINTF manifest or device compatibility matrix was located in the reviewed public device-tree branch. Therefore stock HAL declarations, manifest target level and kernel target level remain unknown.

See [the Android 17 VINTF comparison](../../research/MOTO-G60-ANDROID17-VINTF-COMPATIBILITY.md) for the full evidence, source links, and limitations.

## Next evidence to collect

- Android security patch level and public build properties for the exact XT2135-2 RETEU build.
- Source-tree and tag contents comparison between Motorola's exact Android 12 tag and community kernel branches.
- Device-tree branch provenance and proprietary vendor-interface assumptions.
- Documented HAL and vendor-interface requirements.
- Samsung One UI 9 / Android 17 framework and service dependencies.

No firmware extraction, boot modification or flashing is part of this research milestone.
