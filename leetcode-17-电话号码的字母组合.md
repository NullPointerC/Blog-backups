---
title: leetcode-17-电话号码的字母组合
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
hidden: true
abbrlink: ce888fab
date: 2021-09-14 21:21:46
---

[$link$](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

<hr/>

![image-20210914212235552](https://gitee.com/cao_ziqiang/img/raw/master/20210914212235.png)

<hr/>

首先使用$map$来存储每个数字对应的所有可能的字母,然后在回溯搜索所有可能的取值.

回溯的时候维护一个字符串,表示已有字母的排列(如果没有遍历完电话号码,则已有的字母还不完整).

初始将上述字符串设置为空,每次取电话号码的一位,就从这个数字对应的所有可能字母取出,并将其中一个字母插入到已有字母的排列末尾,然后继续处理电话号码的后一位数字,直到处理完电话号码中的所有数字,然后进行回溯,遍历其他可能的字母排列.

```java
class Solution {
    public List<String> letterCombinations(String digits) {
        List<String> combinations = new ArrayList<String>();
        if (digits.length() == 0) {
            return combinations;
        }
        Map<Character, String> phoneMap = new HashMap<Character, String>() {{
            put('2', "abc");
            put('3', "def");
            put('4', "ghi");
            put('5', "jkl");
            put('6', "mno");
            put('7', "pqrs");
            put('8', "tuv");
            put('9', "wxyz");
        }};
        backtrack(combinations, phoneMap, digits, 0, new StringBuffer());
        return combinations;
    }

    public void backtrack(List<String> combinations, Map<Character, String> phoneMap, String digits, int index, StringBuffer combination) {
        if (index == digits.length()) {
            combinations.add(combination.toString());
        } else {
            char digit = digits.charAt(index);
            String letters = phoneMap.get(digit);
            int lettersCount = letters.length();
            for (int i = 0; i < lettersCount; i++) {
                combination.append(letters.charAt(i));
                backtrack(combinations, phoneMap, digits, index + 1, combination);
                combination.deleteCharAt(index);
            }
        }
    }
}
```

$tip$:当题目要求求出所有组合时,一般都要求我们使用回溯算法来求解