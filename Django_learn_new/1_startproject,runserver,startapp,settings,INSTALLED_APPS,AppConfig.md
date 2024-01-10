# Django

## 简介

- 开放源代码的著名 Python Web 框架
- MVC 模式（Model：模型，View：视图，Controller：控制器）

## 安装

```sh
pip install Django
```

查看版本
```sh
python3 -m django --version
```

## Django 项目（Project）

### 创建项目（startproject）

创建一个名为 mysite 的项目
```sh
python3 -m django startproject mysite
```

### 项目目录结构

让我们看看 **startproject** 创建了些什么：
```sh
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

这些目录和文件的用处是：
- **最外层的 mysite/ 根目录**：只是项目的容器，根目录名称对 Django 没有影响，你可以将它重命名为任何你喜欢的名称
- **manage.py**：一个让你用各种方式管理 Django 项目的命令行工具
- **里面一层的 mysite/ 目录**：包含你的项目，它是一个纯 Python 包。它的名字就是当你引用它内部任何东西时需要用到的 Python 包名（比如 mysite.urls）
- **mysite/__init__.py**：一个空文件，告诉 Python 这个目录应该被认为是一个 Python 包
- **mysite/settings.py**：Django 项目的配置文件（包括数据库配置，时区，安装的APP，中间件，日志配置，以及一些基本的目录配置等，其本质上相当于一个基本的web工程的全局配置）
- **mysite/urls.py**：Django 项目的 URL 路由配置文件
- **mysite/asgi.py**：作为你的项目的运行在 ASGI 兼容的 Web 服务器上的入口
- **mysite/wsgi.py**：作为你的项目的运行在 WSGI 兼容的Web服务器上的入口

### 使用内置的简易服务器运行项目（runserver）

确认一下你的 Django 项目是否真的创建成功了。切换到外层的 mysite 目录，然后运行下面的命令
```sh
python3 manage.py runserver
```
你应该会看到如下输出
![](resources/2024-01-08-19-28-05.png)

此时已经启动了 Django 开发服务器，这是一个用纯 Python 编写的轻量级网络服务器

服务器现在正在运行，通过浏览器访问 http://127.0.0.1:8000/ 
![](resources/2024-01-08-19-30-10.png)

## Django 应用（App）

### Django应用 和 Django项目 的区别

1. 能否直接运行
   1. 一个 Django**项目**（是一个Web应用，可以直接运行）可以包含多个 Django应用
   2. 一个 Django**应用**（不可以直接运行）是一个包含业务逻辑的Python包，提供一定的功能
2. 包含的文件
   1. Django**项目** 包含 **模板、路由、静态文件**等
   2. Django**应用** 包含 **模型、视图、模板、路由、静态文件**等
3. Django应用 和 Django项目 之间的对应关系
   1. ![](resources/2024-01-08-19-46-56.png)
   2. 一个 Django项目 可以包含多个 Django应用
   3. 一个 Django应用 可以被包含到多个 Django项目 中，因为 Django应用 是**可重用**的Python包

### 创建应用（startapp）

创建一个名为 blog 的应用
```sh
python3 manage.py startapp blog
```

### 应用目录结构

让我们看看 **startapp** 创建了些什么：
```sh
blog/
    __init__.py
    admin.py
    apps.py
    models.py
    tests.py
    views.py
    urls.py（自行创建）
    migrations/
        __init__.py
```

这些目录和文件的用处是：
- **最外层的 blog/ 根目录**是应用的容器
- **__init__.py**：一个空文件，告诉 Python 这个目录应该被认为是一个 Python 包
- **admin.py**：定义Admin模块管理对象的文件（可在该文件中注册模型，并将其纳入至Django管理站点中）
- **apps.py**：声明应用的文件
- **models.py**：定义应用**模型**的文件
- **tests.py**：编写应用测试用例的文件
- **views.py**：**视图**处理的文件（每个视图接收一个HTTP请求，经处理后返回一个响应结果）
- **urls.py**：管理应用**路由**的文件
- **migrations/ 目录**：也称**迁移文件夹**，包含数据库模型的迁移脚本，迁移可使Django跟踪模块变化内容，并相应的同步数据库

### 注册 应用（App）到 项目（Project）中

#### 项目中默认注册的应用

**mysite/mysite/settings.py** 文件中的变量 **INSTALLED_APPS**：
该变量的值是一个list，**给出在Django项目（Project）中包含的Django应用（App）**

Django框架默认情况下，Django项目中包含如下Django应用
```py
INSTALLED_APPS = [
    'django.contrib.admin',  # 管理员站点（Admin模块）
    'django.contrib.auth',  # 认证授权系统
    'django.contrib.contenttypes',  # 内容类型框架
    'django.contrib.sessions',  # 会话（session）框架
    'django.contrib.messages',  # 消息（message）框架
    'django.contrib.staticfiles',  # 管理静态文件的框架
]
```
这些应用被默认启用是为了给常规项目提供方便

#### 注册自己创建的应用到项目中

现在，把自己创建 blog 应用安装到项目中

之前创建 blog 应用时，生成的 **blog/apps.py** 文件中，默认包含 **BlogConfig**类（继承 **AppConfig**类）
```py
from django.apps import AppConfig

class BlogConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'blog'
```
> 事实上，一个Django应用就是一个 `django.apps.AppConfig` 类的**扩展子类**

所以 **BlogConfig**类 的点式路径是 `polls.apps.BlogConfig`
添加到在 **mysite/mysite/settings.py** 文件中 **INSTALLED_APPS** 变量中
```py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog.apps.BlogConfig',  # 新增
]
```

现在，项目就包含了 blog 应用

#### 注册应用到项目中的作用

[Django 添加到INSTALLED_APPS的目的是什么](https://geek-docs.com/django/django-questions/81_django_what_is_the_purpose_of_adding_to_installed_apps_in_django.html)

1. **启用应用程序**：将应用程序添加到 INSTALLED_APPS 是启用该应用程序的第一步。通过将应用程序添加到该列表中，Django将自动加载并执行该应用程序及其配置。这允许您在项目中使用该应用程序的所有功能和特性
2. **数据库迁移**：在Django中，应用程序通常与数据库模型相对应。当您添加应用程序到INSTALLED_APPS后，Django会自动检测到该应用程序的数据库模型，并生成对应的迁移文件。这些迁移文件用于同步数据库架构和模型定义，以确保数据库与应用程序的模型保持一致
3. **URL配置**：Django使用URL配置来处理请求并返回相应的视图。每个应用程序通常都具有自己的URL配置文件，其中包含定义应用程序特定URL模式的规则。通过将应用程序添加到INSTALLED_APPS，Django将自动加载并注册该应用程序的URL配置，使得应用程序中定义的URL模式能够正常工作
4. **静态文件**：Django中的静态文件（例如CSS、JavaScript和图像）通常与特定的应用程序相关联。通过将应用程序添加到INSTALLED_APPS，Django将自动处理该应用程序中的静态文件，并将它们提供给Web应用程序
