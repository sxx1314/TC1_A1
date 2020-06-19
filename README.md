# zTC1 a1版
**斐讯TC1智能排插个人固件.**

排插TC1因为服务器关闭,无法使用.

为此,开发供自己使用的FW及对应app,确保自己能够正常使用此排插.取名为zTC1.


>  注意:
>
>  ​	TC1排插硬件分a1 a2两个版本,本固件仅支持**a1版本.**
>
>  ​	a1 a2两个版本仅主控不同,除此之外其他无任何区别
>
>  ​	区分硬件版本见[区分硬件版本](#区分硬件版本)









### 已知BUG

不定时重启断电!!! 

定时任务偶尔无效

为防止商用,以上bug在公开版本不修复

## 特性

本固件使用斐讯TC1排插硬件为基础,实现以下功能:

- [x] 4个USB充电(3个普通和1个快充接口)(硬件直接实现,与软件无关)
- [x] 按键控制所有插口通断
- [x] 控制每个接口独立开关
- [x] 每个接口拥有独立的5组定时开关
- [x] ota在线升级
- [x] 无服务器时使用UDP通信
- [x] MQTT服务器连接控制
- [x] 通过mqtt连入homeassistant
- [x] app实时显示功率(校准数据)
- [ ] ~~根据功率自动开关~~(未做此功能)

<img src="https://raw.githubusercontent.com/wiki/a2633063/zTC1/image/Phicomm_TC1.png" width="540">





## 目录

[前言(必看)](#前言必看)

[区分硬件版本](#区分硬件版本)

[开始](#开始)

[拆机接线及烧录固件相关](#拆机接线及烧录固件相关)

[开始使用/使用方法](#开始使用/使用方法)

[接入home assistant](#接入home-assistant)

[其他内容](#其他内容)

​	[代码编译](#代码编译)

​	[通信协议](#通信协议)





## 前言(必看)

- 除非写明了`如果你不是开发人员,请忽略此项`之类的字眼,否则,请**一个字一个字看清楚看完**整后再考虑动手及提问!很可能一句话就是你成功与否的关键!
- 烧录固件需要专用的烧录器:支持swd的jlink烧录器,目前已知便宜的价格为不到20元包邮.(本人不做烧录器的售卖,所有提供的链接或推荐都为第三方卖家,和本人无关).
- 使用此固件,需要app端配合,见[SmartControl_Android_MQTT](https://github.com/a2633063/SmartControl_Android_MQTT).
- app只有android,因ios限制,本人不考虑免费做ios开发.(不要再问是否有ios端).

> 虽然没有ios端,但固件支持homeassistant,可以使用安卓APP配置完成后,连入homeassistant后,使用ios控制. APP主要仅为第一次使用配对网络及配置mqtt服务器时使用,之后可以用homeassistant控制不再使用app.

> 如果你不知道什么是mqtt或homeassistant,所有有关的内容可以跳过.

> 如果你有任何问题,可以直接在此项目中提交issue,或给我发送邮件:zip_zhang@foxmail.com,邮件标题中请注明[zTC1].
>
> 



## 区分硬件版本

硬件版本在外包装底部,如图所示:

![hardware_version](https://raw.githubusercontent.com/a2633063/zTC1/master/README/hardware_version.png)

如果没有包装,只能[拆开](#拆机接线及烧录固件相关)分辨,如图,左侧为不支持的a2版本,右侧为支持的a1版本

![a1_a2](https://raw.githubusercontent.com/a2633063/zTC1/master/README/a1_a2.png)



## 开始

整体流程如下:拆开TC1,将固件/烧录器/pc互相连接,在pc运行烧录软件进行烧录,烧录固件.

烧录完成后,首次使用前配对网络并配置mqtt服务器,之后就可以使用了.



## 拆机接线及烧录固件相关

见[固件烧录](https://github.com/sxx1314/zTC1/wiki/固件烧录)

烧录固件完成后,即可开始使用



## 开始使用/使用方法

见[开始使用](https://github.com/sxx1314/zTC1/wiki/开始使用)



## 接入home assistant

见[homeassistant接入](https://github.com/sxx1314/zTC1/wiki/homeassistant接入)



## 其他内容

### 代码编译

> 此项为专业开发人员准备,如果你不是开发人员,请跳过此项

TC1使用的主控为EMW3031,基于MiCO(MCU based Internet Connectivity Operating System)开发.[MiCO简介点这里](http://developer.mxchip.com/handbooks/101)

需要按照官方说明才能保证此项目能够编译成功:

1. 安装[MiCO Cube编译工具](http://developer.mxchip.com/handbooks/102)
2. 配置[MICoder IDE环境](http://developer.mxchip.com/handbooks/105)
3. 配置[Jlink下载工具](http://developer.mxchip.com/handbooks/103)
4. check out 此项目,按照[从一个现有的 Git 仓库克隆导入](http://developer.mxchip.com/handbooks/102#%E4%BB%8E%E4%B8%80%E4%B8%AA%E7%8E%B0%E6%9C%89%E7%9A%84-git-%E4%BB%93%E5%BA%93%E5%85%8B%E9%9A%86%E5%AF%BC%E5%85%A5)确认项目编译/下载正常

此项目来自a2633063最后一次开源的版本。由于本人仅使用homeassistant+mqtt控制，因此参考cnk700i的代码会做少量修改。如果你也在做tc1的固件完善欢迎fork。


### 通信协议

> 此项为专业开发人员准备,如果你不是开发人员,请跳过此项

所有通信协议开源,你可以自己开发控制app或ios端

见[通信协议](https://github.com/sxx1314/zTC1/wiki/通信协议)



