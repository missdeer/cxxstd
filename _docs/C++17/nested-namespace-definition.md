---
title: 嵌套名字空间定义
category: C++17
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 6
> * MSVC: 19.0
> * Clang: 3.6
>
> 提案: [N4230](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4230.html)

C++17之前的标准下，有如下代码：

```c++
   namespace A {
      namespace B {
         namespace C {
            struct Foo { };
            //...
         }
      }
   }
```

在C++17时可以这样写：

```c++
   namespace A::B::C {
      struct Foo { };
      //...
   }
```

一个小小的语法糖，让代码更简洁。