---
title: Core-Java学习笔记-11
date: 2021-07-21 16:00:40
categories: Java
tags: [Java,backend,计算机基础]
---

> CoreJava卷2：高级特性 第1章 Java8的流库

## 一、从迭代到流

处理集合时，通常选择迭代遍历它的元素，并在每个元素上执行某项操作。

使用流进行操作可以不必扫描整个代码去进行查找、过滤、计数等操作，方法名就可以直接告诉我们其代码的含义。

1. 流并不存储元素。这些元素可能存储在底层的集合中，或者是按需生成的。
2. 流的操作不会改变数据源。
3. 流的操作是尽可能惰性执行的。

## 二、流的创建

Collection接口的stream方法将任何集合转换为一个流。数组也可以使用静态方法Stream.of方法。

也可以使用Array.stream(array, from, to)用数组中的一部分元素来创建一个流。

若想创建一个不包含任何元素的流，可以用静态的Stream.empty方法。

也可以使用Stream接口中的静态generate方法创建无限流。

要产生迭代序列流，可以使用iterate方法，接受一个种子值以及一个函数。

如果要产生一个有限的流，需要添加一个谓词来描述迭代如何结束即可。

ofNullable方法会用一个对象来创建一个非常短的流。如果对象为null，那么这个流长度为0，否则长度为1，即只包含这个对象。

java.util.stream.Stream

```java
static <T> Stream<T> of(T... values);
// 产生一个元素为给定值的流。
static <T> Stream<T> empty();
// 产生一个不包含任何元素的流。
static <T> Stream<T> generate(Supplier<T> s);
// 产生一个无限流，它的值总是通过反复调用函数s而构建的。
static <T> Stream<T> iterate(T seed, UnaryOperator<T> f);
static <T> Stream<T> iterate(T seed, Predicate<? super T> hasNext, UnaryOperator<T> f);
// 产生一个无限流，它的元素包含seed、在seed上调用f产生的值、在前一个元素上调用f产生的值，等等。第一个方法会产生
一个无限流，而第二个方法的流会在碰到第一个不能满足hasNext谓词的元素时终止。
static <T> Stream<T> ofNullable(T t);
// 如果t为null，返回一个空流，否则返回包含t的流。
```

java.util.Spliterators

```java
static <T> Spliterator<T> spliteratorUnknownSize(Iterator<? extends T> iterator, int characteristics)
// 用给定的特性将一个迭代器转换为一个具有未知尺寸的可分割的迭代器
```

java.util.Arrays

```java
static <T> Stream<T> stream(T[] array, int startInclusive, int endExclusive);
// 产生一个流，元素由数组中指定的范围内元素构成。
```

java.util.regex.Pattern

```java
Stream<String> splitAsStream(CharSqeuence input);
// 产生一个流，它的元素是输入中由该模式界定的部分。
```

java.nio.file.Files

```java
static Stream<String> lines(Path path);
static Stream<String> lines(Path path, Charset cs);
// 产生一个流，它的元素都是指定文件中的行，该文件的字符集为UTF-8，或者为指定的字符集。
```

java.util.stream.StreamSupport

```java
static <T> Stream<T> stream(Spliterator<T> spliterator, boolean parallel);
// 产生一个流，包含了给定的可分割迭代器产生的值
```

java.lang.Iterable

```java
Spliterator<T> spliterator();
// 为这个Iterable产生一个可分割的迭代器。默认实现为不分割也不报告尺寸。
```

java.util.Scanner

```java
public Stream<String> tokens();
// 产生一个字符串流，该字符串是调用这个扫描器的next方法实现的。
```

java.util.function.Supplier<T>

```java
T get();
// 提供一个值
```

## 三、filter、map和flatMap方法

```java
Stream<T> filter(Predicate<? super T> predicate);
// 产生一个流,它包含当前流中所有满足谓词条件的元素
<R> Stream<R> map(Function<? super T, ? extends R> mapper);
// 产生一个流,它包含将mapper应用于当前流中所有元素所产生的结果
<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper);
// 产生一个流,它是通过将mapper应用于当前流中所有元素所产生的的结果连接到一起而获得的
```

## 四、抽取子流和组合流

调用stream.limit(n)会返回一个新的流，它在n个元素之后结束。

```java
Stream<Double> randoms = Stream.generate(Math::random).limit(100);
```

会产生一个包含100个随机数的流。

调用stream.skip(n)丢弃前n个元素。

stream.takeWhile(predicate)调用会在谓词为真时获取流中的所有元素，然后停止。

dropWhile(predicate)正好相反，会在谓词为真时丢弃元素，并产生一个由第一个使该条件为假的字符开始构造的流。

Stream类的静态concat方法将两个流连接起来：

```java
Stream<String> combined = Stream.concat(
	codePoints("Hello"), codePoints("World")	
);
```

## 五、其他的流转换

distinct方法会返回一个流，它的元素是从原有流中产生的，删除重复元素后产生的。

sorted方法会产生一个新的流，它的元素是流中按照顺序排列的元素。

