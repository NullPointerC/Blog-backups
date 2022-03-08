---
title: leetcode-15-三数之和
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
hidden: true
abbrlink: 4cce238
date: 2021-10-17 16:51:29
---

[$link$](https://leetcode-cn.com/problems/3sum/)

![image-20211017165155402](https://gitee.com/cao_ziqiang/img/raw/master/20211017165155.png)

<hr/>

一个比较朴素的做法是根据两数之和的做法，在固定$nums[i]$的情况下，找到$nums[j]+nums[k] = 0 - nums[i]$的$j,k$组合。但是这样虽然在寻找时可以比较方便，但是在后面去重时将会花费大量的时间，并且3数的排列组合会有许多种情况，所以我们考虑在添加时就去重，先考虑使用比较两数之和的方法。

使用两层$for$循环来确定$a,b$的值，再使用哈希法来判断$0-(a+b)$是否在数组中出现过，这个思路本身而言是没有问题的，但是不能有重复的三元组，所以在遍历时就要考虑去重这个问题。

为了在遍历时更方便的去重，可以在遍历前先对数组排序。$Arrays.sort()$。

对于外层的去重，只需要考虑$i\gt0 \text{&&}nums[i] \neq nums[i-1]$。

而对于内层的去重，则需要考虑$b,c$两个元素的去重，所以去重条件为：

$j \gt i + 2 \text{&&} nums[j] \neq nums[j-1] \text{&&} nums[j -1 ] \neq nums[j-2]$

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        int n = nums.length;
        List<List<Integer>> ans = new ArrayList<>();
        if(n < 3) {
            return ans;
        }
        Arrays.sort(nums);
        for(int i = 0; i < n; i++) {
            if(nums[i] > 0) {
                continue;
            }
            if(i > 0 && nums[i] == nums[i - 1]){
                continue;
            }
            Set<Integer> set = new HashSet<>();
            for(int j = i + 1; j < n; j++) {
                if(j > i + 2 && nums[j] == nums[j - 1] && nums[j - 1] == nums[j - 2]) {
                    continue;
                }
                int c = 0 - (nums[i] + nums[j]);
                if(set.contains(c)) {
                    List<Integer> temp = new ArrayList<>();
                    temp.add(nums[i]);
                    temp.add(nums[j]);
                    temp.add(c);
                    ans.add(temp);
                    set.remove(c);
                }else{
                    set.add(nums[j]);
                }
            }
        }
        return ans;
    }
}
```

双指针法：

![15.三数之和.gif](https://gitee.com/cao_ziqiang/img/raw/master/20211017170309.gif)

首先将数组排序，然后有外层$for$循环从0位置处开始一直遍历到数组末尾。再设置两个指针$left,right$。$left$置为$i+1$，$right$置为$n-1$。

当$nums[i]+nums[left]+nums[right] \gt 0$时，说明和大了，所以应该往小的地方移动，即此时$right$指针左移；

当$nums[i]+nums[left]+nums[right]\lt0$时，说明和小了，索引应该往大的地方移动，即此时$left$指针右移；

当恰好相等时，也要做好去重，即$left$指针和$right$指针都跳过和自身相等的元素处。

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        int n = nums.length;
        List<List<Integer>> ans = new ArrayList<>();
        if(n < 3) {
            return ans;
        }
        Arrays.sort(nums);
        for(int i = 0; i < n; i++) {
            if(nums[i] > 0) {
                return ans;
            }
            if(i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int left = i + 1, right = n - 1;
            while(left < right) {
                if(nums[i] + nums[left] + nums[right] > 0) {
                    right--;
                }else if(nums[i] + nums[left] + nums[right] < 0) {
                    left++;
                }else{
                    List<Integer> temp = new ArrayList<>();
                    temp.add(nums[i]);
                    temp.add(nums[left]);
                    temp.add(nums[right]);
                    ans.add(temp);
                    while(left < right && nums[left] == nums[left + 1]) {
                        left++;
                    }
                    while(left < right && nums[right] == nums[right - 1]) {
                        right--;
                    }
                    right--;
                    left++;
                }
            }
        }
        return ans;
    }
}
```

Golang:

```go
func threeSum(nums []int) [][]int {
    ans, n := [][]int{}, len(nums)
    if n < 3 {
        return ans
    }
    sort.Ints(nums)
    // fmt.Println(nums)
    for i := 0; i < n; i++ {
        if nums[i] > 0 {
            return ans
        }
        if i > 0 && nums[i] == nums[i - 1] {
            continue
        }
        l, r := i + 1, n - 1
        for l < r {
            if nums[i] + nums[l] + nums[r] > 0 {
                r--
            } else if nums[i] + nums[l] + nums[r] < 0 {
                l++
            } else {
                temp := []int{}
                temp = append(temp, nums[i], nums[l], nums[r])
                ans = append(ans, temp)
                for l < r && nums[l] == nums[l + 1] {
                    l++
                }
                for l < r && nums[r] == nums[r - 1] {
                    r--
                }
                l++
                r--
            }
        }
    }
    return ans
}
```

