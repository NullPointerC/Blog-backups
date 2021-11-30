---
title: Core-Java学习笔记-8
categories: [Java]
tags:
  - Java
  - backend
  - 计算机基础
date: 2021-07-18 09:15:57
---

> CoreJava第9章 集合

## 一、Java集合框架

### 1.1 集合接口与实现分离

在Java集合类库中，将接口与实现进行了分离。只有在构造集合对象时，才会使用具体的类。可以使用接口类型存放集合引用。便于一种接口有多个实现，只需对程序一个地方进行修改，即调用构造器的地方。

### 1.2 Collection接口

Java中集合类的基本接口时`Collection`接口。其中包括：

```java
public interface Collection<E> {
    boolean add(E element);
    Iterator<E> iterator();
    ...
}
```

`add`方法用于添加元素，`iterator`方法用于返回一个实现了`Iterator`接口的对象，可使用迭代器来访问集合的元素。

### 1.3 迭代器

```java
public interface Iterator<E> {
    E next();
    boolean hasNext();
    void remove();
    default void forEachRemaining(Consumer<? super E> action);
}
```

通过反复调用next方法，可以逐个访问集合中的元素。

`for-each`语法在底层转换为带有迭代器的循环。可以处理任何实现了迭代器接口的对象

也可以不使用循环，调用`forEachRemaining`方法并提供lambda表达式。

```java
iterator.forEachRemianing(element -> do something with element);
```

### 1.4 泛型实用方法

类库的设计者认为应该预留一些对所有集合通用的方法，所有的实现类都应该提供这些方法

```java
/*返回一个用于访问集合中各个元素的迭代器。*/
Iterator<E> iterator()
/*返回当前存储在集合中的元素个数*/
int size();
/*如果集合中没有元素,返回true*/
boolean isEmpty();
/*如果集合中包含了一个与obj相等的元素,返回true*/
boolean contains(Object obj);
/*如果这个集合中包含other集合中的所有元素,返回true*/
boolean containsAll(Collection<?> other);
/*将一个元素添加到集合中。如果由于这个调用改变了集合,返回true*/
boolean add(E element);
/*将other集合中的所有元素添加到集合中。如果由于这个调用改变了集合,返回true*/
boolean addAll(Collection<? extends E> other);
/*从这个集合中删除等于obj的对象。如果有匹配的对象被删除,返回true*/
boolean remove(Object obj);
/*从这个集合中删除other集合中存在的所有元素。如果由于这个调用改变了集合,则返回true*/
boolean removeAll(Collection<?> ohter);
/*从这个集合中删除filter返回true的所有元素。如果由于这个调用改变了集合,则返回true*/
default boolean removeIf(Predicate<? super E> fiiter);
/*从这个集合中删除所有的元素。*/
void clear();
/*从这个集合中删除所有与other集合中元素不同的元素。如果由于这个调用改变了集合,则返回true*/
boolean retainAll(Collection<?> other);
/*返回这个集合中的对象的数组*/
Object[] toArray();
/*返回这个集合中的对象的数组。如果arrayToFill足够大,就将集合中的元素填入这个数组中。
 *剩余空间填入null;
 *否则,分配一个新数组,其成员类型与arrayToFill的成员类型相同,其长度等于集合大小,并填充集合元素。
 */
<T> T[] toArray(T[] arrayToFill);
```

若实现了Collection接口的类都要提供这么多方法自然会十分繁琐。为了让实现者更容易地实现，Java类库提供了一个类`AbstractCollection`，它保持基础方法`size`和`iterator`仍为抽象方法，但是为实现者实现了其他方法。

这样，具体集合类就可以扩展抽象集合类。由更具体的类实现`iterator`方法，`contains`等方法已由超类提供。子类也可以重写这些方法。

对于`Iterator`接口：

```java
/*如果存在另一个可访问的对象,返回true*/
boolean hasNext();
/*返回将要访问的下一个对象。如果已经到达了集合的末尾,将抛出一个NoSuchElementException*/
void next();
/*删除上次访问的对象。这个方法必须紧跟在访问了上一个元素后执行。如果上次访问后集合变化,将抛出异常*/
void remove();
/*访问元素,并传递到指定动作,直到没有更多元素,或抛出异常*/
default void forEachRemaining(Consumer<? super E> action);
```

## 二、集合框架中的接口

集合框架中的接口图：

