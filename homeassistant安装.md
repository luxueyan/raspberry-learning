## ha智能家居的准备工作
1.安装ha，[https://home-assistant.cc/installation/raspberrypi/raspbian/]()
2.使用上面链接的方法2，其中安装python3后，更改国内镜像源之后在进行ha的安装

sudo nano /etc/pip.conf

将文件内容更改为：

```
[global]
timeout = 6000
index-url = http://mirrors.aliyun.com/pypi/simple/
extra-index-url=https://www.piwheels.org/simple/
[install]
use-mirrors = true
mirrors = http://mirrors.aliyun.com/pypi/simple/
trusted-host = mirrors.aliyun.com
```

成功之后 执行hass命令，然后访问 http://树莓派ip:8123
传说中的微信小程序molohub已经打不开。幸好我装树莓派的时候已经搭建好vpn网络 ，可以远程访问控制ha