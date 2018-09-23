---
title: go 语言中方法调用是值传递还是引用传递？
date: 2017-01-27 15:04:14
tags: [Go, Back-End]
---

我们先定义一个叫`Student`的结构体如下：

```Golang
  type Student struct {
    name string
  }
```
<!-- more -->

在看方法调用之前，我们先看一下 go 语言中的函数是怎么调用结构体参数的：

```Golang
  func setNameByValue(student Student, name string)  {
    student.name = name
  }

  func setNameByPoint(student *Student, name string)  {
    student.name = name
  }

  student := Student{"lx"}
  println(student.name) // lx

  setNameByValue(student, "vc")
  println(student.name) // lx

  setNameByPoint(&student, "vc")
  println(student.name) // vc
```

从上面这段代码中我们可以看到：`要想改变结构体的某个字段，就必须给函数传递结构体指针，也就是传递引用`

如果这么这样调用函数会怎样？

```Golang
  setNameByValue(*student, "vc")
  setNameByPoint(student, "vc")
```

会编译报参数不符的错误：`cannot use student (type Student) as type Student(*Student) in argument to setNameByPoint`，也就是说：**给函数传递的参数必须符合函数定义时对参数的要求**

既然传递的是指针，那么 `setNameByPoint` 应该写成：
```Golang
  func setNameByPoint(student *Student, name string)  {
    (*student).name = name
  }
```
其实这是对的，上面那种写法也是对的，这是因为 Golang 的编译器在底层做了优化，它知道传进来的一定是一个指针（不是指针就会编译报错），所以在函数内部使用时即使没有加`*`号，它也知道这是一个指针变量

清楚了 Golang 的函数调用规则之后我们再来看方法调用，在 Golang 中方法也不过就是有接受者（receiver）的函数而已，给`Student`定义下面两个方法：
```Golang
  func (student Student) setNameByValue(name string) {
    student.name = name
  }

  func (student *Student) setNameByPoint(name string) {
    student.name = name
  }
```

接着再分别调用它们：
```Golang
  student := Student{"lx"}
  println(student.name) // lx

  student.setNameByValue("vc")
  println(student.name) // lx

  (&student).setNameByPoint("vc")
  println(student.name) // vc
```
符合预期，我们再用错误的参数来调用看看会不会报错：
```Golang
  (&student).setNameByValue("vc")
  println(student.name) // lx

  student.setNameByPoint("vc")
  println(student.name) // vc
```
没有报错，并且：我明明是给`setNameByValue`方法传递的指针（引用），为什么`student.name`没有改变；给`setNameByPoint`传递的是值，`student.name`却改变了？
> 原来，结构体调用方法不区别是值调用还是指针调用，在调用时再把调用者转换为方法定义时的接受者（receiver）类型，所以，**方法调用是传递值还是传递引用只看这个方法定义的`接受者`是值类型还是指针类型**

思考：在大多数语言中，方法都是传递引用的，而且还没有提供值传递的语法，Golang 两种都支持，并且两者在调用时并没有语法上的区别，我也没有看到值传递时有特别的好处（避免错误地修改了不想改的字段？），所以，为了和其它语言统一，我一般全部使用指针类型定义方法，再使用值类型去调用它们。这样在语法上更统一，也更简洁
