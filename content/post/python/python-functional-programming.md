---
title: "Python 函数式编程"
date: 2017-12-22T16:10:57+08:00
categories: ["python"]
tags: ["Python"]
comments: true
original: true
addwechat: true
original: true
addwechat: true
---




函数式编程是一种编程范式，不同于之前的面向对象编程。它是面向数学的抽象，也就是说，这里的`函数`二字不再是我们编程语言中的函数，而是数学中的`函数`了。

<!--more-->

在数学中，y = f(x) ，则因变量 y 是自变量 x 的函数，则 f(x) 是 x 的函数。在这里，f() 只是数学中的一个式子，它对应的就是编程语言中的函数。而 f(x) 对应编程语言中函数执行完的结果。

所以，当我们说到函数式编程时，要把这个函数理解成数学中的函数，它就是 f(x) 。而在数学中，f(x) 可不是一个具体的数值，它也是要计算的（不然哪来的数学考试）。

明确了函数的概念后，就很方便理解函数式编程了。

比如，我们有一个函数 y = x + 1 ，就可以编写如下代码：

``` python
def func(x):
	return x + 1 
```

此时，假设要求当 x = 2 时 y 的值，那么 y = func(2) ，这就是一个很简单的函数式编程了。

## 函数式编程的特点

在函数式语言中，函数是作为一等公民存在的。函数可以在任何地方定义，在函数内或者函数外；函数可以作为函数的参数和返回值，可以对函数进行组合。

函数式编程具有如下的特点：

### 变量的不可变性
每个函数的执行都不会改变外部的数据，而是在函数内部返回一个新的值。这个比较好理解，我们的 f(x) 返回的是 y 而不改变 x 的值。

纯粹的函数式编程语言编写的函数没有变量，因此，任意一个函数，只要输入是确定的，输出就是确定的，这种纯函数称之为没有副作用的。


### 函数也可以当做变量
一个函数 f(x) 也可以当做变量来使用。假设 y = f(x) ，那么 y 就是一个变量，并且它还是个函数，想要执行函数的运算操作，调用 y() 即可。这样就可以实现惰性求值。

函数式编程的一个特点就是，允许把函数本身作为参数传入另一个函数，还允许返回一个函数。

在 Python 中，函数也是可以当做变量的。

### 尾递归优化
由于递归调用是非常消耗内存的，尤其是递归深度很深时，容易发生栈溢出。这是因为函数调用会在内存形成一个`调用帧`，保存调用位置和调用记录等信息。

假如我们在函数 A 的内部调用函数 B，那么在 A 的调用记录上会形成一个 B 的调用记录，如果在 B 的内部还调用了函数 C ，那么还有一个 C 的调用记录。

假如使用`尾调用`，即：在函数的最后一步调用另一个函数。那么我们就不需要保存外层函数的调用记录了，直接用内层函数的调用记录覆盖外层即可。

如果所有的函数都是尾调用，那么完全可以做到每次执行时，调用记录只有一项，节省了内存。


利用`尾调用优化`，我们就可以在递归操作时，使用`尾递归优化`，确保函数的最后一步操作是调用了函数自身。




### 高阶函数

函数式编程还有特性就是高阶函数。

高阶函数就是参数为函数或返回值为函数的函数。

在 Python 中也提供了 `map`，`reduce`，`filter`，`sorted` 高阶函数。

#### map 用法

map() 函数接收两个参数，一个是函数，一个是 Iterable，map 将传入的函数依次作用到序列的每个元素，并把结果作为新的 Iterable 返回。

``` python
def f(x):
    return x + 1
data = [1,2,3,4]
r = map(f,data)
print(list(r))
```

####reduce 用法

reduce() 把一个函数作用在一个序列上，这个函数必须接收两个参数，reduce 把结果继续和序列的下一个元素做累积计算。

``` python 
from functools import reduce

def f(x,y):
    return  x*y

data = [1,2,3,4]
r = reduce(f,data)

print(r)
```

#### filter  用法

filter() 函数用于过滤序列，filter 函数把传入的函数依次作用于每个元素，返回根据返回的是 True 还是 False 决定保留还是丢弃该元素。

``` python
def is_odd(n):
    return n % 2 == 1

result  = filter(is_odd,[1,2,3,4,5])

print(list(result))
```
#### sorted 用法

sorted() 也是一个高阶函数，接收一个函数来实现自定义的排序。

例如按绝对值排序代码：
``` python
print(sorted([-34,5,-2,43],key=abs))
```


### 闭包

闭包是那些带着它们被定义时的环境的函数。特别的，一个闭包可以引用它定义时存在的变量。

``` python
def func(x):
    def f():
        return  x + 1
    return f

result = func(1)

print(type(result))

print(result())
```

闭包的特点：返回的函数还引用了外层函数的局部变量，所以，要正确的使用闭包，就要确保返回函数不用引用任何循环变量，或者后续会发生变化的变量，否则，闭包所持有的外层函数的局部变量将会是最后一次改变时的值。



## 参考

1. http://coolshell.cn/articles/10822.html
2. https://www.zhihu.com/question/28292740
3. http://www.ruanyifeng.com/blog/2015/04/tail-call.html
4. http://www.liaoxuefeng.com/

