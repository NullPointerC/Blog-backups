---
title: Core-Java学习笔记-5
categories: [Java]
tags:
  - Java
  - backend
  - 计算机基础
date: 2021-07-12 09:32:24
---

> CoreJava第六章---接口、lambda表达式与内部类

## 一、接口

### 1.1 接口的概念

在Java中，接口并不是类，而是希望符合这个接口的类的一组需求。

接口中所有的方法都自动是<font color="blue">`public`</font>方法。因此在接口中，不必提供关键字<font color="blue">`public`</font>。

接口不会有实例字段。在Java8之前，，也不会有实现方法。提供实例字段和方法实现的任务应该由实现接口的那个类来完成。

为了让一个类实现某个接口，要使用<font color="blue">`implements`</font>关键字。

虽然接口中方法没有声明为<font color="blue">`public`</font>，但在实现类中必须声明为<font color="blue">`public`</font>方法，否则编译器将会认为这个方法的访问属性是包可见性，这是类的默认访问属性，之后编译器将会报错。

### 1.2 接口的属性

接口不是类。也就是说不能使用new来实例化一个接口。

但是可以声明接口类型的变量，但是必须引用这个接口的类对象。

也可以使用<font color="blue">`instanof`</font>来检查一个对象是否实现了某个接口。

虽然接口中不能包含有示例字段,但是可以包含有常量。和方法一样，接口中的常量会被自动设置为是<font color="blue">`public static final`</font>。

每个类都只能有一个超类，但是可以实现多个接口。

### 1.3 接口与抽象类

使用抽象类表示通用属性会存在一个严重的问题。每个类只能拓展一个类。但是每个类却可以实现多个接口。

### 1.4 静态和私有方法

在Java8中，允许在接口中增加静态方法。理论上讲，这样是合法的，但是有违背于将接口作为抽象规范的初衷。

通常的做法都是将静态方法放在伴随类中。在Java标准类库中,有成对出现的接口和实用的工具类,如Collection/Collections或Paths/Path。

可以由一个URI或者字符串序列构造一个文件或目录的路径，如Path.get("jdk-11","conf","security")。在jdk11中，Path接口给出了等价的静态方法。

```java
public interface Path{
	public static Path of(URI rui){...}
	public static Path of(String first,String... more){...}
}
```

类似的，在实现自己的接口时，就没有必要再为工具方法提供一个伴随类。

再Java9中，接口中的方法可以时private。private方法可以是静态方法或实例方法。由于私有方法只能在接口本身的方法中使用，所以用法很有限，只能作为接口中其他方法的辅助方法。

### 1.5 默认方法

可以为接口方法提供一个默认实现。<font color="red">必须</font>用<font color='blue'>`default`</font>修饰符标记这样一个方法。

```java
public interface Comparable<T> {
	default int compareTo(T other) {
		return 0;
	}
}
```

默认方法可以调用其他方法。如Collection接口

```java
public interface Collection {
	int size();
	default boolean isEmpty() { return size() == 0; }
}
```

这样实现Collection的程序员就不用担心实现isEmpty()方法了。

默认方法的一个很重要用法是：“接口演化”（interface evolution）。如果接口中新增加了一个方法且不是默认方法，那么它之前的所有实现类都必须去实现这个方法。如果将方法实现为一个默认方法就可以解决这个问题。

### 1.6 解决默认方法冲突

如果接口定义了默认方法，又在超类或另一个接口中定义了同样的方法，Java提供了如下规则：

1. 超类优先。如果超类提供了一个**具体**的方法，同名而且有相同参数类型的默认方法会被忽略。
2. 接口冲突。如果一个接口提供了一个默认方法，另一个接口提供了一个同名而且参数类型（无论是否是默认参数）相同的方法，必须在实现类中覆盖这个方法来解决冲突。

```java
interface Person {
	default String getName() {return ""; };
}

interface Named {
    default String getName() {return getClass.getName() + "_" + hashCode();}
}

class Student implements Person, Named {
    /*类会继承两个接口提供的不一样的getName方法，并且提供一个getName方法覆盖它*/
    @Override
    public String getName() { return Person.super.getName(); }
}
```

