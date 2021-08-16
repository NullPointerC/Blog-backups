---
title: Core-Java学习笔记-4
date: 2021-07-11 08:49:56
categories: Java
tags: [Java,backend,计算机基础]
---

> Core-Java第五章 继承
>
> 第四章主要阐述了类和对象的概念，而这一章主要介绍继承，继承是根据已有的类来创建出新的类

## 一、类、超类和子类

"is -a"关系是继承的一个明显特征

### 1.1 定义子类

使用关键字extends表示继承。表示每正在构造的新类派生与一个已存在的类。已存在的类称为超类（superclass）、基类（base class）或者父类（parent class）；新类称为子类(subclass)、派生类（derived class）或者孩子类（child class）。

通过扩展超类定义子类的时候，只需要指出子类和超类的不同即可。在设计之时，往往把最一般的方法放在超类之中，将更特殊的放在子类之中。

### 1.2 覆盖方法

超类中的方法对子类不一定适用时，就需要提供一个新的方法来覆盖（override）超类中的这个方法。

```java
@Override
public double getSalary() {
    double baseSalary = super.getSalary();
    return baseSalary + bonus;
}
```

如上所示，这里使用了**`super`**关键字，这是因为只有Employee类的方法才可以访问Employee类的私有字段。所以我们需要使用super关键字调用父类的getSalary（）方法

### 1.3 子类构造器

```java
/**
 * @param name   the employee's name
 * @param salary the salary
 * @param year   the hire year
 * @param month  the hire month
 * @param day    the hire day
 */
public Manager(String name, double salary, int year, int month, int day) {
    super(name, salary, year, month, day);
    bonus = 0;
}
```

由于Manager的构造器不能访问Employee类的私有字段，所以必须通过一个构造器来初始化这些私有字段。可以利用特殊的super语法调用这个构造器。

使用super调用构造器的语句必须是子类构造器的第一条语句。

如果子类构造器没有显式地嗲用超类的构造器，将会自动地调用超类的无参构造器。

如果超类又没有无参构造器，子类又没有显式地调用超类其他构造器，将会编译出错。

一个对象变量可以指示多种实际类型地现象称为多态（polymorphism）。在运行时能够自动地选择适当的方法，称为动态绑定（dynamic binding）。

### 1.4 继承层次

一个祖先类可以派生出多个子类。

### 1.5 多态

is-a规则的另一种表述是替换原则。它指出程序中出现超类对象的任何地方都可以使用子类对象替换。

### 1.6 理解方法调用

调用x.f(args)时，隐式参数x声明为类C的一个对象。

1. 编译器查看对象的声明类型和方法名。可能存在多个名为f但是参数类型不一样的方法。编译器将会意义列举C类中所有名为f而且可以访问的方法（超类的私有方法不可访问）。至此，编译器已知道所有可能被调用的候选方法；
2. 编译器确定方法调用中提供的参数类型。如果在所有名为f的方法中存在一个与所提供参数类型完全匹配的方法，就选择这个方法。这个过程称为重载解析（overloading resolution)。由于允许类型转换，所以情况可能变得很复杂。如果编译器没有找到与参数类型匹配的方法，或者发现经过类型转换后有多个方法可以与之对应，编译器将会报告一个错误。至此，编译器已经知道需要调用放到名字和参数类型。；
3. 如果是private方法、static方法、final方法或者构造器，那么编译器可以准确地知道应该调用哪个方法，称为静态绑定（static binding）；如果要调用的方法依赖于隐式参数的实际类型，那么必须在运行时使用动态绑定；
4. 程序运行并且采用动态绑定调用方法时，虚拟机必须调用与x所引用对象的实际类型对应的那个方法。每次调用方法都要完成这个搜索，时间开销相当大。因此，虚拟机预先为每个类计算了一个方法表（method table），其中列出了所有方法的签名和要调用的实际方法。在真正调用方法时，虚拟机查表即可；

**在覆盖一个方法的时候，子类方法不能低于超类方法的可见性。**

### 1.7 阻止继承： final类和方法

不允许继承的类被称为final类。在定义类时使用了final修饰符表明这个类是final类。

字段也可以声明为final。对于final字段，构造对象之后就不允许改变他们的值。

如果一个类声明为final，那么其中的方法自动地声明为final，而不包括字段。

### 1.8 强制类型转换

将一个类型强制转换成另一个类型的过程称为强制类型转换。

```java
double x = 3.405;
int nx = (int)x;
```

