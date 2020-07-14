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


### 添加mqtt服务，默认使用hbmqtt一直收发不到消息，折腾了一天换成mosquitto
`sudo apt-get install mosquitto mosquitto-clients`
安装启动service报错，先不用管
`sudo nano /etc/mosquitto/conf.d/mqtt.conf`
添加以下内容。

```
allow_anonymous false

password_file /etc/mosquitto/pwfile
```

设置MQTT服务的用户名和密码。

`$ sudo mosquitto_passwd -c /etc/mosquitto/pwfile mqtt`


重启后，查看并确认MQTT服务的运行状态。如果状态是失败再次启动服务

`sudo systemctl status mosquitto.service`
`sudo systemctl enable mosquitto.service`
`sudo systemctl start mosquitto.service`

打开一个终端，订阅dev/test这个主题的消息。

`$ mosquitto_sub -d -u mqtt -P mqtt -t dev/test`


再打开一个终端，向 dev/test这个主题发送消息。正常情况下，可以在第一个终端中收到”hello world”消息，表明MQTT服务运行正常。

`mosquitto_pub -d -u mqtt -P mqtt -t dev/test -m "hello world"`