即使只有一个接口中方法为默认方法，实现类中还是需要覆盖同名方法来解决二义性问题。

如果两个接口都没有为共享方法提供默认实现，要么在实现类中实现要么不实现，继续保持抽象。

### 1.7 接口与回调

回调（callback）是一种常见的程序设计模式。在这种模式中，可以指定某个特定事件发生时应该采取的动作。

### 1.8 Comparator接口

比较器(`compartor`)是实现了Comparator接口的类的示例。

```java
public interface Comparrator<T> {
	int compare(T first, T second);
}
```

具体比较时，需要建立一个实例，compare方法需要在比较器对象上调用，而不是在要被比较的对象本身上调用。

### 1.9 对象克隆

Cloneable接口指示一个类提供了一个安全的clone方法。如果希望copy是一个新对象，它的初始状态与original相同，但是之后它们各自会有自己不同的状态，这样情况下需要使用clone方法。

默认的Object类中clone方法时及逆行“浅拷贝”，并没有拷贝对象中所引用的其他对象。

通常子对象都是可变的，需要重新定义clone方法来建立深拷贝，同时克隆整个子对象。

Cloneable接口并没有指定clone方法，只是作为一个标记，指示类设计者了解克隆过程。

这种接口称为标记接口（tagging interface）。

即使有些对象的clone默认实现（浅拷贝）能够完成需求，但是还是需要实现Cloneable接口，将clone重新定义为public，再调用super.clone()

```java
class Employee implements Cloneable {
    public Employee clone() throws CloneNotSupportedException {
        // call Object.clone()
        Employee cloned = (Employee)super.clone();
        // clone mutable fields
        cloned.hireDay = (Date)hireDay.clone();
        return cloned;
    }
}
```

## 二、lambda表达式

### 2.1 为什么引入

lambda是一个可传递的代码块，可以在以后执行一次或者多次。

### 2.2 语法

lambda表达式就是一个代码块，以及必须要传入的代码的变量规范。

```java
(String first, String second) -> first.lentgh() - second.length()
```

参数，箭头(->) 以及一个表达式。如果要完成的代码的计算无法用一个表达式表达，可以像写方法一样，把这些代码都放在`{}`中，并且显式包含`return`语句。

```java
(String first, String second) ->
{
	if(first.length() < second.length()) return -1;
	else if (first.length() > second.length()) return 1;
	else return 0;
}
```

**即使lambda表达式没有参数，仍然需要提供空括号，就像无参方法一样。**

```java
() -> {for (int i = 100; i >= 0; i--) System.out.println(i);}
```

**如果可以推导出一个lambda表达式的参数类型，则可以忽略其类型。**

```java
Coparator<String> comp
	= (first, second) //same as (String first, String second)
		-> first.length() - second.length()
```

在这里,编译器可以推导出first,second必然是字符串,因为这个lambda表达式将要赋值给一个字符串比较器。

**如果方法的参数只有一个，而且这个参数的类型可以推导而出，那么可以直接省略小括号**

```java
ActionListener listener = event ->
	System.out.println("The time is"
	+ Instant.ofEpochMilli(event.getWhen()));
	// instead of (event) -> ... or (ActionEvent event) -> ...
```

无须指定lambda表达式的返回值类型。lambda表达式的返回类型总是会根据上下文推导得出。

```java
(String first, String second) -> first.length() - second.length()
```

如果一个lambda在一个分支返回再另一个分支不返回则是不合法的。

### 2.3 函数式接口

对于只有一个抽象方法的接口，需要这种接口的对象时，就可以提供一个lambda表达式。

这种接口称为函数式接口。（function interface）

```java
Arrays.sort(words,
	(first,second) -> first.length() - second.length());
```

在底层，Arrays.sort方法会接收实现了Comparator<String>的某个类的对象。

在这个对象上调用compare方法会执行lambda表达式的体。

