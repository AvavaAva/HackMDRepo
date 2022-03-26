###### tags: `learn`

# Linux 通用基础

别人的笔记文件：‪E:\Processing\Linux\linux教程_20141002.pdf

- [x] 3/2：完成第一次复习！接下来可以看桌面的书了！

## 1. BackGround

![](https://i.imgur.com/qr69r6Z.png)

安装相关的包都在我的百度云里面有了！VMware + ubuntu

**网络设置**：

![](https://i.imgur.com/K6DtWuY.png)

**虚拟机快照（回滚）**

![](https://i.imgur.com/1ioB5OB.png)

---

## 2. Linux常用命令

实际上有超过200个命令，但实际上我们常用的就那么几个，所以我们只需要记住那几个，其余命令在需要使用时候查阅便可。

![](https://i.imgur.com/dduL3cP.png)
![](https://i.imgur.com/cn4QtKJ.png)

![](https://i.imgur.com/0LUt6fT.png)
![](https://i.imgur.com/FDsSmtM.png)

### 运行级别

![](https://i.imgur.com/r0VH37v.png)

> 就是使用 init x 来改变我们的可视gui。
> 查看运行级别： runlevel


---

### 文件目录指令

![](https://i.imgur.com/yez0mQX.png)
![](https://i.imgur.com/8saeRjz.png)
![](https://i.imgur.com/6tYIE1l.png)
![](https://i.imgur.com/9Qxh1z0.png)
![](https://i.imgur.com/IFd1jAR.png)
![](https://i.imgur.com/s1e30wa.png)
![](https://i.imgur.com/SYnl4Si.png)
![](https://i.imgur.com/7i0ULY6.png)
![](https://i.imgur.com/QMwLjVP.png)
![](https://i.imgur.com/ouV01sB.png)
![](https://i.imgur.com/jctKcoR.png)
> 这里的echo就相当于是编程语言中的return或者print！

![](https://i.imgur.com/NiXpc41.png)
![](https://i.imgur.com/CnKQzCG.png)
> 一个箭头是覆盖（也就是'w'）， 两个箭头是追加(也就是'a')

![](https://i.imgur.com/7DRZqWl.png)
> 相当于创建快捷方式！

![](https://i.imgur.com/eNrztgf.png)

---


### 搜索查找类指令

![](https://i.imgur.com/hg6JYli.png)
![](https://i.imgur.com/DU6gBIO.png)
![](https://i.imgur.com/HsIMrV3.png)
![](https://i.imgur.com/hrUk2mS.png)
> 注： grep也可以利用参数 -v 进行反向查找（就是查找不带后面跟着的字符的）
> 例子如下：

![](https://i.imgur.com/pm4ekbj.png)
**显然我第一次搜索的结果是grep进程本身；但是很明显我们不是想搜索本身进程，所以就 -v grep 把带grep的结果全部排除！就可以实现这个效果。**

---
### 时间日期类指令


![](https://i.imgur.com/zdGR7Bn.png)
![](https://i.imgur.com/A9fIeev.png)



---
### 压缩/解压类指令
![](https://i.imgur.com/zm8vs1D.png)
![](https://i.imgur.com/FM5n8V2.png)
![](https://i.imgur.com/QMciCN0.png)
![](https://i.imgur.com/ysm6yxp.png)
![](https://i.imgur.com/c56oq1I.png)




---

## 3. Linux目录

![](https://i.imgur.com/yxYC3C9.png)
![](https://i.imgur.com/6MUpzeU.png)
![](https://i.imgur.com/8OGN4QW.png)
![](https://i.imgur.com/BZDyCdQ.png)



---

## 4. 远程登录

![](https://i.imgur.com/bQzUXA2.png)

Xshell可以远程登录，而Xftp可以实现文件传输。

链接方式简单，在linux虚拟机上输入 ifconfig查询ip后利用ip连接即可。

---

## 5. Vim编辑器

![](https://i.imgur.com/vgrWXuQ.png)
![](https://i.imgur.com/ie2WvFc.png)
![](https://i.imgur.com/rzQSdh4.png)
![](https://i.imgur.com/vWTRx44.png)


---

## 6. 开关机、用户指令等

![](https://i.imgur.com/HakLgr2.png)
![](https://i.imgur.com/xKxcljS.png)
![](https://i.imgur.com/4srwi6O.png)
![](https://i.imgur.com/cushtVc.png)
![](https://i.imgur.com/Hrrhobk.png)
![](https://i.imgur.com/9cpfk4o.png)
![](https://i.imgur.com/kZ4gen9.png)
![](https://i.imgur.com/9yZ5FOl.png)

![](https://i.imgur.com/J3S5r2C.png)
![](https://i.imgur.com/bjFDgwT.png)
![](https://i.imgur.com/pjDCdPU.png)
![](https://i.imgur.com/3WWaOha.png)


---

### 权限

![](https://i.imgur.com/Cgtix1n.png)

> 第0位的 '-' 代表的是普通文件。

![](https://i.imgur.com/YX4WCcI.png)
![](https://i.imgur.com/G6wyZyI.png)

![](https://i.imgur.com/Sk8U1nB.png)
**当然我们也可以修改权限！**

![](https://i.imgur.com/0Tk7lLe.png)

![](https://i.imgur.com/yFbnMXV.png)



案例：
![](https://i.imgur.com/epVwbse.png)
![](https://i.imgur.com/21Lgox8.png)
![](https://i.imgur.com/SW38kb6.png)


---

### 修改文件所有者与所在组

![](https://i.imgur.com/2EI2W8k.png)
![](https://i.imgur.com/eP8S0Kh.png)

实践：跟着玩玩linux

![](https://i.imgur.com/dwRoTW5.png)

---

## 7. 定时任务调度
### 1. crontab 周期性反复执行任务


![](https://i.imgur.com/3QXXXYm.png)
![](https://i.imgur.com/zk1C2w7.png)
![](https://i.imgur.com/Hc6WMzb.png)
![](https://i.imgur.com/gykfyqj.png)
![](https://i.imgur.com/4m1jFqT.png)
![](https://i.imgur.com/P9UtkY6.png)
![](https://i.imgur.com/861DVyj.png)
![Uploading file..._b5bpc44uo]()



---


### 2. at 一次性执行任务

![](https://i.imgur.com/njUc5SM.png)
![](https://i.imgur.com/kepHyY2.png)
![](https://i.imgur.com/hpCqfiv.png)
![](https://i.imgur.com/aajhyOb.png)
![](https://i.imgur.com/0znSvIg.png)


---

## 8. 磁盘分区，挂载机制

![](https://i.imgur.com/yjqzd4Y.png)

> 意思就是，在linux系统的目录实际上和我们的硬盘是相互对应挂载的！可以通过： lsblk 指令查看。如我的就是下面这样：

![](https://i.imgur.com/P3E7Jrg.png)
![](https://i.imgur.com/L9Dmp6h.png)
> 可以看出来，我的电脑都是SCSI硬盘！且只有一块硬盘挂载！（因为只有sda没有sdb！）
![](https://i.imgur.com/69ShOI5.png)

### 挂载经典案例：增加一块硬盘
![](https://i.imgur.com/r3zjxPr.png)

![](https://i.imgur.com/aFOsqVv.png)
![](https://i.imgur.com/gg8sfgF.png)
![](https://i.imgur.com/iBDtaNp.png)
![](https://i.imgur.com/1vtC7A0.png)

### 查询磁盘/指定目录的使用情况
![](https://i.imgur.com/nl2K90D.png)
![](https://i.imgur.com/xpcy34L.png)
![](https://i.imgur.com/oYspUqx.png)
![](https://i.imgur.com/fEBA9kz.png)
> 为什么上面的指令是这么写呢？？？别忘了！
> 因为所有文件的十位描述分别为开头的'd'或者'-'加上后面的九位o，g，e权限。其中'd'对应目录， '-'对应文件！所以这里直接用grep正则化筛选就ok拉！
> 后面的wc -l 是wordcount，就把筛选出来的东西全部数了一遍！

![](https://i.imgur.com/9pFYJT1.png)

---


## 9. 网络配置

![](https://i.imgur.com/W7vhN3L.png)
![](https://i.imgur.com/5u6JLld.png)
![](https://i.imgur.com/YJ0xYVy.png)


### 网络环境配置方案

![](https://i.imgur.com/FMyILPa.png)
> 为什么不好？因为服务器的ip得要是固定的！随机的ip无法在工作中作为服务器！
![](https://i.imgur.com/N3eqwFr.png)

### 设置主机名和host映射
![](https://i.imgur.com/f8nVG2P.png)
> 那么问题就来了！如果我知道我的主机名但是不知道其IP地址，如何映射查找到此主机？
![](https://i.imgur.com/S6P5AUK.png)
![](https://i.imgur.com/onAgAUz.png)
![](https://i.imgur.com/tgC7bGM.png)


---
## 10. 进程管理（重点）
![](https://i.imgur.com/QJYBgqH.png)
![](https://i.imgur.com/1nao0aR.png)

![](https://i.imgur.com/ocOCDCK.png)
![](https://i.imgur.com/eV1PxpU.png)
![](https://i.imgur.com/QxNM2zu.png)

### 终止进程

![](https://i.imgur.com/IxtUy7O.png)

### 查看进程树：pstree

![](https://i.imgur.com/dFZf48W.png)


---

## 11. 服务管理与监控

![](https://i.imgur.com/Dmh5mTE.png)
![](https://i.imgur.com/BgS58nr.png)

![](https://i.imgur.com/weHPAAT.png)


### systemctl 指令

![](https://i.imgur.com/NXLiiio.png)

### 动态监控进程

![](https://i.imgur.com/KBcbyCr.png)

![](https://i.imgur.com/9v8eC28.png)

### 监控网络状态

![](https://i.imgur.com/dnFB3rU.png)

![](https://i.imgur.com/DUzaUHG.png)


---

## 12. RPM管理（注意，ubuntu里并无rpm， 用的都是deb）

![](https://i.imgur.com/DeNSSjv.png)
![](https://i.imgur.com/O5QT0FD.png)
![](https://i.imgur.com/OIJAXUp.png)
![](https://i.imgur.com/iZWXrnn.png)

### yum与apt（ubuntu是apt， centos是yum，但实际操作基本一致）
![](https://i.imgur.com/mF0FnBb.png)
> 使用ubuntu则将上面的yum全改为apt即可！
> 可见下面我用ubuntu的例子。
![](https://i.imgur.com/tNf8qIQ.png)

**补充：apt**

![](https://i.imgur.com/IZmQRdZ.png)
![](https://i.imgur.com/ru4QnPM.png)
> 现在的语法的话，直接apt就可以，不需要后面的-get


---

# shell编程
## 基础知识


![](https://i.imgur.com/jRfqtsS.png)
![](https://i.imgur.com/55c9Jyg.png)

![](https://i.imgur.com/mKAUktf.png)
![](https://i.imgur.com/vD9dKmF.png)

> **注意！！！ shell编程中等号间不可以打空格！！！**

![](https://i.imgur.com/UJi87gu.png)
![](https://i.imgur.com/srTrGIk.png)

### 设置环境变量

![](https://i.imgur.com/huMOpLI.png)
> 我发现Ubuntu实际上不需要第二步？？？
![](https://i.imgur.com/SIJmI7P.png)
> 尝试使用shell编程输出
![](https://i.imgur.com/QWR8lJo.png)
![](https://i.imgur.com/frdtR9B.png)

### 多行注释

![](https://i.imgur.com/LPuF1FD.png)
![](https://i.imgur.com/7PNk2Wh.png)

> 可以发现，注释掉的内容就不显示了！这和其他语言的多行注释是一个道理。只是操作没这么简便！

### 位置参数变量

![](https://i.imgur.com/zcnOMe7.png)
![](https://i.imgur.com/Rn4ZnRd.png)
![](https://i.imgur.com/1hYZR92.png)
> 其实就是利用$加上数字表示第几个参数，然后在控制台调用时候输入参数而已！！！看起来很吓人但原理和其他语言调用参数一个道理！

### 预定义变量

![](https://i.imgur.com/ncgyY4n.png)
![](https://i.imgur.com/JmxpFeo.png)
![](https://i.imgur.com/jrmYBrs.png)


---

## 逻辑部分


### 运算符

![](https://i.imgur.com/kt79hIn.png)
![](https://i.imgur.com/sQ9UpVp.png)
![](https://i.imgur.com/A1b820A.png)


### 流程控制之条件判断：if

![](https://i.imgur.com/k3gxAhr.png)
![](https://i.imgur.com/aFp8ZUm.png)

![](https://i.imgur.com/ttvCUe5.png)
![](https://i.imgur.com/HrCWboE.png)

![](https://i.imgur.com/rS46EbA.png)
![](https://i.imgur.com/N8j7483.png)

### 流程控制之情况分支：case

![](https://i.imgur.com/KPZPSjL.png)
![](https://i.imgur.com/fJc5hhq.png)

### 流程控制之循环遍历：for

![](https://i.imgur.com/Z7UMAsc.png)
![](https://i.imgur.com/f8ycA3x.png)
![](https://i.imgur.com/JvNJOpl.png)
![](https://i.imgur.com/zmj2jf3.png)
![](https://i.imgur.com/fnTcx0v.png)

### 流程控制之循环遍历：while
![](https://i.imgur.com/bJ9FBPo.png)
![](https://i.imgur.com/ybyUFHU.png)

### read读取控制台输入

![](https://i.imgur.com/Nfp08uO.png)
![](https://i.imgur.com/XIRPfZF.png)
![](https://i.imgur.com/DkL60tY.png)

> 10s之内不输入则退出程序！

### 系统函数

![](https://i.imgur.com/mSEI5gq.png)
![](https://i.imgur.com/fOByS90.png)


![](https://i.imgur.com/wZSy6qi.png)
![](https://i.imgur.com/wzWCpp9.png)

### 自定义函数
![](https://i.imgur.com/w2TaZDf.png)
![](https://i.imgur.com/nyGFlkz.png)


## 综合案例
![](https://i.imgur.com/0pJzRFF.png)
![](https://i.imgur.com/DIbSsTp.png)

步骤：

1. 建立数据库mine（create mine）
2. 写shell脚本
![](https://i.imgur.com/dqnLiiX.png)

4. 使用crontab -e 设置自动启动。
![](https://i.imgur.com/TfzryBW.png)


---


# Linux管理篇

## 日志管理（没有看很细）

![](https://i.imgur.com/oymeB6r.png)
![](https://i.imgur.com/3hTFBcS.png)
![](https://i.imgur.com/uz7OEUH.png)
![](https://i.imgur.com/Nv0eOid.png)
![](https://i.imgur.com/GFqFujt.png)
![](https://i.imgur.com/cJknhyn.png)
![](https://i.imgur.com/vDy5pUO.png)
![](https://i.imgur.com/Oy0qQTw.png)
![](https://i.imgur.com/bBFdXPT.png)


### 内存日志
![](https://i.imgur.com/7CNfUBl.png)

---

## 定制自己的Linux

![](https://i.imgur.com/RqeHlZd.png)
![](https://i.imgur.com/aZKld1m.png)
![](https://i.imgur.com/80G8JEv.png)

[详细步骤！参照老韩课程！](https://www.bilibili.com/video/BV1Sv411r7vd?p=127)

---


## linux系统备份与恢复

> 实体机不像虚拟机可以快照！

![](https://i.imgur.com/BTSPc4c.png)



### 1. 备份：dump

![](https://i.imgur.com/rVoMoW9.png)
![](https://i.imgur.com/M4t8i7R.png)
![](https://i.imgur.com/JZFxB9M.png)

> 当备份为一个独立的文件系统(独立的分区)时，可以使用-u，如果只是备份目录下的文件时，不能使用-u --- 所以说他的案例1是错的！不能用u

![](https://i.imgur.com/MokR5rV.png)
![](https://i.imgur.com/HCeJSQg.png)
> **注：Ubuntu的备份记录文件在上面的路径而不是ppt里那个！**

### 2. 恢复：restore    

![](https://i.imgur.com/cnYhB2r.png)
![](https://i.imgur.com/F9ROhKh.png)
![](https://i.imgur.com/55esx3P.png)
![](https://i.imgur.com/Pb5SNfm.png)

---


## WEBMIN：可视化的命令行界面！

这一节介绍了可视化的命令行界面 webmin，只需在linux系统中安装后便可通过网页进行命令行界面的交互类操作。链接留在下面。
[Webmin的安装与功能介绍](https://www.bilibili.com/video/BV1Sv411r7vd?p=138)


---

## bt宝塔：提升运维效率的服务器管理软件


![](https://i.imgur.com/zHCbYZy.png)
![](https://i.imgur.com/RyzrUcv.png)

感觉比webmin还要强大些呢！阿里云就是用的这个！
[bt宝塔的安装与功能介绍](https://www.bilibili.com/video/BV1Sv411r7vd?p=140)



---

# 面试题

### 1. 如何找回root密码？

1. 开机时按e进入编辑界面；
2. 找到Linux开始的行，在最后面输入： init=/bin/sh；
3. 输入完成后，ctrl+x进入单用户模式；
4. 输入mount -o remount,rw/；
5. 设置新密码！
6. 设置后再输入： touch /.autorelabel;
7. 输入： exec/sbin/init 等待新密码生效！

### 2. 韩p142-统计访问量和连接数！

![](https://i.imgur.com/gw17nXj.png)

code

1.
![](https://i.imgur.com/Rr2Eo2Y.png)
![](https://i.imgur.com/ZwCLV9D.png)
![](https://i.imgur.com/MMvucEN.png)

2.
![](https://i.imgur.com/0ymKFni.png)

老师代码：
![](https://i.imgur.com/WeuhER7.png)


---

### 3. 韩p143-找回mysql的root密码

![](https://i.imgur.com/0SJ57FK.png)

解决方法：
1. 查找my.cnf（在/etc/mysql目录下）
![](https://i.imgur.com/6aDRgza.png)

2. 在文件内输入skip-grant-tables跳过登录权限
![](https://i.imgur.com/FiGFJo2.png)

3. 重启mysqld服务读取修改
![](https://i.imgur.com/POFolbW.png)

4. 输入空密码进入sql，进去改密码！

在数据库里的mysql库-user表-authentication-string修改新密码就是！
![](https://i.imgur.com/zLUmW1e.png)

5. 回到/etc/mysql目录，注掉开始的修改，重启服务就ok！


---

### 4. 韩p144-访问量排名

![](https://i.imgur.com/bQhz2mh.png)

code

![](https://i.imgur.com/pcUseP4.png)

![](https://i.imgur.com/a4jiEyO.png)

![](https://i.imgur.com/RqLTO4t.png)

![](https://i.imgur.com/Ujn7zcg.png)

老师code：
![](https://i.imgur.com/xPqafzQ.png)


---

### 5.韩p144-tcpdump！

![](https://i.imgur.com/AVAFWd6.png)

code

![](https://i.imgur.com/0ECL7jZ.png)
这样就可以不停打印！

然后在原先基础上使用 >> 保存即可！

![](https://i.imgur.com/Gk5CUg6.png)


---

### 6.韩p145~147-系统权限划分！！（重要且有趣！！！）

1. 权限划分题
![](https://i.imgur.com/0s6N88I.png)

敞开谈！没有标准答案，为了表示自己是知道的，我们最好从文件权限含义（在文件与目录上）开始，然后慢慢谈！

![](https://i.imgur.com/AQ59XnO.png)

![](https://i.imgur.com/CH1olRK.png)

+++++++++++

2. 权限思考题（真他妈有趣！）
![](https://i.imgur.com/TTI0frR.png)

回答：
1.yny
2.nnn
3.ynn
4.yny

- 对目录有执行权限才进得去目录，不然连目录都进不去！
- 对目录有读权限才可以使用ls之类的指令！
- 对目录有写权限才可以对目录内的文件进行增删！
- 这些在上面都有


---

### 7.韩p149-io读写监控
![](https://i.imgur.com/vZZ19ig.png)

1. netstat：查看网络状态
2. lsblk：查看硬盘分区情况
3. systemctl：管理一系列服务状态
4. ps/top：静/动态查询任务情况
5. chkconfig：查看服务启动状态
6. crontab/at：设置定时任务


![](https://i.imgur.com/jQPGSYB.png)
查看内存：top

io读写：iotop

磁盘存储：df -lh / lsblk？

端口占用：netstat -tunlp

进程查看：ps -aux

![](https://i.imgur.com/PD9VhUi.png)


---

### 8.韩p150-统计文件个数 & 行数

1.
![](https://i.imgur.com/X8gFLaK.png)

mine

![](https://i.imgur.com/duTihq4.png)
![](https://i.imgur.com/cpu31o4.png)

+++++++++++++++

2.
![](https://i.imgur.com/t7R2vk2.png)

mine

![](https://i.imgur.com/N24uFBJ.png)
![](https://i.imgur.com/cpr4xrK.png)
![](https://i.imgur.com/3EMRXbM.png)

++++++++++

3.
![](https://i.imgur.com/29R7tlZ.png)
![](https://i.imgur.com/cQZQxZo.png)

++++++++++++++++

4.
![](https://i.imgur.com/S38of3f.png)

mine

![](https://i.imgur.com/Wh29omN.png)
![](https://i.imgur.com/j3pWcZE.png)

![](https://i.imgur.com/0ru5buk.png)


---

### Final.韩p152-Linux优化

![](https://i.imgur.com/X65Taeb.png)



---


# 追加：自己学习的补充！

## 1. awk的妙用 - [原文](https://www.linuxprobe.com/linux-awk-clever.html)

简单来说awk就是把文件逐行读入，（空格，制表符）为默认分隔符将每行切片，切开的部分再进行各种分析处理，命令格式如下：

```
awk [-F 分隔符] ‘命令’ 

这里的常用命令一般是 print ！ （注意要用大括号{}括起来）

-F 分隔符： 就是指定分隔符！
```

> [-F 分隔符]是可选的，因为awk使用空格，制表符作为缺省的字段分隔符，因此如果要浏览字段间有空格，制表符的文本，不必指定这个选项，但如果要浏览诸如/etc/passwd文件，此文件各字段以冒号作为分隔符，则必须指明-F选项！

eg.
![](https://i.imgur.com/Y6J9jEg.png)


eg. 用awk 截取/etc/passwd中的用户名！

![](https://i.imgur.com/jUPU89W.png)


eg. 用awk 截取用户名以及登录记录！
![](https://i.imgur.com/6pQ88k4.png)


---
## [下面内容的源地址](https://www.cnblogs.com/xuxinstyle/p/9609551.html)

## 2. Find补充：查找一定时间内修改过的文件！

![](https://i.imgur.com/VQosCpA.png)

eg.
![](https://i.imgur.com/o88l2X7.png)

---

## 3. shell code的一些常识

首先， **#!/bin/bash** 这里的 **#！** 实际上是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种Shell。

另外重复下， shell的多行注释：  
```
第一行前 :<<!
最后一行后 !

eg.
:<<!
echo 'New Skill? Nope!'
my_name='WEi jH'
echo "my_name = "$my_name"! \n"
your_name='L_rui'
echo "my name is $my_name and your name is $your_name"
echo $my_name $your_name
echo ${#my_name}   # 检查字符串长度方式：加#

echo ${my_name:2:5}   # 字符串切片
!
```

并且，shellcode的echo语句的转义符同样为 \ 

此外，很重要的一点：VIM怎么显示行号？

`    ：set nu`

并且，使用if检查条件是否成立时，常和test连用，即 ==if test 语句==
![](https://i.imgur.com/ZxTLobm.png)
![](https://i.imgur.com/d2aH8k4.png)
![](https://i.imgur.com/Ft583iC.png)


---



