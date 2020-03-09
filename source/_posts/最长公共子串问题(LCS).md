---
title: 最长公共子串问题(LCS)
tags: DP
category: 算法
date: 2018-08-04 10:29
---

<font size=5> 

# 题目描述

希腊有一个著名的历史学家，他为了专心写一本史学巨著，而把自己关在一个高塔之上。有一天，他目睹了一场凶杀案的发生，但事后，他惊讶地发现所有围观人的证词都不一样，甚至有的完全相反。于是，他放弃了继续写作的念头。他说：“人们连发生在眼前的事实都分辨不清，我又怎么知道我写的历史是真是假呢？”

虽然历史的真假我们很难辨别，但文章是否抄袭，我们还是可以用所谓的LCS算法解决的。

给出两个字符串S1和S2，我们现在希望了解两个字符串之间的“相似度”。这个相似度是这样定义的：找出第三个字符串S3，组成S3的元素也出现在S1和S2中，而且这些元素必须是以相同的顺序出现，但不必要是连续的。能找到的S3越长则S1和S2就越相似。相似度即是这个S3的长度。例如，当S1="abcde"，S2="dabfda"，S3就是"abd"， S1和S2的相似度就是3。

## 输入

第一行为一个整数，表示第一个字符串的长度。
第二行为第一个字符串。
第三行为一个整数，表示第二个字符串的长度。
第四行为第二个字符串。

## 输出

一个整数，即两个字符串的相似度。

## 样例输入



```
5
abccb
5
acabc
```

## 样例输出



```
3
```



## 代码

```c++
#include<bits/stdc++.h>
#define ll long long
#define met(a) memset(a,0,sizeof(a))
#define inf 0x3f3f3f3f
using namespace std;
const int mod=1e9+7;
const int N=1010;
char a[3000],b[3000];
int dp[3000][3000];
int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    met(dp);
    int i,j,k,m,n,s;
    cin>>m>>a+1>>n>>b+1;
    for(i=1; i<=m; i++)
        for(j=1; j<=n; j++)
        {
            if(a[i]==b[j])
                dp[i][j]=dp[i-1][j-1]+1;
            else
                dp[i][j]=max(dp[i][j-1],dp[i-1][j]);
        }
    cout<<dp[m][n]<<endl;
    return 0;
}
```

