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
	# name            <active_objs> <num_objs> <objsize> <objperslab> <pagesperslab> : tunables <limit> <batchcount> <sharedfactor> : slabdata <active_slabs> <num_slabs> <sharedavail>
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

## References
(1) [http://www.secretmango.com/jimb/Whitepapers/slabs/slab.html](http://www.secretmango.com/jimb/Whitepapers/slabs/slab.html)   
(2) [http://blog.csdn.net/hs794502825/article/details/7981524](http://blog.csdn.net/hs794502825/article/details/7981524)  
(3) [http://static.usenix.org/publications/library/proceedings/bos94/full_papers/bonwick.a](http://static.usenix.org/publications/library/proceedings/bos94/full_papers/bonwick.a)
