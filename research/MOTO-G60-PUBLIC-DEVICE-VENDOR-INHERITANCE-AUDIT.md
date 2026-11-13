# Moto G60 public device/vendor inheritance audit

**Scope:** public build-configuration and VINTF metadata only. This is a source-level research audit, not a device-modification guide. Proprietary vendor binaries are not copied, reproduced, or redistributed.

## Executive finding

The previous conclusion needs one refinement: generated Hanoip vendor build files are not universally unavailable. Public Hanoip vendor-source repositories exist, and public device configurations show how the device tree expects them to be included. However, the exact generated vendor configuration and final merged VINTF state for the user's XT2135-2 RETEU build `S2RIS32.32-20-9-9-2-3` are still **not verified**.

The most useful new evidence is in the public `motorola-sm6150-devs/device_motorola_hanoip` `lineage-20` branch:

- `device.mk` directly inherits `vendor/motorola/hanoip/hanoip-vendor.mk`.
- `BoardConfig.mk` includes three generated vendor BoardConfig files: `vendor/motorola/hanoip/BoardConfigVendor.mk`, `vendor/motorola/hanoip-firmware/BoardConfigVendor.mk`, and `vendor/motorola/hanoip-vendor/BoardConfigVendor.mk`.
- That checked `device.mk` does **not** directly inherit the shared SM6150 `common.mk`. Therefore the shared SM6150 product-package list cannot automatically be treated as part of this Hanoip product.
- The public Hanoip device tree's own `BoardConfig.mk` does not declare a `DEVICE_MANIFEST_FILE` in the inspected source; the generated vendor BoardConfig includes may contribute build settings, but their contents and the final assembled manifest have not been established here.

This is stronger evidence about the intended build graph, but it is not proof of the resulting image contents or runtime HAL services.

## Independent Hanoip configuration corroboration

An independently maintained public Hanoip device-tree fork, `bhawesh2002/android_device_motorola_hanoip`, provides additional, older configuration evidence:

- Its `BoardConfig.mk` sets `DEVICE_MANIFEST_FILE` to the device-tree `manifest.xml`, and also names a device compatibility matrix, framework compatibility matrix, and framework manifest.
- That `manifest.xml` declares target FCM level 4 and contains an IR HIDL declaration.
- A separate `manifest_hanoip.xml` file contains NFC and fingerprint-related HIDL declarations. The inspected `BoardConfig.mk` does not itself show how that separate file is incorporated, so it must not be counted as part of the final manifest without further evidence.
- Its `device.mk` includes a generated Hanoip vendor product makefile if present and lists packages for several hardware services, including graphics mapper, Health HIDL 2.1, QTI Power, and QTI Thermal HIDL 2.0.

This fork is useful as corroboration of the older Hanoip software stack, not as an exact match for the user's RETEU build or the `motorola-sm6150-devs` `lineage-20` product. Its manifest/package lists do not establish Android 17 compliance. In particular, a Health HIDL 2.1 package is not the same interface as the Android 17 FCM 7 Health AIDL 1 requirement.

## Evidence classification

| Question | Finding | Status |
|---|---|---|
| Does the public Hanoip product include generated vendor product configuration? | Yes: the inspected `device.mk` inherits `hanoip-vendor.mk`. | Verified for that branch |
| Does its board configuration include generated vendor board files? | Yes: three vendor BoardConfig paths are included. | Verified for that branch |
| Does that Hanoip product directly inherit shared SM6150 `common.mk`? | No direct inheritance appears in the inspected Hanoip `device.mk`. | Verified for that branch |
| Are the generated vendor makefiles' exact contents and resulting build graph fully audited? | No. | Unknown |
| Is the final merged manifest for the user's exact stock build known? | No. | Unknown |
| Does the older independent fork provide an FCM 4 device manifest? | Yes, its checked `manifest.xml` declares target level 4. | Verified for that fork only |
| Is the separate `manifest_hanoip.xml` proven to be merged? | Not from the inspected BoardConfig. | Unknown |
| Does this prove Android 17 / One UI 9 compatibility? | No. | Not established |

## Implications for the Android 17 / One UI 9 assessment

1. Keep the Android 17 FCM 7 Health AIDL 1 gap open. The older fork's package declaration is Health HIDL 2.1, which does not satisfy that requirement by itself.
2. Do not credit the shared SM6150 common product packages to the exact Hanoip product unless a direct inheritance path or generated vendor makefile proves that they are included.
3. Do not infer final VINTF declarations from package names alone. Product packages, VINTF fragments, assembled manifests, and working runtime services are separate evidence layers.
4. The user's exact Motorola build remains the reference target. The public device trees and community forks are corroborating sources only; none has been demonstrated to match the exact RETEU stock build.
5. The earlier kernel concern remains unchanged: the exact Motorola kernel tag is Linux 4.14.190, while the Android 17 FCM 6 Android 17 kernel profile set starts at 4.19. This remains a major compatibility concern, not a complete proof of impossibility.

## Sources checked

- Hanoip `lineage-20` device.mk: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/device.mk
- Hanoip `lineage-20` BoardConfig.mk: https://github.com/motorola-sm6150-devs/device_motorola_hanoip/blob/lineage-20/BoardConfig.mk
- Independent Hanoip device-tree fork: https://github.com/bhawesh2002/android_device_motorola_hanoip
- Independent fork's manifest.xml: https://github.com/bhawesh2002/android_device_motorola_hanoip/blob/default/manifest.xml
- Independent fork's manifest_hanoip.xml: https://github.com/bhawesh2002/android_device_motorola_hanoip/blob/default/manifest_hanoip.xml
- Independent fork's BoardConfig.mk: https://github.com/bhawesh2002/android_device_motorola_hanoip/blob/default/BoardConfig.mk
- Independent fork's device.mk: https://github.com/bhawesh2002/android_device_motorola_hanoip/blob/default/device.mk
- AOSP VINTF manifest composition rules: https://source.android.com/docs/core/architecture/vintf/objects
- AOSP VINTF matching rules: https://source.android.com/docs/core/architecture/vintf/match-rules
- AOSP Android 17 FCM 7 matrix: https://android.googlesource.com/platform/hardware/interfaces/+/07df4f22df/compatibility_matrices/compatibility_matrix.7.xml

## Conclusion

The audit closes part of the source-availability gap: public Hanoip build configurations confirm that generated vendor product and board files are expected. It does **not** close the exact-build manifest gap or establish Android 17 compatibility. The next meaningful research milestone is a carefully bounded comparison of public Hanoip VINTF declarations across maintained device-tree branches, with every branch/build identity recorded and no assumptions that community configurations equal the user's stock RETEU image.
