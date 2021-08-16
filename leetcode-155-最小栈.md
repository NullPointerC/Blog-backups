---
title: leetcode-155-最小栈
date: 2021-06-17 19:18:14
categories: algorithm
tags: [algorithm,Java,LeetCode]
---

<a href="https://leetcode-cn.com/problems/min-stack/submissions/" style="color:red;text-decoration:none">传送门</a>

<hr/>

题目描述:

设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

push(x) —— 将元素 x 推入栈中。

pop() —— 删除栈顶的元素。

top() —— 获取栈顶元素。

getMin() —— 检索栈中的最小元素。

示例:

<pre>
    输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]
输出：
[null,null,null,null,-3,null,0,-2]
解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
</pre>

思路分析:

题目中的push,pop,top等方法我们可以直接借助Java中Stack类的API来实现,getMin方法我们可以再维护一个valStack的时候,再额外维护一个minStack,这个minStack维护的是valStack的peek()元素作为栈顶元素时的最小元素,即在minStack中维护不同数值时候的min值。

在初始化时，我们其中一个valStack用于对元素val入栈,出栈等操作,另维护minStack用于存储valStack中的最小元素,并且<i>minStack的入栈与出栈要与valStack一致</i>。

代码实现：

```java
class MinStack {

	private Stack<Integer> valStack = null;
    private Stack<Integer> minStack = null;

    /** initialize your data structure here. */
    public MinStack() {
        valStack = new Stack<>();
        minStack = new Stack<>();
    }

    public void push(int val) {
        valStack.push(val);
        if(minStack.isEmpty()) {
            //栈为空则直接入栈
            minStack.push(val);
        }else {
            //不为空则做比较,是否比栈顶更小
            minStack.push(Math.min(val,minStack.peek()));
        }
    }
    public void pop() {
        valStack.pop();
        minStack.pop();
    }

    public int top() {
        return valStack.peek();
    }

    public int getMin() {
        return minStack.peek();
    }
}


/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(val);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

<hr/>

![image-20210617192834932](https://gitee.com/cao_ziqiang/img/raw/master/20210617192835.png)

<hr/>

后来去评论区看了看大佬的题解,有大佬提供了不用额外空间的解法

还是只维护一个valStack,但是在入栈时,连续push两次,一次为真实元素,一次为最小元素

在出栈时,连续pop两次

在需要取栈顶元素时,则将stack.size()-2位置处的元素返回

需要取最小值时,则直接返回stack.peek()元素

![image-20210617193836252](https://gitee.com/cao_ziqiang/img/raw/master/20210617193836.png)

```java
class MinStack {

    private Stack<Integer> stack;
    
    /** initialize your data structure here. */
    public MinStack() {
        stack = new Stack<Integer>();
    }

    public void push(int x) {
        if(stack.isEmpty()){
            stack.push(x);
            stack.push(x);
            }else{
            int tmp = stack.peek();
            stack.push(x);
            if(tmp<x){
                stack.push(tmp);
            }else{
                stack.push(x);
            }
        }
    }

    public void pop() {
        stack.pop();
        stack.pop();
    }

    public int top() {
        return stack.get(stack.size()-2);
    }

    public int getMin() {
        return stack.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(val);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

