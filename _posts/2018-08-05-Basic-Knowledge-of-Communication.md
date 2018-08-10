---
layout:     post
title:      通信基础
subtitle:   TCP/IP模型五层通信机制以及各种设备收发数据处理过程
date:       2018-08-05
author:     Miki
header-img: img/post-bg-communication.jpg
catalog: true
tags:
    - HCIE
    - OSI
    - TCP/IP
    - 通信协议	
---


> 本文首次发布于 [Mlin Blog](http://happymiki.github.io)、[简书](http://www.jianshu.com/u/3f05018752b8)、[CSDN](https://blog.csdn.net/qq_34104227)，作者 [@木林(Mlin)](http://github.com/happymiki) ,转载请保留原文链接。

[TOC]

# 前言

> OSI模型，即开放式通信系统互联参考模型(Open System Interconnection,OSI/RM,Open Systems InterconnectionReference Model)，是国际标准化组织(ISO)提出的一个试图使各种计算机在世界范围内互连为网络的标准框架，简称OSI。 

# 正文

## 一、数据通信过程

### 1 标准化组织

#### 1.1 国际标准化组织（ISO）

- 国际标准化组织（ISO，International Organization for Standardization）。
- 该组织负责制定大型网络的标准，包括与Internet相关的标准。**ISO提出了OSI参考模型**。OSI参考模型描述了网络的工作机理，为计算机网络构建了一个易于理解的、清晰的层次模型。

#### 1.2 电子电器工程师协会（IEEE）

- 电子电器工程师协会（IEEE，Institute of Electrical and Electronics Engineers）
- 提供了网络硬件上的标准使各种不同网络硬件厂商生产的硬件设备相互连通。IEEE LAN标准是当今居于主导地位的LAN标准。它主要定义了802.X协议族，其中802.3为以太网标准协议簇、802.4为令牌总线网（Toking Bus）标准、802.5为令牌环网（Toking Ring）标准、802.11为无线局域网（WLAN）标准。

#### 1.3 美国国家标准局（ANSI）

- 美国国家标准局（ANSI，American National Standards Institute）
- ANSI是由公司、政府和其他组织成员组成的自愿组织，主要定义了光纤分布式数据接口（FDDI）的标准。

#### 1.4 国际电信联盟（ITU）

- 国际电信联盟（ITU，International Telecomm Union）
- 定义了作为广域连接的电信网络的标准，如X.25、Frame Relay等。

#### 1.5 电子工业协会（EIA / TIA）

- 电子工业协会（EIA/TIA，Electronic Industries Association/Telecomm Industries Association）
- 定义了网络连接线缆的标准，如RS232、CAT5、HSSI、V.24等。同时定义了这些线缆的布放标准如EIA/TIA 568B。

#### 1.6 INTERNET架构委员会（IAB）

- INTERNET架构委员会（IAB：Internet Architectrue Board）
- 下设工程任务委员会（IETF）、研究任务委员会（IRTF）、号码分配委员会（IANA）负责各种INTERNET标准的定义，是目前最具影响力的国际标准化组织。 

### 2 网络通信协议

- 下图为常用网络通信协议结构图。

![](https://www.zhoulujun.cn/zhoulujun/uploadfile/2016/0316/20160316144020407.jpg)

### 3 为什么使用分层模型

![](https://upload-images.jianshu.io/upload_images/6757403-69783245bf2ea041.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 降低复杂性
- 标准化接口API
- 简化模块化设计
- 确保技术的互操作性
- 加快发展速度
- 简化教学

### 4 OSI七层模型结构

![](https://upload-images.jianshu.io/upload_images/6757403-5b69e901a1cb72d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 1.1 物理层（Physical Layer）

- 二进制传输，为启动、维护以及关闭物理链路，**定义了电气规范、机械规范、过程规范和功能规范**，即定义了传输介质。
- 在这一层，数据的单位称为**比特（bit）**。
- 示例：802.3、EIA/TIA RS-232、EIA/TIA RS-449、V.35、RJ-45、fddi令牌环网等。

#### 1.2 数据链路层（Data Link Layer）

- 数据链路层在不可靠的物理介质上提供可靠的传输。该层定义了如何格式化数据以便进行传输以及如何控制网络访问。该层的作用包括：物理地址寻址、数据的成帧、流量控制、数据的检错、重发等。
- 在这一层，数据的单位称为**帧（frame）**。
- 示例： ARP、RARP、SDLC、HDLC、PPP、STP、帧中继（FR）等。

#### 1.3 网络层（Network Layer）

- 路由数据包
- 选择传递数据的最佳路径
- 支持逻辑寻址和路由选择
- 在这一层，数据的单位称为**数据包（packet）。**
- 示例： **IP** (Internet Protocol)、**IPX**(Internet work Packet Exchange)、**DDP** (Datagram Delivery Protoco1)     **ICMP**（Internet Control Message Protocol）、APPLETALK。

#### 1.4 传输层（Transport Layer）

- 传输层的功能就是**建立端口到端口的通信**，网络层就是建立主机与主机的通信。
- 在传输层有两个非常重要的协议：UDP 协议和TCP协议。
  - TCP：面向连接，应用于对数据完整性，一对一通信。
    - 确保数据传输的可靠性
    - 建立、维护和终止虚拟电路
    - 通过错误检测和恢复
    - 信息留空来保障可靠性
  - UDP：无连接，应用于实时性传输、数据量大，一对多通信。
- 示例：**TCP**(Transmission Control Protocol)、**UDP** (User Datagram Protocol）、**SPX**(SequenCed Packet ExChange Protocol)等。 **ATP**(AppleTalk Transaction Protocol)，**NBP**(名字绑定协议)  **NetBEUI**(NetBIOS Extended User Internet)。

#### 1.5 会话层（Session Layer）

- 会话层定义了如何开始、控制和结束一个会话。**会话层管理主机之间的会话进程，即负责建立、管理、终止进程之间的会话**。会话层还利用在数据中插入校验点来实现数据的同步。
- 示例：**SSH**（Secure Shell）、**ZIP**（Zone Information Protocol）、**SDP** （Sockets Direct Protocol）、**ADSP**（AppleTalk的数据流协议）、**ASP**（AppleTalk的动态会话协议）、**H.245** （Call Control Protocol for Multimedia Communication）。

#### 1.6 表示层（Presentation Layer）

- 表示层的主要功能是**定义数据格式、加密及压缩**。表示层对上层数据或信息进行变换以保证一个主机应用层信息可以被另一个主机的应用程序理解。
- 示例：**EBCDIC**（extended binary coded decimal interchange code）、**ASCII**（Amercia Standard Code for Information Interchange）；
  - 图像标准：**JPEG**（Joint Photographic Experts Group）、**TIFF**（Tagged Image File Format）、**GIF**。
  - 视频标准：**MIDI**（Musical Instrument Digital Interface）、**MPEG**（Motion Picture Experts Group）、**QuickTime**等。 

#### 1.7 应用层（Application Layer）

- 应用层主要功能是**为操作系统或网络应用程序提供访问网络服务的接口，也就是提供给用户的接口**。

- 通过应用程序来完成网络用户的应用需求。该层的数据放在TCP数据包的数据部分，该层定义了一个很重要的协议——http协议。

- 示例： **Telnet**(远程登录协议)、**FTP** (File Transfer Protocol)、**HTTP** (Hyperrext Transfer Protocol)、**SNMP**(simple Mail Transfer Protocol)、 **BOOTP**(Boot trap．Protocol) 、**AFP**（Apple Talk文件协议--Apple公司的网络协议族，用于交换文件）、 **SNMP** (Simple Network Management Protoco1)、

  **NCP** (NetWare Core Protoco1) 、**NFS** (Network File System)。

### 5 OSI七层模型之间通信过程

#### 5.1 数据封装过程

![](https://upload-images.jianshu.io/upload_images/6757403-108ae2f59208f52d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 首先由应用层提供给用户访问接口，用户使用应用程序产生数据流量。
- 再通过表示层的数据格式化、加密和压缩等处理后，交给会话层。
- 会话层你通过在不同的应用程序会话（进程）中处理，以区分不通的上层应用，再交给传输层。
- 传输层拿到数据，首先区分TCP还是UDP。打上传输层包头后交给网络层。
  - TCP：与对端建立端口到端口的连接，首先在目的端口封装上目的端口号，源端口号采用大于1024的任意数字。与对端通过三次握手建立可靠的连接。
  - UDP：直接封装上目的端口号和源端口。
- 网络层拿到数据，通过封装源IP和目的IP，给数据打上IP报头，再交给数据链路层。
- 数据链路层拿到数据，首先查看MAC地址表，封装上相应出接口的目的MAC地址和源MAC地址，再交给物理层。
- 物理层通过接口和线缆介质，以比特流的形式发送出去。

#### 5.2 数据解封装过程

![](https://upload-images.jianshu.io/upload_images/6757403-4ba4c93b0a47b824.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

>  数据解封装过程和封装过程相反。

#### 5.3 主机间对等通信过程

![](https://upload-images.jianshu.io/upload_images/6757403-d53f08746bcd906f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 6 TCP/IP协议栈

![](https://upload-images.jianshu.io/upload_images/6757403-bca12b0d85d59ff3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- TCP/IP在OSI模型上进行了缩减成了5层模型。

## 二、承载数据帧的格式

### 1 数据链路层

#### 1.1 以太网链路

##### 1.1.1 Ethernet II

- Ethernet Ⅱ帧格式

![](https://upload-images.jianshu.io/upload_images/6757403-441a7a9f86e700cd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

| 字段 | 长度  | 含义                                                         |
| ---- | ----- | ------------------------------------------------------------ |
| DMAC | 6字节 | 目的MAC地址，IPv4为6字节，该字段确定帧的接收者。             |
| SMAC | 6字节 | 源MAC地址，IPv4为6字节，该字段确定帧的接受者。               |
| Type | 2字节 | 协议类型，表明外层协议。                                     |
| Data | 变长  | 数据字段，最小长度必须为46字节，最大长度为1500字节。                                                                                                                                                                                     如果填入该字段的信息少于46字节，该字段的其余部分也必须进行填充。 |
| CRC  | 4字节 | 用于帧内后续字节差错的循环冗余检验（也称为FCS或帧检验序列）。 |

- 各Type值对应的协议

| Type值 | 对应协议       |
| ------ | -------------- |
| 0x0800 | IP             |
| 0x0806 | ARP            |
| 0x8000 | IS-IS          |
| 0x8035 | RARP           |
| 0x86DD | IPv6           |
| 0x880B | PPP            |
| 0x8847 | MPLS-unicast   |
| 0x8848 | MPLS-multicast |

##### 1.1.2 802.3

- 802.3 帧格式

![](https://upload-images.jianshu.io/upload_images/6757403-398271d40fbc131a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

| 字段    | 长度        | 含义                                                         |
| ------- | ----------- | :----------------------------------------------------------- |
| DMAC    | 6字节       | 目的MAC地址。                                                |
| SMAC    | 6字节       | 源MAC地址。                                                  |
| Length  | 2字节       | 指后续数据的字节长度，但不包括CRC检验码。                    |
| DSAP    | 1字节       | 目的服务访问点，若后面类型为IP帧值设为0x06。                 |
| SSAP    | 1字节       | 源服务访问点，若后面类型为IP帧值设为0x06。                   |
| Ctrl    | 1字节       | 该字段值通常设为0x03，表示无连接服务的IEEE 802.2无编号数据格式。 |
| SNAP-ID | 5字节       | 由OUI和Type两部分组成。                                      |
| OUI     | 3字节       | 3字节的组织唯一标识符（Organizationally Unique Identifier），其值通常等于MAC地址的前3字节，即网络适配器厂商代码。 |
| Type    | 2字节       | 标识以太网帧所携带的上层数据类型。                           |
| Data    | 44-1498字节 | 负载。                                                       |
| CRC     | 4字节       | 用于帧内后续字节差错的循环冗余检验（也称为FCS或帧检验序列）。 |

##### 1.1.3 VLAN

![](https://upload-images.jianshu.io/upload_images/6757403-d380994c1c1abe16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

| 字段                | 长度        | 含义                                                         |
| ------------------- | ----------- | ------------------------------------------------------------ |
| Destination address | 6字节       | 目的MAC地址。                                                |
| Source address      | 6字节       | 源MAC地址。                                                  |
| Type                | 2字节       | 表示帧类型。取值为0x8100时表示802.1Q Tag帧。如果不支持802.1Q的设备收到这样的帧，会将其丢弃。 |
| PRI                 | 3bit        | Priority，表示帧的优先级，取值范围为0～7，值越大优先级越高。用于当阻塞时，优先发送优先级高的数据包。 如果设置用户优先级，但是没有VLANID，则VLANID必须设置为0x000。 |
| CFI                 | 1bit        | CFI (Canonical Format  Indicator)，表示MAC地址是否是经典格式。CFI为0说明是标准格式，CFI为1表示为非标准格式。用于区分以太网帧、FDDI（Fiber  Distributed Digital Interface）帧和令牌环网帧。在以太网中，CFI的值为0。 |
| VID                 | 12bit       | VLAN ID，表示该帧所属的VLAN。在VRP中，可配置的VLAN ID取值范围为1～4094。0和4095协议中规定为保留的VLAN ID。                                                                      三种类型：                                                                                                                                                      Untagged帧：VID不计                                                                                                                            Priority-tagged帧：VID为0x000                                                                                                           VLAN-tagged帧：VID范围0～4095                                                                                      三个特殊的VID：                                                                                                                       0x000：设置优先级但无VID                                                                                                0x001：缺省VID                                                                                                                   0xFFF：预留VID |
| Length/Type         | 2字节       | 指后续数据的字节长度，但不包括CRC检验码。                    |
| Data                | 40-1500字节 | 负载（可能包含填充位）。                                     |
| CRC                 | 4字节       | 用于帧内后续字节差错的循环冗余检验（也称为FCS或帧检验序列）。 |

#### 1.2 串行链路

##### 1.2.1 PPP

> PPP帧的内容是指Address、Control、Protocol和Information四个域的内容。各字段的含义如下。

![](https://upload-images.jianshu.io/upload_images/6757403-bf537e30d749651d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

| 字段        | 长度                 | 含义                                                         |
| ----------- | -------------------- | ------------------------------------------------------------ |
| Flag        | 1字节                | Flag域标识了一个物理帧的起始和结束，该字节为0x7E。           |
| Address     | 1字节                | PPP协议是被运用在点对点的链路上，它可唯一标识对方，因此无须知道对方数据链路层地址。所以该字节无任何意义，按协议规定填充为全1广播地址。 |
| Control     | 1字节                | 同Address域一样，PPP数据帧的Control域也没实际意义，规定值为0x03，该域与Address域一起标识了PPP报文，即PPP报文头为FF03。 |
| Protocol    | 1字节或        2字节 | 协议域，可用来区分PPP数据帧中信息域所承载的数据报文的内容。协议域的内容必须依据ISO  3309的地址扩展机制所给出的规定。该机制规定协议域所填充的内容必须为奇数，也就是要求低字节的最低位为“1”，高字节的最低位为“0”。如果当发送端发送的PPP数据帧的协议域字段不符合上述规定，接收端则会认为此数据帧是不可识别的。接收端向发送端发送一个Protocol-Reject报文，在该报文尾部将填充被拒绝报文的协议号。 |
| Information | 0-1500字节           | 信息域最大长度是1500字节，其中包括填充域的内容。信息域的最大长度等于PPP协议中MRU（Maximum Receive  Unit）的缺省值。在实际应用当中可根据实际需要进行信息域最大封装长度选项的协商。                                                                            如果信息域长度不足1500字节，可被填充，但不是必须的。如果填充则需通信双方的两端能辨认出有用与无用的信息方可正常通信。 |
| FCS         | 0/1/2字节            | FCS域计算范围是除了flag域的其他域。                                                                                  校验域的功能主要对PPP数据帧传输的正确性进行检测。                                                                      在数据帧中引入了一些传输的保证机制，会引入更多的开销，这样可能会增加应用层交互的延迟。 |
| Code        | 1字节                | 代码域，主要是用来标识LCP数据报文的类型。在链路建立阶段，接收方接收到LCP数据报文。当其代码域的值无效时，就会向对端发送一个LCP的代码拒绝报文（Code-Reject报文）。如果是IP报文，则不存在此域，取而代之的是IP报文内容。 |
| Identifier  | 1字节                | 标识域的值表示进行协商报文的匹配关系。 标识域目的是用来匹配请求和响应报文。                                                                                                                                                            一般而言，在进入链路建立阶段时，通信双方任何一端都会连续发送几个配置请求报文（Configure-Request报文）。这几个请求报文的数据域的值可能是完全一样的，只是它们的标志域不同。                                                                                                       通常一个配置请求报文的ID是从0x01开始逐步加1的。                                                                当对端接收到该配置请求报文后，无论使用何种报文回应对方，但必须要求回应报文中的ID要与接收报文中的ID一致。当通信设备收到回应后就可以将该回应与发送时的进行比较来决定下一步的操作。 |
| Length      | 2字节                | 长度域表示此协商报文长度，它包含Code域及Identifier域的长度。长度域的值就是该LCP报文的总字节数据。它是代码域、标志域、长度域和数据域四个域长度的总和。                                                                                                                                                  长度域所指示字节数之外的字节将被当作填充字节而忽略掉，而且该域的内容不能超过MRU的值。 |
| Data        | 变长                 | 数据域所包含的是协商报文的内容。                             |

##### 1.2.2 HDLC

> 在HDLC中，数据和控制报文均以帧的标准格式传送。HDLC中的帧类似于BSC的字符块，但不是独立传输的。HDLC的完整的帧由标志字段（F）、地址字段（A）、控制字段（C）、信息字段（I）、帧校验序列字段（FCS）等组成：

![](https://upload-images.jianshu.io/upload_images/6757403-155371c1e749366e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

| 字段        | 长度    | 含义                                                         |
| ----------- | ------- | ------------------------------------------------------------ |
| Flag        | 1字节   | 标志字段，为01111110(0x7e)的比特模式，用以标志帧的开始与结束，也可以作为帧与帧之间的填充字符。 |
| Address     | 1字节   | 地址字段，内容取决于所采用的操作方式，有主节点、从节点、组合节点之分。 |
| Control     | 1字节   | 控制字段，用于构成各种命令及响应，以便对链路进行监视与控制。                                       由于Control字段的构成不同，可以把HDLC帧分为三种类型：信息帧、监控帧、无编号帧，分别简称I帧(Information)、S帧(Supervisory)、U帧(Unnumbered)。在控制字段中，第1位是“0”为I帧，第1、2  位是“1 ”为S帧，第1、2位是“11”为U帧。 |
| Protocol    | 2字节   | 协议字段。表示Information域中的数据封装的协议类型。          |
| Information | 0-N字节 | 信息字段。可以是任意的二进制比特串，长度未作限定。其上限由FCS字段或通信节点的缓冲容量来决定，目前国际上用得较多的是1000～2000比特，而下限可以是0，即无信息字段。但是监控帧中不可有信息字段。 |
| FCS         | 2字节   | FCS(Frame Check  Sequence)：帧检验序列字段，可以使用16位CRC，对两个标志字段之间的整个帧的内容进行校验。 |

##### 1.2.3 FR

### 2 网络层

#### 2.1 IPv4

> IPv4报文格式

![image.png](https://upload-images.jianshu.io/upload_images/6757403-67c1a96218d6e5ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

| 字段                | 长度  | 含义                                                         |
| ------------------- | ----- | ------------------------------------------------------------ |
| Version             | 4bit  | **版本。**4：表示为IPV4； 6：表示为IPV6。                    |
| IHL                 | 4比特 | **首部长度。**如果不带Option字段，则为20，最长为60，该值限制了记录路由选项。以4字节为一个单位。 |
| Type of Service     | 8bit  | **服务类型。**只有在有QoS差分服务要求时这个字段才起作用。    |
| Total Length        | 16bit | **总长度。**整个IP数据报的长度，包括首部和数据之和，单位为字节，最长65535，总长度必须不超过最大传输单元MTU。 |
| Flags               | 3bit  | **标志位。**标识数据包分片或最后一个分片。                                                                                                                                                 Bit 0: 保留位，必须为0。                                                                                                                          Bit 1: DF（Don't Fragment），能否分片位，0表示可以分片，1表示不能分片。                     Bit 2: MF（More Fragment），表示是否该报文为最后一片，0表示最后一片，1代表后面还有。 |
| Fragment Offset     | 12bit | **片偏移。**分片重组时会用到该字段。表示较长的分组在分片后，某片在原分组中的相对位置。以8个字节为偏移单位。 |
| Time to Live        | 8bit  | **生存时间。**可经过的最多路由数，即数据包在网络中可通过的路由器数的最大值。主要用于放环（三层），超过255次丢弃。 |
| Protocol            | 8bit  | **协议。**标识上一层协议。指出此数据包携带的数据使用何种协议，以便目的主机的IP层将数据部分上交给哪个进程处理。 常见值见下表。 |
| Header Checksum     | 16bit | **首部检验和。**只检验数据包的首部，不检验数据部分。这里不采用CRC检验码，而采用简单的计算方法。 |
| Source Address      | 32bit | **源IP地址。**                                               |
| Destination Address | 32bit | **目的IP地址。**                                             |
| Options             | 可变  | **选项字段。**用来支持排错，测量以及安全等措施，内容丰富（请参见下表）。选项字段长度可变，从1字节到40字节不等，取决于所选项的功能。 |
| Padding             | 可变  | **填充字段。**全填0。                                        |

- IP报头协议号常见值

| 协议号 | 协议名称          |
| ------ | ----------------- |
| 0      | 保留              |
| 1      | ICMP              |
| 2      | IGMP              |
| 6      | TCP               |
| 17     | UDP               |
| 47     | GRE               |
| 58     | IPv6-ICMP         |
| 89     | OSPF              |
| 112    | VRRP              |
| 124    | IS-IS             |
| 137    | MPLS              |
| 88     | EIGRP（思科独有） |

#### 2.2 IPv6

> IPv6的基本报头在IPv4报头的基础上，增加了流标签域，去除了一些冗余字段，使报文头的处理更为简单、高效。 

![](https://upload-images.jianshu.io/upload_images/6757403-29d8d55cb90749be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

| 字段                | 长度   | 含义                                                         |
| ------------------- | ------ | ------------------------------------------------------------ |
| Version             | 4bit   | **版本号。** 4：表示为IPV4； 6：表示为IPV6。                 |
| Traffic class       | 8bit   | **流量类别。**该字段及其功能类似于IPv4的业务类型字段。该字段以区分业务编码点（DSCP）标记一个IPv6数据包，以此指明数据包应当如何处理。 |
| Flow Label          | 20bit  | **流标签。**该字段用来标记IP数据包的一个流，当前的标准中没有定义如何管理和处理流标签的细节。 |
| Payload length      | 16bit  | **有效载荷的长度。**有效载荷是指紧跟IPv6基本报头的数据包，包含IPv6扩展报头。 |
| Next header         | 8bit   | **下一报头。**该字段指明了跟随在IPv6基本报头后的扩展报头的信息类型。 |
| Hop limit           | 8bit   | **跳数限制。**该字段定义了IPv6数据包所能经过的最大跳数，这个字段和IPv4中的TTL字段非常相似。 |
| Source Address      | 128bit | 该报文的**源IP地址**。                                       |
| Destination Address | 128bit | 该报文的**目的IP地址**。                                     |
| Extension Headers   | 可变   | **扩展报头。**IPv6取消了IPv4报头中的选项字段，并引入了多种扩展报文头，在提高处理效率的同时还增强了IPv6的灵活性，为IP协议提供了良好的扩展能力。当超过一种扩展报头被用在同一个分组里时，报头必须按照下列顺序出现：                                   IPv6基本报头                                                                                                                                        1.逐跳选项扩展报头                                                                                                                                  2.目的选项扩展报头                                                                                                                                          3.路由扩展报头                                                                                                                                    4.分片扩展报头                                                                                                                                      5.授权扩展报头                                                                                                                                        6.封装安全有效载荷扩展报头                                                                                                              7.目的选项扩展报头（指那些将被分组报文的最终目的地处理的选项。）                               8.上层扩展报头                                                                                                                                 不是所有的扩展报头都需要被转发路由设备查看和处理的。路由设备转发时根据基本报头中Next Header值来决定是否要处理扩展头。 除了目的选项扩展报头出现两次（一次在路由扩展报头之前，另一次在上层扩展报头之前），其余扩展报头只出现一次。 |

### 3 传输层

#### 3.1 TCP

##### 3.1.1 TCP报文头部格式

![](https://upload-images.jianshu.io/upload_images/6757403-6624592379047d9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

| 字段                  | 长度  | 含义                                                         |
| --------------------- | ----- | ------------------------------------------------------------ |
| Source Port           | 16bit | **源端口。**标识哪个应用程序发送。                           |
| Destination Port      | 16bit | **目的端口。**标识哪个应用程序接收。                         |
| Sequence Number       | 32bit | **序列号。**TCP链接中传输的数据流中每个字节都编上一个序号。序号字段的值指的是本报文段所发送的数据的第一个字节的序号。 |
| Acknowledgment Number | 32bit | **确认号。**是期望收到对方的下一个报文段的数据的第1个字节的序号，即上次已成功接收到的数据字节序号加1。只有ACK标识为1，此字段有效。 |
| Data Offset           | 4bit  | **数据偏移。**即首部长度，指出TCP报文段的数据起始处距离TCP报文段的起始处有多远，以32比特（4字节）为计算单位。最多有60字节的首部，若无选项字段，正常为20字节。 |
| Reserved              | 6bit  | **保留。**必须填0。                                          |
| URG                   | 1bit  | **紧急指针有效标识。**它告诉系统此报文段中有紧急数据，应尽快传送（相当于高优先级的数据）。 |
| ACK                   | 1bit  | **确认序号有效标识。**只有当ACK=1时确认号字段才有效。当ACK=0时，确认号无效。 |
| PSH                   | 1bit  | **标识接收方应该尽快将这个报文段交给应用层。**接收到PSH = 1的TCP报文段，应尽快的交付接收应用进程，而不再等待整个缓存都填满了后再向上交付。 |
| RST                   | 1bit  | **重建连接标识。**当RST=1时，表明TCP连接中出现严重错误（如由于主机崩溃或其他原因），必须释放连接，然后再重新建立连接。 |
| SYN                   | 1bit  | **同步序号标识。**用来发起一个连接。SYN=1表示这是一个连接请求或连接接受请求。 |
| FIN                   | 1bit  | **发端完成发送任务标识。**用来释放一个连接。FIN=1表明此报文段的发送端的数据已经发送完毕，并要求释放连接。 |
| Window                | 16bit | **窗口。**TCP的流量控制，窗口起始于确认序号字段指明的值，这个值是接收端正期望接收的字节数。窗口最大为65535字节。 |
| Checksum              | 16bit | **校验字段。**包括TCP首部和TCP数据，是一个强制性的字段，一定是由发端计算和存储，并由收端进行验证。在计算检验和时，要在TCP报文段的前面加上12字节的伪首部。 |
| Urgent Pointer        | 16bit | **紧急指针。**只有当URG标志置1时紧急指针才有效。TCP的紧急方式是发送端向另一端发送紧急数据的一种方式。紧急指针指出在本报文段中紧急数据共有多少个字节（紧急数据放在本报文段数据的最前面）。 |
| Options               | 可变  | **选项字段。**TCP协议最初只规定了一种选项，即最长报文段长度（数据字段加上TCP首部），又称为MSS。MSS告诉对方TCP“我的缓存所能接收的报文段的数据字段的最大长度是MSS个字节”。                                                                       新的RFC规定有以下几种选型：选项表结束，无操作，最大报文段长度，窗口扩大因子，时间戳。                                                                                                               窗口扩大因子：3字节，其中一个字节表示偏移值S。新的窗口值等于TCP首部中的窗口位数增大到（16+S），相当于把窗口值向左移动S位后获得实际的窗口大小。                                                                                                                       时间戳：10字节，其中最主要的字段是时间戳值（4字节）和时间戳回送应答字段（4字节）。                                                                                                                                                               选项确认选项： |
| Padding               | 可变  | 填充字段，用来补位，使整个首部长度是4字节的整数倍。          |
| Data                  | 可变  | TCP负载。                                                    |

##### 3.1.2 TCP三次握手通信过程——建立连接

![](https://upload-images.jianshu.io/upload_images/6757403-4605acbfe2391c81.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. 主机A（通常也称为客户端）发送一个标识了SYN的数据段，表示期望与服务器A建立连接，此数据段的序列号（seq）为a。
2. 服务器A回复标识了SYN+ACK的数据段，此数据段的序列号（seq）为b，确认序列号为主机A的序列号加1（a+1），以此作为对主机A的SYN报文的确认。
3. 主机A发送一个标识了ACK的数据段，此数据段的序列号（seq）为a+1，确认序列号为服务器A的序列号加1（b+1），以此作为对服务器A的SYN报文的确认。

##### 3.1.3 TCP四次挥手通信过程——关闭连接

![](https://upload-images.jianshu.io/upload_images/6757403-bf0ea81f170651ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. 主机A想终止连接，于是发送一个标识了FIN，ACK的数据段，序列号为a，确认序列号为b。
2. 服务器A回应一个标识了ACK的数据段，序列号为b，确认序号为a+1，作为对主机A的FIN报文的确认。
3. 服务器A想终止连接，于是向主机A发送一个标识了FIN，ACK的数据段，序列号为b，确认序列号为a+1。
4. 主机A回应一个标识了ACK的数据段，序列号为a+1，确认序号为b+1，作为对服务器A的FIN报文的确认。

> 之所以设置两边都发起断开连接，是因为有可能是一方数据发送完成，而另一方数据为发送完成，两边分别断开连接确保两边的数据都完成了发送。

#### 3.2 UDP

![](https://upload-images.jianshu.io/upload_images/6757403-429ccda80fced713.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

| 字段             | 长度  | 含义                                  |
| ---------------- | ----- | ------------------------------------- |
| Source Port      | 2字节 | 标识哪个应用程序发送（发送进程）。    |
| Destination Port | 2字节 | 标识哪个应用程序接收（接收进程）。    |
| Length           | 2字节 | UDP首部加上UDP数据的字节数，最小为8。 |
| Checksum         | 2字节 | 覆盖UDP首部和UDP数据，是可选的。      |
| data octets      | 可变  | UDP负载，可选的。                     |

## 三、不同数据的处理方式

### 1 主机

![](https://upload-images.jianshu.io/upload_images/6757403-b819214e06debe66.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 当主机收到数据时，首先拆开数据到数据链路层，查看其目的MAC地址，如果目的MAC地址是自己，或者是广播，或者是所在的组的组播地址，那么就拆包，否则目的MAC地址不是自己则丢弃。
- 继续拆开数据到网络层，查看其目的IP地址，如果目的IP地址是自己，或者是广播地址，或者是所在组的组播地址，那么就继续拆包，否则目的IP地址不是自己，则丢弃。
- 继续拆开数据到传输层，查看其目的端口号，如果目的端口在本机上有开启，那么久交给相应的应用程序处理，否则目的端口未开启则就丢弃。

### 2 交换机

#### 2.1 交换机各种端口转发模式

##### 2.1.1 Access口

- Access接口接收到报文处理过程 

![](https://upload-images.jianshu.io/upload_images/6757403-017e695dee5223d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. Access口收到数据时，首先查看报文中是否携带Tag字段，如果携带，则查看其VID字段，看其所属VLAN，是否和入接口的PVID值相同，如果相同则就接收，不同则就丢弃。
2. 如果不带Tag，则打上入接口的PVID值再进一步处理。

- Access接口发送报文处理过程  

![](https://upload-images.jianshu.io/upload_images/6757403-b931e1c14edc54f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. Access口发送数据时，直接玻璃Tag进行发送。

##### 2.1.2 Trunk口

- Trunk接口接收报文处理过程  

![](https://upload-images.jianshu.io/upload_images/6757403-76550bb3f4b4bad0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. Trunk接收到数据时，首先查看数据段中是否含有Tag字段，如果含有Tag字段，则查看其VID字段，看其所属VLAN
2. 如果报文不带Tag字段，则打上该接口的PVID的Tag，封装入数据段。
3. 比较该接口是否允许该VLAN通过，如果允许通过则进接收，如果不允许通过则丢弃。

- Trunk接口发送报文处理过程  

![](https://upload-images.jianshu.io/upload_images/6757403-7f6766ab2d6c2e83.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. Trunk接收到数据时，首先查看报文中Tag字段的VID值是否和该接口的PVID值相同，如果相同则剥离Tag，如果不同则保持原有的Tag，然后发送。

##### 2.1.3 Hybrid口

- Hybrid接口接收报文处理过程  

![](https://upload-images.jianshu.io/upload_images/6757403-cf8aab9489c8323c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 和Trunk接口类似。

- Hybrid接口发送报文处理过程

![](https://upload-images.jianshu.io/upload_images/6757403-3fcf1f875d3499e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 和Trunk接口类似。

#### 2.2 不同类型接口添加或剥除VLAN标签的比较

| 接口              类型 | 对接收不带Tag的                              报文处理        | 对接收带Tag的                报文处理                        | 发送帧处理过程                                               |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Access接口             | 接收该报文，并打上缺省的VLAN ID。                            | 1.当VLAN ID与缺省VLAN ID相同时，接收该报文。                                    2.当VLAN ID与缺省VLAN ID不同时，丢弃该报文。 | 先剥离帧的PVID Tag，然后再发送。                             |
| Trunk接口              | 1.打上缺省的VLAN ID，当缺省VLAN ID在允许通过的VLAN ID列表里时，接收该报文。         2.打上缺省的VLAN ID，当缺省VLAN ID不在允许通过的VLAN ID列表里时，丢弃该报文。 | 1.当VLAN ID在接口允许通过的VLAN ID列表里时，接收该报文。     2.当VLAN ID不在接口允许通过的VLAN ID列表里时，丢弃该报文。 | 1.当VLAN ID与缺省VLAN ID相同，且是该接口允许通过的VLAN ID时，去掉Tag，发送该报文。                      2.当VLAN ID与缺省VLAN ID不同，且是该接口允许通过的VLAN ID时，保持原有Tag，发送该报文。 |
| Hybrid接口             | 1.打上缺省的VLAN ID，当缺省VLAN ID在允许通过的VLAN ID列表里时，接收该报文。        2.打上缺省的VLAN ID，当缺省VLAN ID不在允许通过的VLAN ID列表里时，丢弃该报文。 | 1.当VLAN ID在接口允许通过的VLAN ID列表里时，接收该报文。               2.当VLAN ID不在接口允许通过的VLAN ID列表里时，丢弃该报文。 | 当VLAN ID是该接口允许通过的VLAN ID时，发送该报文。可以通过命令设置发送时是否携带Tag。 |

 由上面各类接口添加或剥除VLAN标签的处理过程可见：

- 当接收到不带VLAN标签的数据帧时，Access接口、Trunk接口、Hybrid接口都会给数据帧打上VLAN标签，但Trunk接口、Hybrid接口会根据数据帧的VID是否为其允许通过的VLAN来判断是否接收，而Access接口则无条件接收。

- 当接收到带VLAN标签的数据帧时，Access接口、Trunk接口、Hybrid接口都会根据数据帧的VID是否为其允许通过的VLAN（Access接口允许通过的VLAN就是缺省VLAN）来判断是否接收。

- 当发送数据帧时：

  - Access接口直接剥离数据帧中的VLAN标签。
  - Trunk接口只有在数据帧中的VID与接口的PVID相等时才会剥离数据帧中的VLAN标签。
  - Hybrid接口会根据接口上的配置判断是否剥离数据帧中的VLAN标签。

  因此，Access接口发出的数据帧肯定不带Tag，Trunk接口发出的数据帧只有一个VLAN的数据帧不带Tag，其他都带VLAN标签，Hybrid接口发出的数据帧可根据需要设置某些VLAN的数据帧带Tag，某些VLAN的数据帧不带Tag。

#### 2.3 交换机数据转发过程

![](https://upload-images.jianshu.io/upload_images/6757403-eda182cccf7082bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 当交换机接收到数据时，首先拆开数据到数据链路层，查看其源MAC地址，如果源MAC地址不在MAC地址表里面，则就学习地址和相应的接口绑定，并且设置老化时间300s，如果已经学习到了，那么就更新老化时间。
- 然后再查看目的MAC地址，如果目的MAC地址是自己，那么就拆包；如果不是自己，在查看地址表，如果目的MAC地址是组播地址或广播地址或未知单播地址，那么就进行泛洪处理，如果是单播MAC地址，查看MAC地址表的转发出街口，如果和进接口一致则丢弃，不一致则转发。
- 继续拆包到网络层，查看目的IP地址，如果目的IP地址是自己，那么就拆包；如果不是则丢弃。
- 继续拆包到传输层，查看目的端口号，入股目的端口号已开启，则传给相应应用程序处理，如果未开启，则丢弃数据包。

### 3 路由器

![](https://upload-images.jianshu.io/upload_images/6757403-25a8d8ec62e4ea0b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/6757403-6d62c510a99ce56b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 当路由器收到数据包时，首先查看目的MAC地址是不是自己，如果是自己的MAC地址，那么就拆包，如果不是则就丢弃。
- 继续拆包查看目的IP地址，如果目的IP地址是自己的，那么就继续拆包，如果不是自己的就查看路由表。如果路由表没有匹配的路由条目则就丢弃，否则有的话就转发。
- 当路由表有相应的路由条目，则以最长掩码匹配的方法进行转发，如果路由条目仅有出接口，则发送ARP报文获取MAC地址，如果有代理ARP回复，则封装MAC转发，如果无回复，则超时丢弃。如果路由条目有下一跳，则迭代查询路由表，找到对应的条目，查看ARP表，如果ARP表有条目则封装MAC地址进行转发，如果ARP表没有条目，则发送ARP来获取MAC地址，再封装进行转发。
- 继续拆包到传输层，查看目的端口号，如果目的端口号有开启，则传给相应的应用程序进行查理，否则没有开启相应的端口，则丢弃数据包。

# 结语
