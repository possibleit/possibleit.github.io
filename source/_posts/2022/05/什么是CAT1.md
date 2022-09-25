---
title: 什么是CAT 1
tags:
  - 通信
abbrlink: 11926
date: 2022-05-30 23:23:56
---

## 什么是CAT 1


随着 5G 时代的悄然到来，高速率、大容量、低时延的网络特性深刻冲击了普罗大众的认知，而 “万物互联” 的概念也在这一过程中逐渐深入人心。

但是并不是所有场景都需要5G链接的，事实上物联网场景中有接近三成的场景仅需要中速率的通信即可。目前国内运营商2/3G逐步退网，以前海量的2/3G物联网链接需求将会由低成本、兼顾速率、功耗、成本的<font color="red">LTE CAT1</font>代替。

<!--more-->

## CAT 1技术定义

CatX 全称为 LTE UE CategoryX，是 3GPP 用来衡量用户终端设备的无线性能的标准。UE 指用户设备（user equipment），Category 是分类的意思，所以 Cat X 这个词用来衡量设备传输速率的等级，根据 3GPP Release 定义，<font color="red">Cat X 被分为 1-15 个等级，区分传输速率</font>，其中 Cat 1-5 在 R8 组，Cat 6-8 在 R10 组，Cat 9-10 在 R11 组，Cat 11-15 在 R12 组，数字越大那么，传输速度也就越快。UE Category 包含很多的无线特性，其中最重要的一个就是 UE 支持的速率。2009 年 3 月，3GPP 在 Release8（以下简称 R8）中正式提出 LTE，并同步推出 LTE Cat.1（以下简称 Cat.1）、Cat.2、Cat.3、Cat.4、Cat.5 这 5 个终端类别，其中定义的<font color="red"> Cat.1 上 / 下行峰值速率为 5/10Mbps</font>。

3GPP R13提出CAT 1 bis标准，采用单天线设计，并对算法做进一步简化，在降低复杂度的同时，元件成本也得到降低，基于 Cat.1 bis 的 Cat.1 的全兼容性，现有的基站版本可同时支持 Cat.1 和 Cat.1 bis 终端。单天线劣势是带来 4dB 的下行平均性能损失。

## CAT 1终端特点
 
### 频段要求

![v2-42cb375e4d17a979b6c6775ac9970340_r.png](http://tva1.sinaimg.cn/large/006WUTFIgy1h2quq65e6nj30j807vta3.jpg =400x180)

### 通信协议需满足R8及以上

### UE 能力

![006WUTFIgy1h2qurmrplej30j803swg5.jpg](http://tva1.sinaimg.cn/large/006WUTFIgy1h2qurmrplej30j803swg5.jpg  =400x100)

![v2-bb6cbdf86c9977935d2e61b613167643_1440w.png](http://tva1.sinaimg.cn/large/006WUTFIgy1h2qurt6v1yj30j9035dgg.jpg =400x100)

##  Cat 1 通信特点

Cat.1 通信能力属于<font color="red">中低速档次</font>，可以视为低配的 LTE 技术。能满足一定高速移动、时延敏感、支持语音、低成本和低功耗的场景需求，业务上与 2G/3G、CatM1（以下简称 eMTC）、CatNB1（以下简称 NB-IoT）、Cat.4 有一定重合。Cat.1、 eMTC 和 NB-IoT 是专门针对物联网市场的，Cat.1 相比其他几种技术具有如下优势：

### Cat.1 与 NB-IoT 相比，具有网络覆盖、建网成本和通信优势

Cat.1 承载于现有 LTE 网络，而 LTE 网络在全球均有良好覆盖，运营商无需额外升级基站的硬件配置，<font color="red">只需对基站的参数进行配置，即可实现 Cat.1 终端接入</font>。NB-IoT 则需要新建基站增加硬件资源投入，，现有的 NB-IoT 网络覆盖远低于 LTE 网络。在通信方面，NB-IoT 受限于数据传输速度和移动速度、时延需求， 通常适用于小码流、静止状态的场景如三表（水表、电表、燃气表）。Cat.1 可以传输更大的码流，且具有很好的<font color="red">移动性与语音功能</font>。

### Cat.1 与 2G/3G 相比，具有网络覆盖和通信优势

随着 2G/3G 减频退网工作的实施，2G/3G 市场进一步萎缩，势必无法成为未来物联网的发展方向。未来 NB-IoT+4G+5G 将作为物联网海量终端的主要蜂窝承载网络。

### Cat.1 与 eMTC 相比，具有<font color="red">建网成本</font>优势

运营商现有 LTE 基站若想支持 eMTC 则需要支付额外的费用来升级网络。目前国内运营商在 eMTC 网络上并无过多投资，eMTC 在国内的生态、产业链发展均不理想。

### Cat.1 与 Cat.4 相比，具有成本优势

Cat.1 与 Cat.4 网络兼容，且具有 Cat.4 一样的优势。虽然速率和信号质量都稍逊一筹，但 Cat.1 采用单天线、低存储方案设计，硬件架构更简单，拥有更高的集成度和更低的功耗、成本。 Cat.4 在功耗、集成度、价格方面很难满足部分行业的物联网应用需求，而 Cat.1 适合物联网目前的商业模式，受到国内产业链的青睐，可承接上述物联网应用场景的能力。

未来数百亿的物联网连接中，对于网络的能力需求是不一样的，10的节点需要大带宽高速的通信技术，如高速率 4G（LTE Cat.4 以上）、5G 等；30%的节点需要中等速度传输技术，如 2G/3G、Cat.1 与 eMTC；而60%的节点需要低速率的连接技术，如 NB-IoT、LoRa 等。Cat.1 有望成为30%的中速场景中的主要技术。

![v2-fc7f9b78e0ef838adfa4e6e4caedd852_1440w.jpg](http://tva1.sinaimg.cn/large/006WUTFIgy1h2qutgz0ghj30hd09p3z7.jpg =500x300)
