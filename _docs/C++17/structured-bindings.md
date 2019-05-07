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

C++11加入了`std::get`，可以对`std::tuple`，`std::pair`，`std::array`进行操作，获取指定序号的元素，例如：

```c++
auto p = std::make_pair(1, 2);
auto t = std::make_tupe(1, 2, 3);
std::array<int> a = {1, 2, 3, 4};

auto p0 = std::get<0>(p);
auto t1 = std::get<1>(t);
auto a2 = std::get<2>(a);
```

同时也加入了`std::tie`来获取元素，并以这些元素的值创建一个新的`std::tuple`，例如：

```c++
int t0, t1, t2;
std::tie(t0, t1, t2) = t;
```

以上`std::tie`会创建一个新的`std::tuple`对象，该对象保持了对`t0`，`t1`，`t2`的左值引用，所以当再将`t`赋值给此对象时，就把`t`中对应位置的元素的值赋值给`t0`，`t1`，`t2`了。

C++17中引入了反结构化初始化，允许写出这样的代码：

```c++
auto [ x, y, z ] = expr;
```

其中`expr`的类型，是`类tuple`的对象，它的元素可以被绑定到`x`，`y`和`z`上（就是该语句声明的那几个变量）。`类tuple`的对象可以是`std::tuple`，`std::pair`，`std::array`或聚合类型的结构体以及普通数组，例如：

```c++
using Coordinate = std::pair<int, int>;
Coordinate origin() {
  return Coordinate{0, 0};
}

const auto [ x, y ] = origin();

int a[3] = {1,2,3};
auto [a1, a2, a3] = a;
```

这种写法大大简化了代码，不但能达到C++11中引入的`std::get`和`std::tie`的效果，还增加了对聚合类型的结构体以及普通数组的支持。