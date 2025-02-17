### 你怎么理解操作系统里的内存碎片，有什么解决办法？

内存碎片通常分为内部碎片和外部碎片：

1. 内部碎片是由于采用固定大小的内存分区，当一个进程不能完全使用分给它的固定内存区域时就会产生内部碎片，通常内部碎片难以完全避免；

2.外部碎片是由于某些未分配的连续内存区域太小，以至于不能满足任意进程的内存分配请求，从而不能被进程利用的内存区域。

现在普遍采取的内存分配方式是段页式内存分配。将内存分为不同的段，再将每一段分成固定大小的页。通过页表机制，使段内的页可以不必连续处于同一内存区域。

### 内存使用情况 buff和cache有什么区别？

- 都是为了解决内存和IO设备的读写速度不对等的中间缓存，两个都是内存的一部分
- cached 把读取过的数据保存起来，重新读取的时候命中就不用读磁盘了，如果没有命中就会按频率更新cached
- buffers 把分散的写操作集中起来，缓存要输出到io设备的数据（写一次就存一下硬盘贼耗时间，都是缓冲一会再一起写硬盘）
- 拓展一个shared，是共享内存，可以ipcs来查看

### 怎么定位进程cpu占用大是哪一个函数导致的？

```bash

perf top -g -p 246
```

![](https://coding3min.oss-accelerate.aliyuncs.com/2021/03/08/m7R1Bu1111.jpg)

这里推荐

![优惠口令： Happy2021](https://tva1.sinaimg.cn/large/008eGmZEgy1goeyrj9dgrj30u01hdnpd.jpg)

### 什么是程序的堆空间和栈空间？

回答者：海翔

栈是用来保证程序顺序执行的，后入栈的函数先出，完整记录一个函数（方法）调用从开始到结束所做的一切操作。

堆是用来保存变量和对象的，存储临时数据和部分运行时数据。包括函数调用期间产生的临时变量，程序加载启动时载入的全局变量等等。堆内存的分配，应该是在临时变量第一次被使用时分配，全局静态变量是在类加载时分配。不同的变量有不同的生命周期。而垃圾回收，主要也是针对堆内存空间的调整和释放

栈和堆都有其空间大小。

当递归层级过深时会出现栈溢出异常，就是因为要保存的方法栈超过了栈可保存的最大数量。而堆内存不足时常常会遇到OOM异常，堆内存不足以存放新生成的对象或变量了。

### 虚拟地址和物理地址有什么区别，程序编译运行后首先申请到的是什么地址？

因为位数代表最大寻址能力，32位最大寻址能力是4G所以超过4g的内存条会造成浪费

我们知道线程是cpu调度的最小单元，进程是资源分配的最小单元，每个进程之间的资源是独立的，互不影响的，这是怎么实现的呢？

每个进程启动的时候会有独立内存空间，称为虚拟内存，启动时为给每个进程维护一个独立的页表做虚拟内存和物理内存的映射

所以不同进程之间的虚拟内存地址可能是相同的，这没关系，最终映射到的是物理内存不是一个

假如不同进程都访问某个系统的库，就不需要加载两遍到物理内存上，只要映射到同一地址范围就可以

用到了再分配这种机制叫内存的惰性加载。

虚拟内存寻址是cpu到一个叫mmu的硬件，物理内存寻址是mmu到内存条，mmu相当于是个外包

所以虚拟内存虽然大，不一定全部都存在映射，之前说的堆栈空间也是在虚拟内存中的

### Linux 打开文件句柄写入一个文件时，mv这个文件会发生什么

mv操作，目标文件的inode将等于源文件的inode；因此正在写入的文件被mv，数据仍然被写入到mv后的文件里，除非重新open

正在写入的文件被rm后，数据会被写入到系统缓存中，一直会耗尽所有可用的内存

### 日志归档和清空有哪些方式

归档：logrotate 支持归档和删除长期日志

代码级别可以使用滚动日志组件

如果是手动删除可以使用cat /dev/null > xxx.log

### 查询linux文件被哪些pid 读写的命令是什么

lsof abc.txt 显示开启文件abc.txt的进程

lsof -i :22 知道22端口现在运行什么程序

lsof -c nsd 显示nsd进程现在打开的文件

lsof -g gid 显示归属gid的进程情况

lsof +d /usr/local/ 显示目录下被进程开启的文件

lsof +D /usr/local/ 同上，但是会搜索目录下的目录，时间较长

lsof -d 4 显示使用fd为4的进程

lsof -i [i] 用以显示符合条件的进程情况

### 最后

如果文中有误，欢迎提pr或者issue，**一旦合并或采纳作为贡献奖励可以联系我直接无门槛**加入[技术交流群](https://mp.weixin.qq.com/s/ErQFjJbIsMVGjIRWbQCD1Q)

我是小熊，关注我，知道更多不知道的技术

![](https://coding3min.oss-accelerate.aliyuncs.com/2021/03/11/gQDiQ51116.jpg)