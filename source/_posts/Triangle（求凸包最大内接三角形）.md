---
title: Triangle（求凸包最大内接三角形）
tags: 计算几何
category: 算法
date: 2019-10-28 20:42
---

<font size=5> 

# 题目描述

Given n distinct points on a plane, your task is to find the triangle that have the maximum area, whose vertices are from the given points.

## 输入

The input consists of several test cases. The first line of each test case contains an integer n, indicating the number of points on the plane. Each of the following n lines contains two integer xi and yi, indicating the ith points. The last line of the input is an integer −1, indicating the end of input, which should not be processed. You may assume that 1 <= n <= 50000 and −104 <= xi, yi <= 104 for all i = 1 . . . n.

## 输出

For each test case, print a line containing the maximum area, which contains two digits after the decimal point. You may assume that there is always an answer which is greater than zero.

## 样例输入



```
3
3 4
2 6
2 7
5
2 6
3 9
2 0
8 0
6 5
-1
```

## 样例输出



```
0.50
27.00
```



## 思路



## 代码

```c++
#include<bits/stdc++.h>//（求凸包最大内接三角形）O(n^2)

#define ll long long
#define N 50003
using namespace std;
const double pi = acos(-1.0);

struct node {
    double x, y;

    node(double X = 0, double Y = 0) {
        x = X, y = Y;
    }
} a[N], ch[N];

node operator-(node a, node b) {
    return node(a.x - b.x, a.y - b.y);
}

node operator+(node a, node b) {
    return node(a.x + b.x, a.y + b.y);
}

node operator*(node a, double t) {
    return node(a.x * t, a.y * t);
}

bool operator<(node a, node b) {
    return a.x < b.x || a.x == b.x && a.y < b.y;
}

int n, m;

double cross(node a, node b) {
    return a.x * b.y - a.y * b.x;
}

void convexhull() {
    sort(a + 1, a + n + 1);
    m = 0;
    if (n == 1) {
        ch[++m] = a[1];
        return;
    }
    for (int i = 1; i <= n; i++) {
        while (m > 1 && cross(ch[m - 1] - ch[m - 2], a[i] - ch[m - 2]) <= 0) m--;
        ch[m++] = a[i];
    }
    int k = m;
    for (int i = n - 1; i >= 1; i--) {
        while (m > k && cross(ch[m - 1] - ch[m - 2], a[i] - ch[m - 2]) <= 0) m--;
        ch[m++] = a[i];
    }
    m--;
}

double rotating() {
    if (m <= 2) return 0;
    if (m == 3) return fabs(cross(ch[1] - ch[0], ch[2] - ch[0])) / 2;
    double ans = 0;
    for (int i = 0; i < m; i++) {
        int j = (i + 1) % m;
        int k = (j + 1) % m;
        while (fabs(cross(ch[i] - ch[j], ch[i] - ch[k])) < fabs(cross(ch[i] - ch[j], ch[i] - ch[(k + 1) % m])))
            k = (k + 1) % m;
        while (i != j && k != i) {
            ans = max(ans, fabs(cross(ch[i] - ch[j], ch[i] - ch[k])));
            while (fabs(cross(ch[i] - ch[j], ch[i] - ch[k])) < fabs(cross(ch[i] - ch[j], ch[i] - ch[(k + 1) % m])))
                k = (k + 1) % m;
            j = (j + 1) % m;
        }
    }
    return ans / 2.0;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    while (cin >> n && n != -1) {
        for (int i = 1; i <= n; i++)cin >> a[i].x >> a[i].y;
        convexhull();
        double ans = rotating();
        cout << fixed << setprecision(2) << ans << endl;
    }
    return 0;
}
```

