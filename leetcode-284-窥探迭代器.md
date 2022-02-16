---
title: leetcode-284-窥探迭代器
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: d30bd20a
date: 2021-10-05 12:47:32
---

[$link$](https://leetcode-cn.com/problems/peeking-iterator/solution/)

<hr/>

![image-20211005124819153](https://gitee.com/cao_ziqiang/img/raw/master/20211005124819.png)

<hr/>

道理我都懂,可是说好的给我数组$nums$,反过来构造器里给我一个迭代器是什么意思?

一开始没有考虑那么多,题目其实是要我们模拟迭代器的底层原理,最早看到有一个$peek$操作,很自然联想到了$queue$

```java
// Java Iterator interface reference:
// https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html

class PeekingIterator implements Iterator<Integer> {

    private Queue<Integer> queue;    
	public PeekingIterator(Iterator<Integer> iterator) {
	    // initialize any member here.
        queue = new ArrayDeque<>();
        while(iterator.hasNext()) {
            queue.offer(iterator.next());
        }
	}
	
    // Returns the next element in the iteration without advancing the iterator.
	public Integer peek() {
        return queue.peek();
	}
	
	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	@Override
	public Integer next() {
	    return queue.poll();
	}
	
	@Override
	public boolean hasNext() {
	    return !queue.isEmpty();
	}
}
```

但是其实这样在迭代器初始化的时候,使用$queue$来接收的话会消耗大量的时间以及空间,所以时空复杂度都达到了$O(n)$级别.

<hr/>

再回到本题的话,迭代器是$Java$23中设计模式的中的迭代器模式的原型.为的是向用户屏蔽各种集合的差异,提供了一种统一的方式来遍历集合.

迭代器的原型中只提供了$next$以及$hasNext$这两个方法.于是我们考虑要获取$peek$,我们先一步制作一个空间大小为1的缓存区$cache$,并在访问前先一步设置好$cache$.

```java
// Java Iterator interface reference:
// https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html

class PeekingIterator implements Iterator<Integer> {

    private Iterator<Integer> iterator;
    private Integer cache = null; // 第一次peek时, 缓存迭代的元素

    public PeekingIterator(Iterator<Integer> iter) {
        iterator = iter;
    }

    public Integer peek() {
        if (cache == null)
            cache = iterator.next();
        return cache;
    }

    @Override
    public Integer next() {
        if (cache == null)
            return iterator.next();
        Integer next = cache;
        cache = null;
        return next;
    }

    @Override
    public boolean hasNext() {
        return cache != null || iterator.hasNext();
    }
}
```