这些对象和类的管理完全取决于具体实现，与传统的内联类相比，这样更加高效。

并且可以把lambda表达式看作是一个函数，而不是一个对象，另外要接受lambda表达式可以传递到函数式接口。

```java
var timer = new Timer(1000, event ->
                      {
                          System.out.println("At the tonw, the time is "
                                            + Instant.ofEpochMilli(event.getWhen()));
                          Toolkit.getDefaultToolkit().beep();
                      });
```

函数式接口往往有一个特定的用途，而不只是提供一个有指定参数和返回类型的方法。想要用lambda表达式做某些处理，还是需要谨记表达式用途，创建一个特定的函数式接口。

### 2.4 方法引用

lambda表达式涉及一个方法。表达式System.out::println()是一个方法引用（method reference）。

它指示编译器生成一个函数式接口的实例，覆盖这个接口的抽象方法来调用给定的方法。

方法引用也不是对象，但是为一个类型为函数式接口的变量赋值时会生成一个对象。

1. object::instanceMethod
2. Class::instanceMethod
3. Class::staticMethod

在第一种情况下,方法引用等价于传递参数的lambda表达式。

如System.out::println。对象是System.out,所以方法表达式等价于x->System.out.println(x)。

对于第二种情况，第一个参数会称为方法的隐式参数。

例如，String::compareToIgnoreCase等同于(x,y)->x.compareToIgnoreCase(y)。

在第三种情况下，所有的参数都传递到了静态方法：Math::pow等价与(x,y)->Math.pow(x,y)。

| 方法引用          | 等价的lambda表达式       | 说明                                                         |
| ----------------- | ------------------------ | ------------------------------------------------------------ |
| separator::equals | x->separator.equals(x)   | 这是一个包含对象和一个实例方法的方法表达式。<br />lambda参数作为这个方法的显式参数传入。 |
| String::trim      | x->x.trim()              | 这是一个包含类和一个实例方法的方法表达式。<br />lambda表达式会成为隐式参数。 |
| String::concat    | (x,y)->x.concat(y)       | 同样，这里有一个实例方法，不过这次有一个显示参数。<br />与前面一样，第一个lambda参数会成为隐式参数，其余的参数会传递到方法。 |
| Integer::valueOf  | x->Integer::valueOf(x)   | 这是一个包含一个静态方法的方法表达式。<br/>lambda参数会传递到这个静态方法。 |
| Integer::sum      | (x,y)->Integer::sum(x,y) | 这是另一个静态方法，不过这一次有两个参数。<br/>两个lambda参数都可以传递到这个静态方法。<br/>Integer.sum()方法专门创建为作为一个方法引用。<br/>对于lambda表达式，可以只写作(x,y)->x+y。 |
| Integer::new      | x->new Integer(x)        | 这是一个构造器引用。lambda参数会传递到这个构造器。           |
| Integer[]::new    | n->new Integer[n]        | 这是一个数组构造器引用。lambda参数是数组的长度。             |

只有当lambda表达式的体只调用一个方法而不做其他操作时，才能将lambda表达式重写为方法引用。

### 2.5 构造器引用

构造器引用和方法引用和类似。不过方法名为new。

### 2.6 变量作用域

lambda表达式包含3个部分：

1. 一个代码块；
2. 参数；
3. 自由变量的值，这是指非参数而且不在代码中定义的变量。

可以把一个lambda表达式转换为包含一个方法的对象，这样自由变量的值就会复制到这个对象的实例变量中。关于这种代码块及自由变量值也称为闭包（closure）。Java中的闭包就是lambda表达式。

lambda表达式可以捕获外围作用域中变量的值。要确保所捕获的值是明确定义的.<font color="red">**且只能引用值不会改变变量**</font>

```java
public static void countDown(int start, int delay) {
	ActionListener listener = event -> 
    {
        start--;//错误的写法! 不能更改捕获的变量
        System.out.println(start);
    };
    new Time(delay, listener).start();
}
```

因为如果不做限制,那么并发执行多个动作时就会不安全。

