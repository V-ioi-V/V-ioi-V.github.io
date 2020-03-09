---
title: SG函数
tags: 博弈论
category: 算法
date: 2019-04-01 11:42
---



SG(x)=mex({SG(y1),SG(y2),...SG(yk))})

<!--more-->

<font size=5> 

## 代码

```c++
int f[N], SG[M], mex[M];
//f[N]:可改变当前状态的方式，N为方式的种类，f[N]要在getSG之前先预处理
//SG[]:0~n的SG函数值
//mex[]:为x后继状态的集合
void getSG(int n) {
    int i, j;
    memset(SG, 0, sizeof(SG));
    //因为SG[0]始终等于0，所以i从1开始
    for (i = 1; i <= n; i++) {
        //每一次都要将上一状态 的 后继集合 重置
        memset(mex, 0, sizeof(mex));
        for (j = 0; f[j] <= i && j <= N; j++)
            mex[SG[i - f[j]]] = 1;  //将后继状态的SG函数值进行标记
        for (j = 0;; j++)
            if (!mex[j]) {   //查询当前后继状态SG值中最小的非零值
                SG[i] = j;
                break;
            }
    }
}
```

