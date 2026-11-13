# Moto G60 — One UI 9 research requirements

**Target:** Motorola Moto G60, model XT2135-2, Europe / RETEU, 6 GB RAM  
**Reported software:** Android 12, build `S2RIS32.32-20-9-9-2-3`  
**Device family:** hanoip / Qualcomm SM6150 platform  
**Research baseline:** Samsung One UI 9 is based on Android 17, according to Samsung's public beta announcement.  
**Status:** Requirements and evidence checklist; not a porting guide and not proof of feasibility.

## 1. Purpose and acceptance standard

This document defines what would have to be demonstrated before a genuine One UI 9 adaptation for the Moto G60 could be described as technically viable. A successful boot alone would not satisfy the project goal. The assessment must separately consider Android framework compatibility, device HALs, kernel support, Samsung-specific framework dependencies, essential hardware functions, stability, and licensing.

The project is currently **research-only**. No working One UI 9 port is claimed, and this document does not authorize or provide instructions for flashing, unlocking, extracting firmware, accessing partitions, or modifying a device.

## 2. Status vocabulary

| Status | Meaning |
|---|---|
| Verified | Supported by a specific public source or reproducible non-invasive analysis. |
| Partially verified | Some evidence exists, but important details or exact-build confirmation are missing. |
| Unknown | The available public evidence is insufficient to make a reliable determination. |
| Concern | A known mismatch or technical risk needs investigation; not automatically a final blocker. |
| Blocked | A requirement is known not to be met, or a necessary lawful/technical prerequisite is unavailable. |
| Not tested | No implementation or runtime test has been performed. |

A source-tree declaration is not proof that a service is present in a stock image or works at runtime. Keep source evidence, merged-manifest evidence, and runtime test evidence separate.

## 3. Target identity and provenance

| Requirement | Current evidence / acceptance condition | Status |
|---|---|---|
| Exact device identity | XT2135-2, Europe / RETEU, hanoip family | Verified from project-provided device details; independently corroborate with public manufacturer material where possible |
| Exact baseline build | `S2RIS32.32-20-9-9-2-3`, Android 12 | Partially verified; reported build is known, but exact stock image contents are not publicly audited here |
| Hardware platform | Public Hanoip device tree identifies SM6150 | Verified for the community device tree; stock configuration still needs separate confirmation |
| Memory capacity | 6 GB RAM | Reported target specification; detailed memory pressure behavior unknown |
| Source provenance | Record exact repository, branch/tag, commit and date for every source-based conclusion | Required for all future findings |

## 4. Android platform and framework

| ID | Requirement | Evidence needed for acceptance | Current status |
|---|---|---|---|
| AND-01 | One UI 9 Android base | Samsung primary source identifying One UI 9's Android base | Verified: Samsung's May 12, 2026 beta announcement identifies Android 17 |
| AND-02 | Framework compatibility | Android 17 framework requirements and device-side interfaces must be mutually compatible | Concern: significant Android-version gap from the phone's Android 12 baseline |
| AND-03 | Compatibility definition | Relevant Android 17 CDD requirements must be understood; any claimed compatibility needs appropriate test evidence | Unknown / not tested |
| AND-04 | Compatibility test coverage | A lawful test plan must identify applicable CTS/VTS and relevant vendor-interface checks; no pass may be claimed without results | Not tested |
| AND-05 | Framework APIs and behavior | Required framework APIs, permissions, services and system behaviors must be available or independently implemented | Unknown |

## 5. Kernel and low-level platform support

