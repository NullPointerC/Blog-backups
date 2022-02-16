---
title: Go语言学习-1.语言介绍
categories:
  - Go
tags:
  - backend
  - Go
abbrlink: 9382eb68
date: 2021-03-29 12:53:02
---

## 一.Go语言的前世今生

### 1.Go语言的来源

Go语言是一门新生的语言，从出现就备受大家的喜爱。

Go语言是Google在2007年开发的一种开源编程语言，出自Robert Griesemer、Rob Pike、Ken Thompson之手。 这些计算机科学领域的重量级人物设计Go语言的初衷是为了满足Google的需求，设计团队融入了Pascal、Oberon和C语言的设计思想。

- Ken Thompson于20世界70年代实现了最初的UNIX操作系统
- Rob Pike与Ken Thompson设计了UTF-8编码方案

2009年Google Open Source Blog向全球发布了这款语言。

Go语言的主要目标是兼具Python等动态语句的开发速度和C/C++等编译型语言的性能与安全性，是一种静态型、编译型并自带垃圾回收和并发的编程语言。



### 2.Go语言开发的项目

Go是一门为云计算而生的编程语言。包括亚马逊、苹果、迪士尼、脸书、通用电气、谷歌、微软在内的公司都使用Go来开发重要的项目。

#### 1.Docker项目

网址：[github.com/docker/dock…](https://github.com/docker/docker)

Docker是一种操作系统层面的虚拟化技术，可以在操作系统和应用程序之间进行隔离，也可以称之为容器。

#### 2.Golang项目

网址：[github.com/golang/go](https://github.com/golang/go)

Go语言的早期源码是使用C语言和汇编语言写成。从Go1.5版本后，完全使用Go语言自身进行编写。

#### 3.Kubernetes项目

网址：[github.com/kubernetes/…](https://github.com/kubernetes/kubernetes)

该项目是Google公司开发的构建于Docker之上的容器调度服务，用户可以通过Kubernetes集群进行云端容器集群管理。

#### 4.Etcd项目

网址：[github.com/coreos/etcd](https://github.com/coreos/etcd)

该项目是一款分布式、可靠的KV存储系统，可以快速进行云配置。

#### 5.Beego项目

网址：[github.com/astaxie/bee…](https://github.com/astaxie/beego)

该项目是一个类似python的Tornada框架，采用了RESTFul的设计思路，使用Go语言编写的一个极轻量级、高可伸缩性、高性能的Web应用框架。

### 3.Go语言在windows平台的安装

Go可用于FreeBSD、Linux、Windows和macos等操作系统

Go语言的开发包可以在以下站点下载

Go语言官方网站：[golang.google.cn/dl/](https://golang.google.cn/dl/)

![image-20210329125514379](https://i.loli.net/2021/03/29/9PgdZIhuBktq7re.png)

**go1.16.2.src.tar.gz**:源码包，供源码研究，对日常开发的话，不建议下载此包

**go1.16.2.darwin-amd64.pkg**：Mac os平台安装包

**go1.16.2.linux-amd64.tar.gz**：Linux平台安装包

**go1.16.2.windows-amd64.msi**：windows平台安装包

<hr/>

windows安装包一般命名如下：

**go1.16.2.windows-amd64.msi**

- 1.16.2表示Go安装包的版本
- Windows表示这个是一个windows安装包
- Amd64表示匹配的CPU版本，这里表示匹配匹配64位CPU

#### 1.开始安装

![image.png](https://i.loli.net/2021/03/29/ckR782TG9dyApwU.png)

#### 2.设置安装目录

![image.png](https://i.loli.net/2021/03/29/pbtznOVMfqs6Nu5.png)

#### 3.安装过程

![image.png](https://i.loli.net/2021/03/29/1trNQacedujlCw6.png)

![image.png](https://i.loli.net/2021/03/29/5yFwvfg7Dzqcm8E.png)

#### 4.安装目录下产生了几个文件

![image-20210329125718750](https://i.loli.net/2021/03/30/6BuAN2QM3Zb7qyV.png)

**api**:每个版本的api变更差异

**bin**:go源码包编译出来的编译器（go）、文档工具（godoc）、格式化工具（gofmt）

**doc**:英文版的go文档

**lib**：引用的一些库文件

**misc**：其它用途的文件，如Android的编译

**pkg**：windows平台编译好的中间文件

**src**：标准库的源码

**test**：测试用例

#### 5.设置环境变量

![image-20210329130239418](https://i.loli.net/2021/03/29/73Jf6weU5GVYBzH.png)

- 在环境变量中添加变量名“**GOPATH**”,
- 变量值为Go安装路径下的**bin**文件路径，

#### 6.看看go是否配置安装成功

调用命令提示符，输入：

**go version**

看看go是否配置安装成功

![image-20210329130343239](https://i.loli.net/2021/03/29/uGQADONwXH78Jey.png)

到此,go语言开发环境就配置好了!

### 4.Go语言第一个示例

```go
package main
import "fmt"
func main() {
	fmt.Println("Hello world")
}
```

![image-20210329134845342](https://i.loli.net/2021/03/29/ogfH5l1rpGYQmcJ.png)