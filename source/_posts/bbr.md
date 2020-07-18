---
title: 一键安装最新内核并开启 BBR 脚本
date: 2018-06-27 14:16:24
tags: 
    - bbr
    - linux
categories: 
    - linux
---

> 本脚本适用环境

* 系统支持：CentOS 6+，Debian 7+，Ubuntu 12+
* 虚拟技术：OpenVZ 以外的，比如 KVM、Xen、VMware 等
* 内存要求：≥128M
* **日期 : 2018 年 06 月 09 日**

> 使用root用户登录，运行以下命令

``` bash
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh
```

安装完成后，脚本会提示需要重启 VPS，输入 y 并回车后重启

> 验证

``` bash
uname -r
```

查看内核是否为你刚刚选择版本

``` bash
sysctl net.ipv4.tcp_available_congestion_control
```

返回值一般为：
net.ipv4.tcp_available_congestion_control = bbr cubic reno
或者为：
net.ipv4.tcp_available_congestion_control = reno cubic bbr

``` bash
sysctl net.ipv4.tcp_congestion_control
```

返回值一般为：
net.ipv4.tcp_congestion_control = bbr

``` bash
sysctl net.core.default_qdisc
```

返回值一般为：
net.core.default_qdisc = fq

``` bash
lsmod | grep bbr
```

返回值有 tcp_bbr 模块即说明 bbr 已启动。
注意：并不是所有的 VPS 都会有此返回值，若没有也属正常。

> 特别说明

如果你使用的是 Google Cloud Platform （GCP）更换内核，有时会遇到重启后，整个磁盘变为只读的情况。只需执行以下命令即可恢复

``` bash
mount -o remount rw /
```

> 转载自

# [秋水逸冰](https://teddysun.com/489.html)