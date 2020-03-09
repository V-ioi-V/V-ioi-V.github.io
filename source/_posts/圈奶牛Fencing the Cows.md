---
title: 圈奶牛Fencing the Cows
tags: [计算几何,模板]
category: 算法
date: 2019-10-02 21:29
---

<font size=5> 

# 题目描述

农夫约翰想要建造一个围栏用来围住他的奶牛，可是他资金匮乏。他建造的围栏必须包括他的奶牛喜欢吃草的所有地点。对于给出的这些地点的坐标，计算最短的能够围住这些点的围栏的长度。

## 输入

输入数据的第一行包括一个整数 N。N（0 <= N <= 10,000）表示农夫约翰想要围住的放牧点的数目。接下来 N 行，每行由两个实数组成，Xi 和 Yi,对应平面上的放牧点坐标（-1,000,000 <= Xi,Yi <= 1,000,000）。数字用小数表示。

## 输出

输出必须包括一个实数，表示必须的围栏的长度。答案保留两位小数。

## 样例输入



```
4
4 8
4 12
5 9.3
7 8
```

## 样例输出



```
12.00
```



## 代码

```c++
#pragma GCC optimize(2)
#pragma GCC optimize(3, "Ofast", "inline")

#include<bits/stdc++.h>//二维凸包

#define ll long long
#define met(a, x) memset(a,x,sizeof(a))
#define inf 0x3f3f3f3f
#define ull unsigned long long
using namespace std;
const double pi = acos(-1.0);
const int N = 1e5 + 10;
const int M = 1e6 + 10;
struct node {
    double x, y;
} s[N];
int n, top = 2, st[N];
double ans;

inline double power(double x) {
    return x * x;
}

inline double dis(int a, int b) {
    return sqrt(power(s[a].x - s[b].x) + power(s[a].y - s[b].y));
}

inline bool judge(int a, int b, int c) {
    return (s[a].y - s[b].y) * (s[b].x - s[c].x) > (s[a].x - s[b].x) * (s[b].y - s[c].y);//算斜率，乘在一起避免掉精。
}

inline bool cmp(node a, node b) {
    return (a.y == b.y) ? (a.x < b.x) : (a.y < b.y);
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> s[i].x >> s[i].y;
    }
    sort(s + 1, s + n + 1, cmp);
    st[1] = 1, st[2] = 2;
    for (int i = 3; i <= n; i++) {
        while (top > 1 && !judge(i, st[top], st[top - 1]))
            top--;
        st[++top] = i;
    }
    for (int i = 1; i <= top - 1; i++)
        ans += dis(st[i], st[i + 1]);
    top = 2;
    met(st, 0);
    st[1] = 1, st[2] = 2;
    for (int i = 3; i <= n; i++) {
        while (top > 1 && judge(i, st[top], st[top - 1]))
            top--;
        st[++top] = i;
    }
    for (int i = 1; i <= top - 1; i++)
        ans += dis(st[i], st[i + 1]);
    cout << fixed << setprecision(2) << ans << endl;
    return 0;
}
```

