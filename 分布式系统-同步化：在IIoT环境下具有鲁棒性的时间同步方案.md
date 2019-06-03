---
title: 分布式系统-同步化：在IIoT环境下具有鲁棒性的时间同步方案
date: 2019-05-13 13:40:34
tags: 
 - 中间件 
 - 同步化
 - 物联网
categories: 物联网
mathjax: true
---
> 本文根据SCI 1区期刊 《IEEE Transactions on Industrial Informatics, 2018》文献《A Robust Time Synchronization Scheme for Industrial Internet of Things》记录。记录旨在理解该方法，因此只摘录重点信息，若想了解文章详细内容请参考论文原文（在下方给出）。

[原文地址](https://sci-hub.se/https://ieeexplore.ieee.org/abstract/document/8008773/)

# Main Idea

本文提出的方法——R-Sync（由三部分组成）：
- 时间同步的准备工作是利用洪泛协议进行根的选择和生成树的构建。
- 初始时间同步是通过组合SRP和RRP进行消息交换算法。
- Pulling过程是为了避免孤立的节点。
## Pulling timer过程
当N一定时间未同步时间，**PT（Pulling Timer）**过期，则广播一个时间同步消息请求：
此时，
- 若有PN节点，则返回一个应答消息给N；
- 若消息丢失/无PN邻居，则将N节点的PT设为等待状态；
- 若有多个PN节点，则由节点N选择第一个应答的PN应答消息作为同步标准，并忽略其它消息。
## Main idea同步过程
1. 起始状态，所有节点处于**UNs（Undefine Node）**状态
1. 第一轮，通过节点标识随机选择根节点（root）
1. 根节点（root）发布广播消息给邻接节点，收到该消息的邻居节点建立PT（Pulling Timer）
1. 邻居节点继续向下转发消息，直到整个网络建立PT（PT的初始值根据再生成树中的等级产生）
1. 进行同步化后的节点取消PT，未同步化的节点过期后广播消息进行同步化，若无PN（被动节点）或消息丢失则重置计时器PT
1. 逐步通过PT将同步范围外的节点拉入同步范围，并将需要广播的节点转换为**BN（BackBone Node）中枢节点**
1. 通过SRP和RRP进行同步化，逐步在整个网络实现时间同步
1. 进行第二轮同步化，选取新的root节点，条件：在上一轮root节点可通讯范围内，且剩余能源最高（若节点无能源感知功能，则通过信号强度判别）

# Timer Synchronization Process
- **时间同步准备**
    1. 初始化root节点变为BN节点，且等级为0
    2. 发送广播SetT消息，接收到的设置为L+1（L为发送方的等级）
    3. 重置PT，并继续转发消息
    4. 直到所有节点都具有PT
- **初始化时间同步（4步）**
    1. root节点广播一个**初始化信息（init消息）**
    2. 接收到该消息的节点（且由PT的）取消PT，设置ST（Sync Timer）用来选择谁是BN节点：当ST到期，则节点从UN（Undefine node）转换为BN，并广播发送一个**同步消息（Sync message）**，同步消息目的地址为root节点，且记录着该节点的local time（T1）
    3. 当某节点收到了（第2步中的）Sync message：
        a. 若该节点为root的邻居UN节点，则记录收到消息时的本地时间戳为（T5），随后取消ST，将自身转换为PN
        b. 若该节点为root节点，则记录接受到消息的本地时间戳（T2） ，并发送**应答消息（Ack message）**，同时目的地址为原发送Sync message的节点，并记录此时的本地时间戳（T3）
    4. 当某节点收到了（第3步中的）Ack message
        a. 若该节点为**Ack消息**中的目标节点，记录收到Ack消息的本地时间戳（T4），通过SRP（发送-接受协议）及T1-T4值计算时间偏差，使该节点可与root节点同步（本地时钟时间+计算而得的时间偏差）。并广播一个**初始化（Init message）消息**。
        b. 若该节点为PN节点，则记录收到该同步化消息的时间戳RTS（收到消息的时间戳），并使用该RTS与T5计算时间偏差（RRP方法），使得与邻域内其它BN节点同步。（其它节点未被初始化入同步区域的原因为：其邻居节点为PN，或消息丢失了收不到，称为孤立节点，isolated node）

重复以上4步操作，直到叶子节点上的BN节点广播一个**初始化（Init message）消息**无应答为止。
- **Pulling过程根节点选取，进行下一轮同步化**
    当一个UN节点的PT到期，它会广播发送一个**Pulling消息**，若该消息丢失无应答则使该PT重置。
    若该消息未丢失，某一节点收到了该消息。该节点将从PN转换为BN。然后广播一个**初始化消息（Init message）**。
以此重复。
- **为下一轮同步选择根节点（root）**
当上一轮根节点的邻居节点同步化完成后，所有邻居节点将发送自己的剩余能量给该root节点，然后该root节点选择剩余能量最充足的节点发送**（ChRoot message）节点重选消息**，当新节点接收到该消息后，该新节点将成为新的root节点。
# Timers Initialization
- **PT：**
L：消息传入所经过的转发跳数
AT_Period：同步化过程中平均同步每一跳节点的时间
Init_Timer：第一次全局初始化PT所消耗的时间
则，PT的计算公式为：
$$
L*AT\\_Period+Init\\_Time
$$
- **ST：**
为了让传播更快速，应让BN尽可能地分开较远距离。
距离公式：
$$
P_{rcd} = c * P_{tx}/(d^\alpha)
$$
其中$P_{rcd} $表示接收信号功率；$P_{tx}$表示发送信号功率；$c、\alpha$是与传输模型相关的参数
ST的计算公式为：
$$
\alpha+1/distance
$$

该时间同步化方法描述如上。