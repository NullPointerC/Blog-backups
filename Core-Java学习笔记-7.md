---
title: Core-Java-学习笔记7
date: 2021-07-17 12:52:00
categories: Java
tags: [Java,backend,计算机基础]
---

> Core-Java的第8章 泛型程序设计

## 一、为什么引入泛型程序设计

泛型意味着代码对多种不同类型的对象重用。

### 1.1 类型参数的好处

在没有泛型之前，泛型都是通过继承来实现。如`ArrayList`类维护一个`Object`引用数组。

```java
public class ArrayList {
	// before generic classes
    private Object[] elementData;
    // other fileds
    ...
    
    public Object get(int index) { ... }
    public void add(Object obj) { ... }
}
```

这里存在的问题在于。当获取一个值时必须强制类型转换为需要的类型。并且，这里没有类型检查，可以添加任何类的值。泛型为此提供了一个更好的类型参数（type parameter）。使代码可读性变高。

并让编译器知道方法需要传入何种类型的参数，让编译器可以为我们完成检查工作，防止插入错误类型的对象。

### 1.2 通配符类型

详见Java类库。

## 二、定义简单泛型类

泛型类就是有一个或者多个类型变量的类。这个类只关心泛型，不为存储的细节而分心。

```java
public class Pair<T> {
    // instance fields
    private T first;
    private T second;
    // constructor
    public Pair() { first = null; second = null; }
    public Pair(T first, T second) {
        this.first = first;
        this.second = second;
    }
    // getter and setter
    public T getFirst() {
        return first;
    }
    public T getSecond() {
        return second;
    }
    public void setFirst(T newValue) {
        first = newValue;
    }
    public void setSecond(T newValue) {
        second = newValue;
    }
}
```

`Pair`类使用了类型变量`T`，使用尖括号`<>`包裹，放于类名后。

如果有多个类型，可以用逗号分隔即可。

```java
public class Pair<T, U> { ... }
```

一般使用`E`代表集合元素类型，`K`和`V`代表键和值的类型，`T、U 、S`代表任意类型。

也可以使用具体类型来替换类型变量来实例化（instantiate）泛型变量。

## 三、泛型方法

类型变量需放置修饰符后、返回类型前

```java
class ArrayAlg {
	public static <T> T getMiddle(T... a) {
		return a[a.length / 2];
	}
}
```

调用时，可以将具体类型包围在尖括号中、方法名前。

```java
String middle = ArrayAlg.<String>getMiddle("John", "Q.","Public");
```

这种情况下、方法调用可以省略`<String>`类型参数。（大多数情况下都省略）

## 四、类型变量的限定

有时需要对类型变量加以约束。

```java
public static <T extends Comparable> T min(T[] a) {...}
```

约束`T`只能是实现了`Comparable`接口的类。

表示限定类型时一定是使用`extends`而非`implements`。

```java
<T extends BoundingType>
```

表示T应该是限定类型的子类型。`T`和限定类可以是类，也可以是接口，使用`extends`是因为它更接近子类型的概念。

有多个限定时：

```java
T extends Comparable & Serializable
```

用**<font color="red">`&`</font>**分隔限定类型,用**<font color='red'>`,`</font>**分隔类型变量。

## 五、泛型代码和虚拟机

在虚拟机中没有泛型类型对象，所有对象都最终属于普通类。编译器会为我们完成**泛型擦除**这一步的工作。

### 5.1 泛型擦除

无论何时定义一个泛型类型，都会自动提供一个相应的原始类型（raw type）。这个类型的名字就是去掉类型参数后的泛型类型名。类型变量就会被擦除（erased），并替换为其限定类型。（或者，对于无限定类型则替换为object）。

### 5.2 转换泛型表达式

编写一个泛型方法调用时，如果擦除了返回类型，编译器会插入强制类型转换。

### 5.3 转换泛型方法

类型擦除也会出现再泛型方法中。

```java
public static <T extends Comparable> T min(T[] a)
```

并不是一组方法，在擦除类型后，就只剩下一个方法。

```java
public static Comparable min(Comparable[] a)
```

虚拟机中没有泛型，只有普通的类和方法。

所有的类型参数都会替换为他们的限定类型。

会合成桥方法来维持多态。

为保持类型安全性，必要时会插入强制类型转换。

### 5.4 调用遗留代码

