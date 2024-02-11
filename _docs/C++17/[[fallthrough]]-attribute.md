---
title: "&#91;&#91;fallthrough&#93;&#93;属性"
category: C++17
order: 2
---

> 编译器支持最低版本要求:
> * GCC: 7
> * MSVC: 19.1
> * Clang: 3.9
>
> 提案: [P0188R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0188r1.pdf)

C++17之前的标准下，有如下代码：

```c++
switch (device.status())
{
case sleep:
   device.wake();
   // fall thru
case ready:
   device.run();
   break;
case bad:
   handle_error();
   break;
}
```

在C++17时可以这样写：

```c++
switch (device.status())
{
case sleep:
   device.wake();
   [[fallthrough]];
case ready:
   device.run();
   break;
case bad:
   handle_error();
   break;
}
```

区别就是新增的`[[fallthrough]]`属性。如果这里不写`[[fallthrough]]`，编译也是能通过的，但会报诸如`warning: case statement without break`之类的警告。C++中`switch-case`默认是会继续往下走，有的时候程序员可能因为粗心会漏写`break`，就会导致非预期的运行逻辑，有的语言比如[Go](https://go.dev)则默认会跳出当前`case`，C++17中新增`[[fallthrough]]`属性可以提醒程序员这里正常逻辑应该是怎样的。