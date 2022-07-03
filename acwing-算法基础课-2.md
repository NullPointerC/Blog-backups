---
title: acwing-算法基础课-2
hidden: true
categories: algorithm
tags:
  - algorithm
  - 计算机基础
abbrlink: 888fcc69
date: 2022-05-09 16:50:36
---

> 今年又是不出意外的拿了蓝桥杯Java省一，也可能是大学期间最后一场比赛了，希望可以好好学一学算法，再打得好一点，拿一个国奖吧^ _ ^。🍭🍭🍭

## 静态单链表

静态链表也是链表的一种，和动态链表不同点在于，动态链表通常使用结构体和指针来实现，因此每创建一个结点，就需要进行内存分配，对于算法竞赛来说，这是比较耗时的，因而通常会使用静态链表来实现。

静态链表包括4个部分，分别是`head`,`idx`,`e[]`和`ne[]`。

其中`head`指向头结点的下标，下标设置为从0开始。

`idx`记录了当前操作的位置，如操作至第几个数。

`e[]`数组记录了`i`下标的结点值。

`ne[]`数组记录了额`i`标记的下一个结点的下标，其中以`-1`作为尾部结点。

如下为静态单链表的模板：

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 100010;

// head表示头结点下标,e[i]表示节点i的值,ne[i]表示节点i的next指针是多少,idx表示当前已经用到了哪个点
int head, e[N], ne[N], idx;

// 初始化单链表
void init()
{
	head = -1;
	idx = 0;
}

// 向链表头部添加值为x的点作为新的头
void add_to_head(int x)
{
	e[idx] = x;
	ne[idx] = head;
	head = idx;
	idx++;
}

// 在下标k后添加值为x的点
void add(int k, int x)
{
	e[idx] = x;
	ne[idx] = ne[k];
	ne[k] = idx;
	idx++;
}
// 删除下标k后的点
void remove(int k)
{
	ne[k] = ne[ne[k]];
}


int main()
{
	init();
	int m;
	cin >> m;
	while (m--)
	{
		int k, x;
		char op;
		cin >> op;
		if (op == 'H')
		{
			cin >> x;
			add_to_head(x);
		}
		else if (op == 'D')
		{
			cin >> k;
			if (!k) head = ne[head];
			remove(k - 1);
		}
		else 
		{
			cin >> k >> x;
			add(k - 1, x);
		}
	}
	for (int i = head; i != -1; i = ne[i])
	{
		cout << e[i] << ' ';
	}
	cout << endl;
	return 0;
}
```

## 静态双链表

静态双向链表和静态单链表表示法类似,但是由于还需要有前驱指针,因为除了有`r[]`数组表示下一个结点位置的指针外，还有`l[]`数组表示指向上一个结点位置的指针。

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 100010;

int e[N], l[N], r[N], idx;

// 初始化双链表
void init()
{
	// 假定0为头结点下标,1为尾结点下标,初始下标从2开始计算
	r[0] = 1;
	l[1] = 0;
	idx = 2;
}

// 在下标为k的节点后添加值为x的点
void add(int k, int x)
{
	e[idx] = x;
	r[idx] = r[k];
	l[idx] = k;
	l[r[k]] = idx;
	r[k] = idx;
	idx++;
}

// 删除下标为k右边的点
void remove(int k)
{
	r[l[k]] = r[k];
	l[r[k]] = l[k];
}

int main()
{
	return 0;
}
```

## 栈

具有FILO性质的线性表，一般用来表示某种匹配关系。

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 100010;

int stk[N], tt;

// 初始化栈
void init()
{
	tt = 0;
}

// 往栈中填入一个元素x
void push(int x)
{
	stk[++tt] = x;
}
// 弹出栈顶
void pop()
{
	tt--;
}

// 取出栈顶
int top()
{
	return stk[tt];
}

bool empty()
{
	return tt > 0;
}


int main()
{
	return 0;
}
```

## 队列

和栈不同，添加元素和取出元素分别在两端进行，因而具有FIFO的性质。

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 100010;


int q[N], hh, tt;

void init()
{
	hh = 0;
	tt = -1;	
}

// 队尾插入一个元素
void push(int x)
{
	q[++tt] = x;
}

// 删除队头
void pop()
{
	hh++;
}

int top()
{
	return q[hh];
}

// 判断是否为空
bool empty()
{
	return hh > tt;
}

int main()
{
	return 0;
}
```

## 循环队列

解决了队列中假溢出的问题。

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 100010;


// hh 表示队头，tt表示队尾的后一个位置
int q[N], hh, tt;

