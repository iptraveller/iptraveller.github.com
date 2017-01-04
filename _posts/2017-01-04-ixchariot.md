---
layout: post
title: IxChariot打流搭建
excerpt: ""
categories: [tools]
comments: true
---

## 环境搭建

### 软件安装

打流的一端设备安装IxChariot软件，另一端设备安装Endpoint软件。

### 软件配置

进入IxChariot界面，选择“Add Pair”  

![ixchariot1](/img/ixchariot-1.png) 

弹出“Add an Endpoint Pair”对话框，主要设置  

**Pair comment**：pair简要说明  
**Endpoint 1 address**：打流发起终端的源IP地址  
**Endpoint 2 address**：打流发往终端的目的IP地址  
**Network protocol**：网络协议，一般默认TCP  
**Select Script**：一般选择throughput.scr

![ixchariot2](/img/ixchariot-2.png) 

点击OK建立连接

选择创建出的Pair，进行复制、粘贴，创建出一个一样配置的Pair，一般无线打流中创建多个pair可以将性能利用率达到最高。

选择菜单Run -> Set Run Options ... 设置运行参数  
“How to send a test run”中设置固定运行时间，比如设置为1分钟；  
“How to report timings”中可以选择Real-time，实时查看性能数据  
“Polling”中取消勾选Poll endpoints选项

![ixchariot3](/img/ixchariot-3.png) 

点OK保存设置

点击Run按钮，开始运行打流脚本

![ixchariot4](/img/ixchariot-4.png) 

### 结果分析

测试得到的结果主要是通过下图红框中几项进行分析

![ixchariot5](/img/ixchariot-5.png) 

**Throughput**：设备吞吐量

| 选项                      | 说明                                 | |
|---------------------------|--------------------------------------|-|
| Timing Records Completed  | 测试记录完成的次数情况               | |
| 95% Confidence Interval   | 95%可靠区间                          | |
| Average(Mbps)             | 平均吞吐量，多Pair可以看单个及累加值 | |
| Minimum(Mbps)             | 最小吞吐量                           | |
| Maximum(Mbps)             | 最大吞吐量                           | |
| Measured Time(sec)        | 测试运行时间                         | |
| Relative Precision        | 测试精度                             | |

**Transaction Rate**：传输速率，指脚本每秒执行的传输记录数量。

| 选项                      | 说明                                 | |
|---------------------------|--------------------------------------|-|
| Timing Records Completed  | 测试记录完成的次数情况               | |
| 95% Confidence Interval   | 95%可靠区间                          | |
| Transaction Rate Average  | 平均传输速率                         | |
| Transaction Rate Minimum  | 最小传输速率                         | |
| Transaction Rate Maximum  | 最大传输速率                         | |
| Measured Time(sec)        | 测试运行时间                         | |
| Relative Precision        | 测试精度                             | |

**Response Time**：响应时间，指一个测试传输记录完成所需要的时间。

| 选项                      | 说明                                 | |
|---------------------------|--------------------------------------|-|
| Timing Records Completed  | 测试记录完成的次数情况               | |
| 95% Confidence Interval   | 95%可靠区间                          | |
| Response Time Average     | 平均响应时间                         | |
| Response Time Minimum     | 最小响应时间                         | |
| Response Time Maximum     | 最大响应时间                         | |
| Measured Time(sec)        | 测试运行时间                         | |
| Relative Precision        | 测试精度                             | |

**Raw Data Totals**：传输的数据量

| 选项                      | 说明                                 | |
|---------------------------|--------------------------------------|-|
| Timing Records Completed  | 测试记录完成的次数情况               | |
| Transaction Count         | 传输的总量，等于传输次数*每次传输的量| |
| Bytes Sent by E1          | Endpoint 1 发送的字节数              | |
| Bytes Received by E1      | Endpoint 1 接收的字节数              | |
| Measured Time(sec)        | 测试运行时间                         | |
| Relative Precision        | 测试精度                             | |