| ID | Requirement | Evidence needed for acceptance | Current status |
|---|---|---|---|
| KRN-01 | Kernel baseline suitability | Android 17 kernel requirements and supported kernel profiles must be compared with the exact device kernel source | Concern: exact Motorola source tag is Linux 4.14.190; published Android 17 FCM profiles examined so far start at newer kernel versions |
| KRN-02 | Device driver coverage | Public evidence for display, GPU, camera, audio, sensors, storage, USB and connectivity drivers | Partially verified through source-tree inventories; runtime behavior unknown |
| KRN-03 | Kernel configuration | Relevant configuration requirements must be established from authoritative Android and device sources | Partially verified; community configuration exists but exact stock/build configuration is not fully established |
| KRN-04 | Kernel/vendor interface | Driver interfaces and vendor modules must be compatible with the target Android userspace and HAL expectations | Unknown; major compatibility concern |
| KRN-05 | Security and maintenance | Required security properties and a maintainable, appropriately licensed source basis must be demonstrated | Unknown |

## 6. VINTF, HAL and vendor compatibility

A framework compatibility matrix must be matched against the **final merged device manifest** and the actual service implementations. A single community manifest is not enough to establish what is present in the stock device.

| ID | Requirement | Evidence needed for acceptance | Current status |
|---|---|---|---|
| HAL-01 | Correct target FCM level | Exact stock and candidate device manifest target levels, matched to a supported Android 17 framework matrix | Concern: shared SM6150 community manifest declares target-level 4; Android 17 matrices examined do not include FCM 4 |
| HAL-02 | Graphics mapper / allocator | Required mapper and allocator interface declarations, service implementation and compatible driver stack | Unknown: not visible in the shared manifest inspected so far |
| HAL-03 | Graphics composer / display | Compatible composer, display stack, gralloc/mapper and GPU userspace | Unknown |
| HAL-04 | Health | Required Android 17 health HAL declaration and service implementation | Unknown: shared manifest instead shows a Motorola health HIDL declaration; merged stock manifest not established |
| HAL-05 | Power | Required Android 17 power HAL declaration and service implementation | Unknown: not visible in the shared manifest inspected so far |
| HAL-06 | Thermal | Required thermal HAL declaration and service implementation | Unknown: not visible in the shared manifest inspected so far |
| HAL-07 | Audio | Audio and audio-effects interface versions, DSP integration and service behavior satisfy the selected framework matrix | Partially verified: shared manifest declares HIDL audio 6.0 and audio effects 6.0; runtime compatibility unknown |
| HAL-08 | Camera | Camera provider, camera metadata, sensor integration and vendor extensions remain compatible | Partially verified: shared manifest declares camera provider HIDL 2.4; full camera stack unknown |
| HAL-09 | Radio and telephony | Radio, SIM, data, IMS/VoLTE and emergency-call dependencies are identified and compatible | Partially verified: shared manifest lists radio HIDL interfaces; full vendor implementation and carrier behavior unknown |
| HAL-10 | Bluetooth, Wi-Fi and GNSS | Required service declarations, firmware dependencies and framework integration are established | Partially verified from public device configuration; runtime behavior unknown |
| HAL-11 | Other device services | Biometrics/gatekeeper, sensors, USB, NFC if applicable, and other required services are inventoried | Partially verified; full merged manifest and runtime evidence required |
| HAL-12 | VINTF validation | Final manifest/matrix compatibility is checked with documented results from suitable tools and test environments | Not tested |

## 7. Samsung One UI-specific requirements

AOSP compatibility is necessary but not sufficient. Samsung's framework and applications may depend on proprietary services, vendor interfaces, privileged permissions, signatures, hardware features, or Samsung-specific system components.

| ID | Requirement | Evidence needed for acceptance | Current status |
|---|---|---|---|
| SAM-01 | One UI framework dependency map | Publicly documented dependencies and clearly labeled unknown proprietary dependencies | Unknown; no complete public dependency map established |
| SAM-02 | Samsung system services | Each essential service has a lawful source and a technically compatible implementation | Unknown |
| SAM-03 | Privileged permissions and signatures | Required permission/signature relationships are understood; no bypass or unauthorized reuse assumed | Unknown |
| SAM-04 | Samsung applications | Essential apps and their service dependencies are identified; licensing and distribution rights are clear | Unknown |
| SAM-05 | Samsung hardware integrations | Features requiring Samsung-specific hardware or services are identified and classified as unavailable, replaceable, or unverified | Unknown |
| SAM-06 | Genuine One UI identity | Acceptance must be based on actual One UI framework/software components, not a launcher, icon pack, theme or visual imitation | Required project definition |

