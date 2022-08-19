###### tags: `learn`

# Algorithm note

---
## 有意思 & 复杂

### 1. 螺旋矩阵（数组）
[59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/submissions/)

这道题不是算法，是讲究一个模拟仿真，经常会被问到，但是挺有意思也好理解的。附上思路原链接：[59-螺旋矩阵ii](https://programmercarl.com/0059.%E8%9E%BA%E6%97%8B%E7%9F%A9%E9%98%B5II.html#%E6%80%9D%E8%B7%AF)

```
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        # if n == 1:
        #     return [[1]]
        loop, mid = n//2, n//2
        startx, starty = 0, 0
        count = 1
        nums = [[0]*n for _ in range(n)]
        roundn = 1

        while roundn <= loop:
            for i in range(starty, n-roundn):
                nums[startx][i] = count
                count += 1
            for i in range(startx, n-roundn):
                nums[i][n-roundn] = count
                count += 1
            for i in range(n-roundn, starty, -1):
                nums[n-roundn][i] = count 
                count += 1
            for i in range(n-roundn, startx, -1):
                nums[i][starty] = count
                count += 1
            roundn += 1
            startx += 1
            starty += 1

        if n % 2 == 1:
            nums[mid][mid] = count

        return nums
```

---
### 2. 背包问题（个人理解：解决总数未定的排列&组合问题！）

:::success
**1. 01背包**

有n件物品和一个最多能背重量为w 的背包。第i件物品的重量是weight[i]，得到的价值是value[i] 。每件物品只能用一次，求解将哪些物品装入背包里物品价值总和最大。

对于这种问题，我们是用动态规划解决最为方便！特别地，我们选用一维dp数组：
递归公式：`dp[j] = max(dp[j], dp[j - weight[i]] + value[i])`。

```
def test_1_wei_bag_problem():
    weight = [1, 3, 4]
    value = [15, 20, 30]
    bag_weight = 4
    # 初始化: 全为0
    dp = [0] * (bag_weight + 1)

    # 先遍历物品, 再遍历背包容量
    for i in range(len(weight)):
        for j in range(bag_weight, weight[i] - 1, -1):
            # 递归公式
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i])

    print(dp)
    
 
---
# 其实面试很可能问到2维怎么做，2维其实就是1维基础上把不同物品遍历分开写罢了。但是要注意二维是正向遍历（因为遍历物品可以确保只加入了一次）而一维为了防止反复加入需要反向遍历。

def test_2_wei_bag_problem1(bag_size, weight, value) -> int: 
	rows, cols = len(weight), bag_size + 1
	dp = [[0 for _ in range(cols)] for _ in range(rows)]
    
	# 初始化dp数组. 
	for i in range(rows): 
		dp[i][0] = 0
	first_item_weight, first_item_value = weight[0], value[0]
	for j in range(1, cols): 	
		if first_item_weight <= j: 
			dp[0][j] = first_item_value

	# 更新dp数组: 先遍历物品, 再遍历背包. 
	for i in range(1, len(weight)): 
		cur_weight, cur_val = weight[i], value[i]
		for j in range(1, cols): 
			if cur_weight > j: # 说明背包装不下当前物品. 
				dp[i][j] = dp[i - 1][j] # 所以不装当前物品. 
			else: 
				# 定义dp数组: dp[i][j] 前i个物品里，放进容量为j的背包，价值总和最大是多少。
				dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - cur_weight]+ cur_val)

	print(dp)
```
> 为什么是反向遍历？**倒序遍历是为了保证物品i只被放入一次！**
参考链接：[动态规划：关于01背包问题，你该了解这些！（滚动数组）](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-2.html#%E4%B8%80%E7%BB%B4dp%E6%95%B0%E7%BB%84-%E6%BB%9A%E5%8A%A8%E6%95%B0%E7%BB%84)
:::

:::warning
**2. 完全背包**: ==用于求总个数不定的！！！排列&组合总方法==(定数的我们直接回溯啊！)

0-1背包的一维数组遍历方法提到，需要以重量进行反向遍历，原因是防止一个东西被重复加入。
而，完全背包就是东西可以把一个东西无限加入的，那么就正向遍历啊！

```
    for i in range(len(weight)):
        for j in range(weight[i], bag_weight+1, -1):
            # 递归公式
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i])
```

例：[LEETCODE377. 组合总和 Ⅳ：华为机考第二题！](https://leetcode.cn/problems/combination-sum-iv/)

```
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        """
        LEETCODE377. 组合总和 Ⅳ：华为机考第二题！
        这题也是一个完全背包问题，它如果要改成0-1背包则将遍历顺序反过来即可。
        注意这题不能按照物品顺序遍历因为不同序列被视为不同答案！！！
        """
        dp = [0] * (target+1)
        dp[0] = 1

        for i in range(1, target+1):
            for j in nums:
                if i >= j:
                    dp[i] += dp[i-j]

        return dp[-1]
```
:::

![](https://i.imgur.com/awvOPjt.jpg)

---
### 动规DP全览

![](https://i.imgur.com/3BuRcKv.jpg)


---
## 真题

### （华为笔试）关于时间的输出格式

题目直接以图片形式放在下面。要求的输出格式为[11:30, 23:00]之类；
而我当时的输出采用了列表,最后效果为['11:30', '23:00'].
因为输出格式的错误，全部的分基本都没拿到！！！太可惜了
![](https://i.imgur.com/YXum91U.jpg)
![](https://i.imgur.com/qG9qxH0.jpg)
![](https://i.imgur.com/1gdqUUy.jpg)

最后，作为留念还是把答案贴在下面。

```
def func():
    # 贪心求重叠？
    # 输出不是字符串吗？我输出的原本字符串但是显示和答案不一致？？？

    person1 = [each for each in input().split(';')]
    person2 = [each for each in input().split(';')]
    total_time = int(input())
    real_time = total_time / 60
    new_person1 = []
    new_person2 = []
    time_dict = dict()
    for i, each in enumerate(person1):
        new_time_s, new_time_e = int(each[:2]) + float(int(each[3:5]) / 60), int(each[6:8]) + float(int(each[9:]) / 60)
        new_person1.append([new_time_s, new_time_e])
        time_dict[new_time_s] = each[:5]
        time_dict[new_time_e] = each[6:]
    new_person1.sort(key=lambda x: x[0])
    for i, each in enumerate(person2):
        new_time_s, new_time_e = int(each[:2]) + float(int(each[3:5]) / 60), int(each[6:8]) + float(int(each[9:]) / 60)
        new_person2.append([new_time_s, new_time_e])
        time_dict[new_time_s] = each[:5]
        time_dict[new_time_e] = each[6:]
    new_person2.sort(key=lambda x: x[0])

    # 尝试pop（）
    res = []
    while new_person1 and new_person2:
        if new_person1[0][0] > new_person2[0][1]:
            new_person2.pop(0)
        elif new_person1[0][1] < new_person2[0][0]:
            new_person1.pop(0)

        else:
            l = max(new_person1[0][0], new_person2[0][0])
            r = min(new_person1[0][1], new_person2[0][1])
            res.append([l, r])
            if new_person1[0][1] < new_person2[0][1]:
                new_person1.pop(0)
            else:
                new_person2.pop(0)
    
    result = []
    for each in res:
        if each[1] - each[0] >= real_time:
            print('[' + time_dict[each[0]] + ',' + time_dict[each[1]] + ']')
            result.append(1)
            break
    if not result:
        print(result)
```
---

## Else -- 一些有趣的

### 杨辉三角！！！（好神奇的做法）

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
# Algorithm & Data Structure

## 链表

### 基础知识：

1. 处理链表问题常用虚拟节点，如：

```
dummy = ListNode(next=head)
cur_node = dummy
```

这样可以方便不把节点搞混，且在仿真进行时候方便返回表头（直接返回dummy.next）


---

### LC204.两两交换节点（Medium）

link：[Leetcode24. (Medium)两两交换节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

![](https://i.imgur.com/EYC0ZPf.png)
> 在设置虚拟节点后，我们只需要模拟顺序来三步走即可！最后返回虚拟节点的next。


```
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
        
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        dummy = ListNode(next=head)
        pre = dummy

        while pre.next and pre.next.next:
            cur = pre.next
            post = pre.next.next

            cur.next = post.next
            post.next = cur
            pre.next = post

            pre = pre.next.next
        
        return dummy.next
```

---

### 面试题02.07 - 链表相交(Easy??)

link: [面试题02.07 - 链表相交](https://leetcode-cn.com/problems/intersection-of-two-linked-lists-lcci/)

这题实际上也不麻烦，我把网上的做法先附在下面！（因为这个做法可以不考虑谁更短！）

![](https://i.imgur.com/12qoCpL.png)

```
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        curA, curB = headA, headB

        while curA != curB:
            curA = curA.next if curA else headB
            curB = curB.next if curB else headA

        return curA
```

- 实际上，若指定了两个链表谁更长（如说A更长），那么做法还可以增加！
- 思路：我们先分别把长短链表长度遍历一次算出，然后，若二者有交点，则意味二者有公共部分！考虑到链表的概念，二者一定从公共部分开始往后的全部内容都共享！
- 那么，我们先把长链表的指针移动到短链表长度处，因为它两的公共部分长一定小于短链表长！
- 然后，我们从这个指针位和B的表头位开始，一个一个往下遍历对比即可。

![](https://i.imgur.com/LinouwG.png)

---


### LC142.环形链表II（Medium）

[LeetCode141：环形链表I](https://leetcode-cn.com/problems/linked-list-cycle/)

> 此题只需寻找是否有环，方法很简单，设置一快一慢两指针，若有环则快的一定会在环内追上慢的。

而：
[LC142.环形链表II（Medium）](https://leetcode-cn.com/problems/linked-list-cycle-ii/)相对较困难，需要在确认是否有环的同时找到入环位置！！！



---

### LC206.翻转链表（Easy）

这道题不难，但是我们需要会递归与非递归两种写法！我都留在下面（不管是否递归，我们只需注意当前操作的三个节点即可！）

#### 递归

```
        if not head or not head.next:
            return head
        
        p = self.reverseList(head.next)
        head.next.next = head
        head.next = None
        return p
```

#### 利用反转列表

```
        li = []
        
        while head:
            li.append(head)
            head = head.next
        
        for i in range(len(li)-1, 0, -1):
            li[i].next = li[i-1]
        li[0].next = None
        return li[-1]
```


#### 模拟实现

```
        if not head or not head.next:
            return head
        cur = head
        pre = None
        while cur:
            res = cur.next
            cur.next = pre
            pre = cur
            cur = res
        return pre
```


---


## 哈希表

### LC202.快乐数（easy）

[LC202.快乐数](https://leetcode-cn.com/problems/happy-number/)

这道题实际上是考察数学功底。如定义所示，如果是快乐数，那么它一定可以在多次操作后变成1；如果不是，它一定会在多次操作后陷入循环！！！那么很简单，我们用一个哈希表检测其操作后值是否陷入循环，陷入则false，否则继续操作！

```
class Solution:
    def isHappy(self, n: int) -> bool:
        def calc_num(n):
            sum_ = 0
            while n:
                temp = n % 10
                n //= 10
                sum_ += temp ** 2
            return sum_

        res_dic = set()

        while True:
            ans = calc_num(n)
            if ans == 1:
                return True
            else:
                if ans in res_dic:
                    return False
                res_dic.add(ans)
                n = ans
```


---

### LC454.四数相加II（Medium）

[LC454.四数相加II](https://leetcode-cn.com/problems/4sum-ii/)

看起来有点吓人实际上比较简单！
思路如下：

1. 从四个列表中选任意两个（我这里选A，B）用字典遍历统计他们的和与出现次数；
2. 设置counter计数；
3. 遍历另外两个列表找出和，有与之前统计互为相反值则counter加上原本的统计次数即可！

```
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        dic_ab = dict()

        for n1 in nums1:
            for n2 in nums2:
                if n1 + n2 not in dic_ab:
                    dic_ab[n1 + n2] = 1
                else:
                    dic_ab[n1 + n2] += 1

        counter = 0

        for n3 in nums3:
            for n4 in nums4:
                if -1 * (n3 + n4) in dic_ab:
                    counter += dic_ab[-1 * (n3 + n4)]
        return counter
```

---
## 字符串

### 剑指 Offer 05. 替换空格

[剑指 Offer 05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)


下面的实现是基于新字符串的，这确实很简单。如果我们要求空间复杂度O（1）呢？

```
class Solution:
    def replaceSpace(self, s: str) -> str:
        res = ''
        for each in s:
            if each is not ' ':
                res += each
            else:
                res += '%20'
        return res

```
**空间复杂度O（1）做法**

别的不知道，python干不了，因为python字符串除了用内置函数之外无法原地改变！（我的认知）


---

### LC151. 翻转字符串里的单词(Medium)

[151. 翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)

这道题比起算法，更像是模拟过程！所以，解题思路如下：

- 移除多余空格
- 将整个字符串反转
- 将每个单词反转

```
class Solution:
    def reverseWords(self, s: str) -> str:
        # 1. 去除多余空格（头尾，再中间）
        def remove_space(s):
            left, right = 0, len(s) - 1
            while left <= right and s[left] == ' ':
                left += 1
            while left <= right and s[right] == ' ':
                right -= 1

            need_list = []
            while left <= right:
                if s[left] != ' ':
                    need_list.append(s[left])
                elif s[left] == ' ' and need_list[-1] != ' ':
                    need_list.append(s[left])
                left += 1
            return need_list

        # 2. 反转字符串（列表）
        s_list = remove_space(s)
        s_list.reverse()
        s_list.append(' ')

        # 3. 反转里面的单词！
        temp = 0 
        while temp < len(s_list):
            end = temp
            while s_list[end] != ' ':
                end += 1
            s_list[temp:end] = list(reversed(s_list[temp:end]))
            temp = end + 1
        
        # 4. 变为字符串输出
        return ''.join(s_list[:len(s_list) - 1])

```

---

### 剑指 Offer 58 - II. 左旋转字符串

[剑指 Offer 58 - II. 左旋转字符串](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

做法很多！

```
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:
        # method 1: 直接切片
        return s[n:] + s[:n]

        # method 2: 反转后接上
        s = list(s)
        s[0:n] = list(reversed(s[0:n]))
        s[n:] = list(reversed(s[n:]))
        s.reverse()
        
        return "".join(s)

        # method 3: 利用模和下标！
        new_s = ''
        for i in range(len(s)):
            j = (i+n)%len(s)
            new_s = new_s + s[j]
        return new_s
```

---

### KMP算法


#### 简介
[KMP算法](https://programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html)

[知乎KMP介绍（详细）](https://www.zhihu.com/question/21923021)

**下面自己也简单写写，但是肯定不够详细！！！还是要看原文。**

KMP主要应用在字符串匹配上。

KMP的主要思想是当出现字符串不匹配时，可以知道一部分之前已经匹配的文本内容，可以利用这些信息避免从头再去做匹配了。

前缀表代码


#### 例题1： [28. 实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/submissions/)

```
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if not needle:
            return 0
        m, n = len(haystack), len(needle)
        # 1、求next数组
        nxt = [0] * n
        j = 0
        for i in range(1, n):
            while j and needle[i] != needle[j]:
                j = nxt[j - 1]
            if needle[i] == needle[j]:
                j += 1
            nxt[i] = j
        # 2、模式匹配
        j = 0
        for i in range(0, m):
            while j and haystack[i] != needle[j]:
                j = nxt[j - 1]
            if haystack[i] == needle[j]:
                j += 1
            if j == n:
                return i - n + 1
        return -1
```

#### 例题2：[459. 重复的子字符串](https://leetcode-cn.com/problems/repeated-substring-pattern/submissions/)

```
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        # 1. next数组
        def buildNext(s):
            nxt = [0] * len(s)
            j = 0
            for i in range(1, len(s)):
                while j and s[i] != s[j]:
                    j = nxt[j - 1]
                if s[i] == s[j]:
                    j += 1
                nxt[i] = j
            return nxt 

        nxt = buildNext(s)

        # 2. 匹配
        if nxt[-1] != 0 and len(s) % (len(s) - nxt[-1]) == 0:
            return True
        return False
```

#### KMP的要点:构造next数组！
代码放在下面(直接背了)
```
def buildNext(s):
    nxt = [0] * len(s)
    j = 0
    for i in range(1, len(s)):
        while j and s[i] != s[j]:
            j = nxt[j - 1]
        if s[i] == s[j]:
            j += 1
        nxt[i] = j
    return nxt 
```

## 双指针

### LC15. 三数之和（Medium）

[15. 三数之和](https://leetcode-cn.com/problems/3sum/)

这道题的做法有：
1. 哈希表（不推荐）
2. 双指针（三数或者四数和好用）
3. 递归降维（永远的神）

下面贴做法：

```
# 2. 双指针解决

class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        if len(nums) < 3:
            return []
        
        ans = []
        nums.sort()
        for i in range(len(nums) - 2):
            if i == 0 or (i and nums[i] != nums[i-1]):
                j = i + 1
                k = len(nums) - 1
                res = -nums[i]
                while j < k:
                    if nums[j] + nums[k] < res:
                        j += 1
                    elif nums[j] + nums[k] > res:
                        k -= 1
                    else:
                        ans.append([nums[i], nums[j], nums[k]])
                        j += 1
                        k -= 1
                        while j < k and nums[j] == nums[j-1]:
                            j += 1
                        while j < k and nums[k] == nums[k+1]:
                            k -= 1
        return ans
        
# 3. 递归降维

def findNsum(l, r, N, target, List, temp_res, result):
    if N > r - l + 1 or N <= 1 or N * List[l] > target or N * List[r] < target:
        return []

    if N == 2:
        while l < r:
            sum_ = List[l] + List[r]
            if sum_ == target:
                result.append(temp_res + [List[l], List[r]])
                l += 1
                while l < r and List[l] == List[l - 1]:
                    l += 1
            elif sum_ < target:
                l += 1
            else:
                r -= 1


    else:
        for i in range(l, r):
            if i == l or (i > l and List[i] != List[i - 1]):
                findNsum(i + 1,  r, N-1, target - List[i], List, temp_res + [List[i]], result)

result = []
nums.sort()
findNsum(0, len(nums) - 1, 3, 0, nums, [], result)
return result
```


---

## 栈 & 队列

### LC239. 滑动窗口最大值(Hard)

[239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

这题确实比较难理解。

首先，利用了deque实现下标的存储（没错，存的是下标！）把最大元素的下标存在第一位，并且在新元素进入（也就是滑动窗口）时，将所有比该元素小的元素的下标从队列里pop。这样就可以确保第一位永远是最大的；

关于保持窗口长度，我们每次只需要注意不让最大元素下标残留。所以当其下标小于等于 i - k时候， 就将其弹出！

最后。当i也就是下标进行到k - 1开始（也就是形成第一个窗口开始），每走一步都将当前最大值填入答案！代码放在下面。

```
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        res = []
        queue = collections.deque()
        for i, num in enumerate(nums):
            if queue and queue[0] <= i - k:
                queue.popleft()
            while queue and nums[queue[-1]] < num:
                queue.pop()
            queue.append(i)
            if i >= k - 1:
                res.append(nums[queue[0]])
        return res
```


---

## 树！！！

### 遍历（前中后层的递归以及非递归实现）（已弄好）

[二叉树的遍历](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92%E9%81%8D%E5%8E%86.html)

---

### （重！）通过不同遍历顺序构建树！

实际上就是之前遍历的逆问题！也只需要用递归分别完成即可！（我认为）
例题：
[105. 从前序与中序遍历序列构造二叉树（Medium）](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/submissions/)
[106. 从中序与后序遍历序列构造二叉树（Medium）](https://leetcode-cn.com/submissions/detail/248784549/)

> 很好说，因为前序遍历的第一个节点一定是root；相反，后序遍历最后一个节点也一定是root！
> 那么我们找到其在中序的对于index，再划分成小问题分治递归即可！

```
105. code

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
         
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        if not inorder:
            return 
        root_val = preorder[0]
        root = TreeNode(root_val)

        left_in = inorder.index(root_val)
        inorder_left = inorder[:left_in]
        inorder_right = inorder[left_in + 1:]

        preorder_left = preorder[1:len(inorder_left) + 1]
        preorder_right = preorder[len(inorder_left) + 1:]

        root.left = self.buildTree(preorder_left, inorder_left)
        root.right = self.buildTree(preorder_right, inorder_right)
        return root
        
        
106. code

class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        if not postorder:
            return 

        tree_root = postorder[-1]
        root = TreeNode(tree_root)

        separator = inorder.index(tree_root)

        inorder_left = inorder[:separator]
        inorder_right = inorder[separator + 1:]

        postorder_left = postorder[:len(inorder_left)]
        postorder_right = postorder[len(inorder_left):len(postorder) - 1]

        root.left = self.buildTree(inorder_left, postorder_left)
        root.right = self.buildTree(inorder_right, postorder_right)
        return root
```

**类似例题！！**

[654. 最大二叉树（Medium）](https://leetcode-cn.com/problems/maximum-binary-tree/submissions/)
也是通过递归不停构造节点！！可以发现，树和递归真的很配呢！！！
```
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> TreeNode:
        if not nums:
            return

        max_index = nums.index(max(nums))
        root = TreeNode(nums[max_index])

        left_part = nums[:max_index]
        right_part = nums[max_index + 1:]

        root.left = self.constructMaximumBinaryTree(left_part)
        root.right = self.constructMaximumBinaryTree(right_part)
        return root

```
[108. 将有序数组转换为二叉搜索树（Easy）](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)
这道题，一个道理！！！对于升序数组，我们取中间点为当前节点，左边右边分别递归构建子树即可！
代码放下面。
```
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
        if not nums:
            return

        medium = len(nums) // 2
        root = TreeNode(nums[medium])
        root.left = self.sortedArrayToBST(nums[:medium])
        root.right = self.sortedArrayToBST(nums[medium + 1:])
        return root
```

---

### 617. 合并二叉树(Easy)

[617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

这道题我在9月也做过！当时采用的递归是：只要不是两边都没有节点，那么就在没有的地方创建一个0节点用于下一轮递归！代码放下面。

```
def mergeTrees(self, root1: TreeNode, root2: TreeNode) -> TreeNode:
    if not root1 and not root2:
        return 

    if not root1:
        root1 = TreeNode(0)
    elif not root2:
        root2 = TreeNode(0)

    newroot = TreeNode(root1.val + root2.val)
    newroot.left = self.mergeTrees(root1.left, root2.left)
    newroot.right = self.mergeTrees(root1.right, root2.right)
    return newroot
```

这个做法确实可以做出，但是如果是很极端的情况呢？（第一棵树只有左，第二棵树只有右）这种情况我们还是得递归max（len(tree1), len(tree2)）次！！！！！！

那么换个想法，我们是否可以让递归只发生在两边都有节点的情况；如果不是则直接让单边有节点的成为新节点并且结束当前递归！！！！
跑起来的情况证实确实会好很多！！！代码在下面。
```
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
class Solution:
    def mergeTrees(self, root1: TreeNode, root2: TreeNode) -> TreeNode:
        if not root1 and not root2:
            return
        elif root2 and not root1:
            root = root2

        elif root1 and not root2:
            root = root1

        elif root1 and root2:
            root = TreeNode(root1.val + root2.val)
            root.left = self.mergeTrees(root1.left, root2.left)
            root.right = self.mergeTrees(root1.right, root2.right)

        return root
```

---

### 98. 验证二叉搜索树(Medium) & 530. 二叉搜索树的最小绝对差(Easy)

[98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

这题9月的时候显然因为知识储备不够没有做出来！！！
但是！现在不一样了！！

**众所周知，二叉搜索树的中序遍历应该是从小到大的序列！！**
也就是说，我中序遍历一编再去确认这个序列是否从小到大就可以了！
代码如下：

```
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        # 二叉搜索树应该满足中序遍历的从小到大顺序！！！！
        def inOrder(root, res):
            if not root:
                return
            inOrder(root.left, res)
            res.append(root.val)
            inOrder(root.right, res)
        
        res = []
        inOrder(root, res)
        for i in range(1, len(res)):
            if res[i] <= res[i-1]:
                return False
        return True
```

[530. 二叉搜索树的最小绝对差](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/)

这道题也是一个道理，利用中序遍历的递增性很好解决！

```
class Solution:
    def getMinimumDifference(self, root: TreeNode) -> int:
        def inOrder(root, res):
            if not root:
                return
            inOrder(root.left, res)
            res.append(root.val)
            inOrder(root.right, res)

        res = []
        inOrder(root, res)
        min_val = float('inf')
        for i in range(1, len(res)):
            min_val = min(min_val, res[i] - res[i-1])
        return min_val
```
**中序遍历的递增性真的是二叉搜索树的很好的性能！！！**

---
### 669. 修剪二叉搜索树(Medium)

[669. 修剪二叉搜索树](https://leetcode-cn.com/problems/trim-a-binary-search-tree/)

这道题其实有一定的逻辑难度。因为就算当前节点的值不在[low, high]区间，它的子节点也有可能在！（因为二叉搜索树嘛）。所以我们不需要使用迭代去每步完成，我认为，递归才是树的归宿！

归功于递归，我们只需要处理每层逻辑：
1. 若当前层无节点，返回None；
1. 若节点.val < low,那么检查其右子节点；
1. 反之，若节点val > low,那么检查其左子节点；
1. 满足条件，则进入下一层，检查左右节点。
    
代码：
```
class Solution:
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        if not root:
            return

        if root.val <= high and root.val >= low:
            root.right = self.trimBST(root.right, low, high)
            root.left = self.trimBST(root.left, low, high)
            return root
        elif root.val < low:
            return self.trimBST(root.right, low, high)
        else:
            return self.trimBST(root.left, low, high)
```


---
### 知识树图

![](https://i.imgur.com/AnDH7Lo.jpg)


---

## 回溯算法（难）

![](https://i.imgur.com/LRQOdv4.png)
![](https://i.imgur.com/IL6HRnM.png)
![](https://i.imgur.com/Xyh78rP.png)

---
### 77.组合（Medium）

[LC77.组合](https://leetcode-cn.com/problems/combinations/)

排列和组合，是回溯算法常客。思想就是每次取出来一个再用for循环和剪枝配合，达到回溯效果。代码在下面：
```
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        ans = []
        temp = []

        def backtracking(n, k, index):
            if len(temp) == k:
                ans.append(temp[:])
                return
            
            for i in range(index, n-k+len(temp)+2):
                temp.append(i)
                backtracking(n, k, i+1)
                temp.pop()
        backtracking(n, k, 1)
        return ans
```
诚然，我们也可以用python库作弊：
```
# 作弊：
# import itertools
# return itertools.combinations(range(1, n+1), k)
```
---
### 216.组合总和III (Medium)

[216.组合总和III](https://leetcode-cn.com/problems/combination-sum-iii/submissions/)

这题也是个组合问题，所以显然使用回溯即可解决；唯一的难点我认为应该是剪枝（其实也不难，剩下的位置比剩下能选择的数少不就行了吗！）代码如下：

```
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        ans = []
        tmp = []
        def backtracking(k, n, start):
            if k >= n:
                return
            if len(tmp) == k and sum(tmp) == n:
                ans.append(tmp[:])

            for i in range(start, 10):
                tmp.append(i)
                if 9-i >= k-len(tmp):
                    backtracking(k, n, i+1)
                tmp.pop()
        backtracking(k, n, 1)
        return ans
```
---
### 简单写一个从1~n所有k位排列的函数(无重复)
```
ans = []
path = []


def permutation(k, n):
    if len(path) == k:
        ans.append(path[:])
        return

    for i in range(1, n+1):
        if not i in path:
            path.append(i)
            permutation(k, n)
            path.pop()
    return ans


assert len(permutation(4, 9)) == len(list(itertools.permutations(range(1, 10), 4)))

最后使用assert 检查是否正确！
```
---
### 17.电话号码的字母组合(Medium)
[17.电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/submissions/)

这题和上面其实是一样的，只是多了一个字典映射！！！到目前为止我认为回溯最重要的就是：
1. 单层逻辑（简单）
2. 剪枝以及终止条件！！！！（重要）

考虑到这两点，上面几题都差不多，所以我直接进行一个代码的贴！
```
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        ans = []
        keyboard_dic = {'2':'abc', '3':'def', '4':'ghi', '5':'jkl', '6':'mno', '7':'pqrs', '8':'tuv', '9':'wxyz'}

        def backtracking(digits, index, path):
            if not digits:
                return 
            if index == len(digits):
                ans.append(path[:])
                return 

            for alpha in keyboard_dic[digits[index]]:
                path += alpha
                backtracking(digits, index+1, path)
                path = path[:-1]

        backtracking(digits, 0, '')
        return ans
```
---
### 39. 组合总和(Medium) & 40. 组合总和 II（Medium）

[39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

基本同上，一个模样写出两个要点即可。
![](https://i.imgur.com/LNV34td.png)


代码：

```
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        candidates.sort()
        ans = []

        def backtracking(candidates, target, path, index):
            if target == 0:
                ans.append(path[:])
                return
            if target < candidates[0]:
                return

            for i in range(index, len(candidates)):
                path.append(candidates[i])
                backtracking(candidates, target-candidates[i], path, i)
                path.pop()

        backtracking(candidates, target, [], 0)
        return ans
```

[40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)

这题和39不一样处在于，不能重复使用数字！也就是说，我们在for循环里的if条件应该不一样！

代码如下：
```
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        candidates.sort()
        ans = []

        def backtracking(candidates, target, path, index):
            if target == 0:
                ans.append(path[:])
                return
            elif index == len(candidates) or target < candidates[index]:
                return

            for i in range(index, len(candidates)):
                if i > index and candidates[i] == candidates[i-1]:
                    continue
                path.append(candidates[i])
                backtracking(candidates, target-candidates[i], path, i+1)
                path.pop()

        backtracking(candidates, target, [], 0)
        return ans
```
---
### 131. 分割回文串(Medium)

[131. 分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)

此题目的思路：

1. 每次先从最开始找到可以作为回文的部分，加入sub；
2. 剩下部分继续找，直至找到全字符串末尾；
3. 用index来标记每次找到的最后位置！

也可以看看代码启示录的思路（其实差不多！）
![](https://i.imgur.com/6Px3qmY.png)

代码如下：

```
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        ans = []

        def backtracking(s, sub, index):
            if index >= len(s):
                ans.append(sub[:])
                return

            for i in range(index, len(s)):
                tmp = s[index:i+1]
                if tmp == tmp[::-1]:
                    sub.append(tmp)
                    backtracking(s, sub, i+1)
                    sub.pop()
            
        backtracking(s, [], 0)
        return ans

```
---
### 93. 复原 IP 地址(Medium) --- 代了判断分支的回溯！可以看看！

这题其实不难，主要的问题就是要注意‘000’之类的连续的0的处理问题！所以，我们需要条件分支：
- 若首位是0，则提取一个‘0.’出来；
- 若首位不是0，则无需另外判断。

单靠解释不清楚，所以我直接把自己的代码放在下面了！


```
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        '''
        为什么要设置n？因为IP长度是4*8，也就是要用n来确定是4段！
        '''
        def backtracking(s, sub, index, n):
            if index == len(s) and n == 0:
                ans.append(sub[:-1])
                return
            elif n == 0 or index == len(s):
                return

            if s[index] == '0':
                sub = sub + '0.'
                backtracking(s, sub, index+1, n-1)
                sub = sub[:-2]
            else:
                for i in range(index, min(index+3, len(s))):
                    tmp = s[index:i+1]
                    length = len(tmp) + 1
                    if int(tmp) <= 255:
                        sub = sub + tmp + '.'
                        backtracking(s, sub, i+1, n-1)
                        sub = sub[:-1*length]

        ans = []
        backtracking(s, '', 0, 4)
        return ans

```
---
### 78. 子集(Medium) & 90. 子集II（Medium） --- 和39 & 40差不多
[78. 子集](https://leetcode-cn.com/problems/subsets/)

![](https://i.imgur.com/4fUDMfJ.png)

同样的架构写代码就是啦！

```
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        ans = [[]]
        nums.sort()

        def backtracking(nums, sub, index):
            if index == len(nums):
                return

            for i in range(index, len(nums)):
                sub.append(nums[i])
                ans.append(sub[:])
                backtracking(nums, sub, i+1)
                sub.pop()

        backtracking(nums, [], 0)
        return ans
```
---

[90. 子集 II](https://leetcode-cn.com/problems/subsets-ii/submissions/)

和78一样，只是为了去重，和上面的**40题**做了一样的处理！！！
```
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        ans = [[]]
        nums.sort()

        def backtracking(nums, sub, index):
            if index == len(nums):
                return

            for i in range(index, len(nums)):
                if i > index and nums[i] == nums[i-1]:
                    continue
                sub.append(nums[i])
                ans.append(sub[:])
                backtracking(nums, sub, i+1)
                sub.pop()

        backtracking(nums, [], 0)
        return ans
```
---
### 491. 递增子序列(Medium) -- 我只做出来了，但是没有完成剪枝（！）

思路就是，当我目标为空或者当前元素 -ge 目标最后一个元素，就把它append（），最后再pop（）。
代码如下：
```
class Solution:
    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        ans = []

        def backtracking(nums, index, sub):
            if len(sub) >= 2 and sub[:] not in ans:
                    ans.append(sub[:])
            if index == len(nums):
                return

            for i in range(index, len(nums)):
                if not sub or nums[i] >= sub[-1]:
                    sub.append(nums[i])
                    backtracking(nums, i+1, sub)
                    sub.pop()
            
        backtracking(nums, 0, [])
        return ans
```
![](https://i.imgur.com/3o8YxaN.png)

> 问题点我认为出在将结果输入ans时的判断太过于冗杂！怎么解决呢？！
> [代码随想录：剪枝](https://programmercarl.com/0491.%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97.html#%E6%80%9D%E8%B7%AF)

思路：每一层回溯中都分别设置一个可查的表用来查看在当前层这个数字是否已经用过！
代码如下：
```
class Solution:
    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        ans = []

        def backtracking(nums, index, sub):
            used_ones = []
            if len(sub) >= 2:
                    ans.append(sub[:])
            if index == len(nums):
                return

            for i in range(index, len(nums)):
                if (sub and nums[i] < sub[-1]) or nums[i] in used_ones:
                    continue
                sub.append(nums[i])
                used_ones.append(nums[i])
                backtracking(nums, i+1, sub)
                sub.pop()
            
        backtracking(nums, 0, [])
        return ans
```
![](https://i.imgur.com/gws8HHi.png)
> 快了一百倍！

---
### 46. 全排列 & 47. 全排列II (Medium)

同上面的39 & 40的组合，以及 78 & 90的子集看起来很像！但是我发现不能用index标记法!因为排列是可以随意顺序的阿！

[46. 全排列](https://leetcode-cn.com/problems/permutations/)

代码：
```
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        ans = []

        def backtracking(nums, path):
            if len(path) == len(nums):
                ans.append(path[:])
                return

            for i in range(len(nums)):
                if nums[i] in path:
                    continue
                path.append(nums[i])
                backtracking(nums, path)
                path.pop()

        backtracking(nums, [])
        return ans
```

[47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)

**代码(我这个时候还没有剪枝所以效果很差的）**：
```
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        ans = []

        def backtracking(nums, path, used_index):
            if len(path) == len(nums) and path[:] not in ans:
                ans.append(path[:])
                return
            
            for i in range(len(nums)):
                if i in used_index:
                    continue
                path.append(nums[i])
                backtracking(nums, path, used_index+[i])
                path.pop()

        backtracking(nums, [], [])
        return ans

```
![](https://i.imgur.com/WA0mBnY.png)

**剪枝后的代码（自己剪枝！）：**
注意：
- 排列需要注意的剪枝：在下一轮的时候，不把已经用过的东西放入 -- 使用全局的used_index记录；
- 考虑到重复需要注意的剪枝：在当前轮，已经用过的数字不再复用 -- 使用当前for循环的used_ones记录！（**这里可以看上面的 491.递增子序列**）
```
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        ans = []

        def backtracking(nums, path, used_index):
            if len(path) == len(nums):
                ans.append(path[:])
                return
            
            used_ones = []
            for i in range(len(nums)):
                if nums[i] in used_ones or i in used_index:
                    continue
                path.append(nums[i])
                used_ones.append(nums[i])
                backtracking(nums, path, used_index+[i])
                path.pop()

        backtracking(nums, [], [])
        return ans

```
![](https://i.imgur.com/w4RDj1f.png)
> 牛蛙！

---
### 332. 重新安排行程(Hard) -- 说实话我是看的思路做的，思路明白但是自己搞还是...

先贴上思路链接：[代码随想录](https://programmercarl.com/0332.%E9%87%8D%E6%96%B0%E5%AE%89%E6%8E%92%E8%A1%8C%E7%A8%8B.html#%E6%80%9D%E8%B7%AF)

然后丢代码（自己大概能写吧）：
```
class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        path = ['JFK']
        tickets_dict = defaultdict(list)
        for each in tickets:
            tickets_dict[each[0]].append(each[1])

        def backtracking(start_point):
            if len(path) == len(tickets) + 1:
                return True
            
            tickets_dict[start_point].sort()
            for _ in tickets_dict[start_point]:
                end_point = tickets_dict[start_point].pop(0)
                path.append(end_point)
                if backtracking(end_point):
                    return True
                # 回退    
                path.pop()
                tickets_dict[start_point].append(end_point)

        backtracking('JFK')
        return path
```
---
## 贪心算法

### 1.本质

贪心的本质是选择每一阶段的局部最优，从而达到全局最优。
（当然，有的问题是没法通过局部最优达到全局最优的！)

贪心算法一般分为如下四步：

1. 将问题分解为若干个子问题
1. 找出适合的贪心策略
1. 求解每一个子问题的最优解
1. 将局部最优解堆叠成全局最优解

---

### 455.分发饼干(Easy)

[455.分发饼干](https://leetcode-cn.com/problems/assign-cookies/)

> 思路简单，先排序然后用小的饼干先喂食量小的孩子，用res记录。
> python和C++代码都在下面：

```
python：

class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        result = 0
        g.sort()
        s.sort()
        if not s:
            return result
        for i in range(len(s)):
            if result < len(g) and s[i] >= g[result]:
                result += 1
        return result
        
"""
"""
C++:

class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int res = 0;
        if (s.empty()) {
            return res;
        }
        for (size_t i = 0; i != s.size(); ++i) {
            if (res != g.size() && s[i] >= g[res]) {
                res += 1;
            }
        }
        return res;
    }
};
    
```

---
### 376. 摆动序列(Medium)

[376. 摆动序列](https://leetcode-cn.com/problems/wiggle-subsequence/)

> 说实话我甚至觉得这题有点懵！但是反正意思就是找峰值！有趣的是这题不需要讨论最后一个元素和第一个因为它们默认是峰值。

```
python3：

class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        preC, curC, res = 0,0,1
        for i in range(len(nums) - 1):
            curC = nums[i+1] - nums[i]
            if curC*preC <= 0 and curC != 0:
                res += 1
                preC = curC
        return res
        
"""
"""
C++：

class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int preC(0), curC(0), res(1);
        for (size_t i = 0; i != nums.size()-1; ++i) {
            curC = nums[i+1] - nums[i];
            if (curC*preC <= 0 && curC != 0) {
                ++res;
                preC = curC;
            }
        }
        return res;
    }
};
```

---
### 53. 最大子数组和(Easy)

[53. 最大子数组和](https://leetcode-cn.com/problems/maximum-subarray/)

> 很简单，设置当前和以及最大和两个量，每一轮给当前量加入值之后和最大比，取最大值；若当前值小于0则重设成0即可。

```
py3：

class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        res = -float('inf')
        cur = 0
        for i in range(len(nums)):
            cur += nums[i]
            res = max(cur, res)
            if cur < 0:
                cur = 0
        return res
        
"""
"""

C++

#include <limits.h>
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxsum(INT_MIN), cur(0);

        for (size_t i = 0; i != nums.size(); ++i) {
            cur += nums[i];
            maxsum = max(cur, maxsum);
            if (cur < 0) {
                cur = 0;
            }
        }
        return maxsum;
    }
};
```

---

### 122. 买卖股票的最佳时机 II(Medium)

[122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

- 这其实就是摆动序列..找出来低峰高峰然后相邻低峰高峰购入卖出就好。
- 当然我们有更简单的方法，那就是只要赚钱就立马卖掉！这样的方法简单易懂且好写。

```
py3:

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        profit = 0
        for i in range(1, len(prices)):
            profit += max(0, prices[i] - prices[i-1])
        return profit
        
"""
"""

C++:

class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int profit(0);
        for (int i = 1; i != prices.size(); ++i) {
            profit += max(prices[i] - prices[i-1], 0);
        }
        return profit;
    }
};
```

---
### 55. 跳跃游戏(Medium)

[55. 跳跃游戏](https://leetcode.cn/problems/jump-game/)

- 注意这个题只是问你能不能到达终点！那么我从终点往回跳看覆盖区能不能到达0就可以；同理，我从0开始往终点跳看能不能到最后一样的！代码都丢在下面：

```
Py3：

class Solution:
    def canJump(self, nums: List[int]) -> bool:
        '''
        1. 从终点往回走
        '''
        last_position = len(nums) - 1
        for i in range(len(nums) - 2, -1, -1):
            if nums[i] >= last_position - i:
                last_position = i
        return last_position == 0

        '''
        2. 从起点开始走
        '''
        beg = 0
        for i in range(len(nums)-1):
            if beg < i:
                return False
            else:
                beg = max(beg, i+nums[i])
        return beg >= len(nums) - 1

'''

C++

class Solution {
public:
    bool canJump(vector<int>& nums) {
        '''
        1. 正向走
        '''
        int beg = 0;
        for (int i = 0;i != nums.size(); ++i) {
            if (beg < i) {
                return false;
            }
            beg = max(beg, i + nums[i]);
        }
        return (beg >= nums.size() - 1?true:false);

        '''
        2. 反向走
        '''
        auto cur_pos = nums.size() - 1;
        for (size_t i = nums.size() - 2; i != -1; --i) {
            if (nums[i] + i >= cur_pos) {
                cur_pos = i;
            }
        }
        return (cur_pos == 0?true:false);
    }
};
```

> 本题总的来说较为简单，代码也好写。

---
### 45. 跳跃游戏 II(Medium)

[45. 跳跃游戏 II](https://leetcode.cn/problems/jump-game-ii/)

- 此题和上题些许类似，但这次要求的是最小步数。所以： 个人的思路放在下面。


> 1. 一个变量 step 以记录最小步数；
> 2. 从最开始的点起步，每一次寻找最大覆盖域；
> 3. 找到最大覆盖域后，遍历覆盖域中所有原素，找到下一个最大覆盖域；
> 4. 以此类推直到最大覆盖域超过终点为止。

- 结果证明，这个思路是没有问题的，只是他这个写法我一下还没意识过来。

```
Py3：

class Solution:
    def jump(self, nums: List[int]) -> int:
        step = 0
        cur_pos = 0
        cur_end = 0

        for i in range(len(nums) - 1):
            cur_pos = max(cur_pos, i + nums[i])
            if cur_end == i:
                step += 1
                cur_end = cur_pos
        return step

'''

C++：

class Solution {
public:
    int jump(vector<int>& nums) {
        int cur_pos(0), cur_end(0), step(0);

        for (int i = 0; i != nums.size() - 1; ++i) {
            cur_pos = max(cur_pos, i + nums[i]);
            if (i == cur_end) {
                ++step;
                cur_end = cur_pos;
            }
        }
        return step;
    }
};
```
---

### 1005. K 次取反后最大化的数组和(Easy)

[1005. K 次取反后最大化的数组和](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/)

- 这题比较简单，我们将数组以绝对值为序从大到小排列后，顺序遍历一个个用掉负号就好了！

```
Py3：

class Solution:
    def largestSumAfterKNegations(self, nums: List[int], k: int) -> int:
        newnums = sorted(nums, key=abs, reverse=True)

        for i in range(len(newnums)):
            if k and newnums[i] < 0:
                k -= 1
                newnums[i] *= -1

        if k and k%2:
            newnums[-1]  *= -1
        return sum(newnums)

'''

C++：

class Solution {
public:
    static bool cmp(int &a, int &b) { return abs(a) > abs(b); }

    int largestSumAfterKNegations(vector<int>& nums, int k) {
        int sum(0);
        sort(nums.begin(), nums.end(), cmp);
        for (size_t i = 0; i != nums.size(); ++i) {
            if (k > 0 && nums[i] < 0) {
                --k;
                nums[i] *= -1;
            }
        }
        if (k > 0 and k % 2 == 1) {
            nums[nums.size()-1] *= -1;
        }
        for (int num : nums) {
            sum += num;
        }
        return sum;
    }
};
```

---
### 134. 加油站(Medium)

[134. 加油站](https://leetcode.cn/problems/gas-station/)

- 首先，作为基础我们需要知道如果 `sum(gas) < sum(cost)` 那么是肯定没法周游的。反映在py里就是sum判断；在C++里没有sum数组的函数，所以我们需要实时计算sum并在最后判断其与0的大小比较。
- 其次，以贪心思想出发，在已知`sum(gas) >= sum(cost)`  也就是一定能周游的情况下，我们的问题就变成了求出发点。这样就简单了。
- 求出发点：我们从0开始遍历，将当前油量剩余保存于cur_rest变量；当cur_rest<0时，说明从前面这几个点出发不行！就将起点start设置为当前index+1.这样遍历完一圈后，就可以通过sum和start来判断能否走完且能走完的话要从哪里出发。

```
Py3：
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        if sum(gas) < sum(cost): return -1 # 这样说明一定跑不完；反之，不这样就跑的完

        start = 0
        total_rest = 0
        for i in range(len(gas)):
            temp_rest = gas[i] - cost[i]
            total_rest += temp_rest
            if total_rest < 0:
                start = (i+1)%len(gas)
                total_rest = 0

        return start
        
'''
C++：

class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int total_rest(0), cur_rest(0), start(0);
        
        for (size_t i = 0; i != gas.size(); ++i) {
            cur_rest += gas[i] - cost[i];
            total_rest += gas[i] - cost[i];
            if (cur_rest < 0) {
                start = i + 1;
                cur_rest = 0;
            }
        }
        if (total_rest < 0) {
            return -1;
        }
        return start;
    }
};
```

---

### 135. 分发糖果(Hard) - 再看看

[135. 分发糖果](https://leetcode.cn/problems/candy/)

> 说实话我自己看是没有看明白的...但是，看了解说觉得怎么这么简单！！！...
> 思路：
> - 从左往右遍历，每个人与自己左边的人比，比左边的人大则相对于他+1；
> - 然后右向左遍历，每个人与自己右边的人比，比右边大则取现有值与（右值+1）的最大值。
> - 然后sum数组即可。

```
Py3：

class Solution:
    def candy(self, ratings: List[int]) -> int:
        length = len(ratings)
        candies = [1 for _ in range(length)]

        for i in range(1, length):
            if ratings[i] > ratings[i-1]:
                candies[i] = candies[i-1] + 1
        
        for i in range(length-2, -1, -1):
            if ratings[i] > ratings[i+1]:
                candies[i] = max(candies[i], candies[i+1] +1)

        return sum(candies)

'''

C++：

class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int> candyVec(ratings.size(), 1);

        for (int i = 1; i != ratings.size(); ++i) {
            if (ratings[i] > ratings[i-1]) { candyVec[i] = candyVec[i-1] + 1; }
        }

        for (int i = ratings.size() - 2; i >= 0; --i) {
            if (ratings[i] > ratings[i+1]) { candyVec[i] = max(candyVec[i+1]+1, candyVec[i]); }
        }

        int result = 0;
        for (int i = 0; i != ratings.size(); ++i) {
            result += candyVec[i];
        }
        return result;
    }
};
```

---

### 860. 柠檬水找零(Easy)

[860. 柠檬水找零](https://leetcode.cn/problems/lemonade-change/)

- 太简单，拿两个数记录多余的5和10即可！唯独需要注意20找零有两种方式！（这题贪心在哪里？贪心在能用10块绝不用多余的5块！）

```
Py3：

class Solution:
    def lemonadeChange(self, bills: List[int]) -> bool:
        extra_5, extra_10 = 0, 0

        for i in range(len(bills)):
            if bills[i] == 5:
                extra_5 += 1
            elif bills[i] == 10:
                if not extra_5:
                    return False
                else:
                    extra_5 -= 1
                    extra_10 += 1
            elif bills[i] == 20:
                if extra_5 and extra_10:
                    extra_10 -= 1
                    extra_5 -= 1
                elif extra_5 >= 3:
                    extra_5 -= 3
                else:
                    return False
        
        return True

'''
C++：

class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int five(0), ten(0);

        for (auto bill:bills) {
            if (bill == 5) {
                ++five;
            } else if (bill == 10) {
                if (!five) {
                    return false;
                }
                --five;
                ++ten;
            } else if (bill == 20) {
                if (five>0 && ten>0) {
                    --five;
                    --ten;
                } else if (five >= 3) {
                    five -= 3;
                } else {
                    return false;
                }
            }
        }
        return true;
    }
};
```

---

### 406. 根据身高重建队列(Medium)

[406. 根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/)

- 一看就知道是要两种方式进行优先度排序：
- 1. 身高按高到矮排序；
- 2. 同等身高按前面站的人数的数量排序；
- 解决！（为什么，因为per[1]一定是他前面站的比他高的人的数量，而我们第一步就按身高排序了，所以按这个排序就可以把他定位！）

> 注意点：这里我们会用到很多的插入操作！说到插入在C++我们肯定就会优先用list也就是链表！那么链表的一些写法还是要会哈！

```
Py3：

class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        queue = []
        newpeople = sorted(people, key=lambda x: (-x[0], x[1]))

        for peo in newpeople:
            queue.insert(peo[1], peo) # 插入位置，插入值
        return queue
        
        
'''

C++：

class Solution {
public:
    static bool cmp (vector<int> &a, vector<int> &b) {
        if (a[0] == b[0]) {
            return a[1] < b[1];
        }
        return a[0] > b[0];
    }
    // vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
    //     vector<vector<int>> queue;
    //     sort(people.begin(), people.end(), cmp);
    //     for (auto peo : people) {
    //         queue.insert(queue.begin() + peo[1], peo);
    //     }
    //     return queue;
    // }
    // 能够发现，用vector进行插入操作很花时间！！！！那么我们用list来试试。
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        list<vector<int>> queue;
        sort(people.begin(), people.end(), cmp);
        
        for (auto peo : people) {
            list<vector<int>>::iterator iter = queue.begin();
            int position = peo[1];
            while (position) {
                ++iter;
                --position;
            }
            queue.insert(iter, peo);
        }
        return vector<vector<int>>(queue.begin(), queue.end());
    }
};
```

---

### 452. 用最少数量的箭引爆气球(Medium)

[452. 用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)

> 这道题我的思路就是：
> 1. 先按气球右边顺序排序（即，终点）；
> 2. 让第一支箭瞄准第一颗气球的最右边（方便打到右边的气球！）；
> 3. 从第二颗气球开始遍历，每次观察当前气球起点与上一次的瞄准点大小：
    > - 若终点>瞄准点大小，则需要加一支箭，且将新的箭的瞄准点对好当前气球终点；
    > - 若终点<瞄准点大小，则需要调整当前箭的瞄准点直至当前气球终点（才能打中两个气球）；
> 4. 进行循环直至遍历所有气球。

:::warning
代码贴下面：

```
Py3：

class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        arrange = sorted(points, key=lambda x:(x[1]))
        
        array = 1
        pos = arrange[0][1]

        for i in range(1, len(arrange)):
            if arrange[i][0] > pos:
                array += 1
                pos = arrange[i][1]
            else:
                pos = min(pos, arrange[i][1])
        
        return array
```
![](https://i.imgur.com/ncnzF2D.png)

```
C++：

class Solution {
public:
    static bool cmp(vector<int> &a, vector<int> &b) {
        return a[1] < b[1];
    }

    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(), points.end(), cmp);
        int arr(1), pos(points[0][1]);

        for (size_t i = 1; i != points.size(); ++i) {
            if (pos < points[i][0]) {
                ++arr;
                pos = points[i][1];
            } else {
                pos = min(pos, points[i][1]);
            }
        }
        return arr;
    }
};
```
![](https://i.imgur.com/wFf1vO6.png)

> 为啥我发现，C++解决很多问题都比python耗时间和内存？？？
:::

---

### 435. 无重叠区间(Medium) -- 典！

[435. 无重叠区间](https://leetcode.cn/problems/non-overlapping-intervals/)

> 一开始我的想法：
> 将数组以x[0], x[1]的顺序排列，然后取第一个的右边（结束）记录为init_end, 之后不断向右遍历，检查起点和当前的init_end大小：若起点大于init_end，则计数+1，否则init_end=当前的终点。
> 
:::danger
但是这个想法是错误的！因为，如果有一个占区间很长的搅屎棍，那你岂不是会为了保住它而损失很多个小区间吗！
:::

所以，正确的做法应该是：

1. 按x[1]遍历排序，且记最小的x[1]为init_end;使用counter记录重合非区间数量（初始为1！）；
2. 从第二个开始一直遍历到末尾，期间：
    - 若当前的begin(x[0]) 大于init_end说明非重合！非重合计数count+1且将init_end记录为当前的x[1]；
    - 若小于init_end重合，不用记录；
3. 返回值： intervals.size() - count

```
Py3：

class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        counter = 1
        interval_new = sorted(intervals, key=lambda x: x[1])
        init_end = interval_new[0][1]

        for i in range(1, len(interval_new)):
            if interval_new[i][0] >= init_end:
                counter += 1
                init_end = interval_new[i][1]

        return len(interval_new) - counter

''''''

C++：

class Solution {
public:
    static bool cmp(vector<int> &a, vector<int> &b) {
        return a[1] < b[1];
    }

    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), cmp);
        int counter(1);
        int temp_end = intervals[0][1];

        for (size_t i = 1; i != intervals.size(); ++i) {
            if (intervals[i][0] >= temp_end) {
                ++counter;
                temp_end = intervals[i][1]; 
            }
        }
        return intervals.size() - counter;
    }
};

```

==说实话为啥C++效率在这道题比python差这么多？？？==

---

### 763. 划分字母区间(Medium)

[763. 划分字母区间](https://leetcode.cn/problems/partition-labels/)

==不会！这我每加一个字母都得检查完一遍字符串？？？==

解法：参照代码随想录

1. 利用ord()记录每个字母在字符串出现的最后下标并且保存于hash_map数组；
2. 从字符串最左边开始循环，用left, right记录我们需要保存的长度：
    - right = max(right, hash_map[ord(s[i]) - ord('a')])；
    - 当right == i说明全部的都成立了！这时候就可以将其保存为一段字符串，记录长度为: right-left+1, 且left=right+1；
3. 循环到结束就OK。

```
Py3：

class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        res = []
        left, right = 0, 0
        hash_map = [0] * 26
        
        for i in range(len(s)):
            hash_map[ord(s[i]) - ord('a')] = i # 给各个字母最后出现位置排序


        for i in range(len(s)):
            right = max(right, hash_map[ord(s[i])-ord('a')])
            if right == i:
                res.append(right-left+1)
                left = right + 1

        return res
        
''''''

C++：

class Solution {
public:
    vector<int> partitionLabels(string s) {
        vector<int> hash(26, 0);
        for (size_t i = 0; i != s.size(); ++i) {
            hash[s[i] - 'a'] = i;
        }

        vector<int> result;
        int left(0), right(0);

        for (size_t i = 0; i != s.size(); ++i) {
            right = max(right, hash[s[i] - 'a']);
            if (right == i) {
                result.push_back(right-left+1);
                left = right + 1;
            }
        }
        return result;
    }
};
```
![](https://i.imgur.com/7YmH2GA.png)
> 这里C++就快很多了；并且，C++可以直接将字母和编码位进行转换，比起python3的ord()还是方便很多！！！
---
### 56. 合并区间(Medium) - 典中典

[56. 合并区间](https://leetcode.cn/problems/merge-intervals/)

> 这个题太典了...我先试试！
> 结论：比较好想，我先按左排，然后记录第一个的start，end。然后遍历，如果区间连续就把end = max(end, interval[i][1])；反之则表明这个连续区间结束，进入下一个区间，更新start, end。

```
Py3：

class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals_new = sorted(intervals, key = lambda x:x[0])
        result = []

        start = intervals_new[0][0]
        end = intervals_new[0][1]

        for i in range(1, len(intervals_new)):
            if intervals_new[i][0] <= end:
                end = max(end, intervals_new[i][1])
            else:
                result.append([start, end])
                start = intervals_new[i][0]
                end = intervals_new[i][1]
        
        if not result or end != result[-1][1]:
            result.append([start, end])

        return result

''''''

C++：

class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> result;
        sort(intervals.begin(), intervals.end(), [](vector<int> &a, vector<int> &b){return a[0] < b[0];});

        int start = intervals[0][0];
        int end = intervals[0][1];

        for (size_t i=1; i!=intervals.size(); ++i) {
            if (intervals[i][0] <= end) {
                end = max(end, intervals[i][1]);
            } else {
                result.push_back({start, end});
                start = intervals[i][0];
                end = intervals[i][1];
            }
        }

        if (result.empty() || result.back()[1] != end) {
            result.push_back({start, end});
        }
        return result;
    }
}
```

---

### 738. 单调递增的数字(Medium)

[738. 单调递增的数字](https://leetcode.cn/problems/monotone-increasing-digits/)

思路：从后往前遍历。如果全程没有lst[i-1] > lst[i]则说明这个数字本身就是单调递增，不需要变化；如果不满足，就说明这个数字从这一位开始不满足了，那么我们把前一位减1（这样就比原来小了！）然后后面的位全部变成99就可以保证其最大！！！

```
Py3：

class Solution:
    def monotoneIncreasingDigits(self, n: int) -> int:
        lst = list(str(n))

        for i in range(len(lst)-1, 0, -1):
            if int(lst[i-1]) > int(lst[i]):
                lst[i-1] = str(int(lst[i-1])-1)
                lst[i:] = '9' * (len(lst)-i)

        return int(''.join(lst))
        
''''''

C++：

class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        string string_num = to_string(n);
        int flag = string_num.size(); // 标记从哪一位开始后面都变成9！

        for (auto i = string_num.size()-1; i != 0; --i) {
            if (string_num[i-1] > string_num[i]) {
                // --string_num[i-1];
                flag = i;
                --string_num[i-1];
            }
        }

        for (; flag != string_num.size(); ++flag) {
            string_num[flag] = '9';
        }

        return stoi(string_num);
    }
};
```

---

### 714. 买卖股票的最佳时机含手续费(Medium)

[714. 买卖股票的最佳时机含手续费](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

> 和前面不同，你不能够一有钱赚就卖了！因为要交手续费，我们应该在尽量少卖的基础上获利。

> 我的构思：波底买入，波峰卖出，先找波峰再判断买卖！(太难实现了！要考虑很多东西)

==看看别人的！很简单但是需要理解：==[代码随想录：做法](https://programmercarl.com/0714.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BA%E5%90%AB%E6%89%8B%E7%BB%AD%E8%B4%B9.html#%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95)

个人理解这个做法：我们每次有获利就计算当前的利润！但是这样的话我们会卖的次数多所以下一次我们赚的钱就要减少fee这个大小！

```
Py3：

class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        profit = 0
        minprice = prices[0]

        for i in range(1, len(prices)):
            if prices[i] < minprice:
                minprice = prices[i]
            elif prices[i] >= minprice and prices[i] <= minprice + fee:
                continue
            elif prices[i] > minprice + fee:
                profit += (prices[i] - minprice - fee)
                minprice = prices[i] - fee
            
        return profit

''''''

C++：

class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int profit(0), minprice=prices[0];

        for (size_t i = 0; i != prices.size(); ++i) {
            if (prices[i] < minprice) { minprice = prices[i];}
            else if (prices[i] > minprice && prices[i] <= minprice + fee) {continue;}
            else if (prices[i] > minprice + fee) {
                profit += (prices[i] - minprice - fee);
                minprice = prices[i] - fee;
            }
        }
        return profit;
    }
};
```

---
