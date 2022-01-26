---
title: leetcode-2034-股票价格波动
date: 2022-01-23 23:07:02
categories: LeetCode
tags: [LeetCode,algorithm]
---

[2034. 股票价格波动](https://leetcode-cn.com/problems/stock-price-fluctuation/)

![image-20220123231606966](https://gitee.com/cao_ziqiang/img/raw/master/20220123231607.png)

需要同时维护时间戳和价格的对应关系以及任意时刻的最大值和最小值。前者可以用一个$map$来维护，就可以维护好时间戳和价格的关系，put方法确保了每时每刻都是最新的价格。

最值可以用堆来维护，每次有新的时间戳及时间来调用update时，先更新map中的数据。取最值时再从堆中取，维护一个大根堆和一个小根堆，如果堆中的价格是最新的，那么应该满足：

对于堆顶的二元组Stock，`map.get(Stock.timestamp)=Stock.price`。

这是因为每一次update也会将最新的二元组加入堆中，所以可以确保堆中有正确的最值。

如果不是正确的最值，那么他应该是之前的价格，就不能和正确的timestamp对应上，所以可以维护堆中的信息。

```java
class StockPrice {
    private static class Stock {
        public int timestamp;
        public int price;
        public Stock() {}
        public Stock(int timestamp, int price) {
            this.timestamp = timestamp;
            this.price = price;
        }
    }

    private Map<Integer, Integer> map;
    private PriorityQueue<Stock> maxPriceHeap;
    private PriorityQueue<Stock> minPriceHeap;
    private int currentTime;
    public StockPrice() {
        this.map = new HashMap<>();
        this.maxPriceHeap = new PriorityQueue<>((a, b) -> {return b.price - a.price;});
        this.minPriceHeap = new PriorityQueue<>((a, b) -> {return a.price - b.price;});
        this.currentTime = 0;
    }
    
    public void update(int timestamp, int price) {
        this.map.put(timestamp, price);
        Stock s = new Stock(timestamp, price);
        this.maxPriceHeap.offer(s);
        this.minPriceHeap.offer(s);
        this.currentTime = Math.max(this.currentTime, timestamp);
    }
    
    public int current() {
        return this.map.get(this.currentTime);
    }
    
    public int maximum() {
        return getPrice(this.maxPriceHeap);
    }
    
    public int minimum() {
        return getPrice(this.minPriceHeap);
    }

    private int getPrice(PriorityQueue<Stock> pq) {
        while(!pq.isEmpty()) {
            Stock s = pq.peek();
            // 不是最新的价格,出队,重新计算下一个是否为最新的价格
            if(this.map.get(s.timestamp) == s.price) break;
            pq.poll();
        }
        return pq.peek().price;
    }
}

/**
 * Your StockPrice object will be instantiated and called as such:
 * StockPrice obj = new StockPrice();
 * obj.update(timestamp,price);
 * int param_2 = obj.current();
 * int param_3 = obj.maximum();
 * int param_4 = obj.minimum();
 */
```