进行强制类型转换的原因是：要在暂时忽略对象的实际类型之后使用对象的全部功能。

如果将子类的引用赋值给一个超类变量，编译器是允许的，但是如果将一个超类的引用赋值给一个子类变量时，必须进行强制类型转换，这样才可以通过运行时的检查。

但是如果在继承链上进行向下的强制类型转换，并且对象包含的内容不符时，将会产生一个**`ClassCastException`**。

在进行强制类型转换时，应该先查看是否能够成功地转换。为此需要使用instanceof操作符

```java
if(staff[1] instanceof Manager) {
	boss = (Manager)staff[1];
}
```

- 只能在继承层次内进行强制类型转换；
- 在将超类强制转换为子类之前，应该使用instanceof进行检查；

### 1.9 抽象类

如果自下而上在类的继承层次结构中上移，位于上层的类更具有一般性，可能更具有抽象性。

包含一个或多个抽象方法的类本身必须被声明为抽象类。

抽象方法充当着占位方法的角色，它们在子类中具体实现。扩展抽象类可以有两种选择。

一种是在子类中保留抽象类的部分或所有的抽象方法仍未定义，这样就必须将子类也标记为抽象类；

另一种是定义全部方法，这样一来，子类就不是抽象的了；

即使不含有抽象方法，也可以把类声明为抽象类。抽象类不能实例化，也就是说，如果把类声明为抽象类，就不能创建这个类的实例对象。

可以创建一个抽象类的对象变量，但这样的一个变量只能引用非抽象子类的对象。

### 1.10 受保护访问

任何声明为private的内容对其他类都是不可见的。子类也不能访问超类的私有字段。

有些时候，希望限制超类某个方法只允许子类访问，或者更少见地，可以希望允许子类地方法可以访问超类的某个字段。为此，需要将这些类方法或字段声明为受保护（protected）。

|   修饰符    |           权限范围           |
| :---------: | :--------------------------: |
|   private   |         仅对本类可见         |
|   public    |       对外部类完全可见       |
|  protected  |     对本包和所有子类可见     |
| （default） | 对本包可见，也称为缺省修饰符 |

## 二、object类

object类是所有类的始祖，每个类都扩展了object类。

### 2.1 object类型的变量

可以使用object类型的变量引用任何类型的对象。

但是object类型的变量只能用于作为各种值的一个泛型容器。要想对其中内容进行具体的操作，还需要知道对象的原始类型，并且进行强制类型转化。

在Java中，只有基本数据类型（primitive type)不是对象,其他所有的类型，数值、字符、布尔都不是对象。

**其他类都扩展了object类。**

### 2.2 equals方法

equals方法用于检测一个对象是否等于另外一个对象。Object类中实现的equals方法将确定两个对象引用是否相等。

在子类中定义equals方法时，首先调用超类的equals。如果检测失败，对象就不可能相等。

### 2.3 相等测试与继承

1. 自反性：对于任何非空引用x,x.equals(x)应该返回true
2. 对称性：对于任何引用x和y,当且仅当y.equals(x)返回true时，x.equals（y)才返回true
3. 传递性：对于任何引用x、y、z,如果x.equals(y)返回true，y.equals(z)返回true，x.equals(z)也应该返回true
4. 一致性：如果x和y的引用对象没有发生变化，反复调用x.equals(y)应该返回同样的结果
5. 对于任何非空引用x,x.equals(null)应该返回false。

编写equals方法的建议：

1. 显示参数命名为otherObject，稍后需要将它强制转换成另一个名为other的变量。
2. 检测this与otherObject是否相等；if(this == otherObject) return true;
3. 检测otherObject是否为null，如果为null，返回false。if(otherObject == null) return false;
4. 比较this与otherObject的类。如果equals的语义可以在子类中改变，就使用getClass检测;if(getClass() != otherObject.getClass()) return false;如果所有的子类都有相同的相等性语义，可以使用instanceof检测：if(!(otherObject instanceof ClassName)) return false;
5. 将otherObject强制转换为响应类类型的变量：ClassName other = (ClassName) otherObject
6. 现在根据相等性概念比较字段，使用==比较基本数据类型，使用equals比较对象。所有的都匹配，则返回true，否则返回false。

### 2.4 hashCode方法

散列码（hash code）是由对象导出的一个整型值。散列码是没有规律的。

每个对象都有一个默认的散列码，其值由对象的存储地址得出。

