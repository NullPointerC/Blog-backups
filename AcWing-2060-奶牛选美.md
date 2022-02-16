---
title: AcWing-2060-奶牛选美
categories: AcWing
tags:
  - AcWing
  - algorithm
hidden: true
abbrlink: 5e30548d
date: 2022-01-04 17:03:09
---

[2060. 奶牛选美](https://www.acwing.com/problem/content/description/2062/)

<hr/>

#### 题目描述

```txt
听说最近两斑点的奶牛最受欢迎，约翰立即购进了一批两斑点牛。
不幸的是，时尚潮流往往变化很快，当前最受欢迎的牛变成了一斑点牛。
约翰希望通过给每头奶牛涂色，使得它们身上的两个斑点能够合为一个斑点，让它们能够更加时尚。
牛皮可用一个 N×M 的字符矩阵来表示，如下所示：
................
..XXXX....XXX...
...XXXX....XX...
.XXXX......XXX..
........XXXXX...
.........XXX....
其中，X 表示斑点部分。
如果两个 X 在垂直或水平方向上相邻（对角相邻不算在内），则它们属于同一个斑点，由此看出上图中恰好有两个斑点。
约翰牛群里所有的牛都有两个斑点。
约翰希望通过使用油漆给奶牛尽可能少的区域内涂色，将两个斑点合为一个。
在上面的例子中，他只需要给三个 . 区域内涂色即可（新涂色区域用 ∗ 表示）：
................
..XXXX....XXX...
...XXXX*...XX...
.XXXX..**..XXX..
........XXXXX...
.........XXX....
请帮助约翰确定，为了使两个斑点合为一个，他需要涂色区域的最少数量。
```

#### 输入格式

```txt
第一行包含两个整数 N 和 M。
接下来 N 行，每行包含一个长度为 M 的由 X 和 . 构成的字符串，用来表示描述牛皮图案的字符矩阵。
```

#### 输出格式

```txt
输出需要涂色区域的最少数量。
```

#### 数据范围

```txt
1≤N,M≤50
```

#### 输入样例：

```txt
6 16
................
..XXXX....XXX...
...XXXX....XX...
.XXXX......XXX..
........XXXXX...
.........XXX....
```

#### 输出样例：

```txt
3
```

一个比较简单的做法就是把这个图中的两个联通分量中的点分别统计起来，然后枚举两个联通分量中的所有点之间的距离，取最小即为所求的答案。

这里的比较麻烦的地方在于，需要分两次计算这些联通分量。可以定义$k$以及传入参数来确定当前是计算的哪一个联通分量，确定好联通分量后，将处于连通分量内的点都存储在两个集合中，枚举这两个集合中的每个点，得到两个联通分量之间的距离最小值即可。

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <vector>
using namespace std;
const int maxn = 55;
const int dx[] = {-1, 0, 1, 0};
const int dy[] = {0, 1, 0, -1};
typedef struct
{
    int x;
    int y;
} point;
// 定义两个联通分量
vector<point> area1, area2;
char g[maxn][maxn];
bool st[maxn][maxn];
int N, M;
void dfs(int x, int y, int flag)
{
    point p = point{x, y};
    st[x][y] = true;
    g[x][y] = '0' + flag;
    if (flag == 1)
    {
        area1.push_back(p);
    }
    else
    {
        area2.push_back(p);
    }
    for (int i = 0; i < 4; i++)
    {
        int nx = x + dx[i];
        int ny = y + dy[i];
        if (nx < 0 || nx >= N || ny < 0 || ny >= M || g[nx][ny] != 'X' || st[nx][ny])
        {
            continue;
        }
        else
        {
            dfs(nx, ny, flag);
        }
    }
}
int main()
{

    cin >> N >> M;
    for (int i = 0; i < N; i++)
    {
        cin >> g[i];
    }
    for (int i = 0, k = 0; i < N; i++)
    {
        for (int j = 0; j < M; j++)
        {
            if (g[i][j] == 'X' && !st[i][j])
            {
                if (k == 0)
                {
                    dfs(i, j, 1);
                    k++;
                }
                else
                {
                    dfs(i, j, 2);
                }
            }
        }
    }
    int ans = 1e9 + 10;
    for (auto p1 : area1)
    {
        for (auto p2 : area2)
        {
            ans = min(ans, abs(p1.x - p2.x) + abs(p1.y - p2.y) - 1);
        }
    }
    printf("%d\n", ans);
    return 0;
}
```

