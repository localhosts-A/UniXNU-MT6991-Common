# UniXNU MT6991 common

Shared MT6991 support for UniXNU Format 2. The DTSI now records the GICv3,
architected timer and PSCI facts observed in Dali's read-only live FDT. It is a
partial input inventory, not a boot-ready SoC description.

The reference input is MiCode's MTK kernel device modules at the commit pinned
in `soc.toml`. The observed CPU nodes have affinity IDs but no `compatible` or
MIDR data; DRAM ranges also need reconciliation with the reserved-memory map.
Those board inputs remain unresolved, so the Dali board tree must not consume
this fragment for a bootable image. Dali-specific policies stay in the device
and firmware repositories. Real-device qualification is pending.

Read the [SoC support guide](https://unixnu.com/zh/porting/soc-support) and
[device-tree guide](https://unixnu.com/zh/porting/device-tree) before adding
hardware resources. Verify them against the target device's live FDT and keep
private firmware and device backups outside this repository.

The sanitized capture facts and remaining gaps are recorded in
[`docs/hardware-inputs.md`](docs/hardware-inputs.md).
