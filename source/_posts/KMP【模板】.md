---
title: KMP【模板】
tags: [字符串,模板]
category: 算法
date: 2018-08-01 18:51 
---

模板

<!--more-->

<font size=5> 

## 代码

```c++
/*
pku3461(Oulipo), hdu1711(Number Sequence)
这个模板 字符串是从0开始的
Next数组是从1开始的
*/
#include <bits/stdc++.h>
#define ll long long
#define inf 0x3f3f3f3f
#define met(a) memset(a,0,sizeof(a))
using namespace std;
const int mod=1e9+7;
const int N = 1000002;
int Next[N];
char S[N], T[N];
int slen, tlen;
void getNext()
{
    int j, k;
    j = 0; k = -1; Next[0] = -1;
    while(j < tlen)
        if(k == -1 || T[j] == T[k])
            Next[++j] = ++k;
        else
            k = Next[k];

}
/*
返回模式串T在主串S中首次出现的位置
返回的位置是从0开始的。
*/
int KMP_Index()
{
    int i = 0, j = 0;
    getNext();

    while(i < slen && j < tlen)
    {
        if(j == -1 || S[i] == T[j])
            i++,j++;
        else
            j = Next[j];
    }
    if(j == tlen)
        return i - tlen;
    else
        return -1;
}
/*
返回模式串在主串S中出现的次数
*/
int KMP_Count()
{
    int ans = 0;
    int i, j = 0;
    if(slen == 1 && tlen == 1)
    {
        if(S[0] == T[0])
            return 1;
        else
            return 0;
    }
    getNext();
    for(i = 0; i < slen; i++)
    {
        while(j > 0 && S[i] != T[j])
            j = Next[j];
        if(S[i] == T[j])
            j++;
        if(j == tlen)
        {
            ans++;
            j = Next[j];
        }
    }
    return ans;
}
int main()
{

    int TT;
    int i, cc;
    cin>>TT;
    while(TT--)
    {
        cin>>S>>T;
        slen = strlen(S);
        tlen = strlen(T);
        cout<<"模式串T在主串S中首次出现的位置是: "<<KMP_Index()<<endl;
        cout<<"模式串T在主串S中出现的次数为: "<<KMP_Count()<<endl;
    }
    return 0;
}
/*
test case
aaaaaa a
abcd d
aabaa b
*/
```

- 封装版

```c++
//这个模板 字符串是从0开始的
//Next数组是从1开始的
class Solution {
    int nex[100010];

    void getnext(string t) {
        nex[0] = -1;
        int len = t.size();
        int j = 0, k = -1;
        while (j < len) {
            if (k == -1 || t[j] == t[k])
                nex[++j] = ++k;
            else
                k = nex[k];
        }
    }

public:
    int strStr(string s, string t) {
        int i = 0, j = 0;
        getnext(t);
        int len1 = s.size(), len2 = t.size();
        while (i < len1 && j < len2) {
            if (j == -1 || s[i] == t[j])
                i++, j++;
            else
                j = nex[j];
        }
        if (j == len2)return i - len2;
        return -1;
    }
};
```

