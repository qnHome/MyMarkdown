---
title: 解决manjaro安装TeamViewer后显示：未就绪，请检查您的连接(备忘)
date: 2018-08-22 11:08:38
tags:
- Linux
- Manjaro
- TeamViewer
categories: Linux
description: 本文记录如何解决manjaroLinux安装TeamViewer后无法连接网络的问题
---
很简单，就一条命令，不用修改DNS
```
sudo teamviewer --daemon enable
```
