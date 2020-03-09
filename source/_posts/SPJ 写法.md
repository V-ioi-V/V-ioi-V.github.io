---
title: SPJ 写法
tags: 常识
category: 算法
date: 2019-12-10 10:12
---

- SPJ

<!--more-->

<font size=5> 



## 代码

```c++
#pragma GCC optimize(2)
#pragma GCC optimize(3, "Ofast", "inline")

#include <bits/stdc++.h>

#define ll long long
#define met(a, x) memset(a,x,sizeof(a))
using namespace std;
const int N = 3e5 + 10;
int a[N], b[N], c[N];
map<int, int> mp;
//char argc[10][10100];
int main(int argc, char *args[]) {
    ifstream user, in, out;
    in.open(args[1]);//测试输入
    out.open(args[2]);//测试输出
    user.open(args[3]);//用户输出
    int n;
    in >> n;
    for (int i = 1; i <= n; i++) {
        in >> a[i + n];
        a[i] = a[i + n];
        a[i + 2 * n] = a[i + n];
    }
    for (int i = 1; i <= n; i++) {
        in >> b[i + n];
        b[i] = b[i + n];
        b[i + 2 * n] = b[i + n];
    }
    int ans;
    out >> ans;
    if (ans == -1) {
        int x;
        user>>x;
        if(x!=ans)return 1;
    } else {
        for (int i = 1; i <= n; i++) {
            user >> c[i];
            if (mp[c[i]])return 1;
            mp[c[i]]++;
        }
        int numa = 0, numb = 0;
        for (int i = 1; i <= n; i++) {
            if (a[i] == c[1])numa = i + n;
            if (b[i] == c[1])numb = i + n;
        }
        int l = numa - 1, r = numa + 1;
        for (int i = 2; i <= n; i++) {
            if (c[i] == a[l])l--;
            else if (c[i] == a[r])r++;
            else return 1;
        }
        l = numb - 1, r = numb + 1;
        for (int i = 2; i <= n; i++) {
            if (c[i] == b[l])l--;
            else if (c[i] == b[r])r++;
            else return 1;
        }
    }
    return 0;
}
```

