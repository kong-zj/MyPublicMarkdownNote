# 数据库与模型

## 连接数据库

![](resources/2023-02-08-22-23-07.png)

```php
    // 数据库类型
    'type'            => 'mysql',
    // 服务器地址
    'hostname'        => '127.0.0.1',
    // 数据库名
    'database'        => 'grade_learn_tp',
    // 用户名
    'username'        => 'root',
    // 密码
    'password'        => '1',
    // 端口
    'hostport'        => '3306',
    // 连接dsn
    'dsn'             => '',
    // 数据库连接参数
    'params'          => [],
    // 数据库编码默认采用utf8
    'charset'         => 'utf8',
    // 数据库表前缀
    'prefix'          => 'tp_',
    // 数据库调试模式
    'debug'           => true,
```

注意数据库表前缀的配置
![](resources/2023-02-08-22-42-32.png)

在controller目录下新建DataTest.php文件，内容如下
```php
<?php
namespace app\controller;
use think\Controller;
use think\Db;

class DataTest extends Controller
{
    public function index()
    {
        return phpinfo();
        // return 'index';
    }

    public function getNoModelData()
    {
        // 用table的话，需要加前缀
        // $data = Db::table("tp_user")->select();
        // 用name的话，默认帮你加前缀
        $data = Db::name("user")->select();
        return json($data);
    }
}
```

访问```localhost:90/helloworld/public/index.php/data_test/getNoModelData```，报错
![](resources/2023-02-08-23-29-47.png)

解决方法：
[出现could not find driver的解决方法](https://blog.csdn.net/qq_43175099/article/details/84329738)

直接用```phpinfo()```函数查看
![](resources/2023-02-08-23-33-28.png)
发现是PDO驱动没有安装mysql数据库驱动

使用命令```php -m```查看已安装的模块
找不到已安装的mysql驱动
![](resources/2023-02-08-23-38-18.png)

使用命令```sudo apt-cache madison [软件名]```查找可安装的软件
安装```php7.4-mysql```
![](resources/2023-02-08-23-51-07.png)

安装成功
![](resources/2023-02-08-23-52-36.png)

还需要使用命令```sudo service apache2 restart```重启apache2服务

再次访问```localhost:90/helloworld/public/index.php/data_test/getNoModelData```，成功
![](resources/2023-02-08-23-55-59.png)

以上是不结合module来处理的，以下结合module

## 模型定义

![](resources/2023-02-09-22-04-05.png)
User.php文件的内容如下
```php
<?php
namespace app\model;
use think\Model;

class User extends Model
{
    
}
```

创建完User模型后，控制器端DataTest.php文件可以这样写：
```php
<?php
namespace app\controller;
use app\model\User;
use think\Controller;
use think\Db;

class DataTest extends Controller
{
    public function index()
    {
        return phpinfo();
        // return 'index';
    }

    public function getNoModelData()
    {
        // 用table的话，需要加前缀
        // $data = Db::table("tp_user")->select();
        // 用name的话，默认帮你加前缀
        $data = Db::name("user")->select();
        return json($data);
    }

    public function getModelData()
    {
        $data = User::select();
        return json($data);
    }
}
```

访问```localhost:90/helloworld/public/index.php/data_test/getModelData```，成功

其中```User::select()```执行的sql语句是什么？
![](resources/2023-02-09-22-15-44.png)
![](resources/2023-02-09-22-14-03.png)

## 查询数据

### 基本查询

![](resources/2023-02-09-22-23-27.png)
![](resources/2023-02-09-22-26-25.png)

![](resources/2023-02-09-22-28-30.png)

### 链式查询















---
Laravel

到P8




