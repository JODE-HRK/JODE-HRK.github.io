---
title: 洛谷P3063MilkRouting
tags:
  - ACM
  - 算法竞赛
  - 图论
  - dij
---

# 洛谷P3063MilkRouting

传送门：https://www.luogu.com.cn/problem/P3063

分析：首先想的就是最短路，然后发现需要一些判断条件，你每次选择管道后，都需要更新当前路径的最小容量。那么，我们只需要限制最小的容量，多做几次dij就可以了。

要点：dij的灵活运用

代码：

```c++
/*
 * @Descripttion: 
 * @version: 
 * @Author: JODEHRK
 * @Date: 2020-07-06 10:46:02
 * @LastEditors: JODEHRK
 * @LastEditTime: 2020-07-07 21:20:28
 */
#include <bits/stdc++.h>
using namespace std;
struct Edge
{
    int nxt, to, w, dis;
} edge[1001];
int n, m, k, tot = 0, s, t;
int head[1001], dis[1001], c[1001], b[1001], minn = 0x7fffffff;
inline void add_edge(int from, int to, int w, int dis)
{
    edge[++tot].nxt = head[from];
    edge[tot].to = to;
    edge[tot].w = w;
    edge[tot].dis = dis;
    head[from] = tot;
}
inline void add(int from, int to, int w, int dis)
{
    add_edge(from, to, w, dis);
    add_edge(to, from, w, dis);
}
void dij(int x)
{
    queue<int> q;
    memset(dis, 50, sizeof(dis));
    memset(b, 0, sizeof(b));
    q.push(s);
    dis[s] = 0;
    b[s] = 1;
    while (!q.empty())
    {
        int u = q.front();
        q.pop();
        b[u] = 0;
        for (int i = head[u]; i; i = edge[i].nxt)
        {
            int v = edge[i].to;
            if (dis[v] > dis[u] + edge[i].dis && edge[i].w >= x)
            {
                dis[v] = dis[u] + edge[i].dis;
                if (!b[v])
                {
                    q.push(v);
                    b[v] = 1;
                }
            }
        }
    }
}
int main()
{
    cin >> n >> m >> k;
    for (int i = 1; i <= m; i++)
    {
        int x, y, d, l;
        cin >> x >> y >> d >> l;
        add(x, y, l, d);
        c[i] = l;
    }
    sort(c + 1, c + m + 1);
    s = 1, t = n;
    for (int i = 1; i <= m; i++)
    {
        dij(c[i]);
        minn = min(minn, dis[t] + k / c[i]);
    }
    cout << minn;
    return 0;
}
```

