---
layout: post
title: 调试不带符号表的coredump文件
excerpt: ""
categories: [linux]
comments: true
---

## 背景介绍
在小内存的嵌入式设备中，为了压缩进程的大小，通常是去掉符号表后在设备上运行，此时进程崩溃后生成的core文件和对应的二进制文件是不带符号表的，使用gdb进行查看时无法获取到准确信息。

但是我们可以单独保存被删掉的符号表，在gdb的时候进行导入来进行调试。

## 例子
以一个二进制进程链接一个动态库为例<br>
动态链接库为命名为b.c
{% highlight c %}
#include <stdio.h>

int b(int (*a)(void))
{
	return a() + 100;
}
{% endhighlight %}
主程序命名为a.c
{% highlight c %}
#include <stdio.h>

extern int b(int (*a)(void));

int a() 
{
	int *a = NULL;
	*a=0;
	return 1;
}

int c()
{
	return b(a) + 1000;
}

int main()
{
	c();
	return 0;
}
{% endhighlight %}

1. 生成动态链接库及主程序
    gcc -g  b.c -fPIC -shared -o libb.so

    gcc -g a.c -o a.out -L. -lb
2. 提取主程序及动态链接库的符号表
    strip --only-keep-debug -o a.sym a.out
    strip --only-keep-debug -o libb.sym libb.so
3. 删除主程序及动态链接库的符号表
    strip a.out
    strip libb.so
4. 运行程序产生core文件
    root@oa:/home/share/gdb# ./a.out
    Segmentation fault (core dumped)