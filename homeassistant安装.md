## ha智能家居的准备工作

### 基础安装
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

### 添加开机启动


`sudo vim /lib/systemd/system/homeassistant@pi.service`

添加unit

```
[Unit]
Description=Home Assistant
After=network-online.target

[Service]
Type=simple
User=homeassistant
#ExecStartPre=source /srv/homeassistant/bin/activate
ExecStart=/bin/bash -c '\
   source /src/homeassistant/bin/activate; \
   exec /srv/homeassistant/bin/hass -c /home/homeassistant/.homeassistant'
[Install]
WantedBy=multi-user.target

```

`sudo systemctl --system daemon-reload`
`sudo systemctl enable homeassistant@pi.service`
`sudo systemctl start homeassistant@pi.service`