```java
@SuppressWarnings("unchecked")
```

关闭对方法中所有代码的检查。

## 六、限制与局限性

### 6.1 不能用基本数据类型实例化类型参数

不能用基本数据类型代替类型参数，只能用其包装类或单独的类。

```java
Pair<double>// error
Pair<Double>// right
```

类型擦除后，`Pair`类只含有`Object`类型的字段，而`Object`不能存储`double`值。

### 6.2 运行时类型查询只适用于原始类型

虚拟机中的对象总有一个特定的非泛型类型。因此，所有的类型查询只产生原始类型。

`instanceof`关键字、`getClass`等方法只能获得原始类型。

```java
if(a instanceof Pair<String>) // error
if(a instanceof Pair<T>) // error
// 或者进行强制类型转换
Pair<String> p =(Pair<String>) a
```

以上都只可以检查a是否是一个`Pair`类的对象。

```java
Pair<String> stringPair = ...;
Pair<Employee> employeePair = ...;
if(stringPair.getClas() == employeePair.getClass()) // 这两者相等。
```

其比较结果为`true`，因为两次`getClass`都会返回`Pair.class`。

### 6.3 不能创建参数化类型的数组

不能实例化参数化类型的数组

```java
var table = new Pair<String>[10];// error
```

类型擦除后，table的类型为`Pair[]`。可以转换为`Object[]`。数组会记住它的元素类型，如果试图存储其他类型，则会抛出`ArrayStoreException`异常

```java
var table = new Pair<String>[10];
// 强制类型转换为object数组
Object[] objArray = table;
objArray[0] = "hello";//这是错误的,将会抛出ArrayStoreException异常
```

尽管存储一个`Pair`类型可以，但仍会导致一个类型错误。出于这个原因，不允许创建参数化类型的数组

```java
objArray[0] = new Pair<Employee>();
```

只是不允许创建这些数组，但是允许声明类型为参数化类型的数组。

### 6.4 Varargs警告

若向参数个数可变的方法传递一个泛型类型的实例。

```java
public static <T> void addAll(Collection<T> coll, T... ts) {
	for(T t : ts) coll.add(t);
}
```

如上述方法，参数个数可变，实际上为一个数组，包含所有实参。

以下调用过程：

```java
Collection<Pair<String>> table = ...;
Pair<String> pair1 = ...;
Pair<String> pair2 = ...;
addAll(table, pair1, pair2);
```

此时，jvm必须要创建一个`Pair<String>`类型的数组，虽然违反了6.3，但是对于这种情况，规则会有所放松。

将会得到一个警告，而非错误。

可以采用`@SuppressWarnings("unchecked")`抑制这个警告。或Java7中的`@SafeVarargs`直接注解`addAll`方法。

`@SafeVarargs`只能用于声明为`static`、`final`或（Java9中）`private`的构造器和方法。因为其他方法可能被覆盖，使得这个注解没有什么意义。

### 6.5 不能实例化类型变量

即不能在类似 `new T(...)` 表达式中使用类型变量。

```java
public Pair() {first = new T(); second = new T();}
// error
```

类型擦除将`T`变成`Object`。在Java8后，最好让调用者提供一个构造器表达式。

```java
Pair<String> p = Pair.makePair(String::new);
```

`makePair`方法接受一个`Supplier<T>`的函数式接口参数，表示一个无参数且返回值类型为T的方法。

```java
public static <T> Pair<T> makePair(Supplier<T> constr) {
	return new Pair<>(constr.get(), constr.get());
}
```

或者比较传统的方式使用反射调用`Constructor.newInstance`方法构造泛型对象。

```java
public static <T> Pair<T> makePair(Class<T> cl) {
	try {
		return new Pair<>(cl.getConstructor().newInstance(),
		cl.getConstructor().newInstance());
	}catch(Exception e) {
		return null;
	}
}
```

这个方法就可以如下调用：

```java
Pair<String> p = Pair.makePair(String.class);
```

注意，class类本身就是泛型的。如`String.class`就是`Class<String>`的一个实例。

`Class<T>`有且仅有一个实例就是`T.class`。

因此在这里，使用`makePair`方法就可以推断出`Pair`的类型。

### 6.6 不能构造泛型数组

就像不能实例化泛型实例一样，也不能实例化数组。

