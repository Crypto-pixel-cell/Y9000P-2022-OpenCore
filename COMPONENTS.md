# 随包驱动版本

版本由 2026-10-31 内置 EFI 的实际 Info.plist 读取。保留当前启用组合，未额外升级二进制。

| 驱动 | 版本 | Darwin 最低版本 | Darwin 最高版本 |
|---|---|---|---|
| Lilu.kext | 1.7.3 | 不限 | 不限 |
| CpuTscSync.kext | 1.1.2 | 不限 | 不限 |
| DebugEnhancer.kext | 1.1.1 | 24.0.0 | 不限 |
| VirtualSMC.kext | 1.3.8 | 不限 | 不限 |
| YogaSMC.kext | 1.5.3 | 不限 | 不限 |
| WhateverGreen.kext | 1.7.1 | 不限 | 不限 |
| SMCBatteryManager.kext | 1.3.8 | 不限 | 不限 |
| SMCLightSensor.kext | 1.3.8 | 不限 | 不限 |
| SMCProcessor.kext | 1.3.8 | 不限 | 不限 |
| SMCSuperIO.kext | 1.3.8 | 不限 | 不限 |
| AppleALC.kext | 1.9.8 | 不限 | 不限 |
| SimpleHDA.kext | 1.0.0 | 25.0.0 | 25.99.99 |
| AMFIPass.kext | 1.4.1 | 18.0.0 | 不限 |
| RealtekRTL8111.kext | 2.4.2 | 不限 | 不限 |
| USBInjectAll.kext | 1.0 | 不限 | 不限 |
| XHCI-unsupported.kext | 0.9.2 | 17.0.0 | 不限 |
| BlueToolFixup.kext | 2.7.3 | 21.0.0 | 不限 |
| IntelBluetoothFirmware.kext | 2.3.0 | 不限 | 不限 |
| IntelBTPatcher.kext | 2.5.0 | 21.0.0 | 不限 |
| VoodooPS2Controller.kext | 2.3.8 | 不限 | 不限 |
| VoodooPS2Controller.kext/Contents/PlugIns/VoodooPS2Keyboard.kext | 2.3.8 | 不限 | 不限 |
| BrightnessKeys.kext | 1.0.3 | 不限 | 不限 |
| VoodooI2C.kext/Contents/PlugIns/VoodooInput.kext | 1.1.7 | 不限 | 不限 |
| VoodooI2C.kext/Contents/PlugIns/VoodooI2CServices.kext | 1 | 不限 | 不限 |
| VoodooI2C.kext/Contents/PlugIns/VoodooGPIO.kext | 1.1 | 不限 | 不限 |
| VoodooI2C.kext | 2.9.1 | 不限 | 不限 |
| VoodooI2CHID.kext | 1 | 不限 | 不限 |
| AirportItlwm_NativeSequoia.kext | 2.4.0 | 24.2.0 | 24.99.99 |
| AirportItlwm_NativeTahoe.kext | 2.4.0 | 25.0.0 | 25.99.99 |
| RestrictEvents.kext | 1.1.6 | 25.0.0 | 25.99.99 |

Darwin 24 对应 Sequoia；Darwin 25 对应 Tahoe。SimpleHDA、RestrictEvents 和 NativeTahoe 仅在配置指定的 Darwin 25 范围内加载。
未启用的备用驱动不随公开包分发，包括含自动连接网络字段的 itlwm。系统级 NVIDIA 驱动须另外安装，EFI 文件中无法确认系统内实际安装版本。