### 2.5 toString方法

返回对象值的一个字符串。

只要对象与一个字符串通过操作符“+”连接起来。编译器就会自动地调用toString方法来获得这个对象的字符串描述。可以不写x.toString(),而写作“”+x。与toString相比，这条语句即使是基本数据类型也可以执行。

## 三、泛型数组列表

### 3.1 声明数组列表

```java
ArrayList<Employee> staff = new ArrayList<Employee>();
```

数组列表始终管理着一个内部的对象引用数组。最终这个数组空间可能用尽，到那时数组列表会自动地创建一个更大的数组，并将所有的对象从较小的数组拷贝到较大的数组中。

### 3.2 访问数组列表元素

set方法设置元素

get方法访问元素

### 3.3 类型化与原始数组列表的兼容性

## 四、对象包装器与自动装箱

有时，需要将基本数据类型转换为对象。所有的基本数据类型都有一个与之对应的包装类（wrapper）。包装类是不可变的，一旦构建了包装类，就不允许更改包装类在其中的值。同时包装类还是final类，不允许派生子类。

如果将一个基本数据类型赋值为对象时，称为子哦对那个装箱，将对象赋给基本数据类型称为自动拆箱。

装箱与拆箱是编译器需要做的工作，而不是虚拟机，编译器在生成字节码的时候，会插入必要的方法调用。

## 五、参数数量可变的方法

可以提供参数数量可变的方法（有时这些方法又称为变参方法varargs)

如printf方法

```java
public class PrintStream {
	public PrintStream printf(String fmt, Object... args) {
		return format(fmt,args);
	}
}
```

这里的省略号表示这个方法可以接收任意数量的对象。

实际上，这个方法接收两个参数，一个是格式化字符串，另一个是Object[]数组，其中保存着所有其他参数（如果调用者提供的是整数或者其他基本数据类型的值，会把它们自动装箱为对象）

## 六、枚举类

```java
public enum Size{SMALL, MEDIUM, LARGE, EXTRA_LARGE}
```

实际上，这个声明定义的类型是一个类，它刚好有4个实例，不可能构造新的对象。

因此，在比较两个枚举类型的值时，并不需要调用equals，直接使用“==”就可以了。

也可以为枚举类型添加构造器，方法和字段。当前，构造器只在构造枚举常量的时候使用。

```java
public enum Size {
    SMALL("S"), MEDIUM("M"), LARGE("L"), EXTRA_LARGE("XL");

    private Size(String abbreviation) {
        this.abbreviation = abbreviation;
    }

    public String getAbbreviation() {
        return abbreviation;
    }

    private String abbreviation;
}
```

枚举的构造器总是私有的。所有的枚举类型都是Enum类的子类。它们继承了这个类的许多方法。

toString()方法将会返回枚举常量名。

toString()的逆方法时静态方法valueOf.可以将字符串转为枚举常量

每个枚举类都有一个静态的values方法，它将返回一个包含全部枚举值的数组。

```java
Size[] values = Size.values();
```

- static Enum valueOf(Class enumClass, String name)

返回给定类中有指定名字的枚举常量

- String toString()

返回枚举常量名

- int ordinal()

返回枚举常量在enum声明中的位置,位置从0开始计数

- int compareTo(E other)

如果枚举常量出现在other之前,返回一个非负数;如果this==other,则返回0,否则,返回一个正整数。枚举常量的出现次序在enum声明中给出。

## 七、反射

反射库（reflection library）提供了一个丰富且精巧的工具集，可以用来编写动态操纵Java代码的程序。

能够分析类能力的程序称为反射（reflective）。反射的机制极其强大，可以用在运行时动态分析类的能力，在运行时检查对象，实现泛型数组操作代码，利用method对象。

### 7.1 Class类

在程序运行期间，Java运行时系统始终会为所有对象维护一个运行时类型标识。这个信息会跟踪每个对象所属的类。虚拟机利用运行时类型信息选择要执行的正确的方法。

可以使用Object类中的getClass()方法返回一个Class类型的实例；也可以hi用静态方法forName获得类名对应的Class对象；还可以使用使用T.class获得Class对象。

**Class类实际上是一个泛型类。**

虚拟机为每个类型管理一个唯一的Class对象。因此，可以使用==运算符实现两个类对象的比较。

如果有一个Class类型的对象，可以用它构造类的实例。调用getConstructor方法将得到一个Constructor类型的对象，返回使用newInstance()方法来构造一个实例。

