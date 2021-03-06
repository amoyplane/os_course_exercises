# lec5: SPOC思考题

##**提前准备**
（请在上课前完成）

- 完成lec５的视频学习和提交对应的在线练习
- git pull ucore_os_lab, v9_cpu, os_course_spoc_exercises in github repos。这样可以在本机上完成课堂练习。
- 理解连续内存动态分配算法的实现（主要自学和网上查找）

NOTICE
- 有"hard"标记的题有一定难度，鼓励实现。
- 有"easy"标记的题很容易实现，鼓励实现。
- 有"midd"标记的题是一般水平，鼓励实现。


## 思考题
---

## “连续内存分配”与视频相关的课堂练习

### 5.1 计算机体系结构和内存层次

1.操作系统中存储管理的目标是什么？

抽象、保护、共享、虚拟化。

### 5.2 地址空间和地址生成
1.描述编译、汇编、链接和加载的过程是什么？

编译：将源代码转换成汇编代码；汇编：将汇编代码转换成机器码；链接：将机器码组合成可执行环境；加载：将程序加载到内存中；

2.动态链接如何使用？尝试在Linux平台上使用LD_DEBUG查看一个C语言Hello world的启动流程。  (optional)




### 5.3 连续内存分配
1.什么是内碎片、外碎片？

内碎片：分配的内存和实际使用的内存多出来的部分；外碎片：分配给不同进程的内存间的无法利用的内存；

2.最先匹配会越用越慢吗？请说明理由（可来源于猜想或具体的实验）？

会，因为会先找低地址的内存，使得后期低地址有大量无法使用的空间，只能扫描到高地址才能找到可用空间。

3.最差匹配的外碎片会比最优适配算法少吗？请说明理由（可来源于猜想或具体的实验）？

会；每次被分割的内存块都是最大的内存块，分割后剩下的也是较大的块，更容易被使用。

4.理解0:最优匹配，1:最差匹配，2:最先匹配，3:buddy systemm算法中分区释放后的合并处理过程？ (optional)

都是看附近有无满足条件（如buddy system中的起始地址条加），并合并相邻快，修改空闲块信息；


### 5.4 碎片整理
1.对换和紧凑都是碎片整理技术，它们的主要区别是什么？为什么在早期的操作系统中采用对换技术？  

对换是内外存之间发生的，紧凑是内存内的。早期系统内存小，增加内存代价高，使用对换技术成本小，简单。

2.一个处于等待状态的进程被对换到外存（对换等待状态）后，等待事件出现了。操作系统需要如何响应？

寻找可对换内存区域，将进程读入内存。

### 5.5 伙伴系统
1.伙伴系统的空闲块如何组织？

大小满足2^k的形式，用二维方式存储，一维是大小，另一维是起始地址。

2.伙伴系统的内存分配流程？伙伴系统的内存回收流程？

寻找满足条件的空闲页块（刚好满足或需要拆分的更大块），分配并修改空闲块表。回收时判断周边是否有可合并块并进行合并。

## 课堂实践

观察最先匹配、最佳匹配和最差匹配这几种动态分区分配算法的工作过程，并选择一个例子进行分析分析整个工作过程中的分配和释放操作对维护数据结构的影响和原因。

  * [算法演示脚本的使用说明](https://github.com/chyyuu/os_tutorial_lab/blob/master/ostep/ostep3-malloc.md)
  * [算法演示脚本](https://github.com/chyyuu/os_tutorial_lab/blob/master/ostep/ostep3-malloc.py)

例如：
```
python ./ostep3-malloc.py -S 100 -b 1000 -H 4 -a 4 -l ADDRSORT -p BEST -n 5 -c
python ./ostep3-malloc.py -S 100 -b 1000 -H 4 -a 4 -l ADDRSORT -p FIRST -n 5 -c
python ./ostep3-malloc.py -S 100 -b 1000 -H 4 -a 4 -l ADDRSORT -p WORST -n 5 -c
```

### 扩展思考题 (optional)

1. 请参考xv6（umalloc.c），ucore lab2代码，选择四种（0:最优匹配，1:最差匹配，2:最先匹配，3:buddy systemm）分配算法中的一种或多种，在Linux应用程序/库层面，用C、C++或python来实现malloc/free，给出你的设计思路，并给出可以在Linux上运行的malloc/free实现和测试用例。


2. 阅读[slab分配算法](http://en.wikipedia.org/wiki/Slab_allocation)，尝试在应用程序中实现slab分配算法，给出设计方案和测试用例。
