---
layout: post
title: 5G专网
excerpt: ""
categories: [5g]
comments: true

---

## 概念
5G专网称为Non-public Networks，也可称为Private Networks

## 场景
* 工业物联网（IIoT）：制造业、采矿、物流、港口

## 技术方案
### 两种形式
* 独立建立私网
* 共享运营商网络（网络切片）

![01](/img/2020-03-10-01.png) 

### 七种部署方案
* 企业建立的隔离5G局域网（本地5G频率，完全私有，不共享）

![02](/img/2020-03-10-02.png) 

企业在其场所（站点/建筑物）内部署5G网络全套（gNB，UPF，5GC CP，UDM，MEC）。企业中的5G频率是本地5G频率，而不是移动运营商的许可频率。
**优点：**
隐私和安全性：专用网络与公用网络物理上分离，提供完整的数据安全性
超低延迟：由于设备和应用程序服务器之间的网络延迟在几毫秒内，因此可以实现URLLC应用程序服务。
建筑物无光纤：无需进行回程以保持本地服务正常运行。5G服务可以立即提供给没有光回程链路的企业，例如农村地区的工厂。
即使发生移动运营商的5G网络故障：即使移动运营商的设施烧毁，该公司的5G专用网络也可以正常工作。
**缺点：**
部署成本：普通企业要自费购买和部署全套5G网络并不容易。特别是对于小型企业。
运营人员：现有的私有LAN（有线以太网LAN，无线Wi-Fi LAN）运营团队没有专门知识来构建和运营5G网络。企业需要有合适的工程师。

## 参考资料
* [7 Deployment Scenarios of Private 5G Networks](https://www.netmanias.com/en/post/blog/14500/5g-edge-kt-sk-telecom/7-deployment-scenarios-of-private-5g-networks)
