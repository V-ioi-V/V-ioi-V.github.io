---
title: Game of Stones
tags: 博弈论
category: 算法
date: 2019-04-27 22:01
---

<font size=5> 

# 题目描述

Two players, Petyr and Varys, play a game in which players remove stones from N piles in alternating turns. Petyr, in his turn, can remove at most A stones from any pile. Varys, in his turn, can remove at most B stones from any pile. Each player has to remove at least one stone in his turn. The player who removes the last stone wins.
The game has already started and it is Petyr’s turn now. Your task is to determine whether he can win the game if both he and Varys play the game in the best possible way.

## 输入

The ﬁrst input line contains three integers N, A, and B (1 ≤ N ≤ 105 and 1 ≤ A, B ≤ 105 ). N describes the number of piles and A, B represent Petyr’s and Varys’ restrictions. The second line contains N integers X1 , . . . , XN (1 ≤ Xi ≤ 106 ) specifying the current number of stones in all piles.

## 输出

Output the name of the winner.

## 样例输入



```
2 3 4
2 3
```

## 样例输出



```
Petyr
```



## 思路

n堆石子，第一个人可以取a个，第二个人可以取b个，谁先取完谁就获胜，求最后谁获胜了。

## 代码

```c++
#include <bits/stdc++.h>

#define ll long long
#define inf 0x3f3f3f3f
#define met(a, x) memset(a,x,sizeof(a))
#define ull unsigned long long
#define mp make_pair

using namespace std;
const int N = 1e5 + 10;
int n, a, b, X, A[N], cnt = -1;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin >> n >> a >> b;
    for (int i = 0; i < n; ++i) {
        cin>>A[i];
        if (A[i] <= min(a, b))// 如果石子数小于最小值的话就相当于NIM博弈；
            X ^= A[i];
        else if (a == b)// 如果两个人可以取的石子数相同的话就相当于两个人都先取一堆的，相当于巴什博奕
            X ^= A[i] % (a + 1);
        else if (cnt == -1)// 如果只有一堆大于两人可以取得最大值，就记录一下
            cnt = i;
        else {// 如果有两堆以上的石子大于可以取得最小值，那就是可以取得石子数更多的人获胜，
              // 可取数大的人总有方法使两人的位置转移，必胜。
            if (a < b)
                cout<<"Varys"<<endl;
            else
                cout<<"Petyr"<<endl;
            return 0;
        }
    }
    if (cnt != -1) {//如果只有一堆大于最小可取石子的话取决于自己先手的话还有几个石子
                    //并且要假设这是第一堆要取的石子
        if (a > b) {
            cout<<"Petyr"<<endl;
            return 0;
        }
        for (int i = 1; i <= a; ++i) {
            if (A[cnt] - i <= a && (X ^ (A[cnt] - i)) == 0) {
                cout<<"Petyr"<<endl;
                return 0;
            }
        }
        cout<<"Varys"<<endl;
        return 0;
    }
    if (X)
        cout<<"Petyr"<<endl;
    else
        cout<<"Varys"<<endl;
    return 0;
}
```

