# UniXNU MT6991 common

Shared MT6991 support for UniXNU Format 2. This repository is at an initial
inventory stage: its DTSI and configuration do not yet describe working
hardware and must not be used to build a device image.

The reference input is MiCode's MTK kernel device modules at the commit pinned
in `soc.toml`. Dali-specific resources, policies and validation remain in the
Dali device and firmware repositories. Real-device qualification is pending.

Read the [SoC support guide](https://unixnu.com/zh/porting/soc-support) and
[device-tree guide](https://unixnu.com/zh/porting/device-tree) before adding
hardware resources. Verify them against the target device's live FDT and keep
private firmware and device backups outside this repository.
