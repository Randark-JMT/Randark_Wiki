---
title: 内存取证
description: 
published: true
date: 2022-07-12T13:32:24.846Z
tags: 
editor: markdown
dateCreated: 2022-07-11T01:28:40.096Z
---

# 内存取证
内存取证是针对靶机的内存镜像进行取证分析，其中最常用到的工具便是`Volatility`，是一个开源的内存取证框架。内存取证的核心便是针对进程列表进行分析，并结合题目题干查询敏感数据。

针对内存取证，实战中第一步就是针对靶机的内存提取，对Windows来说，可以通过`DumpIt.exe`来实现，而Linux和Macos则需要使用 [lmg](https://github.com/halpomeranz/lmg) 和 `dwarfdump` 来实现。而在安全研究和CTF比赛中，内存镜像也有可能是从`VMware`虚拟机快照文件中所提取出来的。虽然文件的结构和取材途径不同，但是其中的内存数据都是一致的，也都可以利用Volatility来进行分析。

成功得到靶机的内存镜像后，就需要先确定内存的数据结构和格式，这在`Volatility`中已经实现简单化操作，只需指定profile就可以套用已有结构进行分析（针对Linux内存镜像，可能需要针对靶机的结构和平台，自己编译一份profile）。目前，`Volatility`有两个版本分支，分别是`Volatility2`（Volatility 2.7）和`Volatility3`。

就目前而言，`Volatility3`在检材数据结构分析自动识别，以及分析速度上。相较于`Volatility2`有了非常巨大的提升，但是对于功能数量和第三方插件数量而言，`Volatility3`还不是很成熟。故建议一般情况下还是使用`Volatility2`来进行内存镜像研究。

`Volatility`的相关使用语法可以参考`-h`中的帮助信息，也可以参考`狼组安全团队公开知识库`中的文章。 

- [获取Volatility](/forensic/memory-analysis/install_volatility)
- [狼组安全团队公开知识库-Volatility取证分析工具](https://wiki.wgpsec.org/knowledge/ctf/Volatility.html)
{.links-list}