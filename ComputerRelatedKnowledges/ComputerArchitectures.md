# 操作系统：基于计算机硬件，操作应用软件的==软件==。

## 进程（Process）管理

计算机五大组成部件：输入设备，输出设备，存储器，运算器，控制器
![](https://i.imgur.com/ROilgxg.png)
> BIOS: Basic Input/Output System

内核/用户态：实现内核程序与用户程序的分离！！！（如果不分离，则用户程序可以直接跳入内核程序获取信息，会有极大的安全隐患）
* 内核态：可以访问任何数据（cs代码：0）
* 用户态：无法访问内核数据（cs代码：3）
> 操作系统提供的主动进入内核态的方法：中断指令int
> 系统调用的核心；
> 1. 用户程序中包含一段带有Int 0x80的代码；
> 2. 操作系统写中断处理，获取想调程序的编号；
> 3. 操作系统根据编号执行相应代码。

### 多进程图像：从启动开始到关机结束

- main中的fork()创建了第一个进程； `if (!fork()) {init(); }`
- init()执行，启动shell；
- shell再启动其他进程！

> CPU的并发执行中多进程如何记录状态且组织？ ==PCB！==（OS核心!）

### 线程（Thread）

进程的切换需要伴随PCB记忆进程资源的切换，有较大开销；线程实现了资源与指令的分类离，保留并发优点，避免了进程切换的代价！

> 用户级线程：在用户态进行切换的线程（利用yield()），核心并不知晓（用户设定，对内核透明）
> 特点：**两个栈（在用户态）**
> ==问题==：**用户级线程涉及io时仍需通过核心访问io设备，此时由于io耗时而用户级线程对内核透明，则内核会认为该进程进入阻塞态而切换到别的进程！这样就没法完成用户态线程的切换了！**

> 核心级线程：是系统调用，会进入内核进行切换！由于对内核非透明，不容易产生用户级线程的问题，并发性好一些。
> 特点：==两套栈==（用户态和内核态！）
> ==问题==：**需要进入内核切换，需要创建两套栈，内核改动大，开销大，灵活性小。**

![](https://i.imgur.com/vByAyUG.png)

---

### CPU调度

讲几种常见的就好。

1. FCFS(First Come First Service) 先来先服务：最简单但是平均周转时间长；
2. SJF 短作业优先：平均周转时间最小！
    - 问题：响应时间很长！（因为用户操作后需要等前面短作业都完成才开始响应啊！）
    - 解决：按时间片轮转调度（也有问题！时间片小则响应快但吞吐量小！）
3. 优先级调度（同时考虑响应时间和周转时间）
    - 前台任务采用轮转调度，后台任务使用短作业优先调度。
    - 问题：优先级低的后台任务可能会一直轮不到它运行！！！

---

### 进程同步与信号量

进程间同步手段：管道，双向队列，共享内存，信号，信号量，本地套接字。

其他几个的缺点都很好知道，信号的缺点？ - 仅仅有信号无法解决多生产者一消费者产生的生产者睡眠问题！所以我们会引入信号量。
> 信号counter: 当counter = buffersize - 1， 则唤醒一个进程！问题就在于多个生产者会出现无法唤醒问题（因为一次只能唤醒一个）
> 信号量sem: 实际上是一个整形计数器，有进程睡眠等待时则-1，唤醒进程则+1！ 这里的sem就记录了睡眠的进程数！ 
:::success
![](https://i.imgur.com/sbvCaC8.png)
![](https://i.imgur.com/vHHn73y.png)
:::

**临界区**：一次只允许一个进程进入该进程的那一段代码（特别重要，临界区保护修改信号量！）
> **修改信号量的代码必须是临界区！当一个进程执行修改信号量时，另一个进程不能够进入该临界区修改！**
:::info
#### 临界区代码保护原则
基本原则：互斥进入（防止同时进入修改信号量） - 重点：==互斥==！ 它保证了是临界区。

其他有则最好的原则：有空让进，有限等待。

Peterson算法：同时记录进程是否想要进入临界区（谁能进）与轮到谁进入临界区（放谁进）!
- 该算法满足有空让进与有限等待！

多进程？ 面包店算法：仍然利用Peterson思想（标记+轮转），但使用取号机制来记录，每个进程获得一个序号，序号小的先进入；进程离开则将号置为0！
:::

---

### 死锁处理

死锁：互相等待对方释放持有资源而造成的谁都无法执行的状况！

死锁的四个必要条件！
1. 互斥使用
2. 不可抢占
3. 保持请求
4. 循环等待

:::success
**处理死锁**

1. 死锁预防：破坏死锁的四大必要条件！
2. 死锁避免：检测进程的资源请求，若会造成死锁则拒绝！(银行家算法)
3. 死锁检测+恢复：检测到死锁时则让其滚回（rollback）
4. 死锁忽略：死锁是啥？不知道呢。

![](https://i.imgur.com/bdjO1b0.png)

:::

---

## 内存管理

### 内存使用与分段

内存使用：将程序放入内存中，然后CPU开始==取指执行==

重定位：修改程序中的地址（由物理地址到逻辑地址），让CPU能够顺利读到指令。
> 什么时候完成重定位？
> 1. 编译时：可以但很难，因为你很难在一开始就知道哪里是空闲的。优点：效率高！
> 2. 载入时：根据当前内存空闲，让所有指令逻辑地址加上空闲的那段地址的值（也就是，从空闲那里开始重定位！），这样更加灵活！缺点：一旦载入之后，内存就不能动了！（有时候程序载入后还需要移动！这时候就不好了！）
> 3. 运行时重定位！每条指令运行时候才进行重定位！==这才是我们真正使用的！==

- 实际使用时，都是将程序分段（代码段，数据段，etc.）载入内存，以提升实际使用效果。

### 内存分区与分页

:::success
内存分区：内存如何分割？？？

1. 等分：初始化时侯等分为k份（不合适啊很明显！不同进程需求不一定！）
2. 可变分区：这个是真正用的，如何管理？

可变分区：利用两个表来存储正在使用的内存与没使用的内存的初始地址与长度！
- 当有进程释放时，将其加入没使用的内存的表中。
- 当有新进程申请内存时，有多种选择方法给它分配内存：
- 1. 首先适配：未使用表中第一个满足足够大小的内存分配给它；
- 2. 最佳适配：未使用表中的大小足够中的最小的一个分配给它！
:::

:::info
问题：可变分区总会造成部分内存碎片未被使用！（有150k但是程序120k，剩下30k碎片很可能空闲下来了）

--> ==内存分页==：它将虚拟地址和物理地址按固定大小(4K)分割成页(page)和页帧(page frame),并保证页与页帧的大小相同。 这种机制,从数据结构上,保证了访问内存的高效,并使OS能支持非连续性的内存分配。

页表：记录页状况的表。为了提高空间利用率，页应该小。页越小，页表越大；反之页越大，页表越小。
> 问题：页表占用内存大，且查起来速度慢!

> 解决：多级页表！页目录表(章)+页表(节)
> 特点：时间换空间！（毕竟要多级访问！）
> 如何解决？：==快表TLB==：存储最近使用的页号,若命中则可以直接快速查询到！

![](https://i.imgur.com/JXgSwvg.png)

> TLB越大，命中率越高，但是空间开销也大！所以我们通常限制TLB条目数为`[64, 1024]`
> 为什么？==因为程序的地址访问存在局部性！==
:::

---

### 真正的内存管理-段页结合

段页结合：程序员喜欢段，内存喜欢页，所以结合！
> ==如何结合？== 段面向用户，页面向硬件
> 虚拟内存：（用户视角）将代码分段；
> 物理内存：（硬件视角）将虚拟内存与真实内存地址映射形成页。

- 问：虚拟内存究竟是什么？为什么会被称为虚拟内存？
- 答：**==虚拟内存是一种计算机系统内存管理技术==。它使得应用程序认为它拥有连续可用的内存，即一个连续完整的地址空间。而实际上，它通常是被分隔成多个物理内存碎片，还有部分暂时存储在外部磁盘存储器上，在需要时进行数据交换。**

![](https://i.imgur.com/TGdphbR.png)

---

### 内存的换入换出:将虚拟内存映射至物理内存（请求的资源）

虚拟内存机制，让用户的视角中形成了每个进程都可以操作完整 4GB 内存的假象。比如从一个用户代码的角度，它可以随意使用计算机的全部内存地址，就像单独拥有内存（4G）一样。而后续虚拟内存向物理内存的映射，是对用户不可见的。

这样就很容易产生一个问题：

- 每个进程的虚拟内存加起来是很可能大于物理内存的，比如下图中，虚拟内存高达4G，物理内存仅有1G，怎么办？

答：用换入、换出实现用户眼中的 “超大内存”：

- 比如当虚拟内存请求使用0G~1G地址时，将相应的代码、数据从磁盘换入物理内存，将这部分地址由虚拟内存映射到物理内存上，进行读写操作；
  而当虚拟内存请求使用3G~4G地址时，换出之前内存上的数据，换入这部分的代码、数据，再将这部分由虚拟内存映射到物理内存上...
  通过动态出入物理内存的代码数据，来实现更大空间的虚拟内存映射。
  
- 实际使用中我们使用的都是请求调页，因为页的粒度更细，更能提高内存效率

> 内存换入：通过换入可以让用户使用一个规整的虚拟内存！
> 
> - 实现调页换入，主要就是三步：
> 
> 1. 申请空闲物理页；
> 1. 从磁盘中读入；
> 1. 建立映射，修改页表。

---

> 内存换出：将换入的资源移除内存 (get_free_page())

换出算法：
1. FIFO：先进先出，但是刚换入的页需要被马上换出的情况无法顾及到
2. MIN
3. LRU(最优

:::success
==FIFO==

![](https://i.imgur.com/Q3Q6f7P.png)
> 问题：
> 1. D换了A但是后面A马上又要换进来很浪费，换C更好；
> 2. 切换中有7次缺页!
:::

:::info
==MIN==

![](https://i.imgur.com/knzZdAq.png)
> 问题：
> 1. 还是有5次缺页；
> 2. 实际应用中无法预测到将来谁最后使用！
:::

:::warning
==LRU== : 选最近最长一段时间内没有使用的页淘汰（经验型算法？但是总的来说是目前最优）

![](https://i.imgur.com/PDFECmX.png)
> 缺页次数少，无需预测未来！

- 实现1：时间戳！！

![](https://i.imgur.com/oCVdbAa.png)
> 理论实现很简单，维护一个上次出现时间戳就可以。但是实际上，每一次修改时间戳都需要查表找到相应位置再增大，且容易溢出！
> ==实现代价太大！==

- 实现2：页码栈(底层就是队列)！
![](https://i.imgur.com/zvpz7un.png)
> 理论很好，准确实现这东西是真的难！所以我们通常选择==近似实现==。
:::

:::success
LRU近似实现：CLOCK算法

![](https://i.imgur.com/Zn3qHfZ.png)
> 每页标记一个引用位，访问到时候就置1；选择淘汰页面时则依序扫描位，是1清0后在扫描，否则淘汰！

- 和LRU有什么不一样的？
1. LRU是淘汰最长时间是用最少，SCR是淘汰没有使用！
2. CLOCK给了两次机会！（1-0-淘汰）
3. CLOCK底层循环队列

缺点：若内存小则缺页会很少，会退化成FIFO!（因为记录了太长的历史信息了！）
- 解决：
- 1. 更换更大内存（硬件进步永远大于软件进步！）
- 2. 定时清除引用位！ - 追加一个扫描指针在后面追，追到的就清除掉！（这样就是一个CLOCK了！）

![](https://i.imgur.com/bjzs91M.png)

:::

:::danger
问题：给进程分配多少页框合适？

- 分配过多，调页为基础的内存高效利用就没有意义了（上面谈那么多的都是页没这么多的情况，但是页特别多的话进程根本走不满页，别谈调页了！）；
- 分配过少，则进程多的时候每个进程缺页率增大，很长时间都在等待调页，CPU利用率低(颠簸)！

![](https://i.imgur.com/17IvhhK.png)

:::

> 换入换出的整体机制其实很好理解。

> 程序运行，某逻辑地址要求访问内存，MMU 硬件管理单元发现 这个逻辑地址在内存中找不到对应位置，启用缺页中断，从磁盘中读页进来。如果内存没有空闲位置可供磁盘读入，则需要 Clock算法 选择页进行换出；换出后，有空闲位置，则可继续读入需要的页。
> 换入换出的机制实现后，虚拟内存就可以投入使用，进而实现了 段页式内存管理。

![](https://i.imgur.com/Ic1QXgX.png)
> CPU与内存的管理讲完，后面就是一些不这么重要的了！
---

## IO设备驱动


![](https://i.imgur.com/vTFhMan.png)
![](https://i.imgur.com/UV5OA9w.png)

---

## 文件（磁盘）管理

### 生磁盘：根据盘块号来使用磁盘！

> 读取：
> 1. 使用控制器的out()指令写入柱面，磁头，扇区给磁盘控制器，就可以读取磁盘指定位置。
> 2. 输入盘块号，控制器去计算出对应的柱面，磁头与扇区。（显然这个更好！）
> - 磁盘访问时间 = 写入控制器时间+寻道时间（大头）+旋转时间（次大头）+传输时间

磁盘调度：
:::success

FCFS：先来先服务。 磁头移动不确定性大，效率很差。
![](https://i.imgur.com/zqzSGjW.png)

---

> 改进：SSTF（Shortest-seek-time First）短寻道优先
- 问题：读写频繁时磁头总是在中间段移动，位于边缘位置的请求容易饥饿，不公平！

![](https://i.imgur.com/Ri2b4Gi.png)

> 继续改进:SSTF+中途不回折(SCAN)！

![](https://i.imgur.com/0DXQVso.png)
- 问题：看上面的图仍然知道，SCAN并非完全公平，它还是从中间出发，经过中间到边上！

> **继续改进：C-SCAN(SCAN+直接移到另一端！) -- ==电梯算法==！（真实算法）**

![](https://i.imgur.com/Fw7AUVh.png)

:::

---

### 从生磁盘到文件！

**熟磁盘：根据文件来使用磁盘！**

> 问题：
> 1. 如何从生磁盘抽象到文件？
> 2. 如何从文件得到盘块号？

- 文件：
    - 用户视角：字符流
    - 磁盘视角：盘块的集合
    - 关键：==寻找盘块集合到字符流的抽象映射！==
    - 总的过程见下。
![](https://i.imgur.com/4QZuhaT.png)
> 实际上，文件也能通过链式与索引结构实现，这里不详叙。

--- 

### 目录与文件系统

1）磁盘存储的是一堆文件；一堆文件形成树状结构；

> 具体实现是：把整个磁盘的所有盘块进行抽象，组织；
> 所谓抽象就是用数据结构进行组织；
> 如一个文件对应一个inode，一个inode组织文件所有盘块，形成字符序列；

整个磁盘的文件以上述方式进行组织和抽象，形成给用户一个抽象的树状结构；

2）整个磁盘抽象为文件系统

> 文件系统就是映射， 从字符位置到盘块的映射；
> 底层结构是对上层的实现， 上层是对底层的抽象；  


后续： [目录与文件系统笔记](https://blog.csdn.net/PacosonSWJTU/article/details/125953608)

