---
title: cs61a(二）
date: 2025-05-18 18:47:06
tags: cs61a
categories: cs
---
### 1. 数据类型
- **整数（`int`）**：用于表示整数，可以是任意大小。Python 3 中的 `int` 是无界的，这意味着它可以表示任意大的整数，只要计算机的内存足够。
- **浮点数（`float`）**：用于表示小数。浮点数的表示是近似的，这意味着它们不能精确表示所有的小数值。浮点数的精度是有限的，这在进行复杂的数学运算时可能会导致误差。
- **复数（`complex`）**：用于表示复数，包含实部和虚部
- **布尔类型（`bool`）**：用于表示逻辑值，只有两个可能的值：`True` 和 `False`。布尔类型在条件语句和逻辑运算中非常有用。
- **字符串（`str`）**：用于表示文本数据。字符串是不可变的，这意味着一旦创建，就不能修改。
- **元组（`tuple`）**：用于表示一组不可变的值。元组可以包含不同类型的数据。
- **列表（`list`）**：是一种可变的序列类型，用于表示一组有序的值。列表可以包含不同类型的数据，并且可以动态地添加或删除元素
- **字典（`dict`）**：是一种键值对的集合，用于表示一组无序的键值对。字典的键必须是不可变类型（如字符串、整数或元组），而值可以是任意类型。
-  自定义数据类型在后面面对对象时再说吧

## 2. 数据抽象
在编程中，数据抽象是一种强大的设计方法，它允许我们将**数据的表示与数据的使用分离**。通过数据抽象，我们可以构建更加模块化、易于维护和修改的程序。数据抽象的核心思想是将数据的表示与数据的使用分离。这意味着我们可以通过定义一组抽象的操作来处理数据，而不需要关心数据的具体表示细节。这种分离使得程序更加模块化，也更容易维护和修改。 

- 抽象屏障
抽象屏障是数据抽象中的一个重要概念。它通过一组选择器和构造器函数来实现，这些函数定义了如何创建和操作抽象数据。抽象屏障的作用是隐藏数据的具体表示，使得程序的其他部分只需要通过这些函数来操作数据。抽象屏障使得程序的各个部分之间保持独立性。通过抽象屏障，我们可以轻松地修改数据的表示，而不需要修改使用数据的代码。例如，我们可以将有理数的表示从列表改为元组，而不需要修改 `add_rationals`、`mul_rationals` 等函数（只需修改`rational`这个函数）。
```python
def rational(n, d):
    """返回分子为 n、分母为 d 的有理数"""
    return [n, d]

def numer(x):
    """返回有理数 x 的分子"""
    return x[0]

def denom(x):
    """返回有理数 x 的分母"""
    return x[1]
def add_rationals(x, y):
    """返回两个有理数 x 和 y 的和"""
    nx, dx = numer(x), denom(x)
    ny, dy = numer(y), denom(y)
    return rational(nx * dy + ny * dx, dx * dy)

def mul_rationals(x, y):
    """返回两个有理数 x 和 y 的乘积"""
    return rational(numer(x) * numer(y), denom(x) * denom(y))

def print_rational(x):
    """打印有理数 x"""
    print(numer(x), '/', denom(x))

def rationals_are_equal(x, y):
    """检查两个有理数 x 和 y 是否相等"""
    return numer(x) * denom(y) == numer(y) * denom(x)

from fractions import gcd

def rational(n, d):
    """返回分子为 n、分母为 d 的有理数，简化为最简形式"""
    g = gcd(n, d)
    return (n // g, d // g)
```
有理数这个例子可以很好的阐释什么是数据抽象。

## 3. 序列
Python 提供了多种内置的序列类型，如列表和范围，它们都满足序列抽象的条件。这里以列表举例。
```python
# 成员资格
digits = [1, 8, 2, 8]
print(2 in digits)  # 输出 True
print(1828 not in digits)  # 输出 True
# 切片
digits = [1, 8, 2, 8]
print(digits[0:2])  # 输出 [1, 8]
print(digits[1:])   # 输出 [8, 2, 8]
# 加减法
print('Berkeley' + ', CA')  # 输出 'Berkeley, CA'
print('Shabu ' * 2)         # 输出 'Shabu Shabu '

```
- **序列遍历**
在许多情况下，我们希望依次遍历序列的元素并根据元素值执行一些计算。这种情况十分常见，所以 Python 提供了一个额外的控制语句来处理序列的数据：`for` 循环语句。考虑统计一个值在序列中出现了多少次的问题。我们也可以使用 `while` 循环实现一个函数。
```python
def count(s, value):
    """统计在序列 s 中出现了多少次值为 value 的元素"""
    total = 0
    for elem in s:
        if elem == value:
                total = total + 1
        return total
>>> count(digits, 8)
>>> list(range(5, 8))
[5, 6, 7]
>>> list(range(4))
[0, 1, 2, 3]
```

- **序列处理**
**列表推导式（List Comprehensions）**：许多序列操作可以通过对序列中的每个元素使用一个固定表达式进行计算，并将结果值保存在结果序列中。在 Python 中，列表推导式是执行此类计算的表达式。```
```python
>>> odds = [1, 3, 5, 7, 9]
>>> [x+1 for x in odds]
[2, 4, 6, 8, 10]
```

另一个常见的序列操作是选取原序列中满足某些条件的值。列表推导式也可以表达这种模式，例如选择 `odds` 中所有可以整除 25 的元素。
```python
>>> [x for x in odds if 25 % x == 0]
[1, 5]
```
列表推导式的一般形式是：
```python 
[<map expression> for <name> in <sequence expression> if <filter expression>]
# 举个例子
>>> def divisors(n): return [1] + [x for x in range(2, n) if n % x == 0]
>>> divisors(4)
>>> [1, 2] 
>>> divisors(12) 
>>> [1, 2, 3, 4, 6]
```
## 4. 字符串

在计算机科学中，文本值可能比数字更重要。比如 Python 程序是以文本形式编写和存储的。Python 中文本值的内置数据类型称为字符串（string），对应构造函数 `str`。
字符串字面量（string literals）可以表示任意文本，使用时将内容用单引号或双引号括起来。

字符串与序列有很多共通之处
```python
>>> city = 'Berkeley'
>>> len(city)
8
>>> city[3]
'k'
>>> 'Berkeley' + ', CA'
'Berkeley, CA'
>>> 'Shabu ' * 2
'Shabu Shabu '
>>> 'here' in "Where's Waldo?"
True
```
**字符串强制转换（String Coercion）**：通过以对象值作为参数调用 `str` 的构造函数，可以从 Python 中的任何对象创建字符串。字符串的这一特性在用构造各种类型对象的描述性字符串时非常有用。
```python
>>> str(2) + ' is an element of ' + str(digits)
'2 is an element of [1, 8, 2, 8]'
```

## 5. 树
树是一种层次化的数据结构，用于表示具有层次关系的数据。树由节点组成，每个节点可以有多个子节点，这个以图片方式来看会很直观。
以下是树的基本函数。
```python
def tree(root_label, branches=[]):
    return [root_label] + list(branches)# 树

def label(tree):
    return tree[0]# 树的标签，第一个

def branches(tree):
    return tree[1:]# 树的支

def is_tree(tree):
    if type(tree) != list or len(tree) < 1:# 非列表，长度为1
        return False
    for branch in branches(tree):
        if not is_tree(branch):# 分支必须是树
            return False
    return True

def is_leaf(tree):# 叶就是非树的支
    return not branches(tree)
```


树递归（Tree-recursive）函数可用于构造树。例如，我们定义 The **n**th Fibonacci tree 是指以第 n 个斐波那契数为根标签的树。那么当 n > 1 时，它的两个分支也是 Fibonacci tree。这可用于说明斐波那契数的树递归计算。
```python
>>> def fib_tree(n):
        if n == 0 or n == 1:
            return tree(n)
        else:
            left, right = fib_tree(n-2), fib_tree(n-1)
            fib_n = label(left) + label(right)
            return tree(fib_n, [left, right])
>>> fib_tree(5)
[5, [2, [1], [1, [0], [1]]], [3, [1, [0], [1]], [2, [1], [1, [0], [1]]]]]
```
其实还是很简单的，想清楚斐波那契数列是什么样的，还是很简单的
**树递归**同时也可以做到遍历计算
```python
# 树的遍历
def count_leaves(tree):
    if is_leaf(tree):# 是叶子就回复1
        return 1
    else:
        branch_counts = [count_leaves(b) for b in branches(tree)]# 从头到尾遍历
        return sum(branch_counts)

t = tree(3, [tree(1), tree(2, [tree(1), tree(1)])])
print(count_leaves(t))  # 输出 4
```
**e.g.**
依然是分割树，思想还是一样的，但是用树，会多一个label,并且是以T/F来判断分支的情况：
```python
>>> def partition_tree(n, m):
        """返回将 n 分割成不超过 m 的若干正整数之和的分割树"""
        if n == 0:
            return tree(True)
        elif n < 0 or m == 0:
            return tree(False)
        else:
            left = partition_tree(n-m, m)
            right = partition_tree(n, m-1)
            return tree(m, [left, right])

>>> partition_tree(2, 2)
[2, [True], [1, [1, [True], [False]], [False]]]
```
其实更重要的是这个打印函数
```python 
>>> def print_parts(tree, partition=[]):
        if is_leaf(tree):
'''如果当前节点是叶子节点，且该叶子节点的标签为 `True`，则表示当前路径上的分割方案是有效的。'''
            if label(tree):
                print(' + '.join(partition))
        else:
            left, right = branches(tree)# 由前面可知，并非叶节点，肯定为两个分支--左右
            m = str(label(tree))
'''将当前节点的标签值（即当前分割的部分大小）转换为字符串'''
            print_parts(left, partition + [m])
            print_parts(right, partition)
'''递归调用 `print_parts` 函数处理左分支 `left`，并将当前的分割部分 `m` 添加到 `partition` 列表中，形成新的分割累积列表。右边就没有了'''
>>> print_parts(partition_tree(6, 4))
4 + 2
4 + 1 + 1
3 + 3
3 + 2 + 1
3 + 1 + 1 + 1
2 + 2 + 2
2 + 2 + 1 + 1
2 + 1 + 1 + 1 + 1
1 + 1 + 1 + 1 + 1 + 1
```

## 6. 链表
链表由节点组成，每个节点包含一个数据元素和一个指向下一个节点的指针。在Python中，我们可以使用列表来模拟链表的结构。例如，创建一个包含四个元素的链表：1 → 2 → 3 → 4。
```python
four = link(1, link(2, link(3, link(4, empty))))
```
这个链表的结构如下：

`[1, [2, [3, [4, 'empty']]]]`

其中：
- 头节点是 1，指向下一个节点。
- 第二个节点是 2，指向下一个节点。
- 第三个节点是 3，指向下一个节点。
- 第四个节点是 4，指向空值 `empty`，表示链表的结束。
以下是链表的基本函数：
```python
>>> empty = 'empty'
>>> def is_link(s):
        """判断 s 是否为链表"""
        return s == empty or (len(s) == 2 and is_link(s[1]))

>>> def link(first, rest):
        """用 first 和 rest 构建一个链表"""
        assert is_link(rest), " rest 必须是一个链表"
        return [first, rest]

>>> def first(s):
        """返回链表 s 的第一个元素"""
        assert is_link(s), " first 只能用于链表"
        assert s != empty, "空链表没有第一个元素"
        return s[0]

>>> def rest(s):
        """返回 s 的剩余元素"""
        assert is_link(s), " rest 只能用于链表"
        assert s != empty, "空链表没有剩余元素"
        return s[1]
```

链表很多东西都需要自己定义，其基本的思路就是循环剥壳
```python
>>> def len_link(s):
        """返回链表 s 的长度"""
        length = 0
        while s != empty:
            s, length = rest(s), length + 1
        return length

>>> def getitem_link(s, i):
        """返回链表 s 中索引为 i 的元素"""
        while i > 0:
            s, i = rest(s), i - 1
        return first(s)
```
所以递归+rest()就能构成链表的基本架构
下面的就是链表的另外一些函数与其实现方式
```python
>>> def extend_link(s, t):
        """返回一个在 s 链表的末尾连接 t 链表后的延长链表"""
        assert is_link(s) and is_link(t)
        if s == empty:
            return t
        else:
            return link(first(s), extend_link(rest(s), t))

>>> extend_link(four, four)
[1, [2, [3, [4, [1, [2, [3, [4, 'empty']]]]]]]]

>>> def apply_to_all_link(f, s):
        """应用 f 到 s 中的每个元素"""
        assert is_link(s)
        if s == empty:
            return s
        else:
            return link(f(first(s)), apply_to_all_link(f, rest(s)))

>>> apply_to_all_link(lambda x: x*x, four)
[1, [4, [9, [16, 'empty']]]]

>>> def keep_if_link(f, s):
        """返回 s 中 f(e) 为 True 的元素"""
        assert is_link(s)
        if s == empty:
            return s
        else:
            kept = keep_if_link(f, rest(s))
            if f(first(s)):
                return link(first(s), kept)
            else:
                return kept

>>> keep_if_link(lambda x: x%2 == 0, four)
[2, [4, 'empty']]

>>> def join_link(s, separator):
        """返回由 separator 分隔的 s 中的所有元素组成的字符串"""
        if s == empty:
            return ""
        elif rest(s) == empty:
            return str(first(s))
        else:
            return str(first(s)) + separator + join_link(rest(s), separator)
>>> join_link(four, ", ")
'1, 2, 3, 4'
```

后面还有用链表实现分割树，这样算下来已经用函数式，树，链表三种形式去实现分割树了，基本思路都是一样的。


*又是一篇技术博客，好耶！
这次的链表是又学了一遍重新写的笔记，也是重新体会到了链表的魅力（不过还是浅尝即止罢了）。
此外还有一件喜事，我成功转到计算机了！！*