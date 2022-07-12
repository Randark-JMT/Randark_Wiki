---
title: 获取Volatility
description: 
published: true
date: 2022-07-12T13:23:06.779Z
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
# 源码运行Volatility2
> 需要注意的是，Volatility2需要Python2环境来运行，也就是极不推荐在Windows平台上通过源码直接运行Volatility2，并且Volatility运行的时候需要依赖一些特殊的第三方库，直接通过源码运行Volatility可能会导致程序报警告性错误。以下操作基于Linux平台。
## 配置环境
首先，检查环境中是否存在Python2语言环境：
```text
randark@randark-virtual-machine:~$ python2
Python 2.7.18 (default, Mar 12 2022, 06:24:29) 
[GCC 11.2.0] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```
如果不存在，请参阅网上教程手动安装。
其次，确认Python2环境中确实存在pip库管理器：
```text
randark@randark-virtual-machine:~$ python2 -m pip --version
pip 20.3.4 from /usr/local/lib/python2.7/dist-packages/pip (python 2.7)
```
如果发现不存在pip，请考虑手动安装pip
- [Python2 安装pip 参考](https://zhuanlan.zhihu.com/p/425737419)
{.links-list}
# 源码运行Volatility3
# 通过源码安装Volatility