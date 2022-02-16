---
title: Ubuntu复习笔记-认识Linux
categories: Ubuntu
tags:
  - Ubuntu
  - Linux
  - backend
hidden: true
abbrlink: f289a2e9
date: 2022-01-01 20:10:44
---

本次复习基于$Ubuntu20.04$的发行版进行总结，目的是更好记录自己学习的$Linux$。

## 认识Linux

学习$Linux$之前，需要搞懂几个概念，$Linux$桌面操作系统与$Linux$。事实上，前者指具体的某种操作系统，后者指一种**开放源代码**的操作系统内核，普通用户是无法直接使用的，一些商业公司和社区组织将$Linux$内核，其他系统软件以及相关的应用软件集合，产生了发行版。

### 简介

Linux以$POSIX$(可移植性操作系统接口)标准为框架，支持多用户，多任务，多线程和多处理器。它继承了UNIX以网络为核心的设计思想，是一种性能稳定，安全性高的多用户网络操作系统。

#### 常用的Linux发行版

CentOS；

Debian；

Fedora；

Red Hat；

SuSE;

Ubuntu；

#### 内核

进程调度

控制进程对CPU的访问。到选择不同进程在CPU上运行时，由调度算法选择相应进程。

内存管理

管理整个系统的物理内存，同时快速响应内核各子系统对内存分配的请求，允许多个进程安全地共享主内存区域。

虚拟文件系统

虚拟文件系统隐藏了各种不同硬件的具体细节，从而为所有的设备提供了统一的接口。

网络接口

网络接口提供了对各种网络硬件和各种网络标准的支持。网络接口包含网络协议和网络设备驱动程序。

### Ubuntu

Ubuntu十分注重系统的安全性与可用性，与登录系统管理员账号进行管理的方式相比，Ubuntu所有系统相关的任务均采用Sudo工具，并且需要输入密码。

Ubuntu的衍生版又Kubuntu，Edubuntu，Xubuntu和Ubuntu Server  Edition。

Kubuntu采用KDE作为默认桌面环境，更加美观；

Edubuntu是Ubuntu的教育发行版，适合学习；

Xubuntu使用Xfce4作为默认桌面环境；

Ubuntu Server Edition提供了服务器应用程序，如邮箱服务器，LAMP等；

还有如专注于安全工具的$nUbuntu$，为旧电脑设计的$Ubuntu$ $Lite$，$zUbuntu$，$Fluxbuntu$。

Ubuntu采用$dpkg$进行软件包管理，分为四类，$main$组件，$restricted$组件，$universe$组件，$multiverse$组件。

$main$组件只包含符合Ubuntu许可证要求,并且可从Ubuntu团队中获得支持的软件包；

$restricted$组件无法获取源码；

$universe$组件是社区维护，不为Ubutu团队支持；

$multiverse$组件包含了不符合自由软件要求且不被Ubuntu团队支持的软件包；

Ubuntu中个目录的结构：

| 目录名        | 备注                                   |
| ------------- | -------------------------------------- |
| $/$           | $Linux$系统根目录                      |
| $/bin$        | 放置可执行文件                         |
| $/boot$       | 存放开机所需文件，如内核和系统启动文件 |
| $cdrom$       | 挂载光驱文件系统                       |
| $/dev$        | 存放所有设备文件                       |
| $/etc$        | 存放系统所有配置文件                   |
| $/home$       | 用户主目录的默认位置                   |
| $/lib$        | 存放开机时所需要的函数库               |
| $/lost+found$ | 存放由$fsck$放置的零散文件             |
| $/media$      | 存放可删除的设备                       |
| $/mnt$        | 存放暂时挂载额外的设备                 |
| $/opt$        | 可选文件和程序的存放目录               |
| $/proc$       | 虚拟文件系统，系统内存的映射           |
| $/root$       | $root$用户的主目录                     |
| $/sbin$       | 设置系统的可执行命令                   |
| $/selinux$    | 伪文件系统                             |
| $/srv$        | 存放网络服务启动后的数据目录           |
| $/sys$        | 虚拟文件系统，记录与内核相关的信息     |
| $/tmp$        | 存放临时文件                           |
| $/usr$        | 包含所有的命令，说明文件，程序库       |
| $/var$        | 包含日志文件，计划任务                 |