不过原因不同，虽然数组可以填充`null`值，但是数组本身也带有类型，如果带有泛型，则泛型会被擦除。

```java
public static <T extends Coparable> T[] minmax(T... a) {
	// 这是一种错误的写法
    T[] mm = new T[2];
    ...
}
```

类型擦除总是会构造`Comparable[2]`数组。

若数组仅为私有实例字段，可以将元素类型声明为擦除的类型并使用强制类型转换。

如`ArrayList`类可以如下实现：

```java
public class ArrayList<T> {
	private Object[] elements;
	...
	@SuppressWarnings("unchecked")
	public E get(int index) {
		return (E)elements[index];
	}
	public void set(int index, E e) {
		elements[index] = e;
	}
}
```

但是实际，`ArrayList`的设计者采用了如下设计：

```java
public class ArrayList<E> {
	private E[] elements;
	...
	public ArrayList() {
		elements = (E[]) new Object[DEFAULT_CAPACITY];
	}
}
```

这里，强制类型转换`E[]`只是一个假象，而类型擦除使其无法察觉。

### 6.7 泛型类的静态上下文中类型变量无效

不能在静态字段或方法中引用类型变量。因为类型将会被擦除。

```java
/**
 * 以下为错误定义,不能在静态上下文中定义类型变量。
 */
public class Singleton<T> {
    private static T singleInstance;
    public static T getSingleInstance() {
        if(null == singleInstance) {
            // construct new instance of T
        }
        return singleInstance;
    }
}
```

### 6.8 不能抛出或捕获泛型类的实例

既不能抛出也不能捕获泛型类的对象。泛型类也不能扩展`Throwable`。

`catch`子句也不能使用类型变量。在异常规范中，可以使用类型变量。

```java
/*以下定义不能通过编译*/
public class Problem<T> extends Exception{ /* ... */ }
/*catch子句中不能使用类型变量*/
public static <T extends Throwable> void doWork(Class<T> t) {
    try {
        do work;
    } catch (T e) {
        Logger.global.info(...);
    }
}
/*在异常规范中使用类型变量是允许的*/
public static <T extends Throwable> void d {
    try {
        do work;
    } catch (Throwable realCause) {
        t.initCause(realCause);
        throw t;
    }
}
```

### 6.9 可以取消对检查类异常的检查

Java中对于检查型异常的机制是，必须为所有的检查型异常提供处理器。但是可以使用泛型来取消这个机制。

```java
@SuppressWarnings("unchecked")
static <T extends Throwable> void throwAs(Throwable t) throw T {
	throw (T) t;
}
```

若上述方法位于`接口Task`中，且有一个检查型异常e，调用了`Task.<RuntimeException>throwAs(e);`

编译器就会认为e是一个非检查型异常。以下代码能够将所有异常转换为编译器认为的非检查型异常。

```java
try {
	do work;
}catch(Throwable t) {
	Task.<RuntimeException>throwAs(t);
}
```

若有以下需求：我们知道要在一个线程中运行代码，需要把代码放在一个实现了`Runnable`接口的类的`run`方法中。

不过我们知道这个方法不允许抛出检查型异常，只能在内部`try-catch`处理掉，不能往外抛。因为线程是一个独立运行的代码片段，它的问题不能影响到其他线程。我们将提供一个从`Task`到`Runnable`的适配器，它的`run`方法可以抛出任何异常。

```java
interfact Task {
    void run() throws Exception;
    
    @SuppressWarnings("unchecked")
    static <T extends Exception> void throwAs(Throwable t) throw T {
        throw (T)t;
    }
    
    static Runnable asRunnable(Task task) {
		return () -> {
            try {
                task.run();
            } catch(Exception e) {
                Task.<RuntimeException>throwAs(e);
            }
        };
    }
}
```

此时，有如下程序运行了一个线程，它会抛出了一个检查型异常。

```java
public class Test {
	public static void main(String[] args) {
		var thread = new Thread(Task.asRunnable(() -> {
            Thread.sleep(1000);
            System.out.println("Hello, world");
            throw new Exception("Check this out!");
        }));
        thread.start();
	}
}
```

`sleep`方法声明会抛出一个`InterruptedException`，我们不再需要捕获这个异常。由于我们没有中断这个线程，这个异常不会被抛出。不过，程序会抛出一个检查型异常。

