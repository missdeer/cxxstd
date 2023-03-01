---
title: Alignof
category: C++11
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 4.5
> * MSVC: 19.0
> * Clang: 2.9
>
> 提案: [N2341](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2007/n2341.pdf)

C++11引入了一个对齐相关的关键字——`alignof`，它是一个类型查询运算符，用于获取一个类型的对齐要求。

使用`alignof`关键字可以查询一个类型的对齐要求，语法如下：

```cpp
alignof(type)
```

其中，`type`可以是任何类型。例如，我们可以查询一个`double`类型的对齐要求：

```cpp
alignof(double)
```

在C++中，对齐要求是由编译器决定的，它通常取决于CPU架构和操作系统。例如，在x86架构下，通常使用4字节对齐。因此，上面的代码应该返回4。

需要注意的是，`alignof`返回的是一个常量表达式，它的值在编译时就已经确定了。这意味着，我们可以在编译时利用`alignof`来进行静态断言，以确保类型满足特定的对齐要求。例如：

```cpp
static_assert(alignof(double) == 8, "double must be 8-byte aligned");
```

这个静态断言会在编译时检查`double`类型的对齐要求是否为`8`，如果不是，就会产生一个编译错误。

需要注意的是，`alignof`关键字不是一个类型或变量的属性，它只是用于查询类型的对齐要求。因此，它不能用于变量或表达式。例如，下面的代码是不合法的：

```cpp
int i;
alignas(4) int j;
std::cout << alignof(i) << std::endl; // Error!
std::cout << alignof(j) << std::endl; // OK
```
