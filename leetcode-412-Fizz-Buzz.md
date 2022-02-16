---
title: leetcode-412-Fizz-Buzz
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 1757aa6a
date: 2021-10-13 15:55:04
---

[$link$](https://leetcode-cn.com/problems/fizz-buzz/)

<hr/>

![image-20211013155547284](https://gitee.com/cao_ziqiang/img/raw/master/20211013155547.png)

<hr/>

比较简单的遍历即可, 根据条件添加结果即可。

```java
class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> ans = new LinkedList<>();
        for(int i = 1; i <= n; i++) {
            if(i % 5 == 0 && i % 3 == 0) {
                ans.add("FizzBuzz");
            }else if (i % 3 == 0) {
                ans.add("Fizz");
            }else if (i % 5 == 0) {
                ans.add("Buzz");
            }else {
                ans.add(String.valueOf(i));
            }
        }
        return ans;
    }
}
```

<hr/>

还可以趁机练一下lambda表达式：

```java
class Solution {
    public List<String> fizzBuzz(int n) {
        return IntStream.range(1, n + 1).mapToObj(a -> a % 15 == 0 ? "FizzBuzz" : a % 3 == 0 ? "Fizz" : a % 5 == 0 ? "Buzz" : String.valueOf(a)).collect(Collectors.toList());
    }
}
```

