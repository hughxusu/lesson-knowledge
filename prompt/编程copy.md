# 使用GPT模型辅助编程

> [!warning]
>
> Gpt 模型每次生成的内容有所差异，下面的案例仅用来参考。

## 使用Chat GPT辅助编程

使用GPT模型学习第三方代码库

### 使用Python操作Excel

1. 定义角色

```markdown
你是一名程序员，熟悉python语言和数据库，可以熟练使用python相关库操作excel和word文件。
```

2. 询问所需的第三方库

```markdown
在python中可以用于读写excel的第三方库有哪？
列出上述库的优缺点，使用大纲形式展示。
我只需要操作xlsx库，偶尔兼容xls文件，你推荐我使用哪个库？
```

3. 第三方库的安装

```markdown
介绍一下openpyxl的安装和基本使用，按如下步骤列出：
1. 库的安装
2. 导入库
3. 基本调用
给出openpyxl官方文档链接
```

4. 了解Excel的文件结构

```markdown
解释一下excel中的sheet是什么，请举例说明。
一个excel文件中可以有多少个工作表？
```

5. Excel中sheet的操作

```markdown
1. 如何使用openpyxl读取任意一sheet文件？
2. openpyxl读取sheet有哪些接口函数，使用列明形式说明？
3. openpyxl中如何读取sheet的个数？
```

6. Excel中行列的读取

```markdown
1. 下面的问题都基于openpyxl库
2. 如何读取行列的数量？
3. 如何读取一整行的数据？
4. 可以使用索引来读取某一行的数据吗？如果可以如何操作？
5. 如何将一行数据转换成元组或数组形式？
6. 有没有其他接口函数直接转换行为元组？
7. 说明一下".iter_rows"和".rows"的区别，使用表格展示。
8. 如何读取一整列的数据？
9. 如何一次性读取所有的列数据？
```

7. 单元格操作

```markdown
1. 遍历表格中的单元格？
2. 上述方法中遍历的到的cell数据是什么样的数据结构，都有哪些属性和方法，用大纲形式打印出来。
3. 给出Cell对象的文档链接。
```

8. 表格写入

```markdown
1. 如何将二维数组中的数据写入表格?
2. 如何合并多个单元格?
3. 如何修改单元格中数据的样式？
4. 上面的Font是什么样的数据结构，都有哪些属性和方法，用大纲形式打印出来。
5. 如何给表格设置统一的颜色和字体？
6. 如何让单元格的文本数据居中？
```

### 使用Python操作Word

1. 定义角色（同上面的 excel）
2. 询问所需的第三方库

```markdown
在python中可以用于读写word的第三方库有哪？
列出上述库的优缺点，使用大纲形式展?
我主要是批量读写'.docx'文件，你推荐我使用哪个库？
```

3. 第三方库的安装

```markdown
介绍一下python-docx的安装和基本使用，按如下步骤列出：
1. 库的安装
2. 导入库
3. 基本调用
给出python-docx官方文档链接
下面的问题都基于python-docx库
```

4. 写入文档

```markdown
1. 如何插入文档标题？
2. `add_heading`方法都有哪些参数？作用是什么？用大纲的形式列出
3. 如何插入段落文字？
4. `add_paragraph`方法都有哪些参数？作用是什么？用大纲的形式列出
```

## 使用AI编程插件辅助编程

### 辅助编程工具

