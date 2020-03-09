---
title: Princess Principal（思维题）
tags: 瞎搞
category: 算法
date:  2018-10-07 18:23
---

<font size=5> 

# 题目描述

阿尔比恩王国（the Albion Kingdom）潜伏着一群代号“白鸽队（Team White Pigeon）”的间谍。在没有任务的时候，她们会进行各种各样的训练，比如快速判断一个文档有没有语法错误，这有助于她们鉴别写文档的人受教育程度。
这次用于训练的是一个含有n个括号的文档。括号一共有mm种，每种括号都有左括号和右括号两种形式。我们定义用如下的方式定义一个合法的文档:
1.一个空的字符串是一个合法的文档。
2.如果A,B都是合法的文档，那么AB也是合法的文档。
3.如果S是合法的文档，那么aSb也是合法的文档，其中a,b是同一种括号，并且a是左括号，b是右括号。
现在给出q个询问，每次询问只考虑文档第ll至rr个字符的情况下，文档是不是合法的。

## 输入

第一行两个整数n,m,q(1≤n,m,q≤1e6)。
第二行有n个空格隔开的整数x，第i个整数xi(0≤xi<m∗2)代表文档中的第i个字符是第⌊x/2⌋种括号，且如果xi是偶数，它代表一个左括号，否则它代表一个右括号。
接下来q行，每行两个空格隔开的整数l,r(1≤l≤r≤n)，代表询问第l至r个字符构成的字符串是否是一个合法的文档。

## 输出

输出共q行，如果询问的字符串是一个合法的文档，输出"Yes",否则输出"No"。

## 样例输入



```
6 4 3
0 2 3 1 4 7
1 4
1 5
5 6
```

## 样例输出



```
Yes
No
No
```



## 思路

- 判断一段括号序列是合法的就要求每一个括号包含的是空或者是合法的括号序列，因此逐渐递推的话，将那几个中间为空的括号序列抵消掉之后可以将整个合法的括号序列抵消掉；

- 因此我们可以用栈来处理匹配的括号，给每次进入的括号都赋一个值，这个值表示的是把当前栈中存在的合法的括号序列都抵消掉以后存在的栈顶的括号的下标。

- 每次输入的括号都与前一个相对比，如果能配对的话那就将前面那个括号弹出，如果不能配对就将这个括号推进去。

- 最后我们可以通过对比我们要查询的序列进栈之前的值和进栈之后的值是否相同来判断这个括号序列是否合法。

## 代码

```c++
#include<cstdio>
#include<stack>
using namespace std;
stack<int> stk;
int a[1000005];
int vis[1000005];
int Scan()
{   
    int res = 0, flag = 0;  
    char ch;  
    if ((ch = getchar()) == '-') 
    {   
        flag = 1;  
    }    
    else if(ch >= '0' && ch <= '9') 
    {
        res = ch - '0'; 
    }
    while ((ch = getchar()) >= '0' && ch <= '9')  
    {
        res = res * 10 + (ch - '0');  
    }
    return flag ? -res : res;  
}
int main()
{
    int n,m,q;
    n=Scan();m=Scan();q=Scan();
    for(int i=1; i<=n; ++i)
        a[i]=Scan();
    for(int i=1; i<=n; ++i)
    {
        if(stk.empty())
        {
            stk.push(i);
            vis[i]=stk.top();
        }
        else
        {
            int top=stk.top();
            if(a[top]+1==a[i]&&a[i]%2==1)
            {
                stk.pop();
                if(stk.empty())
                   vis[i]=0;
                else vis[i]=stk.top();
            }
            else
            {
                stk.push(i);
                vis[i]=stk.top();
            }
        }
    }
    for(int i=1; i<=q; ++i)
    {
        int l,r;
        l=Scan();r=Scan();
        if(vis[l-1]==vis[r])
        puts("Yes");
        else puts("No");
    }
    return 0;
}
```

