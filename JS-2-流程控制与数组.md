---
title: JS-2.流程控制与数组
date: 2021-04-01 11:16:24
categories: JavaScript
tags: [frontend,JS]
---

## 一.流程控制语句

### 1.1if语句

if语句是最简单的条件语句，也称选择语句。它通常结合else—起使用，表示如果...就....否则..

![image-20210401111807950](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401111807950.png)

- 在if语句中,如果只有一个单分支,else可以省略

- 如果if语句块只有一行,那么可以省略大括号和换行(不推荐)

### 1.2switch语句

![image-20210401112222257](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401112222257.png)

switch语句并不像if语句那样当执行了某一个分支之后会自动跳出if语句体，程序员必须主动调用break来跳出switch语句体。如果不书写break，则后面的所有case都将被视为匹配，直到遇见break.

### 1.3三元运算符

![image-20210401112336708](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401112336708.png)

### 1.4for循环语句

![image-20210401112426437](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401112426437.png)

<hr/>

![image-20210401112442468](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401112442468.png)

### 1.5while循环

while语句也是─种循环结构，是一种“不定范围”循环,和for循环各有不同的用武之地

![image-20210401112712432](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401112712432.png)

### 1.6break和continue

break表示立即终止循环，它只能用在循环语句中，在for循环和while循环中都可以使用;

continue用于跳过循环中的一个迭代，并继续执行循环中的下一个迭代。for循环更经常使用continue

### 1.7do-while语句

do-while循环是一种“后测试循环语句”。它不同于for循环和while循环每次都是“先测试条件是否满足，然后执行循环体”， do-while循环是“先执行循环体，然后测试条件是否满足”。

![image-20210401112945116](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401112945116.png)

## 二.数组

数组(Array)，顾名思义，用来存储一组相关的值

从而方便进行求和、计算平均数、逐项遍历等操作。

![image-20210401113424808](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401113424808.png)

### 2.1数组定义方式

![image-20210401113442581](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401113442581.png)

<hr/>

![image-20210401113452321](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401113452321.png)

<hr/>

![image-20210401113523204](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401113523204.png)

### 2.2数组的修改

![image-20210401113640344](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401113640344.png)

<hr/>

![image-20210401113703101](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401113703101.png)

### 2.3数组的常用方法

![image-20210401113924935](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401113924935.png)

![image-20210401113957569](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401113957569.png)

<hr/>

![image-20210401114053008](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401114053008.png)

<hr/>

![image-20210401114656503](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401114656503.png)

<hr/>

![image-20210401114711261](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401114711261.png)

<hr/>

![image-20210401114835330](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401114835330.png)

<hr/>

![image-20210401115201446](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401115201446.png)

![image-20210401115232166](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401115232166.png)

<hr/>

![image-20210401115325878](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401115325878.png)

![image-20210401115430114](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401115430114.png)

<hr/>

![image-20210401115650737](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401115650737.png)

<hr/>

![image-20210401115808061](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401115808061.png)

<hr/>

![image-20210401120006722](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401120006722.png)

<hr/>

### 2.4二维数组

二维数组:以数组作为数组中元素的数组;

## 三.引用类型

### 3.1基本数据类型和引用数据类型介绍

![image-20210401120924172](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401120924172.png)

![image-20210401120959270](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401120959270.png)

<hr/>

![image-20210401121027316](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401121027316.png)

<hr/>

相等判断时的区别

- 基本类型进行相等判断时，会比较值是否相等
- 引用类型进行相等判断时，会比较址是否相等，也就是说它会比较是否为内存中的同一个东西

![image-20210401121202939](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401121202939.png)

### 3.2深克隆和浅克隆

使用arr1=arr2的语法不会克隆数组

- 浅克隆:只克隆数组的第一层，如果是多维数组或者数组中的项是其他引用类型值，则不克隆其他层
- 深克隆:克隆数组的所有层，要使用递归技术

demo1:

```js
//浅克隆demo
var arr1 = [1,2,3,4];
var result = [];
//遍历原数组中的每一项,把遍历到的项都推入结果数组中
for(var i = 0;i<arr1.length;i++) {
	result.push(arr1[i]);
}
console.log(result);//[1,2,3,4]
console.log(result == arr1);
arr1.push(5);
console.log(result);//[1,2,3,4],没有变化
```

