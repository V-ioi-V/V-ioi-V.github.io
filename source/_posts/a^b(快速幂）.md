---
title: a^b(快速幂)
tags: 数论
category: 算法
date:  2018-07-04 10:29
---

<font size=5> 

# 题目描述

求 a 的 b 次方对 p 取模的值，其中 1≤a,b,p≤1e9

## 输入

三个用空格隔开的整数a,b和p。

## 输出

一个整数，表示a^b mod p的值。

## 样例输入



```
2 3 9
```

## 样例输出



```
8
```



## 代码

```c++
ll ksm(ll a, ll b, ll p) {
    ll ans = 1;
    while (b) {
        if (b % 2) {
            ans *= a;
            ans %= p;
        }
        a *= a;
        a %= p;
        b >>= 1;
    }
    return ans;
}
```

