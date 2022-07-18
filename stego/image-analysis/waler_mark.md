---
title: 数字水印
description: 
published: true
date: 2022-07-18T06:36:01.075Z
tags: 
editor: markdown
dateCreated: 2022-07-18T06:36:01.075Z
---

数字水印目前在版权领域和产权鉴定十分流行，在CTF领域也非常常见。并且由于数字水印的特征会随着水印方式和算法的差异性而不同，故这里只介绍常见的数字水印和处理方式。

盲水印分为不同的种类，其中分为两大题型，分别是给出原图和水印图，要求分离信息，还有一种就是只给出水印图，要求分离信息。这里将两大类型各举一个经典工具，并加以说明。

需要注意的是，盲水印这类加密方式，是基本不可能通过简单查看颜色通道就能发现水印信息。

经典水印算法：

- 离散余弦变换（DCT）
- 离散傅里叶变换（DFT）
- 小波变换（WT）

## 给出了原图和水印图

- [chishaxie/BlindWaterMark: 盲水印 by python *Github*](https://github.com/chishaxie/BlindWaterMark)
- [双目失明，身残志坚 *DASCTF Sept X 浙江工业大学秋季挑战赛*](https://buuoj.cn/match/matches/36/challenges#%E5%8F%8C%E7%9B%AE%E5%A4%B1%E6%98%8E%EF%BC%8C%E8%BA%AB%E6%AE%8B%E5%BF%97%E5%9D%9A)
{.links-list}

这种题目的一般思路，就是两张图拿去做xor对比，看看是那些地方的像素不同，接下来就是套脚本解双图盲水印

## 只给了水印图

在这种情况下，基本上就要开始准备开始入手盲检测。盲检测最常用的方式，就是计算图像的熵，也就是图像中灰度分布的聚集特征所包含的信息量。

> 对于图像熵的计算，可以参考这篇文章：https://blog.csdn.net/a6333230/article/details/81021922
> 

并且给出两个比较知名的处理工具：

- [guofei9987/blind_watermark: 图片盲水印，提取水印无须原图！ *Github*](https://github.com/guofei9987/blind_watermark)
- [fire-keeper/BlindWatermark: 使用盲水印保护创作者的知识产权 *Github*](https://github.com/fire-keeper/BlindWatermark)
{.links-list}

对此，提供以下脚本用于基于FFT分析脚本：

```python
import cv2 as cv
import numpy as np
from matplotlib import pyplot as plt
img = cv.imread('download.jpg', 0)  # 读取图像
f = np.fft.fft2(img)  # 快速傅里叶变换算法得到频率分布
# 默认结果中心点位置是在左上角,调用fftshift()函数转移到中间位置
# 展示结果
plt.figure().patch.set_facecolor('gray')
plt.subplot(231), plt.imshow(img, 'gray'), plt.title('Original Fourier')
plt.axis('off')
fshift = np.fft.fft(f)
fimg = np.log(np.abs(fshift))  # fft结果是复数, 其绝对值结果是振幅
plt.subplot(232), plt.imshow(fimg, 'gray'), plt.title('fft')
plt.axis('off')
fshift = np.fft.fft2(f)
fimg = np.log(np.abs(fshift))  # fft结果是复数, 其绝对值结果是振幅
plt.subplot(233), plt.imshow(fimg, 'gray'), plt.title('fft2')
plt.axis('off')
fshift = np.fft.fftn(f)
fimg = np.log(np.abs(fshift))  # fft结果是复数, 其绝对值结果是振幅
plt.subplot(234), plt.imshow(fimg, 'gray'), plt.title('fftn')
plt.axis('off')
fshift = np.fft.fftshift(f)
fimg = np.log(np.abs(fshift))  # fft结果是复数, 其绝对值结果是振幅
plt.subplot(235), plt.imshow(fimg, 'gray'), plt.title('fftshift')
plt.axis('off')
plt.show()
```
运行脚本后，即可在弹出的窗口中看到结果

另外，有条件的话也可以使用Mathematica的脚本：
```matlab
ftshift[dat_?ArrayQ, k : (_Integer?Positive | All) : All] :=  Module[{dims = Dimensions[dat]}, RotateRight[dat, If[k === All, Quotient[dims, 2],  Quotient[dims[[k]], 2] UnitVector[Length[dims], k]]]]
ifftshift[dat_?ArrayQ, k : (_Integer?Positive | All) : All] :=  Module[{dims = Dimensions[dat]}, RotateRight[dat, If[k === All, Ceiling[dims/2], Ceiling[dims[[k]]/2] UnitVector[Length[dims], k]]]]
cam =; (*图像数据*)
tst = Abs[fftshift[Fourier[ImageData[cam], FourierParameters -> {1, -1}]]]^2;
Image[Log[1 + tst]] // ImageAdjust;
res = ImagePeriodogram[cam];
{w, h} = ImageDimensions[res];
pic1 = ImageTake[res, {0, h/2}, {0, w/2}];
pic2 = ImageTake[res, {0, h/2}, {w/2, w}];
pic3 = ImageTake[res, {h/2, h}, {0, w/2}];
pic4 = ImageTake[res, {h/2, h}, {w/2, w}];
res = ImageCollage[{pic4, pic3, pic2, pic1}];
FillingTransform[res];

```

以上所展示的脚本，都可用来展示图像的灰度分布

相关的检测和理论、轮子，可以参考以下文章：

> Opencv实现盲水印技术—傅里叶变换算法及盲水印实现：[有意思的数字盲水印的简单的实现](https://www.cnblogs.com/Imageshop/p/10097854.html)
>