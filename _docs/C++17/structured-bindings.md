---
title: 结构化绑定
category: C++17
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 7
> * MSVC: 19.1
> * Clang: 4
>
> 提案: [P0217R3](http://wg21.link/p0217r3)

反结构化初始化，允许写出这样的代码：

```c++
auto [ x, y, z ] = expr;
```

其中`expr`的类型，是`类tuple`的对象，它的元素可以被绑定到`x`，`y`和`z`上（就是该语句声明的那几个变量）。`类tuple`的对象可以是`std::tuple`，`std::pair`，`std::array`或聚合类型的结构体，例如：

```c++
using Coordinate = std::pair<int, int>;
Coordinate origin() {
  return Coordinate{0, 0};
}

const auto [ x, y ] = origin();
```