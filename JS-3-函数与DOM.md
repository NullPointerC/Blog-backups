---
title: JS-3.函数与DOM
date: 2021-04-01 12:20:26
categories: JavaScript
tags: [frontend,JS]
---

## 一.函数

### 1.1函数的定义

和变量类似，函数必须先定义然后才能使用

使用function关键字定义函数，function是“功能”的意思

![image-20210401122351962](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401122351962.png)

<hr/>

![image-20210401122426836](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401122426836.png)

### 1.2函数的调用

执行函数体中的所有语句，就称为“调用函数”;

调用函数非常简单，只需在函数名字后书写圆括号对即可

### 1.3函数声明的提升

和变量声明提升类似，函数声明也可以被提升

```js
fun();
function fun() {
	console.log('函数被执行!');
}
```

![image-20210401122843505](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401122843505.png)

但是函数表达式不能够被提升

![image-20210401122920825](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401122920825.png)

<hr/>

![image-20210401132322433](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401132322433.png)

### 1.4函数的参数

参数是函数内的一些待定值，在调用函数时，必须传入这些参数的具体值

函数的参数可多可少，函数可以没有参数，也可以有多个参数，多个参数之间需要用逗号隔开

### 1.5arguments

函数内arguments表示它接收到的实参列表，它是一个类数组对象

类数组对象:所有属性均为从0开始的自然数序列，并且有length属性，和数组类似可以用方括号书写下标访问对象的某个属性值，但是不能调用数组的方法

### 1.6函数返回值

函数体内可以使用return关键字表示“函数的返回值
		调用一个有返回值的函数，可以被当做一个普通值，从而可以出现在任何可以书写值的地方

调用函数时，一旦遇见return语句则会立即退出函数将执行权交还给调用者

### 1.7递归

函数的内部语句可以调用这个函数自身，从而发起对函数的一次迭代。在新的迭代中，又会执行调用函数自身的语句，从而又产生一次迭代。当函数执行到某一次时，不再进行新的迭代，函数被一层一层返回，函数被递归。

递归是一种较为高级的编程技巧，它把一个大型复杂的问题层层转化为一个与原问题相似的规模较小的问题来求解。

边界条件:确定递归到何时终止，也称为递归出口

递归模式:大问题是如何分解为小问题的，也称为递归体

### 1.8实现深克隆

使用var arr2 = arr1这样的语句不能实现数组的克隆

浅克隆:准备一个空的结果数组，然后使用for循环遍历原数组，将遍历到的项都推入结果数组

浅克隆只克隆数组的一层，如果数组是多维数组，则克隆的项会“藕断丝连”。

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
        var arr1 = [33,44,55,[66,77]];
        //准备一个数组
        var result = [];
        for(var i = 0;i<arr1.length;i++) {
            result.push(arr1[i]);
        }
        //但是这样的拷贝只能拷贝第一层,被称为浅拷贝,同时还存在藕断丝连现象
        arr1[3].push(88);
        console.log(result);
    </script>
</body>
</html>
```

输出结果:

![image-20210401155600075](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401155600075.png)



使用递归思想，整体思路和浅克隆类似，但稍微进行一些改动:如果遍历到项是基本类型值，则直接推入结果数组;如果遍历到的项是又是数组，则重复执行浅克隆的操作

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
        //原数组
        var arr1 = [33,44,55,66,[77,88,[33,22,11]]];
        //函数
        function deepClone(arr) {
            var result = [];
            for(var i = 0;i<arr.length;i++) {
                //类型判断,如果又遍历到了数组
                if(Array.isArray(arr[i])) {
                    //那么继续递归
                    result.push(deepClone(arr[i])); 
                }else {
                    //递归的出口,即遍历到了基本类型
                    result.push(arr[i]);
                }
            }
            //返回结果数组
            return result;
        }

        var arr2 = deepClone(arr1);
        console.log(arr2);
        //测试是否藕断丝连
        console.log(arr1[4] == arr2[4]);
        arr1.push(100);
        console.log(arr2);
</script>
</body>
</html>
```

![image-20210401160855924](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401160855924.png)

## 二.变量及作用域

### 2.1全局变量和局部变量

JavaScript是函数级作用域编程语言:变量只在其定义时所在的function内部有意义。

![image-20210401161210223](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401161210223.png)

变量a是在fun函数中被定义的，所以变量a只在fun丞数内部有定义，fun逃数就是a的“作用域”
变量a被称为局部变量。

如果不将变量定义在任何函数的内部，此时这个变量就是全局变量，它在任何函数内都可以被访问和更改。

### 2.2遮蔽效应

如果函数中也定义了和全局同名的变量，则函数内的变量会将全局的变量“遮蔽”

```js
var a = 10;
function fun() {
	var a = 5;
	a++;
	console.log(a);//输出6
}
fun();
console.log(a);//输出10
```



注意变量提升的影响

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
        var a = 10;
        function fun() {
            a++;//此时a还是一个局部变量,值为undefined,自增结果为NaN
            var a = 5;//执行到这里又被重新赋值为5
            console.log(a);//输出5
        }
        fun();
        console.log(a);//输出10
    </script>
