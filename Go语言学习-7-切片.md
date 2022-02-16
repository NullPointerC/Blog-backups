---
title: Go语言学习-7-切片
categories:
  - Go
tags:
  - backend
  - Go
abbrlink: 7ebd31ff
date: 2021-10-14 19:13:36
---

由于Go语言中数组的长度是固定的并且数组的长度是数组的属性的一部分,所以在使用上会有很多的限制。

例如，我们已经编写了一个求和函数如下：

```go
func arraySum(x [3]int) int{
    sum := 0
    for _, v := range x{
        sum = sum + v
    }
    return sum
}
```

这个函数只支持`[3]int`类型，就有很大的局限性。

## 定义

切片（Slice）是一个拥有相同类型元素的可变长度的序列。它是基于数组类型做的一层封装。它非常灵活，支持自动扩容。

切片是一个引用数据类型，它的内部包含地址，长度和容量。一般用于快速地操作一块数据集合。

声明切片的基本语法如下：

```go
var name []T
```

- name:表示变量名
- T:表示切片中的元素类型

可以从如下代码看到切片与数组的不同之处：

```go
func f1() {
	// 声明切片类型
	var a []string              // 声明一个字符串切片
	var b = []int{}             // 声明一个整型切片
	var c = []bool{false, true} // 声明一个布尔切片
	var d = []bool{false, true} // 声明一个布尔切片
	fmt.Printf("%T,%v\n", a, a)
	fmt.Printf("%T,%v\n", b, b)
	fmt.Printf("%T,%v\n", c, c)
	fmt.Printf("%T,%v\n", d, d)
	fmt.Println(a == nil)
	fmt.Println(b == nil)
	fmt.Println(c == nil)
	fmt.Println(d == nil)
	// 声明数组类型
	var a1 [5]string
	var b1 [5]int
	var c1 [2]bool
	var d1 [2]bool
	fmt.Printf("%T,%v\n", a1, a1)
	fmt.Printf("%T,%v\n", b1, b1)
	fmt.Printf("%T,%v\n", c1, c1)
	fmt.Printf("%T,%v\n", d1, d1)
	// fmt.Println(a1 == nil)
	// fmt.Println(b1 == nil)
	// fmt.Println(c1 == nil)
	// fmt.Println(d1 == nil)
}
```