peek方法会产生另一个流，它的元素与原来流中的元素相同，但是在每次获取一个元素时，都会调用一个函数。

```java
Stream<T> distinct();
// 产生一个流,包含当前流中所有不同的元素
Stream<T> sorted();
Stream<T> sorted(Comparator<? super T> comparator);
// 产生一个流,它的元素是当前流中的所有元素按照顺序排列的.
Stream<T> peek(Consumer<? super T> action);
// 产生一个流,它与当前流中元素相同,当获取其中每个元素时,会传递给action
```

## 六、简单约简

约简是一种终结操作,会将流约简为可以在程序中使用的非流值。

约简方法返回类型为Optional<T>的值,要么在其中包装了答案,要么表示没有任何值。

Optional类型是一种表示缺少返回值的更好的方式.

```java
Optional<T> max(Comparator<? super T> comparator);
Optional<T> min(Comparator<? super T> comparator);
// 分别产生这个流的最大元素和最小元素,使用由给定的比较器定义的比较器规则,如果流为空,产生一个空的Optional对象
Optional<T> findFirst();
Optional<T> findAny();
// 分别产生这个流的第一个和任意一个元素,如果流为空,会产生一个空的Optional
boolean anyMatch(Predicate<? super T> predicate);
boolean allMatch(Predicate<? super T> predicate);
boolean noneMathch(Predicate<? super T> predicate);
// 分别在这个流中任意元素、所有元素和没有任意元素匹配给定谓词时返回true
```

## 七、Optional类型

Optional<T>对象是一个包装器对象，要么包装了类型T的对象，要么没有包装任何对象。Optional<T>类型被当作一种更安全的方式，用来替代类型T的引用，这种引用要么引用某个对象，要么为null。

### 7.1 获取Optional值

有效地使用Optional的关键是要使用这样的方法：它的值不存在的情况下会产生一个可替代物，而只有在值存在的情况下才会使用这个值。

```java
T orElse(T other);
// 产生这个optional的值,或者在该optional为空时,产生other
T orElseGet(Supplier<? extends T> other);
// 产生这个optional的值,或者在该optional为空时,产生调用other的结果
<X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier);
// 产生这个optional的值,或者在该optional为空时,抛出调用exceptionSupplier的结果
```

### 7.2 消费Optional值

```java
void ifPresent(Consumer<? super T> action);
// 如果该Optional不为空,就将它的值传递给action
void ifPresentOrElse(Consumer<? super T> action, Runnable emptyAction);
// 如果该Optional不为空,就将它的值传递给action,否则调用emptyAction
```

### 7.3 管道化Optional值

```java
<U> Optional<U> map(Function<? super T, ? extends U> mapper);
// 产生一个optional,如果当前optional值存在,那么产生的optional的值是通过将给定的函数应用于当前的optional的值得到
// 的;否则,产生一个空的optional
Optional<T> filter(Predicate<? super T> predicate);
// 产生一个optional,如果当前的optional的值满足给定的谓词条件,那么所产生的的值就是当前的optional的值;
Optional<T> or(Supplier<? extends Optional<? extends T>> supplier);
// 如果当前optional不为空,则产生当前的optional
```

### 7.4 不适合使用optional值的方式

```java
T get();
T orElseThrow();
// 产生这个optional的值,或者在该optional为空时,抛出一个NoSuchElementException异常
boolean isPresent();
// 如果该optional不为空,则返回true
```

### 7.5 创建optional值

```java
static <T> Optional<T> of(T value);
static <T> Optional<T> ofNullable(T value);
// 产生一个具有给定值的optional,第一个方法在value是null时抛异常,第二个产生一个空optional
static <T> Optional<T> empty();
// 产生一个空Optional
```

### 7.6 用flatMap构建Optional值的函数

```java
<U> Optional<U> flatMap(Function<? super T, ? extends Optional<? extends U>> mapper);
// 如果mapper存在,产生将mapper应用于当前optional值所产的结果,或返回空
```

### 7.7 将optional转换为流

stream方法将Optional对象转换为Stream对象

## 八、收集结果

iterator方法会返回迭代器。

forEach方法将某个函数用于每个元素。

toArray方法获得由流元素构成的数组。

collect方法接收一个Collector接口的实例，将流元素收集到列表。

## 九、收集到映射表中

toMap、toUnmodifiableMap方法产生收集器，产生可修改和不可修改的映射表。keyMapper和valueMapper分别应用于每个收集到元素，从而在所产生的映射表中生成一个键、值项。

## 十、群组和分区

groupingBy、partitioningBy产生收集器。

## 十一、下游收集器

## 十二、约简操作

reduce从流中计算某个值，接收一个二元函数，并从前两个元素开始持续应用。

## 十三、基本数据流

IntStream、LongStream、DoubleStream、CharSequence、Random

## 十四、并行流

parallelStream方法获得集合的一个并行流。

> 据此，Java基础知识已基本覆盖，后续有查缺补漏仍会补上