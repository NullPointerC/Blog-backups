---
title: hduoj-3
date: 2021-05-29 16:25:07
categories: algorithm
tags: [algorithm,Java]
---

## 1. Problem 2006 求奇数的乘积

<a href="http://acm.hdu.edu.cn/showproblem.php?pid=2006" style="color: red">传送门</a>

![acm.hdu.edu.cn_showproblem.php_pid=2006](https://gitee.com/cao_ziqiang/img/raw/master/20210529163241.png)

思路:

没啥特殊的算法在里面,只要注意输入用sc.hasNext(),不要用hasNextLine()

```java
import java.util.Scanner;

/**
 * hduoj-2006
 * 
 * @author Ziqiang CAO
 *
 */
public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		while (sc.hasNext()) {
			int n = sc.nextInt();
			int sum = 1;
			for (int i = 0; i < n; i++) {
				int num = sc.nextInt();
				if ((num & 1) == 1) {
					sum *= num;
				}
			}
			System.out.println(sum);
		}
	}
}
```

## 2. Problem 2007 平方和与立方和

<a href="http://acm.hdu.edu.cn/showproblem.php?pid=2007" style="color: red">传送门</a>

![acm.hdu.edu.cn_showproblem.php_pid=2007](https://gitee.com/cao_ziqiang/img/raw/master/20210529164138.png)

思路:

这题有坑就在让你认为第一个输入的一定是小的那个,但是这样提交会Wrong Answer, 所以要判断哪个是小的哪个是大的

代码

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		while (sc.hasNext()) {
			int m = sc.nextInt();
			int n = sc.nextInt();
			int start = Math.min(m, n);
			int end = Math.max(m, n);
			int square = 0;
			int cube = 0;
			for (int i = start; i <= end; i++) {
				if ((i & 1) == 0) {
					// 偶数
					square += i * i;
				} else {
					cube += i * i * i;
				}
			}
			System.out.println(square + " " + cube);
		}
		sc.close();
	}
}
```

## 3. Problem 2008 数值统计

<a href="http://acm.hdu.edu.cn/showproblem.php?pid=2008" style="color: red">传送门</a>

![acm.hdu.edu.cn_showproblem.php_pid=2008](https://gitee.com/cao_ziqiang/img/raw/master/20210529165309.png)

思路:

按照输入的数判断大小即可,要注意使用nextDouble()接收。同时注意到，每次输入第一个数为0，则不进行判断，所以可以套两层while循环，但是注意内层while完之后要手动break掉。

代码：

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            while (n != 0) {
                int plusConut = 0;
                int minusConut = 0;
                int zeroCount = 0;
                for (int i = 0; i < n; i++) {
                    double num = sc.nextDouble();
                    if (num > 0) {
                        plusConut++;
                    } else if (num < 0) {
                        minusConut++;
                    } else {
                        zeroCount++;
                    }
                }
                System.out.println(minusConut + " " + zeroCount + " " + plusConut);
                break;
            }
        }
        sc.close();
    }
}
```

## 4. Problem 2009 求数列的和

<a href="http://acm.hdu.edu.cn/showproblem.php?pid=2009" style="color: red">传送门</a>

![acm.hdu.edu.cn_showproblem.php_pid=2009](https://gitee.com/cao_ziqiang/img/raw/master/20210529170510.png)

思路：

根据输入的n,m，然后使用Math封装好的平方根函数即可，但是这里输出好像有坑，用printf一直AC不来，所以我这里改用了DecimalFormat这个格式化类来输出

```java
import java.text.DecimalFormat;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        DecimalFormat df = new DecimalFormat("#.00");
        while (sc.hasNext()) {
            int n = sc.nextInt();
            int m = sc.nextInt();
            double sum = 0;
            double temp = n;
            for (int i = 0; i < m; i++) {
                sum += temp;
                temp = Math.sqrt(temp);
            }
            System.out.println(df.format(sum));
        }
        sc.close();
    }
}
```

## 5. Problem 2010 水仙花数

<a href="http://acm.hdu.edu.cn/showproblem.php?pid=2010" style="color: red">传送门</a>

![acm.hdu.edu.cn_showproblem.php_pid=2010](https://gitee.com/cao_ziqiang/img/raw/master/20210529172049.png)

思路:

取出个位,十位,百位依次比较即可,但是要注意最后一个才可以打印换行符!!!

代码:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int m = sc.nextInt();
            int n = sc.nextInt();
            int count = 0;
            int[] targets = new int[1000];
            for (int i = m; i <= n; i++) {
                if (isSXH(i)) {
                    targets[count++] = i;
                }
            }
            for (int i = 0; i < count; i++) {
                if (i < count - 1) {
                    System.out.print(targets[i] + " ");
                } else {
                    System.out.println(targets[i]);
                }
            }
            if (count == 0) {
                System.out.println("no");
            }
        }
        sc.close();
    }

    private static boolean isSXH(int num) {
        int a = num / 100;
        int b = num / 10 % 10;
        int c = num % 10;
        boolean flag = false;
        if (a * a * a + b * b * b + c * c * c == num) {
            flag = true;
        }
        return flag;
    }
}
```

## 6. Problem 2011 多项式求和

<a href="http://acm.hdu.edu.cn/showproblem.php?pid=2011" style="color: red">传送门</a>

![acm.hdu.edu.cn_showproblem.php_pid=2011](https://gitee.com/cao_ziqiang/img/raw/master/20210529182549.png)

思路:

写一个助手方法帮助我们计算多项式的值即可,注意要进行浮点数除非要用1.0去除,不能用1去除

代码:

```java
import java.text.DecimalFormat;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {

        DecimalFormat df = new DecimalFormat("0.00");
        Scanner sc = new Scanner(System.in);
        int m = sc.nextInt();
        for (int i = 1; i <= m; i++) {
            int num = sc.nextInt();
            System.out.println(df.format(helper(num)));
        }
        sc.close();

    }

    private static double helper(int bits) {
        double res = 0;
        for (int i = 1; i <= bits; i++) {
            if ((i % 2) == 1) {
                // 奇数则是加
                res += (1.0 / i);
            } else {
                res -= (1.0 / i);
            }
        }
        return res;
    }
}
```

## 7. Problem 2012 素数判定

<a href="http://acm.hdu.edu.cn/showproblem.php?pid=2012" style="color: red">传送门</a>

![acm.hdu.edu.cn_showproblem.php_pid=2012](https://gitee.com/cao_ziqiang/img/raw/master/20210529184133.png)

思路:

注意输出输出即可,看清楚是全部都为素数

代码:

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int a = sc.nextInt();
            int b = sc.nextInt();
            if (a == 0 && b == 0) {
                break;
            }
            int x = Math.min(a, b);
            int y = Math.max(a, b);
            boolean flag = true;
            for (int i = x; i <= y; i++) {
                if (!isPrime(func(i))) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                System.out.println("OK");
            } else {
                System.out.println("Sorry");
            }
        }
        sc.close();
    }

    private static int func(int n) {
        return (int) Math.pow(n, 2) + n + 41;
    }

    private static boolean isPrime(int number) {
        if (number <= 2) {
            return true;
        }
        boolean flag = true;
        int n = (int) Math.sqrt(number);

        for (int i = 2; i < n; i++) {
            if (number % i == 0) {
                flag = false;
                break;
            }
        }
        return flag;
    }
}
```

