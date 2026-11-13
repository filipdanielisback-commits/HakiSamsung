# Research source register

Sources below are starting points. Each technical claim in a device report should cite the exact source that supports it.

## Samsung One UI 9

- Samsung Mobile Press, One UI 9 official rollout announcement (September 16, 2026): https://api.samsungmobilepress.com/press-releases/one-ui-9-official-rollout-latest-galaxy-experiences
  - Primary source for the official rollout announcement and feature descriptions.
- Samsung Newsroom, One UI 9 beta announcement: https://news.samsung.com/
  - Use the exact regional announcement and date when documenting Android base details. Confirm current product information against Samsung's official pages.

## Android platform

- Android Open Source Project: https://source.android.com/
- Android Compatibility Definition Document: https://source.android.com/docs/compatibility/cdd
- Android Compatibility Test Suite: https://source.android.com/docs/compatibility/cts

## Motorola Moto G60 / hanoip

- Motorola Support, About your hardware — moto g60: https://en-us.support.motorola.com/app/answers/detail/a_id/159299/~/about-your-hardware---moto-g60
- Motorola Hanoip device tree: https://github.com/motorola-sm6150-devs/device_motorola_hanoip
- Hanoip device tree `lineage-20` branch: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/tree/lineage-20
- Hanoip community kernel `lineage-20` branch: https://github.com/motorola-sm6150-devs/android_kernel_motorola_sm6150/tree/lineage-20
- LineageOS SM6150 kernel `lineage-20` branch: https://github.com/LineageOS/android_kernel_motorola_sm6150/tree/lineage-20
- Motorola exact-build kernel-source request #574: https://github.com/MotorolaMobilityLLC/kernel-msm/issues/574
- Motorola exact-build kernel Git tag: https://github.com/MotorolaMobilityLLC/kernel-msm/tree/MMI-S2RIS32.32-20-9-9-2-3
- Motorola exact-build kernel tag target commit: https://github.com/MotorolaMobilityLLC/kernel-msm/commit/f6673a1302e3b9ca84c54e956ad4075d676fdf59
- TeamWin Hanoip device configuration: https://github.com/TeamWin/android_device_motorola_hanoip
- Motorola kernel source organization: https://github.com/MotorolaMobilityLLC/kernel-msm
- Motorola Moto-common target list: https://github.com/moto-common/android_device_motorola_targets
- Motorola Hanoip community source organization (archived): https://github.com/moto-hanoip/
- Independent Moto G60 build/version listing (corroboration only): https://www.phonecopy.com/en/phones/view/motorola_moto_g60

## Xiaomi devices

Add official Xiaomi specifications, public kernel sources and device configuration repositories only after the exact model and regional variant have been confirmed.

## Source quality

Prefer manufacturer documentation, official AOSP documentation, and source repositories maintained for the exact device. Secondary articles can help discover leads, but should not be the only evidence for a compatibility claim. Record the exact branch, build, model and date checked. A repository's existence does not prove that it is maintained or compatible with a particular software build.