</body>
</html>
```

### 2.3形参也是局部变量

![image-20210401162127809](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401162127809.png)

### 2.4作用域链

函数的嵌套:一个函数内部也可以定义一个函数。和局部变量类似，定义在一个函数内部的函数是局部函数。

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
        function fun() {
            function inner () {
                console.log('这是内部局部函数');
            }
            inner();
            console.log('这是外部函数!');
        }
        fun();
    </script>
</body>
</html>
```

作用域链:在函数嵌套中，变量会从内到外逐层寻找它的定义

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
        var a = 10;
        var b = 20;
        function fun() {
           var c = 30;
           function inner() {
               var a = 40;
               var d = 50;
               console.log(a,b,c,d);
           }
           inner();
        }
        fun();
    </script>
</body>
</html>
```



![image-20210401163408738](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401163408738.png)

不加var将会定义全局变量

demo:

```js
function fun() {
	a = 3;
}
fun();
console.log(a);//3
```

### 2.5闭包

JavaScript中函数会产生闭包(closure)。闭包是函数本身和该函数声明时所处的环境状态的组合。

![image-20210401164501422](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401164501422.png)

函数能够“记忆住”其定义时所处的环境，即使函数不在其定义的环境中被调用，也能访问定义时所处环境的变量;

在JavaScript中，每次创建函数时都会创建闭包。;

但是，闭包特性往往需要将函数“换一个地方”执行，才能被观察出来;

闭包很有用，因为它允许我们将数据与操作该数据的函数关联起来。这与“面向对象编程”有少许相似之处;

闭包的功能:记忆性、模拟私有变量;

当闭包产生时，函数所处环境的状态会始终保持在内存中，不会在外层函数调用后被自动清除。这就是闭包的记忆性;

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
        function createCheckTemp(standardTemp) {
            function checkTemp(n) {
                if(n<=standardTemp) {
                    alert('体温正常');
                }else {
                    alert('体温异常');
                }
            }
            return checkTemp;
        }
        var checkTemp_A = createCheckTemp(37.3);
        checkTemp_A(37.2);
        checkTemp_A(37.4);
        var checkTemp_B = createCheckTemp(37.1);
        checkTemp_B(37.0);
        checkTemp_B(37.2);
    </script>
</body>
</html>
```

模拟私有变量:

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
        function fun() {
            //定义一个局部变量
            var a = 0;
            return {
                getA: function() {
                    return a;
                }
            }
        }
        var obj = fun();
        //如果想在外部使用到a,必须通过getA()方法
        console.log(obj.getA());
    </script>
