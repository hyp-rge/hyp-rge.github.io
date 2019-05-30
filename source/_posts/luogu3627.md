---
title: luogu3627 [APIO2009] 抢掠计划(缩点+最长路)
author: hyp-rge
avatar: /images/avatar.png
comments: false
mathjax: true
date: 2019-04-30 08:42:20
authorLink:
authorAbout:
authorDesc:
categories: OI - 题解
tags: [缩点, Tarjan, 最短路]
keywords:
description:
photos: https://i.loli.net/2019/04/30/5cc8137a9db68.jpg
---
[传送门](https://www.luogu.org/problemnew/show/P3627)
藏在一堆紫黑题中的蓝题(
## 分析
首先想到缩点,然后统计每个强连通分量中的点权值和,建一个新图跑最长路.
一旦在原图中找到一条边连接两个分量$i,j$,则在新图中连一条边$(i,j,-(val \underline{ } of \underline{ } colors[j]))$.
$val \underline{ } of \underline{ } colors[i]$ 表示分量$i$中所有点的权值和.
~~当时写的时候心里有点虚,不太确定这样连边对不对~~
求最长路可以给边权取相反数跑SPFA(即上面的连边方法),得到的结果是最长路的相反数.或者拓扑排序+DP.
注意缩点后的操作都是针对连通分量来的.
同机房 pz dalao%%%的拓扑排序+DP代码: [传送门](https://www.luogu.org/recordnew/show/16351042)
## 代码(缩点+SPFA)
```cpp
#include<bits/stdc++.h>
#define _for(i,a,b) for(int i=(a);i<=(int)(b);i++)
#define _ref(i,a,b) for(int i=(a);i>=(int)(b);i--)
#define INF 0x3f3f3f3f
#define MAXM 1000010
#define MAXN 500010
using namespace std;

struct Edge
{
    int u,v,w;
    int next;
}edge1[MAXM],edge2[MAXM];
/* 图存储 */
int Node[MAXN];
int head1[MAXN],head2[MAXN];
int n,m,s,p,size1,size2;
bool isPub[MAXN];
/* Tarjan */
int cnt,dfs_clock;
int dfn[MAXN],low[MAXN],color[MAXN];
int val_of_colors[MAXN];
bool inq[MAXN],vis_tarjan[MAXN];
stack<int> S;
/* SPFA */
int dis[MAXN];
bool vis_spfa[MAXN];
/* 结果 */
int ans = INF;

void addEdge(int u,int v,int w,int head[],int& size,Edge edge[])
{
    size++;
    edge[size].u = u;
    edge[size].v = v;
    edge[size].w = w;
    edge[size].next = head[u];
    head[u] = size;
}
void Tarjan(int u)
{
    dfn[u] = low[u] = ++dfs_clock;
    vis_tarjan[u] = 1;
    S.push(u);
    inq[u] = 1;
    for(int i = head1[u]; i; i = edge1[i].next)
    {
        int v = edge1[i].v;
        if(!vis_tarjan[v])
        {
            Tarjan(v);
            low[u] = min(low[u],low[v]);
        }
        else if(inq[v])
        {
            low[u] = min(low[u],dfn[v]);
        }
    }
    if(low[u] == dfn[u])
    {
        cnt++;
        int t;
        do
        {
            t = S.top();
            S.pop();
            color[t] = cnt;
            inq[t] = 0;
        }
        while(t != u);
    }
}
void SPFA(int s)
{
    queue<int> Q;
    _for(i,1,cnt) dis[i] = INF;
    dis[s] = 0;
    Q.push(s);
    vis_spfa[s] = 1;
    while(!Q.empty())
    {
        int u = Q.front();
        Q.pop();
        vis_spfa[u] = 0;
        for(int i = head2[u]; i; i = edge2[i].next)
        {
            int v = edge2[i].v;
            if(dis[v] > dis[u] + edge2[i].w)
            {
                dis[v] = dis[u] + edge2[i].w;
                if(!vis_spfa[v])
                {
                    vis_spfa[v] = 1;
                    Q.push(v);
                }
            }
        }
    }
}
int main()
{
    scanf("%d%d",&n,&m);
    _for(i,1,m)
    {
        int u,v;
        scanf("%d%d",&u,&v);
        addEdge(u,v,0,head1,size1,edge1);
    }
    _for(i,1,n) scanf("%d",&Node[i]);
    scanf("%d%d",&s,&p);
    _for(i,1,p)
    {
        int t;
        scanf("%d",&t);
        isPub[t] = 1;
    }
    _for(i,1,n)
    {
        if(!vis_tarjan[i])
            Tarjan(i);
    }
    _for(i,1,n)  val_of_colors[color[i]] += Node[i];
    _for(i,1,n)
    {
        for(int j = head1[i]; j; j = edge1[j].next)
        {
            int v = edge1[j].v;
            if(color[i] != color[v])
            {
                addEdge(color[i], color[v], - (val_of_colors[color[v]]), head2, size2, edge2);
            }
        }
    }
    SPFA(color[s]);
    _for(i,1,n)
    {
        if(isPub[i]) ans = min(ans,dis[color[i]]);
    }
    printf("%d\n",val_of_colors[color[s]] +(-ans));
    return 0;
}
```
