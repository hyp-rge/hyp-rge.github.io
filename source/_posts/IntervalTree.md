---
title: 线段树模板
author: hyp-rge
avatar: /images/avatar.png
comments: false
mathjax: true
date: 2019-06-04 14:58:36
authorLink:
authorAbout:
authorDesc:
categories: OI - 算法笔记
tags: 线段树
keywords:
description:
photos: https://i.loli.net/2019/06/04/5cf617c83307914506.jpeg
---
连线段树都不会写了，我太蒻了
这里贴综合网上各种版本后写出来的板子，备忘
`luogu3372` (区间加)
```cpp
#include<bits/stdc++.h>
#define _for(i,a,b) for(int i=(a);i<=(int)(b);i++)
#define INF 0x3f3f3f3f
#define LL long long
#define MAXN 100000
#define MAXM 100000
using namespace std;

LL sum[MAXN*4],plz[MAXN*4]; // plz加法标记
int n,m;

inline int ls(int o) {return o<<1;}
inline int rs(int o) {return (o<<1)|1;}
void pushup(int o)
{
    sum[o] = sum[ls(o)] + sum[rs(o)];
}
void build(int o,int l,int r)
{
    if(l == r)
    {
        scanf("%lld",&sum[o]);
        return;
    }
    int mid = (l+r) >> 1;
    build(ls(o),l,mid);
    build(rs(o),mid+1,r);
    pushup(o);
}
void pushdown(int o,int l,int r)
{
    plz[ls(o)] += plz[o];
    plz[rs(o)] += plz[o];
    int mid = (l+r) >> 1;
    sum[ls(o)] += plz[o] * (mid-l+1);
    sum[rs(o)] += plz[o] * (r-mid);
    plz[o] = 0;
}
void update(int o,int l,int r,int x,int y,LL v) // l~r 树区间 x~y 操作区间
{
    if(x <= l && r <= y)
    {
        plz[o] += v;
        sum[o] += v * (r-l+1);
        return;
    }
    if(plz[o]) pushdown(o,l,r);
    int mid = (l+r) >> 1;
    if(x <= mid) update(ls(o),l,mid,x,y,v);
    if(y > mid) update(rs(o),mid+1,r,x,y,v);
    pushup(o);
}
LL query(int o,int l,int r,int x,int y)
{
    if(x <= l && r <= y) return sum[o];
    pushdown(o,l,r);
    LL res = 0;
    int mid = (l+r) >> 1;
    if(x <= mid) res += query(ls(o),l,mid,x,y);
    if(y > mid) res += query(rs(o),mid+1,r,x,y);
    return res;
}
int main()
{
    scanf("%d%d",&n,&m);
    build(1,1,n);
    _for(i,1,m)
    {
        int opt;
        scanf("%d",&opt);
        if(opt == 1)
        {
            int x,y;
            LL k;
            scanf("%d%d%lld",&x,&y,&k);
            update(1,1,n,x,y,k);
        }
        else
        {
            int x,y;
            scanf("%d%d",&x,&y);
            printf("%lld\n",query(1,1,n,x,y));
        }
    }
    return 0;
}
```

`CF914D` (区间gcd)
```cpp
#include<bits/stdc++.h>
#define _for(i,a,b) for(int i=(a);i<=(int)(b);i++)
#define INF 0x3f3f3f3f
#define MAXN 500000
using namespace std;

int n,m;
int sum[MAXN*4];
int cnt;

inline int ls(int o) {return o<<1;}
inline int rs(int o) {return (o<<1)|1;}
int gcd(int x,int y)
{
    return y ? gcd(y,x%y) : x;
}
void pushup(int o)
{
    sum[o] = gcd(sum[ls(o)],sum[rs(o)]);
}
void build(int o,int l,int r)
{
    if(l == r)
    {
        scanf("%d",&sum[o]);
        return;
    }
    int mid = (l+r) >> 1;
    build(ls(o),l,mid);
    build(rs(o),mid+1,r);
    pushup(o);
}
void update(int o,int l,int r,int x,int v) // l~r树区间 x操作点
{
    if(l == r)
    {
        sum[o] = v;
        return;
    }
    int mid = (l+r) >> 1;
    if(x <= mid) update(ls(o),l,mid,x,v);
    if(x > mid) update(rs(o),mid+1,r,x,v);
    pushup(o);
}
void query(int o,int l,int r,int x,int y,int v)
{
    if(cnt > 1) return;
    if(l == r)
    {
        cnt++;
        return;
    }
    int mid = (l+r) >> 1;
    if(x <= mid && sum[ls(o)] % v) query(ls(o),l,mid,x,y,v);
    if(y > mid && sum[rs(o)] % v) query(rs(o), mid+1,r,x,y,v);
}
int main()
{
    scanf("%d",&n);
    build(1,1,n);
    scanf("%d",&m);
    while(m--)
    {
        int opt;
        scanf("%d",&opt);
        if(opt == 1)
        {
            int x,y,v;
            scanf("%d%d%d",&x,&y,&v);
            cnt = 0;
            query(1,1,n,x,y,v);
            if(cnt > 1) printf("NO\n");
            else printf("YES\n");
        }
        else
        {
            int x,v;
            scanf("%d%d",&x,&v);
            update(1,1,n,x,v);
        }
    }
    return 0;
}
```
感谢：
[线段树(单标记+离散化+扫描线+双标记)+zkw线段树+权值线段树+主席树及一些例题](https://www.cnblogs.com/stxy-ferryman/p/8847406.html)
[线段树区间更新操作及Lazy思想(详解)](https://www.cnblogs.com/ECJTUACM-873284962/p/6791203.html)
[线段树 从入门到进阶](https://www.cnblogs.com/jason2003/p/9676729.html)
[Senior Data Structure · 浅谈线段树（Segment Tree）](https://pks-loving.blog.luogu.org/senior-data-structure-qian-tan-xian-duan-shu-segment-tree)
