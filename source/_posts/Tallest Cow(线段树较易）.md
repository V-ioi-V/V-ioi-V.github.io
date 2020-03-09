---
title: Tallest Cow(线段树较易）
tags: 数据结构
category: 算法
date: 2018-07-11 20:50
---

<font size=5> 

# 题目描述

FJ's N (1 ≤ N ≤ 10,000) cows conveniently indexed 1..N are standing in a line. Each cow has a positive integer height (which is a bit of secret). You are told only the height H (1 ≤ H ≤ 1,000,000) of the tallest cow along with the index I of that cow.
FJ has made a list of R (0 ≤ R ≤ 10,000) lines of the form "cow 17 sees cow 34". This means that cow 34 is at least as tall as cow 17, and that every cow between 17 and 34 has a height that is strictly smaller than that of cow 17.
For each cow from 1..N, determine its maximum possible height, such that all of the information given is still correct. It is guaranteed that it is possible to satisfy all the constraints.

## 输入

- Line 1: Four space-separated integers: N, I, H and R 
- Lines 2..R+1: Two distinct space-separated integers A and B (1 ≤ A, B ≤ N), indicating that cow A can see cow B.

## 输出

Lines 1..N: Line i contains the maximum possible height of cow i.

## 样例输入



```
9 3 5 5
1 3
5 3
4 3
3 7
9 8
```

## 样例输出



```
5
4
5
3
4
4
5
5
5
```



## 思路

区间覆盖并且不会有相交的区间。

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const int M=10100;
int le[M],ri[M];
ll N,H,I;
ll s[M<<2],ans[M];
struct node
{
    int l,r;
    int sum;
} t[M<<2];
void pushdown(int rs)
{
    if(s[rs])
    {
        s[rs*2]+=s[rs];
        s[rs*2+1]+=s[rs];
        s[rs]=0;
    }
}
void build(int l,int r,int rs)
{
    t[rs].l=l;
    t[rs].r=r;
    s[rs]=0;
    if(l==r)
    {
        t[rs].sum=H;
        return ;
    }
    int mid=(l+r)/2;
    build(l,mid,rs*2);
    build(mid+1,r,rs*2+1);
}
void update(int l,int r,int rs)
{
    if(t[rs].l>=l&&t[rs].r<=r)
    {
        s[rs]+=1;
        return ;
    }
    pushdown(rs);
    int mid=(t[rs].l+t[rs].r)/2;
    if(l<=mid)
        update(l,r,rs*2);
    if(r>mid)
        update(l,r,rs*2+1);
}
int query(int k,int rs)
{
    if(t[rs].l==t[rs].r)
        return t[rs].sum-=s[rs];
    pushdown(rs);
    int mid=(t[rs].l+t[rs].r)/2;
    if(k<=mid)
        return query(k,rs*2);
    else
        return query(k,rs*2+1);
}
int main()
{
    std::ios::sync_with_stdio(false);
    std::cin.tie(0);
    int i,j,k=0,m,n;
    int x,y,T,flag;
    cin>>N>>I>>H>>T;
    build(1,N,1);
    while(T--)
    {
        cin>>x>>y;
        if(x>y)
            swap(x,y);
        flag=1;
        for(i=0; i<k; i++)
            if(x==le[i]&&y==ri[i])
            {
                flag=0;
                break;
            }
        if(flag==0)
            continue;
        le[k]=x,ri[k++]=y;
        if(y-x==1)
            continue;
        update(x+1,y-1,1);
    }
    for(i=1; i<=N; i++)
        ans[i]=query(i,1);
    for(i=1; i<=N; i++)
        cout<<ans[i]<<endl;
    return 0;
}
```

