---
title: GRE
tags:
  - web
abbrlink: 20459
date: 2022-04-20 22:57:39
---

## GRE  

在了解GRE之前首先要了解什么是overlay，overlay是一种将二层网络（业务的）架构在三层/四层（传统网络的）报文中进行传递的网络技术。

<!--more -->

NVGRE是将以太网报文封装到GRE内的一种隧道转发模式。采用24比特识别二层网络分段、称为VSI，类似于VLAN ID的作用。为了使NVGRE利用承载网络路由的均衡性，NVGRE在GRE扩展字段flow ID，要求物理网络能够识别GRE隧道的扩展信息，并以flow ID进行流量分担。

<font color="red">GRE</font>协议是思科公司提出，通用路由封装协议