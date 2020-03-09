---
title: 拓扑排序学习例题（Sorting It All Out）
tags: 图论
category: 算法
date: 2019-01-22 21:04
---

<font size=5> 

# 题目描述

An ascending sorted sequence of distinct values is one in which some form of a less-than operator is used to order the elements from smallest to largest. For example, the sorted sequence A, B, C, D implies that A < B, B < C and C < D. in this problem, we will give you a set of relations of the form A < B and ask you to determine whether a sorted order has been specified or not.

## 输入

Input consists of multiple problem instances. Each instance starts with a line containing two positive integers n and m. the first value indicated the number of objects to sort, where 2 <= n <= 26. The objects to be sorted will be the first n characters of the uppercase alphabet. The second value m indicates the number of relations of the form A < B which will be given in this problem instance. Next will be m lines, each containing one such relation consisting of three characters: an uppercase letter, the character "<" and a second uppercase letter. No letter will be outside the range of the first n letters of the alphabet. Values of n = m = 0 indicate end of input.

## 输出

For each problem instance, output consists of one line. This line should be one of the following three: 
Sorted sequence determined after xxx relations: yyy...y. 
Sorted sequence cannot be determined. 
Inconsistency found after xxx relations. 
where xxx is the number of relations processed at the time either a sorted sequence is determined or an inconsistency is found, whichever comes first, and yyy...y is the sorted, ascending sequence. 

## 样例输入



```
4 6
A<B
A<C
B<C
C<D
B<D
A<B
3 2
A<B
B<A
26 1
A<Z
0 0
```

## 样例输出



```
Sorted sequence determined after 4 relations: ABCD.
Inconsistency found after 2 relations.
Sorted sequence cannot be determined.
```



## 思路



## 代码

```c++
#include <bits/stdc++.h>

#define ll long long
#define ull unsigned long long
#define inf 0x3f3f3f3f
#define mp make_pair
#define met(a, x) memset(a,x,sizeof(a))

using namespace std;
const int mod = 1e9 + 7;
const int N = 50005;
vector<int> edge[500];
int in[30], tmp[30];
int m, n;
vector<int> ans;

int topo() {
    ans.clear();
    queue<int> q;
    for (int i = 0; i < m; i++) {
        if (in[i] == 0)
            q.push(i);
    }
    bool flag = false;
    while (!q.empty()) {
        if (q.size() > 1)
            flag = true;
        int p = q.front();
        q.pop();
        ans.push_back(p);
        for (int i = 0; i < edge[p].size(); i++) {
            int y = edge[p][i];
            in[y]--;
            if (in[y] == 0)
                q.push(y);
        }
    }
    if (ans.size() < m) {
        return -1;
    } else if (flag)
        return 0;
    else
        return 1;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    char str[5];
    while (cin >> m >> n && m && n) {
        for (int i = 0; i < 500; i++)
            edge[i].clear();
        met(in, 0);
        met(tmp, 0);
        int flag = 0;
        int num = 0;
        for (int i = 1; i <= n; i++) {
            cin >> str;
            if (flag != 0)
                continue;
            int a = str[0] - 'A';
            int b = str[2] - 'A';
            if (str[1] == '<') {
                in[b]++;
                edge[a].push_back(b);
            } else {
                in[a]++;
                edge[b].push_back(a);
            }
            memcpy(tmp, in, sizeof(in));
            flag = topo();
            memcpy(in, tmp, sizeof(tmp));
            if (flag != 0) {
                num = i;
            }
        }
        char c;
        if (flag == 1) {
            cout << "Sorted sequence determined after " << num << " relations: ";
            for (int i = 0; i < ans.size(); i++) {
                c = (ans[i] + 'A');
                cout << c;
            }
            cout <<'.'<< endl;
        } else if (flag == 0) {
            cout << "Sorted sequence cannot be determined." << endl;
        } else {
            cout << "Inconsistency found after " << num << " relations." << endl;
        }
    }
    return 0;
}
```

