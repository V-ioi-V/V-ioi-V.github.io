---
title: cin.tie与sync_with_stdio加速输入输出
tags: 常识
category: 算法
date: 2018-06-07 18:26 
---

<font size=5> 

我是怎么在不知道这一对函数的情况下活到今天的![img](http://img.baidu.com/hi/jx2/j_0012.gif)，以前碰到cin TLE的时候总是傻乎乎地改成scanf，甚至还相信过C++在IO方面效率低下的鬼话，殊不知这只是C++为了兼容C而采取的保守措施。

## tie

tie是将两个stream绑定的函数，空参数的话返回当前的输出流指针。



```c++
#include <iostream>
#include <fstream>
 
///////////////////////////SubMain//////////////////////////////////
int main(int argc, char *argv[])
{
    std::ostream *prevstr;
    std::ofstream ofs;
    ofs.open("test.txt");
 
    std::cout << "tie example:\n";    // 直接输出到屏幕
 
    *std::cin.tie() << "This is inserted into cout\n";    // 空参数调用返回默认的output stream，也就是cout
    prevstr = std::cin.tie(&ofs);                        // cin绑定ofs，返回原来的output stream
    *std::cin.tie() << "This is inserted into the file\n";    // ofs，输出到文件
    std::cin.tie(prevstr);                                    // 恢复
 
    ofs.close();
    system("pause");
    return 0;
}
///////////////////////////End Sub//////////////////////////////////
```



## 输出

```c++
tie example:
This is inserted into cout
请按任意键继续. . .
```



## sync_with_stdio

这个函数是一个“是否兼容stdio”的开关，C++为了兼容C，保证程序在使用了std::printf和std::cout的时候不发生混乱，将输出流绑到了一起。

## 应用

在ACM里，经常出现数据集超大造成 cin TLE的情况。这时候大部分人（包括原来我也是）认为这是cin的效率不及scanf的错，甚至还上升到C语言和C++语言的执行效率层面的无聊争论。其实像上文所说，这只是C++为了兼容而采取的保守措施。我们可以在IO之前将stdio解除绑定，这样做了之后要注意不要同时混用cout和printf之类。

在默认的情况下cin绑定的是cout，每次执行 << 操作符的时候都要调用flush，这样会增加IO负担。可以通过tie(0)（0表示NULL）来解除cin与cout的绑定，进一步加快执行效率。

如下所示：

```c++
#include <iostream>
int main() 
{
    std::ios::sync_with_stdio(false);
    std::cin.tie(0);
    // IO
}
```

## 转载自：

http://www.hankcs.com/program/cpp/cin-tie-with-sync_with_stdio-acceleration-input-and-output.html