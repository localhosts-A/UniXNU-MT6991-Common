# UniXNU MT6991 common

Shared MT6991 support for UniXNU Format 2. The DTSI records GICv3, architected
timer and PSCI facts observed in Dali's read-only live FDT, plus a derived CPU
compatible mapping. It is a partial input inventory, not a boot-ready SoC
description.

The reference input is MiCode's MTK kernel device modules at the commit pinned
in `soc.toml`. The live CPU nodes have affinity IDs but no `compatible` or
MIDR data; the added compatible mapping is inferred from the reference PMU
clusters and MediaTek's published core mix. Runtime MIDRs and
`cpus/timebase-frequency` still need confirmation. DRAM also needs reconciliation
with fixed and dynamic reservations and an ADT handoff schema. Those board
inputs remain unresolved, so the Dali board tree must not consume this fragment
for a bootable image. Dali-specific policies stay in the device and firmware
repositories. Real-device qualification is pending.

Read the [SoC support guide](https://unixnu.com/zh/porting/soc-support) and
[device-tree guide](https://unixnu.com/zh/porting/device-tree) before adding
hardware resources. Verify them against the target device's live FDT and keep
private firmware and device backups outside this repository.

The sanitized capture facts and remaining gaps are recorded in
[`docs/hardware-inputs.md`](docs/hardware-inputs.md).
