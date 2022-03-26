---
title: Django学习小记-1-简介
categories:
  - Django
tags:
  - Python
  - backend
  - Django
abbrlink: 65dcdaa8
date: 2021-08-07 20:12:34
---

>本系列用于记录本人的Django学习之旅

现如今软件工程的发展迅速，有时也需要快速迭代出产品。若此时使用Java系的产品虽稳定且解决方案众多，但是开发效率上往往不尽如人意，虽然Python系产品的性能为人所诟病，但是在产品的快速迭代初期，可以使用Python系产品，后期再进行二次开发即可。

## 一、Django初识

Django最初是被开发用来管理劳伦斯出版集团旗下的一些以新闻内容为主的网站的。

### 1.1 Django的版本发布过程

Django遵守BSD（Berkeley Software Distribution，伯克利软件发行版）版权，初次发布于2005年7月，并于2008年9月发布了第一个正式版本1.0。从正式版1.0之后，Django的版本发布过程如下。

（1）功能版本：版本号定义为A.B、A.B+1等，大约每8个月发布一次，每个版本均包括新功能以及对现有功能的改进。

（2）补丁版本：版本号定义为A.B.C、A.B.C+1等，用来修复bug或者是安全问题，补丁版本是100%兼容相关的功能版本的，所以，除了由于安全问题或者是可能造成数据丢失的情况之外，都应该升级到最新的补丁版本。

（3）LTS版本：即长期支持的版本，某些功能版本会被指定为LTS版本，如1.8LTS版本，这类版本的安全更新时长至少需要3年。

### 1.2 MTV模式

MVC设计模式：数据存取逻辑、业务逻辑和表现逻辑。

Model代表的是数据存取层，是对数据实体的定义和对数据的增删改查操作；

View代表的是视图层，即系统中选择显示什么和怎么显示的部分；

Controller代表的是控制层，它负责根据从View中输入的指令检索Model中的数据，再以一定的逻辑产生最终的结果输出。

