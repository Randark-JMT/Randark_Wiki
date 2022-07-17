---
title: 图像隐写相关工具
description: 
published: true
date: 2022-07-17T01:55:17.226Z
tags: 
editor: markdown
dateCreated: 2022-07-17T01:55:17.226Z
---

# 工具类介绍

# Stegsolve

Github项目地址：[https://github.com/Giotino/stegsolve](https://github.com/Giotino/stegsolve)

这个工具应该讲图片隐写的时候是必须讲的，因为这个工具即可以实现图片单颜色通道的可视化查看，也实现了两张图片之间的交叉比对、结构数据检查、GIF逐帧查看等等。基本用于初步的图像分通道检查。

![javaw_qfewo6lyzr.png](/javaw_qfewo6lyzr.png)

# pillow

Python早先版本中`PIL`库的继任者，继承了原先`PIL`库的大部分功能，并且做出了加强。

使用语句：

```python
import PIL
```

![ffbcbb8b-a80e-4583-b94a-79a892029c53.png](/ffbcbb8b-a80e-4583-b94a-79a892029c53.png)

使用pillow库，可以做到逐像素生成或处理图像。

> 参考链接：[https://blog.csdn.net/lxy210781/article/details/89290600](https://blog.csdn.net/lxy210781/article/details/89290600)

```python
from PIL import Image
MAX=500
pic = Image.new("RGB", (MAX, MAX))
# pic为新建图像对象，设置为RGB格式，大小为500x500
pic = Image.open("*.jpg")
# 载入某图像文件为图像对象
pic.putpixel([x, y], (0, 0, 0))
# 在pic的（x,y）处放置一个（0,0,0,）纯黑的像素
pic.getpixel([x, y])
# 获取pic在（x,y）处的像素值pic.show()
```

# gnuplot

`gnuplot`是Linux上面的工具，可以接收用户输入的坐标信息，并在坐标系中绘制出来。`gnuplot`可以使用apt快速安装：

```python
$ gnuplot
Command 'gnuplot' not found, but can be installed with:
sudo apt install gnuplot-nox  # version 5.4.2+dfsg2-2, or
sudo apt install gnuplot-qt   # version 5.4.2+dfsg2-2
sudo apt install gnuplot-x11  # version 5.4.2+dfsg2-2
```

基础用法可见于`拼图类隐写`

# PhotoShop

`Photoshop`，以及同类软件（如`Krita`），作为万能的图像编辑工具，主要用来大块拼图的拼接，图像简单处理等领域。对于手撕的3*3的拼图有奇效（建议搭配微信扫码使用）

对于`Photoshop`收费的问题，可以找破解版，或者用`Krita`这类开源的图像编辑软件，或者直接用画图来处理。

# zsteg

Github项目地址：[https://github.com/zed-0xff/zsteg](https://github.com/zed-0xff/zsteg)

这是一个用于检测 PNG 和 BMP 中的隐写数据的软件，用`gem`即可安装在`Linux`上。对于正常数据，`zsteg`很少误报，但对于LSB隐写，Openstego隐写，wbstego4等等方式的隐写数据，都有较高的识别能力。

```bash
$ zsteg
Usage: zsteg [options] filename.png [param_string]

    -c, --channels X                 channels (R/G/B/A) or any combination, comma separated
                                     valid values: r,g,b,a,rg,bgr,rgba,r3g2b3,...
    -l, --limit N                    limit bytes checked, 0 = no limit (default: 256)
    -b, --bits N                     number of bits, single int value or '1,3,5' or range '1-8'
                                     advanced: specify individual bits like '00001110' or '0x88'
        --lsb                        least significant BIT comes first
        --msb                        most significant BIT comes first
    -P, --prime                      analyze/extract only prime bytes/pixels
        --invert                     invert bits (XOR 0xff)
    -a, --all                        try all known methods
    -o, --order X                    pixel iteration order (default: 'auto')
                                     valid values: ALL,xy,yx,XY,YX,xY,Xy,bY,...
    -E, --extract NAME               extract specified payload, NAME is like '1b,rgb,lsb'

        --[no-]file                  use 'file' command to detect data type (default: YES)
        --no-strings                 disable ASCII strings finding (default: enabled)
    -s, --strings X                  ASCII strings find mode: first, all, longest, none
                                     (default: first)
    -n, --min-str-len X              minimum string length (default: 8)
        --shift N                    prepend N zero bits

    -v, --verbose                    Run verbosely (can be used multiple times)
    -q, --quiet                      Silent any warnings (can be used multiple times)
    -C, --[no-]color                 Force (or disable) color output (default: auto)

PARAMS SHORTCUT
        zsteg fname.png 2b,b,lsb,xy  ==>  --bits 2 --channel b --lsb --order xy
```

# jphs

`jphide`算是`jpg`格式中最为常见的隐写方式，jphs则是在Windows平台支持分析`jphide`的软件。

![Untitled](%E5%B7%A5%E5%85%B7%E7%B1%BB%E4%BB%8B%E7%BB%8D%208c0673b8caf24025a0a853c13f951ecb/Untitled%202.png)

 便于在windows平台操作jphide隐写的jpg文件。

[GitHub - h3xx/jphs: jphide & seek steganography tools](https://github.com/h3xx/jphs)

[jphs05.zip](%E5%B7%A5%E5%85%B7%E7%B1%BB%E4%BB%8B%E7%BB%8D%208c0673b8caf24025a0a853c13f951ecb/jphs05.zip)

# imagemagick

[GitHub - ImageMagick/ImageMagick: 🧙‍♂️ ImageMagick 7](https://github.com/ImageMagick/ImageMagick)

# stegdetect

[GitHub - abeluck/stegdetect: UNMAINTAINED. USE AT OWN RISK. Stegdetect is an automated tool for detecting steganographic content in images.](https://github.com/abeluck/stegdetect)

这款工具的下载安装就有点麻烦了，经常会遇到环境不支持的问题，建议直接上`CTFHub`上面下载。其实现JPEG图像Jphide隐写算法工具有多个，比如由Neils Provos开发通过统计分析技术评估JPEG文件的DCT频率系数的隐写工具Stegdetect，它可以检测到通过JSteg、JPHide、OutGuess、Invisible Secrets、F5、appendX和Camouflage等这些隐写工具隐藏的信息，并且还具有基于字典暴力破解密码方法提取通过Jphide、outguess和jsteg-shell方式嵌入的隐藏信息。

[Stegdetect.zip](%E5%B7%A5%E5%85%B7%E7%B1%BB%E4%BB%8B%E7%BB%8D%208c0673b8caf24025a0a853c13f951ecb/Stegdetect.zip)

# **StegoVeritas**

[GitHub - bannsec/stegoVeritas: Yet another Stego Tool](https://github.com/bannsec/stegoVeritas/)

```bash
$ stegoveritas
usage: stegoveritas [-h] [-out dir] [-debug] [-password PASSWORD] [-wordlist WORDLIST] [-meta] [-imageTransform] [-bruteLSB]
                    [-colorMap [N ...]] [-colorMapRange Start End] [-extractLSB] [-red index [index ...]]
                    [-green index [index ...]] [-blue index [index ...]] [-alpha index [index ...]] [-extract_frames]
                    [-trailing] [-steghide] [-exif] [-xmp] [-carve]
                    file
```

个人感觉这款软件的分析速度和执行速度不错，并且一般情况下支持自动解码分析，简单情况下可以大大减少工作量，