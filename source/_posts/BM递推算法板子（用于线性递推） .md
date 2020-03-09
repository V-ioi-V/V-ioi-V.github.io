---
title: BM递推算法板子（用于线性递推） 
tags: [模板,数论]
category: 算法
date: 2019-08-02 18:52
---

感谢dls

<!--more-->

<font size=5> 



## 代码

```c++
#include<bits/stdc++.h>//BM递推算法板子（用于线性递推）
using namespace std;
#define ll long long
#define ull unsigned ll
#define met(a,x) memset(a,x,sizeof(a))
#define SZ(x) ((ll)(x).size())
typedef pair<ll,ll> PII;
typedef vector<ll> VII;
const ll mod=1000000007;
ll qpow(ll a,ll b)
{
    ll ans=1;
    a%=mod;
    while(b)
    {
        if(b&1)
            ans=ans*a%mod;
        b>>=1;
        a=a*a%mod;
    }
    return ans;
}
namespace BM
{
const ll N=10010;
ll val[N],base[N],tmpc[N],tmpd[N];

VII Md;
void mul(ll *a,ll *b,ll k)
{
    for(ll i=0; i<2*k; i++)
        tmpc[i]=0;
    for(ll i=0; i<k; i++)
        if (a[i])
            for(ll j=0; j<k; j++)
                tmpc[i+j]=(tmpc[i+j]+a[i]*b[j])%mod;
    for (ll i=k+k-1; i>=k; i--)
        if (tmpc[i])
            for(ll j=0; j<SZ(Md); j++)
                tmpc[i-k+Md[j]]=(tmpc[i-k+Md[j]]-tmpc[i]*tmpd[Md[j]])%mod;
    for(ll i=0; i<k; i++)
        a[i]=tmpc[i];
}
ll calcu(ll n,VII a,VII b)
{
    ll ans=0,tmp=0;
    ll k=SZ(a);
    for(ll i=0; i<k; i++) tmpd[k-1-i]=-a[i];
    tmpd[k]=1;
    Md.clear();
    for(ll i=0; i<k; i++)
        if (tmpd[i]!=0)
            Md.push_back(i);
    for(ll i=0; i<k; i++)
        val[i]=base[i]=0;
    val[0]=1;
    while ((1ll<<tmp)<=n)
        tmp++;
    for (ll p=tmp; p>=0; p--)
    {
        mul(val,val,k);
        if ((n>>p)&1)
        {
            for (ll i=k-1; i>=0; i--)
                val[i+1]=val[i];
            val[0]=0;
            for(ll j=0; j<SZ(Md); j++)
                val[Md[j]]=(val[Md[j]]-val[k]*tmpd[Md[j]])%mod;
        }
    }
    for(ll i=0; i<k; i++)
        ans=(ans+val[i]*b[i])%mod;
    if (ans<0)
        ans+=mod;
    return ans;
}
VII BM(VII s)
{
    VII C(1,1),B(1,1);
    ll L=0,m=1,b=1;
    for(ll n=0; n<SZ(s); n++)
    {
        ll d=0;
        for(ll i=0; i<(L+1); i++)
            d=(d+(ll)C[i]*s[n-i])%mod;
        if (d==0)
            ++m;
        else if (2*L<=n)
        {
            VII T=C;
            ll c=mod-d*qpow(b,mod-2)%mod;
            while (SZ(C)<SZ(B)+m)
                C.push_back(0);
            for(ll i=0; i<SZ(B); i++)
                C[i+m]=(C[i+m]+c*B[i])%mod;
            L=n+1-L;
            B=T;
            b=d;
            m=1;
        }
        else
        {
            ll c=mod-d*qpow(b,mod-2)%mod;
            while (SZ(C)<SZ(B)+m)
                C.push_back(0);
            for(ll i=0; i<SZ(B); i++)
                C[i+m]=(C[i+m]+c*B[i])%mod;
            ++m;
        }
    }
    return C;
}
ll fun(VII a,ll n)
{
    VII c=BM(a);
    c.erase(c.begin());
    for(ll i=0; i<SZ(c); i++)
        c[i]=(mod-c[i])%mod;
    return calcu(n,c,VII(a.begin(),a.begin()+SZ(c)));
}
};
vector<ll> v;
int main()
{
    v.push_back(20);
    v.push_back(46);
    v.push_back(106);
    v.push_back(244);
    v.push_back(560);
    v.push_back(1286);
    v.push_back(2956);
    v.push_back(6794);
    ll T;
    scanf("%d",&T);
    while (T--)
    {
        ll n;
        scanf("%lld",&n);
        if(n==1)
        {
            puts("3");
            continue;
        }
        else if(n==2)
        {
            puts("9");
            continue;
        }
        n-=3;
        printf("%lld\n",BM::fun(v,n));
    }
    return 0;
}
```

