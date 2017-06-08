---
title: 【备忘】避免SSH长时间不操作而卡死
date: 2017-06-08 12:32:16
urlname: avoid_ssh_stuck_because_of_long_time_no_operation
tags:
    - Linux
---

    SSH日常使用过程中经常出现的一个问题是：过长时间没有数据传输后SSH呈现卡死的状态
    这是因为长时间没有数据传输的情况下，网络中间节点可能会强行断开连接
    而SSH终端与服务器并不知情，于是此时操作就会出现卡死的现象。

<!--more-->

解决方法很简单，设置SSH服务器或客户端的心跳包功能即可，以下两个方案任选其一即可

1. 客户端：编辑`/etc/ssh/ssh_config`，增加一行配置`ServerAliveInterval 60`

2. 服务端：编辑`/etc/ssh/sshd_config`，增加一行配置`ClientAliveInterval 60`后重启SSH服务即可

以上两个方案均是通过让客户端或服务器主动发送心跳包以避免被强行断开连接，故每个连接只需要任意一端配置即可

**参考资料**:

<a href="http://linux-wiki.cn/wiki/zh-hans/%E9%81%BF%E5%85%8DSSH%E8%BF%9E%E6%8E%A5%E5%9B%A0%E8%B6%85%E6%97%B6%E9%97%B2%E7%BD%AE%E6%96%AD%E5%BC%80"  rel="nofollow">http://linux-wiki.cn/wiki/zh-hans/避免SSH连接因超时闲置断开</a>