---
title: Smallest Bounding Rectangle(最小面积外接矩形)
tags: 计算几何
category: 算法
date: 2019-10-28 21:03
---

<font size=5> 

# 题目描述

Given the Cartesian coordinates of *n* (> 0) 2-dimensional points, write a program that computes the area of their smallest bounding rectangle (smallest rectangle containing all the given points).

## 输入

The input file may contain multiple test cases. Each test case begins with a line containing a positive integer *n* (< 1001) indicating the number of points in this test case. Then follows *n* lines each containing two real numbers giving respectively the *x-* and *y-*coordinates of a point. The input terminates with a test case containing a value 0 for *n* which must not be processed*.* 

## 输出

For each test case in the input print a line containing the area of the smallest bounding rectangle rounded to the 4th digit after the decimal point.

## 样例输入



```
3
-3.000 5.000
7.000 9.000
17.000 5.000
4
10.000 10.000
10.000 20.000
20.000 20.000
20.000 10.000
0 
```

## 样例输出



```
80.0000
100.0000
```



## 代码

```c++
#include<bits/stdc++.h>//最小面积外接矩形

#define ll long long
const int N = 50007;
using namespace std;
int n, top;
double ans;
#define eps 1e-8

int dcmp(double x) { return fabs(x) < eps ? 0 : (x > 0 ? 1 : -1); }

struct pt {
    double x, y;

    pt() {}

    pt(double x, double y) : x(x), y(y) {}

    friend bool operator<(const pt &A, const pt &B) {
        return A.x < B.x || (A.x == B.x && A.y < B.y);
    }
} p[N], ham[N];

pt operator-(const pt &A, const pt &B) { return pt(A.x - B.x, A.y - B.y); }

double dot(const pt &A, const pt &B) { return A.x * B.x + A.y * B.y; }

double cross(const pt &A, const pt &B) { return A.x * B.y - A.y * B.x; }

double lenth(const pt &A) { return sqrt(dot(A, A)); }

double node_to_line(pt C, pt A, pt B) {
    return fabs(cross(C - A, B - A)) / lenth(A - B);
}

bool cmp(const pt &A, const pt &B) {
    return dcmp(cross(A - p[1], B - p[1])) < 0 ||
           (dcmp(cross(A - p[1], B - p[1])) == 0 && dcmp(lenth(A - p[1]) - lenth(B - p[1])) < 0);
}

void get_ham(int n) {
    for (int i = 2; i <= n; i++)
        if (p[i] < p[1]) swap(p[i], p[1]);
    sort(p + 2, p + n + 1, cmp);
    top = 0;
    ham[top++] = p[1];
    for (int i = 2; i <= n; i++) {
        while (top >= 2 && dcmp(cross(p[i] - ham[top - 2], ham[top - 1] - ham[top - 2])) <= 0) top--;
        ham[top++] = p[i];
    }
}

void RC(int top) {
    ham[top] = ham[0];
    int j = 1, k = 1, l = 1;
    for (int i = 0; i < top; i++) {
        while (dcmp(cross(ham[j % top] - ham[i], ham[i + 1] - ham[i]) -
                    cross(ham[(j + 1) % top] - ham[i], ham[i + 1] - ham[i])) < 0)
            j++;
        k = max(k, i + 1);
        l = max(l, j);
        while (dcmp(dot(ham[k % top] - ham[i + 1], ham[i] - ham[i + 1]) -
                    dot(ham[(k + 1) % top] - ham[i + 1], ham[i] - ham[i + 1])) > 0)
            k++;
        while (dcmp(dot(ham[l % top] - ham[i], ham[i + 1] - ham[i]) -
                    dot(ham[(l + 1) % top] - ham[i], ham[i + 1] - ham[i])) > 0)
            l++;
        double d = lenth(ham[i + 1] - ham[i]);
        double L = fabs(dot(ham[k % top] - ham[i + 1], ham[i] - ham[i + 1])) / d +
                   fabs(dot(ham[l % top] - ham[i], ham[i + 1] - ham[i])) / d + d;
        double D = node_to_line(ham[j % top], ham[i], ham[i + 1]);
        ans = min(ans, L * D);
    }
    if (top < 3) ans = 0;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    while (cin >> n && n) {
        for (int i = 1; i <= n; i++)cin >> p[i].x >> p[i].y;
        get_ham(n);
        ans = 1e9;
        RC(top);
        cout << fixed << setprecision(4) << ans << endl;
    }
    return 0;
}
```

