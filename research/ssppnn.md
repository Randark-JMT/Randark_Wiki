---
title: ssppnn
description: 
published: true
date: 2022-07-22T02:49:27.895Z
tags: 
editor: markdown
dateCreated: 2022-07-21T11:29:06.781Z
---

# 大致信息
直接通过`wireshark`打开，发现数据包高达179002个，用`wireshark`手动分析已经是不可能的了：
![cd40943c-addd-4858-9f29-960787ca074a.png](/cd40943c-addd-4858-9f29-960787ca074a.png)

于是使用`科来网络分析系统辅助分析`：
![a515ef22-57e2-4b96-92a4-5ba2b5e5e1fd.png](/a515ef22-57e2-4b96-92a4-5ba2b5e5e1fd.png)

并在http流量中发现了疑似攻击流量：
![9d123bb1-535d-4f6d-8195-16749f65e7dc.png](/9d123bb1-535d-4f6d-8195-16749f65e7dc.png)

# 攻击流程梳理
~~TODO~~
