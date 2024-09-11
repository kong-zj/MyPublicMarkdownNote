# 前后端分离与不分离

## 前后端不分离

在前后端不分离的应用模式中，前端页面看到的效果都是由后端控制，由后端渲染页面或重定向，也就是后端需要控制前端的展示，前端与后端的耦合度很高
这种应用模式比较适合纯网页应用，但是当后端对接App时，App可能并不需要后端返回一个HTML网页，而仅仅是数据本身，所以后端原本返回网页的接口不再适用于前端App应用，为了对接App后端还需再开发一套接口

请求的数据交互如下：
![](resources/2023-11-03-10-48-10.png)

简单来说：前后端分不离的系统，没有前端，浏览器所看到的页面，是后端提供的，并且服务只有后台，没有前端服务

## 前后端分离

在前后端分离的应用模式中，后端仅返回前端所需的数据，不再渲染HTML页面，不再控制前端的效果。至于前端用户看到什么效果，从后端请求的数据如何加载到前端中，都由前端自己决定，网页有网页的处理方式，App有App的处理方式，但无论哪种前端，所需的数据基本相同，后端仅需开发一套逻辑对外提供数据即可
在前后端分离的应用模式中 ，前端与后端的耦合度相对较低。在前后端分离的应用模式中，我们通常将后端开发的每个视图都称为一个接口，或者API，前端通过访问接口来对数据进行增删改查

请求的数据交互如下：
![](resources/2023-11-03-10-49-59.png)

简单来说：前后端分离的系统，有前端，浏览器所看到的页面，是前端服务提供，后台开发web接口提供数据给前端，前端需要处理之后再展示到浏览器上

# RESTful API

![](resources/2024-01-15-19-43-11.png)

