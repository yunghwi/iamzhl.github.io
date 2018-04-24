---
title: Common command in macOS
copyright: true
date: 2018-03-30 20:32:47
categories: Hackintosh
description: macOS下一些常用命令
tags: 
- command
- macOS
---

## macOS下一些常用命令
<!--more-->

### Trim
> 开启
```
$ sudo trimforce enable
```

> 关闭
```
$ sudo trimforce disable
```

### 查看启用的`ig-platform-id`
```
$ ioreg -l | grep -y platform-id
```

### 笔记本开启插电源出提示音:
> 开启:
```
$ defaults write com.apple.PowerChime ChimeOnAllHardware -bool true; open /System/Library/CoreServices/PowerChime.app &
```

> 关闭:
```
$ defaults write com.apple.PowerChime ChimeOnAllHardware -bool false; killall PowerChime
```

### 去掉apfs.efi最新版本的日志调试显示
```
$ cd ~/Desktop						
& cp /usr/standalone/i386/apfs.efi .
$ perl -i -pe 's|\x00\x74\x07\xb8\xff\xff|\x00\x90\x90\xb8\xff\xff|sg' ./apfs.efi
```

### 提取显示器`EDID`及设备`ID`厂商`ID`
> EDID
```
$ ioreg -lw0 | grep -i "IODisplayEDID" | sed -e 's/.*<//' -e 's/>//'
```

> PID
```
$ ioreg -l | grep "DisplayProductID"    
```

> VID
```
$ ioreg -l | grep "DisplayVendorID"  
```

### 为`macOS Sierra`以上的`OS X`开启任何来源
```
$ sudo spctl --master-disable
```

### 查看加载的非官方内核扩展 -- `kext`
```
$ kextstat | grep -v "com.apple" | grep -v Energy
```

### 查看显示器硬件信息
> EDID
```
$ ioreg -l | grep "IODisplayEDID"
```

> ProductID
```
$ ioreg -l | grep "DisplayProductID"
```

> VendorID
```
$ ioreg -l | grep "DisplayVendorID"
```

### 待续...


