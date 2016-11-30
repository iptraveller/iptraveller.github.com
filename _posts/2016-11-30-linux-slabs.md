---
layout: post
title: slabs
excerpt: ""
categories: [linux]
comments: true
---

## 数据结构关系

![slab](/img/slab.png) 

### 数据统计

	admin@Ruijie:/# cat /proc/slabinfo 
	slabinfo - version: 2.1
	# name            <active_objs> <num_objs> <objsize> <objperslab> <pagesperslab> : tunables <limit> <batchcount> 
	<sharedfactor> : slabdata <active_slabs> <num_slabs> <sharedavail>
	task_struct           91     91   1064    7    2 : tunables   24   12    0 : slabdata     13     13      0
	pid                  118    118     64   59    1 : tunables  120   60    0 : slabdata      2      2      0
	size-32768             3      3  32768    1    8 : tunables    8    4    0 : slabdata      3      3      0
	size-16384             4      4  16384    1    4 : tunables    8    4    0 : slabdata      4      4      0
	size-8192              7      7   8192    1    2 : tunables    8    4    0 : slabdata      7      7      0
	size-4096           2041   2041   4096    1    1 : tunables   24   12    0 : slabdata   2041   2041      0
	size-2048             27     28   2048    2    1 : tunables   24   12    0 : slabdata     14     14      0
	size-1024            330    332   1024    4    1 : tunables   54   27    0 : slabdata     83     83      0
	size-512             808    808    512    8    1 : tunables   54   27    0 : slabdata    101    101      0
	size-256             195    195    256   15    1 : tunables  120   60    0 : slabdata     13     13      0
	size-192             368    375    256   15    1 : tunables  120   60    0 : slabdata     25     25      0
	size-128             240    240    128   30    1 : tunables  120   60    0 : slabdata      8      8      0
	size-96              870    870    128   30    1 : tunables  120   60    0 : slabdata     29     29      0
	size-64             2070   2190    128   30    1 : tunables  120   60    0 : slabdata     73     73      0
	size-32             7800   8070    128   30    1 : tunables  120   60    0 : slabdata    269    269      0
	
* active_objs  
已经被分配的对象的个数；
* num_objs  
总共创建的对象的个数，等于已分配的对象个数加上还空闲未分配的个数；
* objsize  
对齐后的对象大小；
* objperslab  
每个slab中可以放的对象的个数，num_objs / objperslab 就等于一共申请的slab的个数；
* pagesperslab  
每个slab占用多少页；
* limit  
在array_cache中缓存的对象的个数最大值
* batchcount  
每次从slabs中读到rray_cache缓存中的对象个数。
* active_slabs  
在full和partial链中的slabs个数；
* num_slabs  
在active_slabs中的slabs个数加上在free链中的slabs个数；

