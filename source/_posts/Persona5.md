---
title: Persona5
tags: 数论
category: 算法
date: 2018-06-05 18:49
---

<font size=5> 

# 题目描述

Persona5 is a famous video game. 
In the game, you are going to build relationship with your friends. 
You have N friends and each friends have his upper bound of relationship with you. Let’s consider the ith friend has the upper bound U[i]. At the beginning, the relationship with others are zero. In the game, each day you can select one person and increase the relationship with him by one. 
Notice that you can’t select the person whose relationship with you has already reach its upper bound. If your relationship with others all reach the upper bound, the game ends. 
It’s obvious that the game will end at a fixed day regardless your everyday choices. Please calculate how many kinds of ways to end the game. Two ways are said to be different if and only if there exists one day you select the different friend in the two ways. 
As the answer may be very large, you should output the answer mod 1000000007 

## 输入

The input file contains several test cases, each of them as described below. 
The first line of the input contains one integers N (1 ≤ N≤ 1000000), giving the number of friends you have.  
The second line contains N integers. The ith integer represents U[i]( 1 ≤ U[i]≤ 1000000), which means the upper bound with ith friend. It’s guarantee that the sum of U[i] is no more than 1000000. 
There are no more than 10 test cases. 

## 输出

One line per case, an integer indicates the answer mod 1000000007. 

## 样例输入



```
3
1 1 1
3
1 2 3
```

## 样例输出



```
6
60
```

## 代码

```c++
#include <cstdio>
#include <cstdlib>
#include <algorithm>
#include <iostream>
#include <cstring>
#include <queue>
#include <cmath>
#include <cstring>
#include <string>
using namespace std;
#define ll long long
const int mod=1e9+7;
ll a[1000100],c[1000100];
ll quickpow(ll a, ll b)
{
    if (b < 0)
        return 0;
    ll ret = 1;
    a %= mod;
    while(b)
    {
        if (b & 1)
            ret = (ret * a) % mod;
        b >>= 1;
        a = (a * a) % mod;
    }
    return ret;
}
ll inv(ll a)
{
    return quickpow(a, mod - 2);
}
int main()
{
    std::ios::sync_with_stdio(false);
    std::cin.tie(0);
    ll i,j,k,m,n;
    a[0]=1;
    for(i=1; i<=1000010; i++)
    {
        a[i]=(a[i-1]*i)%mod;
    }
    ll sum=0,cnt=0;
    while(cin>>n)
    {
        sum=0;
        for(i=1; i<=n; i++)
        {
            cin>>c[i];
            sum+=c[i];
        }
        cnt=a[sum];
        for(i=1; i<=n; i++)
        {
            if(c[i]>1)
            {
                cnt=(cnt*inv(a[c[i]]))%mod;
            }
        }
        cout<<cnt%mod<<endl;
    }
 
    return 0;
}
```

