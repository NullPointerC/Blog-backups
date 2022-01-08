---
title: AcWing-2019-拖拉机
date: 2022-01-05 19:37:43
categories: AcWing
tags: [AcWing,algorithm]
---

[2019. 拖拉机](https://www.acwing.com/problem/content/description/2021/)

#### 题目描述

```txt
干了一整天的活，农夫约翰完全忘记了他把拖拉机落在田地中央了。
他的奶牛非常调皮，决定对约翰来场恶作剧。
她们在田地的不同地方放了 N 捆干草，这样一来，约翰想要开走拖拉机就必须先移除一些干草捆。
拖拉机的位置以及 N 捆干草的位置都是二维平面上的整数坐标点。
拖拉机的初始位置上没有干草捆。
当约翰驾驶拖拉机时，他只能沿平行于坐标轴的方向（北，南，东和西）移动拖拉机，并且拖拉机必须每次移动整数距离。
例如，驾驶拖拉机先向北移动 2 单位长度，然后向东移动 3 单位长度。
拖拉机无法移动到干草捆占据的位置。
请帮助约翰确定他需要移除的干草捆的最小数量，以便他能够将拖拉机开到二维平面的原点。
```

#### 输入格式

```txt
第一行包含三个整数：NN 以及拖拉机的初始位置 (x,y)(x,y)。
接下来 NN 行，每行包含一个干草捆的位置坐标 (x,y)(x,y)。
```

#### 输出格式

输出约翰需要移除的干草捆的最小数量。

#### 数据范围

```txt
1≤N≤500001≤N≤50000,
1≤x,y≤10001≤x,y≤1000
```

#### 输入样例：

```txt
7 6 3
6 2
5 2
4 3
2 1
7 3
5 4
6 4
```

#### 输出样例：

```txt
1
```

tags:(双端队列+bfs)

学习一下如何使用双端队列和$bfs$。

开三个数组$grass$记录某个位置$(x,y)$是否有草捆，$used$记录是否走过了某个位置$(x,y)$，$moved$记录从$(0,0)$走到$(x,y)$位置需要移动多少个草捆。

首先需要设置每个位置都不可达，$memset$可以设置每个位置都不可达，然后在利用$bfs$记算从$(0,0)$到$(x,y)$需要移动多少个草捆。

因为我们是从$(0,0)$开始的，需要先找到同一层的没有草捆的位置，所以没有草捆的位置要先去访问，故而添加到头部$push\_front$，而有草捆的位置要后去访问，故而添加到尾部$push\_back$。

到达每一个没有草捆的位置都进行比较，从前一个位置过来的花费是否比之前的花费更少，而到达每一个有草捆的位置也需要进行比较，比较+1后的花费是否比原来的花费更少。

最后输出$moved[x][y]$即可。

```cpp
#include <bits/stdc++.h>
using namespace std;
#define x first
#define y second
typedef pair<int, int> PII;
const int N = 1010;
int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
bool grass[N][N];
bool used[N][N];
int moved[N][N];
void bfs(int x, int y)
{
    deque<PII> q;
    PII start = {0, 0}; //从0,0开始向外bfs
    moved[0][0] = 0;
    used[0][0] = true;
    q.push_back(start);
    while (q.size())
    {
        PII temp = q.front();
        q.pop_front();
        // 向4个方向上移动
        for (int i = 0; i < 4; i++)
        {
            int a = temp.x + dx[i];
            int b = temp.y + dy[i];
            if (a >= 0 && a < N && b >= 0 && b < N)
            {
                // 无草捆的位置权值为0,可以看作是同一层,先找到当前块能够到达的无草捆位置
                if (grass[a][b])
                {
                    // 比较达到这个位置需要移动的草捆数和从当前位置移动过来的需要移动的草捆数
                    moved[a][b] = min(moved[a][b], moved[temp.x][temp.y] + 1);
                    if (!used[a][b])
                    {
                        q.push_back({a, b});
                        used[a][b] = true;
                    }
                }
                else
                {
                    moved[a][b] = min(moved[a][b], moved[temp.x][temp.y]);
                    if (!used[a][b])
                    {
                        q.push_front({a, b});
                        used[a][b] = true;
                    }
                }
            }
        }
    }
    printf("%d\n", moved[x][y]);
}
int main()
{
    int n, x, y; //草捆个数以及拖拉机初始位置
    scanf("%d %d %d", &n, &x, &y);
    while (n--)
    {
        int nx, ny; //草捆位置nx,ny
        scanf("%d %d", &nx, &ny);
        grass[nx][ny] = true;
    }
    memset(moved, 0x3f3f3f3f, sizeof moved);
    bfs(x, y);
    return 0;
}
```

