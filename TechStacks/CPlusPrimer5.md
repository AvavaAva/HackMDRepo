[![hackmd-github-sync-badge](https://hackmd.io/iA_StC3cQLawnDnnrJ7P4g/badge)](https://hackmd.io/iA_StC3cQLawnDnnrJ7P4g)
###### tags: `learn`

# C++Primer5 个人memo

## 1. 字符串转换成int： stoi()！

这个问题很容易困扰人~我在刷算法时候遇到一个：[93. 复原 IP 地址](https://leetcode-cn.com/problems/restore-ip-addresses/)
这题里面需要检查提取的字符串的数字是否小于255！所以stoi就很有用！
![](https://i.imgur.com/b869pDE.png)

---
## 2. 快速从命令行读入数据到容器的方式

```
1. istream_iterator<typename T>
int main()
{
    istream_iterator<int> iter(cin), eof;
    vector<int> ivec(iter, eof);
    for (auto each : ivec) {
        cout << each << ' ';
    }
    cout << "!";
}

2. while循环
int main(){
    vector<int> ivec;
    int i;
    while (cin >> i) {
        ivec.push_back(i);
    }
    auto beg = ivec.begin(), end = ivec.end();
    while (beg != end) {
        cout << *beg++ << ' '; 
    }
    cout << "That's all!" << endl;
}
```

---
# C++ Primer Ver5

:::info
 本书的Part I（`C++`基础）是`C++`基础，必须进行通读掌握。这几章有非常多的小细节比较坑爹，指针与数组、指针与const、sizeof()运算符、this指针等等，看完这部分，一些最基础的程序基本上都能解决。第3、6、7章个人觉得对新手最困难，而且非常重要，需要重点理解。

 Part II（`C++`标准库） ，第8、9章最为重要，IO库和容器对于一个程序来说是比较基础的，记得不要在`C++`中还依然保持C的习惯，使用cout而不是printf()、使用vector而不是内置数组、使用迭代器进行遍历。第10、11章有点基础的看起来不是很难，第十章的泛型算法，如果能用起来会让你的程序更上一层楼。第12章动态内存个人觉得对于一个想要深入学习`C++`的人来说非常重要，对于内存的理解、动态数组、new和delete运算符，对于`C++`的理解更加”底层“一点，最后12.3的例子，一定要自己动手写！

Part III （类设计者的工具），我觉得是中级学习的核心了，第13章对于一个C语言的学习者来说是全新的东西，理解了拷贝和赋值还有移动，可以理解更加深入的理解类，面向对象是一个很重要的概念。第十四章我略过没看，暂时用处不大。第15章面向对象程序设计，学完个人觉得是一个显著的提升过程，最重要的是面向对象的这种思想，第15章中有很多的例子，一定要亲手敲出来！第16章，模版与泛型编程，重在理解，16.1中的函数模版和类模版只要掌握就好了。

Part IV（高级主题），第17章我也略过了，这些东西从来没遇到过（17.5可以看看，作为第八章的延伸也挺有用的），暂时不看，只作了解。第18章个人觉得你要是有志于进行`C++`软件开发，算是很重要的部分，18.1异常处理、18.2命名空间、18.3多重继承与虚继承都要重点理解，个人在看很多github上的大型工程源码时，遇到的次数还是比较多的。第19章，虽然遇到不多，但是面试笔试过程真的很喜欢问这些，new和malloc的区别、new的底层实现过程、union的内存机制等等，建议19.1、19.4、19.6重点理解。
:::
    
# 第一部分：基础内容

## Base


```
使用输入/输出： #include <iostream> --- 这个库是用来处理输入输出的！

输出运算符： <<
输入运算符： >>
作用域运算符: ::
处理输入： cin
处理输出： cout
结束当前行的操作符： endl
取地址符： & （用于引用，以及指针寻址）
解引用符： * （在cout中将指针指向对象输出）


单行注释： //
多行注释： /* ...  */
```

为什么书写的是以下的样子，没有忽略 std:: ？
> 答：这是为了保证命名空间的规范性！也就是说，这里的cout和cin都是来自std库的！

---

## 第二章：变量和基本类型

### 1.数字

![](https://i.imgur.com/ZRnC7F4.png)
![](https://i.imgur.com/iBH7teO.png)
![](https://i.imgur.com/Zz9YlyE.png)

举例：乱用无符号以及有无符号混用的结果：
![](https://i.imgur.com/bmUIkt6.png)

---

### 2.字符与字符串

![](https://i.imgur.com/svw1k3M.png)
> 这里居然会因为引号不同而不同，属于是很细节了（Py怎么就完全不在意这个）

**转义序列**
![](https://i.imgur.com/VjJWds0.png)

---

### 3.指定字面值的类型！

![](https://i.imgur.com/ni7iIfc.png)
![](https://i.imgur.com/S3rXvrs.png)

---

### 4.定义 & 声明

![](https://i.imgur.com/e2k3JVk.png)
![](https://i.imgur.com/2O7abIj.png)

---

### 5.指针（重要且折磨人的）

![](https://i.imgur.com/9QDowcS.png)
> 注意到这里的地址符&，这就是引用用的符号，也就是说引用的本质就是新建一个变量名让它和原有的一个变量名指向同一个地址！那么任意一个的改变肯定就会引起另一者的改变了！

![](https://i.imgur.com/7z0Vv9Y.png)

**关于指针的取地址和解引用的简单试用**：
![](https://i.imgur.com/yKxCcnW.png)

![](https://i.imgur.com/lpgukAW.png)
![](https://i.imgur.com/hbSBV6p.png)
![](https://i.imgur.com/wl95fJO.png)
![](https://i.imgur.com/NMyQFUL.png)
![](https://i.imgur.com/faKYret.png)
```
指针和引用的区别！

1、指针是一个变量，存储的是变量(对象)的地址，引用是变量的别名
2、指针可以为空，引用定义时必须初始化
3、指针在初始化之后可以改变指向，引用在初始化之后不可在改变
4、指针可以有多级，引用只有一级
5、sizeof指针得到的是本指针的大小，sizeof引用得到的是引用所指向变量的大小
6、当把指针作为参数进行传递时，也是将实参的一个拷贝传递给形参，两者指向的地址相同，但不是
同一个变量，在函数中改变这个变量的指向不影响实参，而引用却可以
7、引用本质是一个指针，同样会占4字节内存；指针是具体变量，需要占用存储空间
8、不存在指向空值的引用，必须有具体实体；但是存在指向空值的指针
```

![](https://i.imgur.com/vQuKsxG.png)
![](https://i.imgur.com/lMc5ETU.png)

---

### 6.Const限定符

![](https://i.imgur.com/FnITyjq.png)
![](https://i.imgur.com/XLYn9nd.png)
![](https://i.imgur.com/5enuFg9.png)
> const对象仅在对应文件有效！如果要在多个文件使用请在另外的文件声明！！！

```
此外，const也是可以创建引用的！只是，创建的引用不允许修改对象！
```
![](https://i.imgur.com/T0ln5bi.png)
![](https://i.imgur.com/Nf85GGi.png)
![](https://i.imgur.com/DOuXzVK.png)
![](https://i.imgur.com/fzznRGa.png)

---

### 7.指针与Const

![](https://i.imgur.com/XlGFX1z.png)
![](https://i.imgur.com/7aYmzvP.png)

> 注意：
> 我们遇到过两种const指针（在下面马上就会讲！看我们分析的对不对！）
```
(1).const int *p = i: 这里的意思是p这个指针不能够再赋值，但是可以改变指针位置！

eg.

    ok:   p = &i3 （可以改变指针指向！）
    ng:   *p = 6 （不能够直接修改指针对应值！）    

(2).int *const p = i: 这里的意思是p这个指针只能对着i！但是，我们可以通过解引用符来改变值！

eg.
    ok:  *p = 7 (这样会使i = 7)
    ng:  p = &i2 (不符合p指针不变！)
```
用习题2.27(d)来举例：
![](https://i.imgur.com/MD5DohX.png)

---

### 8.顶层与底层const

![](https://i.imgur.com/3OSHasU.png)
![](https://i.imgur.com/dtSRWUi.png)
> 这么看来！我们的分析没有错！如果不能够改变指针的指向（也就是改变指针的值）我们就称其为顶层const；反之，可以改变指针的值，但不能通过解引用符改变指针对象的值，那么就是底层const。
> 顶层const：表示指针本身是常量
> 底层const：表示指针指向的对象是常量

---

### 9.constexpr变量

![](https://i.imgur.com/w6Ieh6y.png)
> 用constexpr去初始化全局常量表达式!

---

### 10.处理类型（如何让你的变量和类型不混淆）

> 在大型的工程中肯定会有个问题，就是我每个变量对应的类型，用途以及意义。
> 在C++里，支持typedef来进行类型的别名，这样我们就可以将变量根据用途和类型创建不同的别名，便于分辨！
> 同样地，在C++11里，支持使用： `别名 = 类型名` 的方式，对类型起名（见下）。
![](https://i.imgur.com/X7g6VlL.png)
![](https://i.imgur.com/DunZcI4.png)

进行简单的例子再现：
![](https://i.imgur.com/4uaR6xi.png)
![](https://i.imgur.com/4EN5Olv.png)

---
### 11.auto类型符：自动决定类型！

![](https://i.imgur.com/WKjg60v.png)

---

### 12.抽取表达式的类型：decltype()

![](https://i.imgur.com/n23XIuG.png)
![](https://i.imgur.com/akykCoC.png)
![](https://i.imgur.com/aEWmJW2.png)
> decltype()括号内的内容如果也加上了括号，那么结果就是引用。

![](https://i.imgur.com/3Rv74pQ.png)
![](https://i.imgur.com/RFT4RFm.png)
![](https://i.imgur.com/OHq9eLJ.png)
![](https://i.imgur.com/JCQ0ZUa.png)

---

### 13.类：struct 和 class

类的书写：不要忘记分号！
类的导入： #include "文件名"

![](https://i.imgur.com/3AFCcfU.png)
![](https://i.imgur.com/dTr3DVs.png)
![](https://i.imgur.com/srenZml.png)

---

### 14.编写头文件！（保护代码）

![](https://i.imgur.com/xpb2Fxw.png)
![](https://i.imgur.com/sFzUU78.png)
![](https://i.imgur.com/OuwjI3H.png)

---

## 第三章：字符串，向量和数组

### 1.命名空间的using声明！

> 我们在前面使用cin和cout等命名，都需要繁琐的使用： `std::cin  std::cout`来声明命名域，那么有没有办法可以省略呢？
> 不看书，考虑python的情况，我们会想到`from std import cin, cout`这样的操作！其实在C++里面也是利用这样的操作来美化我们的代码！
```
格式：using namespace::name;
https://hackmd.io/
eg.

using std::cout;
using std::cin;
usding std::string;

诚然，我们也可以直接全部引用！就像py的 import library

using namespace 名字;

eg.

using namespace std;
```

那么，我们在使用using声明的情况，再写一次输入两个数的求和函数吧！

![](https://i.imgur.com/OUu5iXJ.png)
![](https://i.imgur.com/23s8bN1.png)
> 可以看出来，确实舒服很多了哦！

---

### 2.标准库类型string

![](https://i.imgur.com/me3BNaM.png)
![](https://i.imgur.com/aKA0YgQ.png)
![](https://i.imgur.com/bMsDVWQ.png)
> 所以直接初始化和拷贝初始化有什么巨大的差别吗？

![](https://i.imgur.com/W1AioDE.png)
![](https://i.imgur.com/ljJjc6O.png)
> **==看清楚了吗，会自动忽略开头的空白！并在读取到下一个空白的时候停止！！！！==**

![](https://i.imgur.com/YELb575.png)
![](https://i.imgur.com/F8o2iCw.png)
> ==注：要使用getline函数，要先 `#include <string>`==

![](https://i.imgur.com/K6BVdSA.png)
![](https://i.imgur.com/N5hdjLk.png)

![](https://i.imgur.com/TGJHSxo.png)

---
#### 字面值和string的相加法则！

![](https://i.imgur.com/DzuYS9i.png)
![](https://i.imgur.com/4FXqZTC.png)

> 和python明显不同，不允许多个“ ”字面值间不含string的直接相加（那你为什么又可以直接用长字面值定义呢？这一点怎么想都不方便的吧喂！）

![](https://i.imgur.com/bqcXA6s.png)

![](https://i.imgur.com/JGOYTlC.png)
![](https://i.imgur.com/P7qrVDw.png)

---

### 3.处理string中的字符！

`#include <cctype>`

![](https://i.imgur.com/BvFE6Z6.png)
![](https://i.imgur.com/Bogv5pQ.png)
![](https://i.imgur.com/rthh7x9.png)
![](https://i.imgur.com/ua8mh0i.png)
![](https://i.imgur.com/8JyAMxJ.png)
> 简单地说，就是可以同py一样使用下标进行快速访问！！！

小小示例1：小写转大写
![](https://i.imgur.com/pQafccI.png)

小小示例2：16进制数转换
![](https://i.imgur.com/Joi2ElA.png)
![](https://i.imgur.com/bdCvGTY.png)

小小示例3：习题3.10
![](https://i.imgur.com/x7CnjzG.png)
![](https://i.imgur.com/EWiLw8Z.png)

---

### 3.标准库类型Vector

![](https://i.imgur.com/fCRKZEi.png)
![](https://i.imgur.com/48z3HyC.png)
![](https://i.imgur.com/4GFkfGw.png)
![](https://i.imgur.com/A1GlOMk.png)
![](https://i.imgur.com/LjgsxRI.png)
> 小括号表示的是列表的长度，花括号才是给列表赋初始值！

#### 3.1 追加元素 & 其他操作

![](https://i.imgur.com/jkfjFf1.png)
> C++用的是 push_back 这一成员函数！！！（py的list.append()）

![](https://i.imgur.com/thtWu4D.png)

![](https://i.imgur.com/4nXPLLd.png)

---
### 4.迭代器

![](https://i.imgur.com/gnVALcr.png)
![](https://i.imgur.com/NabdNQr.png)
![](https://i.imgur.com/R34a3AZ.png)
![](https://i.imgur.com/H4KItB5.png)
![](https://i.imgur.com/sdUHopG.png)
![](https://i.imgur.com/kdJ65tu.png)
![](https://i.imgur.com/tKDrGTY.png)
> 重要， -> 符号！

![](https://i.imgur.com/YLqhniQ.png)
![](https://i.imgur.com/S8RN3lH.png)

```
我认为，迭代器就是写法还不够熟悉，其余的其实并无大异！
```
---

### 5.数组（固定长度的vector！）

![](https://i.imgur.com/euKlAvd.png)
![](https://i.imgur.com/PlvfEL7.png)

> 注意点就是：
    > 1. 定义类形时，不需要向向量一样输入： `vector<type>`，而是直接输入数组内元素类型！比如`int array[10]`；
    > 2. 一定要在名称后跟上长度！或者，[]的空括号，但是右边需要用花括号把元素全部写出！
    > 3. 在定义 `string array`时，如果使用的右边花括号写出元素，要注意长度并不等于你右边写出的长度，而是右边元素个数+1！ 因为会留一个空元素！（上面有，看得到例子！）

---
### 6.指针与数组

![](https://i.imgur.com/sC3Au0Y.png)
![](https://i.imgur.com/vpEv9ej.png)
> 这段是什么意思呢，就是说，我们如果新建变量对其赋值数组，那么编译器会默认这个变量是指向数组第一个元素的指针！例子就摆在上面！

![](https://i.imgur.com/KpKPuwd.png)
![](https://i.imgur.com/Q1JzAdg.png)
> 卧槽？用指针居然可以超出数组的索引范围一位？

==但是实际上，我们肯定想用更人性化的！所以C++11可以允许使用begin和end方法！==
![](https://i.imgur.com/5PMqS3Y.png)
> ==需要先 `include <iterator>`==

**指针运算同前面！但是！这里有很麻烦的东西就是，下标和指针居然是可以交互运算的！**
![](https://i.imgur.com/70KByvW.png)

> 如果我们用ptr指向表头，那么ptr[2]就是指向下标为2的元素，等价于*(ptr+2)!
> 同理，如果ptr指向下标为5的位置，那么ptr[-3]就是指向下标为2的元素，等价于*（ptr-3）！

#### 练习3.36

**数组**
![](https://i.imgur.com/AcmwpA0.png)

**向量**
![](https://i.imgur.com/PTxG6kI.png)

> 这里为什么是分开做数组和向量？因为向量我们通常用迭代器，而数组用指针，所以定义时候的写法是不一样的！但是指针和迭代器的计算操作极度类似，所以后半部分基本相同！

---

## 第四章：表达式

### 1.基础
![](https://i.imgur.com/pgq4Kl4.png)
> C++中可以用这样的结构来写while循环，看起来很厉害！

![](https://i.imgur.com/GOzpImC.png)
![](https://i.imgur.com/7ZVxqbM.png)
> 递增/递减运算通过先/后顺序决定是先赋予左边值再加减，还是先加减再赋值。
> 书里的建议： 使用 ++i。因为这样可以不必去存储原始值（未修改值）

![](https://i.imgur.com/5mOPNjX.png)
> 原来我们可以将解引用和++放在一起写！这也太简约了，py完全做不到这种事！！！

---
### 2.成员位访问运算符 ->
![](https://i.imgur.com/c11sFOd.png)
![](https://i.imgur.com/ZIzy6JH.png)

---
### 3.条件运算符

![](https://i.imgur.com/VZMNnRI.png)
![](https://i.imgur.com/YVKiGqX.png)
> 这个东西很好用而且还可以嵌套使用！但是套娃太多可读性下降，最好不超过两三层。
![](https://i.imgur.com/YP3SNml.png)
> 但我感觉这东西我直接用if不是一样的？

---
### 4.位运算符（未细看）

![](https://i.imgur.com/e0JhhJ0.png)
![](https://i.imgur.com/HPX25fN.png)
![](https://i.imgur.com/tKpteEI.png)
![](https://i.imgur.com/9BML99n.png)
![](https://i.imgur.com/Xw4onN7.png)
![](https://i.imgur.com/D6MIbpO.png)
![](https://i.imgur.com/9TRmK77.png)

---
### 5.sizeof运算符

![](https://i.imgur.com/4Aam7uv.png)

![](https://i.imgur.com/lgo5KQF.png)
> 那么，sizeof与 size()函数有什么差别呢？
> **sizeof 返回的是字节长度！！！！ size返回的是占位长度！**
> ![](https://i.imgur.com/LdU16S4.png)

---
### 6.逗号运算符

![](https://i.imgur.com/MHa5pX7.png)
> 在for循环里想同时两个或者多个值变化时候看起来就还蛮好用？

---
### 7.运算符优先级表

![](https://i.imgur.com/v0tRIFG.png)
![](https://i.imgur.com/YnCVr4b.png)


---
## 第五章：语句

### 1.switch语句

**前面的if这些都老生常谈所以略过。switch给我的感觉就是在很多分支的时候，比if更加美观！实际上效果就是if ... else if...** 
**switch语句格式：**
```
switch (cases){
    case 1:
        operation；
    case 2:
        operation;
    case 3:
        operation;
    ... 
}
```

![](https://i.imgur.com/5SonDNJ.png)
![](https://i.imgur.com/vwcWyNN.png)
![](https://i.imgur.com/dM5Oa12.png)
![](https://i.imgur.com/E0Y65nx.png)
![](https://i.imgur.com/26dZ0CN.png)
![](https://i.imgur.com/3OvXz6b.png)
![](https://i.imgur.com/JJjdGOQ.png)
![](https://i.imgur.com/2idk8WD.png)

> **==注意：当遇到对应case后，编译器会执行之后的所有操作！！！！！！所以，break语句不可或缺！==**

---
### 2. do while语句

和while语句基本一个意思啦！但是，do while语句看也看得出来，无论while是否成立，都会先do一次前面的执行语句！
![](https://i.imgur.com/o94byw7.png)
![](https://i.imgur.com/bUPjqKk.png)

> 这里尝试做一个输出两个字符串中较小的一个直至用户停止输入的小程序！

![](https://i.imgur.com/6LRpmrL.png)

---
### 3.跳转语句

#### 3.1 break

通用。终止最近的while,do while, for或switch语句，并从循环后开始运行。

#### 3.2 continue

通用。停止本轮迭代进入下一轮。只出现在for, while和do while。

#### 3.3 go to语句

![](https://i.imgur.com/ceMeuWy.png)

---
### 4.try和异常处理

#### 4.1 throw 语句

![](https://i.imgur.com/FGNrgrf.png)
![](https://i.imgur.com/MCBX9ZU.png)
![](https://i.imgur.com/how5rLH.png)
![](https://i.imgur.com/cgJHjQU.png)
> 其实就是自定义异常嘛！
> python中也有类似操作 

![](https://i.imgur.com/6aVwltZ.png)
![](https://i.imgur.com/ILQZiG5.png)

---
#### 4.2 try语句

![](https://i.imgur.com/mXoerpG.png)
![](https://i.imgur.com/K2C8THz.png)

在python里是这样的！
```
try
    operation1
except exception ("Error message") // 这里对应C++的throw和catch！（Python放一起了！？）
    operation2
```
---    

#### 4.3 标准异常

![](https://i.imgur.com/X2h1fzp.png)


### 习题！ 

![](https://i.imgur.com/jkE1ZJZ.png)

![](https://i.imgur.com/86zRCRW.png)
> 我的代码！能够很好的实现上面的功能！
> 细看这个代码，和python的try... except exception实际上一样的（毕竟C++是大爹！）只是，C++的错误定义实在try语句里实现的！

---
## 第六章：函数！

### 1.函数基础

> 实参与形参的区别？
> 形参：函数定义的时的参数—也就是无赋值的变量（作用是说明参数的类型）
> 实参：调用函数时使用的参数—也就是有赋值的变量（函数实际操作的对象）

![](https://i.imgur.com/fAjDDF1.png)

>不得不说确实没有全看的必要，毕竟很多我自己早就会了。
>![](https://i.imgur.com/k9QvZZu.png)

![](https://i.imgur.com/qegUcPM.png)
![](https://i.imgur.com/fCJMNRQ.png)

---
#### 1.1 局部对象

![](https://i.imgur.com/DxDQ5l2.png)
![](https://i.imgur.com/Lvdkaxi.png)
> 作用域！！！
> **==注意，局部静态对象从初始化开始一直到整个文件结束都有用！！！==**

---
### 2.参数传递 （好复杂啊！）
#### 2.1 传引用参数 & 传值参数

![](https://i.imgur.com/a66MoRJ.png)

![](https://i.imgur.com/Q7MdkQM.png)
![](https://i.imgur.com/MMIGg9U.png)
![](https://i.imgur.com/WSWnfiY.png)

> 为什么我们更倾向于使用传引用参数呢？
> ==因为传值参数无法在函数内部修改实参！（也就是说，传值参数是把这个参数的值给传入，你在里面并无法修改到这个实参！但是传引用参数传入的是引用对象，不是那个值，所以可以在函数内部修改实参！）==

---
#### 2.2 const形参和实参

![](https://i.imgur.com/bYLX6Cs.png)
![](https://i.imgur.com/0qgam7F.png)

> 简单地说，在函数中不会被修改的对象，最好做成常量引用！也就是 **const string &s**之类的！看下面例题！

![](https://i.imgur.com/s9edQZP.png)
![](https://i.imgur.com/8ARNChX.png)
![](https://i.imgur.com/AfR4f1Z.png)
> 第二个小问要求修改字符串就不能const，但是一旦不修改那么带const就是最好的！


---
#### 2.3 数组形参

![](https://i.imgur.com/MtoZ06o.png)
![](https://i.imgur.com/2TMAKBF.png)
![](https://i.imgur.com/hc9WsyZ.png)
![](https://i.imgur.com/r1wD7uu.png)
![](https://i.imgur.com/DZdZ1dl.png)
![](https://i.imgur.com/NAoGYvH.png)


> 为函数传递一个数组时，实际上传递的是这个数组的首位指针！！！！并且三种管理指针形参的技术里，我很喜欢第二种！！！
- 第一种：对于字符串这些结束会有一个空的对象很管用，只传头指针，每次后移，移到指到空对象就表示遍历完了！
- 第二种：对于所有类型通用，同时传入头指针和尾后指针，然后老规矩！
    - 第三种：和python一样，传入头指针并指定长度和初始化i=0，然后让i += 1直到到长度减一！

---
#### 2.4 main:处理命令行选项

![](https://i.imgur.com/BbTdHSg.png)
![](https://i.imgur.com/R7prryK.png)

```
25、26： 这题非常的重要！！！ ，

需要给main()函数传递实参，之前写的程序基本上main()函数都是空形参列表

特殊点：使用argv中的实参时，一定要记得可选参数从argv[1]开始，argv[0]保存的是程序的名字。
```
![](https://i.imgur.com/SggPVZp.png)

---
#### 2.5 含有可变形参的函数 

![](https://i.imgur.com/DckhsVe.png)
![](https://i.imgur.com/3yMYXH2.png)
![](https://i.imgur.com/5AzDaNq.png)
![](https://i.imgur.com/E52crTe.png)


> 意思这个东西拿来当作函数的形参，可以接收输入相同类型但是长度不定的复数个实参！
> 那我有个问题，为啥不直接用vector?答案就在下面哈！
> ![](https://i.imgur.com/HCbv4Bk.png)

- [x] (对于少数个好说，但是)若数量很长，我难道一个一个写进去实参体当参数？就像上面例子？

---
### 3.返回类型和return语句

![](https://i.imgur.com/ZEUBgGq.png)

#### 3.1 无返回值函数
![](https://i.imgur.com/Rwqv9HY.png)

---
#### 3.2 有返回值函数
![](https://i.imgur.com/qNzkvW3.png)
![](https://i.imgur.com/77Iju76.png)

---
#### ==关于返回值！！（？）==
![](https://i.imgur.com/PM1uc4A.png)
![](https://i.imgur.com/dp2uQQ0.png)
![](https://i.imgur.com/YEn8zTa.png)
![](https://i.imgur.com/5QcsLmZ.png)
![](https://i.imgur.com/UIy6Zsp.png)
![](https://i.imgur.com/1IoEwpx.png)
![](https://i.imgur.com/4Cld8XT.png)

---
#### 返回数组指针

![](https://i.imgur.com/2g0bYvf.png)
![](https://i.imgur.com/RotS06N.png)
![](https://i.imgur.com/FDhkV4O.png)


> 36：
> 知识点：由于数组不能被拷贝，所以函数不可以返回数组，但是我们可以返回函数的指针。利用的是类型别名的方法。
> 
> 1、声明返回数组指针的函数：

```
int (*func(int val))[10]

逐层理解：
1. fun(int val)表示一个形参为int类型的函数
2. (*func(int val))表示可以对这个函数的返回值进行解引用；
3. (*func(int val))[10]表示解引用可以得到大小为10的数组；
4. int (*func(int val))[10]表示数组中的元素为int类！

```
> 例：对于函数的定义形式需要了解
> 
> 2、尾置返回类型(此为C++11新标准，有兴趣的可以研究下)
> 
> 3、使用decltype，已知函数的返回值时，可以使用关键字decltype表示返回类型为指针

![](https://i.imgur.com/jJD2u3a.png)
> 个人觉得第二种方式比较好，既然是C++11的新标准，那么肯定有它创新的意义所在，简短方便。

---
### 4.函数重载 - Python无法函数重载（不利用库的话）！

![](https://i.imgur.com/EPlKPPz.png)
![](https://i.imgur.com/J9W0z0b.png)
![](https://i.imgur.com/apIstqX.png)
![](https://i.imgur.com/U8eOSFt.png)
![](https://i.imgur.com/GtZ0EID.png)
![](https://i.imgur.com/jI8JiNk.png)
![](https://i.imgur.com/kXajVLb.png)
![](https://i.imgur.com/oh3JzhP.png)
![](https://i.imgur.com/vi5OtSE.png)
> 39：
> 
> 知识点1：函数的重载必须有形参数量或者形参类型上的不同
> 
> 知识点2：顶层const不影响传入函数的对象
> 
> 知识点3：此小节虽然只有一题，但是函数的重载是一个很重要的知识点，需要多看，深入理解
> 
> (a)：错误，只是重复生命了
> 
> (b)：错误
> 
> (c)：正确
---

#### 重载与作用域
![](https://i.imgur.com/OljlwYp.png)
![](https://i.imgur.com/CWbWEvZ.png)

> 简单来说，就是如果在局部作用域内定义了与外部同名函数，然后再在域内调用时，外部的函数不会被重载而是会被隐藏！也就是说，==不要在作用域内声明函数！！！==

---
### 5.特殊用途语言特性

#### 5.1 默认实参

![](https://i.imgur.com/1ECdL8L.png)
![](https://i.imgur.com/bIXHa0F.png)
> 和py一样，但是，C++不能调用参数时利用形参来区分，所以得按顺序写！！！

![](https://i.imgur.com/7VPpdQe.png)
![](https://i.imgur.com/3thUzub.png)

> 知识点1：函数反复调用的过程中重复出现的形参，这样的值被称为默认实参。该参数在使用过程中可以被用户指定，也可以使用默认数值
> 
> 知识点2：调用含有默认实参的函数时，可以包含该实参，也可以省略该实参。
> 
> 知识点3：==一旦某个形参被赋予了默认值，其后所有形参都必须有默认值==。
> 
> 知识点4：==顺序很重要！在设计函数时，将默认值的形参放在后面==。
> 
> 知识点5：在给定的作用域中，一个形参只能被赋予一次默认实参，且==局部变量不能作为默认实参==。

---
#### 5.2 内联函数和constexpr函数

![](https://i.imgur.com/JlaYqK7.png)
![](https://i.imgur.com/qb1KvZW.png)

> ==那这破东西就是： 对python里面可以直接用lambda实现一行表示完的函数，前面加个inline就可以让它的速度变快！==

![](https://i.imgur.com/aN4TPOQ.png)
![](https://i.imgur.com/VDVgrcp.png)

```
知识点1：将函数指定为“内联函数(inline)”，将它在每个调用点上“内联的展开”，该说明只是向编译器发出一个请求，编译器可以选择忽略这个请求。内联的机制用于优化规模较小、流程直接、频繁调用的函数，建议不大于75行。

知识点2：constexpr函数是指能用于常量表达式的函数：函数的返回值类型和所有形参的类型必须是“字面值类型”：算术、引用、指针。并且函数体内有且只有一条return语句。

知识点3：将较小的操作如比较两个字符串的大小定义为函数，有很多的优点。

知识点4：inline函数和constexpr函数可以在函数中多次定义，但是通常将其定义在头文件中。
```

---
#### 5.3 调试帮助

##### 5.3.1 assert & NDEBUG
![](https://i.imgur.com/bYAqXlp.png)
![](https://i.imgur.com/GmLtALB.png)
> 注意，需要先 `#include <cassert>`
![](https://i.imgur.com/psViD2C.png)
![](https://i.imgur.com/Yp6C48n.png)

![](https://i.imgur.com/rfH0ggY.png)
![](https://i.imgur.com/oPRM4D7.png)

```
程序的调试帮助：assert和NDEBUG

知识点1：预处理宏assert(expr)：包含一个表达式，expr为真时，assert什么也不做，为假时输出信息并终止程序。包含在cassert头文件中。通常用于检查不能发生的条件

知识点2：assert依赖于一个NDEBUG的预处理变量的状态，如果定义了NDEBUG，assert什么也不做，默认状态下NDEBUG是未定义的。编译器也可以预先定义该变量。

知识点3：也可以使用NDEBUG编写自己的条件调试代码，如果NDEBUG未定义，将执行#ifndef到#endif之间的代码，如果定义了NDEBUG，这些代码将被忽略。
```

---
### 6.函数匹配

![](https://i.imgur.com/yhZapXZ.png)
![](https://i.imgur.com/OFCnS5a.png)

---
### 7.函数指针 -- p = func (然后调用p等于调用func)
![](https://i.imgur.com/yXV5qKR.png)
![](https://i.imgur.com/RegjqBF.png)
![](https://i.imgur.com/f77bWnU.png)

![](https://i.imgur.com/GdDmQI9.png)
![](https://i.imgur.com/DCiOxNr.png)
![](https://i.imgur.com/IkjyFsA.png)


---
## 第七章：类

![](https://i.imgur.com/GDr1Nd2.png)
![](https://i.imgur.com/lKuetbE.png)
![](https://i.imgur.com/DgZjUB6.png)
![](https://i.imgur.com/0pp5fhJ.png)
> 重要！

---
### 1.定义类

![](https://i.imgur.com/clKwXdZ.png)

---
#### 1.1 定义成员函数
![](https://i.imgur.com/KxXlVT1.png)
![](https://i.imgur.com/JbNYYtR.png)
![](https://i.imgur.com/nsozWUp.png)
![](https://i.imgur.com/AAQzTMq.png)

---
#### 1.2 在类的外部定义成员函数！

![](https://i.imgur.com/9lTQUxy.png)
![](https://i.imgur.com/kKXwvw6.png)
![](https://i.imgur.com/sCXhkmi.png)

---
#### 1.3 定义类相关的非成员函数

![](https://i.imgur.com/siAhs7A.png)
![](https://i.imgur.com/V3EDhfq.png)
![](https://i.imgur.com/patDQov.png)

---
#### 1.4 构造函数
![](https://i.imgur.com/0LBzgKj.png)
![](https://i.imgur.com/SPo85IB.png)
![](https://i.imgur.com/awf283y.png)
![](https://i.imgur.com/MAvE5R7.png)
![](https://i.imgur.com/zB8daWR.png)
![](https://i.imgur.com/q7EAxiA.png)
![](https://i.imgur.com/K3ZQDN3.png)

看起来可能会麻烦？但是实际上，意思相当简单。

- 首先，如果我们对类的成员属性设置了默认值，那么在C++11里，我们可以最开始定义一个构造函数， `Struct_name() = default`；
- 在这个基础上，我们可以通过初始化时在小括号内键入值来构造成函数的成员属性；
- 其次，通过定义 read 函数，可以实现先声明类对象，再 `read(cin, 对象)`来初始化成员属性。
- 总之，类的构造函数和例子丢在下面。
![](https://i.imgur.com/y4jDgjm.png)
![](https://i.imgur.com/yIl3YD7.png)

---
### 2.访问控制与封装

> 什么是封装？拿电脑来做例子，我们能够用到的应用以及指令是公有的（public），而后面的实现以及电路组成之类的，对我们而言是未知（私有，private）的。
- public：接口，能够访问到的东西；
- private：封装的实现细节。
- **封装的作用：封装实现了类的接口和实现的分离，隐藏了类的实现细节，用户只能接触到类的接口。**
- 优点：

> 隐藏类的实现细节；
> 让使用者只能通过程序规定的方法来访问数据；
> 可以方便的加入存取控制语句，限制不合理操作；
> 
> 类自身的安全性提升，只能被访问不能被修改；
> 
类的细节可以随时改变，不需要修改用户级别的代码；

我们可以将一部分属性设置成private，这样就无法直接从外部访问到了！
但是有个问题就是，比如我们将上面的sales_item的属性都设置成private，那，read，print和combine这些函数也无法访问到这些属性，不就报错了吗?
- 解决方法：友元（friend），即在类域内使用friend声明使其可以访问私有属性！
![](https://i.imgur.com/PsbOa4c.png)

下面的部分是CPP primer的内容（我把觉得有必要看的拿出来）

![](https://i.imgur.com/MPwUjwd.png)
![](https://i.imgur.com/9sxW3UE.png)
![](https://i.imgur.com/YM9OXRX.png)
![](https://i.imgur.com/N6Ewxyk.png)

![](https://i.imgur.com/BPkxdXy.png)
![](https://i.imgur.com/vEAebY4.png)
> 友元的不足之处：会降低类的封装性！

---
### 3.类的其他特性

![](https://i.imgur.com/pouh8qd.png)
![](https://i.imgur.com/kKk25vj.png)
![](https://i.imgur.com/RAwSYsN.png)
![](https://i.imgur.com/88AFiNi.png)

#### 友元再探

之前的友元是，对于非成员函数使用friend进行内部私有属性的调用。实际上，对于其它的类或者其他类的内部某一函数，我们也可以通过friend来进行友元调用！
![](https://i.imgur.com/nMQ3pVV.png)
![](https://i.imgur.com/BXHZ4yt.png)
![](https://i.imgur.com/WAHQZuG.png)
![](https://i.imgur.com/hVBmZyo.png)
> 注意函数的友元声明需要上面三步！
> 1. 要成为友元的类在类内声明函数（但是不要定义！因为后面才会声明友元）；
> 2. 在对应类的头部或者尾部声明友元！
> 3. 在这之后，才可以在外部定义对应需要友元的函数！！！

---
### 4.类的作用域

![](https://i.imgur.com/Lcfi2TP.png)
![](https://i.imgur.com/j4jEa9h.png)
![](https://i.imgur.com/tiGIwZk.png)
![](https://i.imgur.com/n91YfYA.png)
![](https://i.imgur.com/LkZl2iD.png)
![](https://i.imgur.com/SEBWDYM.png)

![](https://i.imgur.com/U5Qm6rl.png)
> 对于成员属性而言，就是：先找花括号内上方有没有声明/定义，再找类内，类内都没有再找全局；如果有几个重名的然后只是使用某一个就用::符号来声明命名域！

---
### 5.构造函数再探

初始化问题：平常对于类的成员属性我们都可以利用构造函数进行赋值，但是！如果说，某些属性是const或者引用（&）这样的类型，它们无法被赋值，只能初始化时候就定好，应该怎么办呢？？？
```
正确的方式：初始化时候就选定好！

eg. Sales_item::Sales_item(int i): unit_sold(i) {} -- 就是我最喜欢的写法...

这样就是直接初始化成对应值，而非先初始化再赋值！就能解决const与引用的问题！
```
![](https://i.imgur.com/jzCKnCq.png)
![](https://i.imgur.com/kSDDle2.png)

**成员初始化顺序：定义顺序！**

![](https://i.imgur.com/DqRukzf.png)
> 教训：尽量以定义顺序进行成员初始化；且尽量不使用成员去初始化其他成员！

---
#### 委托构造函数

![](https://i.imgur.com/KcLRkLS.png)
> 不必迷惑，简单的来说就是你在构造时候就设定一部分了，只需要构造剩下的部分即可
> （为什么叫委托？看第二三种的形式，都是在输入需要设定的部分后，将其余成员设置成默认的值然后丢进去了第一个构造函数！！！所以叫委托！这个写法会读就可以，很简单！）
> 比如我下面划红线的其实就是一个委托构造函数！它只输入n的值，其他的值设定好后，送入default下面那个构造函数就完事了！
```
这样对比看来，python对于类的书写真的是简单太多了！初始化只需填参数列表就可以！而C++根据不同的初始成员的键入方式需要设置这么多的构造函数哦！
```

![](https://i.imgur.com/6jZ7zTx.png)

---

#### 聚合类
![](https://i.imgur.com/BFCyWbk.png)
![](https://i.imgur.com/1D2ZxP2.png)

---

### 6.类的静态成员

> 就像python的类里可以定义对象属性和类属性一样，我们想要定义与类有关联而非与类的对象关联的成员时，就需要用到静态成员！
> 同样地，python也有静态方法，就是作用于类而非具体的某个类对象！

![](https://i.imgur.com/I0bBGHL.png)
![](https://i.imgur.com/m1hbJV2.png)
![](https://i.imgur.com/YrItzHo.png)
![](https://i.imgur.com/o7I8bzw.png)
![](https://i.imgur.com/qZnGQZb.png)
![](https://i.imgur.com/PfGjg72.png)
> 知识点1：类的静态成员：该成员只需与类的本身有关，而不是与类的对象有关，加上static关键字即可声明，其不与任何实例化对象绑定，但是我们仍然可以使用类作用域运算符访问静态成员。
> 
> 知识点2：static声明在内部。在外部定义时，不加static.类似与一个全局变量，其初始值必须是常量表达式。
> 知识点3：静态成员独立于任何对象，其类型可以是它所属的类类型。而非静态成员只能声明为其类的指针或引用

![](https://i.imgur.com/jhP26wE.png)

---
# 第二部分：C++标准库

---
## 第八章：IO库

![](https://i.imgur.com/IVFFIii.png)

> 下面的两个类型都是利用继承机制继承于iostream基类的！所以使用方法是一样的！

---
### 1.iostream类

![](https://i.imgur.com/YLiL9S7.png)

> 流类型对象无法拷贝，所以我们在函数调用的时候都是使用的引用！（因为不管是作为形参输入，还是作为返回类型，不使用引用的话都是涉及到拷贝的！）

![](https://i.imgur.com/f45JG8l.png)
![](https://i.imgur.com/d1M7CF7.png)

> 每一个io类对象都具有上面的条件状态成员属性与函数！

![](https://i.imgur.com/1NNYGid.png)

![](https://i.imgur.com/K9TNNPn.png)
![](https://i.imgur.com/eOOENO4.png)
![](https://i.imgur.com/TSFQb3g.png)
![](https://i.imgur.com/K2J6h3V.png)

> 知识点：badbit :流已崩溃
> eofbit :流达到了文件结束位置
> failbit ：IO操作失败
> 
> bad、eof、fail在对应错误位置时，cin会在错误状态，即循环中止 

> 知识点：刷新缓冲手段： endl; flush; ends; unitbuf.
> endl:输入一个换行符，然后刷新缓冲区；
> flush：直接刷新缓冲区，不添加任何字符；
> ==ends：输出一个空字符，然后刷新缓冲区；== -> 看起来会比较好用！
> unitbuf： 使用 cout << unitbuf 可以使所有的输出操作后都自动刷新缓冲区！

> 知识点：cin与cout同时出现时是绑定的：也就是有cin的话cout的缓冲区就会自动被刷新！

---

### 2.fstream类 - 文件流

fstream：读入/写出 - 文件与内存！

![](https://i.imgur.com/ZTl8kXf.png)
> 作为iostream的继承类，也拥有上面iostream的全部成员！

![](https://i.imgur.com/MPScB44.png)
![](https://i.imgur.com/9s3q3Kn.png)
![](https://i.imgur.com/yAiHG1J.png)
> 注意，最多只能同时打开一个文件！要打开第二个就要关闭第一个！所以, is_open() 函数可以帮我们很好的检查这一点！！！

![](https://i.imgur.com/NZaZiKS.png)
![](https://i.imgur.com/ZqOep3o.png)
![](https://i.imgur.com/jqFzuUo.png)

---
**练习：**

![](https://i.imgur.com/wrXeSil.png)

4.
![](https://i.imgur.com/iI4TxTM.png)

5.
![](https://i.imgur.com/LbNjls8.png)
> getline表示读一行，而读入字符串直接和cin一样。（其实会发现，f的用法和平常的cin，cout一模一样！）

> 个人尝试：把一个文件的内容读入到另一个文件！
![](https://i.imgur.com/A3VXCXN.png)

---

### 3.sstream类 - string流

![](https://i.imgur.com/u6cor4n.png)
> 通常需要处理行内单词时，就会用到sstream（可以看书上的例子，每一行要分开读取的内容数量不一定相等）

![](https://i.imgur.com/hh0X3zI.png)
![](https://i.imgur.com/rjFkyyj.png)

---
**练习**
![](https://i.imgur.com/o2HPqUI.png)

10.
![](https://i.imgur.com/OXWJlfi.png)

---

### 总结

![](https://i.imgur.com/uvrWzoG.png)
> in，out的功能都一样，它们仅仅是作用对象不同：
> iostream：从命令行到内存的读入/输出；
> fstream：从文件到内存的读入/输出；
> sstream：从字符串到内存的读入/输出！
> 所以in肯定与 >> ，out肯定与 << 符号匹配！

---

## 第九章：顺序容器

![](https://i.imgur.com/LE3eSh0.png)
![](https://i.imgur.com/BGti3L2.png)
![](https://i.imgur.com/v33jEoe.png)

> 重点：不清楚用什么的时候就直接用迭代器书写且尽量不用下标，因为这样有较强替换性！！！（链表无下标，但是所有类型都是可以迭代的！）

---
### 1. 容器通用操作

![](https://i.imgur.com/MNwzqyL.png)
![](https://i.imgur.com/K2sCXGZ.png)
![](https://i.imgur.com/DPsbDvR.png)
![](https://i.imgur.com/vbj0I4T.png)

---
#### 1.1 迭代器

> forward_list(单向链表）不支持递减运算符！！！！

![](https://i.imgur.com/mSoUcX4.png)

---
#### 1.2 容器成员

![](https://i.imgur.com/J23GDu1.png)
![](https://i.imgur.com/7kDM7P6.png)
![](https://i.imgur.com/mgm6ts4.png)
![](https://i.imgur.com/gAcTqJK.png)
![](https://i.imgur.com/fOdYrVH.png)
![](https://i.imgur.com/r0Xhuqd.png)
![](https://i.imgur.com/TrATxOm.png)
![](https://i.imgur.com/cEVdtl2.png)
![](https://i.imgur.com/CACJ2UH.png)
![](https://i.imgur.com/Ve4Ox9T.png)

> assign:赋值， swap:交换

![](https://i.imgur.com/CpwDbxL.png)
![](https://i.imgur.com/2hyU8dh.png)
![](https://i.imgur.com/gaYgiJm.png)

---
### 2.顺序容器操作

#### 2.1 插入
![](https://i.imgur.com/FwHh6E6.png)
![](https://i.imgur.com/torxJM2.png)
![](https://i.imgur.com/NZBQ6cc.png)
![](https://i.imgur.com/6rztBQc.png)
![](https://i.imgur.com/FRtJ7gk.png)
![](https://i.imgur.com/G7588Gy.png)
![](https://i.imgur.com/YfRMmT5.png)
![](https://i.imgur.com/GREgCoj.png)
![](https://i.imgur.com/gvpXlLQ.png)


> 尾部添加：push_back （支持array和forward_list之外的全部类型） - append
> 首部添加：push_front(支持list（双向链表）和deque（队列）) - appendleft
> 中间添加：insert（支持vector, string, deque, list），也可以作为其余类型的push_front使用！！！ - insert （注意，首尾之外插入很费时间！）

---
#### 2.2 访问

![](https://i.imgur.com/ilVDlWJ.png)
![](https://i.imgur.com/d2xO7DO.png)
![](https://i.imgur.com/N8Jf0EE.png)
> 访问可以利用front()和end()函数直接访问头尾！或者直接用下标访问任意位置！
> 注意访问返回的是该元素的引用（说人话就是可以通过访问来修改但是要设置成引用！）

---
#### 2.3 删除

![](https://i.imgur.com/oDSCkxK.png)
![](https://i.imgur.com/mBPqSZY.png)


> ==我超，C++里面的pop()系函数不返回被删除的元素的！==

---
#### 2.4 单向链表：froward_list的特殊操作！

> **数据结构里学过了哈哈哈哈，简单回顾一下就好**

![](https://i.imgur.com/wfqEKD7.png)
![](https://i.imgur.com/OEGBqYC.png)
![](https://i.imgur.com/LJVaISW.png)
![](https://i.imgur.com/zZ9kqD3.png)

> 会发现，链表操作其实差挺多的！这里都可以直接用函数解决，Python反而需要调用next（）来自己搭链子！！？

---
#### 2.5 改变容器大小 - resize()

![](https://i.imgur.com/adRIi1k.png)

> 感觉会用到！

---
#### 2.6 容器操作可能使迭代器失效！

![](https://i.imgur.com/nGSQQxc.png)
![](https://i.imgur.com/I7jLlrR.png)
![](https://i.imgur.com/TLrb5pT.png)
![](https://i.imgur.com/edp6WOZ.png)
![](https://i.imgur.com/ECHGDcB.png)

> 操作建议：
> **==1. 在添加元素后记得把对应位置迭代器往后加，不然迭代器会失效；==**
> **==2. 在while,for循环中如果会增删容器内容的话，不要在事前静态缓存好end，而是在循环条件内调用 lst.end()使其每次循环都不会失效！因为，任意的删除或添加都有可能使原本end迭代器失效！==**

---

### 3. vector对象是如何增长的

![](https://i.imgur.com/e0SuVda.png)
![](https://i.imgur.com/2LIPxvn.png)

----
#### 3.1 知识点习题

![](https://i.imgur.com/Br1i4vT.png)
> 知识点：vector中的元素在内存中是连续存放的，所以需要capacity这个东西~
> 
> list没有capacity是因为其存储元素的内存是不连续的，不需要capacity！
> 
> array是因为其大小是固定的！

---

### 4. 额外的string操作！（专讲string的）

#### 4.1 构造string的其他方法！（切片）
![](https://i.imgur.com/CJ0ThiS.png)
![](https://i.imgur.com/IFKpLrm.png)
![](https://i.imgur.com/WpKQXWf.png)

> 别被吓到了就是切片的实现！

---
#### 4.2 改变string的其他方法

![](https://i.imgur.com/Wnhqy5F.png)
![](https://i.imgur.com/rgFJNUm.png)

> 别怕，还是切片！

---
#### 4.3 string的搜索操作

![](https://i.imgur.com/4YVTgEF.png)
![](https://i.imgur.com/k2RoNQ2.png)
![](https://i.imgur.com/HEugT8D.png)


> 总之，find()和rfind()还是比较好用！其他的用到在查！

---
#### 4.4 compare()函数

![](https://i.imgur.com/FE6XHfO.png)
![](https://i.imgur.com/HUvOx7E.png)

---
#### 4.5 数值转换

![](https://i.imgur.com/cOwVxTN.png)
> 没看，用到再查吧！

---
### 5. 容器适配器

> 栈，单向队列以及优先队列！（简单介绍只是）

![](https://i.imgur.com/niJkLOO.png)
![](https://i.imgur.com/GNaMbNz.png)

---

#### 5.1 stack栈

![](https://i.imgur.com/HFIzK8c.png)

---

#### 5.2 queue队列

![](https://i.imgur.com/JmjgxRo.png)
![](https://i.imgur.com/Ei0irlS.png)

---

### 6. 小结

![](https://i.imgur.com/s6gltEA.png)

> 要点：
>
> 1. 各种容器的共通操作，容器的初始化及格式写法；
> 2. 容器的迭代操作；
> 3. forward_list 单向链表的操作和python差别很大；
> 4. string的切片操作和python也不像，使用时需要熟练；另外string有一个find()操作不错！
> 5. vector的capacity和size！得要回答得起来！
> 6. 有stack和queue，操作和python一样的！但是也得看看！

---

## 第十章：泛型算法

泛型的：指可以用于不同类型的容器和元素的（实际上我们的排序，查找，替换等函数都是泛型的。

> 因为实在没有啥新东西，就直接只写我觉得需要的了。

![](https://i.imgur.com/FQxpaST.png)
![](https://i.imgur.com/icTdaWB.png)

---

### 1. 初识泛型算法

#### 1.1 只读算法：只读取输入范围的值，不改变元素

1. accumulate
![](https://i.imgur.com/ShRT7Y2.png)
![](https://i.imgur.com/izE1hAi.png)
![](https://i.imgur.com/Xvg2Dn1.png)

![](https://i.imgur.com/BxNAS1D.png)

2. equal
![](https://i.imgur.com/Ps792Ce.png)

---
#### 1.2 写容器元素的算法

> ==记住，算法不会执行容器操作，因此它们自身不可能改变容器大小！所以使用时候注意range!==

1. fill & fill_n
![](https://i.imgur.com/Wcy1EXo.png)

-
![](https://i.imgur.com/lEDG7Wh.png)
![](https://i.imgur.com/axVlwPr.png)

---

2. back_inserter

![](https://i.imgur.com/gbkieGQ.png)

---

3. 拷贝算法：copy & replace

![](https://i.imgur.com/CT0IEQn.png)
![](https://i.imgur.com/VkVb50L.png)

---
#### 1.3 重排容器元素的算法 - sort() & unique()

![](https://i.imgur.com/jfij10A.png)
![](https://i.imgur.com/STJayez.png)
![](https://i.imgur.com/oUH8y5D.png)

---
### 2.定制操作 - 神奇！！！可以自定义排序方法！！！

#### 2.1 向算法传递函数 -  神奇！！！自定义排序方法！！！

![](https://i.imgur.com/55WFcQZ.png)
![](https://i.imgur.com/80NLFPI.png)
![](https://i.imgur.com/w3Aeq5E.png)

![](https://i.imgur.com/vVeSUGu.png)
![](https://i.imgur.com/OIV8KrB.png)

**==练习==**
![](https://i.imgur.com/PGtuRGi.png)

![](https://i.imgur.com/tVjArRM.png)

---

#### 2.2 lambda表达式

![](https://i.imgur.com/PYH2Kyk.png)
![](https://i.imgur.com/RUi9G1e.png)
![](https://i.imgur.com/3rx6fX0.png)
![](https://i.imgur.com/6L7feay.png)

![](https://i.imgur.com/xduGZ0m.png)
![](https://i.imgur.com/FFMlnXV.png)
![](https://i.imgur.com/5VyEmak.png)

> lambda在 C++ 写法：

```
[捕获参数列表（写入函数体使用的参数写入）] (参数列表) {返回内容}  
// 注：不需要写返回类型，会自动识别！
```

![](https://i.imgur.com/fqC2ENE.png)
![](https://i.imgur.com/SEf5Ffy.png)

---

#### 2.3 lambda捕获和返回

![](https://i.imgur.com/c1247Xo.png)

![](https://i.imgur.com/Ge1q8fm.png)
![](https://i.imgur.com/k9xYJBH.png)

![](https://i.imgur.com/JSIm6GL.png)
![](https://i.imgur.com/jA3aB4l.png)
![](https://i.imgur.com/Ljamdef.png)

> 隐式捕获看起来会舒服一些？？？

![](https://i.imgur.com/BFX7qN2.png)
![](https://i.imgur.com/7Pa56qf.png)
![](https://i.imgur.com/XdiMIKD.png)

---

**==练习==**

![](https://i.imgur.com/ICbckB8.png)
![](https://i.imgur.com/qNubvSW.png)

---
#### 2.4 参数绑定

![](https://i.imgur.com/ups6f5G.png)

> 为什么会有这个参数绑定存在？
> 如果某个函数我们只利用一两次且函数体简单那么当然用lambda是最方便。但是，如果要用很多次呢？写函数就比较舒服了，毕竟lambda每用一次就得写一次！
> 但问题就来了，lambda可以捕获局域变量传入接受谓词的函数，但是我们定义的函数如果需要两个以上的变量且有变量是局域的，你直接像之前那样丢函数名字 （==如： f(s1, s2, function)==）而不带括号内的参数，就有问题了啊！（比如上面的checksize）。
> 那么为了解决这个问题，我们就需要参数绑定！

![](https://i.imgur.com/DNtGLSs.png)
![](https://i.imgur.com/nyyifph.png)

> 绕回来其实就是教你怎么不用lambda...
> 
![](https://i.imgur.com/QUFVy4p.png)
![](https://i.imgur.com/JpGiQ9t.png)
![](https://i.imgur.com/JSONdaX.png)

---
**==练习==**

![](https://i.imgur.com/bit3vHo.png)
![](https://i.imgur.com/nTc4oSS.png)

---

![](https://i.imgur.com/OuEAKGp.png)
![](https://i.imgur.com/HBakMI8.png)

---

### 3. 再探迭代器 -- 很多花操作！！多看看

![](https://i.imgur.com/kfrhjgb.png)
![](https://i.imgur.com/9BRxcNa.png)

---
#### 3.1 插入迭代器

![](https://i.imgur.com/y3nuwO3.png)
![](https://i.imgur.com/engoiyr.png)

---

#### 3.2 iostream迭代器(有意思！)

**1. istream_iterator**
![](https://i.imgur.com/mwoZDd4.png)
![](https://i.imgur.com/TTz0J1c.png)

> 这个写法还蛮新奇的哈，你让我写我还真写不出来！
![](https://i.imgur.com/6lihFoq.png)

![](https://i.imgur.com/kaeqT0C.png)
![](https://i.imgur.com/7gkfvKE.png)
> 迭代器不一定要在容器内使用，我们也可以用算法流来操作！（所以之前的count_if, find_if, accumulate等可以接收谓语的函数都是可行的！）

---
**2. ostream_iterator**

![](https://i.imgur.com/a1BG02x.png)

![](https://i.imgur.com/JUkZpbR.png)

---
**练习**

![](https://i.imgur.com/M1XHRzL.png)

![](https://i.imgur.com/Dm5dhUV.png)

![](https://i.imgur.com/GDawng0.png)
**30.**
![](https://i.imgur.com/gNyBeXp.png)
**31.**
![](https://i.imgur.com/X15KDrm.png)

---
#### 3.3 反向迭代器rbegin()

![](https://i.imgur.com/YcUNGNp.png)
![](https://i.imgur.com/PG3Bfj4.png)
![](https://i.imgur.com/VuVjJWQ.png)
![](https://i.imgur.com/iQfTnKo.png)

---
**练习**

![](https://i.imgur.com/Sb5cLNf.png)

34.
![](https://i.imgur.com/aD2bZeD.png)

35.
![](https://i.imgur.com/WIsPj1b.png)

36.
![](https://i.imgur.com/xOzkFFX.png)

37.
![](https://i.imgur.com/aPDa1qy.png)

---
### 4. 泛型算法结构

![](https://i.imgur.com/vxqE0VL.png)
> find这样的不写且不添加元素的，sort这种要求写的以及inserter这样添加的，都需要不同类型的迭代器！

![](https://i.imgur.com/v9TuIu9.png)

> 输入迭代器：可以读取序列中的元素。（find, count, read...)
> 输出迭代器，看作是输入迭代器的补集，可写。(sort, for_each...)
> 前向迭代器：单向，支持输入输出。 （list单向链表）
> 双向迭代器：双向，支持读写，还支持递增递减运算符。（forward_list双向链表）
> 随机访问迭代器：基本支持所有功能。（vector向量）

![](https://i.imgur.com/aMJLa9j.png)
> 如果某个算法同时输入两个容器的迭代器，那么第二个容器的迭代器通常只输入beg，因为我们默认第二个至少和第一个等长！

![](https://i.imgur.com/PCXR9hq.png)

---
**练习 & 知识点**
![](https://i.imgur.com/r5C15dR.png)

> 算法的命名规范：_if 非重载，满足谓词条件的版本。_copy拷贝版本的算法
> 
> (a)：遍历beg到end，找到oldVal就用newVal替换
> 
> (b)：遍历beg到end，找到满足pred条件的就用newVal替换
> 
> (c)：遍历beg到end，找到oldVal就用newVal替换，并将其拷贝至dest
> 
> (d)：遍历beg到end，找到满足pred条件的就用newVal替换，并将其拷贝至dest

---

### 5. 特定容器（链表）算法

![](https://i.imgur.com/Q6GFJvv.png)
![](https://i.imgur.com/Kfh2F9M.png)

---
### 小结

![](https://i.imgur.com/oLwdJyQ.png)

---
## 第十一章：关联容器(map, set...hash)

> 看来只是讲字典和集合？那应该很简单哦！

![](https://i.imgur.com/336K0Rt.png)

---

### 1. 使用关联容器

![](https://i.imgur.com/rBODH8x.png)
![](https://i.imgur.com/drwAcvy.png)
![](https://i.imgur.com/04xS5Qs.png)
> 这里和python不一样，python还没有加入键值对的需要自己加入先！才能+1.

> 知识点1：map的定义及使用
> 
> 1：包含map头文件，2：指定关键词和值的类型，3：关键词可作为下标，索引对应值
> 
> 知识点2：size_t是一种机器相关的无符号类型，它被设计的足够大以便能保存内存中的任意对象的大小，在使用数组下标时，经常会用到此类型
> 
> 知识点3：对map对象进行下标操作，如若关键字不在map中，下标运算符会在map中创建一个新元素(关键词)，对应值为默认初始化值。
> 
> 知识点4：从map对象中提取一个元素时，会得到一个pair对象，其保存两个public数据对象，first为其关键字，second为其对应值。


![](https://i.imgur.com/ssUdnce.png)
![](https://i.imgur.com/XEKVatV.png)


![](https://i.imgur.com/Xo0ssyr.png)

---

### 2. 关联容器概述

#### 2.1 定义关联容器

![](https://i.imgur.com/2jdB5xr.png)
![](https://i.imgur.com/fNe9zvI.png)

> 知识点1：关联容器不支持顺序容器的位置相关操作，如：push_back、push_front，因为其是按关键字存储的
> 
> 知识点2：关联容器的迭代器都是双向的，还有一些关于哈希性能的操作
> 
> 知识点3：multimap和multiset允许多个元素具有相同的关键字，所以给multiset和multimap中传入相同的元素，是可行的，而set、map会忽略相同关键字的元素

---
**练习**

![](https://i.imgur.com/4hpge6V.png)
![](https://i.imgur.com/3ZAxt5G.png)

> set会自动忽略重复的关键字，在vector中查询重复并插入元素非常耗时！

---
#### 2.2 pair类型

![](https://i.imgur.com/2YwIZDx.png)
![](https://i.imgur.com/tPoQgSH.png)
> ==那我是否可以理解成，map里存的键值对其实是一个pair类型的数据？！==

---
**练习**

![](https://i.imgur.com/Wwnbqek.png)
![](https://i.imgur.com/X6tj9hz.png)

---

### 3. 关联容器操作

![](https://i.imgur.com/pwsxz53.png)
> **这里map和python的不一样！！！value_type是pair的类型！！！mapped_type才是值类型！！！**

---

#### 3.1 关联容器迭代器

![](https://i.imgur.com/it1DkOV.png)
![](https://i.imgur.com/fSXgPlH.png)

> 知识点1：P295有的类型，关联容器皆支持
> 
> 知识点2：关联容器额外的类型别名：
> 
> 1. key_type：表示此容器类型的关键字类型
> 
> 2. mapped_type：每个关键字关联的类型，只适用于map
> 
> 3. value_type：对于set，与key_type相同，对于map，为pair<const key_type，mapped_type>
> 
> 知识点3：当解引用一个关联容器的迭代器时，我们会得到一个类型为容器的value_type的值的引用
> 
> 知识点4：map的value_type是一个pair，我们可以改变pair的值，但是不能改变关键字成员的值！

---

#### 3.2 添加元素 - insert（注意返回类型为 {pair<T1, T2>, bool}）

![](https://i.imgur.com/RXOTYnI.png)
![](https://i.imgur.com/GCYz0oR.png)
![](https://i.imgur.com/nVYhwH9.png)
![](https://i.imgur.com/gXE34ew.png)

> 知识点1：向map中添加元素，元素的类型必须是pair，但是并没有现成的pair元素，所以需要在insert()的参数列表中创建一个pair
> 
> 知识点2：insert的返回值依赖于容器类型与元素类型，对于不包含重复关键字的容器，insert返回一个pair，第一个成员是指向具有指定关键字的元素，第二个元素是一个bool值，指出插入元素是已经在元素中还是插入成功，若关键词已在容器中，insert什么都不做，第二个参数变为false
> 
> 知识点3：递增计数器：ret.first是pair的第一个成员，是map的迭代器，->之后，解引用迭代器，提取的是map中的元素，也就是一个pair，找到此元素的第二个值，进行递增操作

---

#### 3.3 删除元素 - erase(返回类型：删除掉的元素数量 size_t)

![](https://i.imgur.com/Vv81OBD.png)
![](https://i.imgur.com/58bLTDO.png)

---

#### 3.4 下标操作

![](https://i.imgur.com/Y0bWzF9.png)
> 以键为下标！

---

#### 3.5 访问元素

![](https://i.imgur.com/kRAH1Hm.png)
![](https://i.imgur.com/9juOGKJ.png)

> 以键为准访问！

![](https://i.imgur.com/Wd39XqH.png)
![](https://i.imgur.com/0qIrcXn.png)

> 要找某个键对应长度（在multiset或者multimap），可以使用夹比方法，找lower_bound 和 upper_bound。

![](https://i.imgur.com/MTPhPV1.png)
![](https://i.imgur.com/I9yWLNZ.png)

> 知识点1：lower_bound和upper_bound找不到关键词的话，会指向一个不影响排序的关键词插入位置
> 
> 知识点2：equal_range()函数，接收一个关键字，返回一个迭代器pair，若关键字存在，第一个迭代器指向该关键字匹配的元素，第二个关键字指向最后一个元素匹配之后的位置，如关键字不存在，两个迭代器都指向关键字可以插入的位置

---
### 4. 无序容器

![](https://i.imgur.com/llTe0R6.png)
![](https://i.imgur.com/pRirtwi.png)
> 使用hash函数后，有的东西会被映射到一个空间（称作桶），上面的接口就是拿来管理桶的！

![](https://i.imgur.com/IruajE5.png)

> 知识点1：
> 
> 无序版本优势：当容器中key没有明显的顺序关系时更有用,且不需要耗费多余的时间来维护容器中的key序列
> 有序版本优势：当容器中key有明显的顺序关系时更有用,且我们不需要考虑排序问题,容器自动维护序列(字典序)
> 知识点2：map的key_value是有序的，set本身就是有序的，有序容器的操作可以用于无序容器
> 
> 知识点3：无序容器访问元素时，首先计算元素的哈希值，它指出应该搜索那个桶，因为容器讲哈细致相同的元素放在一个桶中，注意表11.8的各项操作

---
### 小结

![](https://i.imgur.com/mo1lBMM.png)

---

## 第十二章：动态内存（重要）

![](https://i.imgur.com/HAxwTEM.png)

### 1. 动态内存与智能指针（重要！注意智能与自定的差别！）

![](https://i.imgur.com/CSH5Z3z.png)
![](https://i.imgur.com/SHScLNP.png)

---
#### 1.1 shared_ptr 类

![](https://i.imgur.com/3CCC8aU.png)
![](https://i.imgur.com/T7UzgdA.png)

:::success
#### 初始化
![](https://i.imgur.com/eSEKIRu.png)

> 诚然，我们也可以用auto来初始化一个shared_ptr.
```
auto p1 = make_shared<int>(42);
```
:::

:::success
#### 拷贝 & 赋值

![](https://i.imgur.com/AZ3XtOO.png)
![](https://i.imgur.com/F4SdhiF.png)
![](https://i.imgur.com/eScp1DK.png)
![](https://i.imgur.com/0hZjJIH.png)
![](https://i.imgur.com/fadBlBf.png)

:::

:::info
#### shared_ptr使用：自定义StrBlob类
![](https://i.imgur.com/WLpkLnz.png)
![](https://i.imgur.com/yv57AIB.png)
:::

---
#### 1.2 直接管理内存

![](https://i.imgur.com/nuZrQJB.png)

:::success
#### 分配动态内存
![](https://i.imgur.com/c3TnF6u.png)
![](https://i.imgur.com/5NgbW81.png)
![](https://i.imgur.com/cPuL6L0.png)
![](https://i.imgur.com/pZzk6Fk.png)
![](https://i.imgur.com/zmexuAl.png)
![](https://i.imgur.com/ugYtqKg.png)
![](https://i.imgur.com/GPu9kyi.png)

:::

:::info
#### 释放动态内存
![](https://i.imgur.com/jOBrqAC.png)
![](https://i.imgur.com/GgDEx4T.png)
![](https://i.imgur.com/wbYUg1i.png)
![](https://i.imgur.com/R3y1TBy.png)

:::

![](https://i.imgur.com/Q1lSlJa.png)
![](https://i.imgur.com/SuSuBdX.png)
![](https://i.imgur.com/0IYtURW.png)

> 知识点1：new和delete相对于智能指针来说非常容易出错，最好使用智能指针来管理动态内存
> 
> 知识点2：在自由空间分配的内存是无名的，因此new无法为其分配对象命名，而是返回指向该对象的指针
> 
> 知识点3：默认情况下，new分配的对象是默认初始化的，这就说明内置类型或者组合类型将是为定义的(l例如：int，会指向一个为初始化的int)，类类型对象将用默认构造函数进行初始化(例如string，会产生一个空string)。
> 
> 知识点4：建议使用值初始化(在最后加一对小括号即可)，值初始化的内置类型对象有着良好定义的值
> 
> 知识点5：auto只有单一参数时可以使用，可以使用new分配const对象
> 
> 知识点6：delete完成两个操作：销毁给定指针所指向的对象，释放对应的内存，delete的参数必须是指向动态分配的对象或是一个空指针。
> 
> 知识点7：内置类型指针管理的动态内存在被显式的释放前一直都会存在，因为内置类型与类类型不同，虽然内置类型的指针会在离开作用域后被销毁，但是其内存依然存在
> 
> 知识点8：同一块内存释放两次：两个内置类型的指针指向同一块自由空间分配的内存，在对一个指针进行delete之后，其指向的内存也会被释放，若再对第二个指针进行delete，会造成自由空间破坏
> 
> 知识点9：忘记使用delete，使用已经释放掉的对象都是经常发生的(使用new和delete时)，所以尽可能的使用智能指针
> 
> 知识点10：在很多机器上，即使delete了某个内置类型的指针(也就是说释放了对应的内存空间)，虽然指针已经无效，但是其仍然保留这释放空间的对应地址，变成了空悬指针，也就是说我们要保留指针，可以将其置为空指针

---
:::warning
**练习**
![](https://i.imgur.com/f0MAz4m.png)

12.6
![](https://i.imgur.com/DmOqOHY.png)

12.7
![](https://i.imgur.com/NNjLS4o.png)

:::

---
#### 1.3 shared_ptr与new同时使用

![](https://i.imgur.com/0uPuXVu.png)
![](https://i.imgur.com/b1POsp3.png)
![](https://i.imgur.com/E9sipyr.png)
![](https://i.imgur.com/t4J0jEi.png)
![](https://i.imgur.com/hop0T6M.png)
> 也就是说，`int * `和 `shared_ptr<int>` 完全不能混用！我最开始就混用报错了！

![](https://i.imgur.com/r0z0UCO.png)
    
> 不要用同类进行初始化！（常识吧）
> 这里的get()函数还是很不错的！可以返回智能指针同地址的内置指针，方便用于一些函数。
    
![](https://i.imgur.com/IlNt9m2.png)

---
:::warning
**练习**
![](https://i.imgur.com/kfYSUAw.png)

> 12.11:利用P的get()函数得到的内置指针来初始化一个临时的智能指针，一旦调用结束，该临时指针被销毁，内置指针所指对象内存会被释放，使得p变为空悬指针

> 12.12:
> 
> p为普通的内置指针指向一个动态内存，sp为智能指针指向一个动态内存
> 
> (a)：合法，处理sp指针所指向内容，赋值的方式传递参数，处理完毕后内存不会被释放
> 
> (b)：不合法，参数必须是智能指针int类型
> 
> (c)：同上
> 
> (d)：合法，处理完毕后内存会被释放

> 12.13:在p被销毁时候，sp就变成空悬指针了；在后面sp自动销毁时就会产生 double free 的二次销毁错误。
:::

---
#### 1.4 指针智能和异常

![](https://i.imgur.com/ipMF4jh.png)
- 也就是说，不管怎么看用智能指针都更安全！但是平常大家都在说new，为什么呢？

![](https://i.imgur.com/kkAFE1F.png)

---
#### 1.5 unique_ptr

![](https://i.imgur.com/UNTvI6D.png)
![](https://i.imgur.com/nIlJCdX.png)
![](https://i.imgur.com/opHbUZ6.png)
![](https://i.imgur.com/jVTto7y.png)

---

#### 1.6 weak_ptr

![](https://i.imgur.com/4eqM56n.png)
![](https://i.imgur.com/CXK9Wg7.png)

---

### 2. 动态数组

![](https://i.imgur.com/y70pLHr.png)

- 也就是说，标准库安全又方便，那为什么我们需要动态数组之类的呢？

:::success
#### 1.初始化
![](https://i.imgur.com/Ak2BfcO.png)
![](https://i.imgur.com/NfhFKx9.png)

- ==**极度重要：我们说的动态数组并不是真正意义上的数组；它只是给我们返回的绑定于第一位的指针！（所以，对于动态数组使用begin(), end()以及范围for操作都是错误的！）**==
- 
![](https://i.imgur.com/rl7Khcm.png)
![](https://i.imgur.com/X4GUmWe.png)

:::

:::info
#### 2.释放
![](https://i.imgur.com/OOBJcsk.png)

:::

---
#### 3.智能指针与动态数组
![](https://i.imgur.com/edT5H3N.png)
![](https://i.imgur.com/KM5ro8O.png)

---

:::success
#### 4.allocator类
![](https://i.imgur.com/cc3Yh26.png)
![](https://i.imgur.com/PE5oZuN.png)
![](https://i.imgur.com/SspGNVz.png)
![](https://i.imgur.com/brRJk1K.png)
![](https://i.imgur.com/kzciNT6.png)
![](https://i.imgur.com/461giTu.png)

- 知识点1：new内存分配与对象构造组合在了一起，而allocator将内存分配和对象构造分离，它提供一种类型感知的内存分配方法，它分配的内存时原始的、未构造的
- 知识点2：利用allocator分配内存之后。必须在经过construct进行构造对象，两个参数分别为：创建对象位置的指针，元素类型的值
- 知识点3：对构造后的元素进行destory操作，销毁对象，deallocator

![](https://i.imgur.com/UjzUsuj.png)

:::

---
### 练习：12.3 使用标准库-文本查询程序
![](https://i.imgur.com/GfI7CHc.png)
![](https://i.imgur.com/FwGu2n1.png)

---
### 小结
![](https://i.imgur.com/NQ7dEN7.png)

---
# 第三部分：类设计者工具

## 第十三章：拷贝控制

本章内容：类定义构造函数，用来控制初始化对象时的行为；控制类对象的拷贝，赋值，移动以及销毁行为。

### 1. 拷贝，赋值与销毁

#### 拷贝构造函数

如果一个构造函数的第一个参数是自身类型的引用，且任何的额外参数都有默认值，那么这个函数就是一个拷贝构造函数。
```
class Foo {
public:
    Foo();  // 默认构造函数
    Foo(const &Foo); // 拷贝构造函数
    // ...
};
```

> 为什么参数必须是引用？ 因为拷贝构造函数被用来初始化非引用类型参数！

**合成拷贝构造函数**：如果没有在类内定义拷贝构造函数，则编译器会为我们定义一个！

**拷贝初始化**：现在我们就可以理解到什么是直接初始化，什么是拷贝初始化了！
- 直接初始化：编译器使用函数匹配选择提供参数的最合适的函数；
- 拷贝初始化：编译器将右侧运算对象拷贝到正在创建的对象中。

```
string dots(10, ',');           // 直接初始化
string s(dots);                 // 直接初始化
string s2 = dots;               // 拷贝初始化
string null_book = '9-99-9999'  // 拷贝初始化
string nines = string(100, '9') // 拷贝初始化
```

> 通常，拷贝初始化在我们使用 = 时候发生；但在下面情况也会发生：
> 1. 将一个对象作为实参传给非引用类型的形参；
> 2. 从一个返回类型为非引用类型的函数返回一个对象（拷贝后销毁！）；
> 3. 用花括号初始化数组元素或聚合类的成员。


> 知识点1：拷贝构造函数：本身是一个构造函数，其参数是一个自身类类型的引用，且任何额外参数皆有默认值
> 
> 知识点2：每个成员的类型决定了它的拷贝方式，对于类类型，将调用其拷贝构造函数进行拷贝，对于内置类型，则会直接拷贝，对于数组的拷贝是逐个元素的拷贝，若数组的元素是类类型，则使用拷贝构造函数来拷贝
> 
> 知识点3：直接初始化：一对小括号加参数。拷贝初始化：等号右侧对象拷贝到正在创建的对象中，如果需要还需进行类型转换(拷贝初始化没有=号的情况：将一个对象作为实参传递给一个非引用类型的形参时、从一个返回类型非引用类型的函数返回一个对象、用花括号初始化列表初始化一个数组的元素或一个聚合类的成员)
> 
> 知识点4：函数的调用中，非引用类型的参数都要进行拷贝初始化。非引用类型的返回值也会被用来初始化调用方的结果

---

#### 拷贝赋值运算符

重载运算符：operator+运算符号

重载赋值运算符： operator = (运算对象)

拷贝复制运算符，其实就是一个名为 operator= 的函数(operator后加表示要定义的运算符的符号)，重载运算符，有返回类型和参数，返回类型通常是左侧运算符的引用

若在类内未显式定义，则编译器会自动生成合成拷贝赋值运算符，它主要是将运算符右侧的所有非static成员赋给左侧元算对象对应成员(或是用来禁止该类型对象的赋值)

---

#### 析构函数

**构造函数**：初始化对象的非static成员；
**析构函数**：销毁对象的非static成员，释放对象使用的资源。

> 析构函数：是成员函数，由波浪号接类名组成！无返回值，不接受参数（所以==无法重载，也因此，一个类只能有一个析构函数！==）。==本身并不直接销毁成员！而是在析构函数体运行完之后，成员被自动销毁！！！==

```
class Foo {
public:
    ~Foo(); // 析构函数！
};
```

==注意，用析构函数隐式销毁一个内置指针的成员时，不会delete指针所指向的对象！！！==

> 析构函数无需调用，它会在对象被销毁时自动调用，因此无需担心何时释放资源！

**合成析构函数**：当一个类未自己定义析构函数时，编译器会定义一个合成析构函数（函数体为空的）！其实就和上面的Foo的例子是一样的！

---

#### 使用=default

```
class sales_data {
public:
    sales_data() = default;
    sales_data(const sales_data&) = default;
    sales_data& operator = (const sales_data &);
    ~sales_data() = default;
};
```

它有什么用呢？它就是通过=default这一要求来**显式地生成合成构造/析构函数**！

---

#### 阻止拷贝

> 首先，大多数类都应该定义默认构造函数，拷贝构造函数与拷贝赋值运算符，无论显式还是隐式！但有的情况下，这些操作没有什么意义（比如，iostream类是不能拷贝的，我们需要阻止其发生拷贝！）
> 解决方法：定义 ==删除的函数==！
> **删除的函数**：声明了它但是不希望在任何情况下使用它，在函数末尾添加 = delete 以表示定义删除的函数！

```
struct NoCopy {
public:
    NoCopy() = default;                            // 使用合成构造函数
    NoCopy(const NoCopy&) = delete;                // 声明删除的函数，阻止拷贝！
    NoCopy &operator = (const NoCopy&) = delete;   // 声明是删除的函数，阻止赋值！
    ~NoCopy() = default                            // 使用合成析构函数
    // ...  其他成员
};
```

=='= delete'==:通知编译器我们不希望使用该成员！！！，==必须出现在函数第一次声明的时候！==

==注意：析构函数不能是删除的函数！！！！==，**因为如果析构函数被删除，就无法释放该类的对象了**！！！

* 在新标准之前，我们希望避免拷贝和赋值时候是将拷贝初始函数和拷贝赋值符给放在private里的，在新标准之后，我们才可以使用 = delete来阻止拷贝！

```
class PrivateCopy {
    PrivetaCopy(const PrivateCopy&);
    PrivateCopy &operator = (const &PrivateCopy);  // 这两个都是private的！所以用户无法访问！
    
public:
    PrivateCopy() = default;
    ~PrivateCopy();
};
```

---
### 2. 拷贝控制和资源管理

**类型对象的拷贝语义**：在拷贝一个类的对象时，我们可以定义拷贝操作使类的行为像一个值或一个指针。
- 像一个值：也就是深层拷贝，拷贝出来的副本和原对象完全独立，改变一者不会影响到另一者（python的copy(deep=True));   ---> eg. **标准库容器**
- 像一个指针：浅层拷贝，即拷贝出来的副本与原对象共享底层数据，任何一者的改变会影响到另一者！（copy(deep=False)).  ---> eg. **shared_ptr**

#### 一. 行为像值的类

需要一个拷贝构造函数与析构函数，拷贝赋值运算符来==拷贝指针成员的地址==！

#### 二. 行为像指针的类

需要拷贝构造函数和拷贝赋值运算符来==拷贝指针本身==！ 
> （建议看书上的代码，就是拷贝的时候我们选择拷贝地址或者直接拷贝指针本身的差别，因为指针的话就可以让它们同时指向原地址，所以行为就是指针！）（见书p456）

---
### 3. 交换操作 - swap

回想我们之前常用的交换操作，如下面：交换i1和i2

```
HasPtr i1, i2;
HasPtr temp; // 用于当交换的中间量
temp = i1;
i1 = i2;
i2 = temp;
```
但是实际上，在讲究效率的C++里，比起创建临时量交换，交换指针肯定是更好的！！！

```
string temp = i1.ps;
i1.ps = i2.ps;
i2.ps = temp;
```

对于无默认类内swap函数，我们可以自定义：

```
class HasPtr {
    friend void swap(HasPtr&, HasPtr&);
};

void swap(HasPtr &s1, HasPtr &s2) {
    using std::swp   // 这一步什么意思？因为这个HasPtr类没有默认swap函数，所以我们调用std库的！
    swap(s1.ps, s2.ps);
    swap(s1.i, s2.i);
}
```

实际上一看，python写法和这个很相似：对于自定义类内没有的简单函数，我们就选择定义重名函数，然后对于里面的基本类成员调用该函数就是！只是C++这里需要先声明我要调用基本类的成员函数！python代码：

```
class HasPtr:
    def __init__(ps, i):
        self.ps = ps
        self.i = i
        
    def print():
        return print(self.ps, self.i)  // 就这个意思！
```

---
### 4. 拷贝控制实例！

具体见书p460！

```

// Message 部分
class Message {
    friend class Folder; // folder设置成友元方便访问private;
    friend void swap(Message &lhs, Message &rhs);
public:
    explicit Message(const string &str = ""): contents(str) {} // folder隐式初始化为空集合；
    Message(const Message&);  // 拷贝构造函数
    Message& operator = (const Message&);  // 拷贝赋值运算符
    ~Message() { remove_from_folders(); };  // 析构函数 - 调用下面的公共函数

    // 从给定的folder中添加/删除副本
    void save(Folder&);
    void remove(Folder&);

private:
    string contents;  // 消息文本
    set<Folder*> folders;  // 包含该message的folder的指针集合

    // 拷贝构造函数，拷贝赋值运算符和析构函数会使用的工具函数
    void add_to_folders(const Message&);
    void remove_from_folders();
};

void Message::save(Folder &f) {
    folders.insert(&f);
    f.addMsg(*this);
}

void Message::remove(Folder &f) {
    folders.erase(&f);
    f.remMsg(*this);
}

void Message::add_to_folders(const Message &msg) {
    for (auto f:msg.folders)
        f -> addMsg(*this);
}

Message::Message(const Message &msg): contents(msg.contents), folders(msg.folders) 
{
    add_to_folders(msg);
}

void Message::remove_from_folders()  // 析构的公共函数
{
    for (auto f: folders) {
        f -> remMsg(*this);
    }
}

Message& Message::operator=(const Message &msg) {
    remove_from_folders();
    contents = msg.contents;
    folders = msg.folders;
    add_to_folders(msg);
    return *this;
}

void swap(Message &lhs, Message &rhs) {
    using std::swap;
    
    for (auto f:lhs.folders)  
        f -> remMsg(lhs);
    for (auto f:rhs.folders)
        f -> remMsg(rhs);
    
    // 交换
    swap(lhs.folders, rhs.folders);
    swap(lhs.contents, rhs.contents);

    for (auto f:lhs.folders)
        f -> addMsg(lhs);
    for (auto f:rhs.folders)
        f -> addMsg(rhs);
}

// Folder 部分

class Folder {
    friend class Message;
    friend void swap(Message &lhs, Message &rhs);
public:
    Folder() = default;
    ~Folder();
    Folder& operator = (const Folder&);
    Folder(const Folder&);
    void addMsg(Message&);
    void remMsg(Message&);

private:
    set<Message*> messages;

};


void Folder::addMsg(Message &msg) {
    messages.insert(&msg);
}

void Folder::remMsg(Message &msg) {
    messages.erase(&msg);
}
```

---
### 5. 动态内存管理类(好麻烦，没看)

---
### 6. 对象移动

在前面我们有提到过，比起建立中间量然后三次拷贝后删除中间量，我们直接对两个变量进行移动操作是更优秀的！

诚然，在旧C++标准中，没有直接的方法移动对象，也就是说，在对象较大的情况下，拷贝的代价是很高的！

但是，新标准下，移动操作成为了可能。

#### 一. 右值引用

为了支持移动操作，右值引用被新标准引入。 右值引用即必须绑定到右值的引用！通过 `&&`来实现。

:::danger
右值引用的重要性质：只能绑定到一个将要销毁的对象！

左值和右值都是针对表达式而言的，左值是指表达式结束后依然存在的持久对象，右值是指表达式结束时就不再存在的临时对象。一个区分左值与右值的便捷方法是：看能不能对表达式取地址，如果能，则为左值，否则为右值。

![](https://i.imgur.com/DsfWj0a.png)

:::

由于右值引用只能绑定临时对象，则：
- 所引用对象即将被销毁；
- 该对象无其他用户。
**也就是说，使用右值引用的代码可以自由接管引用对象的全部资源！且不能将右值引用绑定到变量上！！**

#### 将左值转换成右值引用：std::move()

```
int &&rr1 = 42;
int &&rr3 = std::move(rr1);
```
这里将rr1这一左值，作为右值引用赋给了rr3.这也就默认了：==在今后除了对rr1赋值或销毁之外，不再使用！（因为它以及变成了右值引用）==
> 注意，一定要是使用std::move()而不是move()以避免名字冲突！

---

#### 移动构造函数和移动赋值运算符

为了让自己定义的类也支持移动和拷贝，我们需要在**拷贝构造函数与拷贝赋值运算符**之外，也定义==移动构造函数与移动赋值运算符==！

**==移动构造函数==**：类似拷贝构造函数，第一个参数为该类对象的一个引用，但是是==右值引用==！任何额外的参数都必须有默认实参。且，在移动完对象后，应保证移动后即便销毁源对象也是无害的！举例代码：

```
StrVec::StrVec(StrVec &&v) noexcept // 不应抛出任何异常
    ：elements(v.elements), first_free(v.first_free), cap(v.cap)
{
    v.elements = v.first_free = v.cap = nullptr; // 保证源对象销毁了也无害！！！这样就算销毁源对象也不会有安全危害。
}
```
> 上面的移动构造函数就相当于对源对象进行了移动，移动前的对象以及可以无安全危害销毁。
> 上面的 noexcept 是新标准中，承诺函数不抛出任何异常的方法。

![](https://i.imgur.com/HsShZrL.png)

==**移动赋值运算符**==：执行与移动构造函数相同工作；若不希望抛出任何异常，需要声明 `noexcept`.

```
StrVec& StrVec::operator = (StrVec &&rhs) noexcept
{
    if (this != &rhs){     // 检测自赋值
        free();            // 释放已有元素
        
        // 从rhs接管资源
        elements = rhs.elements;
        first_free = rhs.first_free;
        cap = rhs.cap;
        
        // 将rhs置于可析构状态
        rhs.elements = rhs.first_free = rhs.cap = nullptr;
    }
    return *this;
}
```

- 同上，要求移动后源对象必须可以析构！！！

---
#### 合成移动构造函数与移动赋值运算符

同拷贝，移动的这两个成员也是可以在未定义基础上，编译器自动合成的！
==但是！如果一个类已经定义了拷贝构造函数，拷贝赋值运算符或析构函数，那么编译器就不会合成移动构造函数与移动赋值运算符了==，且在这个时候，**编译器会使用拷贝来替代移动操作**！

:::info
只有当一个类没有定义任何版本的拷贝控制成员，且类的所有非static成员都可以移动时，编译器才会合成移动构造函数与移动赋值运算符。

```
struct X {
    int i;
    string s;          // 内置类型可移动
}；

struct HasX {
    X mem;             // X有合成的移动操作
};

X x, x2 = std::move(x);
hasX hax, hax2 = std::move(hax);    // 可以实行！
```

![](https://i.imgur.com/99An7Ux.png)
> 移动操作永远不会隐式定义为删除的函数！==但是如果你简单用 = default 让编译器生成移动成员，且编译器无法移动所有成员，或析构函数被定义为被删除的，这时编译器就会将移动操作定义为删除的函数！==
:::

:::danger
移动与拷贝的使用分别：移动右值，拷贝左值，但若没有移动构造函数，右值也被拷贝！
:::

---

#### 右值引用与成员函数

除去构造函数与赋值运算符，如果一个成员函数也可以同时提供拷贝与移动两个版本，那么操作空间就会大很多。

**拷贝版本**：接受指向const的左值引用；
**移动版本**：接受指向非const的右值引用。

如：push_back()
```
void push_back(const X&);   // 拷贝版本，仅接受const的左值引用 
void push_back(x&&);        // 移动版本，接受非const的右值引用 ---> 这个版本会从实参中窃取数据！！！
```
![](https://i.imgur.com/Rzjhow0.png)

![](https://i.imgur.com/EDzOKyZ.png)

---

## 第十四章：重载运算与类型转换

本章内容：重载常用运算符，指定新含义；自定义类型间的转换规则。

### 1. 基本概念

==重载运算符==： operator + 需要定义的符号！ （有返回类型，参数列表，函数体）

![](https://i.imgur.com/oK4gH5c.png)
![](https://i.imgur.com/2kpDFsn.png)

**调用重载运算符**：就和平常使用一样！或者加上一个operator!

![](https://i.imgur.com/UbM3A7D.png)
![](https://i.imgur.com/EkKPnNT.png)
> 当我们把运算符定义成成员函数时，它的左侧运算对象必须是运算符所属类的一个对象：

```
string s = "hello";
string t = s + " world!"; // 这里的+是重载的成员函数，正确
string n = "hi " + s;  // 错误!重载符左侧运算对象必须是所属类的一个对象！

''''''
why?

如果 operator+ 是string的成员，那么我们的第一个加运算符等价于：s.operator+("World"),没问题；
但是第二个加运算符为： "hi".operator+(s)，左对象为const char*，内置类型根本没有成员函数，报错！
```

---

### 2. 输入/输出运算符

```
>> 输入
<< 输出
```
这两个运算符在IO库都有定义内置版本，而类则需定义适合对象的新版本！

#### 2.1 重载输出运算符

> 输出运算符的第一个形参，通常为非常量 ostream 的引用(ostream &os).为什么是引用？因为ostream对象是不能拷贝的！
> 而第二个形参通常为对象类的常量引用以输出对象信息。引用是因为我们希望避免复制浪费，常量是因为打印对象通常不需要改变对象内容！

为了与其他输出运算符保持一致， `operator<<`返回类型为`ostream&`.

举个例子，定义个sales_data的输出运算符（之前试过）

```
ostream &operator<<(ostream &os, sales_data &data)
{
    os << data.isbn() << ' ' << data.unit_sold << '~';
    return os;
}
```

> 注意：输出运算符尽量减少格式化操作！（endl, ends之类！）因为我们自定义输出运算符是为了控制打印的对象！

![](https://i.imgur.com/pxcSMaR.png)

:::danger
！！！！ 注意！！！！ ==输入输出运算符必须是非成员函数==！

成员函数： 类的**成员函数都是以具体对象为基础驱动**， 如sale_data.isbn();
但是cout, cin是以输入输出流对象为基础的！！如果你定义成成员函数，就会出现：
```
sales_data.operator<<()
也就是：
sales_data << cout
了！这样是明显错误的！
```

> 同时，需要注意，我们使用输入输出运算符肯定是要读取类对象的成员的。考虑到基本成员都是私有(private)的，输入输出运算符又不是成员函数，==我们需要将其定义为友元==！
:::

---

#### 2.2 重载输入运算符

输入运算符：第一个形参是`istream&`,第二个形参是对应类对象的==非常量引用==！（为什么？因为我们的目的就是把东西写入，你设成常量还说个🔨？）举例：

```
istream &operator>>(istream &is, sales_data &data)
{
    double price;
    is >> data.no >> data.unit_sold >> price;
    
    if (is) // 检查是否输入成功！
        data.revenue = data.unit_sold * price;
    else
        data = sales_data(); // 输入失败则默认为空状态
    return is;
}

```

![](https://i.imgur.com/RMiMhMr.png)

---

### 3. 算术和关系运算符 & 赋值运算符

十分简单基础，省略！有问题就看书！

---

### 4. 下标运算符operator[]

:::warning
下标运算符必须是成员函数！！！
:::

![](https://i.imgur.com/xxNbhSV.png)
![](https://i.imgur.com/rDAqLUV.png)

---

### 5. 递增/递减运算符++/-- & 成员访问运算符->/*

简短，同样的需要用看书就好！

---

### 6. 函数调用运算符！

什么是调用运算符？看下面的例子。

```
struct AbsInt
{
    int operator() (int val) const {
        return val < 0? -val:val;
    }
}

'''
使用：

int i = 42;
AbsInt AbsOperator;
int ui = AbsOperator(i); // 调用运算！
```

> 从简单理解角度而言，我们允许一个类的对象以函数的形式被调用，称这个类的对象为==函数对象==，它们的行为就像函数一样！
> 当然，这种类除了函数调用运算符以外，也可以包含其他成员的！！！

:::success
到这里我总感觉函数调用运算符用法在前面很眼熟！一想，哦原来是lambda还有bind！

结论：lambda也是函数对象！！！

但是，记得lambda的捕获机制吗？值捕获和引用捕获！
当时不清楚的东西，现在就可以好好说明了！
所谓的引用捕获就是直接利用引用，无需生成新的数据；
**而值捕获的本质就是，捕获值然后利用它初始化一个lambda对象和对应的数据成员！**
:::

---

#### 标准库定义的函数对象！

![](https://i.imgur.com/iyNFJ6X.png)
![](https://i.imgur.com/oEtseo4.png)

---

#### 可调用对象与function

C++中可调用对象：
- 函数
- 函数指针
- lambda表达式
- bind创建的对象
- 有重载函数调用符的类

虽然大家类型不同，但调用形式却完全可以相同。比如上面的所有都可以是 `int xx(int i1, int i2)`表示调用2个整数，返回一个整数（废话！）

> 那么，在实际任务中，什么时候在哪里调用谁，要怎么更好区分呢？
> 为此我们引入：
> - ==函数表==：存储指向这些调用对象的指针！当程序需要对应操作时，在表中查找该调用的对象就好。（map实现）


```
eg.

map<string, int (*)(int, int)> function_map;
function_map.insert({"+", add})； // 存储对应的加运算符和add函数的指针了！

但是：
function_map.insert({"-", mod});  // 报错！因为mod实际是一个lambda表达式不是函数指针！
```
所以要怎么解决这种报错呢？下面我们引入新内容！

==标准库function类型==

![](https://i.imgur.com/PBt5QMx.png)
比如：
```
function<int(int,int)> --- 表示和上面一样的一个调用2个整数，返回一个整数的模板！
```

![](https://i.imgur.com/GXV2e8q.png)
> 虽然感觉没什么暖用但是可以直接用指针，函数对象，lambda来像这样新定义function还真蛮神奇！
> 并且有了这个我们可以再一次定义map！
```
map<string, function<int,(int, int)>> func_map;
```
==这样我们在添加内容时候就不需要顾虑添加的是上面五种的哪一种了！==（就这个作用啊？）

![](https://i.imgur.com/bysswsR.png)

:::danger
注意，重载的函数无法直接使用上面方法加入function_map因为function库理解不了你要加的是重载的哪一个。

解决方法：

存储一个函数指针，然后把指针加进去（这样不就不重名了吗！）
![](https://i.imgur.com/RGvrG66.png)
:::

---

### 7. 重载/类型转换与运算符

润了这一节，感觉没什么好看的啊，不如洗澡去。

---

## 第十五章：面向对象程序设计

内容：数据抽象，继承以及动态绑定

### 1. OOP概述（Object-oriented Programming)

OOP核心：抽象，继承与动态绑定。

- 抽象：将类的接口与实现分离；
- 继承：定义相似的类并对相似关系建模；
- 动态绑定：一定程度上忽略类的区别，以统一的方式使用它们的对象。

:::success
**继承**

派生类继承基类，基类成员在派生类都有，而派生类又根据自己特性具有不同成员。（就和细胞分化一个样子）

==虚函数（virtual function）==：基类希望有的函数，派生类能够各自定义适合的版本，就在==基类里声明成虚函数==给派生类操作空间！==有两种写法！==

```
class Quote 
{
public:
    string isbn() const;
    virtual net_price(size_t n) const; // 虚函数，可以用virtual写在基类表示，也可以像下面一样！
}
```

==如何定义派生类的基类？==：类派生列表！见下：
```
class Bulk_quote: public Quote        // :（public/private) 基类名， 用于定义基类
{
public:
    double net_price(size_t n) const override;   //  override 用于表明是虚函数!这样写就不用在基类写virtual。
}
```

> C++11 新标准：允许派生类通过override来显式地表达它用哪个成员函数改写了基类的虚函数。（加在形参列表后）
:::

:::info
**动态绑定**

举例：对于上面的Quote和Bulk_quote类，如果我们想用同一段代码可以同时处理它们，要怎么办？

**结论：形参写基类的引用就行.并且对应的，编译器会自动根据类选择应该使用的虚函数！**

![](https://i.imgur.com/UA1xyLh.png)
:::

---

### 2. 定义基类和派生类

这一节应该代码居多，可看VSCODE.

**1. 基类**
```
基类

class Quote {
public:
    Quote() = default;
    Quote(const string &book, double sales_price):
        bookNo(book), price(sales_price) {}
    string isbn() const { return bookNo; }
    virtual double net_price(size_t n) const { return n*price; } // 虚函数方便派生类自定义（动态绑定）
    virtual ~Quote() = default;  // 虚函数方便派生类自定义（动态绑定）
private:
    string bookNo;
protected:
    double price = 0.0;
};

```

![](https://i.imgur.com/WAh3WHs.png)

可以看到我们上面有个`protected`,它的作用是访问控制。

访问控制：基类希望只有派生类能访问该成员，则使用`protected`进行访问控制。

> `protected` 和`private`的不同:派生类可以访问`protected`对象； 即便是派生类，`private`对象也不能访问！
---

**2. 派生类**

```
class Bulk_quote:public Quote {
public:
    Bulk_quote() = default;
    Bulk_quote(string &bookNo, double price, size_t n, double discount):
            Quote(bookNo, price), min_quantity(n), discount(discount) {}  // 继承了bookNo, price!所以直接丢给基类构造函数。
    // 很像委托构造函数！
    double net_price(size_t) const override;
private:
    size_t min_quantity = 0;
    double discount = 0.0;
};
```
![](https://i.imgur.com/ZyU1Lmh.png)

> 派生类可以访问基类的 public 和 protected 成员！

```
double Bulk_quote::net_price(size_t n) const 
{
    if (n >= min_quantity) {
        return n * (1 - discount) * price;
    }
    else {
        return n * price;
    }
}
```

==静态成员==：若基类定义了静态成员，我们可以看作这个成员有唯一的实例，所有派生类都无法自定，只能遵从基类。但是，派生类一样是可以访问的（只要基类不将其定义为私有）。

==派生类在声明时不需要写基类，实际定义并且写代码时再写派生列表；但是一个类想要成为基类，那就必须定义而不能仅仅声明！==

派生的派生：和python学过的一样，一个基类也可以是派生类（多代继承嘛）

> 那么就有个问题，如果我不希望某个类被用作基类，我要怎么办？
> C++11新标准：在类名后跟一个final表示不能被用作基类！

```
class NoDerived final;  // 不能被用作基类！！！
```

---

**类型转换与继承**

![](https://i.imgur.com/1vcFzE8.png)
![](https://i.imgur.com/nrA4Cju.png)

> 不存在从基类向派生类的类型转换；派生类向基类转换只存在于指针或引用，对于类类型是无效的！（强扭的瓜不甜）
> 换句话说，将派生类给转换成基类（利用引用或指针）的本质就是对基类的拷贝构造初始化，仅保留基类成员，而派生类成员都被丢弃了！

> 知识点1：通常情况下，如果我们想使用指针或者引用绑定一个对象，则指针或者引用的类型需要和对象的类型一致或者可进行const转换，但是存在继承关系的类是一个重要的例外：我们可以将基类的指针或者引用绑定到派生类的对象上：这就意味着，**当我们使用基类的指针或者引用时，我们并不知道该指针或引用所绑定的对象的真实类型，该对象可能是基类的对象，也可能是派生类的对象**
> 
> 知识点2：当我们使用一个变量或者表达式时，我们需要将其静态类型和动态类型相互区分开，表达式的静态类型是在编译时已知的，是变量声明时的类型或者表达式生成的类型，其动态类型是变量或者表达式表示内存的对象的类型，知道运行时才可知，即如item对象，静态类型为Quote&，动态类型则依赖于item所绑定的实参，直到函数运行时才可知
> 
> 知识点3：**如果表达式既不是指针也不是引用，则其动态类型和静态类型会一直绑定在一起**
 
![](https://i.imgur.com/icQno7S.png)

---

### 3. 虚函数

对虚函数的调用可能是运行时才解析的（也就是说，非静态）

![](https://i.imgur.com/3XoD7mF.png)

虚函数的声明：（前面说过有2种）
1. 在基类中，声明前加个virtual;
2. 在派生类中，形参列表后加个override;
- 注意，所有虚函数的形参列表都得是一致的！

![](https://i.imgur.com/QspevC3.png)
![](https://i.imgur.com/30Q3m7Y.png)


对于我们上面说的，对类可以用`final`防止继承，实际上，对于函数也可以使用`final`防覆盖（也就是被作为虚函数）！

```
struct A
{
public:
    void f1() final;
};

struct B: A
{
public:
    void f1() override;  // 报错，final防止了函数覆盖！！！
};

```

---

### 4. 抽象基类

**纯虚函数**：当你的基类的虚函数完全只是为了给派生类一个形参模板（也就是你永远不会用上基类这个函数）时，可以将其定义为纯虚函数。
==定义方法==为：`func() = 0`;(只能是写在声明部分)

对于含有纯虚函数的基类，我们就称为 ==**抽象基类**==！因为它只负责定义接口，而不会去调用这个接口！
抽象基类特征：==不能够直接去创建一个抽象基类的对象！！！==

![](https://i.imgur.com/6gUxySn.png)

---

### 5. 访问控制与继承

**成员类型**：public(公开的-用户都可以访问)，private(私有的-用户无法直接访问(包括派生类)，可以通过友元调用)，protected(受保护的，派生类可以访问)

![](https://i.imgur.com/hdFI0iE.png)

> 我们在继承时，会选择 `:public/private 基类名`，这里的public/private指的是我们对基类的public成员设成私有还是公有！
> 比如，我们若设成private，则派生类无法访问基类的public成员，只能访问protected成员！

==派生类向基类转换的可访问性==：只有公有继承，才能够使用用户代码进行转换！但无论是公有还是私有继承，派生类的函数和友元都可以使用派生类向基类转换。

==友元与继承==：友元关系不具有继承性！！！
![](https://i.imgur.com/ZOkr59U.png)

> 但是，我们可以通过在派生类使用 using 语句来改变基类个别成员的可访问性！

![](https://i.imgur.com/6wXkgZh.png)

:::success
默认的继承保护级别：struct/class

就像创建类默认公有私有一样，在继承时，我们可以不在继承列表指定public/private而是直接使用struct/class来定义派生类的私有/公有继承。

![](https://i.imgur.com/qTECPp2.png)
> 但是，比起写struct/class而言，还是直接显式地public/private表示出来更好！
:::

---

### 6. 构造函数与拷贝控制

基类的析构函数应该设置成虚函数，因为我们在删除指针时不清楚指向对象是基类还是派生类；若是派生类，就会产生未定义行为！

![](https://i.imgur.com/b0UL3Lk.png)

>剩下有一些拷贝构造以及移动构造的东西。简单的说就是，基类一般而言需要定义，且派生类也需要定义，用类似委托构造的行为包含基类成员部分的构造，剩余部分再写进去就行。具体看书吧。

---

### 7. 容器与继承

有一个问题：如果我们想把基类和派生类的对象都装进同一个容器（比如`vector<Quote>`)，可行吗？
答案是显然的，对于上面的容器，我们即使装入派生类的Bulk_quote对象，编译器也会自动将其转化为基类！

![](https://i.imgur.com/8cCbjqb.png)

> 所以我们要如何解决这个问题呢？方法是：存放对象的智能指针！比如：

```
vector<shared_ptr<Quote>> basket;

basket.push_back(make_shared<Quote>("MyFirstLane", 50));
basket.push_back(make_shared<Bulk_quote>("MySecondLane", 50, "2 people"));

// 这样就可以实现！
```

---

## 第十六章：模板与泛型编程

### 1. 模板

C++和python不同，它是静态语言，也就是说绝大多数内容都是编译时就已经确定，而非运行时再进行动态编译。那么，也就会有下面这种问题。
> Python的话，比较大小的函数可以像下面一样写：
```
def compare(a, b):
    return a if a >= b else b
```
> 也就是说，由于其动态性，我们输入的a, b是未知的,可以是整数，浮点数甚至是单个字符。但是，C++要求函数在编译时就确定变量类型，那么这个时候，如果不希望使用重载，要如何实现？
- 答案是模板！

```
int compare(const string &i1, const string &i2) {
    if (i1 < i2) return -1;
    else return 1;
}

-----------------------
模板：

template <typenameT>
T compare(const T i1, const T i2) {
    if (i1 < i2) return i1;
    else return i2;
}
```

- 模板以关键字 `template` 开始，后面会跟上一个模板参数列表 `<typenameT>`，这个列表是用逗号分隔的一个或者多个模板参数的列表，用 <>包围。
- 注意，这里的模板参数列表不能为空！且类型参数可以用来指定形参类型和返回类型！

```
template <typename T> foo(T* p) {
    T tmp = *p;
    return tmp;
}
```

> 并且，如果参数列表里有类，那么需要单独用class写出来类。
```
template <typename T, class U> T calc(const T&, const U&);
```

> 不光是函数，类也可以通过前置 `template <typename T, class U>` 来实现模板化！

:::danger
后面的太抽象了，看着累的很，润了先 （220627）
:::

---

# 第四部分：高级主题

## 第十七章： 标准库特殊设施

### 1. tuple 类（不是Python的元组）

tuple是类似pair的模板！
- pair：每个pair有两个成员，成员类型不一定相同；
- tuple：每个tuple有任意数量成员，成员类型不一定相同。

如果希望将一些数据结合成一个数据结构，又不想去定义一个新的聚合类时，tuple很有用。tuple的操作如下：


| 成员 | 说明 | 
| -------- | -------- | 
| tuple<T1, T2, ... , Tn> t(v1, v2, ..., vn); | 创建一个长度为n，每个位置存放类型为Ti的对象（当然了，完全可以只声明不初始化让编译器默认初始化！）    |
| make_tuple(v1, v2, ..., vn); | 以给定的值返回一个tuple（编译器会自动推断类型！） |
| t1 != t2; t1 == t2; | 判断两个tuple对象是否相等 |
| `get<i>(t);` | 获得t里面第i个成员 |
- 和python的元组不同，C++的tuple是个==快速且随意==的数据结构！
- ==注意，tuple初始化需要直接初始化！（就是括号初始化），用等号进行拷贝初始化有时候报错！==
- 感觉上来说，make_tuple()进行的初始化比较简单欸！看下面！

```
int main()
{
    // 直接初始化
    tuple <int, int, char, string> t1(1, 1, 'c', "fun");
    cout << get<3>(t1);
    
    // make_tuple()初始化
    auto t2 = make_tuple(1, 2, 3, 'c', "char!");
}
```

---

### 2. bitset 类

bitset类定义在 `bitset` 头文件中，是用于位运算的数据类型。

==什么鬼东西。跳了！==

---

### 3. 正则表达式

#### 3.1 regex介绍
regex头文件：简单介绍！

![](https://i.imgur.com/AN2Bg6k.png)

![](https://i.imgur.com/PEXIrGz.png)
> 若整个输入与表达式匹配则regex_match 返回True；若表达式是输入的一个子序列则regex_search 返回True。

```
int main()
{
    string pattern("[^c]ei"); // 定义模式为：不在c之后的ei！
    pattern = "[[:alpha:]]" + pattern + "[[:alpha:]]*";
    regex r(pattern); // 定义查找模式的regex
    smatch results; // 定义smatch对象来保存结果
    string test_str("receipt freind theif receive");
    if (regex_search(test_str, results, r))
        cout << results.str() << endl;
}

---

freind
```
- 正则式 `[^c]`表明了我们希望查找c以外字母，而 `[^c]ei`表示希望查找c以外字母接ei的表达式。
- `[[:alpha:]]*`表示匹配任意字母！其中：
    - +号表示匹配零个或多个字母；
    - *号表示匹配一个或多个字母！
- 因此，`"[[:alpha:]]" + pattern + "[[:alpha:]]*"`表示：`[^c]ei`前面匹配一个字母，后面匹配零个或多个字母！
- 然后我们用这个表达式去初始化一个 regex 对象！名为results的smatch对象用于存储查找后匹配的内容！查到到则`regex_match`会返回True，且内容被存储于results。
- 但是看上面只返回了True！为什么？因为 `regex_match` 一旦查找到一个成立对象就会停止运行！

![](https://i.imgur.com/tYUKsJh.png)

举例：用正规表达式查找所有的C++文件！
```
int main()
{
    regex r("[[:alnum:]]+\\.(cpp|cxx|cc)$", regex::icase);
    smatch results;
    string filename;
    while (cin >> filename) {
        if (regex_search(filename, results, r)) {
            cout << results.str() << endl;
        }
    }
}
```
> 这个regex对象 r 表示了什么？  
-  首先，前面给了一个`[[:alnum:]]+`，也就是允许有一个或多个字母/数字！
-  一个.表示文件名后格式的点，但是作为转义处理，使用两个反斜线！
-  `(cpp|cxx|cc)`表示文件格式（或！），结合后面的`regex：：icase`表明了不判别大小写！
![](https://i.imgur.com/rV4Nnwi.png)
![](https://i.imgur.com/F8cOk6Q.png)

---

#### 3.2 匹配与Regex迭代器类型

上面我们可以查到：
1. 是否有匹配正规表达式；
2. 第一个匹配的正规表达式。

但是有个问题就是：如果有多个对应表达式，如何获得所有匹配？
这里要用到 `sregex_iterator` 来获得所有匹配。该类的方法如下。

![](https://i.imgur.com/hw2KrXJ.png)
![](https://i.imgur.com/QpXpQE6.png)
> 当使用 `sregex_iterator` 绑定string 和 regex正规式 对象时，迭代器会自动定位到string第一个匹配位置！也就是说 `sregex_iterator` 迭代器会自动调用 `regex_search`函数！解引用迭代器会得到最近一次搜索结果的smatch对象。当我们递增迭代器时，就会寻找string里的下一个匹配。
```
// 之前只能查找到freind的代码修改，可以查找到全部了！
int main()
{
    string pattern("[^c]ei"); // 定义模式为：不在c之后的ei！
    pattern = "[[:alpha:]]*" + pattern + "[[:alpha:]]*";    
    regex r(pattern, regex::icase);
    string file("receipt freind theif receive");

    for (sregex_iterator it(file.begin(), file.end(), r), end_it;
        it != end_it; ++it) {
            cout << it -> str() << endl;
        }
}

---
freind
theif
```
- 附上 smatch 对象的成员。
![](https://i.imgur.com/4AoApu1.png)

---
#### 3.3 使用regex_replace进行查找替换！

![](https://i.imgur.com/xcXwAla.png)



---

### 4. 随机数

random头文件：随机数引擎类与随机数分布类

- 引擎： 类型，生成随机的unsigned数列；
- 分布： 类型，使用引擎返回服从特定概率分布的随机数！

![](https://i.imgur.com/o9zXhHL.png)

#### 4.1 随机数引擎与分布

随机数引擎是函数对象类，含有一个调用运算符，该运算符不接受参数，返回一个随机的unsigned整数，如下：
```
int main()
{
    default_random_engine e; // 生成随机无符号数；
    for (size_t i = 0; i != 10; ++i) 
        cout << e() << ' ';
}


---
16807 282475249 1622650073 984943658 1144108930 470211272 101027544 1457850878 1458777923 2007237709   

```
![](https://i.imgur.com/IeJCO27.png)
> 和python的random很像！也是可以随机种子来确保不变性。
> 当然看到这个，会发现，这个引擎没法指定生成范围啊！也就是说这玩意大多数情况没啥用啊！咋办？

- 使用随机分布类：`uniform_int_distribution<unisigned>`

```
int main()
{
    uniform_int_distribution<unsigned> u(0, 102);
    default_random_engine e;
    for (size_t i = 0; i != 10; ++i) {
        cout << u(e) << ' '; 
    }
}

---
0 13 77 47 54 22 4 69 69 96

```
> 相当于我先使用分布确定了范围，再将这个范围带入引擎里，就可以得到一个有初期条件的随机数了！

- 但是都写到这里了你一定发现了一件事：每一次生成的数，实际上都是一样的！！！！
- 这确实是个问题，如果每次生成都一样的话，又怎么可以叫随机数呢？？
- 比如我们看下面例子：

```
int main(){
    default_random_engine e;
    uniform_int_distribution<unsigned> u(0, 9);\
    vector<int> ivec;
    for (size_t i = 0; i != 10; ++i) {
        ivec.push_back(u(e));
    }
    for (auto each : ivec) {
        cout << each << ' ';
    }
}


---
 哪怕调用100万次，结果也是：0 1 7 4 5 2 0 6 6 9   
```

:::success
正确的编写方法是：将引擎与分布都编写成static的！这样才能够都次调用都生成新的数！

```
int main(){
    static default_random_engine e;
    static uniform_int_distribution<unsigned> u(0, 9);\
    vector<int> ivec;
    for (size_t i = 0; i != 10; ++i) {
        ivec.push_back(u(e));
    }
    for (auto each : ivec) {
        cout << each << ' ';
    }
}
```
这样我们在多次调用的时候生成状态就不再都是初始了，而是从生成的序列里轮番取出！
![](https://i.imgur.com/nHEwNME.png)
:::

:::info
**随机数发生种子**：设置不同的种子会导致引擎生成不同的随机序列，所以当我们希望引擎一直生成同一随机序列时可以固定种子（再现性），相反若希望每次生成不同则可以换用别的种子。

常用的随机数生成种子选择方法： 头文件ctime中的time函数！

```
default_random_engine(time(0)) // 接受单个指针参数，指向用于写入时间的数据结构；若为空则简单地返回时间；
```

time返回以秒计的时间，所以保证了随机性！

![](https://i.imgur.com/RAO38PK.png)
:::

最后，生成浮点数的程序和上面基本完全相同，只有分布类 u()改变，比如： `uniform_real_distribution<double> u(0, 1)` 就是生成0~1的随机浮点数。

---

### 5. IO库再探

#### 5.1 格式化输入与输出：操纵符！



| 操纵符 | 作用 |
| -------- | -------- | 
| boolalpha     | 通常我们在打印bool值时，会是True=1， False=0.但是有时候我们希望打印出来的就是True和False。这种时候，boolalpha操纵符就有效了！| 
| oct/hex/dec | 修改输出整型的进制！ oct: 8进； hex: 16进； dec: 10进。另外可以在一开始使用`cout << showbase;`来表示进制表达方式，见下。 |
| cout.precision(num) | 修改输出浮点数的精确度为num位！在输出的上面写，和上面的`cout << showbase` 一样。 |
| cin >> noskipws | 选择输入不忽略中间的空格！同样放在输入前，且可以最后使用 `cin >> skipws` 还原状态！ |




```
1. boolalpha
int main()
{
    cout << "default bool values:" << true << ' ' << false
    << "\nalpha bool values" << boolalpha 
    << true << ' ' << false << endl;
}

--
default bool values:1 0       
alpha bool valuestrue false   


--------------
2. oct/hex/dec

int main()
{
    cout << showbase;  // 显示进制的表示方式。见下面
    cout << "default " << 20 << " for 20 and " << 1024 << " for 1024;"
    << "\nNow octal: " << oct << 20 << " for 20 and " << 1024 << " for 1024;"
    << "\nNow hex: " << hex << 20 << " for 20 and " << 1024 << " for 1024;"
    << "\nNow decimal: " << dec << 20 << " for 20 and " << 1024 << " for 1024;" << endl;
    cout << noshowbase;
}

---
default 20 for 20 and1024 for 1024;
Now octal: 024 for 20 and 02000 for 1024;
Now hex: 0x14 for 20 and 0x400 for 1024;
Now decimal: 20 for 20 and 1024 for 1024;

--------------
3. cout.precision()

int main()
{
    cout.precision(9);  // 设置精度！
    cout << "precision: " << cout.precision() 
    << ", value: " << sqrt(2.0) << endl;
}

---
precision: 9, value: 1.41421356


--------------
4. cin >> noskipws

// 忽略空格
int main(){
    char ch;
    while (cin >> ch) {
        cout << ch << ' ';
    }
}

---
ascds   sa
a s c d s s a 


// 不忽略空格

int main(){
    char ch;
    cin >> noskipws;
    while (cin >> ch) {
        cout << ch << ' ';
    }
}

---
cin no skip     white space
c i n   n o   s k i p           w h i t e   s p a c e 

```

- 上面4个是常用的！下面把基本列表放着，随时可以查阅！

![](https://i.imgur.com/n8NqXa1.png)
![](https://i.imgur.com/zhSBI5t.png)


---

## 第十八章：用于大型程序的工具

### 1. 异常处理！

#### 1.1 异常语句书写

```
Python：

try
    operation1
except exception ("Error message") // 这里对应C++的throw和catch！（Python放一起了！？）
    operation2
    
--------
C++：
try {
    语句...；
    if （条件）
        throw 错误类型("错误信息")； 
} catch (错误类型) {
    处理语句...
    //（下面这句可以有也可以没） 
    throw; // 意思是第一个catch处理了一部分后，把剩下部分丢给下一个catch语句！
} catch (错误类型) {
    处理语句...
}
```

前面已经简单说过异常处理，这里再详细看看！

---
#### 1.2 抛出异常

C++中通过 **抛出(throw)** 一条表达式 来**引发(raise)异常** 再进行**处理**。

- 当执行一个 throw 时，跟在throw后面的语句就不再执行，而是将控制权交给了catch模块！也就是，**当抛出异常后，程序立马停止暂停后续过程且开始寻找与该异常匹配的catch子句！**
- 由于这个特性，==throw 类似于 return 语句：通常作为条件语句的一部分或某个函数最后（或者唯一）的一条语句！==
- 当在try语句块中运行到throw时，编译器会**由内向外地**寻找对应的chtch语句！这个被称作==栈展开==！
- 如果发生异常且没有找到对应的catch语句，程序就会启动terminate语句退出（也就是报错啦！）

![](https://i.imgur.com/KVbisUX.png)

---
catch到对应语句后，编译器会使用异常抛出式对**异常对象**进行拷贝初始化！完成异常处理后，该对象会被销毁。
- 上面的话告诉我们，异常对象也就是我们抛出异常的表达式必须要有：==1. 拷贝构造函数； 2. 析构函数！==

---

#### 1.3 捕获异常

抛出异常类型与catch能捕获的异常类型需要精准匹配！

:::info
虽然基本没有这么好的事情，但是有时候如果有这么一条语句可以捕获且处理所有异常，那么它的捕获语句一定长下面这样：
```
catch (...) {
    处理语句；
}
```
- 这里的`catch (...)`表示捕获所有的异常种类！
- 但是：
- ![](https://i.imgur.com/k7rorot.png)

:::

---

#### 1.4 noexcept 异常说明（==不抛出说明==）

在C++11新标准中，我们可以通过 `noexcept`来==表明这个函数一定不会抛出异常==！
比如: `int return_calc(int a, int b) noexcept { return a + b; }` 就表示这个函数一定不会抛出异常！

- 虽然我们可以使用这个语句表示这个函数不会抛出异常...
- 但编译器在运行的时候实际上是不会检查有没有 noexcept 说明的！也就是说...
- 哪怕函数声明时有 noexcept 说明，我们在函数体内也可以抛出异常(throw)并且是可以正常运行不会报错的！
- 所以：==我们可以在两种时候使用 noexcept==:
    ==**1. 确认函数不会抛出异常；**==
    ==**2. 根本不知道如何处理异常！**==
    
:::success
noexcept 运算符：通常与noexcept说明符混合使用！

eg.
```
noexcept(func(i));

如果func(i)不抛出异常则返回true；反之返回false。
```
![](https://i.imgur.com/NsdTHPX.png)

:::

---

#### 1.5 异常类层次

![](https://i.imgur.com/jyHhAWm.png)

> 当然我们也是可以自定义异常的，这个很简单就不细说了。看看之前的例子你就知道了。

---

### 2. 命名空间

#### 2.1 命名空间基础
道理很简单，东西太多都放在一个空间名字就容易乱！那么我干脆多搞几个空间，然后每个空间分别管理名字不就不容易混杂了吗！

**命名空间    ： 关键字 `namespace` + 命名空间的名字！**

eg. `namespace MyGarbage { 放我需要的东西; }` 这个代码就定义了一个名为 `MyGarbage`的命名空间，里面存着我需要的命名。==值得注意的是，命名空间作用域后无分号，不像类！==

如何调用？ eg. `MyGarbage::变量名 变量(变量值)`
:::warning
命名空间可以是不连续的！
什么意思？
- 同一个命名空间可以将不同的变量名写在不同的文件里面！
:::

---

#### 2.2 各种命名空间

1. **全局命名空间**：全局作用域都管用的名字。如：`::member_name`
2. **嵌套命名空间**：在其他命名空间里面的命名空间，如：`MyGarbage::First::变量名`，表示在MyGarbage空间中的First空间里面的变量！

:::success
3. 内联命名空间（C++11新标准）
内联命名空间中的名字可以不需要外层命名空间就直接使用！（内联嘛！）
- 也就是说，**无需在空间内变量名字前面加上空间名为前缀**，直接用就完事了！
- 定义内联命名空间方法：**加上 `inline` 关键字即可**！
- eg. `inline namespace NewInlineSpace`
- 并且，这个 `inline` ==只需要在第一次定义时候写==就完事！
:::

:::info

4. 未命名的命名空间：eg. `namespace { 内部空间； }`
- 特点：没名字！（谁这么用啊！）
- 特点：在同一文件内，可以不连续（也就是可以多次追加内容进去，写到哪加到哪，读代码的火葬场了）
- 因为没命名，所以也是可以直接用！
- ==在未命名的名字空间中定义的变量都是静态的。==
![](https://i.imgur.com/Qty1LZV.png)
> 那么，什么时候用呢？
> 在需要静态声明的时候就可以用啊！不写静态，而是直接写入未命名的命名空间。比如：

```
namespace { int i=10; }

希望所定义的对象、函数、类类型或其他实体，只在程序的一小段代码中可见，可进一步的缓解名字空间的冲突。
根据C++11标准，static定义静态变量的做法已取消，现在是定义一个全局的未命名的名字空间。
在未命名的名字空间中定义的变量都是静态的。
```
:::

---

#### 2.3 使用命名空间的成员

==命名空间的别名==：假设我们有一个命名空间：`MyLongestNamespace`，而每次使用都这么长肯定很不爽吧？我们可以使用别名将其缩短！见下面：
**`namespace Mine = MyLongestNamespace`**
![](https://i.imgur.com/PpU7cRN.png)

- ==using声明==：一条using只可以引入命名空间中的一个成员！**`using std::string`**
- ==using指示==：将命名空间直接注入全局作用域！一下引入全部成员！ **`using namespace std`**
> 我们是不是应该无脑用using指示？
> 不是的，全都注入全局很容易让你头晕；万一不同命名空间有同名文件，那编译器跟着一起头晕！

:::danger
在自己的命名空间，请避免使用using指示！（当然 标准库 `using namespace std`是很方便的，完全可以使用！）

![](https://i.imgur.com/3owoQEt.png)
:::

---

### 3. 多重继承与虚继承

**多重继承**：**从多个基类中产生的派生类** 
eg: `class Panda: public Bear, public Endangered {/*...*/};`

![](https://i.imgur.com/mDHLier.png)
> 它的构造函数和前面学过的一样，不同基类属性都使用委托构造形式即可。
> 构造函数可以从基类继承，但是不能从多个基类继承相同的构造函数！会报错。比如，我不能够同时从Bear基类和Endangered基类继承默认构造函数，编译器不知道我用哪一个了！

---

**类型转换与多个基类**

在派生类只有单个基类的时候，我们可以使用派生类的指针或者引用自动转换成基类的指针或引用。**在多基类的情况，也是可以做到的！只需要表明是哪一个基类即可！**

![](https://i.imgur.com/ivUk1Oc.png)

---

**虚继承**：虚继承的目的是令类做出==愿意共享基类的声明，共享的基类子对象就称为虚基类！==
- 虚基类有什么意义？
- 如iostream，同时继承了istream和ostream，然后这两者又有共同基类base_ios,如果没有虚基类的声明那么iostream就继承了base_ios两次，岂不是就会运行同样的操作两次了吗！
- 所以虚基类使其基类可以共享，使得不论虚基类在继承体系出现多少次，在派生类中都只包含唯一一个共享的虚基类子对象。

比如：panda的基类究竟是谁，实际上是有些许争议的。所以我们在继承体系中，panda有可能继承同一基类两次！

![](https://i.imgur.com/gE9152U.png) - **基类继承图**
![](https://i.imgur.com/TwAs5gk.png)

:::success
使用虚基类： virtual 关键字

```
class Bear: virtual public ZooAnimal { /*...*/ };
class Raccon: public virtual ZooAnimal { /*...*/ };
```
- virtual表明：==后续的==派生类将会共享虚基类的同一份实例！

![](https://i.imgur.com/eg2l5Z9.png)
![](https://i.imgur.com/eBJvt57.png)
:::

---

## 第十九章：特殊工具与技术

### 1. 控制内存分配

- 某些应用程序对内存分配有特殊需求如需要使用 `new` 将对象放置在特定内存空间。
- 为了实现这一目的，我们会用到重载 `new` 和 `delete` 运算符以控制内存分配过程。

:::success
**重载 `new` 和 `delete`**

观察下面的`new`表达式：
```
string *sp = new string("a value");
string *arr = new string[10];
```

实际上执行了三步操作：
1. new表达式调用了 `operator new`，该函数分配了一块足够大的未命名的内存空间；
2. 编译器运行对应构造函数以构造对象并传入初始值；
3. 对象被分配到空间并构造完成，返回一个指向它的指针。

同样地，观察下面的`delete`表达式：
```
delete sp;  // 销毁 -> 释放内存空间；
delete [] arr; // 销毁组中元素 -> 释放对应的内存空间。
```
实际上也是分两步的！
1. 销毁对象；
2. 调用`operator delete` 释放内存空间。

- 也就是说！如果希望自己控制内存分配，需要自定义 `operator new` 和 `operator delete`!
- ![](https://i.imgur.com/womEJQZ.png)
- 重载运算符时，必须使用`noexcept`指定其不抛出异常！

> 注意到上面步骤：`operator new`在对象被构造前调用；`operator delete`在对象被析构后调用。==说明它们本身不操作任何的数据成员！==**而是起到管理未命名内存的作用**！

![](https://i.imgur.com/w0vVrDf.png)

:::

---

**malloc与free函数**

`#include <cstdlib>`

- malloc： 接受待分配字节数的`size_t`,返回指向分配空间的指针！（失败则返回0）
- free： 接受`void*`，是`malloc`返回的指针的副本，free将相关内存返回给系统。

---

**定位 `new` 表达式**

- 我们可以使用定位 `new` 来传递一个地址：

```
new (place_address) type;
new (place_address) type(initializer);
new (place_address) type [size];
new (place_address) type [size] {braced initializer list};
```

- 这里的`place_address`必须是一个指针！
- 当通过仅一个地址调用时，定位 `new` 使用 `operator new` 分配内存，这个版本的它无法自定义。==它不分配任何内存，只是简单返回指针实参==。
- ![](https://i.imgur.com/K25KwbU.png)

> 析构：`sp -> ~string()`：箭头运算对sp指针解引用返回内部string对象，再一个波浪符号析构！但是，==析构只会清除给定对象，不会释放该对象所在的空间！==

![](https://i.imgur.com/bhroaSg.png)

---

### 2. 运行时类型识别(RTTI, run time type identification)

由两个运算符实现：
- typeid运算符，返回表达式类型；
- dynami_cast运算符，将基类的指针或引用==安全地==转化称派生类的指针或引用。

> 适用于：想使用基类对象的指针或引用执行某个派生类操作（非虚函数！）
> 一般来说要干这种事情最好都设成虚函数，但并非什么时候都能定义一个虚函数！
> 所以：若无法使用虚函数就使用 RTTI 运算符！（但是肯定不推荐！）
![](https://i.imgur.com/pSc92V9.png)

---
:::info
**dynamic_cast运算符**

使用形式：
```
注：e的类型只能是：    
     1. 目标type的公有派生类；
     2. 目标type的共有基类；
     3. 目标type类型。
    
注：type类型要求：是一个类类型，并且通常含有虚函数。

dynamic_cast<type*> (e);    // e为指针
dynamic_cast<type&> (e);    // e为左值
dynamic_cast<type&&> (e);   // e为右值
```


eg. 对于有派生类Derived的Base，将指向它的对象的指针改为指向它的派生类对象的指针!

```
// 为什么可以用if？因为使用 dynamic_cast创建失败返回的是0！
if (Derived *dp = dynamic_cast<Derived*>(bp)) 
{
    // 使用dp指向的Derived对象；
} else {
    // 使用dp指向的Base对象；
}
```
![](https://i.imgur.com/tWZbuF4.png)
![](https://i.imgur.com/aydQfkn.png)

:::success

> 当使用引用类型时，实际上和指针类型有些许不同。
- 1. 存在空指针，但不存在空引用！所以不能对空对象使用。
- 2. 类型转换失败时不是返回0，而是抛出 `std::bad_cast`异常！（定义在typeinfo头文件）

:::

---
**typeid 运算符**

typeid:返回对象类型： `typeid(e) -> 返回 &type(e)`返回常量对象引用（对象为类类型）

![](https://i.imgur.com/dfLhOQs.png)

---
**type_info类**

`#include <typeinfo>`

![](https://i.imgur.com/E5Dao97.png)

---
### 3. 枚举类型

枚举类型：可以将一组整型常量组织在一起。

:::success
**C++11: 定义类限定作用域的枚举类型**
```
enum class(or struct) 类型名 {枚举成员};
enum class open_modes {input=1, output=2, append=3};
```
- 上面的代码定义了一个 `open_modes`类，它含有`input, output, append`三个枚举成员。
:::

**不限定作用域的枚举类型**：`enum 类型名 {枚举成员};`
比如： `enum open_modes {input=1, output=2, append=3};`

- 枚举成员是const，我们需要提供常量表达式。

:::info
**指定 `enum` 大小**

我们可以在 `enum` 名字后面加上冒号以及我们想使用的类型：
```
enum intValues: unsigned long {
    charTyp = 255, shortTyp = 65535, longTyp = 4294967295UL
};
```

- 若不指定则默认是int！
:::

:::success
**枚举类型的前置声明**
C++11新标准允许提前声明enum。==前置声明必须要指定成员大小！==
```
enum intValues: unsigned int;
enum lass open_mode;
```
:::

---

### 4. 类成员指针

类成员指针：可以指向类的非静态成员的指针！
- 指向的是类的非静态成员，而非类的对象！
- 由于类的静态成员不属于任何对象，所以无需特殊的指针去指向它，普通指针就可以！
- 类成员指针分为：
    - 数据成员指针；
    - 成员函数指针。

下面我们用Screen类来举例。
```
class Screen {
public:
    typedef size_t pos;
    char get_cursor() const { return contents[cursor]; }
    char get() const;
    char get(pos ht, pos wd) const;
private:
    string contents;
    pos cursor;
    pos height, width;
};
```

:::success
**数据成员指针**

旧版本我们可以用以下方式创建：
```
const string Screen::*pdata;
```
这样的指定实际上是没有指明对象的，也就是说可能指向Screen中的任意一个const string成员！所以，==这样的指针只能读取不能写入！==

---
C++11新标准：可以使用auto或者decltype指定！
```
Screen MyScreen;
auto pdata = *MyScreen::contents;
```
- 这样就可以指定指向对象，进行读写操作！`.*   ->*`

> 成员指针访问符：解引用指针且获得该对象的成员。
```
auto cont = Screen.*pdata;

或者解引用：
auto s = MyScreen ->*pdata;
```
:::
:::info
**成员函数指针**

==当该成员函数不含任何形参时==，创建方法和数据成员指针类似！
```
auto pmf =*Screen::get_cursor;
```

当有类内重载时，需要显式声明函数类型！
```
char (Screen::*pmf1)(Screen::pos, Screen::pos) const;
auto pmf2 = &Screen::get;
```

- 使用成员函数指针：`.*   ->*`（同上）
```
char c = (MyScreen->*pmf)();

char c2 = (MyScreen.*pmf2)(0, 0);
```
![](https://i.imgur.com/4R8lcne.png)
![](https://i.imgur.com/kn3xMX3.png)

:::

:::warning
**将成员函数指针用作可调用对象**

1. 之前学习过的functional头文件里的功能可以实行！
```
function<bool (const string&)> fcn = &string::empty;
find_if(vec.begin(), vec.end(), fcn);
```

---
2. 同样在functional头文件的`mem_fn`也可以实现且不需要显示指定调用对象类型！
```
find_if(vec.begin(), vec.end(), mem_fn(&string::empty));
auto f = mem_fn(&string::empty);
```
> mem_fn:member function，就是拿来调用成员函数的！

---
3. bind也可以实现调用！
```
auto f = bind(&string::empty, _1) // 将bind绑定到empty的第一个隐式实参上！
```
:::

---

### 5. 嵌套类

嵌套类：在另一个类的内部的类！常用于定义作为实现部分的类！==与外层类是无关的！==

- 嵌套类在外层类作用域可见（毕竟是实现部分！）但是出了外层类就不可见啦！

嵌套类位于外层类不同地方...
- public部分：可以随处访问；
- protected部分：可以被外层类及友元和派生类访问；
- private部分：只能被外层类和友元访问！

:::success
嵌套类的声明和定义可以分开！也就是说，我们可以在外层类的内部声明，然后再在主文件定义！
```
class TextQuery {
public:
    class QueryResult;  // 先声明，后定义
};

...

class TextQuery::QueryResult {
    定义语句...
};
```

:::

---

### 6. 联合类union-节省空间！

union：可以有多个成员，但是任意时刻只有一个数据成员可以有值！
> 也就是说，当==给某个成员赋值之后，其他成员就变成未定义状态！==

特性：
1. union不能含有引用类型成员（因为引用不能为空！）；
2. 于C++11后，含有构造函数和析构函数的类类型也可以作为union成员类型；
3. union也可以为成员指定public, private, protected；
4. union不能作为基类，不能继承其他类，不能带有虚函数。

**定义union**

==union提供了方便表示一组类型不同的互斥值的手法！==
```
union Token{
    int ival;
    char cval;
    double dval;
};
```
> 定义union时，先是关键字`union`，再接上==可选的名字==，最后是花括号内的成员声明！

**初始化**
```
Token firstone = {'c'};  // 初始化一个Token对象，初始只有cval成员定义了；
Token undifined_token;  // 未初始化的Token对象；
Token *token = new Token;  // 指向一个未初始化的指向Token对象的指针.
```

**访问&修改成员**
```
firstone.cval = 'z';  // 修改cval成员；
token -> ival = 42;  // 解引用指针后修改ival成员.
```

---

:::success
**匿名union**

未命名的union，一旦定义则编译器会自动创建一个未命名对象。

```
union {
    int ival;
    char cval;
    double dval;
};

---
cval = 'c';
ival = 42;
```
> 可以发现，匿名union内定义的成员都是可以直接访问的！
:::

---

**含有类类型成员的union**

- 若union中的成员都是内置类型时候，我们可以简单使用赋值语句去改变union的保存至因为它们都是由默认的构造和析构函数的。
- 但是如果union里含有特殊类型呢？
    - 这个类型一定要有构造和析构函数！因为union中一个成员有值则其他成员为未定义！

- 问题是，对于union而言要想构造或者销毁类类型成员，操作是很复杂的！
    - 因此，==含有类类型的union通常被内嵌在另一个类内使用！== 然后我们利用这个类来管理相关成员的转换。
    - 举个例子：我们将union作为成员定义在Token类内并且在union内添加string成员。这时候Token就能管理这个string成员！

> 问题2：如何追踪union里究竟哪个成员是有值的？
> 我的想法：在Token类里定义一个成员追踪啊！在union变动时候把它也变了就成。
> 但是这其实有点花，建议看p778代码！

---

### 7. 局部类

局部类：定义在函数内部的类！只在函数作用域内管用，所以一般声明和定义放一起！（因为就算不放一起也都得在函数内啊！）

![](https://i.imgur.com/Wu5SmSh.png)

- 局部类一般只有几行代码，不然谁他妈读得懂！
- 局部类也不能使用函数作用域中的变量！因为它对外层访问是受到限制的！
- 因为它在非全局作用域内，它不能含有静态成员！