这里的意义在于，在正常情况下，必须捕获`run`方法中的所有检查型异常，把它们保证到非检查异常里。

在此处我们没有采取包装，而是直接抛出异常，“哄骗”编译器，让其“相信”这不是一个检查型异常、

### 6.10 注意类型擦除后的冲突

当泛型类型被擦除后，不允许创建引发冲突的条件。

```java
public class Pair<T> {
	public boolean equals(T value) {
		return first.equals(value) && second.equals(value);
	}
}
```

则对于`Pair`类，就有两个`equals`方法，一个参数类型为`T`，实际类型为擦除后的类型。

另一个为`Object`，继承自`Object`类。

这时，由于类型擦除，这两个方法就会发生方法冲突。补救的方法就是重新给方法命名。

为支持擦除转换，要施加一个限制：两个接口类型是同一接口的不同参数化，一个类或类型变量就不能同时作为两个接口类型的子类。

```java
class Employee implements Comparable<Employee> { ... }
class Manager extends Employee implements Comparable<Manager> { ... }
```

`Manager`会实现接口，其中的类型参数不同，这就是同一接口的不同参数化。

## 七、泛型类型的继承规则

对于类型参数`T` 、`S`，无论`S`与`T`有什么关系，`Pair<T>`和`Pair<S>`都没有任何关系。

可以将参数化的类型转换为原始类型。如`Pair<Employee>`是原始类型`Pair`的一个子类型。

泛型类可以扩展或实现为其他的泛型类。如`ArrayList<T>`可以转换为`List<T>`。

## 八、通配符类型

### 8.1 通配符概念

即允许类型参数发生变化。如`Pair<? extends Employee>`代表泛型`Pair`类型，且类型参数必须为`Employee`的子类。如`Pair<Manager>`但不是`Pair<String>`。

### 8.2 通配符的超类型限定

通配符限定与类型参数限定十分类似，但还可以指定一个超类型限定。

`? super Manager`限制了类型都要为`Manager`的超类型。

即带有超类型限定的通配符允许写入一个泛型对象，而带有子类型限定的通配符允许读取一个泛型对象。

### 8.3 无限定通配符

还可以使用根本无限定通配符，如Pair<?>。

### 8.4 通配符捕获

通配符不是类型变量，所以不能在编写代码时使用“？”作为一种类型。

```java
public static void swap(Pair<?> p) {
    ? t = p.getFirst();//Error
	p.setFirst(p.getSecond());
	p.setSecond(t);
}
```

以上代码用于交换元素是不能成功的。但是，可以借助一个辅助方法。

```java
public static <T> void swapHelper(Pair<T> p) {
	T t = p.getFirst();
    p.setFirst(p.getSceond());
    p.setSecond(t);
}
```

若使用swap来调用swapHelper

```java
public static void swap(Pair<?> p) {
	swapHelper(p);
}
```

这种情况下称swapHelper方法的参数T捕获通配符。

## 九、 反射与泛型

### 9.1 泛型Class类

`Class`类也是泛型的。`String.class` 实际上是一个`Class<String>`类的对象，也是唯一的对象。

- `T newInstance()`

返回无参数构造器构造的一个新实例。

- `T cast(Object obj)`

如果obj为null或有可能转换成类型T，则返回obj；

否则抛出BadCastException异常。

- `T[] getEnumConstants()`

如果T是枚举类型，返回所有值组成的数组，否则返回null。

- `Class<? super T> getSuperClass()`

返回这个类的超类。如果T不是一个类或者为Object，返回null。

- `Constructor<T> getConstructor(Class... parameterTypes)`
- `Constructor<T> getDeclaredConstructor(Class... parameterTypes)`

获得公共构造器或有给定参数类型的构造器。

- `T newInstance(Object... parameters)`

返回用指定参数构造的新实例。

### 9.2 使用Class<T>参数进行类型匹配

上文提到的`makePair(Class<T> cl)`方法

### 9.3 虚拟机中的泛型类型信息

- `Class`类，描述具体类型
- `TypeVariable`接口，描述类型变量（如`T extends Comparable<? super T>`）
- `WildcardType`接口，描述通配符（如 `? super T`）
- `ParameterizedType`接口，描述泛型类或接口类型（如`Comparable<? super T>`）
- `GenericArrayType`接口，描述泛型数组如（`T[]`）

### 9.4 类型字面量