打开了**CONFIG_DEBUG_SLAB**宏之后/proc/slabinfo文件中多显示了slab的详细统计信息

	admin@Ruijie:/# cat /proc/slabinfo 
	slabinfo - version: 2.1 (statistics)
	# name            <active_objs> <num_objs> <objsize> <objperslab> <pagesperslab> : tunables <limit> <batchcount> <sharedfactor> : slabdata <active_slabs> <num_slabs> <sharedavail> : 
	globalstat <listallocs> <maxobjs> <grown> <reaped> <error> <maxfreeable> <nodeallocs> <remotefrees> <alienoverflow> : cpustat <allochit> <allocmiss> <freehit> <freemiss>
	task_struct           88     98   1088    7    2 : tunables   24   12    0 : slabdata     14     14      0 : globalstat     133     98    17    3    0    0    0    0    0 : cpustat   7597     19   7534      1
	pid                  101    106     72   53    1 : tunables   32   16    0 : slabdata      2      2      0 : globalstat     167    106     3    1    0    0    0    0    0 : cpustat   7630     12   7558      1
	size-32768             3      3  32768    1    8 : tunables    8    4    0 : slabdata      3      3      0 : globalstat       6      4     6    3    0    0    0    0    0 : cpustat      0      6      3      0
	size-16384             4      4  16384    1    4 : tunables    8    4    0 : slabdata      4      4      0 : globalstat       9      5     9    5    0    0    0    0    0 : cpustat     16      9     21      0
	size-8192              7      7   8192    1    2 : tunables    8    4    0 : slabdata      7      7      0 : globalstat      15      8     9    2    0    0    0    0    0 : cpustat     13     12     18      0
	size-4096           2097   2097   4096    1    1 : tunables   24   12    0 : slabdata   2097   2097      0 : globalstat    2126   2097  2097    0    0   17    0    0    0 : cpustat  10779   2100  10782      2
	size-2048             32     32   2048    2    1 : tunables   24   12    0 : slabdata     16     16      0 : globalstat      86     36    33   17    0    0    0    0    0 : cpustat    416     51    435      0
	size-1024            330    332   1024    4    1 : tunables   32   16    0 : slabdata     83     83      0 : globalstat     377    336    84    1    0    0    0    0    0 : cpustat   2553    103   2326      0
	size-512             616    616    512    8    1 : tunables   32   16    0 : slabdata     77     77      0 : globalstat     984    616   110    5    0    4    0    0    0 : cpustat   9819    122   9315     20
	size-256             150    150    256   15    1 : tunables   32   16    0 : slabdata     10     10      0 : globalstat     167    150    11    1    0    0    0    0    0 : cpustat  20906     12  20784      0
	size-192             360    360    256   15    1 : tunables   32   16    0 : slabdata     24     24      0 : globalstat     516    360    26    2    0    0    0    0    0 : cpustat    398     49     87      0
	size-128             297    300    128   30    1 : tunables   32   16    0 : slabdata     10     10      0 : globalstat     318    300    10    0    0    0    0    0    0 : cpustat   7296     25   7027      0
	size-96              530    720    128   30    1 : tunables   32   16    0 : slabdata     24     24      0 : globalstat    1081    856    29    0    0    0    0    0    0 : cpustat   1775     72   1305     17
	size-64             1834   1860    128   30    1 : tunables   32   16    0 : slabdata     62     62      0 : globalstat    2602   1855    62    0    0    0    0    0    0 : cpustat  14481    194  12806     44
	size-32             8910   8910    128   30    1 : tunables   32   16    0 : slabdata    297    297      0 : globalstat   10232   8926   298    0    0    1    0    0    0 : cpustat 144583    696 136290     96
	kmem_cache           114    120    160   24    1 : tunables   32   16    0 : slabdata      5      5      0 : globalstat     142    120     5    0    0    0    0    0    0 : cpustat     87     28      1      0

* listallocs  
对象从slabs中取出填到array_cache中的历史累加次数
* maxobjs  
对象从slabs中取出到array_cache中的峰值个数，也就是被使用加缓存的对象的历史最大值
* grown  
创建slabs的历史累加次数
* allochit  
申请的对象直接就从array_cache中分配的历史累加次数
* allocmiss  
申请的对象无法直接从array_cache中分配的历史累加次数，加上allochit就为申请对象的总次数
* freehit  
释放对象时直接放到array_cache中不触发array_cache回收的历史累加次数
* freemiss  
释放对象时触发array_cache回收的历史累加次数，加上freehit就为释放对象的总次数

## References
(1) [http://www.secretmango.com/jimb/Whitepapers/slabs/slab.html](http://www.secretmango.com/jimb/Whitepapers/slabs/slab.html)   
(2) [http://blog.csdn.net/hs794502825/article/details/7981524](http://blog.csdn.net/hs794502825/article/details/7981524)  
(3) [http://static.usenix.org/publications/library/proceedings/bos94/full_papers/bonwick.a](http://static.usenix.org/publications/library/proceedings/bos94/full_papers/bonwick.a)
