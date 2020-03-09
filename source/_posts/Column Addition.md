---
title: Column Addition
tags: DP
category: 算法
date: 2018-04-21 16:16
---

<font size=5> 

# 题目描述

A multi-digit column addition is a formula on adding two integers written like this:

A multi-digit column addition is written on the blackboard, but the sum is not necessarily correct. We can erase any number of the columns so that the addition becomes correct. For example, in the following addition, we can obtain a correct addition by erasing the second and the forth columns.

Your task is to find the minimum number of columns needed to be erased such that the remaining formula becomes a correct addition.

## 输入

There are multiple test cases in the input. Each test case starts with a line containing the single integer n, the number of digit columns in the addition (1 ⩽ n ⩽ 1000). Each of the next 3 lines contain a string of n digits. The number on the third line is presenting the (not necessarily correct) sum of the numbers in the first and the second line. The input terminates with a line containing “0” which should not be processed.

## 输出

For each test case, print a single line containing the minimum number of columns needed to be erased.

## 样例输入



```
3
123
456
579
5
12127
45618
51825
2
24
32
32
5
12299
12299
25598
0
```

## 样例输出



```
0
2
2
1
```



## 思路

​    就是用DP，开一个二维的数组，dp[i][1],表示这一位取到，dp[i][0]则表示这一位取不到，然后一位一位的判断，如果上下两个数的和模10等于下面的那个数就去前一位的dp[i][0]+1，因为如果前一位取到那这一位就不成立了，但是如果上下两个数的和加一再模10,并且前一位存在进位，那么就取前一位的dp[i][1]+1，然后结尾去一个DP判断一下最大值，因为最后一位不存在进位的情况，所以dp[1][0]即为最大能取到的位数，然后用总位数n减去即可。

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
char a[1500],b[1500],c[1500];
int aa[1500],bb[1500],cc[1500];
int d[1500][2];
int main()
{
    int i,j,k,m,n;
    while(cin>>n&&n)
    {
        memset(d,0,sizeof(d));
        cin>>a+1>>b+1>>c+1;
        for(i=1; i<=n; i++)
        {
            aa[i]=a[i]-'0';
            bb[i]=b[i]-'0';
            cc[i]=c[i]-'0';
        }
        for(i=n; i>=1; i--)
        {
            if((aa[i]+bb[i])%10==cc[i])
            {
                if(aa[i]+bb[i]>=10)
                    d[i][1]=d[i+1][0]+1;
                else
                    d[i][0]=d[i+1][0]+1;
            }
            if(d[i+1][1]&&(aa[i]+bb[i]+1)%10==cc[i])
            {
                if(aa[i]+bb[i]+1>=10)
                    d[i][1]=d[i+1][1]+1;
                else
                    d[i][0]=d[i+1][1]+1;
            }
            d[i][1]=max(d[i][1],d[i+1][1]);
            d[i][0]=max(d[i][0],d[i+1][0]);
        }
        cout<<n-d[1][0]<<endl;
    }
    return 0;
}
```