![image-20210718100903275](https://gitee.com/cao_ziqiang/img/raw/master/20210718100903.png)

有两个基本接口：`Collection`和`Map`。

集合框架中的类图：

![image-20210718103105854](https://gitee.com/cao_ziqiang/img/raw/master/20210718111922.png)

## 三、具体集合

| 集合类型          | 描述                                         |
| ----------------- | :------------------------------------------- |
| `ArrayList`       | 可以动态增长和缩减的一个索引序列             |
| `LinkedList`      | 可以在任何位置高效插入和删除的一个有序序列   |
| `ArrayDeque`      | 实现为循环数组的一个双端队列                 |
| `HashSet`         | 没有重复元素的一个无序集合                   |
| `TreeSet`         | 一个有续集                                   |
| `EnumSet`         | 一个包含枚举类型值的集                       |
| `LinkedHashSet`   | 一个可以记住元素插入次序的集                 |
| `PriorityQueue`   | 允许高效删除最小/大元素的一个集合            |
| `HashMap`         | 存储键/值关联的一个数据结构                  |
| `TreeMap`         | 键有序的一个映射                             |
| `EnumMap`         | 键属于枚举类型的一个映射                     |
| `LinkedHastMap`   | 可以记录键/值项添加次序的一个映射            |
| `WeakHashMap`     | 值不会在别处使用时就可以被垃圾回收的一个映射 |
| `IdentityHashMap` | 用==而不是equals比较键的一个映射             |
### 3.1 链表（LinkedList)

数组基于连续的存储位置存放对象，而链表将每个对象单独的链接。

链表是一个有序集合，每个对象位置十分重要。add方法将对象添加到链表尾部。

### 3.2 数组列表(ArrayList)

访问元素的协议：

通过迭代器；

通过get和set方法随机访问元素

### 3.3 散列集(HashSet)

散列表可以用于快速地查找对象。Java中散列表由链表数组实现。每个列表称为桶（bucket）。要想查找元素，先求出散列码，然后与桶总数取余，即求出了桶的索引。

如果碰到桶中已有元素，则称碰到哈希冲突。需要看新对象是否在桶中已存在。在Java8中，桶满时会从链表变为平衡二叉树。

一般会将桶数设置为预计元素总数的75%~150%。最好将其设置为一个素数，以防止键的聚集。标准类库使用的都是2的幂。

如果散列表太满，则需要再散列，创建一个桶数更多的表，并将所有元素差人到新表中，再丢弃原表。

一般而言，装填因子（load factor）可以确定何时对散列表进行再散列，如默认装填因子为0.75，这时会进行再散列。

散列表可以用于实现集类型。集是没有重复元素的类型。HashSet是实现了散列表的集，可以快速查找某个元素是否已经在集中，只需查看一个桶中的元素而不必查找所有元素。

### 3.4 树集(TreeSet)

TreeSet类与散列集十分类似，但比起散列集有所改进。树集是一个有序集合。可以以任意顺序将元素插入到集合中。

TreeSet是基于红黑树完成，每次添加，都会将其放置在正确的排序位置上。

但是添加到树中比添加到散列表中慢。要使用树集，必须能够比较元素，元素必须实现Comparable接口，或者构造树集时必须提供比较器Comparator。

TreeSet:

```java
TreeSet();
TreeSet(Comparator<? super E> comparator);
TreeSet(Collection<? extend E> elements);
TreeSet(SortedSet<E> s)
```

构造树集，并增加一个集合或有序集中的所有元素。

SortedSet：

```java
/*
 *返回用于对元素进行排序的比较器。如果使用的Comparable接口的方法则返回null
 */
Comparator<? super E> comparator();
/*返回有序集中最小、最大的元素*/
E first();
E last();
```

NaviableSet：

```java
/*返回大于value的最小元素或小于value的最大元素*/
E higher(E value);
E lower(E value);
/*返回大于等于value的最小元素或小于等于value的最大元素*/
E ceiling(E value);
E floor(E value);
/*删除并返回这个集中的最大元素或最小元素*/
E pollFirst();
E pollLast();
/*返回一个按照递减顺序遍历集中元素的迭代器*/
Iterator<E> descendingIterator();
```

### 3.5 队列与双端队列

队列允许在头、尾高效删除、添加元素。双端队列（deuqe）允许在头部和尾部都高效的删除和添加元素。

Java6开始引入了Deuqe接口，ArrayDeuqe和LinkedList都实现了接口。

Queue：

```java
/*如果队列没有满,将给定元素添加到这个队列的队尾并返回true。若满,第一个抛出异常,第二个则返回false*/
boolean add(E element);
boolean offer(E element);
/*如果队列不为空,删除并返回队头元素。如为空,第一个方法抛出异常,第二个则返回null*/
E remove();
E poll();
/*如果队列不为空,返回队头元素，但不删除。若为空,第一个方法抛出异常,第二个返回null*/
E element();
E peek();
```

Deuqe：

```java
/*含义同上*/
void addFirst(E element);
void addLast(E element);
boolean offerFirst(E element);
boolean offerLast(E element);
E removeFirst();
E removeLast();
E pollFirst();
E pollLast();
E getFirst();
E getLast();
E peekFirst();
E peekLast();
```

ArrayDeuqe：

```java
ArrayDeuqe();
ArrayDeuqe(int initialCapacity);
```

### 3.6 优先队列

优先队列中的元素可以按任意顺序插入，但会按照有序的顺序进行检索。无论何时调用remove方法，都会获得当前优先队列中最小的元素。不过优先队列并没有对所有元素进行排序。使用了一种数据结构，称为堆。堆是一个自组织的二叉树。其添加，删除操作都是最小元素移动到根。

PriorityQueue：

```java
PriorityQueue();
PriorityQueue(int initialCapacity);
PriorityQueue(int initialCapacity, Comparator<? super E> c);
```

## 四、 映射

映射（map）用来存储键/值对。

### 4.1 基本映射操作

Java类库提供了两个映射基本实现：HashMap和TreeMap。散列映射对键进行散列，树映射根据键的顺序将元素组织为搜索树。但散列和比较函数只作用于键，不做用于值。

而要检索一个映射，也必须使用键（get），如果没有出现映射中的键，将返回null值。

### 4.2 更新映射条数

```java
counts.put(word, counts.get(word) + 1);//worse
counts.put(word, counts.getOrDefault(word, 0) + 1);//better
```

或先调用其他方法

```java
counts.putIfAbsent(word, 0);
counts.put(word, counts.get(word) + 1);
```

Map在Java8中新增的默认方法：

```java
/*
 *如果key与一个非null值V关联,将函数应用到V和value,将key与结果关联;或结果为null,则删除这个键;
 *否则,将key与value关联,返回get(key)
 */
default V merge(K key, V value, BiFunction<? super V, ? super V, ? extend V> remappingFunction);
/*
 *将函数应用到key和get(key)
 *再将key与结果关联,如果结果为null,则删除这个键。
 *返回get(key)
 */
default V compute(K key, BiFunction<? super K, ? super V,? extends V> remappingFunction);
/*
 *如果key与一个非null值V关联,将函数应用于key和v,将key与结果相关联,结果为空,则删除这个键。
 */
default V computeIfPresent(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction);
/*
 *将这个函数应用到key,除非key与一个非null值相关联。将key与结果关联,或如果结果为null,则删除这个键。
 */
default V computeIfAbsent(K key, Function<? super K, ? extends V> mappingFunction);
/*
 *在所有映射条目上应用这个函数。将键与非null结果关联,对于null结果,则将键删除。
 */
default void replaceAll(BiFunction<? super K, ? super V,? extends V> function);
/*
 * 如果key不存在或与null关联,则将它与value关联,并返回null。否则返回关联的值。
 */
default V putIfAbsent(K key, V value);
```

### 4.3 映射视图

集合框架不认为映射本身是一个集合。不过可以得到映射的视图。

键集、值集合（不是一个集）以及键值对集。

在Map中分别做了如下实现。

```java
/*
 *返回映射中所有键的一个集视图。可以删除元素,删除后map中的键和值都会被删除。但不能添加元素
 */
Set<K> keySet();
/*
 *返回映射中所有值的一个集合视图。可以从中删除元素,同样删除后键和值都会被删除。但不能添加元素
 */
Collection<V> values();
/*
 *返回Map.Entry对象(键值对)的集视图。可以删除对象,但是同样不能添加
 */
Set<Map.Entry<K, V>> entrySet();
```

### 4.4 弱散列映射

WeakHashMap使用弱引用保存键。垃圾回收器不会马上将没有引用的键值回收。

### 4.5 链接散列表与映射

LinkedHashSet和LinkedHashMap类会记住插入元素项的顺序，避免了散列表中项看起来是随机的。在表中插入元素时，会加入到双向链表中。

### 4.6 枚举集与映射

EnumSet是一个枚举类型元素集的实现。由于枚举类型只有有限个实例，所以内部使用位序列实现。如果对应的值在集中，则对应位被置为1。

EnumSet没有构造器。要使用静态工厂方法来构造这个集：

EnumMap是一个键类型位枚举类型的映射。可以直接高效地实现为一个值数组。需要在构造器中声明键类型。

### 4.7 标识散列映射

类IdentityHashMap中键的散列值不使用hashCode函数计算，使用System.identityHashCode方法计算。这是Object.hashCode根据对象的内存地址计算散列码时所使用的方法。而且在进行两个对象比较时，IdentityHashMap类使用==，而不使用equals。

不同键对象即使内容相同，也被视为不同对象。在实现对象遍历算法时，可以标识哪些对象已经被遍历过。

## 五、 视图与包装器

视图即映射中的部分数据的集合，可以在视图中操纵原映射，这种集合称为视图。

### 5.1 小集合

Java9中引入部分静态方法，可以生成给定元素的集或列表，以及给定键、值对的映射。

### 5.2 子范围

可以为集合建立子范围视图。对子范围的操作会反映到元集合。

SortedMap接口和NaviaableSet接口分别有一些对应方法。

### 5.3 不可修改视图

Collections.unmodifiableXxx方法返回不可修改的视图。

并不是集合不可改，而是对视图修改，会抛出异常。仍可以对集合引用进行修改。

### 5.4 同步视图

多线程访问集合，要确保集合不会被破坏。类库设计者在设计时通过视图机制确保常规集合时线程安全的，但没有实现线程安全的集合。

### 5.5 检查型视图

用于对泛型类型可能出现的问题提供调试支持。

### 5.6 关于可选操作的说明

视图通常只读、无法改变大学或只支持删除而不支持插入。如执行不恰当操作可能抛出异常。

```java
/*List接口、Set接口*/
//生成给定元素的一个不可变列表，元素不能为null
static <E> List<E>/Set<E>(E... elements);

/*Map接口*/
//生成给定键和值的一个不可变映射，键和值都不能为null
static <K, V>Map<K, V> of(K k, V v);
//生成一个不可变映射条目，键和值都不能为null
static <K, V>Map.Entry<K, V> entry(K k, V v);
//生成给定映射条目的一个不可变映射。
static <K, V>Map<K, V> ofEntries(Map.Entry<? extends K, ? extends V>... entries);

/*Collections类*/
//构造一个集合视图，视图的更改器方法抛出异常。
static <E>Xxx unmodifiableXxx(Xxx<E> xxx);
//构造一个集合视图，视图中的方法都是同步的。
static <E>Xxx synchronizedXxx(Xxx<E> xxx);
//构造一个集合视图，如果插入一个错误类型的元素，将抛出异常。
static <E>Xxx checkedXxx(Xxx<E> xxx);
//生成一个不可变列表，包含n个相等的值
static <E>List<E> nCopies(int n, E value);
//生成单例列表、集或映射。
static <E>Xxx<E> singletonXxx(E value);
//生成一个空集合、映射或迭代器。
static <E>Xxx<E> emptyXxx();

/*Arrays类*/
//返回一个数组中元素的列表视图。数组可修改但大小不可变
static <E>List<E> asList(E... array);

/*List接口*/
//返回给定位置范围内的所有元素的列表视图
List<E> subList(int firstIncluded, int firstExcluded);

/*SortSet接口*/
//返回给定范围内元素的视图
SortedSet<E> subSet(E firstIncluded, E firstExcluded);
SortedSet<E> headSet(E firstExcluded);
SortedSet<E> tailSet(E firstIncluded);

/*NavigableSet接口*/
//返回给定范围内元素的视图。boolean标志决定视图是否包含边界
NavigableSet<E> subSet(E from, boolean fromIncluded, E to, boolean toIncluded);
NavigableSet<E> headSet(E to, boolean toIncluded);
NavigableSet<E> tailSet(E from, boolean fromIncluded);
```

## 六、 算法

### 6.1 泛型算法

泛型集合的优点在于算法只需实现一次。

### 6.2 排序与混排

Collections类中sort方法实现了对List接口的集合进行排序。shuffle方法随机地混排列表中的元素顺序。

### 6.3 二分查找

元素类型需要实现RandomAccess接口，否则会退化为线性查找。

### 6.4 简单算法

Collections类

```java
//返回集合中最小的或最大的元素
static <T extends Comparable<? super T>> T min(Collection<T> elements);
static <T extends Comparable<? super T>> T max(Collection<T> elements);
static <T> min(Collection<T> elements, Comparator<? super T> c);
static <T> max(Collection<T> elements, Comparator<? super T> c);
//将原列表中的所有元素复制到目标列表的相应位置上。目标列表的长度至少与原列表一样。
static <T> void copy(List<? super T> to, List<T> from);
//将列表中所有位置设置为相同的值。
static <T> void fill(List<? super T> l, T value);
//将所有的值添加到给定的集合中。如果集合改变了，则返回true
static <T> boolean addAll(Collection<? super T>c, T... values);
//用newValue替换所有值为oldValue的元素。
static <T> boolean replaceAll(List<T> l, T oldValue, T newValue);
//返回l中第一个或最后一个等于s的子列表的索引。如果l中不存在等于s的子列表，则返回-1
static int indexOfSubList(List<?> l, List<?> s);
static int lastIndexOfSubList(List<?> l, List<?> s);
//交换给定偏移位置的两个元素。
static void swap(List<?> l, int i, int j);
//逆置列表中元素的顺序。
static void reverse(List<?> l);
//旋转列表中元素，将索引i的元素移动到位置(i+d)%l.size()。
static void rotate(List<?> l, int d);
//返回c中与对象obj相等的元素个数
static int frequency(Collcetion<?> c, Object obj);
//如果两个集合中没有共同的元素，则返回true
boolean disjoint(Collection<?> c1, Collection<?> c2);
```

Collection和List接口中新默认方法

```java
default boolean removeIf(Predicate<? super E> filter);
//删除所有的匹配元素
default void replaceAll(UnaryOperator<E> op);
//对这个列表的所有元素应用这个操作
```

### 6.5 批操作

removeAll、retainAll、addAll等系列方法。

### 6.6 集合与数组的转换

List.of包装器可以将数组转为集合。

toArray方法将集合转为数组。

### 6.7 编写算法

## 七、 遗留的集合

![image-20210719113327564](https://gitee.com/cao_ziqiang/img/raw/master/20210719113327.png)

### 7.1 HashTable

经典的HashTable与HashMap作用一样，接口也基本相同。与Vector类一样，HashTable类中方法也是同步的。如果对遗留代码的兼容性没有要求，应该使用HashMap。需要并发访问应该使用ConcurrentHashMap。

### 7.2 枚举

遗留的集合使用Enumeration接口遍历元素序列。

其中包含hasMoreElements和nextElement类似于Iterator接口的hasNext和next。

```java
boolean hasMoreElements();
//如果还有更多的元素可以查看，则返回true。
E nextElement();
//返回要检测的下一个元素。
default Iterator<E> asIterator();
//生成一个迭代器，可以迭代处理枚举的元素。
```

Collections：

```java
static <T> Enumeration<T> enumeratrion<Collection<E> c);
//返回一个枚举，可以枚举c的元素。
public static <T> ArrayList<T> list(Enumeration<T> e);
//返回一个数组列表，其中包含e枚举的元素
```

### 7.3 属性映射

属性映射（property map）是一个特殊的映射结构。

- 键与值都是字符串。
- 可以保存到文件或从文件加载。
- 有一个二级表存放默认值。

实现属性映射的类称为Properties。常用于指定程序的配置选项。

```java
var settings = new Properties();
settings.setProperty("width", "600.0");
settings.setProperty("filename", "/home/cay/books/cj11/code/v1ch11/raven.html");
```

也可以使用store方法将属性映射保存到一个文件中。

Properties类有两种提供默认值的方式。第一种是查找一个字符串的值，可以指定一个默认值，当键不存在时会自动使用默认值。或设置一个二级属性映射。

### 7.4 栈

Stack类：扩展自Vector类

```java
E push(E item);//入栈
E pop();//弹栈
E peek();//取栈顶元素
```

### 7.5 位集

BitSet存储一个位序列（不是数学意义上的集，是位向量或位数组）。如果需要高效地存储位序列（例如，标志），就可以使用位集。由于位集将位包装在字节里，所以使用位集要比使用Boolean的ArrayList高效许多。

常用埃拉托色尼筛选法测试编译器性能。在最近的版本中，Java编译器性能的优化已经战胜了G++编译器。

C++:

![image-20210719122530511](https://gitee.com/cao_ziqiang/img/raw/master/20210719122530.png)

Java:

![image-20210719122547969](https://gitee.com/cao_ziqiang/img/raw/master/20210719122548.png)

