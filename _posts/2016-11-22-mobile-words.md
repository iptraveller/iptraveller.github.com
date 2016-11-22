---
layout: post
title: 移动通信相关名词
excerpt: ""
categories: [knowledge]
comments: true
---

## IMSI

*	**International Mobile Subscriber Identity**

*	国际移动用户识别码,储存在SIM卡中，可用于区别移动用户的有效信息。其总长度不超过15位，同样使用0~9的数字，由3位MCC+2位MNC+10位MSIN构成。

## MCC

*	**Mobile country code**

*	移动信号国家码，唯一识别移动用户所属的国家，共3位，中国为460

第一位为地区编号  
0 - Test networks  
2 - Europe  
3 - North America and the Caribbean  
4 - Asia and the Middle East  
5 - Oceania  
6 - Africa  
7 - South and Central America  
9 - World-wide (Satellite, Air - aboard aircraft, Maritime - aboard ships, Antarctica)  

## MNC

*	**Mobile network code**

*	移动网络号码，用于识别移动客户所属的移动网络，2-3位

移动：00,02,07,08  
联通：01,06,09  
电信：03,05,11  

## MSIN

*	**mobile subscription identification number**

*	移动用户的识别号码，共10位

结构如下：  
CC+M0M1M2M3+ABCD  
CC由不同运营商分配，其中的M0M1M2M3和MDN号码中的H0H1H2H3可存在对应关系，ABCD四位为自由分配。

## MSISDN 
*	**Mobile Subscriber International ISDN/PSTN number**

*	指主叫用户为呼叫GSM PLMN中的一个移动用户所需拨的号码，作用同于固定网PSTN号码；是在公共电话网交换网络编号计划中，唯一能识别移动用户的号码。

MSISDN = CC + NDC + SN  
CC：Country Code，含义为国家码，中国为86  
NDC：National Destination Code，表示国内目的地码，也称网络接入号。  
SN：Subscriber Number，客户号码。

* 中国移动共计19个号段  
1340~1348,135,136,137,138,139,147,150,151,152,157,158,159,178,182,183,184,187,188

* 中国联通共计10个号段  
130,131,132,145,155,156,175,176,185,186

* 中国电信共计9个号段  
133,1349,149,153,173,177,180,181,189

* 14号段为上网卡专属号段，中国联通上网卡号段为145，中国移动上网卡号段为147，中国电信上网卡号段为149。

* 170、171号段为虚拟运营商专属号段

## ICCID

*	**Integrated circuit card identifier**

* 集成电路卡识别码即SIM卡卡号，相当于手机号码的身份证。 ICCID为IC卡的唯一识别号码，共有20位数字组成

前六位运营商代码：中国移动的为：898600；898602 ，中国联通的为：898601、898609，中国电信898603，898606。

* 中国移动

898600MFSSYYGXXXXXXP  
89： 国际编号  
86： 国家编号，86：中国  
00： 运营商编号，00：中国移动  
M： 号段，对应用户号码前3位  
0：159 1：158 2：150  
3：151 4-9：134-139 A：157  
B：188 C：152 D：147 E：187  
F： 用户号码第4位  
SS： 省编号  
北京01 天津02 河北03 山西04 内蒙古05 辽宁06 吉林07  
黑龙江08 上海09 江苏10 浙江11 安徽12 福建13 江西14  
山东15 河南16 湖北17 湖南18 广东19 广西20 海南21  
四川22 贵州23 云南24 西藏25 陕西26 甘肃27 青海28  
宁夏29 新疆30 重庆31  
YY： 编制ICCID时年号的后两位  
G： SIM卡供应商代码  
0：雅斯拓 1：GEMPLUS 2：武汉天喻 3：江西捷德 4：珠海东信和平  
5：大唐微电子通 6：航天九州通 7：北京握奇 8：东方英卡  
9：北京华虹 A ：上海柯斯  
XXXXXX： 用户识别码  
P： 校验位

* 中国联通

898601YY8SSXXXXXXXXP  
89： 国际编号  
86： 国家编号，86：中国  
01： 运营商编号，01：中国联通  
YY： 编制ICCID时年号的后两位  
8： 中国联通ICCID默认此为为8  
SS：2位省份编码  
10内蒙古 11北京 13天津 17山东 18河北 19山西 30安徽 31上海 34江苏  
36浙江 38福建 50海南 51广东 59广西 70青海 71湖北 74湖南 75江西  
76河南 79西藏 81四川 83重庆 84陕西 85贵州 86云南 87甘肃 88宁夏  
89新疆 90吉林 91辽宁 97黑龙江  
XXXXXX： 卡商生产的顺序编码  
P： 校验位  

* 中国电信

898603YYXMHHHXXXXXXP  
89：国际编号  
86：国家编号，86：中国  
03：运营商编号，03：中国电信  
YY：编制ICCID时的年号后两位  
M ：保留位，固定为0  
HHH：本地网地区代码，位数不够前补零。如上海区号为021，则HHH为'021’；长沙区号为0731，则HHH为‘731’，测试卡代码为001  
XXXXXXP：7位流水号，建议前2位作为批次号  

Author: chenxiawei@gmail.com   

## 参考材料

(1) [https://en.wikipedia.org/wiki/Mobile_country_code](https://en.wikipedia.org/wiki/Mobile_country_code)   
(2) [https://en.wikipedia.org/wiki/Mobile_country_code](https://en.wikipedia.org/wiki/Mobile_country_code)  
(3) [https://zh.wikipedia.org/wiki/%E4%B8%AD%E5%9B%BD%E5%86%85%E5%9C%B0%E7%A7%BB%E5%8A%A8%E7%BB%88%E7%AB%AF%E9%80%9A%E8%AE%AF%E5%8F%B7%E7%A0%81](https://zh.wikipedia.org/wiki/%E4%B8%AD%E5%9B%BD%E5%86%85%E5%9C%B0%E7%A7%BB%E5%8A%A8%E7%BB%88%E7%AB%AF%E9%80%9A%E8%AE%AF%E5%8F%B7%E7%A0%81)  
(4) [http://baike.baidu.com/item/iccid](http://baike.baidu.com/item/iccid)  



