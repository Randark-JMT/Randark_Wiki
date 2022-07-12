---
title: 获取Volatility
description: 
published: true
date: 2022-07-12T13:14:10.561Z
tags: 
editor: markdown
dateCreated: 2022-07-12T13:04:06.045Z
---

Volatility在多个平台上都提供了二进制编译版本，同时也支持直接运行python源码。
# 直接运行二进制编译版本
在官网的`download`页面中，就有Volatility各个版本的下载链接。同时在Github上面的官方仓库中，Release上也有所有的历史版本。
- [Release Downloads *官网下载页面*](https://www.volatilityfoundation.org/releases)
- [volatilityfoundation/volatility  *vol2 - Github*](https://github.com/volatilityfoundation/volatility/tags)
- [volatilityfoundation/volatility3 *vol2 - Github*](https://github.com/volatilityfoundation/volatility3/releases)
{.links-list}
# 直接通过源码运行
> 需要注意的是，Volatility2需要Python2环境来运行，也就是极不推荐在Windows平台上通过源码直接运行Volatility2，并且Volatility运行的时候需要依赖一些特殊的第三方库，直接通过源码运行Volatility可能会导致程序报警告性错误
## Volatility2
### 配置环境