void init()
{
	hh = 0;
	tt = 0;
}

// 向队尾插入一个数
void push(int x)
{
	q[tt ++ ] = x;
	if (tt == N) tt = 0;
}

// 从队头弹出一个数
void pop()
{
	hh ++ ;
	if (hh == N) hh = 0;
}

// 队头的值
int top()
{
	return q[hh];
}

// 判断队列是否为空
bool empty()
{
	return hh == tt;
}

int main()
{
	return 0;
}
```

## 单调栈

常见模型：找出每个数左边距离它最近的比它大、小的数。

推理如下：

首先假定，如果我们要找每个数$a_i$，它左边距离它最近的比它小的元素，假定我们已经将$a_i$左边的所有元素都添加进了栈中，$a_0,a_1,a_2...a_{n-2},a_{n-1}$。

如果存在有这样的关系$a_x >= a_y$且满足$x<y$，那么可以发现$a_x$永远也不会被选中，因为$a_y$距离$a_i$更近，因此，如果发现这样的关系，就可以不必将其加入栈中作为选择，因此，对于栈中的序列，可以发现这是一个**严格单调上升**的序列。

并且，如果我们需要找到$a_i$左边距离最近的更小的数，与栈顶相比较，如果发现$a_i \le top()$，可以得到$top()$也永远不会被$a_i$选中，可以删除，对于$a_i$右边的数来说也是如此。

因为如果对于$a_j(j>i)$而言，若$a_i\lt a_j$，那么$a_i$就可以先于$top()$被选中，因此$top()$可以被删除；

如果$a_i\ge a_j$，而$top() \ge a_i$，因此$top() \ge a_j$，更加不可能作为$a_j$左边距离最近的更小值，因此，综上，当遍历至$a_i$且$a_i \le top()$时，$top()$可以被删去。

模板如下：

```cpp
int tt = 0;
for (int i = 1; i <= n; i ++ )
{
    // check表示为栈顶与i的关系,需要试不同情况而论
    while (tt && check(stk[tt], i)) tt -- ;
    stk[ ++ tt] = i;
}
```

[题目链接](https://www.acwing.com/problem/content/832/)

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 100010;

// stack
int stk[N], tt = 0;

// arr
int q[N];

int main()
{
	int n;
	cin >> n;
	while (n--)
	{
		int x;
		cin >> x;
		while (tt && stk[tt] >= x) tt--;
		if (!tt) cout << -1 << ' ';
		else cout << stk[tt] << ' ';
		stk[++tt] = x;
	}
	return 0;
}
```



## 单调队列

场景模型：滑动窗口内的最值问题。

推理如下：

首先假定，对于数组$a_1,a_2,a_3...a_{n-1},a_n$，选择一个大小为$k$的窗口，依次将窗口滑动，选择每个窗口中的最小值。

由于这个过程涉及一个元素加入到窗口和一个元素被弹出窗口，因此我们可以用队列来模拟，而对于这个过程，我们可以发现对于队列中的值$q_1,q_2,q_3...$。

如果队列中存在有这样的逆序对，$q_i \gt q_j$且$i \lt j$，可以得到$q_i$永远也不会被选中，因为$q_j$的大小首先确保会被选中，其次$j$的位置靠后保证了$q_j$的存活时长会比$q_i$长，因此我们可以将这样的逆序对中的$q_i$删去，最后可以得到$q$是一个**严格单调上升**的序列。

而又因为$q_i$是一个严格单调上升的序列，需要得到序列$q$的最小值，只需要取得队头的值即可。

模板如下：

```cpp
int hh = 0, tt = -1;
for (int i = 0; i < n; i ++ )
{
    while (hh <= tt && check_out(q[hh])) hh ++ ;  // 判断队头是否滑出窗口
    while (hh <= tt && check(q[tt], i)) tt -- ;
    q[ ++ tt] = i;
}
```

我们可以发现，在单调栈和单调队列中，我们都是先使用栈和队列来模拟这个完整的过程，再从中分析哪些是肯定不会入栈或者入队的元素，舍弃这些元素的入栈和入队过程，最后就可以得到经过优化过后的问题模型。

