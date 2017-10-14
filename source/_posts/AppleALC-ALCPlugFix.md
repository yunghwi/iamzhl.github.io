---
title:      AppleALC-ALCPlugFix
date:       2017-08-29
categories: Hacintosh
tags:
    - Hacintosh
    - 黑苹果
---
 
> 简单制作 AppleALC 驱动声卡并解决耳机、外放切换、麦克风无输入以及耳机杂音问题

首先下载 AppleALC 源码，打开终端输入： 

```
git clone https://github.com/vit9696/AppleALC 
```
  
回车之后就将 AppleALC 下载到了你的用户根目录

![ALC-1](http://ovefvi4g3.bkt.clouddn.com/ALC-1.png)

然后下载 Lilu ：

```
链接:http://pan.baidu.com/s/1c2yB3vq 密码:if62
```

将下载的 Lilu 中 Debug 里面的 Lilu.kext 放进 AppleALC 源码根目录，然后删除 /AppleALC/Resources 中多余文件夹，只留下你的声卡型号文件夹、 Pinconfigs.kext 以及四个 plist 文件，以 cx20751 为例剩下如下文件

![ALC-2](http://ovefvi4g3.bkt.clouddn.com/ALC-2.png)

然后打开计算器，显示为编程器

![ALC-3](http://ovefvi4g3.bkt.clouddn.com/ALC-3.png)

打开你的 Codec ，找到 Vendor Id ，拷贝后面的字符串，在计算器选中十六进制，粘贴这个字符串

![ALC-4](http://ovefvi4g3.bkt.clouddn.com/ALC-4.png)

然后选中十进制，就换转换成十进制形式

![ALC-5](http://ovefvi4g3.bkt.clouddn.com/ALC-5.png)

拷贝这个十进制数，打开 /AppleALC/Resources/CX20751_2/Info.plist （此处的 CX20751_2 需要换成你的声卡型号），把 codecid 换成刚才拷贝的十进制数

![ALC-6](http://ovefvi4g3.bkt.clouddn.com/ALC-6.png)

保存退出，右键 PinConfigs.kext 显示包内容，打开里面的 Info.plist ，搜索刚才拷贝的十进制数，记下 LayoutID 数据，有几个记几个，都记下来，然后将 IOKitPersonalities->HDA Hardware Config Resource->HDAConfigDefault 中的其他型号删除，（为避免出错，这里的删除可以不操作，删除只是为了精简做出来的 AppleALC ），保存退出。

然后双击打开 AppleALC 中的工程文件：

![ALC-7](http://ovefvi4g3.bkt.clouddn.com/ALC-7.png)

按图示操作

![ALC-8](http://ovefvi4g3.bkt.clouddn.com/ALC-8.png)

点击右面的 export 

![ALC-9](http://ovefvi4g3.bkt.clouddn.com/ALC-9.png)

 next 

![ALC-10](http://ovefvi4g3.bkt.clouddn.com/ALC-10.png)

 where 填上桌面，点 export 就生成 AppleALC 在桌面上了，一层一层打开它，将其中的 AppleALC.kext 放到 clover 驱动目录，注意之前下载的 Lilu 里面的Release中 Lilu.kext 也要放到 clover 驱动目录，最后不要忘了在 config 注入 LayoutID ：

![ALC-11](http://ovefvi4g3.bkt.clouddn.com/ALC-11.png)

如图 Audio 处写上刚才记下的 LayoutID 到这里 AppleALC 的制作就完了，如果重启后你的声卡不能驱动，就挨个试刚才记录的所有 ID ，如果能驱动但无法做到插入耳机自动切换，接着往下看： 打开终端，输入 

```
git clone https://github.com/goodwin/ALCPlugFix 
```
  
回车就将ALCPlugFix下载到了你的用户目录，打开此目录中的 ALCPlugFix 中的 main.m 下拉到最下方，注意这一部分：

![ALC-12](http://ovefvi4g3.bkt.clouddn.com/ALC-12.png)

下载 had-tools ,将 codec 复制到 had-tools 目录，打开终端， cd 到此目录，输入 

```
./widget_dump.sh codec.txt 
```

回车，此处 codec.txt 要根据你的 codec 名字来，回车可以看到以下输出：

![ALC-13](http://ovefvi4g3.bkt.clouddn.com/ALC-13.png)

找到 nid = 0x19 和 nid = 0x1a ,这里我的 19 为 line in ， 1a 为 mic in ，记录下最后两位，我的是 04 和 24 就这么改

![ALC-14](http://ovefvi4g3.bkt.clouddn.com/ALC-14.png)

保存退出，双击按照 AppleALC 的编译方法编译这个

![ALC-15](http://ovefvi4g3.bkt.clouddn.com/ALC-15.png)

然后将生成的 ALCPlugFix 替换 alc_fix 中的 ALCPlugFix ，终端 cd 到 alc_fix 目录，执行 

```
./install.sh
```
    
耳机就可以自动切换了，三节点的朋友运气好的话杂音应该也解决了，这时插入耳机在执行 

```
./widget_dump.sh codec.txt
```

就可以发现之前的 19 和 1a 后面的数据反过来了

![ALC-16](http://ovefvi4g3.bkt.clouddn.com/ALC-16.png)

如果不行重启一次应该就好了。



