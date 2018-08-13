---
layout:     post
title:      局域网二层技术
subtitle:   局域网二层的ARP、MAC地址表、链路聚合、GVRP的介绍
date:       2018-08-08
author:     Miki
header-img: img/post-bg-Two-layers-LAN.jpg
catalog: true
tags:
    - HCIE
    - LAN
    - 二层
    - MAC地址表
    - 链路聚合
    - GVRP
    - ARP
---


> 本文首次发布于 [Mlin Blog](http://happymiki.github.io)、[简书](http://www.jianshu.com/u/3f05018752b8)、[CSDN](https://blog.csdn.net/qq_34104227)，作者 [@木林(Mlin)](http://github.com/happymiki) ,转载请保留原文链接。

[TOC]

# 前言

# 正文

## 一、ARP

### 1 Proxy ARP

> 如果ARP请求是从一个网络的主机发往同一网段但不在同一物理网络上的另一台主机，那么连接这两个网络的设备就可以回答该ARP请求，这个过程称作ARP代理(Proxy ARP)。

- Proxy ARP分为路由式Proxy ARP、VLAN内Proxy ARP和VLAN间Proxy ARP

| Proxy ARP   方式    | 适用场景                                                     |
| ------------------- | ------------------------------------------------------------ |
| 路由式    Proxy ARP | 需要互通的主机（主机上没有配置缺省网关）处于相同的网段但不在同一物理网络（即不在同一广播域）的场景。 |
| VLAN内  Proxy ARP   | 需要互通的主机处于相同网段，并且属于相同VLAN，但是VLAN内配置了端口隔离的场景。 |
| VLAN间  Proxy ARP   | 需要互通的主机处于相同网段，但属于不同VLAN的场景。           |

- Proxy ARP有以下特点：
  - Proxy ARP部署在网关上，网络中的主机不必做任何改动。
  - Proxy ARP可以隐藏物理网络细节，使两个物理网络可以使用同一个网络号。
  - Proxy ARP只影响主机的ARP表，对网关的ARP表和路由表没有影响。

#### 1.1 路由式Proxy ARP

> 路由式Proxy ARP就是使那些在同一网段却不在同一物理网络上的网络设备能够相互通信的一种功能。

- 在实际应用中，如果连接设备的主机上没有配置缺省网关地址（即不知道如何到达本网络的中介系统），此时将无法进行数据转发。
- 如图所示，PC1的IP地址为1.1.1.1/16，PC2的IP地址为1.1.2.2/16，PC1与PC2处于同一网段。S1通过VLAN2和VLAN3连接两个网络，VLANIF2和VLANIF3的IP地址不在同一个网段。

![](https://upload-images.jianshu.io/upload_images/6757403-4eb2a9efee144850.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 当PC1需要与PC2通信时，由于目的IP地址与本机的IP地址为同一网段，因此PC1以广播形式发送ARP请求报文，请求PC2的MAC地址。但是，由于两台主机处于不同的物理网络（不同广播域）中，PC2无法收到PC1的ARP请求报文，因此也就无法应答。
- 通过在S1上启用路由式Proxy ARP功能，可以解决此问题。启用路由式Proxy ARP后，S1收到ARP请求报文后，S1会查找路由表。由于PC2与S1直连，因此S1上存在到PC2的路由表项。S1使用自己的MAC地址给PC1发送ARP应答报文。PC1将以S1的MAC地址进行数据转发。此时，S1相当于PC2的代理。PC1上的ARP表项中到目的地址PC2的IP地址对应的MAC地址为S1的VLANIF2接口的MAC地址。

#### 1.2 VLAN内ARP代理

> 如果两个用户属于相同的VLAN，但VLAN内配置了端口隔离。此时用户间需要三层互通，可以在关联了VLAN的接口上启动VLAN内Proxy ARP功能。

- PC1和PC2是S1设备下的两个用户。连接PC1和PC2的两个接口在S1属于同一个VLAN2。

![](https://upload-images.jianshu.io/upload_images/6757403-4c8f3923d25b522a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 在S1的两个接口上配置端口隔离： port-isolate enable group 1
- 在S1上启用三层接口int vlanif 2，配置IP地址，并启用VLAN内ARP代理：arp-proxy inner-sub-vlan-proxy enable 
- 由于在S1上配置了VLAN内不同接口彼此隔离，因此PC1和PC2不能直接在二层互通。
- 若S1的接口使能了VLAN内Proxy ARP功能，可以使PC1和PC2实现三层互通。S1的接口在接收到目的地址不是自己的ARP请求报文后，S1并不立即丢弃该报文，而是查找该接口的ARP表项。如果存在PC2的ARP表项，则将自己的MAC地址发送给PC1，并将PC1发送给PC2的报文代为转发。实际上此时S1相当于PC2的代理。

#### 1.3 VLAN间ARP代理

>如果两台主机处于相同网段但属于不同的VLAN，用户间要进行三层互通，可以在关联了这些VLAN的接口（例如VLANIF接口或者子接口）上使能VLAN间Proxy ARP功能。

![](https://upload-images.jianshu.io/upload_images/6757403-a2cfd02015960f2a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 由于PC1和PC2属于不同的Sub-VLAN，PC1和PC2不能直接实现二层互通。
- 如果S1上使能了VLAN间Proxy ARP功能，可以使PC1和PC2实现三层互通。S1的接口在接收到目的地址不是自己的ARP请求报文后，并不立即丢弃该报文，而是查找ARP表项（包括动态学习的ARP表项和静态配置的ARP表项）。如果存在PC2的ARP表项，则将自己的MAC地址发送给PC1，并将PC1发送给PC2的报文代为转发。实际上此时S1相当于PC2的代理。

####  2 免费ARP

> 设备主动使用自己的IP地址作为目的IP地址发送ARP请求，此种方式称免费ARP。

- 免费ARP有如下作用：
  - **IP地址冲突检测**：当设备接口的协议状态变为Up时，设备主动对外发送免费ARP报文。正常情况下不会收到ARP应答，如果收到，则表明本网络中存在与自身IP地址重复的地址。如果检测到IP地址冲突，设备会周期性的广播发送免费ARP应答报文，直到冲突解除。
  - **用于通告一个新的MAC地址**：发送方更换了网卡，MAC地址变化了，为了能够在动态ARP表项老化前通告网络中其他设备，发送方可以发送一个免费ARP。
  - **在VRRP备份组中用来通告主备发生变换**：发生主备变换后，MASTER设备会广播发送一个免费ARP报文来通告发生了主备变换。
- 设备收到免费ARP报文后：
  - 如果未使能ARP表项严格学习功能，设备会进行ARP学习。
  - 如果使能了ARP表项严格学习功能，则进行如下判断：
    - 如果免费ARP报文中源IP地址和自己的IP地址相同，则周期性的广播发送免费ARP应答报文，告知此IP地址在网络中存在冲突，直到冲突解除。
    - 如果免费ARP报文中源IP地址和自己的IP地址不同，免费ARP报文是在VLANIF接口收到的，并且设备上已经有免费ARP报文中源IP地址对应的动态ARP表项，则进行ARP学习，即根据收到的免费ARP报文更新该ARP表项。其余情况收到免费ARP报文后均不进行ARP学习。

缺省情况下，设备未使能ARP表项严格学习功能。

## 二、MAC表

### 1 MAC地址表分类

- 动态表项
  - 通过接口学习源MAC 地址，生成MAC地址表项，表项自动老化（300秒）
- 静态表项
  - 由用户手工配置，并下发到各接口板，表项不老化
- 黑洞表项
  - 用于丢弃含有特定源MAC地址或目的MAC地址的数据帧
  - 由用户手工配置，并下发到各接口板，表项不老化

### 2 端口隔离

> 为了实现用户之间的二层隔离，可以将不同的用户加入不同的VLAN，但这样会浪费有限的VLAN资源。采用端口隔离功能，可以实现同一VLAN内端口之间的隔离。用户只需要将端口加入到同一隔离组中，就可以实现隔离组内端口之间二层数据的隔离。端口隔离功能为用户提供了更安全、更灵活的组网方案。 

![](https://upload-images.jianshu.io/upload_images/6757403-bce53f71c723ef7c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 加入同一隔离组，二层隔离三层互通；二层三层都隔离

- 如下图所示，同一端口隔离组内的用户不能进行二层的通信，但是不同端口隔离组内的用户可以进行正常通行；未划分端口隔离的用户也能与端口隔离组内的用户正常通信。

- 端口隔离分为二层隔离三层互通和二层三层都隔离两种模式： 

  - 如果用户希望隔离同一VLAN内的广播报文，但是不同端口下的用户还可以进行三层通信，则可以将隔离模式设置为二层隔离三层互通。 
  - 如果用户希望同一VLAN不同端口下用户彻底无法通信，则可以将隔离模式配置为二层三层均隔离。

- 配置注意事项：

  - S系列交换机均支持配置二层隔离三层互通模式。 

  - S系列框式交换机均支持二层三层都隔离模式，S系列盒式交换机仅V100R006C05版本仅S2700SI、S2700EI不支持二层三层都隔离模式，V100R002及后续版本S1720、S2720、S2750EI、S5700LI、S5700S-LI不支持二层三层都隔离模式。 
  - 如果不是特殊情况要求，建议用户不要将上行口和下行口加入到同一端口隔离组中，否则上行口和下行口之间不能相互通信。

![](https://upload-images.jianshu.io/upload_images/6757403-d34b087cb108e0af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3 端口安全

- 将交换机接口学习到的MAC地址转变为安全MAC地址，以阻止安全MAC和静态MAC之外的主机通过本接口和交换机通信
- 端口安全学习MAC地址方式
  - 安全动态MAC地址

    - 使能端口安全而未使能Sticky MAC功能时学习到的MAC地址。使能端口安全后，端口上之前学习到的动态MAC地址表项会被删除，然后端口再学习得到的安全动态MAC地址不会被老化，设备重启后安全动态	MAC地址会丢失，需要重新学习。
  - Sticky MAC地址：
    - 使能端口安全后又使能Sticky MAC功能后学习到的MAC地址。Sticky MAC地址不会被老化，保存配置后重启设备，Sticky MAC地址不会丢失，无需重新学习。

### 4 MAC地址表漂移

#### 4.1 MAC地址漂移

- MAC地址漂移
  - 一个接口学习到的MAC地址在另一个接口上也学习到，后学习到的MAC地址表项覆盖原来的表项
- MAC地址漂移避免机制
  - 提高接口MAC 地址学习优先级
  - 不允许相同优先级的接口发生MAC 地址表项覆盖

![](https://upload-images.jianshu.io/upload_images/6757403-b635e36f97a07021.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- MAC地址防漂移避免
  - 提高接口MAC地址学习优先级：当不同接口学到相同的MAC地址表项时，高优先级接口学到的MAC地址表项可以覆盖低优先级接口学到的MAC地址表项，防止MAC地址在接口间发生漂移。
  - 不允许相同优先级的接口发生MAC地址表项覆盖。当网络攻击设备所连的接口优先级与原设备连接接口的优先级相同时，后学习到的网络攻击设备的MAC地址表项不会覆盖之前正确的表项。但如果原网络设备下电，此时交换机将会学习到网络攻击设备的MAC地址，当原网络设备再次上电时，交换机将无法学习到正确的MAC地址。
- 拓扑描述
  - 为防止非法用户PC3伪造PC1的MAC地址攻击网络，可以提高PC1连接的接口Port1的MAC地址学习优先级。

#### 4.2 MAC地址漂移检测

- 利用MAC地址学习时端口跳变实现MAC地址漂移检测，跳变端口即为有可能出现环路的端口

![](https://upload-images.jianshu.io/upload_images/6757403-6defd1ae76b1be1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 拓扑描述
  - 假设交换网络中没有运行防环协议，若S2和S4之间误接网线，则S2、S3、S4之间形成环路。当S1发送一个广播报文给S3后，该报文经过环路，会被S1上的Port1端口收到。在接口Port1上配置MAC 地址漂移检测，此时S1会感知到MAC地址学习端口跳变的现象。若连续出现此现象，则在S1可以判断出现了MAC地址漂移。S1可以通过MAC漂移检测联动error-down端口或者联动端口退VLAN尝试解除网络环路。
- MAC地址漂移检测
  - MAC漂移检测联动端口退VLAN技术与其他端口动态VLAN技术相冲突，不能同时使用。

## 三、以太网链路聚合

### 1 基本概念

- 将多条物理链路捆绑在一起成为一条逻辑链路

- 两端相连的物理接口的数量、速率、双工方式、流控方式必须一致

- 能够增加带宽、提高可靠性和实现负载分担

![](https://upload-images.jianshu.io/upload_images/6757403-24b27502c9754d4f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 以太网链路聚合特点
  - 增加带宽：链路聚合接口的最大带宽可以达到各成员接口带宽之和。
  - 提高可靠性：当某条活动链路出现故障时，流量可以切换到其他可用的成员链路上，从而提高链路聚合接口的可靠性。
  - 分担负载：在一个Eth-Trunk接口内的各条链路可以实现流量的负载分担。
- 以太网链路聚合基本概念
  - Eth-Trunk：链路聚合组LAG（Link Aggregation Group）是指将若干条以太链路捆绑在一起所形成的逻辑链路，简写为Eth-Trunk。
  - 成员接口和成员链路：组成Eth-Trunk接口的各个物理接口称为成员接口。成员接口对应的链路称为成员链路。
  - 活动接口和非活动接口、活动链路和非活动链路：
    - 链路聚合组的成员接口存在活动接口和非活动接口两种。转发数据的接口称为活动接口，不转发数据的接口称为非活动接口。
    - 活动接口对应的链路称为活动链路，非活动接口对应的链路称为非活动链路。
  - 活动接口数上限阈值：设置活动接口数上限阈值的目的是在保证带宽的情况下提高网络的可靠性。当前活动链路数目达到上限阈值时，再向Eth-Trunk中添加成员接口，不会增加Eth-Trunk活动接口的数目，超过上限阈值的链路状态将被置为Down，作为备份链路。
  - 活动接口数下限阈值：设置活动接口数下限阈值是为了保证最小带宽，当前活动链路数目小于下限阈值时，Eth-Trunk接口的状态转为Down。

### 2 转发原理

- Eth-Trunk位于MAC子层与物理层之间，属于数据链路层。

- Eth-Trunk转发表项：
  - HASH-KEY 值：根据数据包的MAC地址或IP地址得出
  - 端口号：将HASH-KEY与端口号对应

![](https://upload-images.jianshu.io/upload_images/6757403-bbf7bc81f2212c2a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3 负载分担

- Eth-Trunk依据HASH-KEY与端口的对应关系，实现逐流的负载分担，保证数据包不乱序
- 可以基于源或目的MAC，源或目的IP、源和目的MAC、源和目的IP等实现负载分担

![](https://upload-images.jianshu.io/upload_images/6757403-38114b7804c005a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 4 链路聚合模式

- 手工模式聚合

  - 需手工创建Eth-Trunk、加入成员端口

  - 无法检测链路层故障和链路错连

- LACP模式聚合
  - 通过LACP协议自动实现链路聚合
  - LACP可以维护链路状态

![](https://upload-images.jianshu.io/upload_images/6757403-3fb8548f2ca06343.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 5 LACP实现原理

- 基于IEEE802.3ad标准，是一种实现链路动态聚合与解聚合的协议
- 通过与对端交互LACPDU来协商主从设备、活动端口、活动链路等

![](https://upload-images.jianshu.io/upload_images/6757403-5c763b1fcb02a3e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 四、GVRP

### 1 基本概念

#### 1.1 应用实体与VLAN注册和注销 

- GVRP

  - GVRP基于GARP机制

  - 用于维护设备动态VLAN属性，实现动态分发、注册和传播VLAN属性

- 应用实体
  - 每个启动GVRP的端口对应一个GVRP应用实体

- VLAN的注册和注销
  - GVRP协议通过声明和回收声明实现VLAN属性的注册和注销

![](https://upload-images.jianshu.io/upload_images/6757403-4bab498e0ba61334.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 1.2 消息类型 

- Join消息
  - JoinEmpty：声明一个自身没有注册的属性
  - JoinIn：声明一个自身已经注册的属性
- Leave 消息
  - LeaveEmpty：注销一个自身没有注册的属性
  - LeaveIn：注销一个自身已经注册的属性
- LeaveAll 消息
  - LeaveAll 消息用来注销所有的属性

#### 1.3 定时器

- Join定时器
  - 控制Join消息（包括JoinEmpty和JoinIn消息）的发送
- Hold定时器
  - 控制Join 消息（包括JoinIn 和JoinEmpty）和Leave消息（包括LeaveIn 和LeaveEmpty）的发送
- Leave定时器
  - 控制属性注销
- LeaveAll定时器
  - 控制LeaveAll消息的发送

### 2 工作过程

#### 2.1 VLAN属性注册

- VLAN属性单向注册和双向注册

![](https://upload-images.jianshu.io/upload_images/6757403-a92f59a286dadb52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. VLAN属性单向注册 

- 在S1上创建静态VLAN2，通过VLAN属性的单向注册，将S2和S3的相应端口自动加入VLAN2。
  - 在S1上创建静态VLAN2后，E1启动Join定时器和Hold定时器，等待Hold定时器超时后，S1向S2发送第一个JoinEmpty消息，Join定时器超时后再次启动Hold定时器，再等待Hold 定时器超时后，发送第二个JoinEmpty消息。
  - S2上接收到第一个JoinEmpty后创建动态VLAN2，并把接收到JoinEmpty消息的E2加入到动态VLAN2中，同时告知E3启动Join定时器和Hold定时器，等待Hold定时器超时后向S3 发送第一个JoinEmpty消息，Join定时器超时后再次启动Hold定时器，Hold定时器超时之后，发送第二个JoinEmpty消息。S2上收到第二个JoinEmpty后，因为E2已经加入动态VLAN2，所以不作处理。
  - S3上接收到第一个JoinEmpty后创建动态VLAN2，并把接收到JoinEmpty消息的E4加入到动态VLAN2中。S3上收到第二个JoinEmpty后，因为E4已经加入动态VLAN2，所以不作处理。
  - 此后，每当Leaveall定时器超时或收到LeaveAll消息，设备会重新启动Leaveall定时器、Join定时器、Hold定时器和Leave定时器。S1的E1在Hold定时器超时之后发送第一JoinEmpty消息，再等待Join定时器+Hold定时器之后，发送第二个JoinEmpty消息，S2向S3发送JoinEmpty消息的过程也是如此。

2. VLAN属性双向注册

- 通过上述VLAN属性的单向注册过程，端口E1、E2、E4已经加入VLAN2，但是E3还没有加入VLAN2（只有收到JoinEmpty消息或JoinIn消息的端口才能加入动态VLAN）。为使VLAN2流量可以双向互通，需要进行S3到S1方向的VLAN 属性的注册过程。
  - VLAN属性的单向注册完成后，在S3上创建静态VLAN2（将动态VLAN转换成静态VLAN），E4启动Join定时器和Hold定时器，等待Hold定时器超时后，S3向S2发送第一个JoinIn 消息（因为E4 已经注册了VLAN2，所以发送JoinIn消息），Join定时器超时后再次启动Hold定时器，Hold定时器超时之后，发送第二个JoinIn消息。
  - S2上接收到第一个JoinIn后，把接收到JoinIn消息的E3加入到动态VLAN2中，同时告知E2启动Join定时器和Hold定时器，等待Hold定时器超时后，向S1发送第一个JoinIn消息，Join定时器超时后再次启动Hold定时器，Hold定时器超时之后，发送第二个JoinIn消息；S2上收到第二个JoinIn后，因为E3已经加入动态VLAN2，所以不作处理。
  - S1上接收到JoinIn之后，停止向S2发送JoinEmpty消息。此后，当Leaveall定时器超时或收到LeaveAll消息，设备重新启动Leaveall定时器、Join定时器、Hold定时器和Leave定时器。S1 的E1在Hold定时器超时之后就开始发送JoinIn消息。
  - S2向S3发送JoinIn消息。
  - S3收到JoinIn消息后，由于本身已经创建了静态VLAN2，所以不会再创建动态VLAN2。

#### 2.2 VLAN属性注销

![](https://upload-images.jianshu.io/upload_images/6757403-0a098bb4676824de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. VLAN属性的单向注销

- 当设备上不再需要VLAN2时，可以通过VLAN属性的注销过程将VLAN2从设备上删除。
  - 在S1上删除静态VLAN2，E1启动Hold定时器，等待Hold定时器超时后，S1向S2发送LeaveEmpty消息。LeaveEmpty消息只需发送一次。
  - S2上接收到LeaveEmpty，E2启动Leave定时器，等待Leave定时器超时之后E2注销VLAN2，将E2从动态VLAN2中删除（由于此时VLAN2中还存在端口E3，所以不会删除VLAN2），同时告知E3启动Hold定时器和Leave定时器，等待Hold定时器超时后，向S3发送LeaveIn消息。由于S3的静态VLAN2还没有删除，E3在Leave定时器超时之前仍然能够收到E4 发送的JoinIn消息，所以S1和S2上仍然能够学习到动态的VLAN2。
  - S3上接收到LeaveIn后，由于S3上存在静态VLAN2，所以E4不会从VLAN2中删除。

2. VLAN属性的双向注销

- 为了彻底删除所有设备上的VLAN2，需要进行VLAN属性的双向注销。
  - 在S3上删除静态VLAN2，E4启动Hold定时器，等待Hold定时器超时后，S3向S2发送LeaveEmpty消息。
  - S2接收到LeaveEmpty消息后，E3启动Leave定时器，等待Leave定时器超时之后E3注销VLAN2，将E3从动态VLAN2中删除并删除动态VLAN2，同时告知E2启动Hold定时器，等待Hold定时器超时后，向S1发送LeaveEmpty消息。
  - S1接收到LeaveEmpty消息后，E1启动Leave定时器，等待Leave定时器超时之后E1注销VLAN2，将E1从动态VLAN2中删除并删除动态VLAN2。

### 3 注册模式

- Normal模式
  - 允许动态VLAN在端口上进行注册，同时会发送静态VLAN和动态VLAN的声明消息
- Fixed模式
  - 不允许动态VLAN在端口上注册，只发送静态VLAN的声明消息
- Forbidden模式
  - 不允许动态VLAN在端口上进行注册，同时删除端口上除VLAN1外的所有VLAN，只发送VLAN1的声明消息。

> 手工配置的VLAN称为静态VLAN，通过GVRP协议创建的VLAN称为动态VLAN。 

## 五、局域网二层技术故障诊断

### 1 MAC相关故障

- 设备上无法创建正确的MAC表项
  - 查看是否配置错误导致MAC地址无法正确学习
  - 检查网络中是否存在环路引起广播风暴，导致MAC表项振荡
  - 检查是否配置了关闭MAC学习功能
  - 检查是否配置了黑洞MAC或基于接口的MAC地址限制
- MAC地址出现漂移
  - 检查设备是否使能MAC地址漂移检测功能
  - 确认检查对应的VLAN没有被屏蔽
  - 查询设备上的MAC漂移信息

### 2 动态VLAN无法生成

- 检查使能GVRP的接口是否up
- 检查是否全局、接口使能GVRP，否将GVRP应用于Trunk链路上
- 检查祖册模式是否正确
- 检查是否创建了相应的静态VLAN

# 结语
