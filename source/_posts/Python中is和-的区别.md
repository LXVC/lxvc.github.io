---
title: Python 中 is 和 == 的区别
date: 2017-01-26 16:23:02
tags: [Python, Back-End]
---

我们经常会遇到判断两个变量是否相等的情况，而在 Python 中判断变量是否相等有两种方式：
`is` 和 `==`，到底我们应该使用哪一种呢？看完这篇文章你就能清楚地知道到底该使用哪一种了

<!-- more -->

先看这一段代码：

```Python
  foo = 'foo'
  bar = 'foo'
  foo == bar # True
  foo is bar # True

  foo = 3
  bar = 3
  foo == bar # True
  foo is bar # True
```

似乎没什么区别啊，那是不是说 `is` 和 `==` 是等价的，用哪种看个人喜好？当然不是，请看下面这个例子：

```Python
  foo = ['foo']
  bar = ['foo']
  foo == bar # True
  foo is bar # False

  foo = {'foo': 'bar'}
  bar = {'foo': 'bar'}
  foo == bar # True
  foo is bar # False
```

为什么这里 `is` 和 `==` 会得到截然相反的结果？看最后一个例子：

```Python
  foo = 'foo'
  bar = foo
  foo = 'bar'
  print(foo) # bar
  print(bar) # foo
  id(foo) # 4497499672
  id(bar) # 4496800208
  foo == bar # False
  foo is bar # False

  foo = ['foo']
  bar = foo
  foo[0] = 'bar'
  print(foo) # ['bar']
  print(bar) # ['bar']
  id(foo) # 4496750728
  id(bar) # 4496750728
  foo == bar # True
  foo is bar # True
```
所以，到这里可以得出我们最后的结论了：

> `is` 实质上是对变量进行 `id()` 求值后的返回值比较，在对**值类型**的变量进行比较时用 `is` 还是 `==`，没有区别；在对**对象实例**进行比较时，如果需要判断两个变量是否是**同一个实例的引用**，用 `is`，如果需要判断两个对象实例变量的值是否完全相等，用 `==`。特别的：如果 `a == b` 为 `True`，`a is b` 不一定为 `True`；如果 `a is b` 为 `True`，则 `a == b` 一定为 `True`

2016-02-01更新：
```Python
  nan = float('NaN')
  nan_copy = nan
  nan_copy == nan # False
  nan_copy is nan # True
```
这个特例不符合上面得出的结论
