# PHP

[PHP教程](https://www.runoob.com/php/php-tutorial.html)

# ThinkPHP 5.1

## 安装

确保已经安装apache2、php7.4、mysql

接着安装composer
```shell
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```
![](resources/2023-02-05-15-50-45.png)

由于在安装过程中，国内访问composer的速度比较慢，可以使用国内镜像
```shell
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```

然后安装ThinkPHP 5.1
```shell
cd /var/www/html
sudo composer create-project topthink/think=5.1.x helloworld
```
注意：apache和nginx中部署的网页文件都放在同一个目录/var/www/html
![](resources/2023-02-05-17-58-57.png)

开启apache服务
```shell
sudo service apache2 start
```
注意：之前，我为了apache的端口和nginx的端口不冲突，把apache的端口号配置为90

访问```http://localhost:90/helloworld/public/```，成功
![](resources/2023-02-05-18-02-02.png)

可以使用php自带webserver快速测试，切换到根目录后，启动命令：```php think run```










