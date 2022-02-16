---
title: leetcode-146-LRU缓存机制
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 303968e
date: 2021-09-23 21:45:00
---

[$link$](https://leetcode-cn.com/problems/lru-cache/)

![image-20210923214549330](https://gitee.com/cao_ziqiang/img/raw/master/20210923214549.png)

<hr/>

首先要理解清楚$LRU$的策略。

对于访问而言，如果最近访问到这个关键字，那么就需要将其设置到“前面”更容易被访问到的地方，以便后续再接着访问，对于最近没有访问的元素，将其设置到“后面”不容易被访问的位置，把位置空出来腾给更经常被访问的地方。

对于插入关键字而言，如果关键字存在，则应该更新它的值。如果不存在，就应该插入这个值。同时，如果插入新值时，缓存达到上限，就应该把最不经常访问的元素删掉，腾出空间来放新元素。

回到本题就可以这么分析，对于一组键值对，我们使用一个结点$Node$保存，同时使用一个双向链表$doubleLinkedList$来建立$Node$之间的关系。

还需要设置一个$map$来保存$key$和结点$Node$的映射，以便在取值的时候方便清楚是否存在$key$对应的$Node$。

```java
class LRUCache {

    private int capacity;
    private DoubleLinkedList<Integer, Integer> list;
    private Map<Integer, Node<Integer, Integer>> map;


    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.list = new DoubleLinkedList<>();
        this.map = new HashMap<>();
    }

    public int get(int key) {
        if (!map.containsKey(key)) {
            return -1;
        }
        // 存在则先取出
        Node<Integer, Integer> node = map.get(key);
        // 删除其原位置
        list.removeNode(node);
        // 放在头部
        list.addFirst(node);
        return node.value;
    }


    public void put(int key, int value) {
        if (map.containsKey(key)) {
            Node node = map.get(key);
            node.value = value;
            map.put(key, node);
            list.removeNode(node);
            list.addFirst(node);
        } else {
            Node<Integer, Integer> newNode = new Node<>(key, value);
            if (map.size() == capacity) {
                Node<Integer, Integer> last = list.getLast();
                map.remove(last.key);
                list.removeNode(last);
            }
            map.put(key, newNode);
            list.addFirst(newNode);
        }
    }

    class Node<K, V> {
        K key;
        V value;
        Node prev;
        Node next;

        public Node() {
            this.prev = this.next = null;
        }

        public Node(K key, V value) {
            this.key = key;
            this.value = value;
            this.prev = this.next = null;
        }
    }

    class DoubleLinkedList<K, V> {
        // 虚拟头节点
        Node<K, V> head;
        // 虚拟尾节点
        Node<K, V> tail;

        public DoubleLinkedList() {
            head = new Node<>();
            tail = new Node<>();
            head.next = tail;
            tail.prev = head;
        }

        // 在头结点后面添加结点
        public void addFirst(Node<K, V> node) {
            if (node == null) {
                return;
            }
            node.next = head.next;
            head.next.prev = node;
            node.prev = head;
            head.next = node;
        }

        // 删除结点
        public void removeNode(Node<K, V> node) {
            if (node == null) {
                return;
            }
            node.prev.next = node.next;
            node.next.prev = node.prev;
            node.prev = node.next = null;
        }

        // 获取最后一个元素
        public Node<K, V> getLast() {
            return tail.prev;
        }
    }

}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```





