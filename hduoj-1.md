---
title: hduoj-1
date: 2021-05-20 11:09:38
categories: algorithm
tags: [algorithm,Java]
---

## 前言:

经过之前的学习,感觉程序设计语言本身和算法同等一样的重要,为了以后进大厂![img](https://gitee.com/cao_ziqiang/img/raw/master/20210520111342.jpg)

今天开始去hduoj上开始刷题了

因为之前啥语言都学了,算法能力感觉一般般,所以就从<a href="http://acm.hdu.edu.cn/listproblem.php?vol=11">Problem Set</a>开始刷题了

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210520111620.jpg)

## 1.Problem 2000 ASCII码排序

![acm.hdu.edu.cn_showproblem.php_pid=2000](https://gitee.com/cao_ziqiang/img/raw/master/20210520111813.png)

思路:

将每一组输入用字符串内置方法`split`分割成字符串数组,然后调用排序方法给每个单个字符组成的字符串按照ASCII码排好序,最后注意一下输出格式,感觉不太好用for-each形式输出

```java
import java.util.Arrays;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNextLine()) {
            String line = sc.nextLine();
            String[] chars = line.split("");
            Arrays.sort(chars);
            for (int i = 0; i < chars.length; i++) {
                if (i == chars.length - 1) {
                    System.out.print(chars[i]);
                } else {
                    System.out.print(chars[i] + " ");
                }
            }
            System.out.println();
        }
    }
}
```

## 2.Problem 2001 计算两点间的距离

![acm.hdu.edu.cn_showproblem.php_pid=2001](https://gitee.com/cao_ziqiang/img/raw/master/20210520120134.png)

思路:

这题倒是不难,就是有坑,之前用的int,错了几次,后面用的double才AC了,用double也要注意用hasNextDouble()方法判断是不是还有数据

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNextDouble()) {
            double x1 = sc.nextDouble();
            double y1 = sc.nextDouble();
            double x2 = sc.nextDouble();
            double y2 = sc.nextDouble();
            double x = Math.abs(x1 - x2);
            double y = Math.abs(y1 - y2);
            double res = Math.sqrt(x * x + y * y);
            System.out.printf("%.2f", res);
            System.out.println();
        }
        sc.close();
    }

}
```

