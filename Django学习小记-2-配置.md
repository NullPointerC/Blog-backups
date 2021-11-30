---
title: Django学习小记-2-配置
categories: [Django]
tags:
  - Python
  - backend
  - Django
date: 2021-08-09 21:27:01
---

## 一、项目配置

### 1.1 配置语言环境和时区

在第一次启动界面可以看到，项目的欢迎信息显示的是英文，这是由于默认的语言环境设置的是en-us，这里将它设置为中文简体：

```python
LANGUAGE_CODE = ‘zh-Hans'
```

设置时区：

```python
LANGUAGE_CODE = 'zh-Hans'

TIME_ZONE = 'Asia/Shanghai'
USE_TZ = False
```

### 1.2 配置开发数据库

Django自带的sqlite3不适合做应用项目的数据库，所以，这里一般用MySQL等开源数据库替代项目的默认数据库。

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'django_bbs',
        'USER': 'root',
        'PASSWORD': '123456',
        'HOST': 'localhost',
        'PORT': '3306'
    }
}
```

ENGINE：配置使用的数据库引擎，可以通过查看django.db.backends确认Django可适配的数据库类型。

NAME：数据库的名称，Django将会利用这里配置的数据库去创建和操作数据表。

USER：数据库用户名。

PASSWORD：数据库密码。

HOST：数据库服务器地址，这里使用本地开发环境。

PORT：数据库服务器端口号，MySQL默认为3306。

### 1.3 INSTALLED_APPS中应用的数据库迁移

应用通常都会需要使用数据表来完成状态或数据的保存，Django自带的应用当然也需要数据表，如果没有创建这些表直接启动项目，那么除了不能使用这些应用提供的功能之外，还会打印警告信息。

manage.py的migrate命令用于将应用的模型定义或修改同步到数据库中。

migrate命令会检查INSTALLED_APPS里配置的应用列表，依次迭代为每个应用创建所需要的数据表。

```python
python manage.py migrate
```

migrate命令在项目数据库中创建了admin、auth等应用所需要的数据表。

Django对于数据库的迁移工作通过两个命令来实现：

```python
python manage.py makemigrate
python manage.py migrate
```

为了保证已经完成的迁移工作不会重复执行，Django会把每一次数据库迁移记录到django_migrations表中，每一次执行migrate前都会比较迁移文件是否已经记录在表中了，只有没出现过的才会执行。对于当前项目的第一次migrate，可以查看django_migrations表中的记录。

### 1.4 管理后台

Django内置应用django.contrib.admin提供了管理后台，方便非技术人员以可视化的形式对应用数据记录实现增删改查的操作。但是，这同时也是非常危险的，需要有用户和权限的概念去约束这些行为。

manage.py提供了createsuperuser命令用于创建超级用户，执行命令：

```python
python manage.py createsuperuser --username=admin --email=admin@email.com
```

当前命令执行后，需要重复输入两次密码（根据自己的需要设置），最终返回“Superuser created successfully”代表成功创建了超级用户。

### 1.5 给项目创建应用

Django项目就是基于Django框架开发的Web应用，它包含了一组配置和多个应用，称作App。一个App就是一个Python包，并且遵循约定有着同样的目录结构。通常一个App可以包含模型、视图、模板和URL配置，可以被应用到多个Django项目中，因为Django应用是可以重用的Python软件包。

利用manage.py提供的startapp命令创建一个post应用的命令如下：

```python
python manage.py startapp post
```

可以在manage.py的同级目录下看到多出了一个post目录，它的目录结构大致如下：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210817221246.jpeg)

外层的__init__.py文件标识post是一个Python包。

admin.py用于将Model定义注册到管理后台，是Django Admin应用的配置文件。

apps.py用于应用程序本身的配置。

migrations目录用于存储models.py文件中Model的定义及修改。

migrations/__init__.py文件标识migrations是一个Python包。

models.py用于定义应用中所需要的数据表。

tests.py文件用于编写当前应用程序的单元测试。

views.py文件用于编写应用程序的视图。这个目录结构包括了post应用的全部内容，后续需要做的就是填充对应的service逻辑对外提供服务。

### 1.6 requirements.txt文件

requirements.txt文件会包含当前项目的依赖包名称及其对应的版本号，所以，这个文件又被称为项目依赖关系清单。

由于开发Python项目通常会用到虚拟环境，因此开发的过程中会不断地安装、卸载、升级不同的依赖包。但是如果当前的项目迁移到其他的环境中或者提供给其他用户使用，就需要有一种方式说明当前项目的环境依赖情况。这也就是requirements.txt文件的作用了，利用它可以快速构建项目所需要的环境依赖。

若要给当前的项目生成requirements.txt文件，需要进入根目录，执行命令：

```python
pip freeze > requirements.txt
```

freeze会列出当前的虚拟环境中安装的依赖包及其版本号，它的输出格式与requirements.txt文件内容格式完全一样，所以，可以将其输出进行重定向，得到依赖清单。

将来，需要重建当前项目环境的时候，就可以执行命令：

```python
pip install -r requirements.txt
```

通常，项目开发的时候，都会区分开发环境（dev）与生产环境（prod），所以，如果不同的环境依赖不同的配置可以把通用的依赖放置在requirements.txt文件中，特定的依赖放在另一个文件中，如requirements-dev.txt。

