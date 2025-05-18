---
title: CS61A（一）
date: 2025-05-13 12:43:48
tags: cs61a
categories: cs
---
## 1. 函数作为抽象工具

函数的一个重要特性是它们可以作为抽象工具，隐藏实现细节。这意味着你可以定义一个函数，而不需要关心它的具体实现，只要知道它的输入和输出即可。
这也是cs61a的核心

**纯函数（Pure functions）**：函数有一些输入（参数）并返回一些输出（调用返回结果）。不改变输出性质，如数学函数
**非纯函数（Non-pure functions）**：除了返回值外，调用一个非纯函数还会产生其他改变解释器和计算机的状态的副作用（side effect）。如print()，输出其实是none，副产物是显示了你的内容
```python
print(print(1),print(2))
1.2
none,none
```
由此，`print` 函数返回 `None`  意味着它不应该用于赋值语句，不能使用嵌套。

实现函数的一个细节就是，实现者为函数的形参选择的名称不应该影响函数行为。所以，以下函数应该提供相同的行为：
```python
def square(x):
    return mul(x, x)
def square(y):
    return mul(y, y)
```
一个函数的含义应该与编写者选择的`参数`名称无关，这个原则对编程语言有重要的意义。最简单的就是函数的参数名称必须在保持函数体局部范围内。

## 2. 语句与表达式

在 Python 中，语句和表达式是两种不同的概念。表达式是有值的，例如 `2 + 2` 或 `x * y`，它们可以被求值并返回一个结果。而语句则是用来执行某些操作的，它们没有值，但会改变程序的状态。
```python
x = 10  # 赋值语句
def square(x):  # def 语句
    return x * x  # return 语句
```
语句的作用是执行某些操作，而不是返回一个值。例如，赋值语句会将一个值绑定到一个变量上，`def` 语句会定义一个函数，而 `return` 语句会从函数中返回一个值。

复合语句是由多个语句组成的结构，它们通常跨越多行，以单行头部开始，并以冒号结尾。复合语句的头部定义了语句的类型，而缩进的句体则包含了要执行的语句。条件语句（if，else）与while是最典型的复合语句
## 3. 函数调用与环境模型

调用一个函数时，Python 会创建一个新的局部环境（或称为帧），在这个环境中，函数的参数被绑定到传递给函数的实际值上。函数体中的代码在这个局部环境中执行，这意味着函数内部的变量不会影响外部的变量。

例如，调用 `square(4)` 时，Python 会创建一个新的局部帧，将参数 `x` 绑定到值 `4`，然后执行 `return x * x`，最终返回 `16`。

这种环境模型确保了函数的局部性，使得函数的实现细节对外部代码透明。每个函数调用都有自己的局部帧，即使多次调用同一个函数，每次调用都有独立的局部帧。

## 4. 局部变量与全局变量

在函数内部定义的变量称为局部变量，它们只在函数的局部帧中有效。与之相对的是全局变量，它们在全局帧中定义，可以在任何地方访问，但不能在函数内部直接修改，除非明确声明。
```python
x = 10  # 全局变量

def func():
    x = 20  # 局部变量
    print(x)  # 输出 20

func()
print(x)  # 输出 10
```

在这个例子中，`func` 内部的 `x` 是局部变量，不会影响全局变量 `x`

## 5. 函数调用
模块导入的两种常见方式——`import math` 和 `from math import sqrt`  

`import math`：隔离的命名空间
- 将整个 `math` 模块导入，但模块内的函数/变量需通过 `math.` 访问。可以有效避免命名冲突
`from math import sqrt`：扁平化到全局作用域

- 将 `sqrt` 函数直接注入当前全局作用域，可直接调用（如 `sqrt()`）。

## 6. 参数默认值

定义通用函数的结果是引入了额外的参数。具有许多参数的函数可能调用起来很麻烦并且难以阅读。