1. [CodeGeeX](https://codegeex.cn/downloadGuide) 免费
2. [Codeium](https://codeium.com/) 免费
3. [Copilot](https://github.com/features/copilot) 收费 可以申请学生计划
4. [tabnine](https://www.tabnine.com/) 收费
5. [cursor](https://cursor.sh/) 收费

### 学习算法

1. 算法的课程和信息学竞赛。
2. 面试和笔试中的算法。（展示合理的思考路径和解决问题的能力，注重基知识）

> [!warning]
>
> 面试算法不等于研究《[算法导论](https://book.douban.com/subject/20432061/)》
>
> 面试是一个综合评价过程，不只是算法的笔试和面试
>
> * 研究生：导师的项目
> * 本科生：毕业设计和实践经理
>
> 面试中不要忽视**最笨的算法**

找工作常用的在线判题系统（online judge）

1. 企业级在线判题系统

   * [leetcode](https://leetcode.cn/)（推荐）

   * [hackerrank](https://www.hackerrank.com/)


2. 信息学竞赛在线判题系统
   * [codeforces](https://codeforces.com/)
   * [topcoder](https://www.topcoder.com/)
   * [codechef](https://www.codechef.com/)

### 算法的复杂度

#### 时间复杂度

算法的时间复杂度 $O$ 表示什么？

$n$ 表示数据规模，$O(f(n))$ 表示运行算法所需要执行的指令数和 $f(n)$ 成正比。

| 名称                    | 复杂度        | 所需指令数          |
| ----------------------- | ------------- | ------------------- |
| 二分查找法              | $O(\log n)$   | $a \times \log n$   |
| 寻找数组中的最大/最小值 | $O(n)$        | $b \times n$        |
| 归并排序算法            | $O(n \log n)$ | $c \times n \log n$ |
| 选择排序算法            | $O(n^2)$      | $d \times n^2$      |

比例两个算法

1. 算法一：$O(n)$ 所需指令数为  $10000 \times n$
2. 算法二：$O(n^2)$ 所需指令数  $10 \times n^2$

| $n$     | 算法一的指令数 | 算法二的指令数 | 倍数  |
| ------- | -------------- | -------------- | ----- |
| $10$    | $10^5$         | $10^3$         | 100   |
| $100$   | $10^6$         | $10^5$         | 10    |
| $1000$  | $10^7$         | $10^7$         | 1     |
| $10000$ | $10^8$         | $10^9$         | 0.1   |
| $10^5$  | $10^9$         | $10^{11}$      | 0.01  |
| $10^6$  | $10^{10}$      | $10^{13}$      | 0.001 |

> [!warning]
>
> 当数据规模 $n$ 达到一定临界点，时间复杂度低的算法一定会比时间复杂度高的算法快，而且数据规模 $n$ 越大效果越明显。
>
> 算法的目标就是处理大量数据。

<img src="https://pic3.zhimg.com/80/v2-e879535dab892ff8458d85bb5ff3648a_1440w.webp" style="zoom:67%;" />

#### 理解数据规模的概念

```python
import time

def calculate_sum(n):
    total_sum = 0
    for i in range(1, n+1):
        total_sum += i
    return total_sum

for i in range(1, 10):
    N = 10 ** i
    start_time = time.time()  # 记录开始时间
    total_sum = calculate_sum(N)
    end_time = time.time()  # 记录结束时间
    execution_time = end_time - start_time  # 计算循环执行时间

    print(f"Sum from 1 to 10^{i}, Execution time: {execution_time} seconds")
```

> [!warning]
>
> 如果想在1秒内解决问题
>
> 算法复杂度为 $O(n^2)$ python 处理数据大约在$10^3$ 左右（根据个人计算机性能）

#### 空间复杂度

多开辟一个辅助数组，空间复杂度为 $O(n)$

多开辟一个辅助的二维数组，空间复杂度为 $O(n^2)$

多开辟常数空间，复杂度为 $O(1)$

> [!warning]
>
> 递归调用是有空间代价的

#### 使用ChatGPT学习

1. 定义角色

```markdown
你是一名程序员，熟悉各种算法，如：排序、二叉树搜索、动态规划等。
```

2. 相关提问

````markdown
跟我解释一下算法的时间复杂度"O"怎么理解，举例说明。  
算法一时间复杂度为O(n)和算法二时间复杂度为O(n^2)，算法一有10000条指令，算法二有10条指令，比较在不同n规模量算法的下的差异，n从10到10^6，使用表格展示。
上述表格用指数形式重新绘制。
绘制时间复杂度为O(1)、O(log n)、O(n)、O(n^2)的曲线示意图。
帮我找一张比较算法时间复杂度的示意图，给出链接。
使用python写一个程序从10到10^9，循环求和并统计每个循环的运行时间

说明一下下列算法的复杂度
```
def calculate_sum(n):
    total_sum = 0
    for i in range(1, n+1):
        total_sum += i
    return total_sum
```

解释一下算法的空间复杂度
````

### 算法问题从何处着手

1. 理解算法的规则。
2. 明确算法中所使用变量的定义。
3. 注意边界值。

二分查找法：

1. 在已排序数组中查找特定元素。
2. 通过反复将搜索区间划分为两半，并确定目标值可能在哪一半中，从而将搜索范围缩小一半。
3. 这个过程不断重复，直到找到目标值或确定目标值不在数组中。

<img src="https://s11.ax1x.com/2024/02/21/pFtj1fO.jpg" style="zoom: 25%;" />

> [!note]
>
> * 二分查找法师1964年提出的。
> * 第一个没有bug的二分查找法在1962年才出现。

```python
def binary_search(arr, target):
    l = 0
    r = len(arr) - 1  # 在 [left ... right] 的范围里寻找 target
    while l <= r:  # 当 l == r 时，区间 [l...r] 依然是有效的
        mid = (l + r) // 2
        if arr[mid] == target:
            return mid
        elif target > arr[mid]:
            l = mid + 1  # target在 [mid+1 ... r] 中
        else:  # target < arr[mid]
            r = mid - 1  # target在 [l ... mid-1] 中
    return -1
  
if __name__ == '__main__':
    arr = [1, 2, 3, 4, 5]
    target = 2
    result = binary_search(arr, target)
    print(result)
```

> [!warning]
>
> 循环不变量（Loop Invariant）是在程序循环中为真的性质或条件。它是一个逻辑表达式，它在每次迭代循环时保持不变。

修改值的定义

```python
def binary_search(arr, target):
    l = 0
    r = len(arr)  # 在 [left ... right) 的范围里寻找 target
    while l < r:  # 当 l == r 时，区间 [l...r) 是无效的 [7, 7)
        mid = (l + r) // 2   # l + (r - l) // 2 防止溢出 
        if arr[mid] == target:
            return mid
        elif target > arr[mid]:
            l = mid + 1  # target在 [mid+1 ... r) 中
        else:  # target < arr[mid]
            r = mid  # target在 [l ... mid) 中
    return -1
```

> [!warning]
>
> 算法测试中注重小数据量的调试。

### 算法面试题

LeetCode 283，27

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

```markdown
输入: nums = [0, 1, 0, 3, 12]
输出: [1, 3, 12, 0, 0]
```

1. 基本思路

```python
class Solution(object):
    def moveZeroes(self, nums):
        nonZero = []
        for num in nums:
            if num:
                nonZero.append(num)

        result = [0 for _ in nums]
        for index, num in enumerate(nonZero):
            result[index] = num

        for index, num in enumerate(nums):
            nums[index] = result[index]

if __name__ == '__main__':
    nums = [0, 1, 0, 3, 12]
    Solution().moveZeroes(nums)
    print(nums)
```

算法的时间复杂度和空间复杂度都是 $O(n)$

2. 原地移动操作

```python
class Solution(object):
    def moveZeroes(self, nums):
        k = 0  # nums 中, [0...k)的元素均为非0元素
        for num in nums:
            if num:
                nums[k] = num
                k += 1

        for i in range(k, len(nums)):
            nums[i] = 0
```

3. 使用数据交换操作

```python
class Solution(object):
    def moveZeroes(self, nums):
        k = 0  # nums 中, [0...k)的元素均为非0元素
        for index, num in enumerate(nums):
            if num:
                nums[k], nums[index] = nums[index], nums[k]
                k += 1
```

4. 避免全部为 0 进行交换

```python
class Solution(object):
    def moveZeroes(self, nums):
        k = 0  # nums 中, [0...k)的元素均为非0元素
        for index, num in enumerate(nums):
            if num:
                if index != k:
                    nums[k], nums[index] = nums[index], nums[k]
                    k += 1
                else:
                    k += 1
```

> [!note]
>
> 使用 Copilot 注释可以直接生成代码。

### 软件测试

软件测试金字塔模型

<img src="https://www.lambdatest.com/resources/images/learning-hub/end-to-end-testing-pyramid.webp" />

#### Python单元测试框架

Unitest是Python自带的单元测试工具。

```python
import unittest

class MyTestCase(unittest.TestCase):
    @classmethod
    def setUpClass(cls):   # 所有test运行前运行一次
        print("setUpClass")

    @classmethod
    def tearDownClass(cls):  # 所有test运行后运行一次
        print("tearDownClass")

    def setUp(self):  # 每个test运行前运行一次
        print("setUp")

    def tearDown(self):  # 每个test运行后运行一次
        print("tearDown")

    def test_01(self):  # 测试用例
        print('test_01')

    def test_02(self):
        print('test_01')


if __name__ == '__main__':
    unittest.main()
```

Unitest中的断言

断言是用于判断程序是否为预期的结果

| 方法                                                         | 检查对象           |
| :----------------------------------------------------------- | :----------------- |
| [`assertEqual(a, b)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertEqual) | `a == b`           |
| [`assertNotEqual(a, b)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertNotEqual) | `a != b`           |
| [`assertTrue(x)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertTrue) | `bool(x) is True`  |
| [`assertFalse(x)`](https://docs.python.org/zh-cn/3/library/unittest.html#unittest.TestCase.assertFalse) | `bool(x) is False` |

```python
class MyTestCase(unittest.TestCase):
    def test_01(self): 
        a = 10
        b = 10
        self.assertEqual(a, b)
        self.assertNotEquals(a, b)

    def test_02(self):
        c = False
        self.assertFalse(c)
        self.assertTrue(c)
```

使用Copilot生成测试用例

```python
class Solution(object):
    def moveZeroes(self, nums):
        for num in nums:  # 增加数值类型判断
            if not isinstance(num, (int, float)):
                raise TypeError('Input must be a list of numbers')
```

