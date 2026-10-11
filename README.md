# Lenovo Y9000P 2022 · macOS Sequoia

联想拯救者 Y9000P 2022 的个人 OpenCore EFI，记录 i7-12700H + RTX 3070 Laptop 在 macOS Sequoia 下的实际配置与排查结果。

**本机已实现内置硬盘正常启动、NVIDIA Metal 图形加速、内屏 165 Hz，以及 AX211 Wi-Fi / 基础蓝牙。睡眠唤醒黑屏仍未解决。** 系统级 NVIDIA 驱动需单独安装，EFI 本身不提供显卡加速。

公开配置已清空序列号、MLB、UUID 和 ROM。使用前必须填写自己的 SMBIOS；本仓库不含私人日志和原始 EFI 备份。

## 硬件与实测环境

| 项目 | 配置 |
|---|---|
| 机型 | Lenovo Legion / 拯救者 Y9000P 2022 |
| CPU | Intel Core i7-12700H，Alder Lake |
| 显卡 | NVIDIA GeForce RTX 3070 Laptop GPU，8 GB |
| 内屏 | 2560 × 1600，165 Hz |
| 无线网卡 | Intel AX211 |
| macOS | Sequoia 15.8.1 |
| OpenCore | 1.0.8 RELEASE，OpenCanopy |
| SMBIOS | MacBookPro16,1 |
| 系统级显卡驱动 | NullMoth / 1401 1.13.0 |

截至 2026-10-11，原机器正常进入 macOS 桌面，并确认升级 1.13.0 后可使用内屏 165 Hz。脱敏后的配置未在另一台机器测试，同型号也应先用 U 盘验证。

## 功能状态

| 功能 | 状态与范围 |
|---|---|
| macOS 启动 | 已确认从内置硬盘正常启动 |
| NVIDIA 图形加速 | 本机已识别 RTX 3070 Laptop，Metal 3；需配套系统级驱动 |
| 内屏 165 Hz | 1.13.0 下本机实测可用；旧驱动曾出现登录闪屏 / 黑屏 |
| AX211 Wi-Fi | NativeSequoia 驱动方案；保留已成功的组合 |
| 基础蓝牙 | AirPods Pro 可连接并播放音乐 |
| AirDrop / iPhone 镜像 | 未实现 |
| 睡眠 / 唤醒 | **未解决**：电源键可恢复灯、风扇和外设，内屏持续黑屏 |
| USB 与其他接口 | 未完成全部端口、全部功能的验证 |

睡眠问题在升级显卡驱动后仍存在。较早的 1.9.0 唤醒日志中，至少一次出现 `Xid 79: GPU has fallen off the bus`，另一轮没有同样错误；不能据此认定所有黑屏都来自同一原因。目前没有经本机验证的修复，日常使用暂不依赖睡眠。

## 两个关键 EFI 设置

这两个设置帮助本机越过早期内核崩溃，进入恢复环境：

```text
Booter -> Quirks -> DevirtualiseMmio = false
Kernel -> Quirks -> ProvideCurrentCpuInfo = true
```

随后遇到的 `Non-monotonic time` 通过 **CpuTscSync 1.1.2** 解决，加载顺序紧跟 Lilu。它们是本机有效的配置记录，其他机器不能直接照搬。

仓库保留已成功的 CPUID 伪装、ACPI、AMFI、显卡启动参数与无线驱动组合。具体版本见 [COMPONENTS.md](COMPONENTS.md)。

## 显卡驱动

从 [NullMoth NVIDIA 驱动的官方发布页](https://github.com/nullmoth/nvidia-macos-driver/releases/tag/v1.13.0) 获取 1401 Mac 安装工具和配套驱动。系统级驱动、固件和 Metal 组件不包含在本仓库中，需按照对应版本的官方安装流程完成安装与授权。

本机 1.13.0 的 165 Hz 结果不代表所有屏幕都支持。上游仍记录部分显示器超过 60 Hz 后黑屏或闪烁；首次安装建议先使用 60 Hz，确认普通启动稳定后再测试高刷新率。

## 使用方法

1. 备份当前可启动的 EFI。
2. 为 `MacBookPro16,1` 生成自己的机器标识，填写 `EFI/OC/config.plist -> PlatformInfo -> Generic` 中的 `SystemSerialNumber`、`MLB`、`SystemUUID` 和 6 字节 Data 类型的 `ROM`。公开文件前三项为空，ROM 为零值。可使用 [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)。
3. 先将公开包的 `EFI` 放入独立 U 盘的 EFI 分区，确认可以进入恢复环境或已安装系统。
4. 按官方流程安装与配置配套 NVIDIA 驱动，先验证普通刷新率和重新启动。
5. 确认可用后迁移到内置硬盘的 EFI 系统分区，保留其他系统已有的引导目录。

```text
EFI/
  BOOT/BOOTx64.efi     OpenCore 1.0.8 官方启动器
  OC/
    config.plist      已脱敏，需填写自己的 SMBIOS
    OpenCore.efi
    ACPI/  Drivers/  Kexts/  Resources/  Tools/
```

当前 USB 设置仍包含 USBInjectAll / XHCI-unsupported 和本机蓝牙端口配置，不能替代另一台机器的 USB 定制。ScanPolicy 使用原机 NVMe + APFS/ESP 扫描范围，需要使用其他存储设备时应自行调整。

## 无线与蓝牙组合

- Wi-Fi：`AirportItlwm_NativeSequoia.kext` 2.4.0，限定 Darwin 24.2–24.99。
- 蓝牙：IntelBluetoothFirmware 2.3.0 RELEASE、IntelBTPatcher 2.5.0 DEBUG、BlueToolFixup 2.7.2 DEBUG。
- 保留 DebugEnhancer 1.1.1；这两个 DEBUG 蓝牙构建属于本机已成功的组合。

基础蓝牙可用不等于苹果连续互通功能可用；本机未实现隔空投送与 iPhone 镜像。

## 启动界面与诊断

配置已移除 `-v` 和无线调试启动参数，关闭 AppleDebug 屏幕输出，保留文件日志、`keepsyms=1` 与 `debug=0x100`。排查启动问题时可先加回 `-v`。

OpenCanopy 保留 macOS 入口；也可引导已有的 Windows / Ubuntu。辅助工具默认隐藏，按空格显示。本包不附带其他系统的引导程序或绑定原机分区的配置。

## 校验与来源

配置通过 OpenCore 1.0.8 `ocvalidate`，启用的 ACPI、驱动及 Kext 路径已检查。公开包包含 SHA-256 清单，在仓库根目录运行：

```sh
shasum -a 256 -c SHA256SUMS
```

静态校验不代替本机启动测试。版本与验证范围见 [VALIDATION.json](VALIDATION.json)。

本项目仅整理配置与实测记录。第三方驱动、程序与图标遵循各自许可证，完整来源见 [THIRD_PARTY.md](THIRD_PARTY.md) 与 `LICENSES/`；整包不重新声明为 MIT。