如果在lambda表达式中引用一个变量，而且这个变量可能在外部改变，这也是不合法的。

```java
public static void repeat(String text, int count) {
	for(int i = 1; i <= count; i++) {
        ActionListener listener = event ->
        {
            System.out.println(i + ": " + text);// 错误的写法!不能引用一个变化的变量i
        }
        new Timer(1000, listener).start();
    }
}
```

lambda表达式中捕获的变量必须实际上是事实最终变量。即，这个变量初始化后就不会再为其赋新值。

lambda表达式中体与嵌套块具有相同的作用域。这里同样适用于命名冲突和遮蔽的有关规则。在lambda表达式中声明与一个局部变量同名的参数或局部变量是不合法的。

```java
Path first = Path.of("/usr/bin");
Comparator<String> comp
    = (first, second) -> first.length() - second.length();//错误的写法，变量first已经被定义了
```

在一个lambda表达式中使用this关键字时，是指这个lambda表达式的方法的this参数。

```java
public class Application {
    public void init() {
        ActionListener listener = event -> {
            System.out.println(this.toString());
        }
    }
}
```

表达式`this.toString()`会调用application对象的toString方法，而不是ActionListener实例的方法。在lambda表达式中，this的使用并没有任何特殊之处。lambda表达式的作用域嵌套在init方法中，与出现在这个方法中的其他位置一样，lambda表达式中this的含义并没有变化。

### 2.7 处理lambda表达式

lambda表达式的重点是延迟执行（deferred execution）。如果想要立即执行代码，完全可以直接执行，而无须把它包装在一个lambda表达式中。

常见的函数式接口

| 函数式接口          | 参数类型 | 返回类型 | 抽象方法名 | 描述                         | 其他方法                 |
| ------------------- | -------- | -------- | ---------- | ---------------------------- | ------------------------ |
| Runnable            | 无       | void     | run        | 作为无参数或返回值的动作执行 |                          |
| Supplier<T>         | 无       | T        | get        | 提供一个T类型的值            |                          |
| Consumer<T>         | T        | void     | accept     | 处理一个T类型的值            | andThen                  |
| BiConsumer<T, U>    | T, U     | void     | accept     | 处理T和U类型的值             | andThen                  |
| Function<T, R>      | T        | R        | apply      | 有一个T类型参数的函数        | compose,andThen,identity |
| BiFunction<T, U, R> | T, U     | R        | apply      | 有T和U类型参数的函数         | andThen                  |
| UnaryOperator<T>    | T        | T        | apply      | 类型T上的一元操作符          | compost,andThen,identity |
| BinaryOperator<T>   | T, T     | T        | apply      | 类型T上的二元操作符          | andThen, maxBy, minBy    |
| Predicate<T>        | T        | boolean  | test       | 布尔值函数                   | and,or,negate,isEqual    |
| BiPredicate<T, U>   | T, U     | boolean  | test       | 有两个参数的布尔值函数       | and, or, negate          |

如果我们想让一个动作重复n。将这个动作和重复次数传递到一个repeat方法。

`repeat(10, () -> {System.out.println("Hello, World!")});`

如果需要接受这个lambda表达式，需要选择一个函数式接口。如Runnable接口。

```java
public static void repeat(int n, Runnable action) {
	for(int i = 0; i < n; i++) action.run();
}
```

调用action.run()时会执行这个lambda表达式的主体。

基本数据类型的函数式接口，使用这些特殊化接口会比通用化接口更加高效。

| 函数式接口       | 参数类型 | 返回类型 | 抽象方法名   |
| ---------------- | -------- | -------- | ------------ |
| BooleanSupplier  | 无       | boolean  | getAsBoolean |
| PSupplier        | 无       | p        | getAsP       |
| PConsumer        | p        | void     | accept       |
| ObjPConsumer<T>  | T, p     | void     | accept       |
| PFunction<T>     | p        | T        | apply        |
| PToQFunction     | p        | q        | applyAsQ     |
| ToPFunction<T>   | T        | p        | applyAsP     |
| ToPBiFunction<T> | T, U     | p        | applyAsP     |
| PUnaryOperator   | p        | p        | applyAsP     |
| PBinaryOperator  | p、p     | p        | applyAsP     |
| PPredicate       | p        | boolean  | test         |

