---
title: 求一个字符串的最小(大)循环节(KMP)
tags: 字符串
category: 算法
date: 2018-07-26 19:47
---

串串

<!--more-->

<font size=5> 

## 

## 代码

- 最小循环节

```c++
#include <bits/stdc++.h>
#define ll long long
#define inf 0x3f3f3f3f
#define met(a) memset(a,0,sizeof(a))
using namespace std;
const int mod=1e9+7;
const int N = 1000010;
int Next[N];
char T[N];
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
int main()
{
    int i;
    cin>>tlen;
    for(i=0;i<tlen;i++)
        cin>>T[i];
    getNext();
    cout<<tlen-Next[tlen]<<endl;
    return 0;
}

//最小循环
```

- 最大循环节

```c++
#include <bits/stdc++.h>
#define ll long long
#define inf 0x3f3f3f3f
#define met(a) memset(a,0,sizeof(a))
using namespace std;
const int N=1e6+10;
char s[N];
int len,nxt[N];
ll f[N],ans;
inline void kmp()
{
    int i,j;
    nxt[0]=-1;
    for(i=0; i<len; ++i)
    {
        j=nxt[i];
        while(j!=-1&&s[i]!=s[j])
            j=nxt[j];
        nxt[i+1]=j+1;
    }
}
int main()
{
    int i,j;
    cin>>len>>s;
    kmp();
    for(i=1; i<=len; ++i)
        if(nxt[i])
        {
            f[i]=f[nxt[i]]+(ll)(i-nxt[i]);
            ans+=f[i];
        }
    cout<<ans<<endl;
    return 0;
}

//最大循环节
```

