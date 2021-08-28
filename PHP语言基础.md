---
title: PHP语言基础
date: 2021-03-22 13:16:56
categories: PHP
tags: [PHP,backend]
---

## 一.PHP的数据类型

php中包括有String(字符串),Integer(整型),Float(浮点型),Boolean(布尔型),Array(数组),

Objec(对象),NULL(空值)7中

### 1.1字符串

一个字符串就是一串字符的序列,比如"Hello world!"

我们可以将任何文本都放在单引号和双引号中最为字符串使用

采用单引号时对引号内的值不识别，直接输出

```php
<? php
    $x = "Hello World!";//使用双引号定义字符串
	echo $x;
	echo "<br/>";
	$y = 'Hello World';//使用单引号
	echo $y;
	$z = '马老师,发生肾么事了?';//汉字也是字符串
	echo $z;
?>
```

### 1.2整形

整型数据只能包括整数

- 整数数据必须至少有一个数字(0~9)
- 整数数据不能包含有逗号或者空格
- 整数数据没有小数点
- 整数数据可以是整数也可以是负数

整数数据可以用3中格式来指定,即十进制,十六进制(以0x为前缀)和八进制(以0作为前缀)

以下示例使用了PHP中的var_dump()函数,该函数可以返回变量的值和类型

```php
<? php
	$x = 5985;//正数
	var_dump($x);
	echo '<br/>';

	$y = -5985;//负数
	var_dump($y);
	echo '<br/>';
	$x = 0x8C;//十六进制
	var_dump($x);
	echo '<br/>';
	$x = 047;//八进制
	var_dump($x);
	echo '<br/>';
?>
```

注意在PHP7中,含有十六进制字符的字符串将不再被视为数字,而是会被当成是普通的字符串

### 1.3浮点型

浮点数数据既可以用来存储整数,也可以用来存储小数和指数

```php
<? php
	$x = 10.365;
	var_dump($x);//float(10.365);
	$x = 2.4e3;
	var_dump($x);//float(2400);
	$x = 8E-5;
	var_dump($x);//float(8.0E-5)
?>
```

### 1.4布尔型

布尔型数据只有两个,即True和False,是用来表示"是"和"非"的概念

```php
<? php
	$x = true;
	$y = false;
?>
```

### 1.5数组

数组是一组数据的集合,是将数据按照一点的规则组织起来的一个整体

数组的本质是存储管理和操作一组变量

```php
<? php
	$cars = array("Volvo","BMW" => array('Z4','X7'),"Toyata");
	var_dump($cars);
?>
```

输出结果如下

```php
array(3) 
{
	[0] => string(5) "Volvo"
	["BMW"] => array(2)
	{
		[0] => string(2) "Z4"
		[1] => string(2) "X7"
	}
	[1] => string(6) "Toyota"
}
```

$cars数组中的元素包括有字符串和子数组,var_dump()将数组以键值对的形式输出

如果没有给数组的索引起别名,那么数组默认的索引从0递增

### 1.6对象

对象数据类型也可以用来存储数据,在PHP中,对象必须先声明,需要使用class关键字声明对象,然后再类中定义数据类型,再实例化一个类的对象出来使用

```php
<? php
    Class Car//使用class声明一个类对象
    {
        var $color;
        function set_color($color = "green") {
            $this->color = $color;
        }
        function get_color() {
            return $this->color;
        }
    }
	$car = new Car();
	$car->set_color('red');
	echo $car->get_color();
?>
```

### 1.7NULL值

NULL值表示变量没有值,NULL是数据类型为NULL的值,指明一个变量是否为空值,同样可以用于数据空值和NULL值的区别,可以通过设置变量值为NULL来清空变量数据

```php
<? php
	$x = "Hello,World";
	var_dump($x);
	
	$x = NULL;
	var_dump($x);
?>
```