在 Python 中，我们可以为函数的参数提供默认值。当调用该函数时，具有默认值的参数是可选的。如果未提供，则将默认值绑定到形参上。
```python
def pressure(v, t, n=6.022e23):
"""计算理想气体的压力，单位为帕斯卡 使用理想气体定律：
v -- 气体体积，单位为立方米 
t -- 绝对温度，单位为开尔文
n -- 气体粒子，默认为一摩尔 """ 
k = 1.38e-23 
# 玻尔兹曼常数 return n * k * t / v
```
`=` 符号在此示例中表示两种不同的含义，具体取决于使用它的上下文。在 def 语句中，`=` 不执行赋值，而是指示调用 `pressure` 函数时使用的默认值。相比之下，函数体中对 `k` 的赋值语句中将名称 `k` 与玻尔兹曼常数的近似值进行了绑定。
```python
>>> pressure(1, 273.15)
2269.974834
>>> pressure(1, 273.15, 3 * 6.022e23)
6809.924502
```
`pressure` 函数的定义接收三个参数，但上面的第一个调用表达式中只提供了两个。在这种情况下，`n` 的值取自 `def` 语句中的默认值。如果提供了第三个参数，默认值将被忽略。


## 7. 测试

测试是验证程序正确性的重要手段。在 Python 中，我们可以使用 `assert` 语句来验证函数的输出是否符合预期。
```python
def square(x):
    return x * x

assert square(2) == 4, "square(2) 应该返回 4"
```
如果 `assert` 语句中的表达式为真，则程序继续执行；如果为假，则程序会抛出一个错误，并显示指定的错误信息。

除了 `assert` 语句，Python 还提供了 `doctest` 模块，用于编写和运行文档测试。文档测试允许我们将测试用例直接写在函数的文档字符串中。
```python
def sum_naturals(n):
    """返回前 n 个自然数的和。

    >>> sum_naturals(10)
    55
    >>> sum_naturals(100)
    5050
    """
    total, k = 0, 1
    while k <= n:
        total, k = total + k, k + 1
    return total
```
通过 `doctest` 模块，我们可以运行这些测试用例，验证函数的正确性，后面的作业检测用这个很多。

## 8. 高阶函数

高阶函数（Higher-Order Functions）是函数式编程中的一个重要概念。它们可以接受其他函数作为参数，或者返回函数作为结果。这种特性使得高阶函数能够将通用的编程模式抽象化，从而提高代码的复用性和可读性。
以下是作为参数传递：
```python
def summation(n, term):
    total, k = 0, 1
    while k <= n:
        total, k = total + term(k), k + 1
    return total
def identity(x):
    return x

def cube(x):
    return x * x * x

def pi_term(x):
    return 8 / ((4 * x - 3) * (4 * x - 1))

def sum_naturals(n):
    return summation(n, identity)

def sum_cubes(n):
    return summation(n, cube)

def pi_sum(n):
    return summation(n, pi_term)
```
高阶函数不仅可以作为参数传递，还可以作为通用方法来表达复杂的计算逻辑。例如，我们可以定义一个通用的迭代改进算法 `improve`，它接受两个函数作为参数：一个更新函数 `update` 和一个检查函数 `close`。
```python
def improve(update, close, guess=1):
    while not close(guess):
        guess = update(guess)
    return guess
```
通过这种方式，我们可以用 `improve` 函数来实现计算黄金比例的算法： 
```python
def golden_update(guess):
    return 1 / guess + 1

def square_close_to_successor(guess):
    return approx_eq(guess * guess, guess + 1)

def approx_eq(x, y, tolerance=1e-15):
    return abs(x - y) < tolerance

phi = improve(golden_update, square_close_to_successor)
```
## 9. **柯里化**：
强制逐参数分解，如 `f(a)(b)(c)`。
def嵌套可以让全局帧更明朗，不至于混乱，更好的来维护全局变量
```python
def curry2(f): """返回给定的双参数函数的柯里化版本"""
  def g(x): 
     def h(y):
         return f(x, y)
        return h
    return g

```
柯里化可以这样看：f(x)(y)=(f(x))(y),

