---
title: Android-入门
date: 2021-06-02 19:26:53
categories: Android
tags: [Android,Java,backend]
---

## 前言

最近快期末考试了,准备把一直欠着没写的数据结构课设写了去,准备写一个基于Android的故宫导游系统,所以来学习了一下Android开发,希望自己能把这个课设写好,也算是多学一门知识

## 1. 基础概念

SDK:

Software Development Kit,软件开发工具包

NDK:

Native Development kit, Android原生工具开发包

以上是我们Android开发需要使用的两个开发包,NDK是基于C/C++的包,一般使用不上,但是在性能有要求的时候将会使用到,一般的小程序使用不到

SDK是Android开发中,Java为我们提供好的一些开发工具,是我们最常用的一些工具包的集合

## 2. Android系统架构

![Android 架构](https://gitee.com/cao_ziqiang/img/raw/master/20210602201724.jpeg)

应用层(Applications)	->	应用框架层(Application Framework)	->	系统库层(Libraries)/系统运行时(Android Runtime)	->		Linux内核(Linux Kenel)

### 2.1 Linux内核

在所有层的最底下是 Linux - 包括大约115个补丁的 Linux 3.6。它提供了基本的系统功能，比如进程管理，内存管理，设备管理（如摄像头，键盘，显示器）。同时，内核处理所有 Linux 所擅长的工作，如网络和大量的设备驱动，从而避免兼容大量外围硬件接口带来的不便。

<hr/>

### 2.2 程序库

在 Linux 内核层的上面是一系列程序库的集合，包括开源的 Web 浏览器引擎 Webkit ，知名的 libc 库，用于仓库存储和应用数据共享的 SQLite 数据库，用于播放、录制音视频的库，用于网络安全的 SSL 库等。

<hr/>

### 2.3 Android程序库

这个类别包括了专门为 Android 开发的基于 Java 的程序库。这个类别程序库的示例包括应用程序框架库，如用户界面构建，图形绘制和数据库访问。一些 Android 开发者可用的 Android 核心程序库总结如下：

- android.app - 提供应用程序模型的访问，是所有 Android 应用程序的基石。
- android.content - 方便应用程序之间，应用程序组件之间的内容访问，发布，消息传递。
- android.database - 用于访问内容提供者发布的数据，包含 SQLite 数据库管理类。
- android.opengl - OpenGL ES 3D 图片渲染 API 的 Java 接口。
- android.os - 提供应用程序访问标注操作系统服务的能力，包括消息，系统服务和进程间通信。
- android.text - 在设备显示上渲染和操作文本。
- android.view - 应用程序用户界面的基础构建块。
- android.widget - 丰富的预置用户界面组件集合，包括按钮，标签，列表，布局管理，单选按钮等。
- android.webkit - 一系列类的集合，允许为应用程序提供内建的 Web 浏览能力。

看过了 Android 运行层内的基于 Java 的核心程序库，是时候关注一下 Android 软件栈中的基于 C/C++ 的程序库。

<hr/>

### 2.4 Android运行时

这是架构中的第三部分，自下而上的第二层。这个部分提供名为 Dalvik 虚拟机的关键组件，类似于 Java 虚拟机，但专门为 Android 设计和优化。

Dalvik 虚拟机使得可以在 Java 中使用 Linux 核心功能，如内存管理和多线程。Dalvik 虚拟机使得每一个 Android 应用程序运行在自己独立的虚拟机进程。

Android 运行时同时提供一系列核心的库来为 Android 应用程序开发者使用标准的 Java 语言来编写 Android 应用程序。

------

### 2.5 应用框架

应用框架层以 Java 类的形式为应用程序提供许多高级的服务。应用程序开发者被允许在应用中使用这些服务。

- 活动管理者 - 控制应用程序生命周期和活动栈的所有方面。
- 内容提供者 - 允许应用程序之间发布和分享数据。
- 资源管理器 - 提供对非代码嵌入资源的访问，如字符串，颜色设置和用户界面布局。
- 通知管理器 - 允许应用程序显示对话框或者通知给用户。
- 视图系统 - 一个可扩展的视图集合，用于创建应用程序用户界面。

------

### 2.6 应用程序

顶层中有所有的 Android 应用程序。你写的应用程序也将被安装在这层。这些应用程序包括通讯录，浏览器，游戏等。

## 3. Android版本

![image-20210602193518839](https://gitee.com/cao_ziqiang/img/raw/master/20210602193519.png)

从C到O,每一个Android版本都会使用一个甜品来命名,在开发过程中,我们一般使用Android4版本开发,并不是版本越高越好,而是追求稳定性和市场占有率

## 4. 开发工具

在开发Andrioid时,我们更多的会使用Android Studio开发工具,这是一个基于idea改造的开发工具,个人觉得由谷歌来维护的,应该也不会难用到哪里去,所以我们跟着大流走就行,这里我去google官网没有下载成功,给大家推荐Andoird Studio的中文社区:<a href="https://www.androiddevtools.cn/index.html" style="color:red;text-decoration:none">AndroidDevTools - Android开发工具 Android SDK下载 Android Studio下载 Gradle下载 SDK Tools下载</a>

安装也是很简单的,一直next就行,如果安装到最后提示没有SDK,先cancel掉,然后后面这个IDE会帮我们下载好对应的SDK,所以我们不用担心。

## 5. 组件

应用程序组件是一个Android应用程序的基本构建块。这些组件由应用清单文件松耦合的组织。AndroidManifest.xml描述了应用程序的每个组件，以及他们如何交互。

以下是可以在Android应用程序中使用的四个主要组件。

| 组件                | 描述                                      |
| :------------------ | :---------------------------------------- |
| Activities          | 描述UI，并且处理用户与机器屏幕的交互。    |
| Services            | 处理与应用程序关联的后台操作。            |
| Broadcast Receivers | 处理Android操作系统和应用程序之间的通信。 |
| Content Providers   | 处理数据和数据库管理方面的问题。          |

### 5.1 Activities

一个活动标识一个具有用户界面的单一屏幕。举个例子，一个邮件应用程序可以包含一个活动用于显示新邮件列表，另一个活动用来编写邮件，再一个活动来阅读邮件。当应用程序拥有多于一个活动，其中的一个会被标记为当应用程序启动的时候显示。

一个活动是**Activity**类的一个子类，如下所示：

```java
public class MainActivity extends Activity {

}
```

### 5.2 Services

服务是运行在后台，执行长时间操作的组件。举个例子，服务可以是用户在使用不同的程序时在后台播放音乐，或者在活动中通过网络获取数据但不阻塞用户交互。

一个服务是**Service**类的子类，如下所示：

```java
public class MyService extends Service {

}
```

### 5.3 Broadcast Receivers

广播接收器简单地响应从其他应用程序或者系统发来的广播消息。举个例子，应用程序可以发起广播来让其他应用程序知道一些数据已经被下载到设备，并且可以供他们使用。因此广播接收器会拦截这些通信并采取适当的行动。

广播接收器是**BroadcastReceiver**类的一个子类，每个消息以**Intent**对象的形式来广播。

```java
public class MyReceiver  extends  BroadcastReceiver {

}
```

### 5.4 Content Providers

内容提供者组件通过请求从一个应用程序到另一个应用程序提供数据。这些请求由**ContentResolver**类的方法来处理。这些数据可以是存储在文件系统、数据库或者其他其他地方。

内容提供者是**ContentProvider**类的子类，并实现一套标准的API，以便其他应用程序来执行事务。

```java
public class MyContentProvider extends  ContentProvider {

}
```

### 5.5 附件组件

有一些附件的组件用于以上提到的实体、他们之间逻辑、及他们之间连线的构造。这些组件如下：

| 组件      | 描述                                             |
| :-------- | :----------------------------------------------- |
| Fragments | 代表活动中的一个行为或者一部分用户界面。         |
| Views     | 绘制在屏幕上的UI元素，包括按钮，列表等。         |
| Layouts   | 控制屏幕格式，展示视图外观的View的继承。         |
| Intents   | 组件间的消息连线。                               |
| Resources | 外部元素，例如字符串资源、常量资源及图片资源等。 |
| Manifest  | 应用程序的配置文件。                             |

## 6. Gradle

Android的主流编译工具,是由google自主开发的工具

在一个项目中,会有一个setting.gradle,build.gradle

setting.gradle会选择哪些模块加入到编译过程,而build.gradle则是全局通用的gradle

minSdkVersion: 最小的API leve

compileSdkVersion: 编译的SDK版本

targetSdkVersion: 目标版本

dependencies: 依赖配置,依赖的库

## 7. 创建模拟器

在2020.3版本中的Android Studio可以在如下图所示位置创建AVD Manager

![image-20210602205341018](https://gitee.com/cao_ziqiang/img/raw/master/20210602205341.png)

在一般开发中我们会选择480*800分辨率的设备进行开发

![image-20210602205537775](https://gitee.com/cao_ziqiang/img/raw/master/20210602205537.png)

在这里我为了向下兼容性,选择了Android6.0版本的API

![image-20210602205705420](https://gitee.com/cao_ziqiang/img/raw/master/20210602205705.png)

下载完成之后如下所示:

![image-20210602205847102](https://gitee.com/cao_ziqiang/img/raw/master/20210602205847.png)