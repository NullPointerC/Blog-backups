---
title: Go语言学习-6-数组
categories: [Go]
tags:
  - backend
  - Go
date: 2021-10-13 21:50:58
---

Go语言中的数组是指同一种数据类型的元素的集合，这一点和其他许多静态语言一样。在Go语言中，数组从声明时就确定了大小，使用时可以修改成员，但是大小不可变。

```go
// 声明一个长度为3且元素数据类型为int的数组a
var a [3]int
```

## 数组定义

```go
var 数组变量名 [元素数量]T
```

数组可以通过下标进行访问，下标是从`0`开始，最后一个元素下标是：`len-1`，访问越界（下标在合法范围之外），则触发访问越界，会panic。

## 数组初始化

初始化数组时可以使用初始化列表来设置数组元素的值。

```go
func f1() {
	var testArray [3]int                        //数组会初始化为int类型的零值
	var numArray = [3]int{1, 2}                 //使用指定的初始值完成初始化
	var cityArray = [3]string{"北京", "上海", "深圳"} //使用指定的初始值完成初始化
	fmt.Println(testArray)                      //[0 0 0]
	fmt.Println(numArray)                       //[1 2 0]
	fmt.Println(cityArray)                      //[北京 上海 深圳]
}
```

按照上面的方法每次都要确保提供的初始值和数组长度一致，一般情况下我们可以让编译器根据初始值的个数自行推断数组的长度，例如：

```go
func f2() {
	var testArray [3]int
	var numArray = [...]int{1, 2}
	var cityArray = [...]string{"北京", "上海", "深圳"}
	fmt.Println(testArray)
	fmt.Println(numArray)
	fmt.Println(cityArray)
}
```

我们还可以使用指定索引值的方式来初始化数组，例如:

```go
func f3() {
	a := [...]int{1: 1, 3: 5}
	fmt.Println(a)                  // [0 1 0 5]
	fmt.Printf("type of a:%T\n", a) //type of a:[4]int
}
```

## 数组遍历

Go语言中可以使用`for`循环来遍历数组,也可以使用内置语法`for range`语法遍历数组。

```go
func f4() {
	a := [...]string{"北京", "上海", "深圳"}
	// for循环
	for i := 0; i < len(a); i++ {
		fmt.Print(a[i] + "\t")
	}
	fmt.Println()
	// for range
	for index, value := range a {
		fmt.Printf("%v -> %v\t", index, value)
	}
	fmt.Println()
}
```

## 多维数组

Go语言也像其他语言一样支持多维数组。

### 定义

```go
func f5() {
	a := [3][2]string{
		{"北京", "上海"},
		{"广州", "深圳"},
		{"成都", "重庆"},
	}
	fmt.Println(a)
	fmt.Println(a[2][1])
}
```

### 遍历

```go
func f6() {
	a := [3][2]string{
		{"北京", "上海"},
		{"广州", "深圳"},
		{"成都", "重庆"},
	}
	// for循环
	for i := 0; i < len(a); i++ {
		for j := 0; j < len(a[i]); j++ {
			fmt.Printf("%v\t", a[i][j])
		}
		fmt.Println()
	}
	fmt.Println("----------------")
	// for range
	for _, v1 := range a {
		for _, v2 := range v1 {
			fmt.Printf("%v\t", v2)
		}
		fmt.Println()
	}
}
```

在多维数组中，只有第一维可以使用`...`让编译器来推导数组长度。

## 数组是值类型

在Go语言中，数组是值类型，赋值和传参都是复制整个数组过去，因此改变副本的值，并不会引起本身的改变。

```go
func f7(x [3]int) {
	x[0] = 100
}

func main() {
	a := [...]int{1, 2, 3}
	fmt.Println(a)
	f7(a)
	fmt.Println(a)
}
```

可以在终端看到$a$的值仍为$[1, 2, 3]$。

![image-20211014190330434](https://gitee.com/cao_ziqiang/img/raw/master/20211014190330.png)

1. 数组支持 “==“、”!=” 操作符，因为内存总是被初始化过的。
2. `[n]*T`表示指针数组，`*[n]T`表示数组指针 。