[题目链接](https://www.acwing.com/problem/content/156/)

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1000010;

int a[N], q[N];
// 队头,队尾索引
int hh, tt;
// 初始化队列
void init()
{
	hh = 0, tt = -1;
}
int main()
{
	init();
	int n, k;
	cin >> n >> k;
	for (int i = 0; i < n; i++) 
	{
		cin >> a[i];
	}
	for (int i = 0; i < n; i++) 
	{
		// 窗口内的下标范围应该在[i - k + 1, i]之间,否则队头出队
		while (hh <= tt && i - k + 1 > q[hh]) 
		{
			hh++;
		}
		// 保证队列内是严格单调上升的
		while (hh <= tt && a[q[tt]] >= a[i])
		{
			tt--;
		}
		// i位置入队
		q[++tt] = i;
		// 窗口内有k个值了开始输出
		if (i >= k - 1)
		{
			cout << a[q[hh]] << ' ';
		}
		
	}
	cout << endl;
	init();
	for (int i = 0; i < n; i++)
	{
		// 窗口内的下标范围应该在[i - k + 1, i]之间,否则队头出队
		while (hh <= tt && i - k + 1 > q[hh]) 
		{
			hh++;
		}
		// 保证队列内是严格单调减少的
		while (hh <= tt && a[q[tt]] <= a[i])
		{
			tt--;
		}
		// i位置入队
		q[++tt] = i;
		// 窗口内有k个值了开始输出
		if (i >= k - 1)
		{
			cout << a[q[hh]] << ' ';
		}
	}
	
	return 0;
}
```

## KMP

首先考虑如何从暴力枚举到优化：

假定从$S[M]$串中找到模式串$P[N]$，我们枚举原串的每一个位置作为匹配的起点，然后从这里开始比较和模式串$P$的每一个字符，若$j$指针刚好可以走$N$步，说明$i$位置正是一个相匹配的位置。

朴素的枚举方式如下所示：

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;
const int M = 100010, N = 10010;
char p[N], s[M];
int n, m;
int main()
{
	cin >> n >> p  >> m >> s;
	for (int i = 0; i < m; i++) 
	{
		int j;
		for (j = 0; j < n; j++)
		{
			if (p[j] != s[i + j]) 
			{
				break;
			}
		}
		if (j == n) 
		{
			printf("%d ", i);
		}
	}
	return 0;
}
```

上述朴素解的时间复杂度为$O(M \times N)$，空间复杂度为$O(1)$。

存在的问题是当模式串$P$走到了不匹配的位置$j$时，没有用到已经匹配的$P[0...j-1]$这段，而是需要同时回退已走过的$i$指针和$j$指针。

![image-20220519135718026](https://static.codenote.xyz/img/202205191357135.png)

但是我们可以发，由于已经匹配了$S[i...j - 1]$和$P[0...j - 1]$这些部分，所以存在这些信息未被利用上。

于是考虑利用原有的匹配信息，模式串$P$能否不**每次只移动一个单位**，而是**移动若干个单位可以直接找到下一段可匹配的位置**。

![image-20220528135658627](https://static.codenote.xyz/img/202205281356754.png)

于是考虑对模式串$P$进行预处理，处理出以每个点为终点的后缀和前缀相等，相等的最大长度是多少，通常也称$next$数组。

$next[i]$也就表示以$i$为终点的后缀，和从$1$开始的前缀相等，而且后缀长度最长的长度。

![image-20220528140405557](https://static.codenote.xyz/img/202205281404630.png)

于是，利用上$next[]$的含义，就可以得到如下的匹配过程。

![image-20220528141928262](https://static.codenote.xyz/img/202205281419364.png)

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 10010, M = 100010;

int n, m;
char p[N], s[M];
int ne[N];


int main()
{
	// 下标从1开始
	cin >> n >> p + 1 >> m >> s + 1;
	
	// 求next过程
	for (int i = 2, j = 0; i <= n; i++)
	{
		while (j && p[i] != p[j + 1])
		{
			j = ne[j];	
		}
		if (p[i] == p[j + 1])
		{
			j++;
		}
		ne[i] = j;
	}
	
	// kmp匹配过程
	for (int i = 1, j = 0; i <= m; i++)
	{
		// j还未退回起点并且当前s[i]不能和p[j+1]匹配
		while (j && s[i] != p[j + 1])
		{
			j = ne[j];
		}
		// 上述循环有两个退出条件,这里需要验证是否已经匹配成功了,匹配成功了则移动j指针
		if (s[i] == p[j + 1])
		{
			j++;
		}
		if (j == n)
		{
			// 匹配成功
			printf("%d ", i - n);
			j = ne[j];// 
		}
	}
	return 0;
}
```

## Trie

字典树，用于高效地**存储和查找字符串集合**的数据结构。

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;


const int N = 100010;


// son[][]存储树中每个节点的子节点
// cnt[]存储以每个节点结尾的单词数量
// idx表示当前用到了第几个编号,0号点既是根节点，又是空节点
int son[N][26], cnt[N], idx;
char str[N];

void insert(char str[])
{
	int p = 0;
	for (int i = 0; str[i]; i++)
	{
		// 判断当前的节点是否有这条路径
		int u = str[i] - 'a';
		// 不存在则创建出来
		if (!son[p][u])
		{
			son[p][u] = ++idx;
		}
		p = son[p][u];
	}
	// p表示每个节点的编号
	cnt[p]++;
}

int query(char str[])
{
	int p = 0;
	for (int i = 0; str[i]; i++)
	{
		int u = str[i] - 'a';
		if (!son[p][u]) return 0;
		p = son[p][u];
	}
	return cnt[p];
}


int main()
{
	int n;
	scanf("%d", &n);
	while (n--)
	{
		char op[2];
		scanf("%s%s", op, str);
		if (op[0] == 'I')
		{
			insert(str);
		}
		else
		{
			printf("%d\n", query(str));
		}
	}
	return 0;
}
```

## 并查集

并查集用于处于集合关系。一般包括**将两个集合合并、询问两个元素在同一个集合中**。

并查集可以在近乎$O(1)$的时间复杂度内完成上述两个操作。

基本原理：每个集合用一颗树来表示，树根的编号就是整个集合的编号。

每个节点存储它的父节点，用$fa[x]$表示$x$的父节点。

树根的条件：$fa[x] = x$。

求得$x$的集合编号：

```cpp
while (fa[x] != x)
{
	x = fa[x];
}
```

含路径压缩的朴素并查集：

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 100010;
// 每个元素的父节点
int fa[N];
// n表示节点格式,m表示操作次数
int n, m;
// 初始化并查集
void init()
{
	for (int i = 1; i <= n; i++)
	{
		fa[i] = i;
	}
}

// 返回x所在集合的编号
int find(int x)
{
	if (x != fa[x])
	{
		// 带有路径压缩
		fa[x] = find(fa[x]);
	}
	return fa[x];
}
// 把a的父亲加入到b所在的集合中
void merge(int a, int b)
{
	fa[find(a)] = find(b);
}

// 判断a,b是否属于同一个集合
bool query(int a, int b)
{
	return find(a) == find(b);
}

int main()
{
	scanf("%d%d", &n, &m);
	init();
	while (m--)
	{
		char op[2];
		int a, b;
		scanf("%s%d%d", op, &a, &b);
		if (op[0] == 'M')
		{
			merge(a, b);
		}
		else if (op[0] == 'Q')
		{
			if (query(a, b)) 
			{
				puts("Yes");
			}
			else
			{
				puts("No");
			}
		}
	}
	return 0;
}
```

以上即为朴素的并查集，可以快速处理集合的合并和查询操作，一般还会进行延伸，包括维护$size$的并查集。

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;


const int N = 100010;
// fa[x]表示节点x的父节点,size[x]只有当x节点为父节点时才有含义,表示为父节点所在集合中节点个数
int fa[N], sz[N];
// n表示节点个数,m表示操作数
int n, m;

// 初始化并查集
void init()
{
	for (int i = 1; i <= n; i++)
	{
		// 将i的父亲设置为自身
		fa[i] = i;
		// 每个节点所在集合初始个数为1
		sz[i] = 1;
	}
}

int find(int x)
{
	if (x != fa[x])
	{
		// 带有路径压缩
		fa[x] = find(fa[x]);
	}
	return fa[x];
}

// 将节点a所在集合并入节点b所在集合
void merge(int a, int b)
{
	a = find(a), b = find(b);
	if (a != b)
	{
		fa[a] = b;
		// 维护个数
		sz[b] += sz[a];
	}
}

// 判断节点a,b是否在同一集合中
bool query(int a, int b)
{
	return find(a) == find(b);
}
// 计算x所在集合中节点个数
int count(int x)
{
	return sz[find(x)];
}

int main()
{
	scanf("%d%d", &n, &m);
	init();
	while (m--)
	{
		string op;
		int a, b;
		cin >> op;
		if (op == "C") 
		{
			scanf("%d%d", &a, &b);
			merge(a, b);
		}
		else if (op == "Q1")
		{
			scanf("%d%d", &a, &b);
			if (query(a, b)) {
				puts("Yes");
			} else {
				puts("No");
			}
		}
		else if (op == "Q2")
		{
			scanf("%d", &a);
			printf("%d\n", count(a));
		}
	}
	return 0;
}
```



