---
title: 前向声明枚举类型
category: C++11
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 4.6
> * MSVC: 3.1
> * Clang: 17.0
>
> 提案: [N2764](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2008/n2764.pdf)

在C++中，可以使用枚举类型（enum）定义一组常量。而在某些情况下，我们需要使用前向声明（forward declaration）来提前声明枚举类型，以便在后续代码中使用该枚举类型，这就是forward enum declarations。

在C++11之前，如果需要前向声明一个枚举类型，需要使用如下的语法：

```cpp
enum Color;  // 前向声明

// 后续代码中定义枚举类型
enum Color { RED, GREEN, BLUE };
```

上述代码中，我们先使用enum Color来提前声明了一个枚举类型Color，然后在后续的代码中使用enum Color {...}来定义这个枚举类型。

然而，这种语法并不太直观，并且容易导致一些错误。在C++11中，引入了新的语法，可以更方便地进行前向声明枚举类型：

```cpp
enum class Color : int;  // 前向声明

// 后续代码中定义枚举类型
enum class Color : int { RED, GREEN, BLUE };
```

上述代码中，我们先使用enum class Color : int来提前声明了一个枚举类型Color，其中int为枚举类型的底层类型。然后在后续的代码中使用enum class Color : int {...}来定义这个枚举类型。

使用新的语法，我们可以更清晰地进行枚举类型的前向声明，并且避免了使用旧语法时可能出现的错误。此外，新的语法还可以更好地支持C++11的类型安全枚举（enum class）特性。

需要注意的是，在使用前向声明时，如果需要在其他代码中使用这个枚举类型，仍然需要保证该枚举类型的底层类型是已知的。因此，在使用前向声明时，需要根据实际情况选择合适的底层类型，并保证后续代码中的枚举类型与前向声明的底层类型相同。