![image-20211014192645339](https://gitee.com/cao_ziqiang/img/raw/master/20211014192645.png)

### 长度和容量

切片拥有自己的长度和容量，我们可以通过使用内置的`len()`函数求长度，使用内置的`cap()`函数求切片的容量。

### 切片表达式

切片表达式从字符串、数组、指向数组或切片的指针构造子字符串或切片。它有两种变体：一种指定low和high两个索引界限值的简单的形式，另一种是除了low和high索引界限值外还指定容量的完整的形式。

#### 简单切片表达式

切片的底层就是一个数组，所以我们可以基于数组通过切片表达式得到切片。 切片表达式中的`low`和`high`表示一个索引范围（左包含，右不包含），也就是下面代码中从数组a中选出`1<=索引值<4`的元素组成切片s，得到的切片`长度=high-low`，容量等于得到的切片的底层数组的容量。

```go
func f2() {
	a := [5]int{1, 2, 3, 4, 5}
	s := a[1:3] // s := a[low:high]
	fmt.Printf("s:%v len(s):%v cap(s):%v\n", s, len(s), cap(s))
}
```

输出的如下结果：

![image-20211014192849291](https://gitee.com/cao_ziqiang/img/raw/master/20211014192849.png)

为了方便起见，可以省略切片表达式中的任何索引。省略了`low`则默认为0；省略了`high`则默认为切片操作数的长度:

```go
func f3() {
	a := [5]int{1, 2, 3, 4, 5}
	s := a[1:3] // s := a[low:high]
	b := a[2:]  // 等同于 a[2:len(a)]
	c := a[:3]  // 等同于 a[0:3]
	d := a[:]   // 等同于 a[0:len(a)]
	fmt.Printf("s:%v len(s):%v cap(s):%v\n", s, len(s), cap(s))
	fmt.Printf("b:%v len(b):%v cap(b):%v\n", b, len(b), cap(b))
	fmt.Printf("c:%v len(c):%v cap(c):%v\n", c, len(c), cap(c))
	fmt.Printf("d:%v len(d):%v cap(d):%v\n", d, len(d), cap(d))
}
```

![image-20211014193335549](https://gitee.com/cao_ziqiang/img/raw/master/20211014193335.png)

对于数组或字符串，如果`0 <= low <= high <= len(a)`，则索引合法，否则就会索引越界（out of range）。

对切片再执行切片表达式时（切片再切片），`high`的上限边界是切片的容量`cap(a)`，而不是长度。**常量索引**必须是非负的，并且可以用int类型的值表示;对于数组或常量字符串，常量索引也必须在有效范围内。如果`low`和`high`两个指标都是常数，它们必须满足`low <= high`。如果索引在运行时超出范围，就会发生运行时`panic`。

```go
func f4() {
	a := [5]int{1, 2, 3, 4, 5}
	s := a[1:3] // s := a[low:high]
	fmt.Printf("s:%v len(s):%v cap(s):%v\n", s, len(s), cap(s))
	s2 := s[3:4] // 索引的上限是cap(s)而不是len(s)
	fmt.Printf("s2:%v len(s2):%v cap(s2):%v\n", s2, len(s2), cap(s2))
}
```

![image-20211014194023644](https://gitee.com/cao_ziqiang/img/raw/master/20211014194023.png)

#### 完整切片表达式

对于数组，指向数组的指针，或切片a(**注意不能是字符串**)支持完整切片表达式：

```go
a[low : high : max]
```

上面的代码会构造与简单切片表达式`a[low: high]`相同类型、相同长度和元素的切片。另外，它会将得到的结果切片的容量设置为`max-low`。在完整切片表达式中只有第一个索引值（`low`）可以省略；它默认为0。

```go
func f5() {
	a := [5]int{1, 2, 3, 4, 5}
	t := a[1:3:5]
	fmt.Printf("t:%v len(t):%v cap(t):%v\n", t, len(t), cap(t))
}
```

![image-20211014194322483](https://gitee.com/cao_ziqiang/img/raw/master/20211014194322.png)

## make()函数构造切片

除了可以使用数组创建切片外，还可以使用`make`函数来创建切片。

```go
make([]T, size, cap)
```

- T:切片的元素类型
- size:切片中元素的数量
- cap:切片的容量

```go
func f6() {
	a := make([]int, 2, 10)
	fmt.Println(a)      //[0 0]
	fmt.Println(len(a)) //2
	fmt.Println(cap(a)) //10
}
```

上述切片`a`的存储空间分配了10个，但是只使用其中的2个。

## 切片的本质

切片的本质就是对底层数组的封装，它包含了三个信息：底层数组的指针、切片的长度（len）和切片的容量（cap）。

举个例子，现在有一个数组`a := [8]int{0, 1, 2, 3, 4, 5, 6, 7}`，切片`s1 := a[:5]`，相应示意图如下。

![slice_01](https://gitee.com/cao_ziqiang/img/raw/master/20211014194637.png)

切片`s2 := a[3:6]`，相应示意图如下：

![slice_02](https://gitee.com/cao_ziqiang/img/raw/master/20211014194700.png)



并且切片之间不能相互比较，比较切片比较合法的操作是和`nil`比较。一个`nil`值的切片并没有使用底层数组，其长度和容量都为0。但是不能说一个长度和容量都是0的切片一定是`nil`。所以要判断切片为空，要使用`len(s)==0`来判断。

```go
func f7() {
	var s1 []int
	s2 := []int{}
	s3 := make([]int, 0)
	fmt.Printf("len(s1)=%d,cap(s1)=%d,s1==nil=>%v\n", len(s1), cap(s1), s1 == nil)
	fmt.Printf("len(s2)=%d,cap(s2)=%d,s2==nil=>%v\n", len(s2), cap(s2), s2 == nil)
	fmt.Printf("len(s3)=%d,cap(s3)=%d,s3==nil=>%v\n", len(s3), cap(s3), s3 == nil)
}
```

![image-20211014195417549](https://gitee.com/cao_ziqiang/img/raw/master/20211014195419.png)

输出结果如上表示，即使不为为`nil`切片，但是它的`len`和`cap`都可能为0。

## 切片的拷贝

切片和数组不一样的地方在于切片是地址传递，所以当一个切片拷贝了另一个切片后，对一个切片的修改会影响到另外的一个切片的内容。

```go
func f8() {
	s1 := make([]int, 3)
	s2 := s1
	s2[0] = 100
	fmt.Println(s1)
	fmt.Println(s2)
}
```

![image-20211014195852068](https://gitee.com/cao_ziqiang/img/raw/master/20211014195852.png)

## 切片的遍历

切片的遍历方式和数组是一致的，支持索引遍历和`for range`遍历。

```go
func f9() {
	s := []int{1, 3, 5}
	for i := 0; i < len(s); i++ {
		fmt.Println(i, s[i])
	}
	fmt.Println("----------")
	for index, value := range s {
		fmt.Println(index, value)
	}
}
```

## 切片的常用方法

### append()

Go语言的内建函数`append()`可以为切片动态添加元素。 可以一次添加一个元素，可以添加多个元素，也可以添加另一个切片中的元素（后面加…）。

```go
func f10() {
	var s []int
	s = append(s, 1) // [1]
	fmt.Printf("s => %v\n", s)
	s = append(s, 2, 3, 4) // [1 2 3 4]
	fmt.Printf("s => %v\n", s)
	s2 := []int{5, 6, 7}
	s = append(s, s2...) // [1 2 3 4 5 6 7]
	fmt.Printf("s => %v\n", s)
}
```

每个切片会指向一个底层数组，这个数组的容量够用就添加新增元素。当底层数组不能容纳新增的元素时，切片就会自动按照一定的策略进行“扩容”，此时该切片指向的底层数组就会更换。“扩容”操作往往发生在`append()`函数调用时，所以我们通常都需要用原变量接收append函数的返回值。

举个栗子:

```go
func f12() {
	//append()添加元素和切片扩容
	var numSlice []int
	for i := 0; i < 10; i++ {
		numSlice = append(numSlice, i)
		fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", numSlice, len(numSlice), cap(numSlice), numSlice)
	}
}
```

![image-20211014201140419](https://gitee.com/cao_ziqiang/img/raw/master/20211014201140.png)

从上面的结果可以看出：

1. `append()`函数将元素追加到切片的最后并返回该切片。
2. 切片numSlice的容量按照1，2，4，8，16这样的规则自动进行扩容，每次扩容后都是扩容前的2倍。

### 切片扩容机制

- 首先判断，如果新申请容量（cap）大于2倍的旧容量（old.cap），最终容量（newcap）就是新申请的容量（cap）。
- 否则判断，如果旧切片的长度小于1024，则最终容量(newcap)就是旧容量(old.cap)的两倍，即（newcap=doublecap），
- 否则判断，如果旧切片长度大于等于1024，则最终容量（newcap）从旧容量（old.cap）开始循环增加原来的1/4，即（newcap=old.cap,for {newcap += newcap/4}）直到最终容量（newcap）大于等于新申请的容量(cap)，即（newcap >= cap）
- 如果最终容量（cap）计算值溢出，则最终容量（cap）就是新申请容量（cap）。

### copy()

当需要拷贝切片时,如果直接复制,那么新的切片和原本的切片指向的是同一块内存地址,修改新的切片那么原先的值也会发生改变。

Go语言提供了`copy()`函数来将一个切片的数据复制到另一个切片空间中。

```go
copy(destSlice, srcSlice []T)
```

- srcSlice: 数据来源切片
- destSlice: 目标切片

```go
func f13() {
	a := []int{1, 2, 3, 4, 5}
	c := make([]int, 5, 5)
	copy(c, a)
	fmt.Println(a)
	fmt.Println(c)
	c[0] = 1000
	fmt.Println(a)
	fmt.Println(c)
}
```

### 切片删除元素

Go语言中并没有删除切片元素的专用方法，我们可以使用切片本身的特性来删除元素。 代码如下：

```go
func f14() {
	// 从切片中删除元素
	a := []int{30, 31, 32, 33, 34, 35, 36, 37}
	// 要删除索引为2的元素
	a = append(a[:2], a[3:]...)
	fmt.Println(a) //[30 31 33 34 35 36 37]
}
```

总结一下就是：要从切片a中删除索引为`index`的元素，操作方法是`a = append(a[:index], a[index+1:]...)`

