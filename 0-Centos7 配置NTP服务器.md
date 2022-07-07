# Centos7 配置NTP服务器



## 一、安装NTP服务

### 1、在线安装

```
# 有网的情况下找开命令窗口，输入下面命令安装

yum install ntp ntpdate -y
```

### 2、离线安装

- 将三个安装文件拷贝到服务器中

- 运行以下命令

  ```
  rpm -ivh autogen-libopts-5.18-5.el7.x86_64.rpm
  rpm -ivh ntpdate-4.2.6p5-29.el7.centos.2.x86_64.rpm
  rpm -ivh ntp-4.2.6p5-29.el7.centos.2.x86_64.rpm
  ```



## 二、修改NTP配置

在命令窗口执行

```
vi /etc/ntp.conf
```

定位到21行左右，按`i`进入编辑模式，将第一个`server xxxxx`内容改成下面这样，后面几个`server`前面加#注释掉

```
# Use public servers from the pool.ntp.org project.
# Please consider joining the pool (http://www.pool.ntp.org/join.html).
server 127.127.1.0 #local clock
fudge 127.127.1.0 stratum 3
#server 1.centos.pool.ntp.org iburst
#server 2.centos.pool.ntp.org iburst
#server 3.centos.pool.ntp.org iburst

```

确认无误后，按`ESC`退出编辑模式，再输入`:wq`保存并退出文件。



## 三、防火墙设置

```
firewall-cmd --permanent --zone=public --add-service=ntp
firewall-cmd --reload
```



## 四、启动服务

```
# 启动服务
systemctl start ntpd

# 查看服务运行状态
systemctl status ntpd

# 设置开机自启
systemctl enable ntpd.service
```



## 五、检查服务

命令窗口输入`timedatectl`

```
[root@localhost ~]# timedatectl
      Local time: Thu 2022-07-07 17:43:20 CST
  Universal time: Thu 2022-07-07 09:43:20 UTC
        RTC time: Thu 2022-07-07 09:43:20
       Time zone: Asia/Shanghai (CST, +0800)
     NTP enabled: yes
NTP synchronized: yes
 RTC in local TZ: no
      DST active: n/a
```

如果`NTP synchronized`是`yes`，说明配置NTP服务成功。

