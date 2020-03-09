---
title: Sandglass
tags: 数论
category: 算法
date: 2018-07-29 18:52
---

<font size=5> 

# 题目描述

We have a sandglass consisting of two bulbs, bulb A and bulb B. These bulbs contain some amount of sand. When we put the sandglass, either bulb A or B lies on top of the other and becomes the upper bulb. The other bulb becomes the lower bulb.
The sand drops from the upper bulb to the lower bulb at a rate of 1 gram per second. When the upper bulb no longer contains any sand, nothing happens.
Initially at time 0, bulb A is the upper bulb and contains a grams of sand; bulb B contains X−a grams of sand (for a total of X grams).
We will turn over the sandglass at time r1,r2,..,rK. Assume that this is an instantaneous action and takes no time. Here, time t refer to the time t seconds after time 0.
You are given Q queries. Each query is in the form of (ti,ai). For each query, assume that a=ai and find the amount of sand that would be contained in bulb A at time ti.

Constraints
1≤X≤109
1≤K≤105
1≤r1<r2<..<rK≤109
1≤Q≤105
0≤t1<t2<..<tQ≤109
0≤ai≤X(1≤i≤Q)
All input values are integers.

## 样例输入



```
180
3
60 120 180
3
30 90
61 1
180 180
```

## 样例输出



```
60
1
120
```



## 思路

由于时间是递增的，所以不需要每一次查询都从头到尾遍历，而是维护一个0和x，最后在和A一起处理，最后加上ans是如果0和x存在斜率为0的时候，A斜率可能为1；

## 代码

```c++
#include<bits/stdc++.h>
using namespace std;
const int maxn=100010;
int a[maxn];
int main()
{
    int i,j,X,K,Q;
    scanf("%d%d",&X,&K);
    for(i=1; i<=K; i++)
        cin>>a[i];
    int L=0,R=X;
    int cnt=0,k=0,s=-1,x=0,ans,num;
    int t,A;
    scanf("%d",&Q);
    for(i=1; i<=Q; i++)
    {
       scanf("%d%d",&t,&A);
        while(k<K&&a[k+1]<=t)
        {
            ans=s*(a[k+1]-cnt);
            L=max(0,min(X,L+ans));
            R=max(0,min(X,R+ans));
            cnt=a[k+1];
            s=-s;
            x+=ans;
            k++;
        }
        num=t-cnt;
        A=max(L,min(R,A+x));
        A=max(0,min(X,A+s*num));
        printf("%d\n",A);
    }
    return 0;
}
```

