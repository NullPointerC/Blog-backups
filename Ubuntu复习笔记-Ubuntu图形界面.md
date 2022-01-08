---
title: Ubuntu复习笔记-Ubuntu图形界面
date: 2022-01-02 20:22:28
categories: Ubuntu
tags: [Ubuntu,Linux,backend]
---

Ubuntu不仅提供了强大的字符界面，而且比较方便的可以定制图形界面。

## 基础操作

### 登录

Ubuntu是一个多用户系统，每次使用前都需要登录，需要输入用户名和密码。

在不关闭终端的前提下，仅需要输入一次$root$用户的密码即可完成身份认证。

### 注销

需要结束当前用户的运行，或者使用另外的账户，就可以注销。

在图形界面下可以在右上角点击注销的图标。

### 关机

在图形化界面右上角有关机选项。

### 重启

许多配置需要重启后才能生效，在右上角也是有重启选项。

## 系统设置

### 显示

若安装了$Tweak$ $Tool$，可以直接比较直观的修改配置。

若没有安装，可以输入如下命令安装即可。

```bash
sudo apt-get install gnome-tweak-tool
```

### 桌面背景

在桌面空白处右击即可修改桌面背景。

### 时间和日期

在System Settings中找到Time & Date进行修改。

### 磁盘管理

Ubuntu将磁盘，光盘和U盘均视为磁盘进行管理。

在Ubuntu中可以使用Disks工具查看磁盘的大小，分区类型，格式和挂载点等信息。

## 应用软件

### 浏览器

在Ubuntu中默认安装了Firefox浏览器。

### 办公应用

Ubuntu中默认安装了OpenOffice，包括了文字处理软件，电子表格软件和文稿演示软件。

### 图像处理

GIMP是一款Linux桌面环境的图像处理软件。

软件包添加

```bash
sudo add-apt-repository ppa:otto-kesselgulasch/gimp
```

软件包升级

```bash
sudo apt-get update
```

软件安装

```bash
sudo apt-get install gimp
```

### 通信

Pidgin是一款开源的即时通信，可以使用软件中心安装。

### 音频播放

Ubuntu中默认安装了Rhythmbox作为音乐播放器和管理软件。默认播放$Ogg$格式的音频文件。

### 视频播放

Totem是一款基于Gnome环境的媒体播放器，并且按照对应视频的解码器进行播放。

## 程序安装

### 添加和删除

Ubuntu在20.04之前都是自带了Ubuntu Software管理软件进行软件管理。在20.04后引入了snap Software管理软件。软件商店是图形化界面，不涉及命令。

### 软件包管理

Ubuntu对于软件包则是采用Debian的软件包管理机制，分为二进制包（Deb）和源码包（Deb-src）。二进制包由可执行文件，库文件，配置等组成，源码包包括源码和编译工具等。

软件包工具分三类，分别是命令行，文本窗口以及图形界面。

常用的dpgk和apt均为命令行工具，

dpgk常用用法

| 选项          | 含义                         |
| ------------- | ---------------------------- |
| -i            | 安装软件                     |
| -R            | 安装一个目录下面所有的软件包 |
| -unpack       | 释放软件包但不配置           |
| -configure    | 重新配置和释放软件包         |
| -r            | 删除软件包                   |
| -update-avail | 替代软件包                   |

篇幅有限，还有其他用法可使用-help选项进行查看。

apt的常用工具由apt-get,apt-cache,apt-cdrom等。

| 选项               | 含义       |
| ------------------ | ---------- |
| apt-cache search   | 搜索包     |
| apt-get install    | 安装包     |
| apt-get reinstall  | 重新安装包 |
| apt-get -f install | 修复安装   |
| apt-get remove     | 删除包     |
| apt-get update     | 更新源     |

### Ubuntu软件库

查看软件库：

```bash
cat /etc/apt/sources.list
```

软件安装：

```bash
sudo apt-get install + 软件名称
```

软件卸载

```bash
sudo apt-get remove + 软件名称
```

添加软件源

修改$sources.list$文件即可。

更新：

```bash
sudo apt-get update
```

