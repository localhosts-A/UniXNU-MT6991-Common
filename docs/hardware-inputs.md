# MT6991 hardware inputs

## Source

The observations below come from a read-only Dali live FDT capture taken on
2026-10-26. Its SHA-256 is
`b1e31720e948e8769a2fcb7fd9a097c30b934200b8033692f9ed1214b1985182`. The raw
FDT contains device-specific identifiers and stays in private local evidence;
it is not included in this repository.

## CPU inventory

The live FDT and public MT6991 reference DTS agree on eight affinity IDs and a
4/3/1 cluster layout. The reference DTS associates cluster 0 with an
`arm,cortex-a720-pmu`, cluster 1 with an `arm,cortex-x4-pmu`, and cluster 2
with a generic PMUv3 node named `pmu_blackhawk`. These cluster facts come from
the [reference DTS pinned at `e45c3199537fbba9aae5fb06c31fc8dbd3c1dcee`](https://github.com/MiCode/MTK_kernel_device_modules/blob/e45c3199537fbba9aae5fb06c31fc8dbd3c1dcee/device_modules/arch/arm64/boot/dts/mediatek/xiaomi_mt6991_common.dts).
MediaTek lists the [Dimensity 9400 CPU mix](https://www.mediatek.com/products/smartphones/mediatek-dimensity-9400)
as four Cortex-A720, three Cortex-X4, and one Cortex-X925; Arm says
[Blackhawk is the former codename for Cortex-X925](https://newsroom.arm.com/blog/armv9-cpus-consumer-devices).

`dts/mt6991.dtsi` records the resulting per-core compatible mapping as a
**derived input**, not as a value captured from the device. Neither the live
FDT nor the public reference DTS supplies CPU compatible strings or MIDRs.
Verify those values from runtime CPU identification before treating this as an
authoritative board identity. The [Linux CPU binding](https://github.com/torvalds/linux/blob/master/Documentation/devicetree/bindings/arm/cpus.yaml)
recognizes these compatible strings and reserves the generic `arm,armv8` value
for software models.

## Shared resources observed

- Root compatible: `mediatek,MT6991`.
- GICv3 distributor: `0x0c400000` size `0x40000`; redistributor region:
  `0x0c440000` size `0x200000`.
- Architected timer: `arm,armv8-timer`, four interrupt tuples as recorded in
  `dts/mt6991.dtsi`, with a 13 MHz clock.
- PSCI conduit: SMC.

These facts are transcribed into `dts/mt6991.dtsi`. They have not been tested
through UniXNU's DTB-to-ADT conversion or on a device. The DTSI remains
incomplete for that conversion because `/cpus/timebase-frequency` is absent;
the converter currently requires it for each emitted CPU node.

## Inputs still required

- The CPU-compatible mapping is an evidence-backed inference. Runtime MIDRs
  remain uncollected.
- The root memory node reports base `0x80000000` and size `0x300000000` (12
  GiB). The FDT contains 58 reserved-memory children: 54 fixed ranges and four
  dynamically allocated ranges. The four dynamic ranges have sizes but no
  assigned addresses, so this DRAM range cannot yet be handed to XNU.
- UniXNU's current FDT-to-ADT converter reads the memory node but does not
  serialize `/reserved-memory` entries or dynamic allocation constraints.
  This must be designed before the board can advertise usable memory.
- Board-level CPU selection, a supported timer/GIC ADT contract and the
  complete fixed and dynamic reservation ownership map still need review.

Until those inputs are available, this SoC repository and its DTSI are
inventory-only and are not a bootable platform description.
