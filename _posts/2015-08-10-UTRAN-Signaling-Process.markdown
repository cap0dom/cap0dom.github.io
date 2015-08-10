---
date: 2015-08-10
title:[连载1] UTRAN 基本信令流程-基本概念和CELL状态
categories:工作 学习
tags: 通信 UMTS
---

#开场白

原本，最开始架这个个人[blog](http://www.cap0dom.com)的愿景是：冲着做一个“full stack”的工程师。然后，上帝很喜欢和逗逼开玩笑，于是，TA 毫不犹豫的和我开了一个玩笑－被一脚踢到了CT。不过，有个人曾经这样告诉我，当上帝给你关闭了一扇门的时候，TA 也会为你开一扇窗。迄今为止，我觉得这扇窗就是，我还保持着一种相对愉快的心态在工作学习。当然，这要感谢XXX部门的几十个同事的帮助和照顾。

%#^&@,跑题了，回来。

因为工作需要，最近这段时间开始学习**UMTS**的相关知识，所以，顺便把周总结的一些东西摘取部分出来放在这里，和大家分享一下。今后，应该还会持续更新。

至于，我是不是一直会走这个CT的道路，我现在也不知道。但是，我很享受现在的学习过程。虽然每天都会遇到很多问题，但是每天都保持一种补脑的状态，那也是不错的。对于，有些东西的热爱，我也从来没有减少，那就是兴趣。说到这里，我想起了之前看到公司内部某某说的一句话：

> 干一行，爱一行

嗯... check it ？

<br/>

#基本概念

先介绍几个基本概念。

RNS：Radio Network Subsystem（无线网络子系统）。它可以是一个UTRAN或者是一个UTRAN的一部分。因为UTRAN是介于CN和UE之间，负责CN与UE之间通信的，它包含1个或者多个RNC和1个或者多个NOdeB。而RNS只包含一个RNC和这个RNC相连接（控制）的NodeB，一个RRC连接需要一个RNS来管理。因此，可以说RNS是一个UTRAN或者UTRAN的一部分。RNS负责分配、释放无线资源，用于UE和UTRAN之间的连接。

SRNS：Source RNS（资源无线网络子系统）。它是直接负责并维持UE和UTRAN相连的RNS。只要UE和UTRAN之间存在一个RRC连接，就需要一个SRNS负责资源管理，SRNS是可以直接与CN相连接的，SRNS里面的RNC叫SRNC。

DRNS：Drift RNS（漂移无线网络子系统）。它的功能和RNS是一样的，只是它是一种负责UE和UTRAN之间连接的某个特定的RNS。当UE和UTRAN之间的连接使用的是非SRNS控制的一个CELL时，这个CELL对应的RNC叫DRNC，对应的RNS叫DRNS，这个DRNS可以为SRNS提供无线资源。简单的说，就是DRNS它是无法实现一个完整的UE与CN之间的连接，它只能实现UE和DRNS连接，然后DRNS与SRNS连接，最后由SRNS与CN相连。

NOTE：这里需要注意区分SRNC和DRNC（也即区分SRNS和DRNS）。从上面可以看出，最直接区分DRNC与SRNC的办法就是，二者中与CN直接相连的就是SRNC，而不能与CN之间相连的就为DRNC。然而，DRNC和SRNC并不是绝对的。在静态迁移中，完成静态迁移之后，原来的DRNC就转变为SRNC了，而原来的SRNC也断去与CN之间的连接，就可能为别的RNC提供无线资源，也就成为DRNC。

CRNC：Controlling RNC（控制RNC）。负责控制UTRAN特定接入点的RNC。一个UTRAN有且只有一个CRNC。CRNC对UTRAN的特定接入点的逻辑资源进行总体控制。

RNTI：Radio Network Temporary Identity（无线网络临时标识）。在RRC连接存在情况下，RNTI主要用于MAC协议或者公共传输信道上面标识UE。它主要分为以下几种RNTI。

C-RNTI（CELL-RNTI）主要用于小区内唯一标识RRC连接的UE，

U-RNTI（UTRAN RNTI）主要用于在UTRAN内唯一标识UE。是分配给与UTRAN有一条RRC连接的UE。

S-RNTI（SRNC RNTI）由SRNC分配并且唯一标识UE的RNTI。

D-RNTI（DRNC RNTI）有DRNC分配并且唯一标识UE。

还有很多其他的RNTI，比如DSCH-RNTI、SI-RNTI、P-RNTI…可以看出，不同的RNTI主要区别是它们标识UE的范围和分配者不同。

<br/>

#UE状态

从UE所处的模式来看，UE主要为两种模式：空闲模式和连接模式。这两种模式分为5种状态。其中空闲模式为IDLE状态，连接模式分为CELL_DCH、CELL_FACH、CELL_PCH、URA_PCH4种状态。

空闲模式和连接模式的分割点是RRC连接的建立。也就是说：在上电之后，UE实际上还是处于空闲模式的，一直到RRC连接建立完成之前都是处于空闲模式。在空闲模式下，UE的识别是通过非接入层IMSI、TMSI、P-TMSI来标识的。在空闲模式下，UTRAN是不保留此模式下的UE信息（因为IDLE状态下，UTRAN没有为UE建立上下文），所以只能访问这个小区下所有UE或者同一个寻呼时刻的所有UE。只有当UE收到网络侧的RRC连接证实消息之后，UE才从空闲模式进入连接模式。进入连接模式之后，UE会被分配一个U-RNTI在公共信道上作为UE的标识使用。连接模式下不同的UE状态，可以反映UE的连接级别和使用的哪些信道。

![UE状态图](http://7xj6ej.com1.z0.glb.clouddn.com/UE_STATE11.jpg)


CELL_DCH状态

状态特征：

1） UE被分配专用物理信道DCH（上行、下行）、上行共享传输信道USCH、下行共享传输信道DSCH；

2） 根据UE当前的活动集可以知道UE所在小区；

3） 根据UTRAN的分配情况，UE可以使用专用物理信道（上行、下行）、上行共享传输信道USCH、下行共享传输信道DSCH或者这些信道的组合。

进入CELL_DCH状态的途径有：

1）UE在空闲模式下，RRC连接建立在DCH上面，UE从IDLE模式进入CELL_DCH；

2）UE处于CELL_FACH状态下使用公共传输信道，通过信道切换后使用专用传输信道，UE从CELL_FACH状态进入CELL_DCH状态。

3）智能终端的新特性可以是UE从CELL_PCH直接进入CELL_DCH状态。（这种途径一般很少遇到）

CELL_FACH状态

 状态特征：

1） 没有给UE分配专用物理信道；

2） UE连续监听一个下行FACH信道；

3） 为UE分配一个默认的上行公共信道（如：RACH）或者上行共享信道，使之能够在接入过程中的任何时间内使用；

4） UE的位置为小区级，具体的位置为UE最近一次小区更新时报告的小区。

在CELL_FACH状态的UE，可以进行的动作

1） 监听一个FACH；

2） 监听当前服务小区的BCH传输信道，解码系统信息消息；

3） 在小区变为另外一个UTRAN小区是，发起一个小区更新过程；

4） 除非选择一个新小区，否则使用当前小区分配的C-RNTI作为公共传输信道上UE的标识；在RACH上传输上行控制信令和小数据包。

 在CELL_FACH状态下，如果数据业务在一段时间里未被激活，UE将进入CELL_PCH状态，以减少功率的损耗。并且，当UE暂时脱离CELL_PCH状态执行小区更新，更新完成后，如果UE和网络侧均无数据传输需求，它将返回CELL_PCH。

CELL_PCH状态

1）没有为UE分配专用信道

2）UE使用非连续接收（DRX）技术，在某个特定的寻呼时刻监听PCH传输信道上的信息

3）不能有任何上行的活动

4）UE的位置在小区级为UTRAN所知，具体为UE在CELL_FACH状态时最近一次发起小区更新时所报告的小区

在CELL_PCH状态，UE进行以下活动：

1）根据DRX 周期监听寻呼时刻，并接收PCH上的寻呼消息

2）监听当前服务小区的BCH传输信道，以解码系统信息

3）当小区改变时发起小区更新过程

4）在该状态下不能使用DCCH逻辑信道。如果网络试图发起任何活动，它需要在UE所在小区的PCCH逻辑信道上发送一个寻呼请求。

UE转换到CELL_FACH状态的方式有两个，一是通过UTRAN寻呼，二是通过任何上行接入。

URA_PCH状态

URA_PCH状态具有如下特征：

1）没有为UE分配专用信道

2） UE使用DRX技术（节能），在某个特定的寻呼时刻监听PCH传输信道上的信息

3）不能有任何上行的活动

4）UE的位置在URA级为UTRAN所知，具体为UE在CELL_FACH状态时最近一次发起URA更新时所报告的URA

在URA_PCH状态，UE进行以下活动：

1）根据DRX周期监听寻呼时刻，并接收PCH上的寻呼消息

2）监听当前服务小区的BCH传输信道，以解码系统信息

3）当URA改变时发起URA更新过程

在该状态下不能使用DCCH逻辑信道。如果网络试图发起任何活动，它需要在UE所在URA的PCCH逻辑信道上发送寻呼请求。

在URA_PCH状态， 没有资源分配给数据传输用。因此，如果UE有数据要传送，需要首先转换到CELL_FACH状态。

总的来看就是：IDLE状态只能在CELL_DCH、CELL_FACH之间进行状态变换；而CELL_DCH、CELL_FACH可以分别进入其他3种连接模式；CELL_PCH、URA_PCH之间不能进行状态变换，而且都可以进入CELL_FACH状态，都不进入CELL_DCH状态。


<br/>

