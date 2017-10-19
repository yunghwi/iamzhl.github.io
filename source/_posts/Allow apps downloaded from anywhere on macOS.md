---
title: Allow apps downloaded from anywhere on macOS
date: 2017-09-15 21:16:31
categories: Hacintosh
description: Allow apps downloaded from anywhere on macOS.md
tags:
    - 黑苹果
    - 终端
    - Hacintosh
    - 任何来源
---

> macOS 打开任何来源

许多朋友升级到 macOS Sierra 后在"安全性与隐私"中找不到"任何来源"选项，这里我教给大家怎么打开"任何来源"。

打开终端，输入以下代码回车：

```
sudo spctl --master-disable
```

会提示输入密码，根据提示输入密码回车之后就可以在"安全性与隐私"中打开"任何来源"选项了

