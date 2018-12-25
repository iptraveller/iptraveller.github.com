---
layout: post
title: 设计思想
excerpt: ""
categories: [design]
comments: true

---


	随着应用程序复杂性的增长，单体应用的益处逐渐减少。它们变得更难理解，而且失去了敏捷性，因为工程师很难推断和修改代码。
	对付复杂性的最好方法之一是将明确定义的功能分成更小的服务，并让每个服务独立迭代。这增加了应用程序的灵活性，允许根据需要更轻松地更改部分应用程序。每个微服务可以由单独的团队进行管理，使用适当的语言编写，并根据需要进行独立扩缩容。
	只要每项服务都遵守强有力的合约，应用程序就可以快速改进和改变。
	
[https://jimmysong.io/kubernetes-handbook/cloud-native/cloud-native-philosophy.html](https://jimmysong.io/kubernetes-handbook/cloud-native/cloud-native-philosophy.html)