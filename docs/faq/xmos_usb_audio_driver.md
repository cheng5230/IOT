---
title: XMOS USB audio 驱动说明
---


# XMOS USB audio 驱动说明

--8<-- "common/phaten_xmos_support_img.md"

## 概述 
XMOS方案的所有产品的一个共同点就是使用USB UAC2.0协议来进行音频的传输。用户需要做USB声卡相关产品的时候需要了解XMOS USB Audio相关的驱动，本文主要讲解 USB Audio 驱动相关说明。

## XMOS USB Audio 方案介绍
XMOS USB Audio 方案的协议中兼容了 USB Audio Class 1.0（UAC 1.0）和 USB Audio Class 2.0（UAC 2.0）协议，XMOS 的USB UAC音频传输协议都是完全遵循USB IF(USB 国际联盟)的UAC2.0和UAC1.0的协议标准。

其中XMOS的USB Audio方案，能够实现的功能如下表:


| 接口 | 功能参数 |
| :---- | :------- |
| USB | USB 2.0 (Full-speed and High-speed)<br>USB Audio Class 1.0<br>USB Audio Class 2.0<br>USB Firmware Upgrade (DFU) 1.1<br>USB Midi Device Class 1.0 |
| audio | S/PDIF<br>ADAT<br>Direct Stream Digital (DSD)<br>PDM Microphones<br>DSD Over PCM<br>MIDI |
| sample rate | 44.1, 48, 88.2, 96, 176.4, 192, 352.8, 384, 705.6, 768 KHz |
| bit deepth | 16 bit, 24 bit, 32 bit |

其中用户通常都使用XMOS 的USB Audio方案的 UAC2.0 high-speed（高速）协议以实现高采样率和多通道的音频规格需求，也才能体现出XMOS芯片方案的USB Audio的高性价比。其中UAC 1.0和UAC 2.0在传输速率和带宽的情况如下表，

UAC 1.0 传输速率为 12Mbit/s
UAC 2.0 最高传输速率达到480Mbit/s
而XMOS的USB Audio的方案中，UAC 1.0和UAC 2.0的大体的区别如下表


| 功能                        | UAC 1.0             | UAC 2.0                             |
| :--------------------------- | :------------------- | :----------------------------------- |
| 应用场合                    | 立体声、普通声卡方案    | HiFi、多通道、专业声卡                  |
| Mac OS、Android、Linux、IOS | 免驱动安装               | 免驱动安装                              |
| Windows（Win7/8/10）        | 免驱动安装               | 需安装UAC 2.0驱动                       |
| 通道数量                    | 立体声（stereo）         | 多通道（multichannel）                |
| 功能接口                    | Stereo立体声输入输出     | 多通道、SPDIF、MIDI、ADAT输入输出        |
| 最高采样率                  | PCM 96KHz@16 bit   | PCM 768KHz@32 bit & DSD Native512    |


由上表所述，其中需要重点说明的是：

UAC 2.0仅在在Windows系统是需要安装USB驱动的, 在MAC OS, Linux 和安卓都是免安装驱动的
用户产品如果有在Windows系统上使用，则需要购买UAC 2.0驱动windows的安装程序
使用XMOS的UAC 2.0 方案才能支持多通道，高采样率， SPDIF、MIDI等功能接口

## Windows系统UAC 2.0 驱动说明
使用XMOS的UAC 2.0 方案时，XMOS官方推荐了几家windows系统驱动程序提供商，较多使用UAC 2.0驱动提供商为Thesycon 的驱动程序。根据XMOS官方文档 [USB Audio 2.0 Driver for Windows - Overview](https://www.xmos.ai/download/USB-Audio-2.0-Driver-for-Windows---Overview(3.34.0).pdf) 的说明，用户在XMOS USB Audio项目进行中，会有可能使用到Thesycon的几个驱动：Thesycon评估版驱动（Evaluation driver），XMOS 立体声驱动（XMOS Stereo Driver），Thesycon商业版驱动。其三种的区别情况如下图描述：


|    -        | Evaluation Driver | XMOS stereo Driver | Thesycon Driver |
| :---------- | :----------------- | :------------------ | :--------------- |
| 获取方式   | 免费获取            | 不再提供             | 付费购买          |
| 最大 PCM 支持 | 768 KHz           | 768 KHz            | 768 KHz          |
| DSD 支持  | DOP & Native DSD  | DOP                | DOP & Native DSD |
| MIDI      | 支持               | 不支持              | 支持             |
| 通道数支持 | Multichannel      | Stereo             | Multichannel     |


### Thesycon评估版驱动（Evaluation driver）
用户在使用我们的Hi-Fi评估板，就可以下载评估板驱动进行[Thesycon评估版驱动(Evaluation driver)](../../assets/download/Thesycon-USB-Audio-Class-2_0-Evaluation-Driver-for-Windows_5_58_0.zip)测试评估，评估板驱动，支持 SPDIF、MIDI、multichannel，以及Stereo 立体声输出 PCM 768KHz 与 DSD Native512。但是评估版驱动驱动存在一个限制商业化，即在设备上电持续播放一小时以后，每隔五分钟会有一个嘟嘟声。该评估版驱动还没授权来进行商用化使用，仅提供给用户测试使用。

### XMOS 立体声驱动（XMOS Stereo Driver）
不在提供

### Thesycon 驱动（Thesycon Driver）
Thesycon 驱动为商业化授权驱动，用户可以先使用Thesycon评估版驱动（Evaluation driver）后，测试产品功能性能没有问题之后，确定需要购买Thesycon 驱动（Thesycon Driver）的情况下，用户需自行联系驱动方Thesycon进行商务合作，包括功能需求，License 费用等。

飞腾云不参与用户与Thesycon或者其他第三方驱动公司的商务商谈。Thesycon为XMOS官方推荐合作的其中一家驱动公司，如需了解相关驱动信息，可到Thesycon官网查看.

### USB 驱动 VID 与 PID说明
USB 设备有VID 和PID 描述符，其中VID：供应商描述码 ID，； PID：产品描述码。VID和PID都是有USB IF根据品牌注册分配使用的。

在使用XMOS USB Audio的方案时，可以选择使用Thesycon分配的VID和PID ，也可以选择自己的VID 和PID(前提得在USB IF注册，且拿到证书)。
