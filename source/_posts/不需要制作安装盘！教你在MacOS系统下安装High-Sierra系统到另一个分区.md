---
title: 不需要制作安装盘！教你在MacOS系统下安装High-Sierra系统到另一个分区
description: 不需要制作安装盘！教你在MacOS系统下安装High-Sierra系统到另一个分区
date: 2017-10-15 20:01:47
categories: Hacintosh
tags: 
    - High Sierra 
    - Hacintosh 
---

### 不需要制作安装盘！教你在 MacOS 系统下安装 High Sierra 系统到另一个分区

* 此方法不支持APFS分区。 只有HFS分区才能通过此安装 High Sierra 。 而且，您还需要一个 GUID 分区表来安装它。

* 我无法通过我的安装 USB 启动 macOS High Sierra 安装程序。 尝试了很多方法，但没有一个实际上使其启动，所以我发现一种新的方式通过我现在使用的实际引导的 macOS 在 HFS 日志分区上安装 macOS High Sierra 成功。 

* 通过这种方式，您不需要创建可引导的U盘，甚至不需要重新启动。 

#### 开始

* 通过 App Store 下载 macOS High Sierra Developer Beta / Public Beta 安装程序。

* 在要使用磁盘实用程序安装测试版的驱动器上创建HFS日志分区（名称不能包含空格）。

* 打开应用程序目录，右键单击macOS Beta安装程序，然后单击显示包内容。 转到 Contents/Shared Support ，然后双击 InstallESD.dmg 和 BaseSystem.dmg 挂载它们。

* 打开安装的 InstallESD.dmg ，打开Packages文件夹，然后打开 OSInstall.mpkg 文件。 这将打开 macOS 安装程序。 是的，现在您可以从启动的 MacOS 系统桌面安装 High Sierra Beta 。 选择您创建的分区以安装它，安装完成后，按照步骤5。
> 备注：如果无法安装请执行(其中 HighSierra 是指卷名)：
> 
> diskutil umount HighSierra
> diskutil mount HighSierra

* 打开已安装的 BaseSystem.dmg 并将 boot.efi 从 /System/Library/CoreServices 复制到同一位置的 High Sierra 分区 (/System/Library/CoreServices)

* 虽然安装了HS测试版，但它尚不可启动。 要使其可引导，请打开 终端 并键入以下命令(假设安装分区为：HighSierra)：

##### 命令1：

```
$ sudo bless --folder "/Volumes/HighSierra/System/Library/CoreServices"
```

##### 命令2：

```
$ sudo bless --mount "/Volumes/HighSierra" --setBoot
```

##### 命令3：检查分区是否可引导。 为此，键入以下命令：

```
$ bless --info /Volumes/HighSierra
```

#### 收工






