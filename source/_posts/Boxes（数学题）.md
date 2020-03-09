---
title: Boxes（数学题）
tags: 数论
category: 算法
date: 2018-04-21 18:08
---

<font size=5> 

# 题目描述

There are N boxes arranged in a circle. The i-th box contains Ai stones.

Determine whether it is possible to remove all the stones from the boxes by repeatedly performing the following operation:

Select one box. Let the box be the i-th box. Then, for each j from 1 through N, remove exactly j stones from the (i+j)-th box. Here, the (N+k)-th box is identified with the k-th box.
Note that the operation cannot be performed if there is a box that does not contain enough number of stones to be removed.

Constraints
1≤N≤1e5
1≤Ai≤1e9

## 输入

The input is given from Standard Input in the following format:

N
A1 A2 … AN

## 输出

If it is possible to remove all the stones from the boxes, print YES. Otherwise, print NO.

## 样例输入



```
5
4 5 1 2 3
```

## 样例输出



```
YES
```

All the stones can be removed in one operation by selecting the second box.

## 思路

- 假设a1有x1个。。。an有xn个，然后全部加起来就是n*（n+1）/2*(x1+x2+...xn)=(a1+a2+...an),求出(x1+x2+...xn)来，然后相邻的两个a相减就可求出

## 代码

```c++
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const int maxn=1e5+10;
ll a[maxn];
int main()
{
    ll i,j,k,m,n;
    cin>>n;
    ll cnt=0,ans=0,sum=0;
    for(i=1; i<=n; i++)
    {
        cin>>a[i];
        sum+=a[i];
    }
    if((2*sum)%(n*(n+1))!=0)
         
    {
        cout<<"NO"<<endl;
        return 0;
    }
    else
    {
        ans=(2*sum)/(n*(n+1));
        for(i=1; i<=n; i++)
        {
            if(i==n)
            {
                if((a[i]-a[1]+ans)%n!=0||(a[i]-a[1]+ans)<0)
                {
                    cout<<"NO"<<endl;
                    return 0;
                }
            }
            else
            {
                if((a[i]-a[i+1]+ans)%n!=0||(a[i]-a[i+1]+ans)<0)
                {
                    cout<<"NO"<<endl;
                    return 0;
                }
            }
        }
        cout<<"YES"<<endl;
    }
    return 0;
}
```

