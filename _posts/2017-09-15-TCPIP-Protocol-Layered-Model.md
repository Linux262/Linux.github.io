---
layout:     post
title:      TCPIP协议分层模型
subtitle:   网络通信协议之TCP/IP
date:       2017-09-15
author:     Mlin
header-img: img/post-bg-tcpip.jpg
catalog: true
tags:
    - IP
    - 通信协议
---


> 本文首次发布于 [Mlin Blog](http://happymiki.top)、[简书](http://www.jianshu.com/u/3f05018752b8)，作者 [@木林(Mlin)](http://github.com/happymiki) ,转载请保留原文链接。

> 上一篇文章 [《OSI七层参考模型》](https://happymiki.github.io/2017/09/11/OSI七层参考模型/)。

# 前言
TCP，Transmission Control Protocol/Internet Protocol的简写，中译名为传输控制协议/因特网互联协议，又名网络通讯协议，是Internet最基本的协议、Internet国际互联网络的基础，由网络层的IP协议和传输层的TCP协议组成。TCP/IP 定义了电子设备如何连入因特网，以及数据如何在它们之间传输的标准。协议采用了4层的层级结构，每一层都呼叫它的下一层所提供的协议来完成自己的需求。通俗而言：TCP负责发现传输的问题，一有问题就发出信号，要求重新传输，直到所有数据安全正确地传输到目的地。而IP是给因特网的每一台联网设备规定一个地址。

# 目录

# 正文
## 一、TCP/IP模型
TCP/IP模型分为5层：应用层、传输层、网络层、数据链路层以及 物理层。分层就类似接口的定义，定义了每个层的行为职责。这样的分层抽象提供了更多实现的自由。
![](http://upload-images.jianshu.io/upload_images/6757403-6ff8082be5602084.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


1、应用层

- 应用层是我们经常接触使用的部分，比如常用的http协议、ftp协议（文件传输协议）、snmp（网络管理协议）、telnet （远程登录协议 ）、smtp（简单邮件传输协议）、dns（域名解析），这次主要是面向用户的交互的。这里的应用层集成了osi分层模型中 的应用、会话、表示层三层的功能。

2、传输层

- 传输层的作用就是将应用层的数据进行传输转运。比如我们常说的tcp（可靠的传输控制协议）、udp（用户数据报协议）。传输单位为报文段。

- tcp（Transmission Control Protocol）面向连接（先要和对方确定连接、传输结束需要断开连接，类似打电话）、复杂可靠的、有很好的重传和查错机制。一般用与高速、可靠的通信服务

- udp（user datagram protocol面向无连接（无需确认对方是否存在，类似寄包裹）、简单高效、没有重传机制。一般用于即时通讯、广播通信等

3、网络层

- 网络层用来处理网络中流动的数据包，数据包为最小的传递单位，比如我们常用的ip协议、icmp协议、arp协议（通过分析ip地址得出物理mac地址）。

4、数据链路层

- 数据链路层一般用来处理连接硬件的部分，包括控制网卡、硬件相关的设备驱动等。传输单位数据帧。

5、物理层

- 物理层一般为负责数据传输的硬件，比如我们了解的双绞线电缆、无线、光纤等。比特流光电等信号发送接收数据。
- 
## 二、TCP/IP协议群（簇）
**物理层**（RS-232、V.35）和 数据链路层(HDLC、X.25)涉及到在通信信道上传输的原始比特流，它实现传输数据所需要的机械、电气、功能性及过程等手段，提供检错、纠错、同步等措施，使之对网络层显现一条无错线路；并且进行流量调控。Bits、Frames

**网络层**检查网络拓扑，以决定传输报文的最佳路由，执行数据转发。其关键问题是确定数据包从源端到目的端如何选择路由。网络层的主要协议有IP、ICMP（Internet Control Message Protocol，互联网控制报文协议）、IGMP（Internet Group Management Protocol，互联网组管理协议）、ARP（Address Resolution Protocol，地址解析协议）和RARP（Reverse Address Resolution Protocol，反向地址解析协议）等。Packets

**传输层**的基本功能是为两台主机间的应用程序提供端到端的通信。传输层从应用层接受数据，并且在必要的时候把它分成较小的单元，传递给网络层，并确保到达对方的各段信息正确无误。传输层的主要协议有TCP、UDP（User Datagraph Protocol，用户数据报协议）。Segments

**应用层**负责处理特定的应用程序细节。应用层显示接收到的信息，把用户的数据发送到低层，为应用软件提供网络接口。应用层 包含大量常用的应用程序，例如HTTP（HyperText Transfer Protocol文本传输协议）、Telnet（远程登录）、FTP（File Transfer Protocol）等。

![](http://upload-images.jianshu.io/upload_images/6757403-3389d87e695717b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 三、TCP/IP的发展
![](http://upload-images.jianshu.io/upload_images/6757403-af96db2bf1f015a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 四、应用层
#### 1.应用层概述
- 应用层也称为应用实体（AE），它由若干个特定应用服务元素（SASE）和一个或多个公用应用服务元素（CASE）组成。每个SASE提供特定的应用服务，例如文件运输访问和管理（FTAM）、电子文电处理（MHS）、虚拟终端协议（VAP）等。CASE提供一组公用的应用服务，例如联系控制服务元素（ACSE）、可靠运输服务元素（RTSE）和远程操作服务元素（ROSE）等。主要负责对软件提供接口以使程序能使用网络服务。术语“应用层”并不是指运行在网络上的某个特别应用程序 ，应用层提供的服务包括文件传输、文件管理以及电子邮件的信息处理。
![](http://upload-images.jianshu.io/upload_images/6757403-72a5a9aaaa87d964.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- TCP/IP应用的架构绝大多数属于客户端/服务器模式。
 - 服务端：提供服务的程序；
 - 客户端：接受服务的程序；

- 在C/S通信模式中，提供服务的程序会预先被部署到主机上，等待接收任何时刻客户可能发送的请求。

#### 2.WWW
**HTTP：**超文本传送协议。是面向事务的应用层协议，它是万维网上能够可靠地交换文件的重要基础。http使用面向连接的TCP作为运输层协议，保证了数据的可靠传输。
![](http://upload-images.jianshu.io/upload_images/6757403-b7f16556dbcab977.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 3.E-mail
![](http://upload-images.jianshu.io/upload_images/6757403-8ef37b1d986eae68.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 4.FTP
**FTP：**文件传输协议。FTP是因特网上使用得最广泛的文件传送协议。FTP提供交互式的访问，允许客户指明文件类型与格式，并允许文件具有存取权限。FTP其于TCP。

![](http://upload-images.jianshu.io/upload_images/6757403-d5c50643f7f6c0d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 5.TELNET & SSH
**Telnet:**远程终端协议。telnet是一个简单的远程终端协议，它也是因特网的正式标准。又称为终端仿真协议。

**SSH:**为 Secure Shell 的缩写，由 IETF 的网络工作小组（Network Working Group）所制定；SSH 为建立在应用层和传输层基础上的安全协议。SSH 是目前较可靠，专为远程登录会话和其他网络服务提供安全性的协议。利用 SSH 协议可以有效防止远程管理过程中的信息泄露问题。S S H最初是U N I X系统上的一个程序，后来又迅速扩展到其他操作平台。S S H在正确使用时可弥补网络中的漏洞。S S H客户端适用于多种平台。几乎所有UNIX平台—包括HP-UX、Linux、AIX、Solaris、Digital UNIX、Irix，以及其他平台—都可运行SSH。

![](http://upload-images.jianshu.io/upload_images/6757403-113da8569d477114.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 6.SNMP
**SNMP：**简单网络管理协议。由三部分组成：SNMP本身、管理信息结构SMI和管理信息MIB。SNMP定义了管理站和代理之间所交换的分组格式。SMI定义了命名对象类型的通用规则，以及把对象和对象的值进行编码。MIB在被管理的实体中创建了命名对象，并规定类型。

![](http://upload-images.jianshu.io/upload_images/6757403-b125a4d2f2df1ff4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 7.DNS
**DNS：**域名系统DNS是因特网使用的命名系统，用来把便于人们使用的机器名字转换为IP地址。
现在顶级域名TLD分为三大类：国家顶级域名nTLD；通用顶级域名gTLD;基础结构域名
域名服务器分为四种类型：根域名服务器；顶级域名服务器；本地域名服务器；权限域名服务器。

## 五、主机到主机层（传输层）
TCP:提供面向连接的、可靠的、有序的、流量控制的传输服务。

UDP:提供无连接、不可靠的、无序的、无流量控制的传输服务。

![](http://upload-images.jianshu.io/upload_images/6757403-85d5c624ad7aac4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 1.TCP报文格式
参考文章：[《TCP报文格式》](http://blog.csdn.net/a19881029/article/details/29557837)

![](http://upload-images.jianshu.io/upload_images/6757403-d9859d9eb0b34ece.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2.端口号
参考文章：[《TCP/UDP 常用端口列表》](http://blog.csdn.net/joyous/article/details/41806771)
![](http://upload-images.jianshu.io/upload_images/6757403-287ecdcef17c8342.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 3.TCP三次握手
![](http://upload-images.jianshu.io/upload_images/6757403-c60653507c2d0efb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 4.TCP四次挥手
![](http://upload-images.jianshu.io/upload_images/6757403-861a210e4d7faa85.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 5.UDP报文格式
![](http://upload-images.jianshu.io/upload_images/6757403-c2f5c6fa4ab499d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 6.TCP与UDP比较
![](http://upload-images.jianshu.io/upload_images/6757403-58573eaf83e087bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 六、Internet层
#### 1.Internet层概述
![](http://upload-images.jianshu.io/upload_images/6757403-cd1fd793567bd92e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2.IP Header
![](http://upload-images.jianshu.io/upload_images/6757403-2714181bc0bca105.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 3.ICMP
- 通过IP数据包传送，用来发送错误和控制信息。
- 封装在IP数据包内。
- Ping
	- 测试网络连通性及通信质量
	- 发送方发送ICMP echo request
	- 接收方收到后，回应ICMP echo reply
- Tracert
	- 探测目标主机经过的三层设备的接口IP
	- 使用ICMP超时机制发现一个数据包到达目的地所经历的路径

#### 4.ARP
![](http://upload-images.jianshu.io/upload_images/6757403-12d6a61d99736f32.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 5.RARP
![](http://upload-images.jianshu.io/upload_images/6757403-0eb3997b1fa5ba53.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 6.测试工具
- Ping
	- 测试网络的连通性及通信质量
	- 发送方发送ICMP echo request
	- 接收方收到后，回应ICMP echo reply
- Tracert
	- 探测到target主机所经过的三层设备的接口IP
	- 源主机发送到target IPTTL置1的探测包
	- 中间设备收到探测包，TTL减1，如果等于0，则丢弃探测包，并向源回应超时错
	- 源主机收到ICMP超时错，TTL在上一个探测包的基础上加1

## 七、TCP/IP通信示例
![](http://upload-images.jianshu.io/upload_images/6757403-dde980e44c0d75ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 结语
