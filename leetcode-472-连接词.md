---
title: leetcode-472-连接词
date: 2021-12-28 16:39:58
categories: LeetCode
tags: [LeetCode,algorithm]
---

[472. 连接词](https://leetcode-cn.com/problems/concatenated-words/)

![image-20211228164039113](https://gitee.com/cao_ziqiang/img/raw/master/20211228164039.png)

题目要理解起来不难，重点在于怎么判断一个字符串是否可以由其他的几个字符串组成。

既然是由其他的字符串组成的，那么肯定是由比它更短的几个组成的，这里用一个$set$记录所有的单词，主要是为了防止由空串这种特例，因为不能由自己来构成自己。

剩下来的步骤就是判断是否可以由$set$里的单词来组成自身，如果可以则加入结果中。

判断的过程如下：

- 如果集合里没有单词或者是这个单词本身就是空串，那么肯定无法组成这个单词；
- 构造$dp$数组，$dp[i]$的含义为$word[0,i)$之间的子串是否可以分解为在$set$中的单词，$dp[n]$就代表是否可以完全分解；
- 那么就可以根据如上判断$word[0,i]$是否可以分解为$set$中的较短单词，先将上式分为$word[0,j-1]$和$word[j,i]$，前者对应了$dp[j]$，后者则为$word[j,i]$，只需判断$set$是否包含有$word[j,i]$即可，如果包含则$dp[i]=true$；
- 返回$dp[word.length()]$

```java
class Solution {
    public List<String> findAllConcatenatedWordsInADict(String[] words) {
        List<String> ans = new ArrayList<>();
        int n = words.length;
        if(n < 3) {
            return ans;
        }
        Set<String> set = new HashSet<>(Arrays.asList(words));
        for(int i = 0; i < n; i++) {
            // 去重空串
            if("".equals(words[i])) {
                continue;
            }
            // 先去重本身,后面再加回来
            set.remove(words[i]);
            if(canBreak(words[i], set)) {
                ans.add(words[i]);
            }
            set.add(words[i]);
        }
        return ans;
    }

    // 判断word是否可由set中的单词组合而成
    private boolean canBreak(String word, Set<String> set) {
        int len = word.length();
        // 如果单词长度为0或者是集合为空了,那么肯定无法组成这个单词
        if(len == 0 || set.size() == 0) {
            return false;
        }
        boolean[] dp = new boolean[len + 1];
        dp[0] = true;
        for(int i = 1; i < len + 1; i++) {
            for(int j = 0; j < i; j++) {
                // 如果无法构成word[0, j- 1]之间跳过
                if(!dp[j]) {
                    continue;
                }
                if(set.contains(word.substring(j, i))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[len];
    }
}
```

除了上述的$dp+set$做法，还可以利用字典树来减少前缀的匹配过程，这样对于每个单词，就不要遍历完所有的字符。

我们可以先把单词数组按照长度从小到大排序，这样就可以边检测边判断是否有连接词，因为连接词是由其他单词组成的，所以它的长度肯定更长，在后面才做匹配过程，如果不是连接词再把它加入到字典树中去。

```java
class Solution {
    // 定义trie
    private static class TrieNode {
        boolean isWord;
        Map<Character, TrieNode> children;

        TrieNode() {
            this.isWord = false;
            this.children = new HashMap<>();    
        }
    }
    // 将单词插入到字典树中
    private void insert(String word) {
        char[] letters = word.toCharArray();
        TrieNode curr = root;
        for(char letter : letters) {
            if(curr.children.get(letter) == null) {
                curr.children.put(letter, new TrieNode());
            }
            curr = curr.children.get(letter);
        }
        curr.isWord = true;
    }
    // 字典树的根节点
    TrieNode root = new TrieNode();
    
    public List<String> findAllConcatenatedWordsInADict(String[] words) {
        List<String> ans = new ArrayList<>();
        Arrays.sort(words, (a, b) -> a.length() - b.length());
        for(String word : words) {
            if(!word.isBlank()) {
                // 可以由字典树中的单词组成则返回结果
                if(dfs(word, 0)) {
                    ans.add(word);
                } else {
                    // 不是连接词的则插入字典树中
                    insert(word);
                }
            }
        }
        return ans;
    }

    private boolean dfs(String word, int position) {
        if(position == word.length()) {
            return true;
        }
        TrieNode curr = this.root;
        while(position < word.length()) {
            // 字典树中不存在该字符
            if(curr.children.get(word.charAt(position)) == null) {
                return false;
            }
            curr = curr.children.get(word.charAt(position));
            // 如果形成了一个完整的单词,则进入下一层寻找是否可继续组成
            if(curr.isWord && dfs(word, position + 1)) {
                return true;
            }
            position++;
        }
        return false;
    }
}
```

