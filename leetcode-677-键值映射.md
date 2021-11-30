---
title: leetcode-677-键值映射
date: 2021-11-14 16:26:02
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/map-sum-pairs/submissions/)

![image-20211114162706562](https://gitee.com/cao_ziqiang/img/raw/master/20211114162706.png)

<hr/>

比较容易想到的办法是直接使用$map$即可，但是很显然这样的效率是比较低的。

```java
class MapSum {
    private Map<String, Integer> map;
    public MapSum() {
        map = new HashMap<>();
    }
    
    public void insert(String key, int val) {
        map.put(key, val);
    }
    
    public int sum(String prefix) {
        int ans = 0;
        for(String word : map.keySet()) {
            if (word.startsWith(prefix)) {
                ans += map.get(word);
            }
        }
        return ans;
    }
}

/**
 * Your MapSum object will be instantiated and called as such:
 * MapSum obj = new MapSum();
 * obj.insert(key,val);
 * int param_2 = obj.sum(prefix);
 */
```

这样也比较依赖系统的库函数，调用了$String.startsWith$，同时还需要遍历一趟所有的$key$。

```go
type MapSum map[string]int

func Constructor() MapSum {
    return MapSum{}
}

func (m MapSum) Insert(key string, val int) {
    m[key] = val
}

func (m MapSum) Sum(prefix string) (sum int) {
    for s, v := range m {
        if strings.HasPrefix(s, prefix) {
            sum += v
        }
    }
    return
}


/**
 * Your MapSum object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Insert(key,val);
 * param_2 := obj.Sum(prefix);
 */
```

<hr/>

```go
type MapSum struct {
    PrefixMap map[string]int
}


func Constructor() MapSum {
    return MapSum{PrefixMap : make(map[string]int)}
}


func (this *MapSum) Insert(key string, val int)  {
    this.PrefixMap[key] = val
}


func (this *MapSum) Sum(prefix string) int {
    ans := 0
    for k, v := range(this.PrefixMap) {
        if strings.HasPrefix(k, prefix) {
            ans += v
        }
    }
    return ans
}


/**
 * Your MapSum object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Insert(key,val);
 * param_2 := obj.Sum(prefix);
 */
```

虽然用Go的效率还可以，但是还是不推荐这样使用。我们看到题目中有要求是$prefix$开头，所以比较自然地会去往字典树上想。

```java
class MapSum {
    private TrieNode root;
    public MapSum() {
        root = new TrieNode();
    }
    
    public void insert(String key, int val) {
        TrieNode cur = root;
        for(Character ch : key.toCharArray()) {
            if (cur.children[ch - 'a'] == null) {
                cur.children[ch - 'a'] = new TrieNode();
            }
            cur = cur.children[ch - 'a'];
        }
        cur.isKey = true;
        cur.value = val;
    }
    private int dfs(TrieNode node) {
        int sum = 0;
        if (node == null) {
            return sum;
        }
        if (node.isKey) {
            sum += node.value;
        }
        for(int i = 0; i < 26; ++i) {
            if (node.children[i] != null) {
                sum += dfs(node.children[i]);
            }
        }
        return sum;
    }
    public int sum(String prefix) {
        TrieNode cur = root;
        for(Character ch : prefix.toCharArray()) {
            if (cur.children[ch - 'a'] != null) {
                cur = cur.children[ch - 'a'];
            } else {
                return 0;
            }
        }
        return dfs(cur);
    }
}
class TrieNode {
    public boolean isKey;
    public int value;
    public TrieNode[] children;
    public TrieNode() {
        isKey = false;
        value = 0;
        children = new TrieNode[26];
    }
}
/**
 * Your MapSum object will be instantiated and called as such:
 * MapSum obj = new MapSum();
 * obj.insert(key,val);
 * int param_2 = obj.sum(prefix);
 */
```

