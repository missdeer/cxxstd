---
title: "&#91;&#91;nodiscard&#93;&#93;属性"
category: C++17
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 7
> * MSVC: 19.1
> * Clang: 3.9
> 
> 提案: [P0189R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0189r1.pdf)

C++17之前的标准下，有如下代码：

```c++
struct SomeInts
{
   bool empty();
   void push_back(int);
   //etc
};

void random_fill(SomeInts & container,
      int min, int max, int count)
{
   container.empty(); // empty it first
   for (int num : gen_rand(min, max, count))
      container.push_back(num);
}
```

在C++17时可以这样写：

```c++
struct SomeInts
{
   [[nodiscard]] bool empty();
   void push_back(int);
   //etc
};

void random_fill(SomeInts & container,
      int min, int max, int count)
{
   container.empty(); // empty it first
   for (int num : gen_rand(min, max, count))
      container.push_back(num);
}
```

加上`[[nodiscard]]`后，编译器发现`empty()`函数被调用时并没有使用它的返回值，则会报一个诸如`warning: ignoring return value of 'bool empty()'`的警告。