注：p、q是`int、long、double`；P、Q是`Int、Long、Double`

大多数标准函数式接口都提供了非抽象方法来生产或合并函数。

如果设计自己的接口，其中只有一个抽象方法，最好可以用`@FunctionalInterface`注解来标记这个接口。

这样如果无意中增加了另一个抽象方法，编译器将会报错；

另外javadoc会指出这个接口是一个函数式接口。

### 2.8 再谈Comparator

Comparator接口包含很多方便的静态方法来创建比较器。这些方法可以用于lambda表达式或方法引用。

静态comparing方法取一个“键提取器”函数，将类型T映射为一个可比较的类型。对要比较的对象应用这个函数，然后对返回的键完成比较。

## 三、内部类

内部类是定义在另一个类中的类。

使用内部类的好处在于：

1. 内部类可以对同一个包中的其他类完成类隐藏
2. 内部类方法可以访问定义这个类的作用域中的数据，包括原本的私有的数据。

### 3.1 内部类访问对象状态

一个内部类方法可以访问自身的数据字段，也可以访问它的外围类对象的数据字段。

内部类对象总有一个隐式引用，指向外部类对象。这个引用在内部类中的定义是不可见的。

外部类的引用会在构造器中设置。编译器会修改所有的内部类构造器，添加一个对于的外部类引用的参数。

```java
public TimePrinter(TalkingClock clock) {
    //自动地引用
    outer = clock;
}
```



### 3.2 内部类的特殊语法规则

表达式`OuterClass.this`表示外围类引用。

反过来，也可以采用`outerObject.new InnerClass(Construction parameters)`来更加明确地编写内部类构造器。

在外部类的作用域之外，还可以使用`OuterClass.InnerClass`来引用内部类。

内部类中声明的所有静态字段必须是`final`，并且初始化一个编译时常量。如果不是一个常量，就可能不唯一。

内部类不能有static方法。Java语言规范没有做出解释。也可以允许有静态方法，但是只能访问外围类的静态字段和方法。

### 3.3 内部类是否有用、必要和安全

内部类是一个编译器现象，与虚拟机无关。编译器会将内部类转换为是常规的类文件，用$（美金符号）分割外部类名与内部类名，而虚拟机却一无所知。

内部类有着更强大的访问权限，天生就比常规类功能更强大。

但是，如果内部类访问了私有数据字段，就有可能通过外围类所在包中增加的其他类访问那些字段。

### 3.4 局部内部类

一个类名字只出现了一次，且只是在某个方法中创建这个类型的对象时使用了一次。遇到这种情况，可以在一个方法中局部地定义这个类。

声明局部内部类时不能有访问说明符（即public或private）。局部类的作用域被限定在声明这个局部类的块中。

局部类还有一个很大的优势，即对外部世界完全隐藏，甚至外部类中其他代码也不能访问它。

### 3.5 由外部方法访问变量

与其他内部类相比，局部类的优点在于不仅能够访问外部类的字段，还可以访问局部变量！不过，那些局部变量必须是事实最终变量（effectively final)。这说明，它们一旦赋值就绝对不会更改。

当方法局部变量时，就会把方法的字段的值传递给内部类构造器，并且存储下来。编译器能够检测内部类对局部变量的访问，并为每一个变量建立相应的实例字段，并将局部变量复制到构造器，从而能够初始化这些字段。

### 3.6 匿名内部类

使用内部类时，还可以更进一步。加入只是想创建一个这个类的对象，甚至不需要为类指定名字。这样的类称为匿名内部类。

