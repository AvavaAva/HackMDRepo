###### tags: `learn`

# Py随笔记

## 0. Python教程--忘了可以参考！

[Python教程](https://www.liaoxuefeng.com/wiki/1016959663602400)


---

## 1. 关于内存地址的事（随便碰torch有感）

内存地址这个问题关乎到占用，所以随时观察程序内的变量与函数等是否被使用，对于未被使用项目进行释放删除是很重要的。Python的垃圾回收机制也有所学习过，主要分为：

1. 记次数（对于各种变量等）
2. 画有向图（对于各种可能的循环嵌套结构）
3. 通常是，越早创建的内容越有可能被使用；越新创建的内容越容易是无用的，根据这个模式来监测。

**源地址放在下面：**

[python的垃圾回收机制](https://blog.csdn.net/xiongchengluo1129/article/details/80462651)

而现在注意到，对于一个变量（基本类型或容器类型，这里是tensor张量），我们改变其值通常有两种写法：

    1. Y += X
    
    2. Y = Y + X

这两种写法有什么不同呢？
**实际上是有的！** 第一种写法不会改变变量的内存地址，而第二种会给Y创建一个新的内存地址！
我们可以用自带的 id() 函数查看变量内存地址。结果图片放在下面。

![](https://i.imgur.com/5y7J2Qi.png)

可以发现，我们用第一种加法是改变了内存地址的！而第二种却没有！

并且，也可以发现我们新建同值的变量，两个变量实际上会指向一个内存！！！也就是上面的Z与Y同内存！**那么， 我修改Z的时候，Y也会被同步修改！**

![](https://i.imgur.com/dplCb7j.png)

:::success
对于不可变容器类型，如果创建两个相同容器则第二个实际上是第一个的引用，二者共享地址。（列表不行，因为列表可变！）

对于基础数据类型，同值则同地址，但是修改后就不同地址了。
:::

---

## 2. 装饰器Decorator

出处： [装饰器的理解及应用](https://www.bilibili.com/read/cv11252337?spm_id_from=333.999.0.0)

- 定义: 它是一个函数，它接收另一个函数并扩展后一个函数的行为，而无需显示修改它.

使用示例：

![](https://i.imgur.com/8xFHGyf.png)

![](https://i.imgur.com/yIjIpg9.png)

*案例： 装饰器实现运行计时*

![](https://i.imgur.com/iVkBXa3.png)
![](https://i.imgur.com/8MOpZfB.png)

---

:::success
**特殊装饰器：@classmethod, @staticmethod, @property**

类中定义的普通方法（即实例方法），需要先实例化类的一个对象再调用，无法直接用类调用。而被@classmethod或@staticmethod装饰过的方法，可以不需要实例化，直接以“类名.方法名()”的方式来调用。
三个装饰器都是在类中使用：

    @property : 将函数封装为 ！！只读属性！！ 。需要参数self，实例对象直接调用该方法，无需()。

    @classmethod ：用于装饰“类方法”。需要参数cls，无需self。该类方法可以直接被调用，而无需实例化。无 self 参数，也无法访问实例化后的对象该类方法只能访问类属性，而无法访问实例属性。

    @staticmethod ：静态方法。无需参数cls、self。被装饰的方法会成为静态方法，无需实例化可以调用。


:::

---


## 3. 生成器 & 迭代器

### 生成器：

通过列表生成式，我们可以直接创建一个列表。但是，受到内存限制，列表容量肯定是有限的。而且，创建一个包含100万个元素的列表，不仅占用很大的存储空间，如果我们仅仅需要访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了。

所以，如果列表元素可以按照某种算法推算出来，那我们是否可以在循环的过程中不断推算出后续的元素呢？这样就不必创建完整的list，从而节省大量的空间。在Python中，这种一边循环一边计算的机制，称为生成器：generator。

要创建一个generator，有很多种方法。第一种方法很简单，只要把一个列表生成式的[]改成()，就创建了一个generator：

![](https://i.imgur.com/zt8ghU1.png)
![](https://i.imgur.com/v2SL6d5.png)


例：利用生成器构造斐波那契数列

```
def fib(maximum):
    n, a, b = 0, 0, 1
    while n < maximum:
        yield b
        a, b = b, b + a
        n += 1
    return 'All'


f = fib(5)
while 1:
    try:
        print(next(f))
    except StopIteration:
        print('生成器已经打印完')
        break

```
![](https://i.imgur.com/NBmQsPi.png)

**第二种方法：使用yield关键字**

==一个带有 yield 的函数就是一个生成器函数==，当我们使用 yield 时，它帮我们自动创建了__iter__() 和 next() 方法，而且在没有数据时，也会抛出 StopIteration 异常，也就是我们不费吹灰之力就获得了一个迭代器，非常简洁和高效。
带有 yield 的函数执行过程比较特别：

    调用该函数的时候不会立即执行代码，而是返回了一个生成器对象；
    当使用 next() (在 for 循环中会自动调用 next() ) 作用于返回的生成器对象时，函数 开始执行，在遇到 yield 的时候会『暂停』，并返回当前的迭代值；
    当再次使用 next() 的时候，函数会从原来『暂停』的地方继续执行，直到遇到 yield语 句，如果没有 yield 语句，则抛出异常；

---
#### 杨辉三角！！！（很神奇的例子！）

![](https://i.imgur.com/8zjhXU7.png)

请构造一个杨辉三角的生成器！！！
输出应该如下，举例：

![](https://i.imgur.com/ruXTTu6.png)

代码：
一看不清楚为啥，仔细一想确实是！杨辉三角的新的一行就是以上一行为基础，左边加个0和右边加个0之后相加就是！！！！！太神奇了！！！！真的！！！！

```
def triangles():
    L = [1]
    while True:
        yield L
        X = [0] + L
        Y = L + [0]
        L = [X[i] + Y[i] for i in range(len(X))]
```


---


### 迭代器


所有具有__next__()功能的对象统称迭代器。（也就是可以通过next（）调用）

可以通过 isinstance（obj, Iterable）来判断对象是否为迭代器。

```
print(isinstance([], collections.abc.Iterable))

 >> True
```
**注：**

凡是可作用于for循环的对象都是Iterable类型；

凡是可作用于next()函数的对象都是Iterator类型，它们表示一个惰性计算的序列；

集合数据类型如list、dict、str等是Iterable但不是Iterator，不过可以通过iter()函数获得一个Iterator对象。

Python的for循环本质上就是通过不断调用next()函数实现的！


---

## 4. 高阶函数：map & reduce & filter！

[总介绍](https://www.liaoxuefeng.com/wiki/1016959663602400/1017329367486080)
### map

map()函数接收两个参数，一个是函数，一个是Iterable，map将传入的函数依次作用到序列的每个元素，并把结果作为新的Iterator返回。

举例说明，比如我们有一个函数f(x)=x2，要把这个函数作用在一个list [1, 2, 3, 4, 5, 6, 7, 8, 9]上，就可以用map()实现如下：
![](https://i.imgur.com/Km2A9MI.png)


```
r = map(math.exp, [2, 3, 4, 5, 0.1])
print(r)

>> <map object at 0x000001B926FD2FA0>

print(list(r))
>> [7.38905609893065, 20.085536923187668, 54.598150033144236, 148.4131591025766, 1.1051709180756477]
```

**map()传入的第一个参数是f，即函数对象本身。由于结果r是一个Iterator，Iterator是惰性序列，因此通过list()函数让它把整个序列都计算出来并返回一个list。**


--------

### reduce

reduce把一个函数作用在一个序列[x1, x2, x3, ...]上，这个函数必须接收两个参数，reduce把结果继续和序列的下一个元素做累积计算，其效果就是：

```
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```

![](https://i.imgur.com/RZYQavL.png)

**reduce在python3里我没调出来，并且这玩意儿看来也挺鸡肋的。**


---

### filter

Python内建的filter()函数用于过滤序列。

和map()类似，filter()也接收一个函数和一个序列。和map()不同的是，filter()把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素。

下面给出一个例子用来保留列表中的奇数：

```
def is_odd(n):
    return n % 2 == 1


print(*filter(is_odd, [x for x in range(7, 15)]))

>> [7, 9, 11, 13]

---

def is_prime(num):
    if num == 2:
        return True
    elif num < 2:
        return False
    elif not num % 2:
        return False
    else:
        for i in range(3, num//2, 2):
            if not num % i:
                return False
    return True


if __name__ == "__main__":
    print("测试数据：{}".format(lst1))
    target = filter(is_prime, lst1)
    print("其实是素数的是：", *target)
    
    
---
>>   测试数据：[1, 2, 3, 6, 5, 32, 75, 12, 5]
>>   其实是素数的是： 2 3 5 5
```

#### 小例子：找出所有回数！

给定一个范围，找出其中所有回数。

```
def is_palindrome(n):
    return str(n)[::-1] == str(n)


print(*filter(is_palindrome, [x for x in range(900, 1000)]))

>> [909, 919, 929, 939, 949, 959, 969, 979, 989, 999]
```


---

## 5. 启发式算法 & 元启发式算法

**启发式算法(Heuristic Algorigthm)：**

是一种基于直观或经验构造的算法,在可接受的花费(指计算时间、计算空间等)给出待解决优化问题的每一实例的一个可行解，该可行解与与最优解的偏离程度一般不可以事先预计。

启发式算法是一种技术,这种算法可以在可接受的计算费用内找到最好的解,但不一定能保证所得到解的可行性及最优性，甚至大多数情况下无法阐述所得解与最优解之间的近似程度。


**元启发式算法（MetaHeuristic Algorigthm）：**

是启发式算法的改进，它是随机算法与局部搜索算法相结合的产物。

启发式算法包括遗传算法、模拟退火算法、粒子群算法、禁忌搜索算法及神经网络算法等。

**比较：**

1）两者无法保证得到某个优化问题的全局最优解；
2）两者存在意义就是快速地求解那些不存在或者暂时未找到多项式时间内算法的问题。比如TSP问题，flowshop问题。而像Linear programming就不会用heuristics或者meta－heuristics去求解了；
3）不同点在于：heuristics算法中不会存在“随机因素”。对于同样一个问题，只要给定了一个输入，那么算法执行的步骤就固定下来了，输出也因此固定。但是meta－heuristics就不一样了，里面包括了“随机因素”。就拿遗传算法来说，同样一个初始的种群（输入一致），运行两次（每次100代），得到的结果很可能是不同的。因为交叉、变异操作的对象是随机选择的。在比如模拟退火，按照metropolis准则跳出局部最优解时也是随机的，这也解释了为什么SA可能得到全局最优解。

**总之，heuristics在固定的输入下得到固定的输出。meta-heuristics在固定的输入下得到的输出不固定。**


---

## 6. Python的正则表达式 -- re模块！（补足部分看：[爬虫文件](https://hackmd.io/mto5X-HqQbOCXRH6c0xWNA)）

![](https://i.imgur.com/1v5nIUQ.png)

Python对字符的描述：

- \d: 可匹配一个数字（digital）
- \w: 可匹配一位数字/字母/下划线！
- \s: 可匹配一个空格！
- . : 匹配除去换行符外的任意一位！

所以：如果想要表示
```
'007'
```

我们可以使用
```
'00\d' or '00\w'
```
;
同理若想要表示
```
'00a'
```

我们可以使用
```
'00\w'
```
却不能使用
```
'00\d'
```
!
推广而言我们可以用 **'\d\d\d'** 来表示任意三位数组合的字符串；
也可以使用 **'\w\w\w'** 来表示任意三位数字与字母的组合。

- 另外， '.'可以匹配任意一个字符，如：
    - 'py.' 可以匹配 'pyc', 'py1', 'py!'
    
**- 如何匹配变长的字符呢？
    1. 用 '*' 匹配0~n个长度的字符
    2. 用 '?' 匹配0~1个长度的字符
    3. 用 '+' 匹配最少1个长度的字符
    4. 用 '{n}' 匹配n个长度的字符；或用 '{n, m}' 匹配n~m长度的字符**
    
接下来我们来看个例子：
```
\d{3}\s+\d{3, 8}

解释：

    \d{3}： 3个长度的数字；
    \s+: 至少1个长度的空格；
    \d{3, 8}: 3~8个长度的数字！
    
也就是说，上面的正则式可以表示任意一个带区号的且用空格隔开的座机号码！
```

![](https://i.imgur.com/M9dWfLi.png)
![](https://i.imgur.com/08j94Bg.png)
![](https://i.imgur.com/bzFVLa1.png)
![](https://i.imgur.com/xTTRwlJ.png)
![](https://i.imgur.com/ria6YsS.png)

### 练习：

![](https://i.imgur.com/7XdzoTx.png)

![](https://i.imgur.com/uZKX0ei.png)
![](https://i.imgur.com/7Yi9F7G.png)

---


## 7. collections 模块

### 1. collections.namedtuple

众所周知，在py里面(x, y)表示的是一个集合set，那么我要如何表示成坐标呢？？

这个时候，namedtuple就很牛逼了！它支持自定义对于集合内每一个元素的命名！（其实感觉更像简单生成利用元组（集合）的子类！）

**比如：**
```
Point = collections.namedtuple('Point', ['x', 'y'])
p = Point(1, 3)
print(p.x, p.y)

>> 1 3
```

类似地！我们也可以用 **namedtuple** 创建圆的信息！
```
Circle = collections.namedtuple('Yuan', ['x', 'y', 'r'])
en1 = Circle(0, 0, 4)
print(en1.r)
print(isinstance(en1, tuple))

>> 4  True
```

### 2. deque

过于熟悉，留个图就行
![](https://i.imgur.com/Po52crd.png)

### 3. defaultdict

可以在查询到无键时不报错的字典！
![](https://i.imgur.com/KslbCT6.png)


### 4. ordereddict

可以对键进行先后排序的dict！

![](https://i.imgur.com/eXhKSKD.png)
![](https://i.imgur.com/80Y7kZn.png)


### 5. Counter

老熟人了！
但是之前一直没有注意它还有个 update() 函数！

![](https://i.imgur.com/Ra7T4iN.png)
```
c = collections.Counter('program')
print(c)
c.update('my fking god!')
print(c)

>> 
Counter({'r': 2, 'p': 1, 'o': 1, 'g': 1, 'a': 1, 'm': 1})
Counter({'g': 3, 'r': 2, 'o': 2, 'm': 2, ' ': 2, 'p': 1, 'a': 1, 'y': 1, 'f': 1, 'k': 1, 'i': 1, 'n': 1, 'd': 1, '!': 1})
```


---

## 8. itertools 模块：迭代好帮手

### 1. itertools.count()

会创造一个无限的迭代器！
```
natuals = itertools.count(1)
for n in natuals:
    print(n)
    
就会从1开始无限往下打！！！
```

### 2. itertools.cycle()

会创造一个无限的循环序列！（但是是将序列元素拆分开的！）
```
cs = itertools.cycle('ABC') # 注意字符串也是序列的一种
for c in cs:
    print(c)
    
就会不停地 A B C A B C A B C A B C...
```

### 3. itertools.repeat()

类似cycle()!但是序列内元素是放一起形成整体！

```
cs = itertools.repeat('ABC') # 注意字符串也是序列的一种
for c in cs:
    print(c)
    
就会不停地 ABC ABC ABC ABC ABC ...
```

然而！repeat()有一个可选参数，用于控制迭代轮数！

```
cs = itertools.repeat('ABC', 4) # 注意字符串也是序列的一种
for c in cs:
    print(c)
    
>> ABC ABC ABC ABC
```

### 4. itertools.chain()

顾名思义，将两个迭代内容链接！

```
for c in itertools.chain('abc', 'ABC'):
    print(c)
   
>>
a
b
c
A
B
C
```

### 5. itertools.groupby()

将序列内的相邻重复元素挑出来放一起！

```
for key, group in itertools.groupby('AAABBBCCAAA'):
    print(key, list(group))
    
>> 
A ['A', 'A', 'A']
B ['B', 'B', 'B']
C ['C', 'C']
A ['A', 'A', 'A']

```

---
## 9. 函数的参数问题

在C++我们可以通过initialzed_list 来输入不定长参数。那么python呢？

==Python对列表的传递是**引用传递**！== 但是我们都知道python无法修改字符串，所以对于字符串而言，是生成新的对象。

:::success
*：输入不定长的参数。
**：输入不定长的键值对！

- 注意，它们必须放在形参列表的最末尾且只能有一个，不然会报错！
```
def showallvars(name, *vars)：
    print(name,'\n')
    for each in vars:
        print(each)
    
    
def showkeyvals(**items):
    for each in items:
        print('key is {} and val is {}'.format(each[0],each[1]))
```
:::

---

## 10. 静态类

就像C++拥有函数对象（也就是，可以对类对象进行类似函数的调用），并且C++也拥有纯虚函数和抽象基类，它们不能够被创建实例一样，**python的静态类**也是不支持创建实例的！

**静态类**

1. **静态类内部无self关键字，也就是说，不能被实例化**；
2. 静态类不能通过类名传递参数；
3. 静态类不支持 __init()__ 初始化函数（这就是个构造函数，不允许实例化当然不能用它）；
4. 静态类不能被实例化；但**它可以集成变量和函数**，是带结构的数据类型（和抽象基类很相似）！

```
例.静态类的使用

class StaticC():
    name = 'Wei'
    age = 25
    address = 'Japan'
    
    def a():
        i = 0   
        i += 1
        print('函数a:{}'.format(i))
        
    def b(add=2):
        print('函数b:{}'.format(add))
        
调用：

StaticC.name  // 'Wei';
StaticC.a() // '函数a：1'
StaticC.b(add=77) // '函数b：77'
...

```

---

## 11. 异常处理(复习)

异常捕捉语法：
```
try:
    异常捕捉代码
except 异常原因Exception:
    处理异常代码
finally:
    最后一定执行的代码（可有可无）
```

**捕捉特定异常信息**：



| 异常类名 | 功能 | 
| -------- | -------- |
| ValueError | 对象值不正确 |
| IndexError | 容器类型下标越界！|
| NameError | 指定对象名不存在 |
| KeyError | 指定字典的键不存在 |
| TypeError | 提供了错误类型的对象 |
| ModuleNoFoundError | 找不到模块，常为拼写错误 |
| SyntaxError | 语法无效错误 |
| AttributeError | 对象的方法或属性使用不当 |

---

自定义异常：抛出异常 raise

语法：
```
raise Exception（ErrorMessage） - 选上面的类


eg.

if i > 5:
    raise ValueError('i的值不应该超过5！')
```
---

## 12. json文件

JSON（Java脚本对象标注符）是一种轻量级的数据交换格式，被当作不同程序之间数据共享的一种技术标准。

Python数据 -> JSON文件 ：序列化
JSON文件 -> Python数据 ：反序列化

### 1. JSON常用的两种结构数据类型

1. 键值对的集合：对应python的dict与C++的map；
2. 值的有序列表：对应python与C++的有序容器。

Python自带JSON模块。

```
import json

python_data = {'tom':29, 'wei':25} // Python数据
py_to_json = json.dump(python_data)  // dump储存
py_to_json

>> '{'tom':29, 'wei':25}'  // JSON格式（多了一对单引号）
```

--- 

### 2. 读写JSON文件：dump 和 load

- json.dump(python_data, save_file_name_and_path)
- json.load(json_file_name_and_path)

---

## 13. XML文件

**XML文件**：可扩展标记语言，用于进行共通化的文档编码，信息传输等。

### 1. XML数据结构示例

```
<storehouse>
    <goods categroy='fish'>
        <title>淡水鱼</title>
        <name>鲫鱼</name>
        <amount>18</amount>
        <price>8</price>
    </goods>
    
    <goods categroy='fruit'>
        <title>热带水果</title>
        <name>猕猴桃</name>
        <amount>100</amount>
        <price>5</price>
    </goods>
</storehouse>
```

> 格式易记，且相当简单明了。
- 根元素：数据文件的根文档，比如上面的`<storehouse>`.
- 子元素：除去根元素外，上面所有成对出现的都是子元素！且子元素可以不断嵌套（注意缩进）！
- 标签：带`<>`的就是标签。如上面，`<storehouse>`是开始标签，而`</storehouse>`是结束标签。
- 属性与文本：对每个元素进行标注，上面有例子。

> XML树形结构三要素：元素，属性，文本。

---

### 2. XML模块

Python带有xml模块，其中有两个API接口：SAX 和 DOM。`xml.sax`

---

#### 2.1 SAX接口

SAX是只读的，且处理大文件时DOM的速度更快。



| 函数 | 功能 | 
| -------- | -------- | 
|  make_parser(parser_list=[])    | 建议且返回一个SAX解析器的XMLReader对象；若parser_list提供，则指定第一个解析器的模块名。     |
|  parse(file_or_stream, handler, error_handler=handler=ErrorHandler()) | 建立一个SAX解析器，用来解析XML文档。 file_or_stream 指定XML文件名， handler指定SAX的ContentHandler实例对象。 |
|parseString(string, handler, error_handler=handler=ErrorHandler())|  类似parser，但从string参数中解析XMl。 |
|  SAXException(msg, exception=None) | 封装操作错误相关警告。解析出错时提供错误信息。 |


其余内容可以看书。

---

### 3. DOM模块

```
xml.dom.minidom.parse(filename, parser, bufsize=None)

返回指定XML文件的文档对象
```

其余的同样看书。实际上基本用不上倒也。

---

## 14. pymysql简单笔记

```
使用模块：pymysql


常用功能：
// 创建链接（精准到哪一个数据库！）
conn = pymysql.connect(host='localhost',user='root',passwd='sdtao0303' ,db='exercise', port=3306)

conn.close() // 关闭连接

// 必要：设置游标 & 写sql语句
cursor = conn.cursor()
sql_command = "select * from employee" // 和mysql一模一样写就好（无论增删查改）

// 执行语句 - 返回查询到的结果的行数！！！！
rows = cursor.execute(sql_command)

// 查询结果的保存
cursor.fetchall() -- 全部结果
cursor.fetchmany(num) -- 返回num条结果
cursor.fetchone() -- 返回一条结果并移动游标

// 移动游标
cursor.scroll(num, mode) // 选择模式，在指定模式下移动num个位置

mode： 1. absolute：从头开始移动
      2. relative：基于当前位置移动

// 保存事务
conn.commit()
```

流程（同mysql操作） -- 上面有
1. 建立连接
2. 创建游标
3. 增删改查操作（sql语句，execute）
4. 关闭链接

---

## 15. pyqt5简单笔记

pyqt5：自带模块，用于生成GUI。使用创建类来进行实现。

:::success

我们可以不使用代码，而是类似APP Inventor 一样模块式构架GUI。
[QTdesigner](https://build-system.fman.io/qt-designer-download)
[QTdesigner的使用](https://blog.csdn.net/stone0823/article/details/104101130)
:::

### 1. 完成GUI框架
---
**空白窗体**

```
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QDesktopWidget

class MainWindow(QWidget):
    def __init__(self):
        super().__init__()

        # 窗体标题与尺寸
        self.setWindowTitle('我的xxGUI')
        self.resize(1080, 720)

        # 窗体位置
        qr = self.frameGeometry()
        cp = QDesktopWidget().availableGeometry().center()
        qr.moveCenter(cp)


if __name__ == "__main__":
    app = QApplication(sys.argv)

    window = MainWindow()
    window.show()

    sys.exit(app.exec_())
```

---

**页面布局**

布局模块： `PyQt5.QtWidgets.QHBoxLayout(水平), QVBoxLayout(垂直)`
Pyqt5支持垂直或者水平方向的布局。

==弹簧==：将元素（也就是按钮）中间位置占满的组件。如果两个元素中间加个弹簧，就相当于这个弹簧将两个元素顶到两边去。布局由元素与弹簧构成（和APP INVENTOR一个样子，也就是说布局内还可以有布局！）。

> ![](https://i.imgur.com/QDbCYxq.png)  eg. 弹簧与按钮构成布局

:::success
布局代码及效果：
```
import sys
from PyQt5.QtWidgets import QApplication, QWidget, QDesktopWidget, QHBoxLayout, QVBoxLayout
from PyQt5.QtWidgets import QPushButton, QLineEdit, QTableWidget, QLabel

class MainWindow(QWidget):
    def __init__(self):
        super().__init__()
        '''
        ----------------------------- 设置窗口属性！
        '''
        # 窗体标题与尺寸
        self.setWindowTitle('我的xxGUI')
        self.resize(1080, 720)

        # 窗体位置
        qr = self.frameGeometry()
        cp = QDesktopWidget().availableGeometry().center()
        qr.moveCenter(cp)

        '''
        ----------------------------- 创建总的垂直布局
        '''
        layout = QVBoxLayout()

        '''
        ----------------------------- 1.创建顶部菜单布局
        '''
        header_layout = QHBoxLayout()
        layout.addLayout(header_layout)

        # 1.1 创建顶部菜单内的按钮
        btn_start = QPushButton("开始")
        ## 1.1.1 设置按钮尺寸
        # btn_start.setFixedSize()
        header_layout.addWidget(btn_start)

        btn_stop = QPushButton("停止")
        header_layout.addWidget(btn_stop)

        header_layout.addStretch()

        '''
        ----------------------------- 2.创建上面标题布局
        '''
        upper_layout = QHBoxLayout()
        layout.addLayout(upper_layout)

        # 2.1 创建输入框
        txt_asin = QLineEdit()
        ## 2.1.1 输入默认提示文字
        txt_asin.setPlaceholderText("请输入您想查询的选项！例如: C++类说明")

        upper_layout.addWidget(txt_asin)

        '''
        ----------------------------- 3.创建中间表格布局
        '''
        table_layout = QHBoxLayout()
        layout.addLayout(table_layout)

        # 3.1 创建表格
        table_widget = QTableWidget(0, 8) # 2行8列
        table_widget.setColumnWidth(0, 120)
        table_widget.setColumnWidth(1, 300)

        # 3.1.1 添加表格标题
        table_widget.setHorizontalHeaderLabels(['标题', '网址', '3rd', '4th', '5', '6', '7', '8'])

        table_layout.addWidget(table_widget)

        '''
        ----------------------------- 4.创建底部菜单布局
        '''
        footer_layer = QHBoxLayout()

        # 设置左侧标签label

        label_status = QLabel('未检测',self)
        footer_layer.addWidget(label_status)

        # 设置需要的按钮
        footer_layer.addStretch() # 用蛋黄让按钮和标签分布在两侧

        btn_reinit = QPushButton("重新初始化")
        footer_layer.addWidget(btn_reinit)

        btn_recheck = QPushButton("重新检测")
        footer_layer.addWidget(btn_recheck)

        btn_resetcount = QPushButton("次数清零")
        footer_layer.addWidget(btn_resetcount)

        btn_delete = QPushButton("删除检测项目")
        footer_layer.addWidget(btn_delete)

        btn_alert = QPushButton("SMTP报警配置")
        footer_layer.addWidget(btn_alert)

        btn_proxy = QPushButton("代理IP")
        footer_layer.addWidget(btn_proxy)

        layout.addLayout(footer_layer)

        '''
        ----------------------------- 5. 创建弹簧将上面元素顶上去（不然上面布局就会充盈屏幕）
        '''
        layout.addStretch()

        # 给窗体设置元素的排列方式
        self.setLayout(layout)


if __name__ == "__main__":
    app = QApplication(sys.argv)

    window = MainWindow()
    window.show()

    sys.exit(app.exec_())
```

展示效果：
![](https://i.imgur.com/0R63788.png)

:::

---
**组件拆分**：把每个区域（即每个layout）都写成函数，然后直接调用！这样才简洁嘛！

![](https://i.imgur.com/Q3GLDMs.png)
> 这样写起来会比较简洁！！！

---

### 2. 表格数据初始化

初始化表格时，如何在表格里展示数据？
1. 我们需要有数据 - 什么格式存储？ JSON！

![](https://i.imgur.com/imwaCKO.png)

3. 需要展示这些数据 - 如何展示？

这里我们仅仅展示 table_layout。

:::info
```
    def init_table(self):
        table_layout = QHBoxLayout()

        # 3.1 创建表格
        table_widget = QTableWidget(0, 8) # 2行8列
        table_widget.setColumnWidth(0, 120)
        table_widget.setColumnWidth(1, 300)

        # 3.1.1 添加表格标题
        table_widget.setHorizontalHeaderLabels(['类别', '网址', '价位', '具体', '备注', '发售日', '喜爱度', '接受度'])

        table_layout.addWidget(table_widget)

        # 3.2 初始化表格数据
        # 读取数据文件
        file_path = os.path.join(os.getcwd(), 'json_want.json')
        with open(file_path, mode='r', encoding='utf-8') as f:
            data = f.read()
        data_list = json.loads(data)

        cur_row_count = table_widget.rowCount() # 获取当前表格行数

        for row in data_list:
            table_widget.insertRow(cur_row_count)

            for i, each in enumerate(row):
                cell = QTableWidgetItem(each)
                if i in [0, 4, 5, 6]:
                    # 设置成不可修改！
                    cell.setFlags(Qt.ItemIsSelectable | Qt.ItemIsEnabled)
                table_widget.setItem(cur_row_count, i, cell)

            cur_row_count += 1

        return table_layout
```

成果展示：

![](https://i.imgur.com/vUiAySk.png)
> 我们在这里设置了0，4，5，6列不可修改，做法见上面（也就是设置或！）这样这几列就无法被修改了！

:::

---

### 3. 添加监控项

可以利用爬虫爬取数据加入我们的表格了！

**价格检测应用** 直接看代码吧！

重点是按钮点击时的绑定事件的设置代码，`button.clicked.connect(绑定函数！)`

---

### 4. 右键操作

表格里右键点击可以设置功能，如何设置？看文件。

---
### 5. 开始&停止

需要另外创建一个线程来控制开始与结束！从这开始涉及很多线程问题了，但是我不会写分布式编程！

---
### 6. 线程问题 

这里需要学习线程后再看一遍了！！！！

---


## 16. python的多线程 & 多进程（面试高频！）

先把原文贴在这里！[
Python之多进程和多线程详解
](https://blog.csdn.net/lianjiaokeji/article/details/83095187)

[python之多进程and多线程](https://zhuanlan.zhihu.com/p/356220352)


### IO编程

[IO编程](https://www.liaoxuefeng.com/wiki/1016959663602400/1017606916795776)

IO在计算机中指Input/Output，也就是输入和输出。由于程序和运行时数据是在内存中驻留，由CPU这个超快的计算核心来执行，涉及到数据交换的地方，通常是磁盘、网络等，就需要IO接口。

比如你打开浏览器，访问新浪首页，浏览器这个程序就需要通过网络IO获取新浪的网页。浏览器首先会发送数据给新浪服务器，告诉它我想要首页的HTML，这个动作是往外发数据，叫Output，随后新浪服务器把网页发过来，这个动作是从外面接收数据，叫Input。所以，通常，程序完成IO操作会有Input和Output两个数据流。当然也有只用一个的情况，比如，从磁盘读取文件到内存，就只有Input操作，反过来，把数据写到磁盘文件里，就只是一个Output操作。

IO编程中，Stream（流）是一个很重要的概念，可以把流想象成一个水管，数据就是水管里的水，但是只能单向流动。Input Stream就是数据从外面（磁盘、网络）流进内存，Output Stream就是数据从内存流到外面去。对于浏览网页来说，浏览器和新浪服务器之间至少需要建立两根水管，才可以既能发数据，又能收数据。

由于CPU和内存的速度远远高于外设的速度，所以，在IO编程中，就存在速度严重不匹配的问题。举个例子来说，比如要把100M的数据写入磁盘，CPU输出100M的数据只需要0.01秒，可是磁盘要接收这100M数据可能需要10秒，怎么办呢？有两种办法：

第一种是CPU等着，也就是程序暂停执行后续代码，等100M的数据在10秒后写入磁盘，再接着往下执行，这种模式称为同步IO；

另一种方法是CPU不等待，只是告诉磁盘，“您老慢慢写，不着急，我接着干别的事去了”，于是，后续代码可以立刻接着执行，这种模式称为异步IO。

同步和异步的区别就在于是否等待IO执行的结果。好比你去麦当劳点餐，你说“来个汉堡”，服务员告诉你，对不起，汉堡要现做，需要等5分钟，于是你站在收银台前面等了5分钟，拿到汉堡再去逛商场，这是同步IO。

你说“来个汉堡”，服务员告诉你，汉堡需要等5分钟，你可以先去逛商场，等做好了，我们再通知你，这样你可以立刻去干别的事情（逛商场），这是异步IO。

很明显，使用异步IO来编写程序性能会远远高于同步IO，但是异步IO的缺点是编程模型复杂。想想看，你得知道什么时候通知你“汉堡做好了”，而通知你的方法也各不相同。如果是服务员跑过来找到你，这是回调模式，如果服务员发短信通知你，你就得不停地检查手机，这是轮询模式。总之，异步IO的复杂度远远高于同步IO。

操作IO的能力都是由操作系统提供的，每一种编程语言都会把操作系统提供的低级C接口封装起来方便使用，Python也不例外。


---

### 进程 & 线程：

**进程是分配资源的最小单位，线程是系统调度的最小单位。**

当应用程序运行时最少会开启一个进程，此时计算机会为这个进程开辟独立的内存空间，不同的进程享有不同的空间，而一个CPU在同一时刻只能够运行一个进程，其他进程处于等待状态。

一个进程内部包括一个或者多个线程，这些线程共享此进程的内存空间与资源。相当于把一个任务又细分成若干个子任务，每个线程对应一个子任务。

### 多进程 & 多线程：

对于一个CPU来说，在同一时刻只能运行一个进程或者一个线程，而单核CPU往往是在进程或者线程间切换执行，每个进程或者线程得到一定的CPU时间，由于切换的速度很快，在我们看来是多个任务在并行执行（同一时刻多个任务在执行），但实际上是在并发执行（一段时间内多个任务在执行）。

而多核CPU就可以做到同时执行多个进程或者多个进程，也就是并行运算。在拥有多个CPU的情况下，往往使用多进程或者多线程的模式执行多个任务。

### GIL锁：

Python在设计的时候，还没有多核处理器的概念。
因此，为了设计方便与线程安全，直接设计了一个锁。
这个锁要求，任何进程中，一次只能有一个线程在执行。
因此，并不能为多个线程分配多个CPU。所以Python中的线程只能实现并发，而不能实现真正的并行。

但是Python3中的GIL锁有一个很棒的设计，在遇到阻塞（不是耗时）的时候，会自动切换线程。

python中的线程需要先获取GIL（Global Interpreter Lock）锁才能继续运行，每一个进程仅有一个GIL，线程在获取到GIL之后执行100字节码或者遇到IO中断时才会释放GIL，这样在CPU密集的任务中，即使有多个CPU，多线程也是不能够利用多个CPU来提高速率，甚至可能会因为竞争GIL导致速率慢于单线程。

**所以对于计算密集任务往往使用多进程，IO密集任务使用多线程。**

### python内的多进程 & 多线程实现

[实现多进程](https://www.liaoxuefeng.com/wiki/1016959663602400/1017628290184064)

[多线程](https://www.liaoxuefeng.com/wiki/1016959663602400/1017629247922688)

**总结：**
> Python的线程虽然是真正的线程，但解释器执行代码时，有一个GIL锁：Global Interpreter Lock，任何Python线程执行前，必须先获得GIL锁，然后，每执行100条字节码，解释器就自动释放GIL锁，让别的线程有机会执行。这个GIL全局锁实际上把所有线程的执行代码都给上了锁，所以，多线程在Python中只能交替执行，即使100个线程跑在100核CPU上，也只能用到1个核。
> ==GIL锁：限制了每一个进程中同时只能有一个线程在一个cpu上运行；也就是说限制了多线程，但是我们仍旧可以使用多进程。只是多进程的调用开销大。==
> 
> GIL是Python解释器设计的历史遗留问题，通常我们用的解释器是官方实现的CPython，要真正利用多核，除非重写一个不带GIL的解释器。
> 
> 所以，在Python中，可以使用多线程，但不要指望能有效利用多核。如果一定要通过多线程利用多核，那只能通过C扩展来实现，不过这样就失去了Python简单易用的特点。
> 
> 要实现多任务，通常我们会设计Master-Worker模式，Master负责分配任务，Worker负责执行任务，因此，多任务环境下，通常是一个Master，多个Worker。
> 
> 如果用多进程实现Master-Worker，主进程就是Master，其他进程就是Worker。
> 
> 如果用多线程实现Master-Worker，主线程就是Master，其他线程就是Worker。
> 
> 多进程模式最大的优点就是稳定性高，因为一个子进程崩溃了，不会影响主进程和其他子进程。（当然主进程挂了所有进程就全挂了，但是Master进程只负责分配任务，挂掉的概率低）著名的Apache最早就是采用多进程模式。
> 
> 多进程模式的缺点是创建进程的代价大，在Unix/Linux系统下，用fork调用还行，在Windows下创建进程开销巨大。另外，操作系统能同时运行的进程数也是有限的，在内存和CPU的限制下，如果有几千个进程同时运行，操作系统连调度都会成问题。
> 
> 多线程模式通常比多进程快一点，但是也快不到哪去，而且，多线程模式致命的缺点就是任何一个线程挂掉都可能直接造成整个进程崩溃，因为所有线程共享进程的内存。在Windows上，如果一个线程执行的代码出了问题，你经常可以看到这样的提示：“该程序执行了非法操作，即将关闭”，其实往往是某个线程出了问题，但是操作系统会强制结束整个进程。

---

### 20220616补充！

进程，是线程的容器！（一个进程可以包含多个线程）
线程，是程序执行流的最小单元！

- 守护线程：独立周期执行，为其它线程提供服务的线程。随主进程时同步启动终止。所以，==如果希望在主线程退出前处理其他资源（也就是不希望啪-的一下就关掉了）尽量采用非守护线程方式！==

---
### Python3的多线程模块

模块包括： `_thread, threading, queue` 等！

#### 1. _thread模块

在python3中已被废弃！！！不详谈

#### 2. threading模块

取代了thread模块，提供更多高级线程功能。

**4.1 函数**：

| 名称 | 使用 |
| -------- | -------- |
| active_count()     | 返回当前活动线程数     |
| current_thread() | 返回当前线程对象 |
| get_ident() | 返回当前线程id（非零整数） |
| enumerate() | 枚举函数，列表形式返回当前活动的所有线程对象 |
| main_thread() | 返回主线程对象（主线程，Python解释器启动的线程） |
| settrace(func) | 为从线程模块启动的线程设置追踪，func为自定义函数名 |
| setprofile(func) | 为从线程模块启动的线程设置配置文件功能 |
| stack_size([size]) | 返回创建新线程时使用的线程堆栈大小，size可0~32768（32kb），默认值为0！| 

**4.2 常量**：

模块提供TIMEOUT_MAX常量。
当Lock.acquire(), RLock.acquire(), Condition.wait()等方法超过常量限定的时间后，就会抛出OverflowError错误！

```
import threading

print(threading.active_count())  # 1,因为只有编译器创建的主进程

threading_lst = threading.enumerate() # 返回了所有线程对象
print(threading_lst, '\n', threading.TIMEOUT_MAX) # 打印返回对象与最大延时
```

**4.3 类**：

threading 提供了 Thread, Lock, RLock, Condition, Semaphore, Event 等功能。

:::success

**4.3.1 Thread**

Thread类通过调用用户指定函数func，独立生成一个活动的线程！
方法只有两种：
1. Thread创建实例对象时把func以参数形式传入；
2. 继承Thread类重写run()方法，调用func。（注意，Thread的派生类中只有__init__()和run()可以重写！其他你别碰）

Thread类构造函数形式：

```
thread = Thread(group=None, target=None, name=None, args=(), kwargs={}, *, daemon=None)

>> group: 用于保留作为将来扩展功能，通常忽略就ok；
>> target: 设置线程执行的自定义函数func！如target=func；在设置完成后使用 thread.run()进行调用；若target=None，线程不执行任何动作！
>> name: 指定需要执行的进程名称，不指定时自动生成一个Thread_N形式的名称！
>> args: 自定义的func带有参数时，以元组形式传入；
>> kwargs: 自定义的func带有参数时，以字典形式传入；
>> daemon: 当daemon不为None时，通过设置daemon=True或daemon=False来确定是否守护线程；若daemon=None则守护线程会继承父线程的状态！若为False，则不守护主线程，也就是说主线程结束了也会等子线程结束再结束这个进程；若为True，则守护主线程，主线程结束就立马结束进程，不管子线程运行是否结束（这样就会有不稳定性，很可能按着代码顺序主线程走完，但是子线程还在跑就被掐断了）。
```

Thread类的主要成员（方法）有下列：



| 名称 | 使用描述 | 
| -------- | -------- | 
| start()     | 线程启动状态（一个线程只能调用该函数一次），需要在run()前调用！     | 
| run() | 运行线程，使其处于活动状态，在run()方法里执行用户的自定义函数func。此方法可在派生类重写（override) |
| join(timeout=None) | 阻塞调用线程。等待调用该方法的线程对象执行时，一直到该线程执行终止，阻塞才释放。timeout为阻塞时间（秒）；若timeout=None，该操作阻滞直至线程终止！注意该方法需要在run()后运行。|
| name | 线程名，由构造函数设置！（也就是上面的name） |
| ident | 线程id号，若未启动则是None |
| daemon | 若线程为守护线程则返回True，反之返回False。|
| is_alive() | 返回线程是否存在 | 
| setDaemon() | 于Py 3.6已经被遗弃 |

1. 线程对象被创建；
2. 调用start()方法被激活；
3. 调用run()方法执行指定的自定义函数func;
4. （若需要）调用join()阻塞线程；
5. 该线程终止，才能执行后续代码。

---

**4.3.1.1 Lock**

Lock类对象为==原始锁==， 一旦某线程获得了锁，则获取它的尝试都会被阻塞直到锁被释放；
```
Lock.acquire()：建立一个锁，成功则返回True，失败返回False；
Lock.release()：释放一个锁；
```

---

**4.3.1.2 RLock**

RLock为可==重复锁==，可以被一个线程多次获取，可避免Lock多次锁定产生的死锁问题（确实，如果没有方法调用查看我是否锁定了不就尴尬了吗！）
==与Lock的不同：可以嵌套调用锁定和解锁方法！==
```
RLock.acquire():获取锁，同于Lock；
RLock.release():释放锁；
```

---

**4.3.1.3 Condition**

Condition对象提供了对复杂线程同步问题的支持功能。它也提供锁类似的acquire(), release()方法，且提供wait(), notify()方法。

---

**4.3.1.4 Semaphore**

一个该对象管理一个计数器，该计数器由每个acquire()调用递减，release()调用递增！

:::

---

#### 线程池

通常我们会利用threading.Thread来创建多个线程，但是有时候线程太多反而可能导致反复切换线程结果效率更低了！这时候我们通常会选择线程池来管理！

```
线程池： concurrent.futures.ThreadPoolExecutor

pool = ThreadPoolExecutor(num) // num表示线程池中最大线程数

pool.submit(task, args=()) // 将task和参数args函数安排进线程池！

pool.shutdown(True)  // 同join,阻塞主线程以防其他线程还没跑完主线程就提桶跑路了！
```

![](https://i.imgur.com/T1w5z68.png)
> 蚂蚁的课程中的写法

---
### 小实践！
在pycharm里简单尝试了一下，可以查看！

```
import threading
from time import *
import datetime

# print(threading.active_count())  # 1,因为只有编译器创建的主进程

# threading_lst = threading.enumerate() # 返回了所有线程对象
# print(threading_lst, '\n', threading.TIMEOUT_MAX, '\n', threading.current_thread()) # 打印返回对象与最大延时
# print(threading.current_thread() == threading.main_thread())

# temp_thread = threading.Thread()

'''
模拟一下非进程模块，直接嗯写程序买票
'''

tickets = [
    ['2018-4-7 8:00', 'beijing', 'shenyang', 10, 120],
    ['2018-4-7 9:00', 'shanghai', 'ningbo', 5, 100],
    ['2018-4-7 12:00', 'tianjin', 'beijing', 23, 55],
    ['2018-4-7 14:00', 'guangzhou', 'wuhan', 0, 231],
    ['2018-4-7 16:00', 'chongqing', 'chengdu', 3, 192],
    ['2018-4-7 16:30', 'shanghai', 'shenzhen', 32, 780],
    ['2018-4-7 18:00', 'wuhan', 'changsha', 10, 210]
]

def buy_ticket(name, nums, time, start_station):
    i = 0
    sleep(1)   # 执行暂停1s,方便查看后续函数耗时;
    for record in tickets:
        if record[0] == time and record[1] == start_station:
            if record[3] >= nums:
                tickets[i][3] = record[3] - nums
                return nums
            else:
                print('{} 现存票数不够，无法购买'.format(name))
                return -1
        i += 1

    print('%s 今日无票'%(name))
    return -1


# if __name__ == '__main__':
#     print("开始运行... ", datetime.time())
#     res_1 = buy_ticket('张三', 3, '2018-4-7 9:00', 'shanghai')
#     if res_1 > 0:
#         print('购入了%d张票！'%(res_1))
#     res_2 = buy_ticket('张问', 1, '2018-4-7 14:00', 'guangzhou')
#     res_3 = buy_ticket('Jone', 3, '2018-4-7 18:00', 'wuhan')
#
#     print("运行结束...", datetime.time())
#     print("剩余票数为：")
#     for tic in tickets:
#         print(tic)

'''
可以发现不分线程确实可以做到，但是无法进行并发处理，后面的人抢的就没这么快！
所以我们使用并发处理的多线程技术尝试！
'''

if __name__ == '__main__':
    print("开始运行... ", ctime())
    thread_1 = threading.Thread(target=buy_ticket, args=['张三', 3, '2018-4-7 9:00', 'shanghai'])
    thread_2 = threading.Thread(target=buy_ticket, args=['Jone', 3, '2018-4-7 18:00', 'wuhan'])
    thread_3 = threading.Thread(target=buy_ticket, args=['张问', 1, '2018-4-7 14:00', 'guangzhou'])

    thread_1.start()
    thread_2.start()
    thread_3.start()

    thread_1.join()
    thread_2.join()
    thread_3.join()

    print("运行结束...", ctime())
    print("剩余票数为：")
    for tic in tickets:
        print(tic)
```

> 好处在哪？并行处理可以省时间！！！比较两边时间你会发现快很多！ 
> 但是也有问题，你会发现上面的代码要写三个线程，如果我要运行1000000个呢？？
> 下面我们来利用继承Thread类修改一下写法使其更灵活！


```
class MThread(threading.Thread):
    def __init__(self, target, args):
        threading.Thread.__init__(self)
        self.target = target   # 要运行的函数
        self.args = args       # 函数的参数列表

    def run(self):
        self.target(*self.args)


if __name__ == "__main__":
    visitors = [
        ['张三', 3, '2018-4-7 9:00', 'shanghai'],
        ['Jone', 3, '2018-4-7 18:00', 'wuhan'],
        ['张问', 1, '2018-4-7 14:00', 'guangzhou']
    ]

    class_do_list = []

    print("start >>> ", ctime())
    for each in visitors:
        get_one = MThread(target=buy_ticket,args=each)
        class_do_list.append(get_one)

    for i in range(len(class_do_list)):
        class_do_list[i].start()
    for i in range(len(class_do_list)):
        class_do_list[i].join()
    print("Over >>> ", ctime())
    print("Left . . . \n",)
    for tic in tickets:
        print(tic)
```

> 这样就算调用一千亿个线程，都可以直接函数解决了！

---

### 线程同步

和sql一个道理，不同线程（对应sql的不同事务）同时争抢共享数据时，就会导致数据出现异常现象！（那咱也知道sql用了锁），而这边可以通过**线程同步**实现！

**GIL锁**：由于Cpython采用的是单核CPU技术，解释器上带有GIL（global interpreter lock)锁，它会保护代码的线程独立使用共享数据！
> 也就是说，==CPython解释器的一个进程，同一时间只有一个获得GIL保护的线程在执行==！即，它的多线程是并发，而非并行！是个假的多线程！

- 那么如何绕过这个大坎呢！
- 在CPython内，最常见的是利用multiprocessing模块！

#### 加锁！

我们可以通过加锁解决类似SQL事务读写一样的线程竞争问题（当然它还是建立在并发的基础的，并没有帮助实现并行）。在上个例子里，我们可以如下加入线程锁。

```
class MThread(threading.Thread):
    def __init__(self, target, args):
        threading.Thread.__init__(self)
        self.target = target   # 要运行的函数
        self.args = args       # 函数的参数列表
        self.threadLock = threading.Lock()

    def run(self):
        self.threadLock.acquire()   # 上锁
        self.target(*self.args)
        self.threadLock.release()   # 解锁
``` 

当然，上面这个语句我们可以使用熟悉的with语句来替换！

```
def run(self):
    with self.threadLock:   
        self.target(*self.args)
```
> 和打开文件类似，使用with执行，with语句会在代码块执行前自动获取锁，且在执行结束后自动解锁！

---

#### 防止死锁！

如果在Lock锁中对一个线程反复加锁，就会导致死锁问题！
怎么解决？别傻子一样用Lock锁来锁去的！或者，我们改用RLock锁！它不会造成死锁问题！


---

### 线程队列模块 - queue


> 我们之前只使用过collections.deque作为双向队列，然而queue也是可以实现线程队列的！接下来我们会再探索一次queue模块！

**queue模块**


|  类名 | 功能 |
| -------- | -------- | 
| queue.Queue(maxsize=0)     | FIFO队列（先进先出顺序队列），若maxsize=0则表示大小无限！ |
| queue.LifoQueue(maxsize=0) | LIFO队列（栈！） |
| queue.PriorityQueue(maxsize=0) | 优先队列！元素保持排序但是先检索最低值元素！ |

**queue.Queue(maxsize=0)成员**



| 方法 | 描述 |
| -------- | -------- |
| qsize() | 返回队列大小     |
| put(item, block=True, timeout=None) | 放入元素item。若block=True,且timeout=None,则队列元素满时会禁止进队列直到队列为空。如果设置了timeout,则为阻塞秒数。 |
| get(block=True, timeout=None) | 移除队首元素并返回。 |
|join() | 阻塞线程直至队列中所有元素都被获取并处理。 |
| empty() | 队列空则返回True；反之返回False。 |
| full() | 队列满则返回True；反之返回False。 | 
> LifoQueue 以及 PriorityQueue类除去get()移除元素不同外其余基本相同。

- Queue保证了多线程竞争下数据的安全性！！！

---

### 并发进程模块 - multiprocessing

> 由于IO密集型需要实际上真正运行消耗算力不大，我们很少会用多进程去处理，而是使用多线程；反之，计算密集型要求每个CPU进行高算力工作，我们更倾向于使用多进程去处理！

:::success
**1. 基于 Process 的多进程**

```
multiprocessing.Process类，用于创建进程！

Process(group=None, target=None, name=None, args=(), kwargs={}, *, daemon=None)

和多线程的Thread参数一致。
```

Process类的方法和属性也与Thread类相当类似。



| 方法或属性 | 描述 | 
| -------- | -------- | 
| start()     | 启动线程     |
| run() | 执行进程需要处理的自定义函数 |
| join([timeout]) | 阻塞进程直至调用完毕或超过超时长度 |
| is_alive() | 返回该进程是否空闲的状态值True/False |
| terminate() | 强制结束进程 |
| name | 进程名 |
| daemon | 若为守护进程则返回True，反之返回False。 |
| pid | 进程号 |
| exitcode | 判断子进程是否退出，若为None表示子进程未结束 |
| authkey | Process对象创建时生成授权键 |
| sentinel | 提供哨兵事件处理句柄数字 |

- 总的来说就是，Process类的使用方法和多线程的Thread类基本一模一样！！！

![](https://i.imgur.com/2gH8NMi.png)

:::

:::info
**2. 基于 Pool 的多进程**

> Process适用于小规模的并发进程。当产生大规模并发进程时，大量内存资源被消耗，容易出现异常！
> 为了解决这个问题，引入 ==**进程池**== 概念。**“进程池是资源进程、管理进程组成的技术的应用。”**

```
multiprocessing.Pool(processes,initializer,initargs,maxtasksperchild,context)

>> processes：
可选参数，要使用的进程数量。若为None则默认使用由os.cpu_count()返回数字！）

>> initializer：
可选参数，若参数不为None，则在每个工作进程启动时调用initializer(*initargs)!

>> maxtasksperchild：
可选参数，一个工作进程在退出前可以完成的任务数！
在完成标定数量后会被释放并被新的工作进程替代。
默认值为None也就是说工作进程的生存时间和进程池一样长！

>> context：
可选参数，指定用于启动工作进程的上下文。
```

- Pool类常用方法

| 方法 | 描述 | 
| -------- | -------- | 
| apply(func, args, kwds)     | 每次向进程池追加一个进程，三个参数都是可选参数默认为None     |
| apply_async(func, args, kwds, callback, error_callback) | 一次性向进程池追加任务直至进程池上限！可以实现真正的多进程并发使用。 |
| close() | 阻止提交新进程到进程池中。可以让工作进程正常退出。 |
| join() | 阻塞等待工作进程退出。在使用join()前必须先使用 close() 或 terminate()。 |
| terminate() | 强制停止池中进程运行！ |
| shutdown() | 填入若为True则主进程会等待进程池执行完毕再开始（主进程阻塞），False则不会等待。 |
| map(func, iterable, chunksize=None) | 实现自定义函数func与可迭代对象成员的映射关系。该方法将迭代器切成多块然后作为单独的任务提交给进程池！切块的大小由chunksize设置。 |

> 简单的代码仍旧丢在pycharm！

:::

---
#### 进程的生成模式： fork & spawn

设置方法：在使用多进程的最开始确定：
```
multiprocessing.set_start_method("fork" or "spawn")
```
- fork：github那边也是这个词，意思就是你生成子进程的时候，子进程会将主进程的全部状态拷贝（主进程的变量，内存中的缓存），相对而言子进程写起来就快，但是容易出现变量名重复等影响阅读的问题！

- spawn：生成的子进程并不会直接拷贝主进程的状态，需要自己将需要的变量等作为参数传入！这样更考验写代码的水平，但是阅读上会舒服很多！

==怎么选择？水平上去就选spawn。并且，python3.8之后版本，fork只在macOS系统可用，windows选择会报错！==

总之以后写代码需要注意，要像写C++一样，使用：

```
int main(){}    // C++

if __name__ == "__main__":   // Python
```

#### 进程间的共享问题

无论是fork还是spawn，它们都是以传值的方式往子进程传入变量。也就是说子进程上对主进程的变量进行修改编辑是不行的。那么我们要如何实现变量等的共享呢？

- 1. 可利用到Python以C为底的基础了。我们可以设置C风格变量（指针的感觉）这样就可以实现主进程与子进程的共享了！

![](https://i.imgur.com/j0kk3Lr.png)
![](https://i.imgur.com/7EadVP0.png)

---

- 2. 可以利用multiprocessing的Manager或者Pipes模块进行变量管理！

![](https://i.imgur.com/G4ePkkx.png)

![](https://i.imgur.com/SbBtZ6B.png)


---
### 单例模式

```
class Fooer:
    pass

foo1 = Fooer()
foo2 = Fooer()
print(foo1, '\n', foo2)

>> <__main__.Fooer object at 0x0000027BD98BEEF0> 
>> <__main__.Fooer object at 0x0000027BD98BEA10>
```
> 我们在每次实例化都会创建一个对象，它们并非同一个对象。但是有时候我们会被要求都使用同一个对象！这时候我们就需要使用到单例模式！
> 单例模式就是在存放对象定义代码的文件中创建一个对象，然后导入的时候直接导入这个对象并且对这个对象进行操作就好！

```
class Singleton:
    instance = None

    def __init__(self, name):
        self.name = name

    def __new__(cls, *args, **kwargs):
        '''
        创建新对象时检查是否已经有对象！已有则直接返回该对象（不再新创建！）
        '''
        with cls.lock:   # 申请锁以防多线程时候出现混乱！
            if cls.instance:
                return cls.instance
            cls.instance = object.__new__(cls)
            return cls.instance


obj1 = Singleton('wei')
print(obj1)

obj2 = Singleton('wei')
print(obj2)

>> <__main__.Singleton object at 0x000001AA6CD247C0>
>> <__main__.Singleton object at 0x000001AA6CD247C0>

/ 二者是同一个对象！！！非要说就相当于是同时指向这个对象的指针的感觉
```
> 面试很有可能就会问你什么是单例模式，高频！所以上面的单例模式的模板（其实就是__new__(cls)的写法）一定要很熟悉！

---

### 多线程 & 多进程速度比较！

当进行计算密集型时，多进程 > 一般 > 多线程（因为线程切换也要资源！）
代码在下面：

```
import sys
import os
import threading
import time
import math
from time import *
import datetime
import queue
import random
import multiprocessing
import utils
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor
sys.path.append(r"C:\Users\99531\PycharmProjects\MyProjects\Mine\Samples")
import MyPackages


'''
使用单线程，多线程，多进程来计算大数字是否为素数的时间比较！
'''

def is_prime(number):
    if number == 2:
        return True
    if number < 2:
        return False
    if number % 2 == 0:
        return False
    sqrt_n = int(math.floor(math.sqrt(number)))
    for i in range(3, sqrt_n + 1, 2):
        if number % i == 0:
            return False
    return True

@MyPackages.my_decorator
def single_thread(primes):
    for num in primes:
        is_prime(num)


@MyPackages.my_decorator
def multi_thread(primes):
    with ThreadPoolExecutor() as pool:
        pool.map(is_prime, primes)


@MyPackages.my_decorator
def multi_process(primes):
    with ProcessPoolExecutor() as pool:
        pool.map(is_prime, primes)

if __name__ == "__main__":
    PRIMES = [112272535095293] * 50
    print('Single thread...')
    single_thread(PRIMES)
    print('\n')
    print('multi_thread...')
    multi_thread(PRIMES)
    print('\n')
    print('multi_process...')
    multi_process(PRIMES)
    

------------------------------
Single thread...
函数开始运行...
运行完成！函数运行时间为:18.8849 s


multi_thread...
函数开始运行...
运行完成！函数运行时间为:20.4623 s


multi_process...
函数开始运行...
运行完成！函数运行时间为:4.6550 s

进程已结束，退出代码为 0
```

---

## 17. 回调函数

函数的名字可以当作另一个函数的名字！（啊这个在前面线程和进程对象初始化时候不就有吗，target=func)这样的函数就叫做回调函数！

回调函数的作用：
1. 让系统在恰当时候去调用某个函数，起通知作用（比如设置每天上午8点的闹钟，就是到了八点然后就调用了闹钟函数)
2. 使代码更灵活（比如C++和Python的排序函数都可以传入一个函数去改变排序基准！）

---

## 18. 协程

协程，不同于线程和进程，是程序员通过代码搞出来的东西。协程又被称为微线程，是用户态内的上下文切换技术，其实就是通过仅仅一个线程实现代码块的相互切换执行（也就是单线程内代码不是完全按着上下顺序执行！）。

==剩下的部分在收藏夹的异步编程！==

蚂蚁的协程部分：
![](https://i.imgur.com/hvC2cQp.png)

异步IO库： asyncio
![](https://i.imgur.com/LN1udJ2.png)
==> 在python3.7之后，上面的: asyncio.get_event_loop() 与 loop.run_until_complete() 可以直接用一句 asyncio.run() 解决！==

---

**==路飞内容：==**

异步编程：可以防止单线程的阻塞！

:::success
### 实现协程的方法简括

> 1. greenlet (早期)
> 2. yield 关键字
> 3. asyncio装饰器
> 4. async, await关键字（Python3.5之后）

---
#### 1. greenlet

看代码，感觉写起来不是很舒服！

---
#### 2. yield关键字

看代码。不重要。

---
#### 3. asyncio模块

代码也在。这个牛逼！它遇到IO阻塞会自动切换！

---
#### 4. asyncio与await的关键字！（用这个）

和第三个一个意思，但是它好写好读！所以以后要就用这个了！
:::

---

### 异步编程

#### 1. 事件循环

就是一个检测并执行某些代码的循环。

```
loop = asyncio.get_event_loop() // 生成/获取一个事件循环！

loop.run_until_complete(任务)  // 将任务放入任务列表

---

'''
诚然为了方便，在Python3.7以后的版本，上面两句话可以合并为下面：
'''

asyncio.run( 任务 )
```

---
#### 2. 快速上手

协程函数：定义时使用 `asyncio def func()` 的函数;
协程对象：执行协程函数得到的对象。

```
asyncio def func():    // 协程函数
    pass
    
result = func()    // 协程对象， ！！注意，设置协程对象时函数是不会执行的！
                                这个不表示运行函数且赋返回值，毕竟上面函数都没有返回值。

asyncio.run( result )   // 就这样三步走就完事了！
```
> 执行协程函数创建协程对象，函数内部代码不会运行。

---

#### 3. await 关键字（async wait）

await + 可等待对象！

可等待对象：（IO等待）
1. 协程对象；
2. future对象；
3. task对象。

```
示例1： await+协程对象

async def func():
    print("协程函数测试...")
    response = await asyncio.sleep(2)
    print("结束测试")
    

if __name__ == "__main__":
    result = func()  # 这里协程函数创建协程对象，实际上函数并未执行！
    asyncio.run(result)
    
    
---

示例2： await+协程对象

async def other():
    print("start")
    await asyncio.sleep(2)
    print("end")
    return "返回值"
async def fun1():
    print("执行协程函数内部代码")

    # 遇到IO挂起，跳转至other()协程函数！
    res = await other()
    print("IO结果：", res)

if __name__ == "__main__":
    asyncio.run( fun1() )
```
==**await 就是等待对象的值得到结果之后再往下走！**==

---
#### 4. Task对象

task对象：用于调度协程,即在事件循环中添加多个任务的！

创建Task对象：`asyncio.create_task()`(推荐), `loop.create_task()`或者`asyncio.ensure_future()`也可以创建（最前面的例子就是）但是较为低级。

```
'''
Task对象： 使用 asyncio.create_task(函数名) 创建。
'''
async def func_task():
    print(1)
    await asyncio.sleep(2)
    print(2)
    return "Who you are？"

async def main():
    print("主函数开始")

    # 创建Task对象，将要执行任务添加到事件循环！
    task1 = asyncio.create_task(func_task())
    task2 = asyncio.create_task(func_task())

    print("主函数结束！")
    ret1 = await task1
    ret2 = await task2
    print(ret1, ret2)


if __name__ == "__main__":
    asyncio.run( main() )
    
    
输出：
主函数开始
主函数结束！
1
1
2
2
Who you are？ Who you are？
```
> 上面的写法是一个一个创建Task对象且添加，但是任务很多的时候肯定不能这样！就像前面线程进程。
> 怎么办呢？asyncio的Task对象可以通过列表式创建，然后使用独特的`asyncio.wait(Task_list)` 来一次性加入事务循环！

```
上面的代码修改：
async def main():
    '''
    一个一个创建和列表式创建都有，拿列表式创建当示例即可。
    '''
    print("主函数开始")

    # 修改，使用列表式简化书写
    
    task_list = [
        asyncio.create_task(func_task(), name='n1'),
        asyncio.create_task(func_task(), name='n2')
    ]

    # 修改，使用wait()函数一次性加入事务循环。
    # pending没什么意义，done是任务完成后的返回值。
    
    done, pending = await asyncio.wait(task_list)
    print("主函数结束！")
    print(done)


if __name__ == "__main__":
    asyncio.run( main() )
```

---

#### 5. asyncio.Future对象（用的少）

Future类是Task类的基类！它是等待异步结果的更低级的接口。（我们的await就是等task的结果，future就是它的基类所以也能等待）

:::success

concurrent.futures.Future对象

看名字和上面的asyncio的很像，实际上半毛钱关系都没有！这个是在进程池或者线程池里面用的！

:::

---

#### 6. 异步迭代器

迭代器：有iter()和next()方法的对象。

异部迭代器：实现了 `__aiter__()`和`__anext__()`方法，且返回为`awaitable`对象的迭代器。

---

#### 7. 带有aiohttp & aiofiles（协程爬虫&文件写入）的协程模板！

见下面的例子：爬取西游记

```
'''
爬取小说！爬取百度小说的西游记！


# http://dushu.baidu.com/api/pc/getCatalog?data={"book_id":"4306063500"}    -> 所有章节名称与cid
# http://dushu.baidu.com/api/pc/getChapterContent?data={"book_id":"4306063500","cid":"4306063500|1569782244","need_bookinfo":1}
# -> 某一章节的内容！我们所需要的！

也就是说，我们可以通过所有章节的cid来拼接出来下面的网页并请求！

1. 同步操作：访问getCatalog 拿到所有章节的cid和名称；
2. 异步操作：访问getChapterContent 下载所有文章内容
'''
import os
import time
import json
import asyncio
import aiohttp
import aiofiles
import requests


async def getCatalog(url, b_id):
    """
    这个操作应该是同步且只做一次的。目的是返回所有章节名称与cid
    """
    tasks = []
    resp = requests.get(url)
    dic = resp.json()
    for each in dic['data']['novel']['items']: # 对应每一个章节的内容！
        # print(each['title'], '\t', each['cid']) # 确实无误！
        title = each['title']
        cid = each['cid']
        tasks.append(asyncio.create_task(aiodownload(cid, b_id, title)))
    await asyncio.wait(tasks)


async def aiodownload(cid, b_id, title):
    data = {
        'book_id': b_id,
        'cid': '{}|{}'.format(b_id, cid),
        'need_bookinfo': 1
    }
    data = json.dumps(data)
    url = 'http://dushu.baidu.com/api/pc/getChapterContent?data={}'.format(data)

    # 异步请求用aiohttp
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as resp:
            dic = await resp.json()
            os.makedirs(os.path.join(os.getcwd(), '西游记'), exist_ok=True)
            save_path = os.path.join(os.getcwd(), '西游记')
            async with aiofiles.open(os.path.join(save_path, title), mode='w', encoding='utf-8') as f:
                await f.write(dic['data']['novel']['content'])


if __name__=="__main__":
    book_id = '4306063500'
    url = 'http://dushu.baidu.com/api/pc/getCatalog?data={"book_id":"' + book_id + '"}'
    t1 = time.time()
    asyncio.run(getCatalog(url, book_id))
    t2 = time.time()
    print("下载完成！耗时：", t2 - t1)
```

- 写下来个人认为的规律：
- 1. async 就是指我这个函数用得上协程（等待别的），总之我进行网页，文件等io操作的话就能用的上！
- 2. await 写在io操作前面，就是表示我需要等待你这个操作！
- 3. 创建aiohttp.CilentSession() 和 session.get() 都用了 with 语句，好处就是它会自动关闭连接！当然你会发现，除了这里需要前面跟上一个 async 之外，其他的和普通语句都是一模一样的！并且，你也可以不用with语句而是直接： `session = aiohttp.CilentSession()`, 但这样你容易忘记关闭就惨了！所以还是**推荐能用with就用with**！

---

## 19. 代码测试

### 1. doctest模块

doctest： documentation test （文件测试）

> 这个没这么好用，看看我写的实例代码就可以了。

---

### unittest模块

模块带有四个基本类：

- TestLoad:加载测试用例，返回测试套件；
- TestSuite:创建测试套件；
- TextTestRunner：运行测试用例；
- TextTestResult：提供测试结果信息。

> 但是实际上我们会用的更多的是TestCase类！下面我们介绍这个类。



| 方法 | 功能 | 
| -------- | -------- | 
| assertEqual(a, b)     | 若测试a与b结果不相等就给出错误信息     |
| assertNotEqual(a, b)     | 若测试a与b结果相等就给出错误信息     |
| assertTrue(x)     | 测试x最后是否为True     |
| assertFalse(x)     | 测试x最后是否为False     |
| assertIs(a, b)     | 测试a最后是否为b     |
| assertNot(a, b)     | 测试a最后是否不为b     |
| assertNone(x)     | 测试x最后是否为None     |
| assertNotNone(x)     | 测试x最后是不为None     |
| assertIn(a, b)     | 测试a是否在b中     |
| assertNotIn(a, b)     | 测试a是否不在b中     |
| assertIsInstance(a, b)     | 测试a是否为b类的一个实例     |
| assertNotIsInstance(a, b)     | 测试a是否不为b类的一个实例     |

```
unittest.main()

对文件进行测试！ // 这个和上面不一样，不用指定assert！但是测的就是文件报错问题了
```
> **这个很管用！在函数和类里写完上面的assert之后可以在主体用这个快速查，见下面**

```
'''
unittest: 更强大！
'''

def test_file():
    x = False
    testcase = unittest.TestCase()
    testcase.assertIsInstance(x, bool)
    print("x是真的！")

if __name__ == "__main__":
    test_file()
    unittest.main()

---

Testing started at 20:57 ...
C:\Program Files\JetBrains\PyCharm Community Edition 2021.1.3\plugins\python-ce\helpers\pycharm\_jb_pytest_runner.py:6: DeprecationWarning: The distutils package is deprecated and slated for removal in Python 3.12. Use setuptools or check PEP 632 for potential alternatives
  from distutils import version
Launching pytest with arguments C:/Users/99531/PycharmProjects/MyProjects/Mine/code_test.py --no-header --no-summary -q in C:\Users\99531\PycharmProjects\MyProjects\Mine

============================= test session starts =============================
collecting ... collected 1 item

code_test.py::test_file PASSED                                           [100%]x是真的！


============================== 1 passed in 0.01s ==============================

进程已结束，退出代码为 0
    
```

---

## 20. 代码打包

光会写代码肯定是不行的！如何把代码打包成可执行文件？

### 1. distutils 模块（我的错，Py3.12已经没有了！！！）

在python中需要打包的对象为：纯python模块，扩展模块与包。

1. 纯python模块：简单说就是python代码编写的模块。.py。
2. 扩展模块：由C，C++编写的Python可调用的模块。
3. 包含其他模块的模块。通过包含在文件系统的一个目录中，通过__init__.py的存在区别于其他目录。

==**distutils模块（包）**==

该模块可以实现：
1. 编写安装脚本（于setup.py里）；
2. 编写安装配置文件；
3. 创建源代码分发文件；
4. 创建一个或多个内置二进制分发文件。

**==主要使用：distutils.core==**



| distutils.core函数或类 | 功能 | 
| -------- | -------- | 
| setup(arguments)     | 可以实现distutils大多数功能, arguments多，下面介绍     | 
| run_setup(script_name, script_args=None, stop_after="run") | script_name是将被读取且使用exec()运行的文件，在期间sys.argv[0]被替换为脚本。script_args为可选的字符串列表，若提供则sys.argv[1]将在调用期间被script_args替换。 |
| Extension | 描述了安装脚本中的单个C或者C++扩展模块 |
| Disrtibution | 描述如何构建，安装和打包Python软件包并分发 |
| Command | 一个Command类实现一个distutils命令 |

**setup（arguments)的参数使用如下：**
```
name : 包的名称
version ： 包的版本号
description ： 单行说明包的基本信息
author ： 包作者名
author_email ： 包作者的email
maintainer ： 参与维护的其他作者
maintainer_email ： 其他作者的email
url ： 包相关网站的主页
download_url ： 包的下载地址
packages ： distutils需要操作包的清单
py_modules ： distutils需要操作的模块清单
scripts ： 要构建和安装的独立脚本文件列表
ext_modules ： 要构建的Python扩展模块列表
classifiers ： 包的类别列表
distclass ： 要使用的分发类
script_name ： setup.py脚本的名称，默认为 argv[0]
script_args ： 提供给安装脚本的参数
options ： 安装脚本的默认选项
license ： 包的许可证
keywords ： 描述性元数据
cmdclass ： 命令名称与Command子类的映射
data_files ： 要安装的数据文件列表
package_dir ： 包到目录名称映射
```

---

#### 基本安装与打包

见示例。另建一个setup.py然后在里面使用distutils.core.setup()函数打包需要的包就可以了！

```
from distutils.core import setup

setup(name="setup_file", version='1.0', py_modules=['code_test_package.py'])
```

完成上述内容后，在windows的话，==先cd到目录路径==再使用命令行 `setup.py sdist` 即可创建一个压缩文件！

**安装文件**

1. 下载压缩包；
2. 解压，在安装包目录路径，使用： `python setup.py install` 即可！之后就可以import了！

- 若觉得命令提示符安装不舒服，可以使用windows环境下的指令： `python setup.py bdist_wininst` 生成 setupFile-1.0.win32.exe 可执行文件！

> ==问题！Py3.12中distutils被废弃了！要到setuptools用了！==

---

### 2. Pyinstaller 模块

[python文件（.py）转换为可执行文件（.exe）操作](https://blog.csdn.net/qq_22433397/article/details/122061613)

```
pip install pyinstaller
```

---

## 21. csv模块

当爬虫爬取表格形式数据时，如何将其存为.csv文件？
下面上个示例代码，很简单。都按着这个来就行！
```
pip install csv


import csv

with open("data.csv", mode='w', encoding='utf-8') as f:
    csvwriter = csv.writer(f)
    ...
    csvwriter.writerow(txt)
```

---

## 22. Python变量命名


python中主要存在四种命名方式：

1. object #公用方法

2. _object #半保护
                 #被看作是“protect”，意思是只有类对象和子类对象自己能访问到这些变量，
                  在模块或类外不可以使用，==不能用’from module import *’导入==。
                #__object 是为了避免与子类的方法名称冲突， 对于该标识符描述的方法，父
                  类的方法不能轻易地被子类的方法覆盖，他们的名字实际上是
                  _classname__methodname。
                  
3. _ _ object  #全私有，全保护
                       #私有成员“private”，意思是只有类对象自己能访问，连子类对象也不能访
                          问到这个数据，不能用’from module import *’导入。
                          
4. _ _ object_ _     #内建方法，用户不要这样定义 

---

## 23. python中的4种作用域

L：Local，局部作用域，也就是我们在函数中定义的变量；

E：Enclosing，嵌套的父级函数的局部作用域，即包含此函数的上级函数的局部作用域，但不是全局的；

G：Globa，全局变量，就是模块级别定义的变量；

B：Built-in，系统内置模块里面的变量，比如int, bytearray等。 

Python变量作用域LEGB原则：从内到外
 
变量的查找顺序：Local 局部变量->Enclosing 外部嵌套作用域->Global 全局作用域->Built-in 内置模块作用域

---

## 24. 如何将数正确四舍五入

> python3中，`round()`函数并非直接四舍五入，而是四舍六入五保偶。如果需要保留整数位是偶数，则直接舍弃后面的5；如果是奇数则入为偶数。如：
```
round(3.5) = 4
round(2.5) = 2 # 保留成偶数！


round(2.675, 2) # 2.67  这是因为计算机对小数保存时有损失
```

所以通过round()是无法实现真正的四舍五入的！要实现四舍五入，我们需要`decimal`模块

```
from decimal import Decimal, ROUND_HALF_UP


def rightRound(num, keep_n):
    if isinstance(num, float):
        num = str(num)
    return Decimal(num).quantize((Decimal('0.' + '0' * keep_n)), 
        rounding=ROUND_HALF_UP)
        
rightRound(2.5) = 3
```

- ceil: 向上取整
- floor: 向下取整
- round: 尽量避免使用

---

## 25.垃圾回收机制

### 1. 引用计数器

底层：环状双向链表（存储不同类型数据时，存储内容可能不一样，但是通常都存储的：

[上一个对象，下一个对象， 类型， 引用个数] -> 称为Pyobject （类型封装结构体）！

对于容器类型还会存储容器内全部元素以及容器长度。

> 当有人引用对象时，引用计数器就发生变化（引用计数器默认为1）！当引用计数器为0则将对象从链表删除，销毁对象，归还内存。

---

### 2. 标记清除

引用计数器的bug：循环引用！

标记清楚：解决引用计数器引起的循环引用问题！

实现：在底层再维护一个链表存放可能存在循环引用的对象（扫描引用计数器的双向链表）。

---

### 3. 分代回收

标记清除的问题： 
    1. 什么时候扫描？
    2. 扫描代价太大怎么办？
    
解决：分代回收（将可能存在循环引用的对象维护成三个链表：
- 0代：0代对象达到700个
- 1代：0代10次 1代扫一次
- 2代：1代10次 2代扫一次

---

### 4. 小结

python中维护一个refchain的双向链表，存储顺序创建对象的类型封装结构体，体内存储有引用次数；当引用次数为0时则回收垃圾（销毁对象，链表中移除）

但是有个问题就是循环引用，所以python又维护了一个标记清除链表。

但是标记清除链表也有个问题就是扫描时间与扫描代价。所以在这个基础上Python引入了分代回收，也就是维护三代链表，0代对象700个扫描一次，1代为10次0代扫描则扫描；2代为10次1代扫描则扫描！

---

