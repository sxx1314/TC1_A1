## 高能预警 ##
fork的版本号较低，可能存在不少bug，请充分测试后再用于实际场景。

## 说明 ##
基于[斐讯TC1智能排插固件zTC1](https://github.com/a2633063/zTC1/tree/v0.10)进行精简的固件，详细说明[传送门](https://ljr.im/articles/fibonacci-tc1-firmware-lite/)。

###1. 改动说明
基于原固件v0.10进行修改：

设定WIFI和MQTT设置直接烧录，懒得再装APP了
去掉UDP通信功能，只保留MQTT通信功能
去掉插座命名、定时功能，这块用Home Assistant来管理
MQTT通信协议根据自己使用习惯进行了调整：调整topic、增加上线下线通知、使用qos
把编译的warning警告基本去掉了
WARING：因为精简掉UDP通信协议，使用上会造成不方便，精简带来的好处实际也不明显，请自行权衡。

###2. 编译环境
使用docker。

- 拉一个python2.7的镜像

`docker pull python:2-slim`

- 启动容器并进入容器

- 自己调整主机映射目录/home/mico；容器内工程目录设置为/workdir，可根据喜好调整，但注意后续命令也要调整。
```docker run -it --name mico -v /home/mico:/workdir python:2-slim bash
```
DEBUG：后续进入容器可以用命令docker exec -it mico bash，更多docker命令介绍传送门。

- 更新源、安装软件

```
#容器bash
apt update && apt install git wget lib32ncurses5
```
INFO：一个坑，不安装lib32ncurses5库编译会报"arm-none-eabi-gcc: not found"错误。

- python bin

```
ln -s /usr/local/bin/python /usr/bin/python
```

INFO：一个坑，不设置软链接编译会报"/usr/bin/python: not found"错误。

- 安装MiCo编译环境（mico-cube、MiCoder）

```
#容器bash
pip install mico-cube && \
cd /workdir && \
wget http://firmware.mxchip.com/MiCoder_v1.1.Linux.tar.gz && \
tar -zxf MiCoder_v1.1.Linux.tar.gz && \
rm MiCoder_v1.1.Linux.tar.gz && \
mico config --global MICODER /workdir/MiCoder
```
DEBUG：mico-cube是MXCHIP的MiCO项目开发管理工具包；MiCoder是MiCO编译和调试系统必须的工具软件包。

###3.编译固件

- 创建一个空项目，名为TC1，自己开发则自定义

```
#容器bash
cd /workdir && \
mico new TC1 --create-only
```
INFO：命令执行完后，会在当前目录生成名称为TC1项目目录。

- 添加mico-os组件

```
#容器bash
cd /workdir/TC1 && \
mico add https://code.aliyun.com/mico/mico-os.git/#6c465211d3ff8797cd835e400ec54a06530dd476
```
INFO：需要在项目目录下执行。

- 添加项目代码，代码目录在项目目录下的项目同名目录，/workdir/TC1/TC1，自己开发则自定义
```
#容器bash
git clone https://github.com/cnk700i/tc1_mqtt.git && \
mv tc1_mqtt/TC1 . && \
rm tc1_mqtt -r
```
DEBUG：下载zip解压后再拷贝进去也可以。

- 设置WiFi及MQTT，修改项目代码main.h文件

```
//自定义
#define CONFIG_SSID "wifi_ssid"                 //WiFi名称
#define CONFIG_USER_KEY "wifi_password"         //WiFi密码
#define CONFIG_MQTT_IP "mqtt_ip"                //MQTT服务器IP
#define CONFIG_MQTT_PORT 1883                   //MQTT服务器端口     
#define CONFIG_MQTT_USER "mqtt_user"            //MQTT用户名
#define CONFIG_MQTT_PASSWORD "mqtt_password"    //MQTT密码
#define STATE_UPDATE_INTERVAL 10000             //功率上报间隔，单位ms
#define MQTT_CLIENT_SUB_TOPIC   "cmnd/%s"       //命令控制接收topic，%s代表名称，默认tc1_xxxxxxxxxxxx（xxx为mac地址）
#define MQTT_CLIENT_PUB_TOPIC   "stat/%s"       //状态信息topic，%s代表名称，默认tc1_xxxxxxxxxxxx（xxx为mac地址）
```

- 编译项目代码

```
#容器bash
#/workdir/TC1
mico make TC1@MK3031@moc
```

###4.刷固件
- 编译成功后的固件文件

```
/workdir/TC1/build/TC1@MK3031@moc/binary/TC1@mailto:MK3031@moc.all.bin，用于线刷
/workdir/TC1/build/TC1@MK3031@moc/binary/TC1@mailto:MK3031@moc.ota.bin，用于OTA
```

INFO：主机映射目录/home/mico/TC1/build/TC1@MK3031@moc/binary内可找到固件。

- 线刷方法见原固件教程的固件烧录
- OTA方法见原固件教程的通信协议
###5.小结
- MiCo开发平台比之前DC1用的ESPHome容易上手好多，搭建环境也简单些，遇到一些小坑还是比较快就解决了。
- C语言看着真的头大，又感觉到了被指针支配的恐惧，还好原固件作者代码的业务逻辑还是比较清晰，于是就简单地改一改。
