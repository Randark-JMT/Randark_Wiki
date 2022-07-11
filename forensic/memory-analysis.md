---
title: 内存取证
description: 
published: true
date: 2022-07-11T01:28:40.096Z
tags: 
editor: markdown
dateCreated: 2022-07-11T01:28:40.096Z
---

# 内存取证
内存取证是针对靶机的内存镜像进行取证分析，其中最常用到的工具便是`Volatility`，是一个开源的内存取证框架。

针对内存取证，实战中第一步就是针对靶机的内存提取，对Windows来说，可以通过`DumpIt.exe`来实现，而Linux和Macos则需要使用 [lmg](https://github.com/halpomeranz/lmg) 和 dwarfdump 来实现。

成功得到靶机的内存镜像后，就需要先确定内存的数据结构和格式，这在`Volatility`中已经实现简单化操作，只需指定profile就可以套用已有结构进行分析（针对Linux内存镜像，可能需要针对靶机的结构和平台，自己编译一份profile）。目前，`Volatility`有两个版本分支，分别是`Volatility2`（Volatility 2.7）和`Volatility3`，鉴于目前`Volatility3`的功能开发还不是非常完善，故建议先使用`Volatility 2.7`。

`Volatility`的相关使用语法可以参考`-h`中的帮助信息，也可以参考 [狼组安全团队公开知识库-Volatility取证分析工具](https://wiki.wgpsec.org/knowledge/ctf/Volatility.html)

内存取证的核心便是针对进程列表进行分析，并结合题目题干查询敏感数据。