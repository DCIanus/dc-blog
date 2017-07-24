---
title: 【备忘】Linux下USB无线网卡经常自动休眠以致断线的解决方案
date: 2017-07-02 01:50:04
urlname: avoid_automatic_dormancy_of_wireless_lan_under_linux
tags:
    - Linux
---

**本文没有任何技术含量**

最近组装了个台式机，本想着台式机当开发机装Linux，但是后来忍不住买了张1050TI显卡，虽然性能并不算强，但是相比笔记本的960M显卡还是要强很多，于是台式机装了Windows玩游戏。
<!--more-->
自己是个版本控，生产项目就算了，但是个人娱乐项目里不管是依赖库还是开发工具一定要尽量使用最新版，这种需求下，Arch这种积极更新的发行版就非常对我的胃口了，各种工具直接用官方的包管理器就可以直接安装最新版，而不必自己手动编译（说的就是你，Centos）。

按照[viseator同学的教程非常顺利的安装了Arch](http://www.viseator.com/2017/05/17/arch_install/)，笔记本自带网卡太差，即使是同一个路由器下，台式机ping笔记本网络延迟也高达50ms，SSH连接是停滞感严重。

在京东购买了COMFAST CF-WU810N USB无线网卡，成功将网络延迟缩短到10ms以内，但是随后又出现了新的问题：SSH模式下偶发性无响应几秒后才可继续操作，严重时甚至SSH连接超时中断。经过仔细排查，最终确定原因是无线网卡的自动休眠机制。

搜索后发现其他人也有[类似情况](http://m.blog.csdn.net/ferstar/article/details/51093696)出现，但是对方与我无线网卡芯片不同，不可直接使用，使用lsusb命令结果如下
```bash
➜  ~ lsusb
Bus 002 Device 002: ID 8087:8000 Intel Corp.
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 8087:8008 Intel Corp.
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 003: ID 05e3:0745 Genesys Logic, Inc. Logilink CR0012
Bus 003 Device 002: ID 8087:07dc Intel Corp.
Bus 003 Device 006: ID 0bda:8179 Realtek Semiconductor Corp. RTL8188EUS 802.11n Wireless Network Adapter
Bus 003 Device 005: ID 0b95:772b ASIX Electronics Corp. AX88772B
Bus 003 Device 004: ID 05e3:0608 Genesys Logic, Inc. Hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```
以下行对应我的无线网卡，其中`RTL8188EUS`为其无线网卡芯片型号
        Bus 003 Device 006: ID 0bda:8179 Realtek Semiconductor Corp. RTL8188EUS 802.11n Wireless Network Adapter

编辑`/etc/modprobe.d/8188eus.conf`文件，其中内容为:
```conf
# 加点参数, 禁用自动休眠
options 8188eus rtw_power_mgnt=0
```
成功搞定！

# 题外话:

这款无线网卡在工作时会有蓝色小灯亮起，SSH时每敲一个字符就会发送一个数据包，小灯就会亮一下，我玩这个玩了半天233333