[RESTful 架构详解](https://www.runoob.com/w3cnote/restful-architecture.html)

## API 简介

API 即 应用程序接口（英文：Application Programming Interface，简称API）。那么它是一个怎样的接口呢，现在我们常将它看成一个HTTP接口即 HTTP API。也就是说这个接口得通过HTTP的方式来调用，做过前后端开发的小伙伴可能知道，后端开发又叫做面向接口开发，我们往往会提供一个接口供前端调用，或者供其他服务调用。举个例子，我们程序中往往会涉及到调用第三方接口，比如调用支付宝或者微信的支付接口来实现我们程序中的支付功能、调用带三方的短信接口来向用户发送验证码短信等等

### HTTP API 举例

比如说我们有一个可以允许我们查看（view），创建（create），编辑（edit），删除（delete）图书的应用程序。我们可以创建一个可以让我们执行这些功能的 HTTP API：
`http://demo.com/view_books`
`http://demo.com/view_book?id=1`
`http://demo.com/create_new_book?name=shuxue`
`http://demo.com/update_book?id=1&name=shuxue`
`http://demo.com/delete_book?id=1`

这是5个 HTTP API，分别实现了图书的查看全部、查看一本、新增、编辑、删除的操作，当我们把接口发布出去的时候，别人就可以通过这些接口来调用相关的服务了。但是这样做有什么不方便的地方呢？你可能发现了，这种API的写法有一个缺点，那就是没有一个统一的风格，比如说第一个接口表示查询全部图书的信息，我们也可以写成这样：
`http://demo.com/books/list`

那这样就会造成使用我们接口的其他人，必须得参考API才能知道它是怎么运作的
不用担心，REST会帮我们解决这个问题

## REST 简介

REST 即 表述性状态传递（英文：Representational State Transfer，简称REST），是一种软件架构风格。它是一种针对网络应用的设计和开发方式，可以降低开发的复杂性，提高系统的可伸缩性

### HTTP 动词

REST 规定，对于资源的具体操作类型，由 HTTP 动词表示

HTTP 总共包含以下动词：
- **GET**：从服务器取出资源（一项或多项）
- **POST**：在服务器新建一个资源
- **PUT**：在服务器更新资源（客户端提供改变后的完整资源）
- PATCH：在服务器更新资源（客户端只提供需要改变的属性）
- **DELETE**：从服务器删除资源
- OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的
- HEAD：获取资源的元数据（描述数据的数据）
- TRACE：回显服务器收到的请求，主要用于测试或诊断
- CONNECT：HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器

### RESTful API 举例

REST 的作用是将我们上面提到的查看（view），创建（create），编辑（edit），删除（delete）直接映射到 HTTP 中已实现的 GET，POST，PUT，DELETE 方法

我们重新将上面的5个接口改写成REST风格

查看所有图书：
- GET http://demo.com/books

查看一本图书：
- GET http://demo.com/books/1

新增一本书：
- POST http://demo.com/books
- Data: name=shuxue

修改一本书：
- PUT http://demo.com/books/1
- Data: name=shuxue

删除一本书：
- DELETE http://demo.com/books/1

> 统一资源接口要求使用标准的HTTP方法对资源进行操作，所以URL只应该来表示资源的名称，而不应该包括资源的操作（**URL不应该使用动作来描述**）

这样改动之后API变得统一了，我们只需要改变请求方式就可以完成相关的操作，这样大大简化了我们接口的理解难度，变得易于调用，这就是REST风格的意义

### 过滤

![](resources/2024-01-22-10-36-34.png)

### HTTP 状态码

REST 的另一重要部分就是为既定好请求的类型来响应正确的状态码。当你请求 HTTP 时，服务器会响应一个状态码来显示你的请求是否成功，然后客户端应如何继续。以下是四种不同层次的状态码：
- 2xx = Success（成功）
- 3xx = Redirect（重定向）
- 4xx = User error（客户端错误）
- 5xx = Server error（服务器端错误）

我们常见的是 200（请求成功）、404（未找到）、401（未授权）、500（服务器错误）

### API 格式响应

上面介绍了REST API的写法，响应状态码，剩下就是请求的数据格式以及响应的数据格式。说的通俗点就是，我们用什么格式的参数去请求接口并且我们能得到什么格式的响应结果

我这里只介绍一种用的最多的格式：**JSON格式**（**前端的js对象** 和 **后端的字典** 之前的桥梁）
目前json已经发展成了一种最常用的数据格式，由于其轻量、易读的优点

所以我们经常会看到一个请求的header信息中有这样的参数：
```json
Accept:application/json
```
这个参数的意思就是接收来自后端的json格式的信息

#### json 响应例子

```json
{
	"code": 200,
	"books": [{
		"id": 1,
		"name": "yuwen"
	}, {
		"id": 2,
		"name": "shuxue"
	}]
}
```

### 返回结果

针对不同操作（如GET、POST），服务器向用户返回的结果应该符合以下规范：
- GET /collections：返回资源对象的列表（数组）
- GET /collections/identity：读取资源时，传入标识符（identity），服务端返回标识符指定的单个资源对象
- POST /collections：返回新生成的资源对象
- PUT /collections/identity：返回完整的资源对象
- PATCH /collections/identity：返回完整的资源对象
- DELETE /collections/identity：返回空的资源对象

# Django 和 DRF 的区别和联系

[一图看懂Django和DRF](https://zhuanlan.zhihu.com/p/53957464)

## Django 框图

![](resources/2024-01-16-21-50-05.png)

## DRF 框图

![](resources/2024-01-16-21-50-32.png)

## 最少语言描述 Django

将数据库的东西通过 ORM 的映射取出来，通过 view 文件，按照 template 文件排出的模板渲染成 HTML。当用户请求相应的 url 时，返回相应的结果

## 最少语言描述 DRF

将数据库的东西通过 ORM 的映射取出来，通过 view 和 serializers 文件绑定 REST 接口，当前端请求时，返回序列化好的 json

## 最少语言描述 DRF 在 Django 的基础上做了什么

DRF 是 Django 的超集，去掉了模板的部分，提供了一个 REST 的接口，同时也提供了满足该接口的代码工作流。同时，在 REST 的规范下，升级了权限和分页等功能，增加了限流和过滤搜索等功能

## 总结

Django + DRF 将后端变成一种声明式的工作流，只要按照 **models -> serializers -> views -> urls** 的模式去一个个py文件去配置，即可生成一个很全面的通用的后端。当然，如果需求不那么通用，这种设计就变成了一个累赘

事实上，过重的设计降低了灵活性，报错基本得去翻源码实现，然后再吐槽一遍源码实现，这也是有得必有失。当然，现在 Django 和 DRF 一直在优化 middeware（中间件）的设计，也有 api_view 这种类似 flask 的装饰器的实现方式，也是在灵活性方面的一种权衡，不过对于初学者来说，仍然是个不大不小的坎

# 准备工作

[官方快速入手教程（DRF 的 tutorial）](https://www.django-rest-framework.org/tutorial/quickstart/)

> DRF 的 tutorial 讲的是 serializers 怎么写，view 怎么写，在 DRF 中 view 这一层既可以一个个 get、post 从头开始写起，也可以采用抽象程度比较高的 viewset 去按配置生成。另外还讲了一些 DRF 相较于 Django 升级和新增的功能

## 项目前置配置（Django部分）

### 使用虚拟环境（venv）

在新创建的目录 /home/kzj/django_rest_learn 中
用如下命令创建一个名为 django_rest_env 的python虚拟环境
```sh
python3 -m venv django_rest_env
```
并用如下命令激活这个虚拟环境
```sh
source django_rest_env/bin/activate
```

![](resources/2024-01-15-20-37-34.png)

#### 如何停止使用虚拟环境

如果当前虚拟环境已经处于激活状态，可以使用如下命令退出虚拟环境
```sh
deactivate
```

#### 如何删除虚拟环境

删除虚拟环境，只需要**将这个虚拟环境目录删除**即可

### 安装 django

在激活的虚拟环境中，用如下命令安装 django
```sh
pip install django
```

### 创建与配置 项目和应用

用如下命令创建 drf_tutorial 项目和 course_api 应用
```sh
django-admin startproject drf_tutorial
django-admin startapp course_api
```

![](resources/2024-01-15-20-45-57.png)

因为添加了一个新的app，我们需要告诉Django
因此，确保将 course_api 添加到 setting.py 文件中的 INSTALLED_APPS 列表中

**drf_tutorial/drf_tutorial/settings.py** 文件的部分对应内容修改为：
```py
ALLOWED_HOSTS = ["*"]
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'course_api',   # 新增
]
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'djangoRESTdb',
        'HOST': '127.0.0.1',
        'PORT': 3306,
        'USER': 'root',
        'PASSWORD': '1',
    }
}
TIME_ZONE = 'Asia/Shanghai'
```

### 配置数据库

这里使用MySQL
创建名为djangoRESTdb的数据库
```sql
create database djangoRESTdb default charset=utf8;   
```

安装pymysql
```sh
pip install pymysql
```

在 **\_\_init\_\_.py** 文件（与settings.py同一个目录）中引入模块和进行配置
```py
import pymysql
pymysql.install_as_MySQLdb()
```

### 定义数据模型（Model）

在 models.py 文件的 Course 类中，我们创建2个字段: id、name
修改 **drf_tutorial/course_api/models.py** 文件的内容为：
```py
from django.db import models
from django.conf import settings

class Course(models.Model):
    name = models.CharField(max_length=255, unique=True, help_text='课程名称', verbose_name='名称')
    introduction = models.TextField(help_text='课程介绍', verbose_name='介绍')
    # teacher 用外键关联到 Django 默认的用户表
    teacher = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE, help_text='课程讲师', verbose_name='讲师')
    price = models.DecimalField(max_digits=6, decimal_places=2, help_text='课程价格', verbose_name='价格')
    created_at = models.DateTimeField(auto_now_add=True, help_text='课程创建时间', verbose_name='创建时间')
    updated_at = models.DateTimeField(auto_now=True, help_text='课程更新时间', verbose_name='更新时间')
    
    class Meta:
        verbose_name = '课程信息'
        verbose_name_plural = verbose_name
        ordering = ('price',)

    def __str__(self):
        return self.name
```

### 执行数据同步

创建新的迁移文件并更新同步到数据库
```sh
python3 manage.py makemigrations
python3 manage.py migrate
```

### 配置管理后台 admin

我们希望在Django的后台中管理我们的数据，因此，我们在 admin.py 文件中注册 Course 类
修改 **drf_tutorial/course_api/admin.py** 文件的内容为：
```py
from django.contrib import admin
from .models import Course

@admin.register(Course)
class CourseAdmin(admin.ModelAdmin):
    list_display = ('name', 'introduction', 'teacher', 'price')
    search_fields = list_display
    list_filter = list_display
```

创建一个超级管理员帐户（用户名：admin，密码：123456）
```sh
python3 manage.py createsuperuser
```

### 运行项目

启动 django web 服务
```sh
python3 manage.py runserver
```

进入 http://127.0.0.1:8000/admin/
手动添加一些 Course 数据，如下图
![](resources/2024-01-15-22-20-56.png)

到此，我们已经完成了Django部分，由于本示例是为了创建一个API服务，所以我们不需要创建模板和视图。相反，我们还需要继续添加 **Django Rest** 库来处理将模型数据转换为 **RESTful API**

# 使用 Django REST framework

[Django REST framework 官网](https://www.django-rest-framework.org/)

## 安装 djangorestframework 及其依赖包

在激活的虚拟环境中，用如下命令安装 djangorestframework 及其依赖包
```sh
pip install djangorestframework
pip install markdown
pip install Pygments
pip install django-filter
pip install PyYAML
pip install uritemplate
```

## Django Rest Framework 的模块介绍

20个：

序列化
视图
路由
认证
权限
限流
过滤
解析器
渲染器
分页
版本
测试
概要
异常处理
配置
验证
状态码
内容协商
缓存
元数据

## Django Rest Framework 配置

### INSTALLED_APPS 中加入 `'rest_framework'`

**drf_tutorial/drf_tutorial/settings.py** 文件的部分对应内容修改为：
```py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',   # RESTful API
    'rest_framework.authtoken', # DRF 自带的Token认证
    'course_api',
]
```

### 根urlpatterns 中加入 `path('api-auth/', include('rest_framework.urls'))`

修改 **drf_tutorial/drf_tutorial/urls.py** 文件的内容为：
```py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api-auth/', include('rest_framework.urls')),  # DRF 的登录和退出
]
```

### 添加 DRF 全局配置

在 **drf_tutorial/drf_tutorial/settings.py** 文件中添加：
```py
REST_FRAMEWORK = {
    # 分页
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    # 每页的数据条数
    'PAGE_SIZE': 10,
    # 时间显示格式
    'DATETIME_FORMAT': '%Y-%m-%d %H:%M:%S',
    # 当 DRF 返回 Response 对象时，使用哪个 render 类
    'DEFAULT_RENDERER_CLASSES': [
        'rest_framework.renderers.JSONRenderer',
        'rest_framework.renderers.BrowsableAPIRenderer',
    ],
    # 如何解析 Request 请求中的 data
    'DEFAULT_PARSER_CLASSES': [
        'rest_framework.parsers.JSONParser',
        'rest_framework.parsers.FormParser',
        'rest_framework.parsers.MultiPartParser',
    ],
    # 权限控制
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ],
    # 认证
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.BasicAuthentication',
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.TokenAuthentication',
    ]
}
```

### 执行数据同步

创建新的迁移文件并更新同步到数据库
```sh
python3 manage.py makemigrations
python3 manage.py migrate
```

### 运行项目

启动 django web 服务
```sh
python3 manage.py runserver
```

进入 http://127.0.0.1:8000/api-auth/login/
用之前创建的 admin 用户登录
![](resources/2024-01-15-23-00-01.png)

## 序列化（serializers）

### 简介

序列化，也叫 序列化器，用于将 **查询集QuerySet** 或 **模型类实例Instance** 这种 Django数据类型，转化为 **JSON** 或 **XML** 格式（方便前端渲染）的数据
**序列化**：`queryset, instance  ->  json/xml/yaml`（处理从数据库中查询到的数据，传给前端）
**反序列化**：`queryset, instance  <-  json/xml/yaml`（处理前端传来的数据，保存到数据库）

序列化 与 反序列化 存在的原因：方便前后端分离架构下的数据交互

### 如果使用 Django 默认的序列化器（serializers）

进入 Django Shell
```sh
python3 manage.py shell
```

```py
from course_api.models import Course
from django.core import serializers
# 把查询到的数据，序列化为json格式
serializers.serialize('json', Course.objects.all())
# fields参数，可以指定只序列化哪些字段
serializers.serialize('json', Course.objects.all(), fields=("name"))
```

![](resources/2024-01-31-21-03-03.png)

![](resources/2024-01-31-21-03-31.png)

Django 默认的序列化器还有许多要完善的地方：
1. 对前端提交的数据进行验证（前端传来的数据放到 request.data 中）
2. 验证器的参数
3. 不方便同时序列化多个对象
4. 序列化的过程中添加上下文（在序列化的结果中，添加 查询集QuerySet 没有的数据）
5. 没有对无效的数据异常处理

而 DRF 默认的序列化器 已经帮我们做好了

### 使用 DRF 默认的模型类序列化器（ModelSerializer）

**ModelSerializer模型类序列化器** 的写法和用法，类似 **ModelForm模型表单类**

模型类序列化器 继承 `serializers.ModelSerializer`

新建 **drf_tutorial/course_api/serializers.py** 文件，其内容如下：
```py
from rest_framework import serializers
from .models import Course
from django.contrib.auth.models import User

class CourseSerializer(serializers.ModelSerializer):
    teacher = serializers.ReadOnlyField(source='teacher.username')  # 外键字段，只读
    class Meta:
        # 关联数据表（前面不是变量名）
        model = Course
        # 确定需要序列化的字段（返回给用户的具体表中的字段）（前面不是变量名）
        fields = ['id', 'name', 'introduction', 'price', 'created_at', 'updated_at', 'teacher']
        # 表示全部字段
        # fields = '__all__'

class UserSerializer(serializers.ModelSerializer):
    class Meta:
        model = User
        fields = '__all__'
```


### HyperlinkedModelSerializer

继承 HyperlinkedModelSerializer

https://www.jianshu.com/p/a66e04af8a31

---

跳过p9


 



## 开发 RESTful API 接口

目标：使用 DRF 开发 RESTful API 接口（4种实现方式）
效果：用DRF的多种视图，实现课程信息的增删改查

### 如果仅使用 Django 而不使用 DRF

修改 **drf_tutorial/course_api/views.py** 文件的内容为：
```py
import json
from django.http import JsonResponse, HttpResponse
from django.views.decorators.csrf import csrf_exempt
from django.views import View

# 模拟从数据库中查询到的数据
course_dict = {
    'name': '课程名称',
    'introduction': '课程介绍',
    'price': 0.11,
}

# Django原生 FBV 编写API接口
@csrf_exempt
def course_list(request):
    if request.method == 'GET':
        # return HttpResponse(json.dumps(course_dict), content_type='application/json')
        # 等价于
        return JsonResponse(course_dict)
    if request.method == 'POST':
        course = json.loads(request.body.decode('utf-8'))
        return JsonResponse(course, safe=False)


# Django原生 CBV 编写API接口
class CourseList(View):
    def get(self, request):
        return JsonResponse(course_dict)
    
    @csrf_exempt
    def post(self, request):
        course = json.loads(request.body.decode('utf-8'))
        return JsonResponse(course, safe=False)
```

#### 配置路由

新建 **drf_tutorial/course_api/urls.py** 文件，其内容如下：
```py
from django.urls import path, include
from course_api import views

urlpatterns = [
    # FBV
    path("fbv/list", views.course_list, name="fbv-list"),
    # CBV
    path("cbv/list", views.CourseList.as_view(), name="cbv-list"),
]
```

修改 **drf_tutorial/drf_tutorial/urls.py** 文件的内容为：
```py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api-auth/', include('rest_framework.urls')),
    path('course/', include('course_api.urls')),
]
```

### DRF的 基础函数视图 Function Based View（FBV）

REST framework 允许使用基于函数的视图。它提供了一套简单的**装饰器**来包装你的函数视图，以确保它们接收 `Request`（而不是 Django `HttpRequest`）实例，并允许它们返回 `Response`（而不是 Django `HttpResponse`），并允许你配置该请求的处理方式

#### 装饰器 `@api_view()`

用法：`@api_view(http_method_names=['GET'])`（用 http_method_names 来设置视图允许响应的 HTTP 方法列表），也可简写为 `@api_view(['GET'])`

#### 获取所有课程信息、新增一个课程 的接口

修改 **drf_tutorial/course_api/views.py** 文件的内容为：
```py
from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework import status
from .models import Course
from .serializers import CourseSerializer

# FBV
@api_view(["GET", "POST"])
def course_list(request):
    # 获取所有课程信息，或新增一个课程
    if request.method == "GET":
        # 序列化
        # many=True参数，实现序列化多个对象
        s = CourseSerializer(instance=Course.objects.all(), many=True)
        return Response(data=s.data, status=status.HTTP_200_OK)
    elif request.method == "POST":
        # 反序列化
        # partial=True参数，表示允许部分更新（非必填的字段可以不填）
        s = CourseSerializer(data=request.data, partial=True)
        # 对数据进行校验
        if s.is_valid():
            # teacher为当前登录的用户
            s.save(teacher=request.user)
            return Response(data=s.data, status=status.HTTP_201_CREATED)
        else:
            return Response(s.errors, status=status.HTTP_400_BAD_REQUEST)
```

##### 配置路由

修改 **drf_tutorial/course_api/urls.py** 文件的内容为：
```py
from django.urls import path, include
from course_api import views

urlpatterns = [
    # FBV
    path("fbv/list", views.course_list, name="fbv-list"),
]
```

启动 django web 服务
```sh
python3 manage.py runserver
```
进入 http://127.0.0.1:8000/course/fbv/list ，如果提示登录则登录
可以用 **GET方法** 获取所有课程信息
![](resources/2024-01-31-23-29-00.png)

可以用 **POST方法** 新增一个课程
![](resources/2024-01-31-23-41-20.png)
![](resources/2024-01-31-23-40-28.png)

#### 获取单个课程信息、更新课程信息、删除课程信息 的接口

修改 **drf_tutorial/course_api/views.py** 文件的内容为：
```py
from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework import status
from .models import Course
from .serializers import CourseSerializer

# FBV
@api_view(["GET", "POST"])
def course_list(request):
    # 省略
  
@api_view(["GET", "PUT", "DELETE"])
def course_detail(request, pk):
    try:
        # 拿到对应的课程
        course = Course.objects.get(pk=pk)
    except Course.DoesNotExist:
        return Response(data={"msg": "没有此课程信息"}, status=status.HTTP_404_NOT_FOUND)
    else:
        if request.method == "GET":
            s = CourseSerializer(instance=course)
            return Response(data=s.data, status=status.HTTP_200_OK)
        elif request.method == "PUT":
            s = CourseSerializer(instance=course, data=request.data, partial=True)
            if s.is_valid():
                s.save()
                return Response(data=s.data, status=status.HTTP_200_OK)
        elif request.method == "DELETE":
            course.delete()
            return Response(status=status.HTTP_204_NO_CONTENT)
```

注意 模型类序列化器 的 构造实例对象 方法的参数中：
1. `instance` 参数，表示要 **序列化** 的 **模型类对象实例**
2. `data` 参数，表示要 **反序列化** 的 **前端传来的数据**

##### 配置路由

修改 **drf_tutorial/course_api/urls.py** 文件的内容为：
```py
from django.urls import path, include
from course_api import views

urlpatterns = [
    # FBV
    path("fbv/list", views.course_list, name="fbv-list"),
    path("fbv/detail/<int:pk>", views.course_detail, name="fbv-detail"),
]
```

进入 http://127.0.0.1:8000/course/fbv/detail/1
可以用 **GET方法** 获取id为1的课程信息
![](resources/2024-02-03-17-31-51.png)

接下来使用 Postman 来测试接口 http://127.0.0.1:8000/course/fbv/detail/1

测试 **GET方法**

如果不使用认证，会报错
![](resources/2024-02-03-17-36-13.png)

使用 Basic Auth 认证
![](resources/2024-02-03-17-42-01.png)

测试 **PUT方法**

![](resources/2024-02-03-17-50-39.png)

测试 **DELETE方法**

![](resources/2024-02-03-17-52-26.png)

### DRF的 基础类视图 Class Based View（CBV）（APIView）

REST framework 允许使用基于类的视图。APIView 与 Django 的 View 类似，我们的业务类只需要**继承** `APIView`，在URL传递过程，我们只需要调用 APIView 的 `as_view()` 方法，然后 URL 就会调用业务类对应的 HTTP 方法

和 FBV 相比，CBV 可以用到类的特性（封装、继承、多态）

#### 需要继承 `APIView` 类

`APIView` 是 DRF框架 视图的基本类，**继承自 View**，在 View 的基础上封装了大量基础功能

##### APIView 属性

renderer_classes：渲染器类
parser_classes：解析器类
authentication_classes：验证类
throttle_classes：限流类
permission_classes：权限类
content_negotiation_class：内容协商类

##### APIView 的get型方法

get_renderers(self)：获取渲染器方法
get_parsers(self)：获取解释器方法
get_authenticators(self)：获取认证方法
get_throttles(self)：获取限流方法
get_permissions(self)：获取权限方法
get_content_negotiator(self)：获取内容协商方法

##### APIView 的check型方法

check_permissions(self, request)：检查权限
check_throttles(self, request)：检查节流
check_content_negotiation(self, request, force=False)：检查内容协商

##### APIView 的调度方法

initial(self, request, *args, **kwargs)：执行任何操作,需要发生在处理程序方法之前被调用。这个方法是用来执行权限和节流,并执行内容协商
handle_exception(self, exc)：抛出的任何异常处理程序方法将被传递给这个方法,而返回响应实例,或者re-raises异常
initialize_request(self, request, *args, **kwargs)：确保请求对象传递给处理程序方法是request的一个实例，而不是django的HttpRequest
finalize_response(self, request, response, *args, **kwargs)：确保任何响应处理程序方法返回的对象将被呈现到正确的内容类型

#### 获取所有课程信息、新增一个课程 的接口

修改 **drf_tutorial/course_api/views.py** 文件的内容为：
```py
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from .models import Course
from .serializers import CourseSerializer

# CBV
class CourseList(APIView):
    def get(self, request):
        queryset = Course.objects.all()
        s = CourseSerializer(instance=queryset, many=True)
        return Response(data=s.data, status=status.HTTP_200_OK)
    
    def post(self, request):
        s = CourseSerializer(data=request.data, partial=True)
        if s.is_valid():
            s.save(teacher=self.request.user)
            return Response(data=s.data, status=status.HTTP_201_CREATED)
        else:
            return Response(s.errors, status=status.HTTP_400_BAD_REQUEST)
```

##### 配置路由

和上面 FBV 中不同的是，这里需要使用 `as_view()`

修改 **drf_tutorial/course_api/urls.py** 文件的内容为：
```py
from django.urls import path, include
from course_api import views

urlpatterns = [
    # FBV
    # path("fbv/list", views.course_list, name="fbv-list"),
    # path("fbv/detail/<int:pk>", views.course_detail, name="fbv-detail"),
    # CBV
    path("cbv/list", views.CourseList.as_view(), name="cbv-list"),
]
```

使用 Postman 来测试接口 http://127.0.0.1:8000/course/cbv/list

测试 **GET方法**

![](resources/2024-02-03-22-55-49.png)

测试 **POST方法**

![](resources/2024-02-03-22-57-46.png)

#### 获取单个课程信息、更新课程信息、删除课程信息 的接口

修改 **drf_tutorial/course_api/views.py** 文件的内容为：
```py
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from .models import Course
from .serializers import CourseSerializer

# CBV
class CourseList(APIView):
    # 省略

class CourseDetail(APIView):
    @staticmethod
    def get_object(pk):
        try:
            return Course.objects.get(pk=pk)
        except Course.DoesNotExist:
            return None
    
    def get(self, request, pk):
        course = self.get_object(pk)
        if not course:
            return Response(data={"msg": "没有此课程信息"}, status=status.HTTP_404_NOT_FOUND)
        else:
            s = CourseSerializer(instance=course)
            return Response(data=s.data, status=status.HTTP_200_OK)
        
    def put(self, request, pk):
        course = self.get_object(pk)
        if not course:
            return Response(data={"msg": "没有此课程信息"}, status=status.HTTP_404_NOT_FOUND)
        else:
            s = CourseSerializer(instance=course, data=request.data, partial=True)
            if s.is_valid():
                s.save()
                return Response(data=s.data, status=status.HTTP_200_OK)
    
    def delete(self, request, pk):
        course = self.get_object(pk)
        if not course:
            return Response(data={"msg": "没有此课程信息"}, status=status.HTTP_404_NOT_FOUND)
        else:
            course.delete()
            return Response(status=status.HTTP_204_NO_CONTENT)
```

##### 配置路由

修改 **drf_tutorial/course_api/urls.py** 文件的内容为：
```py
from django.urls import path, include
from course_api import views

urlpatterns = [
    # FBV
    # path("fbv/list", views.course_list, name="fbv-list"),
    # path("fbv/detail/<int:pk>", views.course_detail, name="fbv-detail"),
    # CBV
    path("cbv/list", views.CourseList.as_view(), name="cbv-list"),
    path("cbv/detail/<int:pk>", views.CourseDetail.as_view(), name="cbv-detail"),
]
```

使用 Postman 来测试接口 http://127.0.0.1:8000/course/cbv/detail/2

测试 **GET方法**

![](resources/2024-02-03-23-17-32.png)

测试 **PUT方法**

![](resources/2024-02-03-23-19-11.png)

测试 **DELETE方法**

![](resources/2024-02-03-23-20-04.png)

### DRF的 通用类视图 Generic Class Based View（GCBV）（generics）

上面的 基础函数视图（FBV）和 基础类视图（CBV）中，各种功能都需要亲自实现，代码的重复率还是比较高的，下面，我们使用更简洁的写法

通用类视图 就是把一些常见的增删改查操作对应的类，通过 **mixins混合**，然后通过**继承**，来快速实现这些功能

#### 需要继承 `generics` 包里的类（高级通用类视图）

![](resources/2024-02-04-14-54-28.png)

![](resources/2024-02-04-15-07-14.png)

其中：
`ListAPIView`
`CreateAPIView`
`UpdateAPIView`
`DestroyAPIView`
`RetrieveAPIView`
`ListCreateAPIView`
`RetrieveUpdateAPIView`
`RetrieveDestroyAPIView`
`RetrieveUpdateDestroyAPIView`

都继承自 `GenericAPIView`

而 `GenericAPIView` 继承自 `APIView`

##### 默认继承 `GenericAPIView` 类（基础通用类视图）

`GenericAPIView` 是DRF中的通用视图类（**提供序列化器与数据库查询的方法**），**继承自 APIView**，通用视图类，可以让你只需要配置好类属性，就可以实现一整套的增删查改流程

###### queryset

`queryset` 属性，用来指定**查询集**

###### serializer_class

`serializer_class` 属性，用来指定**序列化器**

##### mixins混合（模型混入类视图）

![](resources/2024-02-04-15-16-16.png)

`ListModelMixin`：提供 `list()` 方法，列出queryset
`CreateModelMixin`： 提供 `create()` 方法，创建和保存一个Model对象
`UpdateModelMixin`： 提供 `update()` 方法，更改一个Model对象
`DestroyModelMixin`：提供 `destroy()` 方法，删除一个Model对象
`RetrieveModelMixin`：提供 `retrieve()` 方法，获取一个存在的model对象

![](resources/2024-02-04-15-07-14.png)

总结：
1. 视图工具类mixins 提供了**五大类**实现了**六大方法**
2. 减少我们自己的代码量，他都给封装好了
3. 必须与 `GenericAPIView` 一起使用

#### 获取所有课程信息、新增一个课程 的接口（ListCreateAPIView）

修改 **drf_tutorial/course_api/views.py** 文件的内容为：
```py
from rest_framework import generics
from .models import Course
from .serializers import CourseSerializer

# GCBV
class GCourseList(generics.ListCreateAPIView):
    # ListCreateAPIView 中的 List 代表查看所有，Create 代表新增一个
    queryset = Course.objects.all()
    serializer_class = CourseSerializer
    # 本来只要写上面两行代码就好，但是
    # ListCreateAPIView 的 mixins.CreateModelMixin 的源代码中，直接 serializer.save()，不符合我们的要求，那就重写
    def perform_create(self, serializer):
        serializer.save(teacher=self.request.user)
```

`ListCreateAPIView` 中的 `List` 代表**查看所有**，`Create` 代表**新增一个**

##### 配置路由

修改 **drf_tutorial/course_api/urls.py** 文件的内容为：
```py
from django.urls import path, include
from course_api import views

urlpatterns = [
    # FBV
    # path("fbv/list", views.course_list, name="fbv-list"),
    # path("fbv/detail/<int:pk>", views.course_detail, name="fbv-detail"),
    # CBV
    # path("cbv/list", views.CourseList.as_view(), name="cbv-list"),
    # path("cbv/detail/<int:pk>", views.CourseDetail.as_view(), name="cbv-detail"),
    # GCBV
    path("gcbv/list", views.GCourseList.as_view(), name="gcbv-list"),
]
```

使用 Postman 来测试接口 http://127.0.0.1:8000/course/gcbv/list

测试 **GET方法**

![](resources/2024-02-04-00-06-42.png)
与前面的 FBV 和 CBV 不同，GCBV 的这个接口还帮我们做了其他的事情，比如分页

测试 **POST方法**

![](resources/2024-02-04-00-12-56.png)

#### 获取单个课程信息、更新课程信息、删除课程信息 的接口（RetrieveUpdateDestroyAPIView）

修改 **drf_tutorial/course_api/views.py** 文件的内容为：
```py
from rest_framework import generics
from .models import Course
from .serializers import CourseSerializer

# GCBV
class GCourseList(generics.ListCreateAPIView):
    # 省略

class GCourseDetail(generics.RetrieveUpdateDestroyAPIView):
    # RetrieveUpdateDestroyAPIView 中的 Retrieve 代表查询一个，Update 代表修改一个，Destroy 代表删除一个
    queryset = Course.objects.all()
    serializer_class = CourseSerializer
    # 这里不需要重写 perform_create()方法
```

`RetrieveUpdateDestroyAPIView` 中的 `Retrieve` 代表**查询一个**，`Update` 代表**修改一个**，`Destroy` 代表**删除一个**

按住`Ctrl`点击 `RetrieveUpdateDestroyAPIView`，可以查看源代码
![](resources/2024-02-04-00-30-08.png)
这里对 修改一条记录，提供了两种方法，分别是 **PUT方法**（整体修改）和 **PATCH方法**（部分修改）

按住`Ctrl`点击 `RetrieveModelMixin`，可以查看源代码
![](resources/2024-02-04-15-00-24.png)
可见，符合上面所说的结构

##### 配置路由

修改 **drf_tutorial/course_api/urls.py** 文件的内容为：
```py
from django.urls import path, include
from course_api import views

urlpatterns = [
    # FBV
    # path("fbv/list", views.course_list, name="fbv-list"),
    # path("fbv/detail/<int:pk>", views.course_detail, name="fbv-detail"),
    # CBV
    # path("cbv/list", views.CourseList.as_view(), name="cbv-list"),
    # path("cbv/detail/<int:pk>", views.CourseDetail.as_view(), name="cbv-detail"),
    # GCBV
    path("gcbv/list", views.GCourseList.as_view(), name="gcbv-list"),
    path("gcbv/detail/<int:pk>", views.GCourseDetail.as_view(), name="gcbv-detail"),
]
```

这里这里的 `/<int:pk>` 中的变量名 `pk` 对应源代码中的
![](resources/2024-02-04-16-05-11.png)

使用 Postman 来测试接口 http://127.0.0.1:8000/course/gcbv/detail/3

测试 **GET方法**

![](resources/2024-02-04-00-14-31.png)

测试 **PUT方法**

![](resources/2024-02-04-00-39-24.png)
源代码中使用参数 `partial=False`，所以不能部分修改

测试 **PATCH方法**

![](resources/2024-02-04-00-39-51.png)
源代码中使用参数 `partial=True`，所以可以部分修改

测试 **DELETE方法**

![](resources/2024-02-04-00-40-23.png)

### DRF的 视图集 ViewSet

GCBV 中，我们分别通过 `GCourseList`类 和 `GCourseDetail`类 实现两个接口，能不能两个接口能不能**合二为一**？

REST framework 允许将一组相关的逻辑视图聚集在一个类，`ViewSet` 类是一个简单类型的基于类的视图，没有提供任何方法处理程序如`get()`，`post()`等，而提供代替方法比如`list()`，`retrieve()`，`create()`，`update()`，`destroy()`等

> list(): 提供一组数据
> retrieve(): 提供单个数据
> create(): 创建数据
> update(): 保存数据
> destory(): 删除数据

#### 需要继承 `viewsets` 包里的类

![](resources/2024-02-04-17-02-07.png)

##### `GenericViewSet`

`GenericViewSet` 继承 `GenericAPIView`，提供了默认的`get_queryset()`和`get_object()`等方法来获取model数据，但不提供任何请求处理方法

##### `ModelViewSet`

`ModelViewSet` 同时继承 `CreateModelMixin`、`RetrieveModelMixin`、`UpdateModelMixin`、`DestroyModelMixin`、`ListModelMixin`、`GenericViewSet` 类，增加了一些请求处理方法，如`list()`，`retrieve()`，`create()`，`update()`，`partial_update()`，`destroy()`等

一连串的继承关系是：
`ModelViewSet` -> `GenericViewSet` -> `GenericAPIView` -> `APIView` -> `View`

##### `ReadOnlyModelViewSet`

`ReadOnlyModelViewSet` 同时继承 `RetrieveModelMixin`、`ListModelMixin`、`GenericViewSet` 类，增加了一些请求处理方法，如`list()`，`retrieve()`等

#### 一步实现上述的两个接口（ModelViewSet）

只需要写一个视图类

继承 `ModelViewSet` 可以同时实现增删改查方法，而不用我们自己去写，减少代码量

修改 **drf_tutorial/course_api/views.py** 文件的内容为：
```py
from rest_framework import viewsets
from .models import Course
from .serializers import CourseSerializer

class CourseViewSet(viewsets.ModelViewSet):
    queryset = Course.objects.all()
    serializer_class = CourseSerializer
    def perform_create(self, serializer):
        serializer.save(teacher=self.request.user)
```

按住`Ctrl`点击 `ModelViewSet`，可以查看源代码
![](resources/2024-02-04-15-54-09.png)

按住`Ctrl`点击 `GenericViewSet`，可以查看源代码
![](resources/2024-02-04-15-54-56.png)

##### 配置路由

使用 ViewSet 类实现接口，配置路由的方法与上面的 FBV、CBV、GCBV 均不同

###### 手动绑定

修改 **drf_tutorial/course_api/urls.py** 文件的内容为：
```py
from django.urls import path, include
from course_api import views

urlpatterns = [
    # FBV
    # path("fbv/list", views.course_list, name="fbv-list"),
    # path("fbv/detail/<int:pk>", views.course_detail, name="fbv-detail"),
    # CBV
    # path("cbv/list", views.CourseList.as_view(), name="cbv-list"),
    # path("cbv/detail/<int:pk>", views.CourseDetail.as_view(), name="cbv-detail"),
    # GCBV
    # path("gcbv/list", views.GCourseList.as_view(), name="gcbv-list"),
    # path("gcbv/detail/<int:pk>", views.GCourseDetail.as_view(), name="gcbv-detail"),
    # DRF viewsets
    path("viewsets", views.CourseViewSet.as_view(
        {"get": "list", "post": "create"}
        ), name="viewsets-list"),
    path("viewsets/<int:pk>", views.CourseViewSet.as_view(
        {"get": "retrieve", "put": "update", "patch": "partial_update", "delete": "destroy"}
        ), name="viewsets-detail"),
]
```

和 FBV、CBV、GCBV 中配置路由的区别是，这里是**同一个类视图，对应两个不同的URL**
所以还需要，**指定要调用视图的哪些方法**，给`as_view()`方法传入一个字典，字典的 `key` 是 **HTTP方法**，`value` 则是该方法对应的 **视图方法**

按住`Ctrl`点击 `ModelViewSet`，可以查看源代码
![](resources/2024-02-04-15-54-09.png)

按住`Ctrl`点击 `ListModelMixin`，可以查看源代码
![](resources/2024-02-04-16-19-47.png)
这里可以看到 `list()` 方法

举个例子，`"get": "list"` 中的：
- `get` 指的是 HTTP 的 GET方法
- `list` 指的是 视图集 `CourseViewSet` 的父类 `ModelViewSet` 的父类 `ListModelMixin` 中的 `list()` 方法

使用 Postman 来测试接口 http://127.0.0.1:8000/course/viewsets

测试 **GET方法**

![](resources/2024-02-04-16-29-02.png)

测试 **POST方法**

![](resources/2024-02-04-16-30-30.png)

使用 Postman 来测试接口 http://127.0.0.1:8000/course/viewsets/6

测试 **GET方法**

![](resources/2024-02-04-16-34-46.png)

测试 **PUT方法**

![](resources/2024-02-04-16-35-43.png)

测试 **PATCH方法**

![](resources/2024-02-04-16-36-56.png)

测试 **DELETE方法**

![](resources/2024-02-04-16-37-22.png)

###### 自动绑定（使用 DefaultRouter）

这样实现起来，代码量少很多

修改 **drf_tutorial/course_api/urls.py** 文件的内容为：
```py
from django.urls import path, include
from course_api import views
from rest_framework.routers import DefaultRouter

router = DefaultRouter()
router.register(prefix="viewsets", viewset=views.CourseViewSet)

urlpatterns = [
    # FBV
    # path("fbv/list", views.course_list, name="fbv-list"),
    # path("fbv/detail/<int:pk>", views.course_detail, name="fbv-detail"),
    # CBV
    # path("cbv/list", views.CourseList.as_view(), name="cbv-list"),
    # path("cbv/detail/<int:pk>", views.CourseDetail.as_view(), name="cbv-detail"),
    # GCBV
    # path("gcbv/list", views.GCourseList.as_view(), name="gcbv-list"),
    # path("gcbv/detail/<int:pk>", views.GCourseDetail.as_view(), name="gcbv-detail"),
    # DRF viewsets
    # path("viewsets", views.CourseViewSet.as_view(
    #     {"get": "list", "post": "create"}
    #     ), name="viewsets-list"),
    # path("viewsets/<int:pk>", views.CourseViewSet.as_view(
    #     {"get": "retrieve", "put": "update", "patch": "partial_update", "delete": "destroy"}
    #     ), name="viewsets-detail"),
    path("", include(router.urls)),
]
```

使用 Postman 来测试接口 http://127.0.0.1:8000/course/viewsets
效果不变

使用 Postman 来测试接口 http://127.0.0.1:8000/course/viewsets/6
效果不变

## 认证（authentication）








## 权限（permission）













---
---



this:
djangoREST
https://www.bilibili.com/video/BV1Dm4y1c7QQ/


开始：P17
P22已完成



剩 P1 演示postman
P3 postman github API
P4 配置文件 static
跳过 P9



教学资源
https://github.com/liaogx/drf-tutorial



---


djangoREST2
https://www.bilibili.com/video/BV1k5411p7Kp

P5


