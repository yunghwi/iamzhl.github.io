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

