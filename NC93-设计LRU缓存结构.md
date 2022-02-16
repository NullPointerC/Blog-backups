---
title: NC93-设计LRU缓存结构
categories:
  - nowcoder
tags:
  - algorithm
  - nowcoder
abbrlink: f30ffccd
date: 2021-09-28 21:47:42
---

[$link$](https://www.nowcoder.com/practice/e3769a5f49894d49b871c09cadd13a61?tpId=188&&tqId=38550&rp=1&ru=/activity/oj&qru=/ta/job-code-high-week/question-ranking)

<hr/>

![image-20210928214949697](https://gitee.com/cao_ziqiang/img/raw/master/20210928214949.png)

<hr/>

![image-20210928215022233](https://gitee.com/cao_ziqiang/img/raw/master/20210928215022.png)

![image-20210928215036048](https://gitee.com/cao_ziqiang/img/raw/master/20210928215046.png)

<hr/>

可以用一个双向链表$doubleLinkedList$来作为缓存保存每一组键值对,最近被访问多的放在链表头部,而访问少的放在链表尾部,以便快速获取到。

为了加快访问的速度，快速得到某个结点，还可以用$map$存储$key \Rightarrow node$的关系，以便快速得到某个结点。

设计好底层的数据结构，那么就考虑如何设计$LRU$的$get$方法以及$set$方法。

$get$时，可以先用$map$判断其是否存在，不存在直接返回$-1$即可，存在则返回该结点的值；

$set$时，也要先判断其是否存在，存在要覆盖它原来的值，又因为这算做一次访问，所以要将其放置在链表头部。再考虑不存在的情况，如果不存在，则可以先将键值对创建出来并插入到链表头部，同时判断插入前此时链表的个数是否达到给定的容量$k$，如果达到了容量，还需要将链表尾部即最不经常访问到的值从缓存中移除，同时移除$map$的关系。

```java
import java.util.*;


public class Solution {
    
    
    private int capacity;
    private Map<Integer, Node<Integer, Integer>> map;
    private DoubleLinkedList<Integer, Integer> cache;
    /**
     * lru design
     * @param operators int整型二维数组 the ops
     * @param k int整型 the k
     * @return int整型一维数组
     */
    public int[] LRU (int[][] operators, int k) {
        // write code here
        this.capacity = k;
        this.map = new HashMap<>();
        this.cache = new DoubleLinkedList<>();
        int n = operators.length;
        List<Integer> ans = new ArrayList<>();
        for(int i = 0; i < n; i++) {
            if(operators[i][0] == 1) {
                this.set(operators[i][1], operators[i][2]);
            }else{
                ans.add(this.get(operators[i][1]));
            }
        }
        int[] arr = new int[ans.size()];
        for(int i = 0; i < ans.size(); i++) {
            arr[i] = ans.get(i);
        }
        return arr;
    }
    
    public int get(int key) {
        if(!map.containsKey(key)) {
            return -1;
        }
        Node<Integer, Integer> node = map.get(key);
        cache.remove(node);
        cache.addFirst(node);
        return node.value;
    }
    
    public void set(int key, int value) {
        if(map.containsKey(key)) {
            Node node = map.get(key);
            node.value = value;
            map.put(key, node);
            cache.remove(node);
            cache.addFirst(node);
        }else{
            Node<Integer,Integer> newNode = new Node<>(key, value);
            if(map.size() == capacity) {
                Node<Integer, Integer> last = cache.getLast();
                map.remove(last.key);
                cache.remove(last);
            }
            cache.addFirst(newNode);
            map.put(key, newNode);
        }
    }
    

    
    public class Node<K, V> {
        K key;
        V value;
        Node prev;
        Node next;
        public Node() {
            this.prev = this.next = null;
        }
        public Node(K key, V value) {
            this();
            this.key = key;
            this.value = value;
        }
    }
    
    public class DoubleLinkedList<K, V> {
        // 虚拟头结点
        Node<K,V> head;
        // 虚拟尾结点
        Node<K,V> tail;
        
        public DoubleLinkedList() {
            this.head = new Node<>();
            this.tail = new Node<>();
            head.next = tail;
            tail.prev = head;
        }
        
        // 在头部添加
        public void addFirst(Node<K, V> node) {
            if(node == null) {
                return;
            }
            node.next = head.next;
            head.next.prev = node;
            node.prev = head;
            head.next = node;
        }
        
        // 删除结点
        public void remove(Node<K,V> node) {
            if(node == null) {
                return;
            }
            node.prev.next = node.next;
            node.next.prev = node.prev;
            node.prev = node.next = null;
        }
        
        // 获取最后一个元素
        public Node<K,V> getLast() {
            return tail.prev;
        }
    }
}
```

