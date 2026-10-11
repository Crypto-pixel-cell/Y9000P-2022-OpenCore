# 第三方来源与许可

此包是个人 EFI 配置整理，不代表 Lenovo、Apple、Ubuntu 或各驱动作者的官方支持。第三方作品的版权和许可证保持原样；配置文档不赋予额外的第三方权利。

| 组件 | 上游来源 |
|---|---|
| OpenCore / OpenRuntime / OpenCanopy / 工具 | https://github.com/acidanthera/OpenCorePkg/tree/1.0.8 |
| OpenCanopy 资源、HfsPlus | https://github.com/acidanthera/OcBinaryData |
| Lilu | https://github.com/acidanthera/Lilu |
| CpuTscSync | https://github.com/acidanthera/CpuTscSync |
| DebugEnhancer | https://github.com/acidanthera/DebugEnhancer |
| VirtualSMC 及 SMC 插件 | https://github.com/acidanthera/VirtualSMC |
| YogaSMC | https://github.com/zhen-zen/YogaSMC |
| WhateverGreen | https://github.com/acidanthera/WhateverGreen |
| AppleALC | https://github.com/acidanthera/AppleALC |
| AMFIPass、未启用的遗留 Apple 网络组件 | https://github.com/dortania/OpenCore-Legacy-Patcher |
| RealtekRTL8111 | https://github.com/Mieze/RTL8111_driver_for_OS_X |
| USBInjectAll / XHCI-unsupported | https://github.com/RehabMan/OS-X-USB-Inject-All |
| IntelBluetoothFirmware / IntelBTPatcher | https://github.com/OpenIntelWireless/IntelBluetoothFirmware |
| BlueToolFixup | https://github.com/acidanthera/BrcmPatchRAM |
| VoodooPS2Controller | https://github.com/acidanthera/VoodooPS2 |
| BrightnessKeys | https://github.com/acidanthera/BrightnessKeys |
| VoodooI2C / VoodooI2CHID | https://github.com/VoodooI2C/VoodooI2C |
| AirportItlwm / NativeSequoia 方案 | https://github.com/OpenIntelWireless/itlwm ; https://github.com/laobamac/itlwm |
| Ubuntu 图标 | https://github.com/ubuntu/yaru/blob/master/icons/Yaru/256x256/places/start-here.png |
| 系统级 NVIDIA 驱动（不在本包内） | https://github.com/nullmoth/1401 |

`COMPONENTS.md` 列出实际二进制声明的版本，`SHA256SUMS` 标识具体文件。原 EFI 基于已有 Alder Lake 笔记本配置整理；部分原始二进制的构建提交、ACPI 作者及生成器版本没有可靠记录，不将上表当前源码冒充与所有二进制逐字对应的源码快照。

`LICENSES/` 保存可获取的原始许可证文本；各组件还可能附带其他版权声明。上游源码、构建说明与版本发布可从相应链接获取。遗留 Apple 网络组件保持未启用；上传压缩包中排除这些备用二进制，公开仓库的 .gitignore 也排除它们。

Ubuntu 图标由 Yaru 的 `start-here.png` 缩放为 128 / 256 像素，再封装为 ICNS，未改变图形。图标遵循 Yaru 原许可（CC BY-SA 4.0）；Ubuntu 名称和标志仍属相应权利人。OpenCanopy 修改的 Ubuntu 文本标签由本项目生成，其余资源来自 Acidanthera。