</body>
</html>
```

不能滥用闭包，否则会造成网页的性能问题，严重时可能导致内存泄露。所谓内存泄漏是指程序中己动态分配的内存由于某种原因未释放或无法释放。

### 2.6立即执行函数IIFE

IlFE (lmmediately Invoked Function Expression立即调用函数表达式)是一种特殊的JavaScript函数写法一旦被定义，就立即被调用。

![image-20210401170407822](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401170407822.png)

为变量赋值:当给变量赋值需要一些较为复杂的计算时（如if语句)，使用lIFE显得语法更紧凑;

lIFE可以在一些场合（如for循环中）将全局变量变为局部变量，语法显得紧凑。

## 三.DOM

DOM (Document Object Model，文档对象模型)是JavaScript操作HTML文档的接口，使文档操作变得非常优雅、简便。

DOM最大的特点就是将文档表示为节点树。

![image-20210401171446484](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401171446484.png)

绿色是元素节点

红色是属性节点

蓝色是文本节点

![image-20210401194211106](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401194211106.png)

### 3.1访问元素结点

所谓“访问”元素节点，就是指“得到”、“获取”页面上的元素节点

对节点进行操作，第一步就是要得到它

访问元素节点主要依靠document对象

document对象是DOM中最重要的东西，几乎所有DOM的功能都封装在了document对象中;

document对象也表示整个HTML文档，它是DOM节点树的根

document对象的nodeType属性值是9



![image-20210401200034502](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401200034502.png)

<hr/>

![image-20210401200136234](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401200136234.png)

<hr/>

![image-20210401200432678](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401200432678.png)

数组方便遍历，从而可以批量操控元素节点

即使页面上只有一个指定标签名的节点，也将得到长度为1的数组

任何一个节点元素也可以调用getElementsByTagName()方法，从而得到其内部的某种类的元素节点

<hr/>

![image-20210401200617849](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401200617849.png)

getElementsByClassName()方法从IE9开始兼容

某个节点元素也可以调用getElementsByClassName()方法，从而得到其内部的某类名的元素节点

<hr/>

![image-20210401200700693](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401200700693.png)

querySelector()方法只能得到页面上一个元素，如果有多个元素符合条件，则只能得到第一个元素

querySelector()方法从IE8开始兼容，但从IE9开始支持CSS3的选择器，如:nth-child()、:[src^='dog']等CSS3选择器形式都支持良好

<hr/>

![image-20210401200906685](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401200906685.png)

<hr/>

### 3.2节点的关系

![image-20210401201021429](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401201021429.png)



![image-20210401201121487](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401201121487.png)

### 3.3节点操作

![image-20210401204354558](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401204354558.png)

<hr/>

![image-20210401204452094](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401204452094.png)

<hr/>

![image-20210401204624674](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401204624674.png)

<hr/>

![image-20210401204804131](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401204804131.png)

<hr/>

![image-20210401204909650](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401204909650.png)

新创建出的节点是“孤儿节点”，这意味着它并没有被挂载到DOM树上，我们无法看见它;

必须继续使用appendChild()或insertBefore()方法将孤儿节点插入到DOM树上

任何已经在DOM树上的节点，都可以调用appendChild()方法，它可以将孤儿节点挂载到它的内部，成为它的最后一个子节点;

父节点.appendChild(孤儿节点);

任何已经在DOM树上的节点，都可以调用insertBefore()方法，它可以将孤儿节点挂载到它的内部，成为它的“标杆子节点”之前的节点;

父节点.insertBefore(孤儿节点，标杆节点);

<hr/>

如果将已经挂载到DOM树上的节点成为appendChild()或者insertBefore()的参数，这个节点将会被移动;

新父节点.appendchild(已经有父亲的节点);

新父节点.insertBefore(已经有父亲的节点，标杆子节点);

这意味着一个节点不能同时位于DOM树的两个位置;

<hr/>

removeChild()方法从DOM中删除一个子节点

父节点.removeChild(要删除子节点);

节点不能主动删除自己，必须由父节点删除它;

<hr/>

cloneNode()方法可以克隆节点，克隆出的节点是“孤儿节点”;

var 孤儿节点=老节点.cloneNode();

var 孤儿节点=老节点.cloneNode(true);

参数是一个布尔值，表示是否采用深度克隆:如果为true，则该节点的所有后代节点也都会被克隆，如果为false，则只克隆该节点本身;

### 3.4事件监听

DOM允许我们书写JavaScript代码以让HTML元素对事件作出反应

什么是“事件”︰用户与网页的交互动作

当用户点击元素时

当鼠标移动到元素上时

当文本框的内容被改变时

当键盘在文本框中被按下时

当网页已加载完毕时

“监听”，顾名思义，就是让计算机随时能够发现这个事件发生了，从而执行程序员预先编写的一些程序

设置事件监听的方法主要有onxxx和addEventListener()两种

![image-20210401210750818](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401210750818.png)

<hr/>

![image-20210401211312523](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401211312523.png)

<hr/>

![image-20210401211510504](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401211510504.png)

<hr/>

![image-20210401211729141](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401211729141.png)

事件的传播

实际上，事件的传播是:先从外到内，然后再从内到外

![image-20210401212224389](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401212224389.png)

onxxx这样的写法只能监听冒泡阶段

![image-20210401212406214](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401212406214.png)

### 3.5事件对象

事件处理函数提供一个形式参数，它是一个对象，封装了本次事件的细节

这个参数通常用单词event或字母e来表示

![image-20210401213225432](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401213225432.png)

![image-20210401213354553](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401213354553.png)

![image-20210401223124922](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401223124922.png)

![image-20210401222319287](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401222319287.png)

![image-20210401223104871](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401223104871.png)

![image-20210401223139346](https://gitee.com/cao_ziqiang/img/raw/master/image-20210401223139346.png)

### 3.6事件委托

新增元素必须分别添加事件监听，不能自动获得事件监听

大量事件监听、大量事件处理函数都会产生大量消耗内存

事件委托:

利用事件冒泡机制，将后代元素事件委托给祖先元素

![image-20210402221050856](https://gitee.com/cao_ziqiang/img/raw/master/image-20210402221050856.png)

<hr/>

![image-20210402221302857](https://gitee.com/cao_ziqiang/img/raw/master/image-20210402221302857.png)

![image-20210402221329204](https://gitee.com/cao_ziqiang/img/raw/master/image-20210402221329204.png)

使用事件委托时要注意:不能委托不冒泡的事件给祖先元素

![image-20210402221544291](https://gitee.com/cao_ziqiang/img/raw/master/image-20210402221544291.png)

### 3.7定时器和延时器

![image-20210402221641257](https://gitee.com/cao_ziqiang/img/raw/master/image-20210402221641257.png)

第一个参数是函数(具名函数也可以传入setInterval)具名函数当做第一个参数，注意没有圆括号

第二个参数是间隔时间，以毫秒为单位，1000毫秒是1秒

setInterval()函数可以接收第3、4......个参数,它们将按顺序传入函数

![image-20210402221940558](https://gitee.com/cao_ziqiang/img/raw/master/image-20210402221940558.png)

![image-20210402222235165](https://gitee.com/cao_ziqiang/img/raw/master/image-20210402222235165.png)

![image-20210402222256110](https://gitee.com/cao_ziqiang/img/raw/master/image-20210402222256110.png)

![image-20210402222309192](https://gitee.com/cao_ziqiang/img/raw/master/image-20210402222309192.png)

