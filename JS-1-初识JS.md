---
title: JS-1.初识JS
date: 2021-03-31 21:24:46
categories: JavaScript
tags: [frontend,JS]
---

## 一.JS简单介绍

![image-20210331212725923](https://gitee.com/cao_ziqiang/img/raw/master/image-20210331212725923.png)

![image-20210331212813464](https://gitee.com/cao_ziqiang/img/raw/master/image-20210331212813464.png)

![image-20210331213347141](https://gitee.com/cao_ziqiang/img/raw/master/image-20210331213347141.png)

![image-20210331213555515](https://gitee.com/cao_ziqiang/img/raw/master/image-20210331213555515.png)

## 二.JS的基础语法

![image-20210331213653429](https://gitee.com/cao_ziqiang/img/raw/master/image-20210331213653429.png)

JS不能脱离HTML网页运行(但NodeJS使得JS能够成为一个独立的运行平台)

demo:

```html
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
        alert('你好,JavaScript!');
    </script>
</body>
</html>
```

![image-20210331214110492](https://gitee.com/cao_ziqiang/img/raw/master/image-20210331214110492.png)

我们看到浏览器弹出弹窗,同时可以发现左上角页面处于加载状态,说明浏览器在加载JavaScript时会等待alert()执行完成

demo:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script src="my.js"></script>
</body>
</html>
```

<hr/>

```javascript
alert('你好2,JavaScript!2');
```

### 2.1输出语句

- alert()	弹出警告框

- console.log()	控制台输出

### 2.2注释

单行注释 //alert('你好世界');

多行注释: /* */

文档注释: /** */

### 2.3变量

变量是计算机中能够储存值和计算结果的抽象概念

#### 2.3.1定义变量:

`var a = xx;`

使用`var`关键字进行定义变量

<hr/>

#### 2.3.2合法命名:

由字母,数字,下划线,$组成,但是不能以数字开头;

不能是关键字或者保留字;

变量名大小写敏感;

<hr/>

#### 2.3.3变量命名法:

驼峰命名法:第一个单词全小写,后单词首字母大写

C风格命名法:每个单词全小写,单词之间以'_'进行分隔

匈牙利命名法:iMathTestScore,首字母i表示变量类型,其他字母首字母大写

<hr/>

变量的默认值:一个变量只有被定义,但是没有赋初值,默认值是`undefined`

<hr/>

变量常见错误:

不用var定义,就直接将值赋给他,虽然不会引起报错,但是会引起变量的作用域问题;

尝试使用一个既没有被var,也没有被赋值的变量,就会引起引用错误;

<hr/>

#### 2.3.4变量声明提升:

可以使用一个稍后才声明的变量,而不会引发异常;

在执行代码前,JS会进行预解析阶段,会预读所有的变量的定义;

**但是变量声明提升只会提升定义,不会提升值**

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
        console.log(a);
        var a = 123;
        console.log(a);
    </script>
</body>
</html>
```

![image-20210331222139455](https://gitee.com/cao_ziqiang/img/raw/master/image-20210331222139455.png)

我们可以看到虽然第一次console.log(a)没有报引用错误,但是却是`undefined`

第二次才输出了a的真实值

变量声明提升只有在JS中才有,如果在C++,Java等语言中,这样使用则编译器会报错;

在实际开发中不要刻意使用变量提升的特性,一定要先定义再去给变量赋值;

<hr/.

### 2.4JS数据类型

`typeof运算符`,可以检测出变量或者值的类型

#### 2.4.1基本数据类型

Number,String,Boolean,Undefined,Null

![image-20210331223005239](https://gitee.com/cao_ziqiang/img/raw/master/image-20210331223005239.png)

##### 数字类型(Number):

所有数字不分大小,不分整浮,不分正负,都是数字类型

一个特殊的数字类型值NaN

NaN是英语"not a number"的意思,即"不是一个数",但是它是一个数字类型的值

例如0/0的结果就是NaN,事实上,在数学运算中,结果不能得到数字,往往都是NaN

NaN具有**不自等性**

<hr/>

##### 字符串类型(String):

字符串就是人类的语言;

字符串要用引号包裹,双引号或单引号都可以;

加号可以用于拼接多个字符串;

字符串的length属性表示字符串的长度;

字符串常用方法:

![image-20210401094341614](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401094341614.png)

substring(a,b);

从索引a开始截取到索引b-1的字符串,若省略b则默认到结尾

substr(a,b);

从索引a开始截取长度为b的字符串,若省略b,则默认到结尾;

同时支持从结尾-1做索引开始截取

slice();

从a开始到b结束(不包括b)的子串;

slice的参数可以是负数;

在slice中a的值必须比b的值小;

<hr/>

##### 布尔类型(Boolean):

布尔值只有两个,true和false,分别表示真和假

<hr/.

##### Undefined类型(Undefined):

一个没有被赋值的变量默认值是undefined,而undefined的类型也是undefined

**即undefined即是类型又是值,这种类型只有他自己这一种值**

demo:

```javascript
console.log(a);
console.log(typeof a);
var a = 10;
```

<hr/>

##### null类型(Object):

null表示空,即空对象;

当我们需要将对象销毁,数组销毁或者删除监听事件时,常常将他们设置为null

<hr/>

##### 数据类型的转换:

其他值->数字:

- 使用Number()函数

将其他类型转为整数,如果无法转换则会转换成NaN

```js
Number('123');//123
Number('123.4');//123.4
Number('123年');/NaN
Number('2e3');/2000
Number('');//0
Number(true);//1
Number(false);//0
Number(undefined);//NaN
Number(null);//0
```

- 使用parseInt()函数

将字符串转为整数,但是parseInt会自动截取掉第一个非数字字符后所有的字符

```js
parseInt('3.14');//3
parseInt('3.14是圆周率');//3
parseInt('圆周率是3.14');//NaN
parseInt('3.99');//3
```

- 使用parseFloat()函数

将字符串转为浮点数,会自动截取掉第一个非数字字符,非小数点后的所有字符

```js
parseFloat('3.14');//3.14
parseFloat('3.14是圆周率');//3.14
parseFloat('圆周率是3.14');//NaN
parseFloat('3.99');3.99
```

其他值->字符串

- 使用String()函数

```js
String(123);//'123'
String(123.4);//'123.4'
String(2e3);//2000
String(NaN);//'NaN'
String(Infinity);//'Infinity'
String(0xf);//'15'
String(true);//'true'
String(false);//'false'
String(undefined);//'undefined'
String(null);//'null'
```

- toString()方法

几乎所有值都有toString()方法,功能就是转为字符串

- Boolean()函数

其他值转为布尔值

```js
Boolean(123);//true
Boolean(0);//false
Boolean(NaN);//false
Boolean(Infinity);//true
Boolean(-Infinity);//true
Boolean('');//false
Boolean('abc');//true
Boolean('false');//true
Boolean(undefined);//false
Boolean(null);//false
```



#### 2.4.2复杂数据类型

Object,Array,Function,RegExp,Date,Map,Set,Symbol等等

复杂数据类型都是引用类型

### 2.5表达式与操作符

#### 2.5.1算术运算符

![image-20210401110329750](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401110329750.png)

#### 2.5.2关系运算符

![image-20210401110554912](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401110554912.png)

两个等号==的运算符不比较值的类型,他会进行隐式转换后比较值是否相等;

三个等会===的运算符,不仅会比较值是否相同,还会比较类型是否相同

![image-20210401110809370](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401110809370.png)

![image-20210401110848647](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401110848647.png)

#### 2.5.3逻辑运算符

![image-20210401111027151](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401111027151.png)

#### 2.5.4赋值运算符

![image-20210401111122092](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401111122092.png)

