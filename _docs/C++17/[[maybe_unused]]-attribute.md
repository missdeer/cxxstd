---
title: "&#91;&#91;maybe_unused&#93;&#93;属性"
category: C++17
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 7
> * MSVC: 19.1
> * Clang: 3.9
>
> 提案: [P0212R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0212r1.pdf)

有代码如下：

```c++
bool res = step1();
step2();
etc();
```

编译时可能会报警告说`res变量定义了却没有使用`诸如此类的话。C++17引入了`[[maybe_unused]]`属性，可以修改如下：

```c++
[[maybe_unused]] bool res = step1();
step2();
etc();
```

这样编译器就不会再对`res`变量没有被使用而报警告了。

除了可以修饰变量，此属性也可用于修饰函数，例如：

```c++
[[maybe_unused]] void f()
{
   /*...*/
}
int main()
{
}
```