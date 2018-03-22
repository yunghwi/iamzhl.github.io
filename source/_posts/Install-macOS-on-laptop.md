---
title: Install macOS on laptop
date: 2017-12-12 00:28:15
categories: Hacintosh
description: Install macOS on laptop 原版黑苹果安装参考贴
tags:
- Hacintosh
- 黑苹果
---

### 前言
对于支持`UEFI`的机器，我们通常用`CLOVER`引导原版安装，这种方式最大的优点就是有恢复分区可以正常升级，当然前提要把引导做好。

### 前期知识储备
#### `CLOVER`的目录结构
`CLOVER`正常工作需要的完整目录包括`config.plist`、`CLOVERX64.efi`两个文件以及`ACPI`、`drivers64UEFI`、`kexts`、和`themes`几个目录，如图所示：

![2018-02-12-01](http://ovefvi4g3.bkt.clouddn.com/2018-02-12-01.png)

其中，`config.plist`是最核心的文件----配置文件，`CLOVER`所实现的多数功能都是通过这个文件进行配置的，对其进行配置修改的最好用的工具就是`Clover Configurator`，主页面如下：

![2018-02-12-02](http://ovefvi4g3.bkt.clouddn.com/2018-02-12-02.png)

详细的配置方法下面会有介绍。

另外一个文件是`CLOVERX64.efi`，这个文件用以启动`CLOVER`引导，通过`EasyUEFI`或者`BIOS`对启动项进行添加操作时，就是指向的这个文件。

`ACPI`是用以存放机器`ACPI`表单的，全称是"高级配置和电源管理接口"`(Advanced Configuration and Power Interface)`，其子目录由`origin`、`patched`、`WINDOWS`构成，其中`origin`用以保存通过在`CLOVER`引导界面按`F4`或`Fn F4`提取的原始表单，此目录的所有表文件是不加载的，需要对其进行编译得到`.dsl`文件，然后对其进行修改拍错，最后保存成`.aml`文件保存至`patched`目录才会在启动时加载，而`WINDOWS`目录则可以忽略不计。在黑果中，我们用到的表单文件只有`SSDT`和`DSDT`，其中`DSDT`主要是对各种设备的描述，而`SSDT`则主要是用以实现某个功能。

`drivers64UEFI`是由各种`EFI`驱动组成，在笔记本黑果需要用到的有`FSInject-64.efi`、`HFSPlus-64.efi`、`OsxAptioFixDrv-64.efi`、`APFS.efi`以及`OsxFatBinaryDrv-64.efi`，在新版`CLOVER`中只需要`FSInject-64.efi`、`HFSPlus-64.efi`、`OsxAptioFixDrv-64.efi`、以及`APFS.efi`

`kexts`主要用于存储各种驱动(`OS X`称为内核扩展)

`themes`用以存储`CLOVER`引导界面的主题

#### 根据机器配置定制`kext`
`kext`在`EFI`的配置中是相当重要的，好的`kext`配置可以弥补`config`的不足，不好的`kext`配置也会让本应完美的`config`发挥不出作用。

`FakeSMC.kext`:`FakeSMC`是现今的⿊黑苹果过程中唯⼀一的”必要性”内核扩展程序。对于⿊黑苹果有着⽆与伦比的重要性，但是很多⼈并不知道`FakeSMC`为什么重要，只是知道必须有它才⾏行，。简⽽而⾔言之就是: `FakeSMC`是⽤用于将`PC`主板上的各种控制芯⽚片伪装成Mac独有的硬件控制芯片`SMC`以骗过系统从⽽而是系统正常启动的⼀一个内核扩展(其实很复杂,这⾥里不多说了)。在系统启动的阶段，`FakeSMC`负责告知系统有关主板上`SMC`芯⽚片(伪装出来的)的各种加密信息，欺骗系统。也就是说呢，无论你笔记本是什么配置，此驱动是必须的。

`ApplePS2SmartTouchPad.kext`、`VoodooI2C.kext`和`VoodooPS2Controller.kext`:此驱动用以驱动键盘鼠标以及触摸板，两者选择一个即可，两者区别就是适用的类型不一样，有`PS/2`、`Synaptics`、`alps`、`i2c`等等，其中`Synaptics`、`alps`用`ApplePS2SmartTouchPad.kext`适配性好一些，`VoodooI2C.kext`比较麻烦，仅适用于`i2c`触摸板。具体怎么确定走的总线类型，大家参考百度就好了，这里就不再赘述。

`FakePCIID.kext`:这个kext的目的是与`IOPCIDevice`设备建立连接，以便当另一个驱动程序连接到同一设备时，它可以提供备用的`PCI ID`。也就是说，如果用到`Fake-PCI-ID`中的其他任何`kext`的话，此驱动都是必要的。

`FakePCIID_Intel_HD_Graphics.kext `:此驱动主要用于核显`HD4200 HD4400 HD4600 P4600`、`Iris 540 Iris 550 Iris Pro 580`、`HD510 HD515 HD520 HD530 P530`（多数530不需要这个）、`P4000`、`P6300 - 162a`、`UHD620 KabyLake-R `、`UHD630 CoffeeLake`。

`FakePCIID_Intel_HDMI_Audio.kext`:其目的是为不支持的`HDAU`提供支持(通常称为`B0D3`，但需要将其重命名为`HDAU`，以满足`Apple`的期望值)在`Haswell`以上的系统中提供`HDMI-audio`的设备。

`FakePCIID_BCM57XX_as_BCM57765.kext`:这个`kext`将与众多不受支持的`BCM57XX`以太网设备建立连接，以使本机驱动程序为兼容的更广泛的`BCM`以太网芯片组工作。

`FakePCIID_Intel_GbX.kext`:这个`kext`将与一些`Intel`以太网设备建立连接，以使基于`Intel`芯片组的驱动程序工作。

`FakePCIID_XHCIMux.kext`:将会连接到8086:1e31, 8086:9c31, 8086:9cb1, 8086:9c31, 8086:8cb1这个注入器是正常的`FakePCIID`任务的一部分。它实际上并没有伪造任何`PCI id`。相反，它将某些值强加于`Intel XHCI USB3`控制器上的`XUSB2PR` (PCI配置偏移`0xD0`)。其效果是将任何`USB2`设备与`XHC`端口上的`USB2`引脚连接到`EHC1`。换句话说，使用USB2驱动而不是`USB3`驱动程序(`AppleUSBEHCI vs AppleUSBXHCI`)处理`USB2`设备。

`FakePCIID_AR9280_as_AR946x`:这是`FakePCIID.kext`的特殊应用，是在一个`AR9280`被重新命名为其他设备的情况下使用的。例如，在联想`u430`中，将一个`AR9280`作为`AR946x`重新命名是很有用的，因为该设备可以被`BIOS`白名单所允许，而`AR9280`不是。通过使用`FakePCIID`，我们可以将`PCI id`重新映射回`AR9280` (168c:002a)，即使该设备本身报告的是168c:0034。

`FakePCIID_Broadcom_WiFi.kext`:这个`kext`将连接到14e4:43b1, 14e4:4357, 14e4:4331, 14e4:4353, 14e4:432b, 14e4 . 432b, 14e4:43a3，或14e4:43a0。以及106b:4e, 14e4:4312, 14e4:4313, 14e4:4318, 14e4:431a, 14e4:4320, 14e4:4324, 14e4:4324, 14e4:4328, 14e4:432d。
最初是为`BCM94352Z`创建的，这个特殊的FakePCIID应用程序。在使用多种支持的Broadcom WiFi设备时，kext被用来模拟真正的`Apple Airport`(苹果无线网卡)。

`ACPIBatteryManager.kext`:用以使笔记本正确显示电量，但通常需要配合`DSDT`的`patch`才能发挥作用。

`VoodooHDA.kext`:万能声卡驱动，用以禁用`AppleHDA`来驱动声卡。

`AppleALC.kext`:通过对`AppleHDA`的动态`patch`实现对`AppleHDA`的完整加载。

`Lilu.kext`:一个开放源码的内核扩展，为macOS系统提供了一个任意的kext、库和程序补丁的平台。

`IntelGraphicsDVMTFixup.kext`:修复因`BIOS`显存分配不足造成的`KP`。建议`broadwell+`平台使用。

`IntelGraphicsFixup.kext`:动态修复核显的各种问题(例如腾讯视频死机，开机二阶段花屏等)，建议`Haswell+`平台使用。

`CoreDisplayFixup.kext`:为不受支持的4K机器(非`Iris`)开启高分辨率支持。

`AzulPatcher4600.kext`:针对`HD4600`的额外修复，仅推荐`HD4600`使用。

`HibernationFixup.kext`:修复睡眠，以支持某些机器在3和28休眠模式下的正常休眠。

`NvidiaGraphicsFixup.kext`:修复某些n卡的黑屏。

`WhateverGreen.kext`:用以驱动A卡。

`RealtekRTL8111.kext`:用以驱动`RealtekRTL8111.kext`以太网卡设备。

`AppleIGB.kext`、`IntelMausiEthernet.kext`:用以驱动`Intel`板载网卡设备。

#### 根据机器配置定制`config`

#### 了解`drivers64UEFI`各个`.EFI`文件的作用，精简引导

#### 了解`acpi`的工作原理，完美黑苹果(进阶篇)

### 准备工作
- 集成`CLOVER`的原版镜像
> 链接:https://pan.baidu.com/s/1gfTmRj9  密码:s3dv
- `Transmac`
> 链接:https://pan.baidu.com/s/1oAn79Zc  密码:yafn
- `easyUEFI`
> 链接:https://pan.baidu.com/s/1nwqbnMp  密码:gxoc
- `Clover Configurator`四叶草助手
> 链接:https://pan.baidu.com/s/1ht2wFQW  密码:tbce
- `DiskGenius`
> 链接:https://pan.baidu.com/s/1cVyULo  密码:pfrm
- 鲁大师/`AIDA64`(推荐)/也可以用设备管理器
> 自行度娘

### 安装步骤
#### 利用`Transmac`将原版镜像写入u盘

#### 利用鲁大师等软件查看自己机器的配置信息，来定制`config`和需要用的`kext`

#### 重启利用`U`盘启动选择安装盘

#### 选择磁盘工具，并将事先分好的分区抹成扩展日志式或者`apfs`

#### 退出磁盘工具，选择安装`macos`选中刚才抹掉的分区开始安装

### 转移`CLOVER`到硬盘`ESP`，摆脱`U`盘引导

### 后期的驱动安装以及优化
#### 屏蔽无用的独显降低温度
参考我之前的帖子：[Disable the discrete GPU in laptop](https://blog.iamzhl.top/2017/10/04/Disable%20the%20discrete%20GPU%20in%20laptop/)

#### 摆脱万能声卡，利用`AppleALC`加载原生声卡
参考我之前的帖子：[谈谈黑果的声卡](https://blog.iamzhl.top/2017/11/06/%E8%B0%88%E8%B0%88%E9%BB%91%E6%9E%9C%E7%9A%84%E5%A3%B0%E5%8D%A1/)

#### 通过对`DSDT`打补丁完善电池显示
参考`pcbeta`[daxuexinsheng的帖子](http://bbs.pcbeta.com/viewthread-1595139-1-1.html)
`tonymacx86`[RehabMan的帖子](https://www.tonymacx86.com/threads/guide-how-to-patch-dsdt-for-working-battery-status.116102/)

#### 加载`x86`实现变频和原生电源管理，完善节能器信息
- 对于`Haswell`以及`Broadwell`平台，利用`ssdtPRGen`生成`SSDT`，在`config`中`drop`掉`CpuPm`和`Cpu0Ist`两个表，并利用`FakeSMC`或`DSDT`或`hotpatch`加载`AppleLPC`
- 对于`Skylake`及以上平台，选择支持`HWP`的合适的机型，并勾选`HWPEnable`即可开启完整电源特性

#### 修复八苹果二阶段花屏等问题完善显卡驱动

#### 注入`HiDPI`和显示器信息完善唤醒后的花屏问题

