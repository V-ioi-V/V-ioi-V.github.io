---
title: C++内存分配(静态内存、栈、堆)
tags: C++
category: CS大学生必备
---

内存分为三种：静态内存、栈内存、堆内存。

<!--more-->

<font size=4>

## 静态内存

**保存的对象：**

1.局部static对象

```c++
void func()
{
	static int a;    //局部static对象
}
```

2.类static数据成员

```c++
class Function(){
public:
	static void func();    //类中static函数c
private:
	static int i_func;    //类中static变量
};
```

3.全局变量

**生命周期：**

- 从被定义开始，一直存在于程序的整个生命周期中。

## 栈内存

**保存的对象：**

1. 定义在函数内的非static对象

```c++
void func()
{
	int i_func;    //定义在函数内的非static对象
}
```

**生命周期：**

- 仅在其定义的程序块运行时才存在。

## 堆（动态内存）

**保存的对象：**

1. 动态分配的对象（程序运行时分配的对象）

**生命周期：**

- 由我们手动分配和摧毁

**使用动态内存的原因：**

1. 程序不知道自己需要使用多少对象
2. 程序不知道所需对象的准确类型
3. 程序需要在多个对象间共享数据

**动态内存分配方式：**

1. 运算符：new
2. 语法：`Type *pointer = new Type;`
3. 返回：分配失败则返回空指针

**动态内存摧毁方式：**

1. 运算符：delete
2. 语法：`delete pointer;`

**初始化动态分配对象：**

```c++
int *pi = new int(10);    		    //动态分配一个int对象，初始值为10
string *ps = new string(2, '9');    //动态分配一个string对象，初始值为"99"
int *pa = new int[10]();              //动态分配一个int数组，大小为10，初始值为0
```

- 转自https://blog.csdn.net/weixin_41316331/article/details/89033197