`curry2` 函数接受一个双参数函数 `f` 并返回一个单参数函数 `g`。当 `g` 应用于参数 `x` 时，它返回一个单参数函数 `h`。当 `h` 应用于参数 `y` 时，它调用 `f(x, y)`。因此，`curry2(f)(x)(y)` 等价于 `f(x, y)`

## 10.  匿名函数lambda
一个 lambda 表达式的计算结果是一个函数，它仅有一个返回表达式作为主体。不允许使用赋值和控制语句。匿名函数，使用方法相同
```python

 s = lambda x: x * x
>>> s
<function <lambda> at 0xf3f490>
>>> s(12)
144
```

## 11. 函数装饰器

函数装饰器（Decorator）是一种特殊的语法，用于在定义函数时应用高阶函数。装饰器可以用来扩展函数的功能，而不需要修改函数的定义。

例如，我们可以定义一个 `trace` 装饰器来追踪函数的调用，这也是我目前看到过的用法：
```python
def trace(fn):
    def wrapped(x):
        print('-> ', fn, '(', x, ')')
        return fn(x)
    return wrapped

@trace
def triple(x):
    return 3 * x
```
通过这种方式，我们可以用 `trace` 装饰器来追踪 `triple` 函数的调用：
```python
triple(12)  # 输出 ->  <function triple at 0x102a39848> ( 12 ) 和 36
```

## 12. 递归函数

递归函数是一种在其函数体中直接或间接调用自身的函数。递归函数的关键在于能够将一个复杂的问题分解为更简单的问题，直到问题变得足够简单可以直接解决。这种分解过程通常被称为“递归分解”。

如：计算一个正整数的所有数字位之和。例如，数字 18117 的数字位之和是 1 + 8 + 1 + 1 + 7 = 18。我们可以使用递归函数来实现这一功能。
```python
def sum_digits(n):
    """返回正整数 n 的所有数字位之和"""
    if n < 10:
        return n
    else:
        all_but_last, last = n // 10, n % 10
        return sum_digits(all_but_last) + last
```
在这个例子中，`sum_digits` 函数将问题分解为两部分：
 最后一位数字 `n % 10`。足够简单，即基线条件
  除最后一位以外的所有数字的和 `sum_digits(n // 10)`。
  
递归函数通常具有以下结构：
**基线条件**：定义了最简单的情况，直接返回结果。
**递归调用**：将问题分解为更简单的问题，并调用自身来解决这些子问题。

## 13. 树递归

树递归是指一个函数在每次调用时会生成多个递归调用。这种模式在解决某些问题时非常自然和直观。例如，计算斐波那契数列的递归实现：
```python
def fib(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib(n - 1) + fib(n - 2)
```
在这个例子中，`fib(n)` 会生成两个递归调用 `fib(n - 1)` 和 `fib(n - 2)`。这种树递归的结构使得问题的分解非常直观，但可能会导致大量的重复计算。这个可能需要先看看树那一节才能更好理解了。

## 14. 分割数
```python

def count_partitions(n, m):   
"""计算使用最大数 m 的整数分割 n 的方式的数量"""     
if n == 0:  
    return 1     
elif n < 0:     
    return 0     
elif m == 0: 
    return 0    
 else:     
     return count_partitions(n-m, m) + count_partitions(n, m-1)
```
使用最大数为 m 的整数分割 n 的方式的数量等于
1. 使用最大数为 m 的整数分割 n-m 的方式的数量，加上//即，存在一个m，继续分割，这个直到n-m-m-m....每一次减就是一层，直到小于m，这个分之就变成余数被m-1分割......（即2的情况）
2. 使用最大数为 m-1 的整数分割 n 的方式的数量，递归，直到变成最小，不是最小就回到1的情况分割
分割(6,4)
即包含4和不包含4的，不包含4的再分成包含3和不包含3的……

*哦哦哦，第一篇技术博客完成啦~
虽然写的很简略，而且只是看公开课的笔记整理了一下就发上来了，不过还是很有成就感的嘛~
目前还都是存货，下一篇应该会有些新内容吧~*