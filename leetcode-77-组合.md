---
title: leetcode-77-组合
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: e545650c
date: 2021-11-10 15:07:00
---

[$link$](https://leetcode-cn.com/problems/combinations/)

<hr/>

![image-20211110150739934](https://gitee.com/cao_ziqiang/img/raw/master/20211110150739.png)

<hr/>

比较常见的回溯算法。在这里总结一下回溯，回溯其实就是暴力的搜，通常这样的题在规模不大的时候可以用套循环来解决，但是当$k$的数值很大时，就会使得循环很长，回溯就是在递归中隐式调用了循环。

很多大佬总结说过回溯算法就是在遍历一棵N叉树。

可以根据样例1分析，一开始集合是$[1,2,3,4]$，我们从左往右取值，取过的数，就不能再重复取了分别取$[2,3,4]$就得到了$[1,2],[1,3],[1,4]$，所以我们每次从集合中取过数之后，可以选择的元素就要被压缩，调整可以选择的范围。

在本题中，$n$即为树的宽度，$k$为树的深度，从这棵树遍历到底是，就要自底向上的收集每个结果，所以就得到了这种组合的所有结果。

```java
class Solution {
    List<List<Integer>> ans = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> combine(int n, int k) {
        ans.clear();
        path.clear();
        dfs(n, k, 1);
        return ans;
    }
    private void dfs(int n, int k, int start) {
        if (path.size() == k) {
            ans.add(new LinkedList(path));
            return ;
        }
        for(int i = start; i <= n; ++i) {
            path.add(i);
            dfs(n, k, i + 1);
            path.removeLast();
        }
    }
}
```

Go：

```go
var res [][]int 
func combine(n int, k int) [][]int {
    res=[][]int{}
    if n <= 0 || k <= 0 || k > n {
	    return res
	}
    backtrack(n, k, 1, []int{})
	return res
}
func backtrack(n,k,start int,track []int){
    if len(track)==k{
        temp:=make([]int,k)
        copy(temp,track)
        res=append(res,temp)
    }
    if len(track) + n - start + 1 < k {
		return
	}
    for i := start; i <= n; i++ { 
        track=append(track,i)
        backtrack(n,k,i+1,track)
        track=track[:len(track)-1]
    }
}
```

Go语言的切片要删除元素真是挺麻烦的呢，也没有封这个方法。。。

