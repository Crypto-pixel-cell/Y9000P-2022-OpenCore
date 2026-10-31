# Lenovo Y9000P 2022 · macOS Tahoe

联想拯救者 Y9000P 2022 的个人 OpenCore EFI：**i7-12700H + RTX 3070 Laptop 8 GB**。以当前机器的 Tahoe 配置为主，保留 Sequoia 分支的驱动加载范围。

**2026-10-31 机主确认：显卡加速、内屏 165 Hz、基础蓝牙以及睡眠唤醒均可用。与 iPhone 的连接仍未实现。** 这份 EFI 不包含系统级 NVIDIA 驱动，需单独安装配套 1401。

## 当前环境与验证范围

| 项目 | 配置 / 状态 |
|---|---|
| 机器 | Lenovo Legion / 拯救者 Y9000P 2022 |
| CPU | Intel Core i7-12700H，Alder Lake |
| GPU | NVIDIA GeForce RTX 3070 Laptop GPU，8 GB |
| 内屏 | 2560 × 1600，165 Hz |
| 无线网卡 | Intel AX211 |
| macOS | Tahoe **26.7.1**，机主报告的版本号 |
| 显卡驱动 | **1401 2.6.5**，机主确认 |
| OpenCore | 1.0.8 RELEASE / OpenCanopy；核心二进制与上一公开版一致 |
| SMBIOS | MacBookPro16,1 |
| 启动与图形 | 本机可用，显卡加速与 165 Hz 已确认 |
| 基础蓝牙 | 本机正常；此前已验证 AirPods Pro 音频 |
| 睡眠 / 唤醒 | **当前组合已确认可用**；不等于完成全部电源场景的长期测试 |
| iPhone 连接 | 未实现；不宣称 AirDrop 或 iPhone 镜像可用 |
| 音频、亮度、全部 USB 接口 | 有相应配置，本轮未逐项复测 |

系统与功能状态来自机主当日反馈。本次在 Windows 读取并校验内置 EFI，未独立读取 macOS 的系统 build、已加载显卡模块或驱动安装记录；脱敏公开包没有在另一台机器启动测试。

## 本次更新

- 同步 2026-10-31 的内置 OC：增加 Tahoe 专用驱动范围，包括 NativeTahoe、SimpleHDA 与 RestrictEvents。
- BlueToolFixup 更新为当前 EFI 中的 2.7.3；保留 IntelBluetoothFirmware 2.3.0、IntelBTPatcher 2.5.0。
- 同步 `SSDT-NBCI` 背光适配表、现有启动参数与当前 BAR 设置：`ResizeAppleGpuBars=0`、`ResizeGpuBars=-1`。
- 工具项默认隐藏，按空格可调出 NVIDIA 临时禁用 / 移除工具；不需要为了精简菜单删除恢复工具。
- 公开配置清空机器标识，排除旧配置备份、恢复碎片、缓存及未启用的备用无线驱动。

睡眠唤醒在旧组合中曾黑屏，当前机主已确认正常。这里记录的是新组合的结果，不把改善归因于某一个未经单独对照的参数。旧版仍保留在 [2026-10-11 Release](https://github.com/zsct648gn6-dev/Y9000P-2022-OpenCore/releases/tag/v2026.10.11)。

## 两个关键启动突破项

早期排查中，RapidEFI 基线下先设置：

```text
Booter -> Quirks -> DevirtualiseMmio = false
```

原来的 `type 14 / CR2=0xFF00004C` 崩溃随后变为 `Non-monotonic time`；再加入 **CpuTscSync.kext 1.1.2**，加载顺序紧跟 Lilu，才进入 Sequoia 恢复界面。

`Kernel -> Quirks -> ProvideCurrentCpuInfo=true` 同样保留在当前配置中，属于基础兼容设置；历史突破不能只概括为它与 MMIO 两个开关。这些都是本机记录，不是通用于所有 12 代笔记本的模板。

## 使用方法

1. 备份自己可启动的 EFI，并先使用独立 U 盘测试。
2. 使用 [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) 为自己的机器生成标识，填写 `EFI/OC/config.plist -> PlatformInfo -> Generic` 的 `SystemSerialNumber`、`MLB`、`SystemUUID` 和 6 字节 Data 类型的 `ROM`。公开包前三项为空、ROM 为零值，不能直接照抄使用。
3. 按 [NullMoth 官方项目](https://github.com/nullmoth/nvidia-macos-driver) 对应版本的流程安装系统级显卡驱动、固件和 Metal 组件；EFI 不能替代这些组件及安装授权。本机报告使用 1401 2.6.5。
4. 先验证普通启动与基础显示，再测试高刷新率、无线和睡眠。确认可用后迁移到自己的内置 EFI 分区，保留其他系统的引导目录。

```text
EFI/
  BOOT/BOOTx64.efi     OpenCore 1.0.8 官方启动器
  OC/
    config.plist      已脱敏，须填写自己的机器标识
    OpenCore.efi
    ACPI/  Drivers/  Kexts/  Resources/  Tools/
```

公开包只更新 EFI 的发布副本，没有改写本机引导分区。BOOT 使用与现有 OpenCore 配套的官方启动器，不附带其他操作系统的引导程序。

## Tahoe / Sequoia 驱动分支

- `AirportItlwm_NativeSequoia.kext`：Darwin 24.2–24.99。
- `AirportItlwm_NativeTahoe.kext`：Darwin 25.0–25.99。
- `SimpleHDA.kext` 1.0.0：Tahoe 音频适配，加载顺序在 Lilu / AppleALC 之后；系统内 HAL 插件不随 EFI 打包。
- `RestrictEvents.kext` 1.1.6：Darwin 25 范围，包括当前 CPU 显示名称相关配置。

两套无线分支都列在配置中，但按 Darwin 范围分别加载。没有分发含自动连接网络字段的备用 itlwm，也没有启用旧的 IOSkywalk / IO80211 遗留方案。完整版本见 [COMPONENTS.md](COMPONENTS.md)。

USB 仍使用现有 USBInjectAll / XHCI-unsupported 和本机内部端口设置，不能代替其他机器的 USB 定制。背光表与 ACPI 热补丁也与本机固件相关。

## 菜单与诊断

正常菜单保留系统入口，工具作为辅助项隐藏，按空格显示；已有 Windows / Ubuntu 引导仍可由 OC 调用。

当前没有 `-x`、`-v` 或 `-nvoff`。配置保留 `keepsyms=1 debug=0x100`，且 `AppleDebug=true`：本次忠实同步现有诊断配置，没有重新关闭调试开关。需要详细跑码时再加 `-v`。

## 校验与来源

配置通过同版本 OpenCore 1.0.8 `ocvalidate`；已核对所有启用的 ACPI、Kext、驱动及工具路径。除脱敏和移除未启用备用条目外，保留内置 EFI 的启用配置和二进制文件。详细范围见 [VALIDATION.json](VALIDATION.json)，差异见 [CHANGELOG.md](CHANGELOG.md)。

在仓库根目录校验文件：

```sh
shasum -a 256 -c SHA256SUMS
```

第三方驱动、程序与图标遵循各自许可证。来源与已收集的许可文本见 [THIRD_PARTY.md](THIRD_PARTY.md) 和 `LICENSES/`，本项目不将整包重新声明为 MIT。