![img](http://static.codenote.xyz/img/20210817205439.jpeg)

MVC的3层之间紧密相连，但是又相互独立。每一层的修改都不会影响其他层，每一层都提供了各自独立的接口供其他层调用，这种模块化的开发极大地降低了代码之间的耦合，也增加了模块的可重用性。

在Django框架的设计模式借鉴了经典的MVC思想，将交互过程分成了3层，主要目的是降低各个模块之间的耦合。

（1）M（Model）：数据存取层，这一层处理所有与数据相关的事务，提供在数据库中管理（添加、修改、删除）和查询记录的机制。

（2）T（Template）：表现层，处理页面的显示，即所有与表现相关的决定都由这一层去处理。

（3）V（View）：业务逻辑层，负责处理业务逻辑，会在适当的时候将Model与Template组合在一起，通常被认为是联通M与T的桥梁。

从概念上可以看出，Django也是一个MVC框架。但在Django中，C（Controller）是由框架自行处理的，它由框架的URLConf来实现，其机制是使用正则表达式匹配URL，再去调用合适的Python函数。

![img](http://static.codenote.xyz/img/20210817205714.jpeg)

### 1.3 功能模块

1. ORM。Django为ORM提供了强大的支持，适配了多种常用的数据库，如PostgreSQL、MySQL、Oracle等。Django把表模型定义为Model，其定义过程非常简单，只需要继承自django.db.models中的Model类就可以了，Model类中的每一个属性都能映射到表的对应字段。

2. 用户模块与权限系统。Django的用户模块定义在auth应用中。Django还提供了一些与登录、注销、权限验证等功能相关的函数与装饰方法。

3. Admin后台管理系统。Django提供了Admin Web后台管理系统。

4. 视图。Django视图是MTV设计模式中的V，它在Django中的体现是一个Python函数或者类，接收Web请求并返回Web响应。

5. 模板系统。模板系统用于将页面设计的HTML代码和用于逻辑处理的Python代码分离开来。

6. 表单系统Form。使用Django提供的表单系统可以获取用户提交的表单项。

7. 信号机制。发布-订阅模式，当系统中有event（事件）发生，一组senders（发送者）将signals（信号）发送给一组receivers（接收者），receivers再去执行预定的操作。

8. 路由系统。Django利用URLconf构建起URL模式与视图函数之间的映射关系，即利用Django的特定配置方式，设定好哪个URL可以去执行哪一段Python代码。

9. 中间件。中间件是一个插件系统，嵌入在Django的Request和Response之间执行，可以对输入和输出内容做出修改。中间件是业务无关的技术类组件，是用来定义处理所有请求和响应的通用处理架构。

	![img](http://static.codenote.xyz/img/20210817210207.jpeg)

10. 缓存系统。Django提供一个稳健的缓存系统，实现了不同级别的缓存粒度：可以缓存单个视图的结果输出，缓存难以生成的片段，或者是缓存整个网站。

## 二、虚拟环境

### 2.1 pip常用指令

```python
（1）安装package：pip install<package>。
（2）升级package：pip install–U<package>。
（3）卸载package：pip uninstall<package>。
（4）列出已经安装的package：pip list。
```

### 2.2 虚拟环境的安装与配置

在做应用程序开发的时候不可避免地会遇到不同的应用程序依赖不同的包版本的情况。这样就有可能会出现依赖冲突的问题，特别是时间长了之后，这种情况会变得更加难以处理。为了解决这个问题，需要安装Virtualenv，利用它可以给每一个应用搭建虚拟且独立的Python运行环境。

安装虚拟环境比较简单，需要利用Python的包管理工具，执行命令：

```python
pip install virtualenv
```

创建应用运行的虚拟环境,如下创建名为xxxx的虚拟环境

```python
virtualenv xxxx
```

激活、退出虚拟环境

首先需要进入到虚拟环境的Scirpt目录下

```python
# 激活
source activate
# 退出
deactivate
```

### 2.3 django脚手架

Django提供了django-admin这个功能强大的命令行管理工具，其中最重要的就是可以利用它来完成项目的创建。

```python
django-admin startproject 项目名称
```

Django对容器的名字是不敏感的，用户可以在创建之后再修改成自己感兴趣的名称。容器目录如（my_bbs）的内部结构如下：

![img](http://static.codenote.xyz/img/20210817211108.jpeg)

（1）内层的my_bbs是项目中的Python包名称，即导入Python包所使用的名称。

（2）__init__.py文件用于标识当前所在的目录是一个Python包。

（3）settings.py是Django项目的配置文件。

（4）urls.py文件用于记录Django项目的URL映射关系。

（5）wsgi.py是WSGI服务器程序的入口文件，用于启动应用程序。

（6）manage.py是用于管理Django项目的命令行工具。

启动项目

```python
python manage.py runserver
```

输入localhost:8000

![image-20210817211325768](http://static.codenote.xyz/img/20210817211325.png)

### 2.4 文件配置项解析

django-admin在创建项目的时候会生成settings.py文件，项目的所有配置都可以在这个文件中完成。Django定义了一些默认的配置，后期可以根据应用的需要进行修改。

（1）BASE_DIR

```python
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
```

__file__是Python语言的语法，显示当前文件的位置；os.path.abspath(__file__)返回当前文件的绝对路径；

os.path.dirname(os.path.abspath(__file__))得到当前文件所在的目录；

os.path.dirname(os.path.dirname(os.path.abspath(__file__)))返回目录的上一级，对应于当前的项目，BASE_DIR定义的是项目所在的完整路径。

（2）SECRET_KEY

这个变量本质上是一个加密盐，用于对各种需要加密的数据做Hash处理，例如密码重置、表单提交、session数据等。所以，一定要保证这个值不被泄露，否则，恶意用户可以通过反序列获得原始数据，给系统增加安全风险。通常的做法是将它存储到系统环境变量中，通过os.getenv(key,default=None)的方式去获取。

（3）DEBUG通常在开发环境中将它设置为True，项目在运行的过程中会暴露出一些出错信息和配置信息以方便调试。但是在线上环境中应该修改其为False，避免敏感信息泄露。

（4）ALLOWED_HOSTS用于配置可以访问当前站点的域名，当DEBUG配置为False时，它是一个必填项，设置ALLOWED_HOSTS=['*']允许所有的域名访问。

（5）INSTALLED_APPS这个参数配置的是当前项目需要加载的App包路径列表。Django默认会把admin（管理后台）、auth（权限系统）、sessions（会话系统）加入进去，可以根据项目的需要对其增加或删除配置。

（6）MIDDLEWARE当前项目中需要加载的中间件列表配置。与INSTALLED_APPS变量类似，Django也会默认加入一些中间件，例如用于处理会话的SessionMiddleware、用于处理授权验证的AuthenticationMiddleware等。同样，可以根据项目的需要对其增加或删除配置。

（7）ROOT_URLCONF这个变量标记的是当前项目的根URL配置，是Django路由系统的入口点。

（8）TEMPLATES这是一个列表变量，用于项目的模板配置，列表中的每一个元素都是一个字典，每个字典代表一个模板引擎。Django默认会配置自带的DTL（DjangoTemplates）模板引擎。

（9）WSGI_APPLICATIONDjango的内置服务器将使用的WSGI应用程序对象的完整Python路径。

（10）DATABASES这是一个字典变量，标识项目的数据库配置，Django默认会使用自带的数据库sqlite3，同时，Django项目支持多数据库配置，如果需要，可以配置多个键值对。在实际的项目开发中会使用功能更强大的数据库（如MySQL），所以，这个变量通常会被改动。

（11）AUTH_PASSWORD_VALIDATORS Django默认提供了一些支持插拔的密码验证器，且可以一次性配置多个。其主要目的是避免直接通过用户的弱密码配置申请。

（12）LANGUAGE_CODE和TIME_ZONE这两个变量分别代表项目的语言环境和时区。

（13）USE_I18N和USE_L10N Web服务搭建完成之后，可以面向不同国家的用户提供服务，这就要求应用支持国际化和本地化。这两个布尔类型的变量标识当前的项目是否需要开启国际化和本地化功能。I18N是国际化的意思，名字的由来是“国际化”的英文单词Internationalization开头和结尾的字母分别是I和N，且I和N的中间有18个字母，简称I18N。L10N是本地化的意思，名字的由来是“本地化”的英文单词Localization开头和结尾的字母分别是L和N，且L和N的中间有10个字母，简称L10N。

（14）USE_TZ标识对于时区的处理，如果设置为True，不论TIME_ZONE设置的是什么，存储到数据库中的时间都是UTC时间。

（15）STATIC_URL用于标记当前项目中静态资源的存放位置。