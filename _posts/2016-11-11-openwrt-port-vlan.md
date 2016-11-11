---
layout: post
title: Openwrt端口vlan配置收发带tag报文
excerpt: ""
categories: [openwrt]
comments: true
---

## 拓扑

	+---------+         +---------+
	|         |         |         |
	|   port3 +---------+ gi0/1   |
	|         |         |         |
	+---------+         +---------+
	  device1             device2
	  
主测设备device1使用基于Atheros AR9344 SoC芯片设备，辅测设备device2使用普通3层交换机

主测设备lan口中的的port3口与辅测设备的gi0/1口直连，测试二者之间收发带vlan tag 5的报文通路

## 配置

### 交换机

	interface GigabitEthernet 0/1
	 switchport mode trunk
	 switchport trunk native vlan 3

	interface VLAN 5
	 ip address 2.2.2.10 255.255.255.0

创建SVI 5，接口配置成trunk口并且native vlan配置为非5。

### openwrt设备

设备默认eth0为交换芯片口

* 将端口3从默认vlan1中移出并加入vlan5
{% highlight bash %}
	swconfig dev eth0 vlan 1 set ports '0 1 2 4'
	swconfig dev eth0 vlan 5 set ports '0t 3t'
{% endhighlight %}
* 使能switch配置
{% highlight bash %}
	swconfig dev eth0 set apply
{% endhighlight %}
* 创建vlan接口并配置IP地址
{% highlight bash %}
	vconfig add eth0 5
	ifconfig vlan5 2.2.2.3 netmask 255.255.255.0
{% endhighlight %}
* 查看配置的交换口
{% highlight bash %}
	admin@Ruijie:/# swconfig dev eth0 show
	Global attributes:
			enable_vlan: 1
	Port 0:
			pvid: 1
			link: port:0 link:up speed:1000baseT full-duplex txflow rxflow 
	Port 1:
			pvid: 1
			link: port:1 link:down
	Port 2:
			pvid: 1
			link: port:2 link:down
	Port 3:
			pvid: 1
			link: port:3 link:up speed:100baseT full-duplex auto
	Port 4:
			pvid: 1
			link: port:4 link:down
	VLAN 1:
			vid: 1
			ports: 0t 1 2 4 
	VLAN 5:
			vid: 5
			ports: 0t 3t 
{% endhighlight %}
* 查看创建的vlan接口
{% highlight bash %}
	admin@Ruijie:/# ifconfig vlan5
	vlan5     Link encap:Ethernet  HWaddr 58:69:6C:18:AB:72  
			  inet addr:2.2.2.3  Bcast:2.2.2.255  Mask:255.255.255.0
			  UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
			  RX packets:7 errors:0 dropped:0 overruns:0 frame:0
			  TX packets:7 errors:0 dropped:0 overruns:0 carrier:0
			  collisions:0 txqueuelen:0 
			  RX bytes:512 (512.0 B)  TX bytes:574 (574.0 B)
{% endhighlight %}
测试结果
{% highlight bash %}
	admin@Ruijie:/# ping 2.2.2.10
	PING 2.2.2.10 (2.2.2.10): 56 data bytes
	64 bytes from 2.2.2.10: seq=0 ttl=64 time=0.825 ms
	64 bytes from 2.2.2.10: seq=1 ttl=64 time=0.755 ms
	64 bytes from 2.2.2.10: seq=2 ttl=64 time=0.905 ms  
{% endhighlight %}

* 通过uci进行配置保存
{% highlight bash %}
	config interface 'vlan'
			option ifname 'eth0.5'
			option proto 'static'
			option ipaddr '2.2.2.3'
			option netmask '255.255.255.0'
			option type 'bridge'
			option force_link '1'
	
	config switch_vlan
			option device 'switch0'
			option vlan '1'
			option ports '0 1 2 4'
	
	config switch_vlan
			option device 'switch0'
			option vlan '5'
			option ports '0t 3t'
{% endhighlight %}
**如果想把wan口(eth1)也加入到同一个vlan中进行收发，则只需要修改network配置**
{% highlight bash %}
	config interface 'vlan'
		option ifname 'eth0.5 eth1.5'
{% endhighlight %}
## 配置命令

### swconfig
{% highlight bash %}
	swconfig dev <dev> [port <port>|vlan <vlan>] (help|set <key> <value>|get <key>|load <config>|show)
{% endhighlight %}
1. VLAN配置中的vid不一定要与VLAN编号一致，可以配置不一样，例如VLAN编号1的vid为10
{% highlight bash %}
	VLAN 1:
			vid: 10
			ports: 0t 1 2 4 
{% endhighlight %}
2. VLAN配置中的ports中，数字后带t表示收发带tag报文，例如端口3收发带vlan tag 5报文
{% highlight bash %}
	VLAN 5:
			vid: 5
			ports: 0t 3t 
{% endhighlight %}
3. ports配置中的pvid表示，收到该端口收到不带tag报文时，默认转发的vlan，例如端口3收到不带tag报文后默认在vlan 3中进行转发
{% highlight bash %}
	Port 3:
			pvid: 3
			link: port:3 link:up speed:100baseT full-duplex auto
{% endhighlight %}
### vconfig
{% highlight bash %}
	Usage: vconfig COMMAND [OPTIONS]

	Create and remove virtual ethernet devices

			add             IFACE VLAN_ID
			rem             VLAN_NAME
			set_flag        IFACE 0|1 VLAN_QOS
			set_egress_map  VLAN_NAME SKB_PRIO VLAN_QOS
			set_ingress_map VLAN_NAME SKB_PRIO VLAN_QOS
			set_name_type   NAME_TYPE
{% endhighlight %}
Author: chenxiawei@gmail.com  

## 参考材料

(1) [https://wiki.openwrt.org/doc/uci/network/switch](https://wiki.openwrt.org/doc/uci/network/switch) 