No requirement in this section should be treated as satisfied merely because an Android 17 AOSP build can start.

## 8. Essential device functionality

Any future feasibility claim must define the functions being targeted and report each independently. The following list is a test-planning checklist, not a claim that these functions work.

- Display, brightness, refresh behavior and touch input.
- GPU acceleration and composition.
- Rear and front cameras, video capture and camera switching.
- Microphone, speakers, earpiece, audio routing and Bluetooth audio.
- Wi-Fi, Bluetooth, mobile network, SIM detection, mobile data and IMS features where supported.
- GNSS/location services.
- Sensors, rotation, proximity and other installed sensor functions.
- Charging, battery reporting, health and thermal monitoring.
- Storage, USB connectivity and device encryption.
- Biometrics or other security hardware features, where present.

**Current status:** Not tested against One UI 9. Community device-tree support is useful evidence for the device family, not proof of compatibility with Samsung's framework.

## 9. Stability, performance and security

| Requirement | Acceptance evidence | Current status |
|---|---|---|
| Boot and sustained operation | Reproducible test records from a lawful test environment | Not tested |
| Memory behavior | Evidence that core services and essential workloads operate reliably within device memory limits | Unknown |
| Thermal behavior | Reliable temperature reporting and safe thermal management under representative workloads | Unknown |
| Battery behavior | Reliable charging, battery reporting and standby behavior | Unknown |
| Security model | Verified encryption, verified-boot implications, SELinux policy and permission boundaries | Unknown |
| Updates and recovery | A documented, maintainable update/support model; no unsupported safety claims | Unknown |
| Reliability | Repeated test results, logs and known-issue records | Not tested |

## 10. Legal, licensing and source constraints

- Use public documentation, source metadata, and original independently written code for this research.
- Do not publish Samsung firmware, proprietary applications, Motorola vendor blobs, firmware images, boot images, or other copyrighted binaries without clear authorization.
- Check the license of each source repository and component before reuse.
- Record provenance and license information for any component considered for lawful use.
- Do not assume that publicly viewable files are freely redistributable.
- Keep Samsung trademarks and product names descriptive; do not imply endorsement or official support.

**Current status:** Research policy established; component-by-component licensing review remains outstanding.

## 11. Current feasibility summary

| Area | Assessment |
|---|---|
| One UI 9 Android base | Android 17 confirmed by Samsung's public announcement |
| Moto G60 identity and platform | Device details recorded; public community source identifies SM6150 / hanoip |
| Kernel baseline | Major concern: exact Motorola source tag is Linux 4.14.190 |
| Android 17 VINTF / FCM | Major concern: inspected community manifest target-level 4 is not among the Android 17 framework matrix levels examined |
| Required HAL coverage | Incomplete evidence; graphics mapper, Android health, power and thermal need merged-manifest/service investigation |
| Samsung-specific dependencies | Unknown and likely substantial research area |
| End-to-end functionality | Not tested |
| Genuine One UI 9 port | No working port established or claimed |

**Overall result: feasibility remains undetermined, with significant technical concerns.** The kernel and VINTF findings are serious enough that the project should not claim the device is compatible. They do not, by themselves, prove impossibility.

## 12. Research exit criteria

The project should not move from feasibility research to any implementation claim until all of the following have evidence:

1. The exact device/build baseline and relevant source provenance are documented.
2. The exact stock and candidate merged VINTF manifests are understood.
3. Android 17 framework matrices and device-side HAL services have a defensible compatibility assessment.
4. Kernel, drivers and vendor modules have a credible compatibility path supported by public evidence.
5. Essential Samsung-specific dependencies are identified, with lawful availability or independently implementable alternatives established.
6. Licensing and redistribution constraints are reviewed.
7. A future test plan defines measurable acceptance criteria and clearly separates untested, passed and failed functions.

Until then, the correct project status remains **research only**.
