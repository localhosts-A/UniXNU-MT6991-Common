# MT6991 hardware inputs

## Source

The observations below come from a read-only Dali live FDT capture taken on
2026-10-26. Its SHA-256 is
`b1e31720e948e8769a2fcb7fd9a097c30b934200b8033692f9ed1214b1985182`. The raw
FDT contains device-specific identifiers and stays in private local evidence;
it is not included in this repository.

## Shared resources observed

- Root compatible: `mediatek,MT6991`.
- GICv3 distributor: `0x0c400000` size `0x40000`; redistributor region:
  `0x0c440000` size `0x200000`.
- Architected timer: `arm,armv8-timer`, four interrupt tuples as recorded in
  `dts/mt6991.dtsi`, with a 13 MHz clock.
- PSCI conduit: SMC.

These facts are transcribed into `dts/mt6991.dtsi`. They have not been tested
through UniXNU's DTB-to-ADT conversion or on a device.

## Inputs still required

- The capture has eight CPU affinity IDs, grouped as four, three and one, but
  no CPU `compatible` strings or MIDR values. Do not infer core models from
  PMU-node names.
- The root memory node reports base `0x80000000` and size `0x300000000`. This
  cannot be handed to XNU until the reserved-memory and secure-region ownership
  map is reconciled.
- Board-level CPU selection, a supported timer/GIC ADT contract and the
  complete memory reservation map still need review.

Until those inputs are available, this SoC repository and its DTSI are
inventory-only and are not a bootable platform description.
