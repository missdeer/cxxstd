---
title: 右尖括号
category: C++11
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 4.3
> * MSVC: 14.0
> * Clang: Yes
>
> 提案: [N1757](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2005/n1757.html)

C++编译器因为实现方式的原因，对右尖括号的处理不是那么智能，比如：

```c++
typedef std::vector<int> IntVector;
typedef std::list<std::vector<int> > IntVecList;
void func1(std::list<std::vector<int> > = IntVecList());
void func2(std::vector<int> = IntVector());
```

这里`> >`，`> > =`和`> =`中间都加了空格，才得以正确编译通过，如果不加空格，编译器就会识别成错误的符号，`>>`是按位右移操作符，`>=`是大于等于比较符，`>>=`就不知道是识别成`>> =`还是`> >=`了。

在C++11中，对第一种情况做了改良，能够正常识别出`>>`了，也就是说下面这条代码也是合法的了：

```c++
typedef std::list<std::vector<int>> IntVecList;
```

实在是个微小的改进。