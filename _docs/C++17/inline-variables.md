---
title: Inline变量
category: C++17
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 7
> * MSVC: 
> * Clang: 3.9
>
> 提案: [P0386R2](http://wg21.link/p0386r2)

C++17之前的标准，如果在多个文件中使用同一个全局变量或类的静态成员变量，需要在头文件中声明，在cpp文件中定义，例如：

```c++
// foo.h
extern int foo;

struct Foo {
   static int foo;
};

// foo.cpp
int foo = 10;

int Foo::foo = 10;
```

C++17扩展了`inline`关键字的功能，只需要在头文件中声明并定义即可达到相同的效果：

```c++
// foo.h
inline int foo = 10;

struct Foo {
   static inline int foo = 10;
};
```

