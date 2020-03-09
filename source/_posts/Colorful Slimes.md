---
title: Colorful Slimes
tags: 瞎搞
category: 算法
date: 2018-04-10
---

<font size=5> 

# 题目描述

Snuke lives in another world, where slimes are real creatures and kept by some people. Slimes come in N colors. Those colors are conveniently numbered 1 through N. Snuke currently has no slime. His objective is to have slimes of all the colors together.

Snuke can perform the following two actions:

Select a color i (1≤i≤N), such that he does not currently have a slime in color i, and catch a slime in color i. This action takes him ai seconds.

Cast a spell, which changes the color of all the slimes that he currently has. The color of a slime in color i (1≤i≤N−1) will become color i+1, and the color of a slime in color N will become color 1. This action takes him x seconds.

Find the minimum time that Snuke needs to have slimes in all N colors.



Constraints
2≤N≤2,000
ai are integers.
1≤ai≤1e9
x is an integer.
1≤x≤1e9

# 输入

The input is given from Standard Input in the following format:

N x
a1 a2 … aN

# 输出

Find the minimum time that Snuke needs to have slimes in all N colors.

# 样例输入



```
2 10
1 100
```

# 样例输出



```
12
```



# 提示

Snuke can act as follows:

Catch a slime in color 1. This takes 1 second.
Cast the spell. The color of the slime changes: 1 → 2. This takes 10 seconds.
Catch a slime in color 1. This takes 1 second.

# 思路

时间最小的当然就是移动一次啦，直接上暴力，然后判断当前这个还是经过i次移动移过来的小，一个一个统计比较大小即可。



# 代码

```c++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
ll b[2010],a[2010];
int main()
{
    ll i,j,k,m,n,x;
    ll cnt,ans=1ll * 2010 * 1e9;
    memset(a,0x3f,sizeof(a));
    cin>>n>>x;
    for(i=0; i<n; i++)
        cin>>b[i];
    for(i=0; i<n; i++)
    {
        cnt=0;
        for(j=0; j<n; j++)
            a[j]=min(a[j],b[(j-i+n)%n]);
        for(j=0; j<n; j++)
            cnt+=a[j];
        ans=min(ans,i*x+cnt);
    }
    cout<<ans<<endl;
    return 0;
}
```

