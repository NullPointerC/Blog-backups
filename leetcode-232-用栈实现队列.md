---
title: leetcode-232-用栈实现队列
date: 2021-07-13 09:06:11
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

<hr/>

题目描述：

<pre>
请你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作（push、pop、peek、empty）：
实现 MyQueue 类：
	void push(int x) 将元素 x 推到队列的末尾
	int pop() 从队列的开头移除并返回元素
	int peek() 返回队列开头的元素
	boolean empty() 如果队列为空，返回 true ；否则，返回 false
说明：
	你只能使用标准的栈操作 —— 也就是只有 push to top, peek/pop from top, size, 和 is empty 操作是合法的。
	你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。
进阶：
	你能否实现每个操作均摊时间复杂度为 O(1) 的队列？换句话说，执行 n 个操作的总时间复杂度为 O(n) ，即使其中一个操作可能花费较长时间。
</pre>

示例:

<pre>
输入：
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
输出：
[null, null, null, 1, 1, false]
解释：
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
</pre>




<hr/>

思路:

<pre>
这里我是使用了ArrayList来模拟操作,考虑到队列取头比较多,所以使用了ArrayList
</pre>

代码：

```java
class MyQueue {
    private ArrayList<Integer> list;
    /** Initialize your data structure here. */
    public MyQueue() {
        list = new ArrayList<>();
    }

    /** Push element x to the back of queue. */
    public void push(int x) {
        list.add(x);
    }

    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if(!this.empty()) {
            return list.remove(0);
        }else {
            throw new IllegalArgumentException("The queue is empty");
        }
    }

    /** Get the front element. */
    public int peek() {
        if(!this.empty()) {
            return list.get(0);
        }else {
            throw new IllegalArgumentException("The queue is empty");
        }
    }

    /** Returns whether the queue is empty. */
    public boolean empty() {
        return list.isEmpty();
    }
}
```

![image-20210713091812048](https://gitee.com/cao_ziqiang/img/raw/master/20210713091812.png)

![image-20210713091824304](https://gitee.com/cao_ziqiang/img/raw/master/20210713091824.png)

