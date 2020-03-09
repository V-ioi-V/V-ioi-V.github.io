---
title: Beauty Contest（求凸包最大直径） 
tags: 计算几何
category: 算法
date: 2019-10-29 18:16
---

<font size=5> 

# 题目描述

贝茜在牛的选美比赛中赢得了冠军”牛世界小姐”。因此,贝西会参观N(2 < = N < = 50000)个农场来传播善意。世界将被表示成一个二维平面,每个农场位于一对整数坐标(x,y),各有一个值范围在-10000…10000。没有两个农场共享相同的一对坐标。

尽管贝西沿直线前往下一个农场，但牧场之间的距离可能很大,所以她需要一个手提箱保证在每一段旅程中她有足够吃的食物。她想确定她可能需要旅行的最大可能距离,她要知道她必须带的手提箱的大小。帮助贝西计算农场的最大距离。

## 输入

第一行：一个整数n

第2~n+1行：xi yi 表示n个农场中第i个的坐标

## 输出

一行：最远距离的[b]平方[/b]

## 样例输入



```
4
0 0
0 1
1 1
1 0
```

## 样例输出



```
2
```



## 代码

```c++
#include<bits/stdc++.h>//求凸包最大直径

using namespace std;
const int MAXN = 50010;
const int INF = 0x7fffffff;

struct Point {
    double x, y;

    Point(double x = 0, double y = 0) : x(x), y(y) {}
};

typedef Point Vector;
Point in[MAXN], out[MAXN];

Vector operator-(Vector A, Vector B) {
    return Vector(A.x - B.x, A.y - B.y);
}

double cross(Vector A, Vector B) {
    return A.x * B.y - A.y * B.x;
}

bool operator<(const Point &a, const Point &b) {
    return a.x < b.x || (a.x == b.x && a.y < b.y);
}

int convexHull(Point *p, int n, Point *ch) {
    sort(p, p + n);
    int m = 0;
    for (int i = 0; i < n; i++) {
        while (m > 1 && cross(ch[m - 1] - ch[m - 2], p[i] - ch[m - 2]) <= 0) m--;
        ch[m++] = p[i];
    }
    int k = m;
    for (int i = n - 2; i >= 0; i--) {
        while (m > k && cross(ch[m - 1] - ch[m - 2], p[i] - ch[m - 2]) <= 0) m--;
        ch[m++] = p[i];
    }
    if (n > 1) m--;
    return m;
}

double length(Vector A) {
    return A.x * A.x + A.y * A.y;
}

double rotateCalipers(Point *ch, int n) {
    double ans = -INF;
    ch[n] = ch[0];
    int q = 1;
    for (int i = 0; i < n; i++)//枚举边(i,i+1)
    {
        //寻找面积最大的三角形
        while (cross(ch[q] - ch[i + 1], ch[i] - ch[i + 1]) < cross(ch[q + 1] - ch[i + 1], ch[i] - ch[i + 1])) {
            q = (q + 1) % n;
        }
        ans = max(ans, max(length(ch[q] - ch[i]), length(ch[q + 1] - ch[i + 1])));
    }
    return ans;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n, ans;
    while (cin >> n) {
        for (int i = 0; i < n; i++) {
            cin >> in[i].x >> in[i].y;
        }
        ans = convexHull(in, n, out);
        cout << fixed << setprecision(0) << rotateCalipers(out, ans) << endl;
    }
    return 0;
}
```

