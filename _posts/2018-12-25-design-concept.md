---
layout: post
title: 设计思想
excerpt: ""
categories: [design]
comments: true

---

* **云原生的设计哲学**

  > 随着应用程序复杂性的增长，单体应用的益处逐渐减少。它们变得更难理解，而且失去了敏捷性，因为工程师很难推断和修改代码。  
  > 对付复杂性的最好方法之一是将明确定义的功能分成更小的服务，并让每个服务独立迭代。这增加了应用程序的灵活性，  
  > 允许根据需要更轻松地更改部分应用程序。每个微服务可以由单独的团队进行管理，使用适当的语言编写，并根据需要进行独立扩缩容。  
  > 只要每项服务都遵守强有力的合约，应用程序就可以快速改进和改变。

  [https://jimmysong.io/kubernetes-handbook/cloud-native/cloud-native-philosophy.html](https://jimmysong.io/kubernetes-handbook/cloud-native/cloud-native-philosophy.html)
  
* **如何降低软件的复杂性**

  > 复杂性的来源主要有两个：代码的含义模糊和互相依赖。
  
  [http://www.ruanyifeng.com/blog/2018/09/complexity.html](http://www.ruanyifeng.com/blog/2018/09/complexity.html)
  
* **微服务架构的理论基础 - 康威定律**

  > 设计系统的组织，其产生的设计等同于组织之内、组织之间的沟通结构。用通俗的说法就是：组织形式等同系统设计。  
  > 人与人的沟通是非常复杂的，一个人的沟通精力是有限的，所以当问题太复杂需要很多人解决的时候，我们需要做拆分组织来达成对沟通效率的管理  
  > 组织内人与人的沟通方式决定了他们参与的系统设计，管理者可以通过不同的拆分方式带来不同的团队间沟通方式，从而影响系统设计  
  > 如果子系统是内聚的，和外部的沟通边界是明确的，能降低沟通成本，对应的设计也会更合理高效  
  > 复杂的系统需要通过容错弹性的方式持续优化，不要指望一个大而全的设计或架构，好的架构和设计都是慢慢迭代出来的
  
  [https://yq.aliyun.com/articles/8611](https://yq.aliyun.com/articles/8611)
  
* **CAP 定理的含义**
  
  > 分布式系统有三个指标。Consistency、Availability、Partition tolerance；它们的第一个字母分别是 C、A、P。  
  > Eric Brewer 说，这三个指标不可能同时做到。这个结论就叫做 CAP 定理。
  
  [http://www.ruanyifeng.com/blog/2018/07/cap.html](http://www.ruanyifeng.com/blog/2018/07/cap.html)
  
* **When not to use microservices**
  
  > 程序员知道种种优势，却对代价一无所知。  
  > 所有软件系统都可以分解为两个主要元素：策略和细节。策略元素包含所有业务规则和过程。策略是系统的真正价值所在。  
  > 架构师的目标是为系统创建一个外形。这个外形将策略看做是系统中最重要的元素，同时做到细节与策略无关。这使得有关细节的决策可以延迟和推后。
  
  [http://zhuanlan.51cto.com/art/201812/588622.htm](http://zhuanlan.51cto.com/art/201812/588622.htm)
  [https://www.feval.fr/posts/microservices/](https://www.feval.fr/posts/microservices/)
