


## 初试 API（shell）

通过以下命令打开 Python 命令行：
```sh
python3 manage.py shell
```

> 我们使用这个命令而不是简单的使用"python3"是因为 manage.py 会设置 DJANGO_SETTINGS_MODULE 环境变量，这个变量会让 Django 根据 mysite/settings.py 文件来设置 Python 包的导入路径

在shell中可进行如下操作：
![](resources/2023-10-29-17-46-24.png)

上图中的
```sh
<Question: Question object (1)>
``` 
对于我们了解这个对象的细节没什么帮助。让我们通过编辑 Question 模型的代码（位于 polls/models.py 中）来修复这个问题
即给 Question 和 Choice 增加 \_\_str\_\_() 方法：
```py
import datetime
from django.db import models
from django.utils import timezone

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField("date published")
    # __str__() 方法
    def __str__(self):
        return self.question_text
    # 添加一个自定义方法
    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
    # __str__() 方法
    def __str__(self):
        return self.choice_text
```

给模型增加 \_\_str\_\_() 方法是很重要的，这不仅仅能给你在命令行里使用带来方便，Django 自动生成的 admin 里也使用这个方法来表示对象

重新运行如下命令：
```sh
python3 manage.py shell
```

在shell中可进行如下操作：
![](resources/2023-10-29-18-06-27.png)

## Django 管理页面

Django 可以全自动地根据模型创建后台界面
管理界面不是为了网站的访问者，而是为管理者准备的

### 创建一个管理员账号（createsuperuser）

```sh
python3 manage.py createsuperuser
```

![](resources/2023-10-29-18-15-07.png)
这里密码设置为 123456

### 启动开发服务器（runserver）

Django 的管理界面默认就是启用的。让我们启动开发服务器，看看它到底是什么样的

```sh
python3 manage.py runserver
```

进入 http://127.0.0.1:8000/admin/
![](resources/2023-10-29-18-20-01.png)

### 进入管理站点页面

登陆后，你将会看到 Django 管理页面的索引页：
![](resources/2023-10-29-18-21-52.png)

你将会看到几种可编辑的内容：组和用户。它们是由 django.contrib.auth 提供的，这是 Django 开发的认证框架

### 向管理页面中加入投票应用

但是我们的投票应用在哪呢？它没在索引页面里显示

只需要再做一件事：我们得告诉管理，问题 Question 对象需要一个后台接口
修改polls/admin.py文件的内容为：
```py
from django.contrib import admin
from .models import Question

admin.site.register(Question)
```

### 体验便捷的管理功能

现在我们向管理页面注册了问题 Question 类。Django 知道它应该被显示在索引页里：
![](resources/2023-10-29-18-31-36.png)

点击 Questions
![](resources/2023-10-29-18-37-05.png)

选择一条 Question 做修改
![](resources/2023-10-29-18-38-07.png)

可以查看修改的历史记录
![](resources/2023-10-29-18-39-14.png)

