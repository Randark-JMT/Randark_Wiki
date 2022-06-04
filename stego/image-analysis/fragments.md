---
title: 图像碎片
description: 
published: true
date: 2022-06-04T01:36:10.290Z
tags: 
editor: markdown
dateCreated: 2022-06-04T01:31:46.533Z
---

最简单的对半分的拼图，这里就不讲了，一般情况都可以用图像编辑软件直接一把梭。这里着重讲的是两种拼图类隐写以及解题用的脚本。顺便也讲一些拼接二维码的题型。

## 只给出碎片

这个刷过BUUCTF的人应该都比较熟悉，最经典的拼图题莫过于[MRCTF2020-不眠之夜](https://buuoj.cn/challenges#%5BMRCTF2020%5D%E4%B8%8D%E7%9C%A0%E4%B9%8B%E5%A4%9C)。

首先，将会用到两个工具：

1. montage

`ImageMagick`是一套适用于Linux的工具，可让您从命令行操作图像。其中含有一个工具：`montage`，可以用来将多张图片合并为一张大图片。

举个例子：

```shell
montage *jpg -tile 10x12 -geometry +0+0 flag.jpg
*jpg #指输入此目录下所有的jpg文件，可以替换为*.png之类的
-title #设置大图片每一边由几张小图片来组成
-geometry #设置每一张图片之间的间距
flag.jpg #设置输出文件
```

1. gaps

https://github.com/nemanja-m/gaps ，基于遗传算法的拼图求解器，具有块大小自动检测功能。 用于自动化拼图。

举个例子：

```shell
gaps --image=flag.jpg --generations=40 --population=120 --size=100
–image 指向拼图的路径
–size 拼图块的像素尺寸，即碎片的宽度
–generations 遗传算法的代的数量
–population 个体数量
–verbose 每一代训练结束后展示最佳结果
–save 将拼图还原为图像
```

就是40代的遗传算法来计算图像边界，来实现自动拼图。
需要注意的是，`gaps`在计算和识别图像碎片的时候，碎片的形状必须是正方形，如果出现碎片形状是长方形的情况，则需要算出其最小公约的正方形块的大小。

## 给了原图和大量碎片

这一块内容与上面有所不同，这种情况下虽然有原图信息，但是往往给出了大量的碎片（这些碎片）

```python
from cv2 import cv2
from PIL import Image
import os
import shutil
# 读取目标图片
target = cv2.imread(r"./flag.jpg")


def match(temp_file):
    # 读取模板图片
    template = cv2.imread(temp_file)
    # 获得模板图片的高宽尺寸
    theight, twidth = template.shape[:2]
    # 执行模板匹配，采用的匹配方式cv2.TM_SQDIFF_NORMED
    result = cv2.matchTemplate(target, template, cv2.TM_SQDIFF_NORMED)
    # 归一化处理
    cv2.normalize(result, result, 0, 1, cv2.NORM_MINMAX, -1)
    # 寻找矩阵（一维数组当做向量，用Mat定义）中的最大值和最小值的匹配结果及其位置
    min_val, max_val, min_loc, max_loc = cv2.minMaxLoc(result)
    return abs(min_val)


dst_path = r"./flag"
dirs = os.listdir(r"./png")
count = 0
for k in dirs:
    if(k.endswith('png')):
        count += 1
        print("processing on pic"+str(count))
        real_path = os.path.join(r"./png", k)
        rect = match(real_path)
        if rect > 1e-10:
            print(rect)
            shutil.move(real_path, dst_path)
        else:
            continue

```

如上，直接调用PIL库进行模板比对，可以非常快速地将与原图不对应（即被修改过的，涂鸦过的）的图像碎片快速筛选出来。

## 按顺序给出01

这类题目一般都是直接给`1111101010010`这样的数据，以此来表示二维码从左到右，从上至下的像素排列。对此，可以使用Python中的PIL库来进行绘图：
```python
from PIL import Image
import math
with open("test.txt", "r") as f:
    str = f.read()
i = 0
MAX = math.sqrt(len(str))
# 要求二维码是正方形
print(int(MAX))
pic = Image.new("RGB", (MAX, MAX))
for y in range(0, MAX):
    for x in range(0, MAX):
        if(str[i] == '1'):
            pic.putpixel([x, y], (0, 0, 0))
        else:
            pic.putpixel([x, y], (255, 255, 255))
    i = i+1
pic.show()

```

## 给出坐标，输出二维码

二维码的像素坐标有可能是要去找，也有可能题目会直接提供，但是都要求选手要自己绘画出来。届时可以利用`plot`绘制出坐标