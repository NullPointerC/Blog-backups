---
title: hduoj-2
date: 2021-05-21 19:34:02
categories: algorithm
tags: [algorithm,Java]
---

## 1. Problem 2002 计算球体积

<a href="http://acm.hdu.edu.cn/showproblem.php?pid=2002" style="color: red">传送门</a>

![acm.hdu.edu.cn_showproblem.php_pid=2002](https://gitee.com/cao_ziqiang/img/raw/master/20210521194026.png)

思路:

定义PI,注意到题目给了我们PI, 不要用Math类中的常量PI,然后输入输出即可

```java
import java.util.Scanner;
public class Main {
    public static final double PI = 3.1415927d;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        while (sc.hasNextDouble()) {
            double radius = sc.nextDouble();
            System.out.printf("%.3f", PI * Math.pow(radius, 3) * 4 / 3);
            System.out.println();
        }
        sc.close();
    }
}
```

## 2. Problem 2003 绝对值

<a href="http://acm.hdu.edu.cn/showproblem.php?pid=2003" style="color: red">传送门</a>



![acm.hdu.edu.cn_showproblem.php_pid=2003](https://gitee.com/cao_ziqiang/img/raw/master/20210521194513.png)

思路:

输入之后判断大小即可,使用Math.abs也可以

Math.abs()方法使用的是三元运算符,看上去形式更紧凑了一些

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNextDouble()) {
            double d = sc.nextDouble();
            if (d >= 0) {
                System.out.printf("%.2f", d);
            } else {
                System.out.printf("%.2f", 0 - d);
            }
            System.out.println();
        }
        sc.close();
    }
}
```

## 3. Problem 2004 成绩转换

<a href="http://acm.hdu.edu.cn/showproblem.php?pid=2004" style="color: red">传送门</a>



![acm.hdu.edu.cn_showproblem.php_pid=2004](https://gitee.com/cao_ziqiang/img/raw/master/20210521194727.png)

思路:

这题没啥好说的, 输入,判断一下就好了

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNextInt()) {
            int score = sc.nextInt();
            if (score >= 90 && score <= 100) {
                System.out.println("A");
            } else if (score >= 80 && score <= 89) {
                System.out.println("B");
            } else if (score >= 70 && score <= 79) {
                System.out.println("C");
            } else if (score >= 60 && score <= 79) {
                System.out.println("D");
            } else if (score >= 0 && score <= 59) {
                System.out.println("E");
            } else {
                System.out.println("Score is error!");
            }
        }
        sc.close();
    }
}
```

## 4. Problem 2005 第几天？

<a href="http://acm.hdu.edu.cn/showproblem.php?pid=2005" style="color: red">传送门</a>



![acm.hdu.edu.cn_showproblem.php_pid=2005](https://gitee.com/cao_ziqiang/img/raw/master/20210521201900.png)



思路：

这题没有啥技巧, 就硬算。或者可以掉Calendar类，但是不想调库我就没用，月份的大小也可以存进数组里，我这样比较麻烦还不好改。后面写完AC了也懒得改了。

```java
import java.util.Scanner;

/**
 * HDU-OJ-2006-第几天?
 * 
 * @author Ziqiang CAO
 *
 */
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNextLine()) {
            String date = sc.nextLine();
            String[] datas = date.split("/");
            // 如果是闰年
            if (isLeapYear(Integer.parseInt(datas[0]))) {
                // 判断月份
                if (Integer.parseInt(datas[1]) == 1) {
                    System.out.println(datas[2]);
                } else if (Integer.parseInt(datas[1]) == 2) {
                    System.out.println(31 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 3) {
                    System.out.println(31 + 29 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 4) {
                    System.out.println(31 + 29 + 31 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 5) {
                    System.out.println(31 + 29 + 31 + 30 + +Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 6) {
                    System.out.println(31 + 29 + 31 + 30 + 31 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 7) {
                    System.out.println(31 + 29 + 31 + 30 + 31 + 30 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 8) {
                    System.out.println(31 + 29 + 31 + 30 + 31 + 30 + 31 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 9) {
                    System.out.println(31 + 29 + 31 + 30 + 31 + 30 + 31 + 31 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 10) {
                    System.out.println(31 + 29 + 31 + 30 + 31 + 30 + 31 + 31 + 30 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 11) {
                    System.out.println(31 + 29 + 31 + 30 + 31 + 30 + 31 + 31 + 30 + 31 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 12) {
                    System.out
                            .println(31 + 29 + 31 + 30 + 31 + 30 + 31 + 31 + 30 + 31 + 30 + Integer.parseInt(datas[2]));
                }
            } else {
                // 不是闰年
                // 判断月份
                if (Integer.parseInt(datas[1]) == 1) {
                    System.out.println(datas[2]);
                } else if (Integer.parseInt(datas[1]) == 2) {
                    System.out.println(31 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 3) {
                    System.out.println(31 + 28 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 4) {
                    System.out.println(31 + 28 + 31 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 5) {
                    System.out.println(31 + 28 + 31 + 30 + +Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 6) {
                    System.out.println(31 + 28 + 31 + 30 + 31 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 7) {
                    System.out.println(31 + 28 + 31 + 30 + 31 + 30 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 8) {
                    System.out.println(31 + 28 + 31 + 30 + 31 + 30 + 31 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 9) {
                    System.out.println(31 + 28 + 31 + 30 + 31 + 30 + 31 + 31 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 10) {
                    System.out.println(31 + 28 + 31 + 30 + 31 + 30 + 31 + 31 + 30 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 11) {
                    System.out.println(31 + 28 + 31 + 30 + 31 + 30 + 31 + 31 + 30 + 31 + Integer.parseInt(datas[2]));
                } else if (Integer.parseInt(datas[1]) == 12) {
                    System.out
                            .println(31 + 28 + 31 + 30 + 31 + 30 + 31 + 31 + 30 + 31 + 30 + Integer.parseInt(datas[2]));
                }
            }
        }
    }

    private static boolean isLeapYear(int year) {
        boolean flag = false;
        if ((year % 4 == 0 && year % 100 != 0) || year % 400 == 0) {
            flag = true;
        }
        return flag;
    }
}
```

