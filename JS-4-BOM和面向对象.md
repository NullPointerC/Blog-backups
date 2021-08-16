---
title: JS-4-BOM和面向对象
date: 2021-04-03 10:45:53
categories: JavaScript
tags: [frontend,JS]
---

## 一.BOM

BOM (Browser Object Model，浏览器对象模型）是JS与浏览器窗口交互的接口;

一些与浏览器改变尺寸、滚动条滚动相关的特效，都要借助BOM技术;

### 1.1Window对象

- window对象是当前JS脚本运行所处的窗口，而这个窗口中包含DOM结构，window.document属性就是document对象;

- 在有标签页功能的浏览器中，每个标签都拥有自己的window对象;也就是说，同一个窗口的标签页之间不会共享一个window对象;

- 全局变量会成为window对象的属性

demo:

```js
var a = 10;
console.log(window.a == a);//true
```

这就意味着，多个js文件之间是共享全局作用域的，即js文件没有作用域隔离功能

- 内置函数普遍是window的方法

如setlnterval().alert()等内置函数，普遍是window的方法

- ![image-20210403105201715](https://gitee.com/cao_ziqiang/img/raw/master/image-20210403105201715.png)

	

### 1.2Navigator对象

window.navigator属性可以检索navigator对象，它内部含有用户此次活动的浏览器的相关属性和标识

![image-20210403105617563](https://gitee.com/cao_ziqiang/img/raw/master/image-20210403105617563.png)

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script>
        console.log("浏览器品牌"+navigator.appName);
        console.log("浏览器版本" + navigator.appVersion);
        console.log("用户代理"+navigator.userAgent);
        console.log("操作系统"+navigator.platform);
    </script>
</body>
</html>
```

### 1.3History对象

window.history对象提供了操作浏览器会话历史的接口

常用操作就是模拟浏览器回退按钮

```js
history.back();//等同于点击浏览器的回退按钮
history.go(-1);//等同于history.back();
```

### 1.4Location对象

window.location标识当前所在网址，可以通过给这个属性赋值命令浏览器进行页面跳转

## 二.面向对象

### 2.1认识对象

对象(object)是“键值对”的集合，表示属性和值的映射关系

### 2.2对象的方法

如果某个属性值是函数，则它也被称为对象的“方法”

![image-20210403110938259](https://gitee.com/cao_ziqiang/img/raw/master/image-20210403110938259.png)

### 2.3对象的遍历

和遍历数组类似，对象也可以被遍历，遍历对象需要使用for. . . in...循环

使用for. . .in...循环可以遍历对象的每个键

![image-20210403111054993](https://gitee.com/cao_ziqiang/img/raw/master/image-20210403111054993.png)

### 2.4对象的深浅克隆

浅克隆:只克隆对象的“表层”，如果对象的某些属性值又是引用类型值，则不进一步克隆它们，只是传递它们的引用

使用for. . .in.. .循环即可实现对象的浅克隆

demo:

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script>
        var obj1 = {
            a: 1,
            b: 2,
            c: [44,55,66]
        };
        var obj2 = {};
        for(k in obj1) {
            obj2[k] = obj1[k];
        }
        obj1.c.push(77);
        console.log(obj2);
    </script>
</body>
</html>
```

![image-20210403111622864](https://gitee.com/cao_ziqiang/img/raw/master/image-20210403111622864.png)

可以看到只是浅克隆,这时对obj1中的引用类型修改,obj2中的类型也会修改

深克隆:克隆对象的全貌，不论对象的属性值是否又是引用类型值，都能将它们实现克隆

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script>
        var obj1 = {
            a: 1,
            b: 2,
            c: [33,44,{
                m: 55,
                n: 66,
                p: [77,88,99]
            }]
        };
        function deepClone(o) {
            if(Array.isArray(o)) {
                var result = [];
                for(var i = 0;i<o.length;i++) {
                    result.push(deepClone(o[i]));
                }
            } else if (typeof o == 'object') {
                var result = {};
                for(var k in o) {
                    result[k] = deepClone(o[k]);
                }
            } else {
                var result = o;
            }
            return result;
        }
        var obj2 = deepClone(obj1);
        console.log(obj2);
    </script>
</body>
</html>
```

![image-20210403112413053](https://gitee.com/cao_ziqiang/img/raw/master/image-20210403112413053.png)

### 2.5上下文

函数中可以使用this关键字，它表示函数的上下文;

与中文中“这”类似，函数中的this具体指代什么必须通过调用函数时的“前言后语”来判断;

同一个函数，用不同的形式调用它，则函数的上下文不同;

函数如果不调用，则不能确定函数的上下文;

规则1:对象打点调用它的方法函数，则函数的上下文是这个打点的对象;

规则2:圆括号直接调用函数，则函数的上下文是window对象;

规则3:数组（类数组对象)枚举出函数进行调用，上下文是这个数组（类数组对象);

什么是类数组对象:所有键名为自然数序列(从0开始)，且有length属性的对象

规则4: IIFE中的函数，上下文是window对象;

规则5︰定时器、延时器调用函数，上下文是window对象;

规则6:事件处理函数的上下文是绑定事件的DOM元素;

call和apply能指定函数的上下文

![image-20210403121416256](https://gitee.com/cao_ziqiang/img/raw/master/image-20210403121416256.png)

![image-20210403121508098](https://gitee.com/cao_ziqiang/img/raw/master/image-20210403121508098.png)

### 2.6用new操作符调用函数

![image-20210403122219788](https://gitee.com/cao_ziqiang/img/raw/master/image-20210403122219788.png)

### 2.7构造函数

用new调用一个函数，这个函数就被称为“构造函数”，任何函数都可以是构造函数，只需要用new调用它

顾名思义，构造函数用来“构造新对象”，它内部的语句将为新对象添加若干属性和方法，完成对象的初始化

构造函数必须用new关键字调用，否则不能正常工作，正因如此，开发者约定构造函数命名时首字母要大写Java、C++等是“面向对象”( object-oriented)语言

JavaScript是“基于对象”( object-based）语言

JavaScript中的构造函数可以类比于OO语言中的“类”,写法的确类似，但和真正OO语言还是有本质不同

### 2.8protype

任何函数都有prototype属性,prototype是英语“原型”的意思

prototype属性值是个对象，它默认拥有constructor属性指回函数

![image-20210403134515066](https://gitee.com/cao_ziqiang/img/raw/master/image-20210403134515066.png)

