---
layout: post
title: 手机微信账号识别
excerpt: "通过抓包分析手机微信内部账号的提取方法"
categories: [app-identify]
comments: true
---

## 报文首部特征

* 报文类型：TCP报文

* 报文IP： 无固定目的IP

* 报文端口：目前抓包观察服务器端口号为80等

## 报文数据特征

### 特征1

* TCP数据区前4个字节为TCP数据段中微信内容的长度

* TCP数据区开始，偏移4个字节，取第5个字节开始的4字节长度必须为**0x00,0x10,0x00,0x01**认为可能是手机微信协议报文；

* 从匹配的最后位置开始，查找到字节0xbf（0xbf所处的位置可能不固定），跳过一个字节，必须是0x5f;

* 从0x5f后，偏移4个字节，取4个字节的十六进制数字，即为手机微信的特征id；

![wx1](/img/wx1.png) 
![wx2](/img/wx2.png) 
![wx3](/img/wx3.png) 

### 特征2

* HTTP请求报文

* Host为 Host: mmsns.qpic.cn，或者有可能是其他Host

* referer中的原路径中带有的uin=后的数字为微信内部id的十进制表示形式

![wx4](/img/wx4.png) 

Author: chenxiawei@gmail.com  
