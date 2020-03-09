---
title: You Like Cake(背包容量过于大的折半搜索法)
tags: 分治
category: 算法
date: 2019-01-15 19:57
---

<font size=5> 

# 题目描述

双十一就要来啦！而Yuno刚刚获得了一笔X元的奖金。那么是不是应该清空下购物车呢？
购物车总共有N个物品，每个物品的价格为Vi，Yuno想尽可能地把手头的奖金给花光，所以她要精心挑选一些商品，使得其价格总和最接近但又不会超过奖金的金额。那么Yuno最后最少可以剩下多少钱呢？

## 输入

第一行，两个正整数N和X。
第二行，N个正整数Vi表示第i个物品的价格。

## 输出

输出一个整数，表示Yuno最后最少可以剩下的钱数。

## 样例输入



```
4 50
1 2 3 4
```

## 样例输出



```
40
```

对于100的数据，N≤40，X,Vi≤109

## 思路

由于背包容量过大，所以用不了01背包，但是物品的数目只有40个，但如果依次枚举的话，40^2的复杂度会超时，因此我们分成两部分进行暴搜，第一部分先是随机选20个以内的物品，将这些物品的总价值存入一个数组中，第二个部分是对剩下的20个物品进行暴搜，再将第二部分取到的物品的价值与第一部分存下的那个数组相加求出最小剩余钱数来。

## 代码

```c++
#include <bits/stdc++.h>
#define ll long long
#define inf 0x3f3f3f3f
#define ull unsigned long long
#define met(a, x) memset(a,x,sizeof(a))
#define mp make_pair
using namespace std;
const int mod = 998244353;
const int N = 1e6 + 1e5;
ll w[50],tmp[N];
ll n,x,ans=0,mid;
ll sum=0;
void dfs1(ll k,ll cnt){
    if(cnt>x){
            return ;
    }
    ans=min(ans,x-cnt);
    if(k>mid){
        tmp[++sum]=cnt;
        return;

    }
    dfs1(k+1,cnt);
    dfs1(k+1,cnt+w[k]);
}
void dfs2(ll k,ll cnt){
    if(cnt>x)
        return ;
    if(k>n){
        ll pos=upper_bound(tmp+1,tmp+1+sum,x-cnt)-tmp;
        if(pos)
        ans=min(ans,x-cnt-tmp[pos-1]);
        return ;
    }
    dfs2(k+1,cnt);
    dfs2(k+1,cnt+w[k]);
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    cin>>n>>x;
    ans=x;
    for(int i=1;i<=n;i++){
        cin>>w[i];
    }
    mid=(n+1)/2;
    dfs1(1,0);
    sort(tmp+1,tmp+1+sum);
    if(n)
    {
        dfs2(mid+1,0);
    }
    cout<<ans<<endl;
    return 0;
}
```

