---
title: luogu2704 [NOI2001]炮兵阵地(状压DP)
author: hyp-rge
avatar: /images/avatar.png
comments: false
mathjax: true
date: 2019-03-08 22:23:42
authorLink:
authorAbout:
authorDesc:
categories: OI - 题解
tags: [状态压缩, DP]
keywords:
description:
photos: https://i.loli.net/2019/04/30/5cc8141da61b6.jpg
---
[传送门](https://www.luogu.org/problemnew/show/P2704)
似乎没见lg的题解有这么写的?
## 分析
记$f[i][s1][s2]$表示前$i$行最后两行的状态为$s1,s2$的时候的最大炮兵数量.
对于s1,s2来说诸如`00000011`,`00001101`的状态是根本不可能的,枚举每行时枚举到这些状态纯粹是浪费时间。
所以先找出一行内所有可行状态(get_state),这样节省了时间,还节省了空间
$f[101][1<<10][1<<10]$ 开不下,而由get_state知m == 10时可行状态才只有60种,这就能开下了.
## 代码
```cpp
#include<bits/stdc++.h>
#define _for(i,a,b) for(int i=(a);i<=(b);i++)
#define _ref(i,a,b) for(int i=(a);i>=(b);i--)
#define INF 0x3f3f3f3f
using namespace std;

int n,m;
int allowCnt = 0;
int allowState[200];
int allowStateNum[200];
int s[100+1];
int f[101][66][66];

void init()
{
    scanf("%d%d",&n,&m);
    _for(i,1,n)
    {
        char c[11];
        int nowline = 0;
        scanf("%s",c);
        _for(k,0,m-1)
        {
            if(c[k] == 'H') nowline |= (1<<k);
        }
        s[i] = nowline;
    }
}
int numof(int j) //统计该状态中1的个数
{
    int cnt = 0;
    while(j)
    {
        cnt++;
        j = j & (j-1);
    }
    return cnt;
}
void get_state()
{
    _for(j,0,(1<<m)-1)
    {
        if(((j<<2) & j) == 0 && ((j>>2) & j) == 0 && ((j<<1) & j) == 0 && ((j>>1) & j) == 0)
        {
            allowState[++allowCnt] = j;
            allowStateNum[allowCnt] = numof(j);
        }
    }
}
int main()
{
    int ans = 0;
    init();
    get_state();
    _for(i,1,n)
    {
        for(int s0 = 1; s0 <= allowCnt; s0++)//上两行
        {
            for(int s1 = 1; s1 <= allowCnt; s1++)//上一行
            {
                for(int s2 = 1; s2 <= allowCnt; s2++)//当前行
                {
                    if((allowState[s0] & allowState[s1]) == 0 && (allowState[s0] & allowState[s2]) == 0 && (allowState[s1] & allowState[s2]) == 0 && (allowState[s2] & s[i]) == 0)
                    {
                        f[i][s1][s2] = max(f[i][s1][s2], f[i-1][s0][s1] + allowStateNum[s2]);
                        /* 也许是快速找法? */
                        if(i == n)
                        {
                            ans = max(ans,f[i][s1][s2]);
                        }
                        /* 也许是快速找法? */
                    }
                }
            }
        }
    }
    printf("%d",ans);
    return 0;
}
```

