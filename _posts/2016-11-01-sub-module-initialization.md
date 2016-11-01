---
layout: post
title: 子模块初始化函数构建
excerpt: "通过section编译属性实现子模块初始化函数构建"
categories: [linux-c]
comments: true
---

## 问题背景

对于一个较为复杂的功能模块进程，往往包含若干个子模块功能，每个子模块间实现相对独立的功能避免耦合。  
但是通常这些子模块都需要一些初始化的逻辑，通常我们会在子模块中定义初始化函数，在主程序框架中调用这些函数，  
那么可能就会引入一个问题，当某些功能不希望编译入进程时，在不编译子模块源文件的同时还要避免主程序调用该子模块定义的初始化函数，  
一般情况下回采用宏包含等方式，但是使用section编译属性能使得耦合性得到更好的解决。

## 例子

### 公共头文件

公共头文件中定义相关结构体及section宏定义
 
这里我们section名称取名为**.hello**  
section起始地址取名为**_hello_start**  
section结束地址取名为**_hello_end**

	typedef struct hello_s {
		char *name;
		void (*func)(void);
	} hello_t;
	
	extern hello_t _hello_start[];
	extern hello_t _hello_end[];
	
	#define _init __attribute__((unused, section(".hello")))
	#define module_init(name, func) \
		static const hello_t hello_##func _init = {name, func}

### 子模块

子模块注册初始化函数

	static void module_1_init(void)
	{
		printf("module 1 init\n");
	}
	module_init("module_1", module_1_init);

	static void module_2_init(void)
	{
		printf("module 2 init\n");
	}
	module_init("module_2", module_2_init);

### 主框架初始化

主框架遍历初始化函数进行调用

	int main(void)
	{
		hello_t *call_ptr = _hello_start;
		do {
				printf("call module %s %p\n", call_ptr->name, call_ptr->func);
				(*call_ptr->func)();
				++call_ptr;
		} while (call_ptr < _hello_end);
		return 0;
	}
	
### 运行

上述代码已经基本完成的编码，但是明显有个疑问hello_start和hello_end只有声明没有定义，如何能跑起来。  
果然编译时提示找不到

	root@oa:/home/share# gcc a.c
	/tmp/ccrLcAHk.o: In function `do_initcalls':
	a.c:(.text+0x31): undefined reference to `_hello_start'
	a.c:(.text+0x63): undefined reference to `_hello_end'
	collect2: error: ld returned 1 exit status

要让这代码运行起来还需要lds文件

可以使用命令 **gcc -Wl,--verbose > s.lds** 或者使用命令**ld --verbose > s.lds**

生成lds文件，用文本工具打开s.lds文件，  
删除两个"=================="串及其前后的内容，保留中间的内容。

并找到**__bss_start = .;**的位置，并在其之前插入

	.hello   : 
	{
	_hello_start = .;
	*(.hello)
	_hello_end = .;
	}
	code_segment    : { *(code_segment) }  

其中，*.hello*就是代码中定义的section的名字，**_hello_start**和**_hello_end**就是代码中定义的地址

编译时使用**-T**参数加上该lds文件，就可以编译过了

	root@oa:/home/share# gcc a.c -Ts.lds
	root@oa:/home/share# ./a.out 
	call module module_1 0x804844d
	module 1 init
	call module module_2 0x8048461
	module 2 init

Author: chenxiawei@gmail.com  
	
## 参考材料

(1) [https://my.oschina.net/u/180497/blog/177206](https://my.oschina.net/u/180497/blog/177206) 

