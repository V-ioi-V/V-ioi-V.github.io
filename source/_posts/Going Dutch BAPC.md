---
title: Going Dutch BAPC（ 状态转移DP）
tags: DP
category: 算法 
date: 2018-04-09
---

<font size=5> 

# 题目描述

​     You and your friends have just returned from a beautiful vacation in the mountains of the Netherlands. When on vacation, it’s annoying to split the bill on every expense every time, so you just kept all the receipts from the vacation, and wrote down who paid how much for who. Now, it is time to settle the bill.

​    You could each take all the receipts showing that someone paid something for you, and then pay that person back. But then you would need a lot of transactions, and you want to keep up the lazy spirit from your trip. In the end, it does not matter who transfers money to whom; as long as in the end, everyone’s balance is 0.

​    Can you figure out the least number of transactions needed to settle the score? Assume everyone has enough spare cash to transfer an arbitrary amount of money to another person.

## 输入

 Input consists of

• A line containing two integers M, the number of people in the group, with 1 ≤ M ≤ 20,and N, the number of receipts from the trip, with 0 ≤ N ≤ 1000.

• N lines, each with three integers a, b, p, where 0 ≤ a, b < M, and 1 ≤ p ≤ 1000,signifying a receipt showing that person a paid p euros for person b.

## 输出

Output a single line containing a single integer, the least number of transactions necessary to settle all bills.

## 样例输入



```
4 2
0 1 1
2 3 1
```

## 样例输出



```
2
```

### 代码

```c++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const int maxn=1<<20;
int sum[20],a[maxn],b[maxn];
int main()
{
    int i,j,k,m,n;
    cin>>m>>n;
    memset(a,0,sizeof(a));
    memset(b,0,sizeof(b));
    memset(sum,0,sizeof(sum));
    while(n--)
    {
        cin>>i>>j>>k;
        sum[i]+=k;
        sum[j]-=k;
    }
    for(i=0; i<(1<<m); i++)
        for(j=0; j<m; j++)
            if(i&(1<<j))
                b[i]+=sum[j];
    for(i=0; i<(1<<m); i++)
    {
        for(j=0; j<m; j++)
            if(!(i&(1<<j)))
            {
                if(!b[i|(1<<j)])
                    a[i|(1<<j)]=max(a[i|(1<<j)],a[i]+1);
                else
                    a[i|(1<<j)]=max(a[i|(1<<j)],a[i]);
            }
    }
    cout<<m-a[(1<<m)-1]<<endl;
    return 0;
}
```

