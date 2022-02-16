---
title: 剑指Offer-30-包含min函数的栈
categories: 剑指Offer
tags:
  - 剑指Offer
abbrlink: 7eef573e
date: 2021-12-10 16:41:30
---

[剑指 Offer 30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

<hr/>

![image-20211210164221364](https://gitee.com/cao_ziqiang/img/raw/master/20211210164221.png)

<hr/>

tags ：(栈)

<hr/>

比较简单的办法是创建两个栈，一个栈存原本需要入栈的元素，一个存当前栈的最小值，在入栈的时候比较新入栈元素与原栈顶元素的大小关系决定哪个元素进入最小栈，原本入栈的元素按照原来的顺序入栈。

```java
class MinStack {
    private Deque<Integer> stack;
    private Deque<Integer> minStack;
    /** initialize your data structure here. */
    public MinStack() {
        stack = new ArrayDeque<>();
        minStack = new ArrayDeque<>();
    }
    
    public void push(int x) {
        if(minStack.isEmpty()) {
            stack.push(x);
            minStack.push(x);
        } else {
            if (x < minStack.peek()) {
                minStack.push(x);
                stack.push(x);
            } else {
                minStack.push(minStack.peek());
                stack.push(x);
            }
        }
    }
    
    public void pop() {
        minStack.pop();
        stack.pop();
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int min() {
        return minStack.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.min();
 */
```

利用一个栈也可以，不过可以往栈中存储一个结点$node$，保存有当前的值和当前的最小值两个元素。

```java
class MinStack {
    private static class Node {
        private int val;
        private int currMin;
        public Node() {

        }
        public Node(int val, int currMin) {
            this.val = val;
            this.currMin = currMin;
        }
    }
    private Deque<Node> stack;
    /** initialize your data structure here. */
    public MinStack() {
        stack = new ArrayDeque<>();
    }
    
    public void push(int x) {
        if(stack.isEmpty()) {
            Node node = new Node(x, x);
            stack.push(node);
        } else {
            if (x < stack.peek().currMin) {
                Node node = new Node(x, x);
                stack.push(node);
            } else {
                Node node = new Node(x, stack.peek().currMin);
                stack.push(node);
            }
        }
    }
    
    public void pop() {
        stack.pop();
    }   
    
    public int top() {
        return stack.peek().val;
    }
    
    public int min() {
        return stack.peek().currMin;
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.min();
 */
```

