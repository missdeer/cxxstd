---
title: if和switch的初始化语句
category: C++17
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 7
> * MSVC: 19.1
> * Clang: 3.9
>
> 提案: [P0305R1](http://wg21.link/p0305r1)

C++17之前的标准下，有如下代码：

```c++
QVariant var = getAnswer();
if (var.isValid())
   use(var);
```

在C++17时可以这样写：

```c++
if (QVariant var = getAnswer(); var.isValid())
   use(var);
```

除了`if`外，`switch`也支持这种新写法：

```c++
switch (Device dev = get_device(); dev.state())
{
case sleep: /*...*/ break;
case ready: /*...*/ break;
case bad: /*...*/ break;
}
```

这个写法秉持了C++相对于C的一个理念上的区别：变量/对象在需要时才声明和定义。这里还更进一步，缩短变量/对象的生命周期。