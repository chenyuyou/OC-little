# 添加缺失的部件

## 描述

添加缺失的部件只是一种完善方案，非必要！

### 使用说明

**DSDT中:**

- 搜索 `PNP0200`，如果缺失，可添加 ***SSDT-DMAC***。

- 搜索 `MCHC`，如果缺失，可添加 ***SSDT-MCHC***。

- 搜索 `PNP0C01`，如果缺失，可添加 ***SSDT-MEM2***。

- 搜索 `0x00160000`，如果缺失，可添加 ***SSDT-IMEI***。

- 6 代以上机器，搜索 `0x001F0002`，如果缺失，可添加 ***SSDT-PPMC***。

- 6 代以上机器，搜索 `PMCR` 、 `APP9876`，如果缺失，可添加 ***SSDT-PMCR***。

  说明：@请叫我官人 提供方法，目前已成为 OpenCore 官方的 SSDT 示例。
  > Z390 芯片组 PMC (D31:F2) 只能通过 MMIO 启动。由于 ACPI 规范中没有 PMC 设备，苹果推出了自己的命名 `APP9876`、从 AppleIntelPCHPMC 驱动中访问这个设备。而在其它操作系统中，一般会使用 `HID: PNP0C02`、`UID: PCHRESV` 访问这个设备。  
  > 包括 APTIO V 在内的平台，在初始化 PMC 设备之前不能读写 NVRAM（在 SMM 模式中被冻结）。  
  > 虽然不知道为什么会这样，但是值得注意的是 PMC 和 SPI 位于不同的内存区域，PCHRESV 同时映射了这两个区域，但是苹果的 AppleIntelPCHPMC 只会映射 PMC 所在的区域。  
  > PMC 设备与 LPC 总线之间毫无关系，这个 SSDT 纯粹是为了加快 PMC 的初始化而把该设备添加到 LPC 总线下。如果将其添加到 PCI0 总线中、PMC 只会在 PCI 配置结束后启动，对于需要读取 NVRAM 的操作来说就已经太晚了。

- 搜索 `PNP0C0C`，如果缺失，可添加 ***SSDT-PWRB***。

- 搜索 `PNP0C0E`，如果缺失，可添加 ***SSDT-SLPB***，《PNP0C0E睡眠修正方法》需要这个部件。

- 9代+机器，搜索 `XSPI` 、 `0x001F0005`，如果缺失，可添加 ***SSDT-XSPI***。

### 注意

使用以上部分补丁时，注意 `LPCB` 名称应和原始ACPI名称一致。
