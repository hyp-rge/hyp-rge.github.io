---
title: luogu2814 家谱(字符串+并查集)
author: hyp-rge
avatar: /images/avatar.png
comments: false
mathjax: true
date: 2019-05-28 08:15:52
authorLink:
authorAbout:
authorDesc:
categories: OI - 题解
tags: 并查集
keywords:
description:
photos: https://i.loli.net/2019/05/28/5cec7f01ef6df89367.jpeg
---
[传送门](https://www.luogu.org/problemnew/show/P2814)
这题不该是蓝的吧?(但我想了很久)
## 分析
用`map<string,string>` 存储人的名字和他祖先的名字.
输入过于毒瘤。~~在这之前想过离线,queue,set,LCA各种乱搞,都写不出来,我太蒻了~~
对于这么奇怪的输入(一不小心就把人名首字母吃了)当然要用奇怪的方法(?)
详见代码.

## 代码
```cpp
#include<bits/stdc++.h>
#define _for(i,a,b) for(int i=(a);i<=(int)(b);i++)
#define _ref(i,a,b) for(int i=(a);i>=(int)(b);i--)
#define INF 0x3f3f3f3f
using namespace std;

map<string,string> names; // key -> 自己, value -> 祖先

string findx(string x)
{
    return names[x] == x ? x : names[x] = findx(names[x]);
}
void handle_quest()
{
    char c = '?';
    while(c == '?')
    {
        string ch;
        cin >> ch;
        cout << ch << " " << findx(ch) << endl;
        cin >> c;
    }
}
char handle_child(string fa)
{
    char c = '+';
    cin >> c;
    while(c == '+')
    {
        string ch;
        cin >> ch;
        if(!names.count(ch)) names[ch] = ch;
        names[ch] = findx(fa);
        cin >> c;
    }
    if(c == '#') return '#';
    return '?';
}
void handle_fa()
{
    char c = '#';
    cin >> c;
    while(c == '#')
    {
        string fa;
        cin >> fa;
        if(!names.count(fa)) names[fa] = fa;
        c = handle_child(fa);
    }
    handle_quest();
}

int main()
{
    ios::sync_with_stdio(false);
    handle_fa();
    return 0;
}
```
