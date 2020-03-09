---
title: Ultra-QuickSort(裸树状数组求逆序数)
tags: 数据结构
category: 算法
date:  2018-07-25 16:20 
---

<font size=5> 

# 题目描述

In this problem, you have to analyze a particular sorting algorithm. The algorithm processes a sequence of n distinct integers by swapping two adjacent sequence elements until the sequence is sorted in ascending order. For the input sequence 
9 1 0 5 4 ,
Ultra-QuickSort produces the output 
0 1 4 5 9 .
Your task is to determine how many swap operations Ultra-QuickSort needs to perform in order to sort a given input sequence.

## 输入

The input contains several test cases. Every test case begins with a line that contains a single integer n < 500,000 -- the length of the input sequence. Each of the the following n lines contains a single integer 0 ≤ a[i] ≤ 999,999,999, the i-th input sequence element. Input is terminated by a sequence of length n = 0. This sequence must not be processed.

## 输出

For every input sequence, your program prints a single line containing an integer number op, the minimum number of swap operations necessary to sort the given input sequence.

## 样例输入



```
5
9
1
0
5
4
3
1
2
3
0
```

## 样例输出



```
6
0
```

## 代码

```c++
#include <bits/stdc++.h>

#define ll long long
#define ull unsigned long long
#define met(a, x) memset(a,x,sizeof(a))
#define inf 0x3f3f3f3f
#define mp make_pair;

using namespace std;
const int mod = 1e9 + 7;
const int N = 1e6 + 10;
const int M = 1e5 + 10;
int n,C[2*N];
struct node{
    int x,y;
}s[2*N];
int lowbit(int x){
    return x&(-x);
}
void add(int x,int y){
    for(;x<=n;x+=lowbit(x))
        C[x]+=y;
}
int getsum(int x){
    int ans=0;
    for(;x;x-=lowbit(x)){
        ans+=C[x];
    }
    return ans;
}
bool cmp(node a,node b){
    if(a.x==b.x)
        return a.y>b.y;
    return a.x>b.x;
}
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    cin>>n;
    for(int i=1;i<=n;i++){
        cin>>s[i].x;
        s[i].y=i;
    }
    sort(s+1,s+1+n,cmp);
    ll ans=0;
    for(int i=1;i<=n;i++){
        add(s[i].y,1);
        ans+=getsum(s[i].y-1);
    }
    cout<<ans<<endl;
    return 0;
}
```

