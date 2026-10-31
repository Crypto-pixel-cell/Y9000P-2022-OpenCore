# 更新记录

## 2026-10-31 · Tahoe 配置快照

- 从当前内置硬盘读取 EFI；OpenCore 仍为 1.0.8，核心文件与上一公开版一致。
- 同步 NativeTahoe、SimpleHDA、RestrictEvents、SSDT-NBCI，BlueToolFixup 为 2.7.3。
- 同步当前 `ResizeAppleGpuBars=0` / `ResizeGpuBars=-1`、背光/蓝牙/CPU 名称相关参数和 ACPI 配置。
- 保留已有工具隐藏方式；新增的“Start once without NVIDIA driver”同样为辅助项，按空格显示。
- 机主报告 macOS 26.7.1 + 1401 2.6.5：图形加速、165 Hz、基础蓝牙、睡眠唤醒正常，iPhone 连接未实现。
- 更正关键突破项的说明：早期 MMIO 修改后，还需 CpuTscSync 才进入恢复界面。
- 排除所有旧配置、FSCK 碎片、缓存及未启用备用无线驱动。公开机器标识重新脱敏。
- 当前快照保留 AppleDebug=true 与诊断参数；没有为发布而改变运行中的配置。

## 2026-10-11 · 首次 Sequoia 发布

- Sequoia 15.8.1、1401 1.13.0；本机 165 Hz 和基础蓝牙可用，当时睡眠唤醒黑屏。
- 该历史 Release 保留，不用当前 Tahoe 结果替换历史测试结论。