```java
public void start(int interval, boolean beep) {
    var listener = new ActionListener() {
        public void actionPerformed(ActionEvent event) {
            System.out.println("At the tone, the time is " + Instance.ofEpochMilli(event.getWhen()));
            if(beep) Toolkit.getDefaultTookit().beep();
        }
    };
    var timer = new Timer(interval, beep);
    timer.start();
}
```

它的含义是：创建一个类的对象，这个类实现了接口，需要实现的方法在括号内部定义。

```java
new SuperType(Construction parameters) {
	inner class methods and data;
}
```

其中，SuperType可以时接口，也可以是超类。由于构造器名称必须与类名相同，而匿名内部类没有名称，所以匿名内部类没有构造器。实际上，构造参数需要传递给超类构造器。

如果构造参数列表的结束小括号后跟着一个开始大括号，就是在定义匿名内部类。

虽然匿名内部类不能有构造器，但是可以有一个对象初始化块。

如今，内部类也可以使用lambda表达式来替换。并且显得简洁，可读性强。

### 3.7 静态内部类

有时候，使用内部类只是为了把一个类隐藏在另一个类的内部，并不需要内部类有外围对象的一个引用。为此，可以考虑把内部类声明为static，这样就不会产生那个引用。

只有静态内部类可以声明为是static。静态内部类类似于其他内部类，不过静态内部类没有引用外围的对象。

只要内部类不需要访问外部类对象，就应该使用静态内部类。

与常规内部类不同，静态内部类可以有静态字段和方法。

**在接口中声明的内部类自动是static和public。**

## 四、服务加载器

JDK提供了一个加载服务的简单机制。这种机制由Java平台模块系统提供支持。

通常提供一个服务时，程序希望服务设计者能有一些自由，能够确定如何实现服务的特性。另外希望有多个选择可以实现。

好吧，这里暂时我看不懂？？？

## 五、代理

利用代理可以在运行时创建一组给定接口的新类。只有在编译期无法确定需要实现哪个接口时才有必要使用代理。

### 5.1 何时使用代理？

假如想构造一个类的对象，这个类实现了一个或多个接口，但是在编译器可能并不知道接口到底是什么。

这样就导致了不能实例化接口，只能在运行时定义一个新类。而代理机制则是一种更好的解决方案。代理类可以在运行时创建全新的类。这样的代理类就能够实现我们指定的全部接口。

代理类需要包括指定接口所需要的全部方法，Object类中全部方法，如toString,equals等

不过不能在运行时为这些方法定义新代码。实际上，必须提供一个调用处理器（invocation handler）。调用处理器是为了实现InvocationHandler接口类的对象。这个接口只有一个方法：

`Object invoke(Object proxy, Method method, Object[] args)`

无论何时调用代理对象的方法,调用处理器的invoke方法都会被调用,并向其传递Method对象和原调用参数。之后调用的处理必须确定如何处理这个调用。

### 5.2 创建代理对象

要想创建一个代理对象，需要使用Proxy类的newProxyInstance方法。需要传入三个参数

- 一个类加载器（class loader）。作为Java安全模型的一部分，可以对平台和应用类、从因特网上下载的类使用不同的类加载器。
- 一个Class对象数组，每个元素对应需要实现的各个接口；
- 一个调用处理器

```
package cn.homyit.coreJava.chap6.proxy;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.util.Arrays;
import java.util.Random;
public class ProxyTest {
    public static void main(String[] args) {
        var elements = new Object[1000];
        // fill elements with proxies for the integers 1 . . . 1000
        for (int i = 0; i < elements.length; i++) {
            Integer value = i + 1;
            var handler = new TraceHandler(value);
            Object proxy = Proxy.newProxyInstance(
                    ClassLoader.getSystemClassLoader(),
                    new Class[]{Comparable.class}, handler);
            elements[i] = proxy;
        }

        // construct a random integer
        Integer key = new Random().nextInt(elements.length) + 1;

        // search for the key
        int result = Arrays.binarySearch(elements, key);

        // print match if found
        if (result >= 0) {
            System.out.println(elements[result]);
        }
    }
}

/**
 * An invocation handler that prints out the method name and parameters, then
 * invokes the original method
 */
class TraceHandler implements InvocationHandler {
    private Object target;
    public TraceHandler(Object t) {
        target = t;
    }
    @Override
    public Object invoke(Object proxy, Method m, Object[] args) throws Throwable {
        // print implicit argument
        System.out.print(target);
        // print method name
        System.out.print("." + m.getName() + "(");
        // print explicit arguments
        if (args != null) {
            for (int i = 0; i < args.length; i++) {
                System.out.print(args[i]);
                if (i < args.length - 1) {
                    System.out.print(", ");
                }
            }
        }
        System.out.println(")");
        // invoke actual method
        return m.invoke(target, args);
    }
}
```

