---
title: RGB Coloring(组合数模板）
tags: 数论
category: 算法
date: 2019-03-27 16:52
---

<font size=5> 

# 题目描述

Takahashi has a tower which is divided into N layers. Initially, all the layers are uncolored. Takahashi is going to paint some of the layers in red, green or blue to make a beautiful tower. He defines the beauty of the tower as follows:
The beauty of the tower is the sum of the scores of the N layers, where the score of a layer is A if the layer is painted red, A+B if the layer is painted green, B if the layer is painted blue, and 0 if the layer is uncolored.
Here, A and B are positive integer constants given beforehand. Also note that a layer may not be painted in two or more colors.
Takahashi is planning to paint the tower so that the beauty of the tower becomes exactly K. How many such ways are there to paint the tower? Find the count modulo 998244353. Two ways to paint the tower are considered different when there exists a layer that is painted in different colors, or a layer that is painted in some color in one of the ways and not in the other.

Constraints
1≤N≤3×1e5
1≤A,B≤3×1e5
0≤K≤18×1e10
All values in the input are integers.

## 输入

Input is given from Standard Input in the following format:
N A B K

## 输出

Print the number of the ways to paint tiles, modulo 998244353.

## 样例输入



```
4 1 2 5
```

## 样例输出



```
40
```

In this case, a red layer worth 1 points, a green layer worth 3 points and the blue layer worth 2 points. The beauty of the tower is 5 when we have one of the following sets of painted layers:
1 green, 1 blue
1 red, 2 blues
2 reds, 1 green
3 reds, 1 blue
The total number of the ways to produce them is 40.

## 思路

绿色等于红色和蓝色分数和，因此可以看做是蓝色和红色涂在同一层，因此计算红色和蓝色涂色方法的时候不需要考虑这一层涂没涂，只需分别计算即可，即A * a + B * b = k。对每一个a,b，ans += C(n,a) * C(n,b)。

## 代码

```c++
#include<bits/stdc++.h>

#define ll long long
#define met(a, x) memset(a,x,sizeof(a))
#define inf 0x3f3f3f3f
#define ull unsigned long long
#define mp make_pair

using namespace std;
const int mod = 998244353;
const int N = 3e5 + 10;
const int M = 1e6 + 10;
ll fac[N], inv[N];

ll ksm(ll a, ll b) {
    ll ans = 1;
    while (b) {
        if (b % 2)
            ans = ans * a % mod;
        a = a * a % mod;
        b >>= 1;
    }
    return ans;
}
void init() {
    int i;
    fac[0] = 1;
    for (i = 1; i < N; i++) {
        fac[i] = (fac[i - 1] * i) % mod;
    }
    inv[N - 1] = ksm(fac[N - 1], mod - 2);
    for (i = N - 2; i >= 0; i--) {
        inv[i] = inv[i + 1] * (i + 1) % mod;
    }
}
long long C(ll n,ll m) {
    if (n < m) return 0;
    return fac[n] * inv[m] % mod * inv[n - m] % mod;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    ll n, a, b, k;
    cin >> n >> a >> b >> k;
    ll ans = 0;
    init();
    for (ll i = 0; i <= n; i++) {
        if (a * i > k) {
            break;
        }
        if ((k - a * i) % b) {
            continue;
        }
        ll cnt = (k - a * i) / b;
        if (cnt > n) {
            continue;
        }
        ans = (ans + C(n, i) % mod * C(n, cnt) % mod + mod) % mod;
    }
    cout << ans << endl;
    return 0;
}
```

