---
title: 基于范围的for循环可以拥有不同类型的begin和end
category: C++17
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 6
> * MSVC: 19.1
> * Clang: 3.9
>
> 提案: [P0184R0](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0184r0.html)

[C++11引入的基于范围的for循环](../../C++11/range-for-loop)要求`begin`和`end`（起始值和结尾值）具有相同的类型，这对于大多数情况来说并没有什么问题，比如在遍历STL容器时，总是能返回相同类型的`begin`和`end`。

但是有人觉得这个规范过于受限，于是放开了这个限制，将原来的等价代码修改如下：

```c++
{
  auto && __range = for-range-initializer;
  auto __begin = begin-expr;
  auto __end = end-expr;
  for ( ; __begin != __end; ++__begin ) {
    for-range-declaration = *__begin;
    statement
  }
}
```

与C++11中的相比，唯一的不同就是`__begin`和`__end`可以具有不同类型了，只要它们两个支持通过`operator!=`比较即可。

这为类作者、库作者提供了更多的灵活性。

#### 相关链接

- C++11 [基于范围的for循环](../../C++11/range-for-loop)