分析上面的代码：

1. 首先向elements数组中填入整数类型的一个包装器类对象TraceHandler的代理，这个包装器类替我们包装了一个类型。
2. 然后使用代理对象跟踪一个二分查找。
3. 这里先在数组中填充1~1000的代理，然后调用Arrays类的binarySearch方法在数组中查找一个随机整数。
4. 最后打印出匹配的元素。

代理对象属于在运行时定义的一个类（它有一个名字，如$Proxy0）。这个类也实现了Comparable接口。不过，它的compareTo方法调用了代理对象处理器的invoke方法。

![image-20210714111053797](https://gitee.com/cao_ziqiang/img/raw/master/20210714111053.png)

binarySearch方法会有一下调用：

if(elements[i].compareTo(key) < 0) ...

由于数组中填充的是代理对象，所以`compareTo`调用了TraceHandler处理器包装类中的`invoke`方法。

这个方法先会打印方法名和参数，然后在包装的`Integer`对象上调用`compareTo`。

最后因为有一句打印调用。所以又调用了代理对象的toString，也会重定向到调用处理器。

![image-20210714111428325](https://gitee.com/cao_ziqiang/img/raw/master/20210714111428.png)

以上为程序的一次运行结果截图示例。

可以看到二分查找关键字的过程，即每一步都将会查找区间一半的。

同时可以看到，尽管`toString`方法不是由comparable接口定义的,但是也会被代理,所以**某些object方法总是会被代理**

### 5.3 代理类的特性

代理类是程序运行过程中动态创建的。然而，一旦创建，就变成了常规类，与虚拟机中其他类没有区别。

所有的代理类都扩展`Proxy`类。一个代理类只能有一个实例字段--即调用处理器。它在`Proxy`超类中定义。完成代理对象任务所需要的任何额外数据都必须存储在调用处理器中。如上文的`TraceHandler`就包装了实际的对象。

所有代理类都必须覆盖`toString`、`equals`和`hashCode`方法。如同所有代理方法一样，这些方法只是在调用处理器上调用invoke。Object类中的其他方法（如`clone`、`getClass`）没有重新定义。

没有定义代理类的名字，代理类将被生成一个以字符串`$Proxy`开头的类名。

对于一个特定的类加载器和预设的一组接口来说，只能有一个代理类。也就是说，如果需要使用同一个类加载器和接口数组调用两次`newProxyInstance`方法，将会得到同一个类的两个对象。也可以使用getProxyClass方法来获得这个类。

```java
Class proxyClass = Proxy.getProxyClass(null, interfaces);
```

代理类总是public和final。如果代理类实现的所有接口都是public，这个代理类就不属于任何特定的包；

否则，所有非公共的接口都必须属于同一个包，同时，代理类也属于这个包。

可以通过Proxy类的isProxyClass方法检测一个特定的Class对象是否表示一个代理类。

- Object invoke(Object proxy, Method method, Object[] args)

定义这个方法包含的一个动作，你希望只要在代理对象上调用一个方法就完成这个动作。

- static Class<?> getProxyClass(ClassLoader loader, Class<?>... interfaces)

返回实现指定接口的代理类

- static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler handler)

构造实现指定接口的代理类的一个新实例。所有方法都调用给定处理对象的invoke方法

- static boolean isProxyClass(Class<?> cl)

如果cl是一个代理类则返回true。