---
title: c++设置输出左右对齐、保留小数和取整
tags: 常识
category: 算法
date: 2018-07-08 22:56
---

<font size=5> 

```c++
//左对齐输出和右对齐输出
void main()
{
    cout<<setiosflags(ios::left)    //设置左对齐输出，空格在后

        <<setw(5)<<10<<endl
        <<setw(5)<<100<<endl
        <<setw(5)<<1000<<endl;
}
-----------------------*



*-----------------------
void main()
{
    cout<<setiosflags(ios::right)   //设置右对齐输出，空格在前

        <<setw(5)<<10<<endl
        <<setw(5)<<100<<endl
        <<setw(5)<<1000<<endl;
}
-----------------------*



*-----------------------
#include<iomanip> // 头文件
cout<<fixed<<setprecision(3)<<mid<<endl;//保留小数
-----------------------*



*-----------------------
cout<<setfill('0')<<setw(8)<<a<<endl;//输出前导0（或任何前导）
-----------------------*



*-----------------------
//取整
ceil(double)    //向上取整

floor(double)   //向下取整
```

