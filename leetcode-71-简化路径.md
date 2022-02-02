---
title: leetcode-71-简化路径
date: 2022-01-06 10:46:01
categories: LeetCode
tags: [LeetCode,algorithm]
---

[71. 简化路径](https://leetcode-cn.com/problems/simplify-path/)

![image-20220106104642452](https://gitee.com/cao_ziqiang/img/raw/master/20220106104642.png)

<hr/>

tags: 栈、字符串

一个比较简单的办法是先将给定的$path$按照$'/'$为分隔符进行划分,划分完成后,依次遍历每一个部分,如果为空或是$'.'$,则直接跳过即可,如果为$'..'$则返回上一层,其他则直接压入栈中,在将栈倒置即为正确答案。

```java
class Solution {
    public String simplifyPath(String path) {
        if(path.isBlank() || path == null) {
            return null;
        }
        Deque<String> stack = new ArrayDeque<>();
        String[] dirs = path.substring(1).split("/");
        for(var dirName : dirs) {
            if(dirName.equals(".") || dirName.isBlank()) {
                continue;
            } else if(dirName.equals("..") && !stack.isEmpty()) {
                stack.pop();
            } else if(dirName.equals("..") && stack.isEmpty()){
                continue;
            } else {
                stack.push(dirName);
            }
        }
        Deque<String> ans = new ArrayDeque<>();
        for(var name : stack) {
            ans.push(stack.pop());
        }
        if(ans.isEmpty()) {
            return "/";
        }
        StringBuilder sb = new StringBuilder();
        for(var name : ans) {
            sb.append("/");
            sb.append(name);
        }
        return sb.toString();
    }
}
```

