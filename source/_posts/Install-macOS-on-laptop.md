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

#### 根据机器配置定制`kext`

#### 根据机器配置定制`config`

#### 了解`drivers64UEFI`各个`.EFI`文件的作用，精简引导

#### 了解`acpi`的工作原理，完美黑苹果(进阶篇)

### 准备工作
- 集成`CLOVER`的原版镜像
> 链接:https://pan.baidu.com/s/1kVmXVh1  密码:ryn9
- `Transmac`
> 链接:https://pan.baidu.com/s/1nuFJNz7  密码:w3xw
- `easyUEFI`
> 链接:https://pan.baidu.com/s/1eSnNWB4  密码:vzbh
- `Clover Configurator`四叶草助手
> 链接:https://pan.baidu.com/s/1bUZrjg  密码:0752
- `DiskGenius`
> 链接:https://pan.baidu.com/s/1dESlt8P  密码:2yp2
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

