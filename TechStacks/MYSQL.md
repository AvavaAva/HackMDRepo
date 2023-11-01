###### tags: `learn` `DataBase`

# MySQL

首先， 命令行连接至mysql方式和linux相同：
```
mysql -u 用户名 -p密码
```
**我在第一次尝试使用命令行时候失败了！为什么呢？**
> 因为你需要事先将mysql路径添加入环境变量阿混蛋！


---

## 总体结构

![](https://i.imgur.com/9QJNzE3.png)
![](https://i.imgur.com/sfrow6Y.png)
![](https://i.imgur.com/jCQOXEB.png)


---

## 操作指令

### 数据库相关： database

#### 创建数据库：CREATE
![](https://i.imgur.com/rqiEVEH.png)
```
CREATE DATABASE mine_03 character set utf8 collate utf8_bin；
```
#### 查看，删除数据库：SHOW , DROP
![](https://i.imgur.com/K5MBimQ.png)
> 注：有时候创建的数据库名字若是内置量名，则会报错，比如我不能直接：
> CREATE DATABASE INT
> 这样就报错。但是如果加上反引号‘ ’就可以成功创建！



#### 备份/恢复数据库：mysqldump与source

![](https://i.imgur.com/1G3CSmM.png)
![](https://i.imgur.com/aWUPl5P.png)
![](https://i.imgur.com/9QpnndI.png)
> 实际上-B 不用写！！！
> 在这里我遇到一个问题，就是我在sql命令行里疯狂地报错！
> 但是实际上**备份根本就不是在sql命令行进行！是在dos命令行进行**！！！！！
恢复相对简单。在 mysql命令行执行即可。

![](https://i.imgur.com/x79MCK1.png)


---

### 单表相关

#### 创建表

![](https://i.imgur.com/1tFuAnl.png)
> 自己使用命令行创建了密码表！

![](https://i.imgur.com/B7NIzg4.png)

##### Mysql数据库常用的数据类型！（重要）

![](https://i.imgur.com/5XZY2P5.png)


![](https://i.imgur.com/CtbkGHF.png)
![](https://i.imgur.com/Ms5vKfZ.png)
> 其中用的比较多的：
    > 整数：int
    > 小数：double, decimal（精度较高）
    > 文本：char, varchar
    > 日期：datetime



---

1. 整型

![](https://i.imgur.com/orQr0uq.png)
**注意， 整型若不指定unsigned， 则默认有符号！！！（也就是说会少一位来存符号！）**
![](https://i.imgur.com/Mo7Fe0K.png)


![](https://i.imgur.com/LQXfYaN.png)
> 创建一个test数据库用于测试！看看tinyint是否真的只存到这么大。
![](https://i.imgur.com/HRa4qAn.png)
> 可以发现！超出范围，也就是说确实范围是被限制住了！


---

2. 小数类型

![](https://i.imgur.com/LbiJo7s.png)

> 例： 创建一个表来看看double和decimal对付长位数小数！
> ![](https://i.imgur.com/v38Vir7.png)
> ![](https://i.imgur.com/uuhC1H5.png)
> 可以很明显看出来， decimal这边给我没满20位的地方全部补上了0！！！！很棒，精度够高。


---

3. 字符串类型

![](https://i.imgur.com/ofRWRop.png)
> **注意，varchar虽然最长65535个字节，我们却不能填写65535！ 为什么呢？因为我们需要最大三个字节去记录我们字符串的大小！！！！）**
> 由于utf8是三个字节表示一个字符，所以如果是utf8就是最大21844个字符！
![](https://i.imgur.com/GcnIHsu.png)


---


##### 字符串使用细节

![](https://i.imgur.com/nlJLSF0.png)
> 上面也有提及，varchar是变长，而且我们输入的长度是字符长度，不是字节长度，要换算成字节后小于65532（比如，utf8 则乘以3因为是3字节为1字符；gbk则乘以2）
![](https://i.imgur.com/sRwmsK8.png)


---

![](https://i.imgur.com/iiNEg19.png)
> 所以说varchar和char不同在于：它是可变长！
> 可变长体现在哪里？
    > 1. 创建时最大长度虽是65535字节，但会根据编码类型而变为不等长的字符（比如，utf8 则乘以3因为是3字节为1字符所以utf8最长是21844；gbk则乘以2所以最长为32766）
    > 2. 插入数据的时候，若数据长度没达到最大长度，多余空间除去1-3字节存放长度信息外，是可以分出去再利用的；而char就算未满最大长度，多余空间也会被占掉。


---

![](https://i.imgur.com/wUyKCXW.png)
> char的优点：查询快！

---
![](https://i.imgur.com/EWPqlLj.png)
![](https://i.imgur.com/5kWeqqR.png)


---

4. 日期类型

![](https://i.imgur.com/V5pJNLq.png)

> 案例： 创建一个试试
> ![](https://i.imgur.com/wAm2G6T.png)
> ![](https://i.imgur.com/DnefqdC.png)
> 发现，时间戳不需要我们手动计入，会自动更新！！！！
> （这玩意儿用在记录我的消费记录或者密码记录之类的应该很有用！）


---

##### 练习

![](https://i.imgur.com/cFogfz5.png)

我的代码：
![](https://i.imgur.com/ghGeaBm.png)
![](https://i.imgur.com/99odfNl.png)


---

#### 修改表

![](https://i.imgur.com/TfI5je5.png)

应用：
![](https://i.imgur.com/aSEHsTf.png)

> 我的
> 0.![](https://i.imgur.com/OVvz7eP.png)
>
> 1.![](https://i.imgur.com/YBqRQq3.png)
>
> 2.![](https://i.imgur.com/Jj29sHq.png)
>
> 3.![](https://i.imgur.com/hrU3sUY.png)
>
> 4.![](https://i.imgur.com/uHOCK40.png)
>
> 5.![](https://i.imgur.com/7qoGtJ9.png)
>
> 6.![](https://i.imgur.com/lB2nHPt.png)

> check：![](https://i.imgur.com/ep7GIhX.png)



老韩答案：
![](https://i.imgur.com/7e9FecA.png)
![](https://i.imgur.com/3xOV6Q9.png)

---


#### 数据库的CRUD


![](https://i.imgur.com/IaKkVwF.png)

##### insert语句

![](https://i.imgur.com/bXvPivW.png)

案例：在这个基础上我加了一个add_time自动记录加入数据时间！（很有用感觉hhh）
![](https://i.imgur.com/CHnzrVW.png)

> my code
> ![](https://i.imgur.com/NRQZ9i2.png)
> ![](https://i.imgur.com/bHvtGIb.png)
> ![](https://i.imgur.com/n9mmH4d.png)


##### insert的注意事项

![](https://i.imgur.com/GM5nJu2.png)
![](https://i.imgur.com/NBz63B5.png)
![](https://i.imgur.com/1QsAYXA.png)


---

##### update语句

![](https://i.imgur.com/6XA3TKf.png)
![](https://i.imgur.com/FYkFSbD.png)


> 并未使用他的案例，而是把自己的拿来随便试试。
> my code：
> ![](https://i.imgur.com/CSCbSiP.png)
> ![](https://i.imgur.com/pSZ5bqc.png)


---

##### delete语句

![](https://i.imgur.com/Lmjif7Q.png)
![](https://i.imgur.com/vK69siw.png)
> 不写后面的where语句则会将表全部删除！！！要删除某一列还是用alter！！
> ![](https://i.imgur.com/e78UHv3.png)


案例实现：先加入老妖怪记录再删除！！

![](https://i.imgur.com/I2sUC4r.png)
![](https://i.imgur.com/Yyf93d1.png)


---

##### select语句

![](https://i.imgur.com/F5NDpw7.png)


尝试自己做个student表玩玩
![](https://i.imgur.com/DQBMEXe.png)
![](https://i.imgur.com/YwzKuWN.png)
![](https://i.imgur.com/efMIwBA.png)
![](https://i.imgur.com/44sDKHQ.png)
![](https://i.imgur.com/Whfkc6b.png)

**开始提升**
![](https://i.imgur.com/GDvi3sz.png)

![](https://i.imgur.com/vSD1Vmh.png)

> my code：
> 1.![](https://i.imgur.com/N4u4jP8.png)
> 
> 2.![](https://i.imgur.com/xgaT6Uz.png)
> 
> 3.前两步都用了别名！
> 
> 课后很简单没什么搞的必要哈。

![](https://i.imgur.com/m7tiyPc.png)

![](https://i.imgur.com/jkOLOQq.png)


> my code：
> 因为数据不同，只是用差不多形式搞了下课后那几个（也就是条件查找）
> ![](https://i.imgur.com/5xs1LFv.png)
> 
> ![](https://i.imgur.com/5oYA8AP.png)
> 
> ![](https://i.imgur.com/RZ3s7Up.png)

![](https://i.imgur.com/YGQAldY.png)

> ![](https://i.imgur.com/OSK6Fyc.png)


---

### 函数

#### 统计函数
![](https://i.imgur.com/DZfycgg.png)

![](https://i.imgur.com/Se4kNIr.png)
> 也就是说 count(*)是不会排除null的！！！！！


![](https://i.imgur.com/RdGaB0P.png)
![](https://i.imgur.com/3Num3Pn.png)
> 另外同理还有max 和 min函数等！就不写啦！


---


#### select中的过滤/分组函数 groupby

![](https://i.imgur.com/fMxfnA4.png)
![](https://i.imgur.com/p5YyyNK.png)

![](https://i.imgur.com/l9NN3Nb.png)


---

#### 字符串函数

![](https://i.imgur.com/jtTmMLq.png)

示例：
![](https://i.imgur.com/JvwIhno.png)

![](https://i.imgur.com/1H3tsAv.png)

![](https://i.imgur.com/H9niIhk.png)

![](https://i.imgur.com/IgKxhrh.png)

![](https://i.imgur.com/bElNQfo.png)
![](https://i.imgur.com/1RRqwBt.png)

![](https://i.imgur.com/q5JTAcn.png)
> 按照字节返回！！！

![](https://i.imgur.com/g8yPDrQ.png)
> replace和其他语言一样，就实现替换功能！

> 练习：以首字母大写的形式返回所有学生的姓名！
>1. ![](https://i.imgur.com/cDicG4L.png)
>
>2. ![](https://i.imgur.com/te6YM8b.png)
>可以看出来，这些函数真的很灵活欸！


---

#### 数学函数

![](https://i.imgur.com/MhvrW2h.png)

> ![](https://i.imgur.com/PqYKedn.png)
> 这里的函数都很简单！！！


---

#### 日期函数

![](https://i.imgur.com/C8M8Or9.png)
![](https://i.imgur.com/oUhwWjJ.png)
![](https://i.imgur.com/FLKHfi1.png)
```
注意！ CURRENT_TIME(), TIMESTAMP(), NOW()完全等价！！！
```

> 案例，求求我还能活多少天（按80岁）
> ![](https://i.imgur.com/Ew7GVmU.png)


---

#### 加密函数

![](https://i.imgur.com/Fy1xJD0.png)
> 注：PASSWORD()函数已经没有了哦！
![](https://i.imgur.com/dixHdJq.png)

==**示例：使用MD5加密存放密码！！！**==
![](https://i.imgur.com/19swTgp.png)

> 注意，MD5()是不可逆的！查不到原密码！


---

#### 流程控制函数

![](https://i.imgur.com/fQjDkz4.png)

![](https://i.imgur.com/OUY3Nq0.png)

> ==首先，尝试if！==
> if()是典型的三元表达式：若满足第一元，则输出第二元；否则输出第三元。
> ![](https://i.imgur.com/dRn28C8.png)

> ==其次尝试IFNULL！==
> 这个函数也很简单，如果第一元为空则返回第二元；否则返回第一元。
> ![](https://i.imgur.com/OOB9ErA.png)
> 因为我第一个为空，所以全返回第二个辣！

![](https://i.imgur.com/qPL7I3n.png)


---

### 多表查询（难点）

#### 复习单表的查询要点

首先我们创建了三张表！然后会根据这三张表进行慢慢的学习！

先注意一点！在sql中日期可以直接进行比较！！！
![](https://i.imgur.com/vXGJFwJ.png)

模糊查询：
![](https://i.imgur.com/wVIX9hs.png)

==查询null不用 = null 而是 is null！==
![](https://i.imgur.com/t0EoVKF.png)

==如何同时进行两次排序的查询？
按主次顺序写在后面就可以！==
![](https://i.imgur.com/YHp8y6B.png)

==分页查询==
![](https://i.imgur.com/QTYUrIJ.png)

![](https://i.imgur.com/Jp8ToxH.png)
![](https://i.imgur.com/3j8qxFR.png)


![](https://i.imgur.com/sa2o2wx.png)

my code：
![](https://i.imgur.com/ZEz7cR4.png)


---

#### 多表查询部分

![](https://i.imgur.com/OSWl7XZ.png)

my code：
![](https://i.imgur.com/R1Xkivi.png)

![](https://i.imgur.com/u7wgr5x.png)

![](https://i.imgur.com/mxEFrTi.png)
![](https://i.imgur.com/ZQ0UM7d.png)
![](https://i.imgur.com/sfXY8Cx.png)

my code：
![](https://i.imgur.com/xeweTxb.png)
![](https://i.imgur.com/q1WDPhL.png)
![](https://i.imgur.com/MkhgYzT.png)

#### 自连接
![](https://i.imgur.com/usHoqwn.png)
![](https://i.imgur.com/YBqw612.png)

my code：
![](https://i.imgur.com/jYb2P0P.png)

![](https://i.imgur.com/hKVBfzJ.png)


---


#### 子查询

![](https://i.imgur.com/D6GHioJ.png)

my code：
![](https://i.imgur.com/bE4mLHh.png)


![](https://i.imgur.com/bwAdW4s.png)

> my code：

![](https://i.imgur.com/K7naXtJ.png)


==同样地，子查询出来的结果也可以作为临时表与其他结合进行结合查询！！！==
![](https://i.imgur.com/gzH3H0l.png)

**==也就是说，子查询不仅可以用在WHERE句，也可以用在FROM句！==**


---

**all & any**

满熟悉了，all其实就是对比出来取最高的，而any就是对比仅仅去除最低的！

++示例：用all查最高！++
![](https://i.imgur.com/JdfjDjP.png)

++示例：用any排除最低！++
![](https://i.imgur.com/mTKdWK5.png)


---

**多列子查询**
![](https://i.imgur.com/9Lj8mBw.png)
由于我表里smith没有符合要求的这里我换成了WARD。
my code：
![](https://i.imgur.com/dKstvnJ.png)


---

##### 子查询例题（综合提升

> 1.
> ![](https://i.imgur.com/MDd70Pn.png)
> 
> my code：
> ![](https://i.imgur.com/ZznDkUH.png)
> 

> 2. 
> ![](https://i.imgur.com/RqoqSrG.png)
> 
> my code：
> ![](https://i.imgur.com/s8QPhLt.png)


> 3.
> ![](https://i.imgur.com/pMvuxsx.png)
> 
> my code：
> ![](https://i.imgur.com/wa1ZIFD.png)



---

##### 表复制

![](https://i.imgur.com/dGB8nZp.png)
> 每一次自我复制会让表的内容翻倍！也就是2的指数次方爆炸。
> 但是注意，==若表内有主键，自我复制就会报错！（毕竟主键不可重复）==


```
面试题：如何对表进行去重？（这个应该是指不带主键的表吧，不然没必要去）
```

my code：
首先，我们创建一张表用于测试。这里有个语法： create table xxx like xxx. 
会创建和后者格式一样的空表！
![](https://i.imgur.com/cHrVT18.png)

之后，我们将emp表的内容复制入新建的test表中。
![](https://i.imgur.com/nboQgWf.png)

其次，我们再复制一遍test表内内容，这样表内就有重复了！
![](https://i.imgur.com/L5Hgq3V.png)

![](https://i.imgur.com/ZD8XSq7.png)
![](https://i.imgur.com/ALqwMS1.png)


---

#### 合并子查询

![](https://i.imgur.com/RUnRhY8.png)
> 第一眼会让人觉得直接用 OR！
> 注意哪里不同于OR？
    > union all：不去重！！！不同于OR。
    > ![](https://i.imgur.com/IJAGhwp.png)
    > union：去重，很像OR。
    > ![](https://i.imgur.com/73aPDBq.png)


---

#### 外连接

![](https://i.imgur.com/avCKClI.png)

> 为了搞明白，我们也创建这个表来试试！
> 
> ![](https://i.imgur.com/NAUT8qo.png)
> ![](https://i.imgur.com/humscHi.png)
> ![](https://i.imgur.com/7BwCyyH.png)


创建完成后，我们就可以开始尝试测试左右外连接！
![](https://i.imgur.com/Qr0BS8W.png)

my code：
> 1.左连接
> ![](https://i.imgur.com/cn23gPj.png)
> 
> 2.右连接
> ![](https://i.imgur.com/JjbzgTH.png)


> 课堂习题：
> ![](https://i.imgur.com/qUL674W.png)
> 1.右外连接
> ![](https://i.imgur.com/phS2OKq.png)
> 2.左外连接
> ![](https://i.imgur.com/4lvuLtZ.png)


---

### 约束
![](https://i.imgur.com/GSdm59Z.png)

![](https://i.imgur.com/lg1r0wr.png)


#### 键约束

![](https://i.imgur.com/ql8Ex3O.png)

创建一个带主键的表
![](https://i.imgur.com/VCFUinO.png)
![](https://i.imgur.com/pXj5lb5.png)
> 会发现当主键重复时候就会报错！

==主键的细节==

![](https://i.imgur.com/7f0iSJ0.png)
![](https://i.imgur.com/yCO4gdn.png)

==复合主键==
![](https://i.imgur.com/wUHJ37n.png)


---

==非空 & 非重复约束==
![](https://i.imgur.com/jnLmoKt.png)
![](https://i.imgur.com/YaFpbKo.png)


---

==外键==

![](https://i.imgur.com/urmzYiW.png)

使用细节
![](https://i.imgur.com/pK3QRrW.png)
![](https://i.imgur.com/oBN4oaS.png)
![](https://i.imgur.com/tGtDg5i.png)
![](https://i.imgur.com/pe19zKo.png)

![](https://i.imgur.com/Hk4W0MW.png)
> 注：==只有innoDB引擎才支持外键！！！！==


---

#### check约束

![](https://i.imgur.com/OzHgzMu.png)


举例：尝试修改emp表的工资上下限并试试效果。

![](https://i.imgur.com/FDbnKNP.png)
![](https://i.imgur.com/ajLOPMZ.png)
> 为啥？？？？？


那我新建一个表试试
![](https://i.imgur.com/hoLHdCN.png)

![](https://i.imgur.com/tFJOtRC.png)
> 可以发现是有用的！
> 但是，通过desc 查出来的信息却看不到？？？
> ![](https://i.imgur.com/oEOj1oe.png)


##### 综合案例

![](https://i.imgur.com/uvOuzsB.png)

my code：
1.goods：
![](https://i.imgur.com/WQNNiaE.png)

2.customer：
![](https://i.imgur.com/VdEGvuI.png)
![](https://i.imgur.com/Pz4rFLJ.png)
> 可以发现这个check约束生效！

3.purchase：
![](https://i.imgur.com/qs0OYls.png)

==总的来说，搞清楚谁是外键，就很简单！==

---

#### 自增长

![](https://i.imgur.com/YjMNEWv.png)

自增长尝试（用于自动更新主键id）
![](https://i.imgur.com/RXWir0l.png)
![](https://i.imgur.com/2ade15S.png)

![](https://i.imgur.com/hUldrgC.png)
> 很神奇地自动更新了！


==细节==
![](https://i.imgur.com/sXuGvyz.png)

> 修改自增长！
> ![](https://i.imgur.com/djRpneI.png)
> ![](https://i.imgur.com/29YrmaY.png)
> ![](https://i.imgur.com/Pnrb76r.png)

> 成功！相当于就是修改自增长的底数！！！


---

## 索引！（常问到哦！）

![](https://i.imgur.com/mZjZNVx.png)
![](https://i.imgur.com/iUeTkAr.png)


### 创建索引
![](https://i.imgur.com/bxgWWkq.png)
![](https://i.imgur.com/staMOIb.png)
> 注意，==创建索引后，仅仅对创建的索引的列的查询有加速效果！==
> **这是当然的，毕竟相当于给这个列加了个目录**。

### 索引机制

#### 1. 索引的效率问题
==面试常见问题==：（我网易就被问到了！！呜呜呜）
![](https://i.imgur.com/DNhleZn.png)
> 为什么用索引快？因为会形成索引的数据结构！比如二叉树！！！（当然，根据数据库课程来看，是B树和B+树啦，B树所有节点都有对应源文件的目录索引，而B+树仅叶节点有对应源文件目录索引。）
> 索引代价：每次进行dml（插入，修改与删除）时，索引的结构文件也会被修改，当然会花时间了！

#### 2. 索引种类

![](https://i.imgur.com/wydgGIn.png)
> 1. 主键自动成为索引！所以根据主键查询很快呀！
> 2. ==unique 属性也会自动成为索引，所以查起来也比较快。仅仅在InnoDB引擎下才可以用！==
> 3. 普通索引用的最多！因为我们并不总是根据唯一值查询！同时，普通索引由于其不唯一性，一定是稀疏索引！！毕竟我们定位到了某一个可能有重复项后还要继续查也有可能！
> 4. ==全文索引：用于MyISAM引擎！==


#### 3. 创建，修改及删除索引

![](https://i.imgur.com/7SAQduH.png)
![](https://i.imgur.com/6clSe8Z.png)

```
show indexes from table_name;
```

![](https://i.imgur.com/CLAqBTC.png)

![](https://i.imgur.com/Dx6amG3.png)
![](https://i.imgur.com/9BG2RIG.png)
![](https://i.imgur.com/btpkORr.png)
![](https://i.imgur.com/MibXEaw.png)
![](https://i.imgur.com/o0MXmlh.png)



---

## 事务（面试高频！）

![](https://i.imgur.com/SU767XQ.png)

> 事务的特性：ACID （from数据库md）
> 
>     原子性Atomicity: 事务的一组更新操作原子不可分；要么都做，要么都不做。
> 
>     一致性Consistency: 事务的操作状态应正确且符合一致性规则。
> 
>     隔离性Isolation: 多个并发执行事务间互不影响。
> 
>     持久性Durability: 已提交事务的影响时持久的，且被撤销事务的影响是可恢复的。

### 事务内容 & 细节 （注，事务仅仅在InnoDB中才有！！）

![](https://i.imgur.com/Hqc3Uyb.png)

举例：（当然我已经很熟悉了）
![](https://i.imgur.com/15vBf1k.png)

> 尝试保存点！
> 
> 设置保存点a，之后插入一条数据
> ![](https://i.imgur.com/zCfriK1.png)
> 
> 再设置保存点b，再插入一条数据
> ![](https://i.imgur.com/ol0J0cs.png)
> 
> 我们回退至b保存点！
> ![](https://i.imgur.com/sgCpt6u.png)
> 
> 再回退回a保存点！
> ![](https://i.imgur.com/fbXu8Py.png)

```
如果我们直接写rollback 就是全体撤销该事务！
反正，如果使用commit 就是提交！提交后就没法修改啦！

总之，要使用rollback就必须在开始前开启事务也就是 START TRANSACTION!!!
```
![](https://i.imgur.com/TAPm1mf.png)
![](https://i.imgur.com/uOYLYFU.png)
> 注意！！！只有InnoDB存储引擎才可以使用事务机制！！！！


---

### 事务的隔离级别

![](https://i.imgur.com/8uH27oi.png)
![](https://i.imgur.com/B6EufeZ.png)

> 注意： 不可重复读指的是修改和删除；幻读指的是插入！
> 注意2：在mysql8.0里，查看隔离级别语句： SELECT @@transaction_isolation

![](https://i.imgur.com/gCDirnW.png)
> ==注意，幻读和不可重复读 是在事务已提交的基础上！==


==举例说明：==
![](https://i.imgur.com/6ruukMX.png)

> 我们可以使用两个控制台实现！
> ![](https://i.imgur.com/fmVhdgk.png)
> 
> ==**1.使用语句查看当前隔离级别： SELECT @@transaction_isolation**==
> ![](https://i.imgur.com/WE3Kcjp.png)
> 也就是说，我们的默认隔离级别是可重复读！对应上面的第三个级别！（不加锁但不会出现上面的几种问题）
> 
> ==**2.修改其中一个控制台的隔离级别！SET SESSION TRANSCATION ISOLATION LEVEL READ UNCOMMITED；**==
> 直接设置到最低的隔离级别！
> ![](https://i.imgur.com/nJHvg8T.png)
> ![](https://i.imgur.com/WWkJbQu.png)
> 修改完成！
> 
> ==**3.两边都开始事务！**==
> ![](https://i.imgur.com/PQTpbvD.png)
>
> ==**4.在隔离级别高的那边创建一张表！**==
> ![](https://i.imgur.com/17eiLBe.png)
> 
> ![](https://i.imgur.com/P9VeZYh.png)
> 可以发现，现在在两边查都是空表！
> 
> ==**5.现在在隔离级别高的命令行里添加一条数据但是先不提交！**==
> ![](https://i.imgur.com/rsrheO6.png)
> 
> 我们用隔离级别低的那个看一下能不能检索到
> ![](https://i.imgur.com/qFL1jF6.png)
> ==明明还没提交，就能查到了！这就是脏读！==
> 
> **==6.我们在隔离等级高的那个命令行进行一系列修改，再用低的查一查！==**
> **6.1 修改**
> ![](https://i.imgur.com/yPOkcRB.png)
> ![](https://i.imgur.com/ErkncgH.png)
> 
> **6.2 查询**
> ![](https://i.imgur.com/crqy8TQ.png)
> 发现不可重复读（读到修改内容）和幻读（读到添加内容）都有发生！！！
> ==注意，幻读和不可重复读 是在事务已提交的基础上！==

==可串行化：有加锁读功能==
![](https://i.imgur.com/zPn8vYY.png)
如上图示意，若有其他终端在对表进行事务操作，那么隔离级别设置为可串行化的终端，在事务提交或撤回完成前无法读取那张表！但是一旦提交，就立马能看到了！好神奇哦

![](https://i.imgur.com/r7qOFXY.png)


---

## 存储引擎 InnoDB & MyISAM （面试高频！）

![](https://i.imgur.com/HhD0Ob9.png)

使用 SHOW ENGINES 显示支持的引擎
![](https://i.imgur.com/2mPcHKV.png)
> InnoDB：写的很清楚，支持事务，外键以及行级锁。
> MEMORY：存在内存的表，巨快但是通常用于临时表！

![](https://i.imgur.com/1PLYIJx.png)
![](https://i.imgur.com/20JIirn.png)
![](https://i.imgur.com/RxPrVT4.png)
![](https://i.imgur.com/wpxxPpA.png)


---

## 视图

### 基本原理
![](https://i.imgur.com/yn3X04x.png)
![](https://i.imgur.com/DJHc1AU.png)
![](https://i.imgur.com/xLsAlcw.png)
> 视图可以由一张表或多张表构成！
> 视图与原表的关系是相互对照；任意一侧的修改都会影响另一侧。

### 视图使用

![](https://i.imgur.com/zNJYHJL.png)

> my code：
> ![](https://i.imgur.com/0OaulCQ.png)
> ![](https://i.imgur.com/zedcgGz.png)

> 视图上的修改也会影响到原表。
> 我在原表的Engineer并非全大写，那么我们尝试通过update视图来修改原表。
> ![](https://i.imgur.com/Grkkh2E.png)
> 下面查原表：
> ![](https://i.imgur.com/y8xFb8F.png)
> 还真的是！

==视图的实践==
![](https://i.imgur.com/JRUJn1l.png)


![](https://i.imgur.com/PFp1Vhz.png)

> my code：
> ![](https://i.imgur.com/E9jbnpV.png)
> 
> ![](https://i.imgur.com/4ShYCqy.png)


---

## 用户管理！

![](https://i.imgur.com/xTiAOSf.png)
![](https://i.imgur.com/DOIoj49.png)
![](https://i.imgur.com/SJ638fA.png)


> 创建用户并设置密码
> ![](https://i.imgur.com/Ul1qdJz.png)
> 
> 尝试用该用户登录
> ![](https://i.imgur.com/V1489nZ.png)
> 成功！
> 删除用户：
> ![](https://i.imgur.com/kj06JZ1.png)

### 用户权限管理

![](https://i.imgur.com/t6s1Y9c.png)
![](https://i.imgur.com/iVnpiN4.png)
![](https://i.imgur.com/LjHsqIL.png)


==老师代码实例==
![](https://i.imgur.com/l12K3F9.png)
![](https://i.imgur.com/QX0H2yP.png)
![](https://i.imgur.com/bxfnEXq.png)


### 用户管理细节

![](https://i.imgur.com/xsA80Fs.png)


---

## 恭喜你，看完了！（但是他的课程里没有讲触发器的知识，还有比较重要的数据库三范式，这些都可以参照隔壁DataBase的md文件。


---

## 习题

![](https://i.imgur.com/bYgty8L.png)

2.--------
![](https://i.imgur.com/tUU1ILW.png)

3.--------

3.1
![](https://i.imgur.com/GueuJza.png)

3.2（这里会用到if或者ifnull，我认为ifnull更合适题目要求就用了它）
![](https://i.imgur.com/w89pX8O.png)

4.--------

4.1
![](https://i.imgur.com/4PtDtEC.png)


4.2（我用的not，当然也可以创建一个在该范围的子表然后排除掉）
![](https://i.imgur.com/NRj9Yrx.png)

4.3
![](https://i.imgur.com/AoyP4dn.png)

4.4
![](https://i.imgur.com/tR0rZKa.png)

4.5
![](https://i.imgur.com/YqXYJSV.png)


5.--------

5.1
![](https://i.imgur.com/NxtbtM5.png)

5.2
![](https://i.imgur.com/V5P3Jf0.png)


6.--------
![](https://i.imgur.com/DYm3uKh.png)
![](https://i.imgur.com/RyMSzFD.png)

6.1
![](https://i.imgur.com/e8PsRYH.png)

6.2
![](https://i.imgur.com/MUiWpRb.png)

6.3
![](https://i.imgur.com/4ZP7Z7K.png)

6.4
![](https://i.imgur.com/R14jYRE.png)

6.5
![](https://i.imgur.com/AU9Sp2m.png)

6.6
![](https://i.imgur.com/YJeeIFt.png)

6.7
![](https://i.imgur.com/TCusLiy.png)

6.8
![](https://i.imgur.com/5sPG8jv.png)

6.9（迷惑？）
![](https://i.imgur.com/OCV9urV.png)

6.10
![](https://i.imgur.com/720YH2k.png)

6.11（两种方法： 1.replace， 2. concat）
1.![](https://i.imgur.com/Y66Yzb5.png)

2.![](https://i.imgur.com/fBzrrAS.png)

6.12
![](https://i.imgur.com/f1phXQm.png)

6.13
![](https://i.imgur.com/ScGhWTK.png)

6.14
![](https://i.imgur.com/MLcCowK.png)

6.15
![](https://i.imgur.com/TZWyKnq.png)

6.16
![](https://i.imgur.com/K0uBdCj.png)

6.17
![](https://i.imgur.com/Xk6SVzd.png)

6.18
![](https://i.imgur.com/zXHMCO9.png)

6.19（考察多排序）
![](https://i.imgur.com/7AtBMhr.png)

6.20（考察多排序）
![](https://i.imgur.com/jOJ4DGH.png)

6.21
![](https://i.imgur.com/WL6SmKl.png)

6.22
![](https://i.imgur.com/ezJKQJ3.png)

6.23
![](https://i.imgur.com/bAn8Whb.png)

6.24
![](https://i.imgur.com/74GVw7q.png)

6.25
![](https://i.imgur.com/uCrxv1G.png)

7.--------
![](https://i.imgur.com/UVRcAyf.png)
![](https://i.imgur.com/iXAyJuZ.png)


7.1
![](https://i.imgur.com/y1ETMGC.png)

7.2
![](https://i.imgur.com/HJqpC2t.png)

7.3
![](https://i.imgur.com/AZPaXZU.png)

7.4（外连接）
![](https://i.imgur.com/xbW36Z8.png)

7.5
![](https://i.imgur.com/pGIUfsp.png)

7.6
![](https://i.imgur.com/vc9FT1Y.png)

7.7
![](https://i.imgur.com/iO1EV9F.png)

7.8
![](https://i.imgur.com/95DwkcQ.png)

7.9
![](https://i.imgur.com/bEROwvH.png)

7.10
![](https://i.imgur.com/3vaL7pe.png)

7.11
![](https://i.imgur.com/pr0fa95.png)

7.12
![](https://i.imgur.com/nHcCyyQ.png)

7.13
![](https://i.imgur.com/Fy6a3d9.png)

7.14
![](https://i.imgur.com/ZlfXGxJ.png)

7.15
![](https://i.imgur.com/S3UF63m.png)

7.16
![](https://i.imgur.com/B33tMZX.png)

8.--------

![](https://i.imgur.com/TCRnxS1.png)
![](https://i.imgur.com/7tfDeL5.png)


==创建数据库1~2==
![](https://i.imgur.com/zS6mGuh.png)

![](https://i.imgur.com/ccQpcY9.png)

![](https://i.imgur.com/oYZlncV.png)

![](https://i.imgur.com/QShCn8b.png)

![](https://i.imgur.com/H0vkEQW.png)

+++++++++++++++++++
==CURD3~4==

3.（查）
3.1
![](https://i.imgur.com/D6hjmGo.png)

3.2
![](https://i.imgur.com/75i1vwI.png)

3.3
![](https://i.imgur.com/K0x2zMH.png)

4.（改）
4.1
![](https://i.imgur.com/uMRJBhd.png)

4.2
![](https://i.imgur.com/eYsehrq.png)

![](https://i.imgur.com/ui1ly7d.png)

- [x] ==全部习题已自己动手做完！！！没有看答案！！！==