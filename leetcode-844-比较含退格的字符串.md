---
title: leetcode-844-比较含退格的字符串
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 20c33e34
date: 2021-11-17 20:53:35
---

[$link$](https://leetcode-cn.com/problems/backspace-string-compare/)

<hr/>

![image-20211117205412356](https://gitee.com/cao_ziqiang/img/raw/master/20211117205412.png)

<hr/>

tags： (栈、双指针)

看到这个退格操作，会很自然的联想到计算机本身的删除和撤销操作，就是基于栈来实现的。

于是我们利用两个栈，$stackS$和$stackT$来分别存储最终的$s$和$t$。若遍历到$\#$，那么判断栈是否为空，否则退一格，即出栈一个元素，是则继续下一步。遍历到其他元素，则直接入栈即可。

最后依次出栈元素，判断栈顶元素是否相等即可。

ps:在Java中的$Stack$是基于$Vector$的，而后者的所有方法都加了$synchronized$同步锁，所以我们这里使用效率更高的$Deque$。

```java
class Solution {
    public boolean backspaceCompare(String s, String t) {
        Deque<Character> stackS = new ArrayDeque<>();
        Deque<Character> stackT = new ArrayDeque<>();
        for(char c : s.toCharArray()) {
            if (c == '#') {
                if (!stackS.isEmpty()) {
                    stackS.pop();
                }
            } else {
                stackS.push(c);
            }
        }
        // System.out.println(stackS);
        for(char c : t.toCharArray()) {
            if (c == '#') {
                if (!stackT.isEmpty()) {
                    stackT.pop();
                }
            } else {
                stackT.push(c);
            }
        }
        // System.out.println(stackT);
        if (stackS.size() != stackT.size()) {
            return false;
        } else {
            while(!stackS.isEmpty()) {
                if (stackS.pop() != stackT.pop()) {
                    return false;
                }
            }
            return true;
        }
    }
}
```

这里突发奇想，想到了一种在Java中判断集合是否完全相同的方法，就是先转为字符串，再调用字符串的$equals$方法，看上去会简洁不少。

```java
class Solution {
    public boolean backspaceCompare(String s, String t) {
        Deque<Character> stackS = new ArrayDeque<>();
        Deque<Character> stackT = new ArrayDeque<>();
        for(char c : s.toCharArray()) {
            if (c == '#') {
                if (!stackS.isEmpty()) {
                    stackS.pop();
                }
            } else {
                stackS.push(c);
            }
        }
        // System.out.println(stackS);
        for(char c : t.toCharArray()) {
            if (c == '#') {
                if (!stackT.isEmpty()) {
                    stackT.pop();
                }
            } else {
                stackT.push(c);
            }
        }
        return stackS.toString().equals(stackT.toString());
        // System.out.println(stackT);
        // if (stackS.size() != stackT.size()) {
        //     return false;
        // } else {
        //     while(!stackS.isEmpty()) {
        //         if (stackS.pop() != stackT.pop()) {
        //             return false;
        //         }
        //     }
        //     return true;
        // }
    }
}
```

使用栈的时间复杂度为$O(m+n)$，空间复杂度也为$O(m+n)$。

看到题目的标签是双指针，一开始想到了从后往前遍历，但是当需要处理#号时就不知道怎么处理了，怎么移动$i$指针和$j$指针都不对，还是太菜了。。。

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211117211835.gif)

这里分享一个大佬的题解,上面也提到了,当遇到了#时,不知道该移动多少个好。这里就维护两个计数器$skipS$和$skipT$，然后倒序遍历字符串。

当遍历到`#`时，就将对应的计数器+1；

当遍历到字符时，如果计数器不为0，就跳过字符且计数器减少，若计数器为0，则无需跳过，比较两个指针对应的字符是否相等。

若两个指针都指向字符，则比较是否相等，若不等，则可以直接返回$false$，否则继续往前遍历剩下的字符。

如果两个字符串都遍历完了，那么说明它们是相等的字符串。

说实话，思路想到了，也不一定能写出来，因为要考虑的细节很多，一不小心就容易写错，掉进debug的坑里出不来了。

```java
class Solution {
    public boolean backspaceCompare(String s, String t) {
        int i = s.length() - 1, j = t.length() - 1;
        int skip1 = 0, skip2 = 0;
        while(i >= 0 || j >= 0) {
            while(i >= 0) {
                if (s.charAt(i) == '#') {
                    skip1++;
                } else if (skip1 != 0) {
                    skip1--;
                } else {
                    break;
                }
                i--;
            }
            while(j >= 0) {
                if (t.charAt(j) == '#') {
                    skip2++;
                } else if (skip2 != 0) {
                    skip2--;
                } else {
                    break;
                }
                j--;
            }
            // s或t遍历到头了
            if(i < 0 || j < 0) {
                break;
            }
            
            if (s.charAt(i--) != t.charAt(j--)) {
                return false;
            }
        }
        return i < 0 && j < 0;
    }
}
```