### 7.2 声明异常入门

异常有两种类型的异常：非检查型（unchecked）异常和检查型（checked）异常。对于检查型异常，编译器将会检查程序员是否知道这个异常并做好准备来处理后果。

### 7.3 资源

Class类提供了查找资源文件的方法

1. 获得拥有资源的类的Class对象；
2. 调用方法，如getResource(URL url)，接收描述资源位置的URL；
3. 否则，使用getResourceAsStream方法得到一个输入流来读取文件中的数据。

- URL getResource(String name)

- InputStream getResourceAsStream（String name)

找到与类位于同一个位置的资源，返回一个可以用来加载资源的URL或者输入流。如果没有找到资源，则返回努力了，所以不会抛出异常或者I/O错误

### 7.4 利用反射分析类的能力

在java.lang.reflect包中有三个类，Field,Method和Constructor分别用于描述类的字段、方法和构造器。这三个类都有一个叫做getName方法，用来返回字段、方法和构造器的名称。

Field类有一个`getType`方法，可以用来描述字段类型的一个对象，这个对象类型同样是Class。Method和Constructor类有报告参数类型的方法，Method类还有一个报告返回类型的方法。

这三个类都有一个名为`getModifiers`的方法，它将返回一个整数，用不同的0/1位来描述所使用的修饰符，如public和static。另外还可以使用Modifier类中的静态方法分析getModifiers方法返回的整数。如isPublic,isPrivate或者isFinal

Class类中的getFields,getMethods和getConstructors方法将分别返回这个类支持的公共字段、方法和构造器的数组，其中包括超类的公共成员。getDeclareFields、getDeclareMethods和getDeclareConstructors方法将返回类中声明的全部字段、方法和构造器数组，其中包括私有成员、包成员和受保护成员，但不包括超类成员。

- Field[] getFields()
- Field[] getDeclaredFields()

getFields方法将返回一个包含Field对象的数组，这些对象对应这个类或其超类的公共字段。getDeclaredField方法也返回包含这个Field对象的数组，这些对象对应这个类的全部字段。如果类中没有字段，或者Class对象描述的都是基本数据类型或数组类型，这些方法将返回一个长度为0的数组。

- Method[] getMethods()
- Method[] getDeclaredMethods()

返回包含Method对象的数组：getMethods将返回所有的公共方法，包括从超类继承而来的公共方法；getDeclaredMethods返回这个类或接口的全部方法，但不包括由超类继承的方法。

- Constructor[] getConstructors()
- Constructor[] getDeclaredConstructors()

返回包含Constructor对象的数组，其中包括Class对象所表示的类的所有公共构造器或全部构造器

- String getPackageName()

得到包含这个类型的包的包名，如果这个类型是一个数组类型，则返回元素类型所属的包，或者如果这个类型是一个基本类型，则返回“java.lang”。

- Class getDeclaringClass()

返回一个Class对象,表示定义了这个构造器、方法和字段的类。

- Class[] getExceptionTypes()

返回一个Class对象数组，其中各个对象表示这个方法所抛出异常的类型。

- int getModifiers()

返回一个整数，描述这个构造器、方法或字段的修饰符。使用Modifier类中的方法来分析这个返回值

- String getName()

返回一个表示构造器名、方法名或字段名的字符串

- Class[] getParameterTypes()

返回一个Class对象数组、其中各个对象表示参数的类型。

- Class getReturnType()

返回一个用于表示返回类型的Class对象。

### 7.5 使用反射在运行时分析对象

使用Field类的get方法查看一个对象的字段。

### 7.6 使用反射编写泛型数组代码

### 7.7 调用任意方法和构造器

可以使用Method类的invoke方法，允许我们调用包装在当前method对象中的方法。

- public Object invoke(Object implicitParameter, Object[] explicitParameters)

调用这个对象描述的方法，传入给定参数，并返回方法的返回值。对于静态方法，传入null作为隐式参数。使用包装器传递基本数据类型值。基本数据类型的返回值必须是未包装的。

## 八、继承的设计技巧

1. 将公共操作和字段放在超类中。
2. 不要使用受保护的字段。
3. 使用继承实现“is-a"关系。
4. 除非所有继承的方法都有意义，否则不要使用继承。
5. 在覆盖方法时，不要改变预期的行为。
6. 使用多态，而不要使用类型信息。
7. 不要滥用反射，反射会绕过编译器检查，更难以维护。

