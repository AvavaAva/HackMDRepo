[![hackmd-github-sync-badge](https://hackmd.io/buZ1sbfwRYOOhBR2y7wcEQ/badge)](https://hackmd.io/buZ1sbfwRYOOhBR2y7wcEQ)
###### tags: `learn`

# Deep Learning & Machine Learning 
@q5nHNJguTPGM0TmdJtmRQw 


[TOC]


# 基础

### 网页爬取工具：selenium.webdriver

使用chrome等headless（无GUI）浏览器便于爬取，大致代码放在下面（谨慎使用）

源地址：[李沐-数据抓取](https://www.bilibili.com/video/BV1JM4y137kK?spm_id_from=333.999.0.0)

```
from selenium import webdriver

chrome_options = webdriver.ChromeOptions()
chrome_options.headless = True
chrome = webdriver.Chrome(chrome_options=chrome_options

page = chrome.get(url)  -----  输入需要的url
```

##### 数据标注

**半监督学习（Semi-supervised learning) SSL:**

若一部分有标签而另一部分没有，则是标准的半监督问题。
半监督学习的假设：
1. 连续性假设：特征相近的样本更有可能拥有相同标签；
2. 聚类假设：数据都是按类分的；数据若聚类为同一类，则有更高可能性拥有相同标签；
3. 流形假设：数据常常服从比特征更低维度的流形规则。


**常用算法：自学习（Self-training)**
![自学习](https://i.imgur.com/MUQM8lJ.png)

关键点：如何保证高置信度 --- 这个和自己选择的算法有关。当然，在训练的时候，不计成本使用昂贵模型也是没问题的。

## 数据工程

![清理步骤](https://i.imgur.com/Hyubasj.png)


#### 1. CSV文件处理

**1. 转换为压缩格式**
首先， pd.read_csv() 函数支持读取.zip, .rar文件， 并且，读取的速度实际上比读取 .csv更快。
所以， 建议使用 .csv文件时压缩！**（文本训练文件时可以压缩）**



---

**2. 转换为羽化格式（feather, ftr文件）**

相比于 .csv, .ftr的读取效率更高，但是占用内存会更多（经典空间换时间，不亏）

转换方式：
```
data = pd.read_csv('fr.rar')
data.to_feather('fr.ftr')

# 这样就将csv文件转换成了读入效率更高（但是更花内存）的羽化文件。

# .ftr的读取

fr = pd.read_feather(filename)
```


---
#### 2. 数据变换（归一化， 正规化， 取对数， etc...）

![数据变换方法](https://i.imgur.com/eFPwS1l.png)

* 注意， 第三个方法右边是让所以的x处于-1~1的区间（即 max(abs(x)) <= 1)

- 图片： 
    1.减小尺寸（Downsampling and cropping)
    2.Whitening

- 视频：
    1.片段切取（只取需要片段）
    2.采样关键帧（很费机器）

- 文本：
    1.语义化：如is, are, am ---- be， 只收集词义
    2.切分为需要部分
        - 需要部分为词： text.split(' ')
        - 需要部分为单字符：text.split('')

---

#### 3. 特征工程
![特征工程](https://i.imgur.com/bu3OFOW.png)

![文字特征](https://i.imgur.com/wHKPDKj.png)

#### 4. 总结
![总流程](https://i.imgur.com/Mugvd2g.png)

---


## 深度学习基础 --- 简单介绍

### 多层感知机MLP
神经网络：机器提取特征的能力通常比手动强的多！

![NN](https://i.imgur.com/P52vi9Q.png)

**1. Multilayer Perception (MLP) --- 多层感知机**

拥有一个 dense layer(全连接层) -- 计算 y = W@x + b (与softmax相似)

线性模型通式：

![线性模型](https://i.imgur.com/QkOYBrF.png)

**MLP**

使用sigmoid函数 与 ReLU（x），将多层全连接层进行非线性转化。

```
sigmoid(x) = 1 / (1 + exp(-x))

ReLU(x) = max(0, x)
```

---

**感知机**：
左为一层隐藏层， 右为多层隐藏层

（为什么叫隐藏层？ 因为我们从output角度来看， 它只看得到前一个全连接层，在这之前的层都看不到！）

![](https://i.imgur.com/45OHXCb.png)
![](https://i.imgur.com/W8wjUgB.png)

示例：单隐藏层MLP

![](https://i.imgur.com/VMYo5aF.png)

H：第一个隐藏层得出参数并使用ReLU函数激活后的结果
Y：最终输出

---
### 卷积神经网络CNN

**1. 卷积层**

Dense layer ---->  Convolution layer

***如何由全连接层过渡到卷积层***

全连接层：在进行图片处理等任务时，需要处理的参数太多不现实。

卷积层：处理图片时，显然我们只需要关注附近一块的内容，不需要考虑全图，因此我们只取像素点及附近一定范围的其他像素点计算；并且，各个部分的权重可以相等，而不是每个地方都要有不同权重。见下图。

![全连接层 vs 卷积层](https://i.imgur.com/1DyKToi.png)

***简单的卷积层代码***：

很有滑动窗口的味道

![卷积层代码](https://i.imgur.com/nd47kpk.png)


---

**2.汇聚层 Pooling layer**

考虑到***卷积层是对附近像素点进行计算，对位置极其敏感***！！！而我们实际上在拍摄照片时由于各种影响，很容易带来干扰（如手抖让某个物体显示变形），就引入了对这类问题鲁棒性更强的汇聚层Pooling layer。

汇聚层与卷积层一样，每次计算一个(*h * w*)窗口的像素点。
但是，与卷积不同，它计算的是该窗口的mean or max（平均值或最大值，对应叫平均汇聚与最大汇聚），这样它就对位置不这么敏感了！！

**为什么**？

- 因为它在对(*h * w*)窗口计算时取的平均或最大，那么实际上就等于允许了图片的相等尺寸的位移！！！！也就是说，在(*h * w*)大小内的误差是可以被捕捉住的！

汇聚层简单代码：
![汇聚层代码](https://i.imgur.com/ODylfaD.png)

---

**3. 卷积神经网络CNN**

CNN：堆砌卷积层的神经网络，抽取图片的空间信息，并将激活层用于卷积层之后

简单例子（简单结构与古老的lenet）：

![简单CNN](https://i.imgur.com/Ngr3mCI.png)
![lenet](https://i.imgur.com/FsDy3FU.png)

现在的CNN已经有更优化的结构，具体后面讲述：ResNet, VGG, AlexNet, MobileNet...)


---


### 循环神经网络RNN

**1. 发源**

对于现实世界在输入文字时进行接下来输入的预测，我们可以用MLP。也就是说，对于下一个会出现的词，我们做一个特别长的向量（使用独热编码）来做多分类，用全连接层与softmax进行预测并按概率高低推送下一个词。

这样确实可以解决问题，但如果需要我们预测下几个或者说多次预测呢？这时候我们可以把之前输入的内容一起考虑，再来进行预测。这就是RNN的初始模型。

![1.png](https://i.loli.net/2021/11/01/hzZ4angyeHqESjT.png)

![RNN](https://i.imgur.com/LTmmnPP.png)

![Simple example](https://i.imgur.com/HcYfe71.png)

![CODE](https://i.imgur.com/FMrin5j.png)

**双向RNN：**

![双向RNN](https://i.imgur.com/D7HEhl8.png)

**多层RNN：**

![DEEP RNN](https://i.imgur.com/NVyGGKi.png)

**MODEL SELECTION**

![](https://i.imgur.com/2nUo0Wx.png)


---
## 插入 --- 机器学习/深度学习常见问题

模型评价：先是熟悉的P,R,F（没什么问题）

![](https://i.imgur.com/2L4tMjF.png)

---

**再是AUC与ROC！
（x：负类被预测成正类的比例（1-负召回）， y：正类被正类预测的比例（正召回））**
![](https://i.imgur.com/EM7V91h.png)
![](https://i.imgur.com/ayfqu01.png)


过拟合：训练误差低，泛化误差高（低误差高方差）
欠拟合：训练误差与泛化误差高（高误差方差）

![](https://i.imgur.com/VwFFLbj.png)

![](https://i.imgur.com/UPsgHi8.png)


#### Huber Loss

预测与真实值差的小时采用MSE, 差得远则采用绝对损失的损失函数。可以保证优化的平滑！！！

![](https://i.imgur.com/C2sXEvI.png)


---

####  权重衰退

处理过拟合。（正则？）

**1. L2范数**

![](https://i.imgur.com/Ogcow4a.png)
![](https://i.imgur.com/PVrohYW.png)
![](https://i.imgur.com/9e4zoxP.png)
![](https://i.imgur.com/5Gho18e.png)


**2， L1范数**

与L2不同，L1是绝对值形式，图像上来看是以原点为圆心的方形，且数据会服从拉普拉斯分布，有很好的特征筛选作用。

#### 丢弃法 Dropout

一个好的模型应该对数据输入有较高的扰动鲁棒性。

丢弃法：在层之间加入噪音！！！（也算是一种正则）
- 但是要确保期望不变！！！


![](https://i.imgur.com/1FeqpU6.png)

![](https://i.imgur.com/dDlLCjs.png)


> 
> **为什么推理（输出）中的dropout直接返回输入？（面试常问）**
> 
> 推理：输出层。
> 
> dropout作为正则项，作用是控制模型复杂度；而在做推理的时候（也就是输出时），我们不会进行模型复杂度的更新！！！毕竟输出就是根据前面的模型给出一个最后的预测结果罢了。所以dropout并不需要加在推理中。
> 
> 当然，我们也可以在推理使用，但考虑到dropout的随机丢弃，若要使用在输出那么我们需要多次进行推理并取其平均或众数，不然很有可能我单次为1的结果被丢弃而变成0！！！

> **dropout和权重衰退相比有什么不同？**
> 
> 1. dropout仅用于全连接层。
> 2. dropout易于调参，而权重衰退的超参数计算较为麻烦。
> 3. dropout最直观！
> 4. dropout有可能使收敛变慢 -- 随机性。


---

#### 数值稳定性

![](https://i.imgur.com/skhctja.png)

---

#### **梯度爆炸 & 梯度消失**

**梯度爆炸** --- ReLU常见

![](https://i.imgur.com/6nfQfkL.png)
![](https://i.imgur.com/gAg37Bo.png)
![](https://i.imgur.com/WItE3pD.png)

---

**梯度消失** --- sigmoid常见
![](https://i.imgur.com/JT73BgA.png)
![](https://i.imgur.com/IysNyif.png)
![](https://i.imgur.com/d7T4Drw.png)


**如何解决？？**

1. 将每次的层间乘法变加法：
    - ResNet， LSTM

2. 归一化：
    - 梯度归一化
    - 梯度裁剪

3. 合理的权重初始与激活函数

**3. 合理的权重初始与激活函数**

- 让每层的方差和均值都保持一致！
    - 将每层的输出与梯度都看作随机变量；
    - 让其均值方差都保持一致。

- 权重初始化
    - 在合理区间里随机选择初始参数；
    - （0， 0.01）的正态分布可以用于小网络，但是对于深度神经网络来说不是一直都可行的。

![](https://i.imgur.com/SAPpTlQ.png)
![](https://i.imgur.com/n5Els81.png)
![](https://i.imgur.com/Dp3lGtM.png)

**Xavier初始**
![](https://i.imgur.com/tp7jYTW.png)


**调整sigmoid激活函数**

调整使其与其它几个激活函数有类似性质。

![](https://i.imgur.com/ptOryTa.png)


## torch的使用！

![](https://i.imgur.com/aeRyVMT.png)
![](https://i.imgur.com/BiCeCZW.png)
![](https://i.imgur.com/IuJiXdr.png)
![](https://i.imgur.com/ZZ9FjWk.png)
![](https://i.imgur.com/cuNDF4H.png)
![](https://i.imgur.com/sTmtCiT.png)
![](https://i.imgur.com/euCEAHh.png)



---

**计算梯度**
![](https://i.imgur.com/Qt4QQzu.png)


---

**数据读取**
![](https://i.imgur.com/iyMeM0T.png)
![](https://i.imgur.com/QcQcvq7.png)

---

**建立网络**
![](https://i.imgur.com/iUmnkHQ.png)
![](https://i.imgur.com/BOLevNi.png)
![](https://i.imgur.com/DYbqffx.png)
![](https://i.imgur.com/DR550Hh.png)
![](https://i.imgur.com/o31rrer.png)

---

**更新模型optim**
![](https://i.imgur.com/8qkZCIK.png)

---

**训练train**
![](https://i.imgur.com/VZOplVy.png)
![](https://i.imgur.com/Batssxy.png)
![](https://i.imgur.com/6nBXTG3.png)

**test**
![](https://i.imgur.com/XnJkgvD.png)

---

**保存save**
![](https://i.imgur.com/zTd9GGk.png)



---


# 卷积神经网络
## 卷积神经网络CNN基础 -- 卷积层 & 池化层
### 1. 什么是卷积

发源：由MLP解决图片的识别问题，会产生特征量过多的问题！！！

![](https://i.imgur.com/U5WpZrK.png)

在上面的例子里，单层的MLP就需要14G的内存去存储所有的特征，很显然不现实！！！

**实际上**，考虑到图片的识别问题，应该满足：
- 平移不变性：这个物体不管存在于图片的哪里，它本身的存在是不会变的，只是再图片的位置进行了平移；**也就是说，我们在进行卷积的时候，对于每一次步骤，使用的卷积核是不变的！**
- 局部性：识别某个物体只需要看图片的局部就可以。**也就是说，卷积无需选择全图，只需要选择某个大小的窗口（卷积核的尺寸）即可！**

因此，我们对全连接层进行考察：

 **一**
 ![](https://i.imgur.com/Bpnrkj2.png)

**注意点**：
> 
> 1. 为什么说权重是4D的？
>  
>     - ans：我们的全连接层就是将输入的特征（1维，复数个）经过Linear变成另一个大小的1维特征，也就是2维（2D）的；**但是**！进行卷积的时候，我们是只取一部分的数据进行卷积，这时候我们取出来的数据实际上可以看作是有***高和宽的矩阵***，是二维（2D）的；同时输出是另一个有有***高和宽的矩阵***，也是二维（2D）的。所以，我们的卷积层必定是一个四维（4D）的！！！

 **二**
 ![](https://i.imgur.com/xV2QJFb.png)

>- 对应的平移不变性：这个物体不管存在于图片的哪里，它本身的存在是不会变的，只是再图片的位置进行了平移；也就是说，我们在进行卷积的时候，对于每一次步骤，使用的卷积核是不变的！

 **三**
 ![](https://i.imgur.com/h4QKowg.png)
>- 对应了局部性：识别某个物体只需要看图片的局部就可以。也就是说，卷积无需选择全图，只需要选择某个大小的窗口（卷积核的尺寸）即可！

---

### 2. 卷积层ConvolutionLayer

二位交叉（卷积基础）：

![](https://i.imgur.com/yCST3dd.png)

就很像一个滑动窗口，一直在图片上滑动直到遍历完所有单元！

![](https://i.imgur.com/bayX5fr.png)

![](https://i.imgur.com/UUTu9Bc.png)

![](https://i.imgur.com/7jBYsoy.png)


**注意**， 卷积层的超参数不是权重W与偏差b， 而是我们选择的卷积层窗口大小！！！！！

##### 所有代码都在Jupyter有。

---

### 3. 卷积层的填充与步幅

填充与步幅：控制卷积层大小的超参数！

#### 3.1 填充

![](https://i.imgur.com/eqB70qk.png)

**如果没有填充，那么几次的卷积之后，整个图像尺寸就变得特别小，不能进行深度的学习！！**

![](https://i.imgur.com/729J85P.png)
![](https://i.imgur.com/nTzIXOh.png)

为什么通常取这个值？？？因为，***取这个值可以让卷积后的尺寸与卷积前相同***！！

-----------

#### 3.2 步幅

给定一个尺寸卷积核，若图片尺寸特别大二卷积核小，就需要很多次才可以把图片卷积到小尺寸！

这样就需要大量计算！！

![](https://i.imgur.com/XdBxUmA.png)

#### 3.3 总结

![](https://i.imgur.com/5eHBzqG.png)


> 卷积层的超参数：
> 
> 1. 核大小kernel_size：最重要，确定了卷积的大小！
> 2. 填充padding:不是特别重要，一般取默认值。
> 3. 步幅stride:取决于想要控制的模型复杂度！！！
> 4. 输出通道：很重要，后面介绍！
> 
> 代码实现同样地，在JupyterMd!


### 4. 输入输出通道

#### 4.1 输入通道

彩色图像可能会有RGB三个通道，若转换为灰度会丢失信息！

![](https://i.imgur.com/5Jl403Q.png)

![](https://i.imgur.com/ZH5N4Ui.png)

- **不同的通道与对应的不同卷积核进行操作！！！操作后再进行相加！！！**

![](https://i.imgur.com/wnOx1bP.png)

#### 4.2 输出通道

![](https://i.imgur.com/zTtBKt3.png)

> 我们可以认为：
> - 每一个输出通道都是在识别不同的特定模式！
> - 每一个输入通道核都是在识别并组合输入中的模式！

---

### 5. （1 * 1） 卷积层

![](https://i.imgur.com/OKx4AxB.png)

> 我的理解：
> 
> 1*1 卷积层并不会进行识别，但是考虑到多通道的运算（见上面），如果是1 * 1 的层进行相加，就相当于直接把多层的信息进行了相加！！！！！也就是说，将图片的多个输入通道进行了融合！！！



---

### 6. 二维卷积层复杂度

![](https://i.imgur.com/OAnpxIO.png)

> - ci : 输入通道
> - co : 输出通道
> - nh, nw: 图片尺寸（高，宽）
> - kh, kw: 卷积核大小
> - mh, mw: 输出尺寸（高，宽）

![](https://i.imgur.com/x4ChoCr.png)

代码实现在jupyterMd.

---

### 池化层PoolingLayer

卷积层对位置及其敏感！

![](https://i.imgur.com/D4yqWd6.png)

而我们在拍摄时常因为抖动，使图片有一定变化！

 - 我们需要一定程度的平移不变性！！！ - 池化层解决

#### 最大池化（MaxPooling）

仅返回每次窗口中的最大值！！（和卷积很像，但不需计算输出！）

![](https://i.imgur.com/mkoPqeH.png)


比如，拿垂直边缘检测为例：

![](https://i.imgur.com/Z06FY78.png)

#### 超参数：填充，步幅与多通道

![](https://i.imgur.com/A8TAi2F.png)

池化不做多通道融合，因为一般留给卷积做！ 

#### 平均池化（AvgPooling）

![](https://i.imgur.com/KQOe6RD.png)

> * 注意，池化层是没有需要学习的参数的！！！！因为本质上来说它只是取最大或平均，并不进行其他的计算！！！ 
> * 池化不做多通道融合，因为一般留给卷积做！ 

> - 为什么现在池化层用的越来越少了？
    > - 因为：
    >     1. 卷积层和步幅一起使用实际上能达到一定的降低位置敏感度效果；
    >     2. 现在我们常在使用学习前进行数据增强，池化的作用也就随之变小了。


---

## LeNet

![](https://i.imgur.com/Bsb9BX9.png)
![](https://i.imgur.com/cqMXt6Z.png)


> 结构：
> 1. 卷积（核5 x 5， 输出通道6）
> 2. 池化（核2 x 2， 输出通道6）
> 3. 卷积（核5 x 5， 输出通道16）
> 4. 池化（核2 x 2， 输出通道16）
> 5. 全连接（输出120）
> 6. 全连接（输出64）
> 7. Gauss（现在很少用了，可以看作全连接加gauss分类，得到十分类概率）
> 
> 先使用卷积层学习图片空间信息，再使用全连接层转换到类别空间！

### 结构代码

![](https://i.imgur.com/H5WDTcE.png)

训练部分见Jupyter。


---


## AlexNet

![](https://i.imgur.com/k51HzsH.png)
![](https://i.imgur.com/vViomM6.png)
![](https://i.imgur.com/4g8k9J9.png)
![](https://i.imgur.com/ikJ9w2M.png)
![](https://i.imgur.com/TZRuVhd.png)
![](https://i.imgur.com/VTuc3ir.png)


### 代码构架

与LeNet差别不大！
![](https://i.imgur.com/WbvMgea.png)


---


## VGG-使用块的网络

AlexNet的问题：网络形状不规则（虽然和LeNet很像，但它长得很随意），不便于进行加深/加宽！

![](https://i.imgur.com/CkIbKv9.png)

![](https://i.imgur.com/FCGb60A.png)

> 问题：
> 1. VGG块的超参数有哪些？
>     - 有： 1. 卷积层层数n； 2. 输出通道数m
> 
> 2. VGG块为什么使用3 * 3卷积核而不是大一些的？
>     - VGG曾经比较过3 * 3 和 5 * 5， 发现比起更大的核与较浅的深度，小的核与更深的深度有更好的效果！所以最后使用的3 * 3.

**VGG架构：串联多个块**

![](https://i.imgur.com/LWSkzJs.png)
![](https://i.imgur.com/RLu1ToK.png)
![](https://i.imgur.com/OxTtLnW.png)


### 架构代码
![](https://i.imgur.com/iggyeVt.png)




---


## NiN - 网络中的网络（Network in Network）

![](https://i.imgur.com/FbQ1Cu2.png)

> NiN思想：抛弃全连接层，使用1 * 1卷积层替代！（减少学习的参数）
> 
![](https://i.imgur.com/4pHVv3J.png)
![](https://i.imgur.com/ov5ldmL.png)
![](https://i.imgur.com/aIbG5JO.png)
![](https://i.imgur.com/eVeuTgc.png)


### 构架代码

![](https://i.imgur.com/QMcH9pu.png)


---


## 含并行连结的网络 GoogLeNet / Inception V3

![](https://i.imgur.com/DGoivoj.png)

**思路：选个p，我全都要！**

![](https://i.imgur.com/ovB2uDF.png)

![](https://i.imgur.com/hRV525B.png)

![](https://i.imgur.com/UJQNH1K.png)

### 网络构架

![](https://i.imgur.com/upDirkn.png)

![](https://i.imgur.com/MMiIxgK.png)

![](https://i.imgur.com/evwVCpi.png)

![](https://i.imgur.com/3qYRSp6.png)

![](https://i.imgur.com/aaciDzf.png)

### 总结

![](https://i.imgur.com/TFUn02b.png)

### 代码架构

#### inception塊

![](https://i.imgur.com/4UFe9DY.png)

#### stage段

![](https://i.imgur.com/tT4Ps8J.png)


---


## 批量归一化

![](https://i.imgur.com/ASG8GMF.png)

> 个人解释：
    >   利用forward计算，我们是从底部（也就是数据输入）开始，一层层往顶部（输出）运算。而，损失函数是出现在最后的，也就导致了底部运算较慢，而顶部运算较快（收敛的快）。
    >   那么如果，我们对底部层进行一些改变，相应地顶部层也需要变化，导致最后的层我们需要学习很多次！收敛也就相应变慢！
    >   为了防止这种现象，批量归一化在2016年被提出。

![](https://i.imgur.com/E3i1YWE.png)

![](https://i.imgur.com/Nv7Zxzk.png)

![](https://i.imgur.com/7Ow98Xt.png)

![](https://i.imgur.com/hD4uHtF.png)


### 代码实现

![](https://i.imgur.com/JwYWU5u.png)

![](https://i.imgur.com/vi1xXSc.png)


### 库内调用

![](https://i.imgur.com/kasFxM6.png)


---


## ResNet

**老师原话：假设你要在卷积神经网络里只了解一个，那么选择ResNet就好！**

残差网络ResNet：听名字很像GBDT。

思想：很多时候，加更多的层并不一定能够改进精度！

![](https://i.imgur.com/poPHbrv.png)

### 残差块

![](https://i.imgur.com/lz9P40D.png)
> 约等于在后面的网络里嵌入了之前学习到的小网络，让网络越变越宽，学到的东西也就多！

![](https://i.imgur.com/zRdKSGG.png)
> 其余也有许多不同的残差块！

### ResNet架构

![](https://i.imgur.com/ejITnIf.png)

![](https://i.imgur.com/TCEYkj6.png)

![](https://i.imgur.com/fycevHG.png)


### 代码实现

![](https://i.imgur.com/4a3OgSE.png)

![](https://i.imgur.com/5sHqA4Y.png)


### ResNet如何处理梯度消失？

![](https://i.imgur.com/Uzbltdi.png)

> 上面紫色的是其他的网络，每一次的梯度都是在之前网络基础上乘以当前网络，若当前网络的梯度值较小，则容易梯度消失（大数乘小数为小数），这也解释了为什么sigmoid容易梯度消失(因为都给你换到0~1区间去了！)
> 而绿色的是ResNet，它的梯度计算不同，有加法模型的感觉，就算底层每次学习到的梯度特别小，大数加小数为大数，也不会让梯度消失！



# 计算机视觉相关（应用1的图片分类已在CNN完成）
## 数据增广

![](https://i.imgur.com/ienn50y.png)

![](https://i.imgur.com/XsIUMVi.png)

![](https://i.imgur.com/d8Bzhgp.png)

> 实际应用时应考虑到测试集内容进行部署！！！（也就是，只需要训练出来够我测试集可能出现的特殊情况的识别能力就可以了，不需要啥稀奇古怪的都给整上）

---

### 代码实现

各种增广实现：

![](https://i.imgur.com/KLqrTbp.png)

![](https://i.imgur.com/QjFb29H.png)

![](https://i.imgur.com/lh7XuF1.png)

![](https://i.imgur.com/ZvOsoh0.png)

![](https://i.imgur.com/6rP9NVG.png)

![](https://i.imgur.com/CGG3m4D.png)

![](https://i.imgur.com/VuRuU8r.png)


---


## 微调 ---- 迁移学习

**计算机视觉相关最重要的技术！**

![](https://i.imgur.com/DB9oY6U.png)

![](https://i.imgur.com/BoqdVvf.png)

![](https://i.imgur.com/DPtQIPs.png)

![](https://i.imgur.com/Zqf5Zb2.png)

![](https://i.imgur.com/HbZmYJc.png)

![](https://i.imgur.com/OK5IcUF.png)

> 建议：以后都从微调（也就是预训练过的模型）开始，而不是从头开始优化！
> 
> 迁移学习将从源数据集中学到的知识“迁移”到目标数据集，微调是迁移学习的常见技巧。
> 
> 除输出层外，目标模型从源模型中复制所有模型设计及其参数，并根据目标数据集对这些参数进行微调。但是，目标模型的输出层需要从头开始训练。
> 
> 通常，微调参数使用较小的学习率，而从头开始训练输出层可以使用更大的学习率。


### 代码实现

见jupyter。


---

## 应用2：物体检测

### 边缘框与实现

![](https://i.imgur.com/eCw1Q21.png)

![](https://i.imgur.com/YdNv2Pv.png)

![](https://i.imgur.com/LjbOrRP.png)

![](https://i.imgur.com/0mHnFfV.png)


---


### 锚框 --- 太难了，根本没看懂

![](https://i.imgur.com/cE61FIH.png)

![](https://i.imgur.com/IJt5pDH.png)

![](https://i.imgur.com/yQ2bv3v.png)

![](https://i.imgur.com/oJsp7E9.png)


### 物体检测算法

#### R-CNN

> 应用场景：极度关心预测精度时使用（特别耗计算资源）

![](https://i.imgur.com/lgNcFOK.png)

![](https://i.imgur.com/iKqe4xw.png)

![](https://i.imgur.com/p5uM6WV.png)

![](https://i.imgur.com/1Y0v3PI.png)

![](https://i.imgur.com/tKzPWpE.png)

![](https://i.imgur.com/TRZX1Nq.png)


#### 单发多框检测SSD

> 速度快但是精度不咋地

![](https://i.imgur.com/ijRfvNS.png)

![](https://i.imgur.com/jyMYfEW.png)


#### YOLO：You Only Look One

![](https://i.imgur.com/PtNXnSG.png)

![](https://i.imgur.com/GxoGkig.png)


---


## 应用3：语义分割

![](https://i.imgur.com/uaV7IBM.png)

应用：

1. 背景虚化

2. 路面分割（应用于自动驾驶）

### 转置卷积

![](https://i.imgur.com/Dr7Vka1.png)

![](https://i.imgur.com/dZiXyH1.png)

![](https://i.imgur.com/pzZXuZr.png)

实际上在模块中也有！简单调用

![](https://i.imgur.com/lmGsT1W.png)

> 转置卷积是把卷积还原吗？如果是的话，那么我一开始不卷积不就好了？？？
> - 并不是。转置卷积在超参数完全一致的情况下确实可以还原卷积；但我们利用转置卷积是为了让图片的尺寸增大！！！而不是还原卷积！

![](https://i.imgur.com/3OP7KPx.png)

> 上采样：使输出的形状/尺寸变大！！！

**转置卷积过程详解**：

![](https://i.imgur.com/0eM1gFA.png)

![](https://i.imgur.com/ELZgL7M.png)

![](https://i.imgur.com/M8syoh8.png)


---


## 全连接卷积神经网络FCN(Fully connect network)

![](https://i.imgur.com/7W4SkTX.png)

### 模型构造

![](https://i.imgur.com/nfeorG2.png)



---

## 应用4：样式迁移（最后一个！）

![](https://i.imgur.com/g9lCGfk.png)


我们学习基于CNN的样式迁移！

![](https://i.imgur.com/WwZR0xY.png)

> 思想：对于左右提供内容与风格图片以及中间的生成图片，三者是在同一CNN的。其中，负责内容的图片在某一个卷积层可以和生成图片相匹配，负责风格的图片也可以在某一卷积层与生成图片匹配。
> 接下来，我们通过正向传播（实线箭头方向）计算样式迁移的损失函数，并通过反向传播（虚线箭头方向）迭代模型参数，即不断更新合成图像。 样式迁移常用的损失函数由3部分组成： (i) 内容损失使合成图像与内容图像在内容特征上接近； (ii) 样式损失使合成图像与样式图像在样式特征上接近； (iii) 总变差损失则有助于减少合成图像中的噪点。 最后，当模型训练结束时，我们输出样式迁移的模型参数，即得到最终的合成图像。
> 我们训练的东西不是每一层的权重，而是我们的生成图片！！！！！！

### 代码实现

很长，但是并不是很难的代码，我认为很有意思，啥时候都可以回头看看！ ----  CycleGAN！！
    
---


# 序列模型与循环神经网络RNN

## 序列模型

针对有时序结构的数据，我们需要不一样的评价/决策手段（如股价等，不能打乱数据时序进行分析！）

![](https://i.imgur.com/flj0pP6.png)

![](https://i.imgur.com/O7bLcf2.png)

![](https://i.imgur.com/akgLk8J.png)

![](https://i.imgur.com/n4CrLjl.png)
> 这就是马尔可夫模型的起点，假设当前结果与前n个结果相关（也是N-gram问题！）

![](https://i.imgur.com/FFYjGnV.png)
> RNN的思维起点，在预测新的一轮时，会同时将上层及之前预测结果也代入考虑。

![](https://i.imgur.com/jvMj49h.png)


---

## 文本预处理（Funny） 

将文本转换成词元！！看Jupyter已经写好了！
- 若想进行中文的预处理，推荐使用jieba库！（没错，就叫这个！）

## 语言模型

![](https://i.imgur.com/1jTPq92.png)


![](https://i.imgur.com/NcWKryS.png)

> 其实就是条件概率公式！

**多元语法N-gram：τ的不同！**

![](https://i.imgur.com/1jXI30I.png)

> 二元，三元语法用的蛮多的！因为可以减少时间开销！（空间换时间，储存所有二元或多元的条件概率即可！）

![](https://i.imgur.com/ASR1Dll.png)

### 代码见Jupyter


---

## 循环神经网络RNN

![](https://i.imgur.com/0nMVz3o.png)
![](https://i.imgur.com/mB2uGQt.png)

> 首先，上图的公式错了！！！和图一起！！！应该是 Whx * Xt (下面有很多)
> 简单地说，我们利用xt训练隐变量ht, 再利用ht去预测ot！（ht从ht-1来（通过whh也就是箭头！！！），且使用xt训练）
> 然后得到的ot使用xt+1去计算损失！！！又可以更新ht+1！注意预测ot的时候是不需要激活函数的！！！！！

![](https://i.imgur.com/SK33uG7.png)

![](https://i.imgur.com/bWZ67OZ.png)

![](https://i.imgur.com/WV9kOQR.png)

![](https://i.imgur.com/zBc1XA8.png)


### 代码见Jupyter



---

## 门控循环单元GRU

![](https://i.imgur.com/QqSTg0V.png)

![](https://i.imgur.com/b0Gvd3x.png)

![](https://i.imgur.com/xy7HTtz.png)

![](https://i.imgur.com/M7apRus.png)

![](https://i.imgur.com/w5mSCIy.png)
> 理解：Rt作为重置门， 在公式内影响到候选隐状态的值。而在GRU中，隐状态取决于候选隐状态与前一次的隐状态，决定这个比例的是我们的更新门Zt。
> Rt与Zt都是与隐状态Ht同形状的，也就是说计算量会变为原先的三倍！


---


## 长短期记忆网络LSTM

![](https://i.imgur.com/h6UrMPS.png)
> 比GRU多个门！！！

![](https://i.imgur.com/3yg1iKq.png)

![](https://i.imgur.com/omgrqjN.png)

![](https://i.imgur.com/FCKMuLA.png)

![](https://i.imgur.com/Di4SNO6.png)

![](https://i.imgur.com/p93DaJu.png)

> 我的理解：
> 首先，与GRU类似采用了记忆这一概念（当然，GRU就是从LSTM来的！所以也简短些）。
> 且三个门（F, I, O）与候选记忆单元（Ct_hat)的计算也是与GRU的R, Z完全一样！
> 由总结的公式可以看出来三个门与记忆单元的作用：
> 1. Input gate: 选择让记忆单元（Ct）记住候选记忆单元（Ct_hat)的多少；
> 2. Forget gate: 选择让记忆单元（Ct）忘记上一个记忆单元（Ct-1）的多少；
> 3. Output gate: 选择让最后的隐状态Ht记忆住激活后的记忆单元的多少！
> 4. Ct： 记忆单元，记住一定的模式，每次的更新都会通过I, F 选择记忆候选值与旧值的比例，然后通过tanh归一化再点乘O 成为新一轮的隐状态。


---

## 深度循环神经网络Deep RNN

我们前面讲的都是很浅的网络！！！！这里开始要深！毕竟是深度学习嘛！

![](https://i.imgur.com/by8067t.png)

![](https://i.imgur.com/bCdkfkT.png)

![](https://i.imgur.com/ADjksAg.png)
> 简单，直接堆隐状态层就是，只需要最后的参数num_layers改一改，见Jupyter。


---

## 双向循环神经网络

![](https://i.imgur.com/GEkl4pj.png)

![](https://i.imgur.com/P7XjldO.png)

![](https://i.imgur.com/IzOj4g2.png)

![](https://i.imgur.com/3AsxymZ.png)


![](https://i.imgur.com/Y2yreQD.png)

> 实现更加简单，参数 bidirectional=True 就完成！
> 注意！！！ 双向循环神经网络不能用于预测未来！！！！！也就是说它无法做文本生成类的预测模型！


---

## 编码器/解码器

![](https://i.imgur.com/znMOU9p.png)

![](https://i.imgur.com/FprQBRB.png)

![](https://i.imgur.com/GBIAB0l.png)
> 编码：对原始数据进行特征抽取！
> 解码：将特征编码后的中间表示变成输出！
> 差异：Encoder不需要做预测，可以看到整个特征！而Decoder通常是做预测的，不能够看到全部！
> 所以，双向RNN可以用在Encoder，但由于其不能看到未来，不能用在Decoder！！



---

## 序列到序列学习（seq2seq）

![](https://i.imgur.com/YY7faby.png)
> 由于双向RNN特性，它常被应用于编码器！！！

![](https://i.imgur.com/5izraBX.png)
> 其实就是，将编码器训练出来的最后的隐状态用作解码器的初始隐状态！！！这样就记忆住了许多特性！

![](https://i.imgur.com/Uw11tQ9.png)

![](https://i.imgur.com/48Y2TVy.png)
> 这个BLEU实际上很好理解，就是：
> 1. min后面的部分表示，如果我预测的长度和实际输入长度差很多，那么预测肯定是有问题的！这个时候我的min就会让后面一坨为负数，而exp负数肯定是个极小的数！就说明效果不行。
> 2. 后面的pn的1/2的n次方表示的是：对于n元语法（pn）我们的权重就更高！参照上面，也就是我们对n-gram中多元给予高权重，而仅仅预测一个或者连续两个词的单元或二元语法给予低权重！
> 实际上的实现我会在后面贴上代码！

![](https://i.imgur.com/D3vkGUz.png)


---


## 束搜索Beam Search

![](https://i.imgur.com/R10TYzW.png)

![](https://i.imgur.com/7tP6uKz.png)

> 贪心最快， 穷举效果最好，但是二者都过于极端！那么能否有折中效果的方法呢？有的！

![](https://i.imgur.com/JjDXxFz.png)

![](https://i.imgur.com/qaVUS18.png)
> 理解：保存前k大的贪心？！！！？
> 下面公式什么意思？？？
> L：长度！！！！！如果不设置长度的话，那我束搜索为了保证乘积最大肯定搜索路径很短！（毕竟都是softmax回归值小于1， 那肯定少乘几次会更大！）

![](https://i.imgur.com/vwNdhec.png)


---


# 注意力机制

## 注意力机制Attention - 60年代

![](https://i.imgur.com/nsNdA9T.png)

![](https://i.imgur.com/uYDW2W1.png)
> 随意：跟随意念。比如上面的图，我喝了咖啡有了精神，就可能顺着想看看书，这就叫随意。

![](https://i.imgur.com/9cQfsmR.png)

> 非参：指这个层没有任何需要学习的参数！仔细看上面公式确实，所有的参数都是输入给出的。

![](https://i.imgur.com/aXhk1P2.png)

> 也就是说，对于非参注意力池化层，若选用高斯核，则相当于我们对数据进行softmax后变为权重套用在yi上！！！

![](https://i.imgur.com/iK13X8A.png)

![](https://i.imgur.com/qgotnKs.png)


---


## 注意力分数

![](https://i.imgur.com/nE7Jvpf.png)

![](https://i.imgur.com/BS0DquY.png)

> 问题：如何设计a？？？？？

**思路1：可加性注意力**

![](https://i.imgur.com/YgaW3jT.png)
> 可用在query和key不同长度情况！！！
> 上面写错了，应该是将key和query使用对应的Wk Wq相乘，使其变为长度位h的向量后进行tanh归一化后乘以v的转置。


---

**思路2：点积注意力机制**
![](https://i.imgur.com/Xo20wiC.png)


![](https://i.imgur.com/HdsVSAK.png)


### 代码实现！！见Jupyter


---


## 使用注意力机制的Seq2Seq

> 看完上一章，你肯定会问：query， key， value分别是什么？它们的维度和长度为什么是这样？下面我们用使用注意力机制的Seq2Seq来进行实例分析。

![](https://i.imgur.com/zIsS1fu.png)

![](https://i.imgur.com/huKbPCO.png)
> 根据上一个词预测的结果在attention中找寻键值对，一同预测下一个词！

![](https://i.imgur.com/bYVAKA3.png)


---


## 自注意力和位置编码

教你怎么选key, value和query！

![](https://i.imgur.com/8SdtNKr.png)

> 公式说明：
    > 对于 yi 而言，query是当前的xi， 而key， value就是x1到xn（键值相同）

![](https://i.imgur.com/Mpat3Co.png)

![](https://i.imgur.com/hAvZ8ld.png)

> 为什么自注意力没有记录位置信息？
> 看上面的结构图就可以知道，自注意力相当于是把q,k,v进行了一次全连接层运算！当然没有相互的位置关系啦。
> 所以我们加入位置编码（加在输入！而不是模型）来使其具有位置信息。

![](https://i.imgur.com/YLGAmXn.png)

![](https://i.imgur.com/Y5YWJ78.png)

> 位置编码：
> 1. 绝对位置信息：由上面的图可见，不同列的同一位置都被赋予了不同的值，这个值是唯一可辨的；
> 2. 相对位置信息：i+σ位置处的信息可由i处的位置信息通过矩阵乘法得到！（就很灵活）

![](https://i.imgur.com/6WRgVF9.png)



---


## Transformer 

> 说实话上一节我有一点半懂不懂的...只是知道是个什么东西，怎么表示，位置编码的代码也看得懂， 但是！！！ 关于矩阵维度这些，各个维度的意思我还是不甚了解。

### 架构 -- transformer块
![](https://i.imgur.com/7F9Wr4y.png)

![](https://i.imgur.com/tS04QJA.png)

![](https://i.imgur.com/85h6dtJ.png)

![](https://i.imgur.com/8PI03RF.png)
> 就是说，我们进行输出的时候，虽然知道整个序列，但对于我们输出元素后面的元素，我们应该假装不知道（不然就不实际了）所以用掩码掩埋（就是前面的mask函数， 把后面的都变成0之类的）

![](https://i.imgur.com/LDzaQfQ.png)

> 在经过多头注意力后我们的输出为（b, n, d)也就是batch_size, 序列长度n， 维度d。但是这样不行，因为我们的n会变！！！

![](https://i.imgur.com/LX4BoXq.png)
> - add: 等同于ResNet的残差块，这样可以把网络做深；
> - norm： layer norm 归一化（为什么不是batch norm？）
> 因为：我们每个句子的长度可能不同！以批量为单位的归一化不稳定！
> 换个理解就是：
    > - batch norm是把一批量句子中的第几个词进行归一化；
    > - group norm是把一个句子的所有词同时归一化！

> b:bath_size, d:dimension(特征维度）, len:序列长度

![](https://i.imgur.com/TmUokpT.png)

![](https://i.imgur.com/gaqUxAf.png)

![](https://i.imgur.com/8j4EfCq.png)

> JUPYTER 写起来看上去超参数很多，实际上只有2个：
    > - num_head: 多头注意力的头数
    > - num_hiddens: 隐藏层数

### 其他人的Transformer讲解

[手推Transformer](https://www.bilibili.com/video/BV1UL411g7aX/?spm_id_from=333.788.recommend_more_video.2)


---


## BERT

![](https://i.imgur.com/QnEU20h.png)

![](https://i.imgur.com/nGEIJgl.png)

![](https://i.imgur.com/xjbMf4P.png)

![](https://i.imgur.com/6Bqz7PU.png)

![](https://i.imgur.com/9c1883r.png)

![](https://i.imgur.com/h9Gk41n.png)

![](https://i.imgur.com/3Hdfcky.png)

### 实现在Jupyter都有！相对而言不是那么难

### BERT 微调

![](https://i.imgur.com/kVSPTzz.png)

![](https://i.imgur.com/VKSgs8B.png)

![](https://i.imgur.com/difY07s.png)

![](https://i.imgur.com/RqBEXTs.png)

![](https://i.imgur.com/msPqkIO.png)


---


# 优化算法

## 其他常见

![](https://i.imgur.com/1c7Meb0.png)

![](https://i.imgur.com/EfHH1i1.png)

![](https://i.imgur.com/7VlUxb5.png)

![](https://i.imgur.com/7cwD5xS.png)

![](https://i.imgur.com/27YkqcN.png)

![](https://i.imgur.com/p860SOG.png)

![](https://i.imgur.com/EUKNC6p.png)

![](https://i.imgur.com/bSCmxMr.png)

![](https://i.imgur.com/DL8ij0S.png)



---


## Adam -- 对学习率不敏感，平滑

![](https://i.imgur.com/eaevrLm.png)

![](https://i.imgur.com/4KDHqVb.png)

![](https://i.imgur.com/u5OdWHH.png)

