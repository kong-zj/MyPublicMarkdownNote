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
`http://demo.com/create_new_book?name=shuxue`
`http://demo.com/update_book?id=1&name=shuxue`
`http://demo.com/delete_book?id=1`

这是4个 HTTP API，分别实现了图书的查看、新增、编辑、删除的操作，当我们把接口发布出去的时候，别人就可以通过这四个接口来调用相关的服务了。但是这样做有什么不方便的地方呢？你可能发现了，这种API的写法有一个缺点，那就是没有一个统一的风格，比如说第一个接口表示查询全部图书的信息，我们也可以写成这样：
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
- PATCH：在服务器更新(更新)资源（客户端只提供需要改变的属性）
- **DELETE**：从服务器删除资源
- OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的
- HEAD：获取资源的元数据（描述数据的数据）
- TRACE：回显服务器收到的请求，主要用于测试或诊断
- CONNECT：HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器

### RESTful API 举例

REST 的作用是将我们上面提到的查看（view），创建（create），编辑（edit），删除（delete）直接映射到 HTTP 中已实现的 GET，POST，PUT，DELETE 方法

我们重新将上面的四个接口改写成REST风格

查看所有图书：
- GET http://demo.com/books

新增一本书：
- POST http://demo.com/books
- Data: name=shuxue

修改一本书：
- PUT http://demo.com/books
- Data: id=1,name=shuxue

删除一本书：
- DELETE http://demo.com/books
- Data: id=1

这样改动之后API变得统一了，我们只需要改变请求方式就可以完成相关的操作，这样大大简化了我们接口的理解难度，变得易于调用，这就是REST风格的意义

### HTTP 状态码

REST 的另一重要部分就是为既定好请求的类型来响应正确的状态码。当你请求 HTTP 时，服务器会响应一个状态码来显示你的请求是否成功，然后客户端应如何继续。以下是四种不同层次的状态码：
- 2xx = Success（成功）
- 3xx = Redirect（重定向）
- 4xx = User error（客户端错误）
- 5xx = Server error（服务器端错误）

我们常见的是 200（请求成功）、404（未找到）、401（未授权）、500（服务器错误）

### API 格式响应

上面介绍了REST API的写法，响应状态码，剩下就是请求的数据格式以及响应的数据格式。说的通俗点就是，我们用什么格式的参数去请求接口并且我们能得到什么格式的响应结果

我这里只介绍一种用的最多的格式：JSON格式
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

# Django REST 框架入门

目标：使用 DRF 开发 RESTful API 接口（4种实现方式）
效果：用DRF的多种视图，实现课程信息的增删改查

## 项目配置



















## 序列化（serializers）
















## 视图集（viewsets）
















## 路由（routers）















## 认证（authentication）








## 权限（permission）













---
---



this:
djangoREST
https://www.bilibili.com/video/BV1Dm4y1c7QQ/


djangoREST2
https://www.bilibili.com/video/BV1k5411p7Kp




P4



剩 P1 演示postman
P3 postman github API


教学资源
https://github.com/liaogx/drf-tutorial
