---
title: leetcode-1078-Bigram分词
abbrlink: bc26d4c3
date: 2021-12-26 10:36:25
---

![image-20211226104421953](https://gitee.com/cao_ziqiang/img/raw/master/20211226104422.png)

tags:  字符串、模拟

比较容易的一题，只需按空格分为数组，再循环比较即可，条件为$words[i]==first \&\& words[i+1]==second$。

```golang
func findOcurrences(text string, first string, second string) []string {
    if len(text) == 0 || len(first) == 0 || len(second) == 0 {
        return nil
    }
    ans := []string{}
    words := strings.Split(text, " ")
    for i := 0; i < len(words) - 2; i++ {
        if words[i] == first && words[i + 1] == second {
            // fmt.Println(words[i + 2])
            ans = append(ans, words[i + 2])
        }
    }
    return ans
}
```

Java:

```java
class Solution {
    public String[] findOcurrences(String text, String first, String second) {
        if(text.isBlank()) {
            return null;
        }
        List<String> ansList = new ArrayList<>();
        String[] words = text.split(" ");
        int n = words.length;
        for(int i = 0; i < n - 2; i++) {
            if(words[i].equals(first) && words[i + 1].equals(second)) {
                ansList.add(words[i + 2]);
            }
        }
        return ansList.toArray(String[]::new);
    }
}
```

