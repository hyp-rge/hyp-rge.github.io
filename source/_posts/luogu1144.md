---
title: luogu1144 最短路计数(最短路+"DP")
author: hyp-rge
avatar: /images/avatar.png
comments: false
mathjax: true
date: 2019-04-03 22:47:27
authorLink:
authorAbout:
authorDesc:
categories: OI - 题解
tags: 最短路
keywords:
description:
photos: https://i.loli.net/2019/04/30/5cc813da562f6.jpg
---
[传送门](https://www.luogu.org/problemnew/show/P1144)
最终还是投入 $SPFA$ 的怀抱.
## 分析
在SPFA的过程中进行一点处理.
记$ans[i]$为结点1到节点i的最短路条数.
①第一次发现结点x 如图中(结点2 -> 结点5)
{% asset_img graph.png %}
此时只能通过x的u到达x,所以$ans[x] = ans[x.u]$.
②再次找到结点x时(结点3 -> 结点5)
{% asset_img graph2.png %}
多了一条路(3 -> 5),所以$ans[x]=(ans[x] + 1) \% MOD$.

**别忘了取模！**

## 代码
```cpp
#include<bits/stdc++.h>
#define _for(i,a,b) for(int i=(a);i<=(b);i++)
#define _ref(i,a,b) for(int i=(a);i>=(b);i--)
#define INF 0x3f3f3f3f
#define MOD 100003
#define MAXN 1000010
#define MAXM 2000010
using namespace std;

struct Edge
{
    int from,to,dist;
    Edge(int u,int v,int w): from(u),to(v),dist(w) {}
};
struct HeapNode
{
    int d,u;
    bool operator < (const HeapNode& HN) const {return d > HN.d;}
};
int n,m;
int d[MAXN];
int ans[MAXN];
int inq[MAXN];
vector<int> G[MAXN];
vector<Edge> edges;

void addEdge(int u,int v,int w)
{
    edges.push_back(Edge{u,v,w});
    int m = edges.size();
    G[u].push_back(m-1);
}
void spfa()
{
    queue <int> Q;
    Q.push(1);
    _for(i,1,n) d[i] = INF;
    d[1] = 0;
    ans[1] = 1;
    while(!Q.empty())
    {
        int x = Q.front();
        Q.pop();
        //inq[x] = 1;
        _for(i,0,G[x].size()-1)
        {
            Edge e = edges[G[x][i]];
            if(d[e.to] == d[x] + 1) ans[e.to] = (ans[e.to] + ans[x]) % MOD;
            else if(d[e.to] >= d[x] + 1)
            {
                d[e.to] = d[x] + 1;
                //inq[e.to] = 1;
                Q.push(e.to);
                ans[e.to] = ans[x];
            }
        }
        //inq[x] = 0;
    }
}
int main()
{
    scanf("%d%d",&n,&m);
    _for(i,1,m)
    {
        int u,v;
        scanf("%d%d",&u,&v);
        if(u == v) continue; // 防自环
        addEdge(u,v,1);
        addEdge(v,u,1);
    }
    spfa();
    _for(i,1,n)
        printf("%d\n",ans[i]);
    return 0;
}
```
~~为什么用了紫书特有的图存储法还跑得这么慢啊！~~
