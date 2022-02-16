---
title: Core-Java学习笔记-6
categories:
  - Java
tags:
  - Java
  - backend
  - 计算机基础
abbrlink: 34f78c49
date: 2021-07-15 09:09:28
---

> CoreJava第7章 异常、断言和日志

## 一、 处理错误

假设一个程序运行期间出现了一个错误，用户期望在出现错误时，程序能够采取合理的行为。如果因为出现错误而使得某些操作没有完成，程序应当可以返回一种安全状态，并能让用户执行其他的命令或者允许用户保存所有的工作结果，并以妥善的方式终止程序。

### 1.1 异常分类



![image-20210715092855871](https://gitee.com/cao_ziqiang/img/raw/master/20210715092855.png)

异常都是派生于`Throwable`类的一个类实例。所有的异常都由`Throwable`**继承**而来。

在下一层被分解为两个分支：`Error`和`Exception`。

`Error`类层次结构描述了Java运行时系统内部的错误和资源耗尽错误。如果程序出现了这种错误，程序员是无法解决的，但是`Error`一般很少出现。

`Exception`又派生为两个分支，由编程错误导致的异常均属于`RuntimeException`，如果程序本身没有问题，由于像I/O这样的错误引起的异常属于`IOException`。

又称派生于`Error`类或`RuntimeException`类的所有异常称为是非检查型（unchecked）异常，所有其他的异常都称为是检查型异常（checked）。

### 1.2 声明检查型异常

如果遇到了无法处理的情况，Java的方法可以抛出一个异常。

1. 调用了一个抛出检查型异常的方法时需要抛出异常；
2. 方法检测到一个错误，需要利用`throw`语句抛出一个检查型异常；
3. 程序出现错误，如a[-1] = 0这样的许局需要抛出异常；
4. Java虚拟机或运行时库出现内部错误会抛出异常；

有些方法包含在类中，对于这些方法，就需要通过方法首部的异常规范声明这个可能抛出的异常。

有多个异常需要抛出时，需要列出所有异常，用逗号分隔开。

不需要声明`Error`类型，也不需要声明`RuntimeException`类型的异常。

这些运行时错误完全可以由程序员控制之下解决，如果担心出错，应该解决，不应该抛出这些异常。

在继承时，如果子类继承了超类的一个方法，子类方法中声明的检查型异常不能比超类方法中声明的异常更加通用。即范围不能超过父类的异常。若超类没有声明异常，子类也不能抛出异常。

### 1.3 如何抛出异常

如果一个已有的异常类能够满足要求，抛出这个异常非常简单，只需要找到一个合适的异常类，创建这个类对象，将对象抛出即可。

### 1.4 创建异常类

如果已有的标准异常类无法描述需要表达的异常，这时我们就可以创建自己的异常类。只需要定义一个派生于`Exception`类，或`Exception`的某个子类，自定义的这个类应该包含两个构造器，一个是默认的构造器，另一个是描述信息的构造器。

- `Throwable()`

构造一个新的`Throwable`对象，但没有详细的描述信息。

- `Throwable(String message)`

构造一个新的`Throwable`对象，带有指定的详细描述信息。按照惯例，所有派生的异常类都支持一个默认构造器和一个带有描述信息的构造器。

- `String getMessage()`

获得`Throwable`对象的详细描述信息。

## 二、捕获异常

### 2.1 捕获异常

有些代码必须捕获异常。如果发了某个异常，但没有在任何地方捕获它，程序就会终止，并在控制台打印信息，包括异常信息和堆栈轨迹。

要想捕获一个异常，需要设置`try-catch`语句块。

```java
try{
	code;
	more code;
	more code;
}catch(ExceptionType e) {
	handler for this type;
}
```

如果`try`语句块中的任何代码抛出了`catch`子句中指定的一个异常类，那么程序将跳过`try`的其余代码，执行`catch`子句中的处理器代码。

如果`try`语句块没有抛出任何异常，那么程序将跳过`catch`子句。

如果`try`语句块抛出了`catch`子句中没有声明的异常，那么这个方法将会退出。

通常，对待方法中异常最好的选择是什么也不做，将异常传递给调用者。但必须声明可能会抛出一个`Exception`。

编译器将会严格执行`throws`说明符。如果调用了一个抛出了<font color='red'>**检查型异常**</font>的方法，就必须处理这个异常，或者继续传递异常。

**一般而言，我们需要捕获我们知道如何处理的异常，而继续传播那些不知道怎样处理的异常。**

如果想继续传播异常，就需要在方法首部添加一个throws说明符，提醒调用者这个方法可能会抛出异常。

但是同样需要注意子类与父类抛出异常的关系。

### 2.2 捕获多个异常

可以为每个异常类型使用一个单独的`catch`子句。

在Java7中，同一个`catch`子句可以捕获多个异常。中间使用`‘|’`运算符分隔开

捕获多个异常时，异常变量隐含为`final`变量。不能为变量赋不同的值

```java
catch (FileNotFoundException | UnknowHostException e) { ... }
```

### 2.3 再次抛出与异常链

可以在`catch`子句中再抛出一个异常。通常，希望改变异常的类型时会选择这么做。

### 2.4 finally子句

如果抛出异常时，则会终止剩余代码执行，并退出方法。如果这个方法已经获得了一些本地资源，那么此时必须要先清理资源。这时可以在`finally`语句块中处理这些资源。

不管是否有异常被捕获，`finally`中的代码都一定会被执行。

如果`finally`语句中也包含有`return`语句，那么最后执行的是`finally`语句中的`return`语句。

所以应当尽量把处理资源的语句放入`finally`中，不要把控制流语句放入`finally`子句中。

### 2.5 try-with-Resources

若资源属于一个实现了`AutoCloseable`接口的类，Java7就提供了更快捷的方式。

接口中有一个方法

- `void close() throws Exception`

语句形式为：

```java
try(Resource res = ..) {
	work with res;
}
```

`try`块退出后，会自动调用`res.close()`。

```java
try (var in = new Scanner(
	new FileInputStream("/usr/share/dict/words"), StandardCharsets.UTF_8);
    var out = new PrintWriter("out.txt", StandardCharsets.UTF_8)) {
	while(in.hasNext()) {
        out.println(in.next().toUpperCase());
    }		
}
```

不论这个块如何退出，`in`和`out`都会关闭。

在Java9中，可以在try首部提供之前声明的事实最终变量:

```java
public static void printAll(String[] lines, PrintWriter out) {
	try(out) {
		// effectively final variable
        for(String line : lines) {
            out.println(line);
        }
    }
    // out.close() called here
}
```

### 2.6 分析堆栈轨迹元素

堆栈轨迹是程序执行过程中某个特定点上所有挂起的方法调用的一个列表。

可以使用`Throwable`类的`printStackTrace`方法访问堆栈轨迹的文本描述信息。

或可以使用`StackWalker`类，，生产一个栈帧（`StackFrame`）实例流，其中每个实例描述一个栈帧。

```java
StackWalker walker = StackWalker.getInstance();
walker.foreach(frame -> analyze frame);
```

利用`StackFrame`类可以得到所执行代码行的文件名和行号，以及对象名和方法名。

## 三、使用异常的技巧



1. 异常处理不能代替简单的测试；
2. 不要过分细化异常；
3. 充分利用异常层次结构；
4. 不要压制异常；
5. 在检测错误时，“苛刻”比放任更好，检测到了错误时，应该抛出异常而不是选择默认值；
6. 不要羞于传递异常，最好是继续传递异常，而不是自己捕获。

## 四、使用断言

### 4.1 断言的概念

断言机制允许在测试期间向代码中插入一些检查，而在生产代码中会自动删除这些检查。

```java
assert condition;
assert condition : expression;
```

会传入计算条件，若false，则抛出异常。

在第二个语句中，表达式将传入`AssertionError`对象的构造器中，并转换成一个消息字符串。

### 4.2 启用和禁用断言

在默认情况下，断言是禁用的。需要在程序运行时加上参数。“`-enableassertions`”或“`-ea`”选项启用断言。

```bash
java -enableassertions MyApp
```

启用或禁用是类加载器的事，不必重新编译源程序。

也可以在某个类或整个包中都启用断言。

```bash
java -ea:MyClass -ea:com.mycompany.mylib MyApp
```

以上命令将会为`MyClass`类以及`com.mycompany.mylib`包和它的子包中的所有类都打开断言。选择"`-ea`"将打开无名包中所有类的断言。

也可以使用选项“`-disableassertions`”或“`-da`”在某个特定包或类中禁用断言

```bash
java -ea:... -da:MyClass MyApp
```

有些类不由虚拟机加载，由JVM直接加载的。可以使用这些开关有选择的启用或禁用断言。

### 4.3 使用断言完成参数检查

断言失败是致命的，不可恢复的错误，断言只是在开发和测试阶段打开。

对于有前置条件的方法应该使用断言。

### 4.4 使用断言提供假设文档

## 五、日志

### 5.1 基本日志

要生成简单的日志，可以使用全局日志记录器(global logger)并调用其info方法：

```java
Loger.getGlobal().info("FIle->Open menu item selected");
```

### 5.2 高级日志

在专业的日志中，会有日志记录器logger，可以调用getLogger方法创建或获取日志记录器

```java
private static final Logger myLogger = Logger.getLogger("com.mycompany.myapp")
```

任何未被变量引用的日志记录器都有可能会被回收，所以要使用静态变量存储日志记录器的引用。

与包名类似，日志记录器也有层次结构。事实上，与包相比，日志记录器的层次性更强。对于包来说，包与父包之间没有语义关系，但是日志记录器的父和子之间将会共享属性。例如对父包日志记录器设置了日志级别，子包记录器也会继承这个级别。

通常日志有7个级别

- `SEVERE`
- `WARNING`
- `INFO`
- `CONFIG`
- `FINE`
- `FINER`
- `FINEST`

在默认情况下，只记录前面三个级别。也可以使用`setLevel`方法来设置不同级别。

```java
logger.setLevel(Level.FINE);
```

此时，`FINE`以所有更高级的日志都会被记录。

还可以使用`Level.ALL`开启所有级别的日志记录，或使用`Level.OFF`关闭所有级别的日志。

所有级别都有日志记录方法，如：

```java
logger.warning(message);
logger.fine(message);
```

或者，可以使用log方法并指定级别

```java
logger.log(Level.FINE, message);
```

### 5.3 修改日志管理器配置

可以通过编辑配置文件来修改日志系统的各个属性。文件位于`conf/logging.properties`

### 5.4 本地化

即一个程序须包含多个资源包，将语言消息映射放于`logmessages_xx.properties`文件中。

请求日志记录器时，可以指定一个资源包

```java
Logger logger = Logger.getLogger(loggerName, "com.mycompany.logmessages");
```

### 5.5 处理器

在默认情况下，日志记录器将记录发送到`ConsoleHandler`，并由它输出到`System.err`流。具体地，日志记录器会把记录发送到父处理器，最终由`ConsoleHandler`处理。

处理器也有日志级别。对于要记录的日志，它的级别必须高于日志处理器和处理器二者的阈值。默认也为`INFO`。

### 5.6 过滤器

默认情况下，会根据级别进行日志过滤。每个日志记录器和处理器都有一个可选的过滤器来完成附加的过滤。要定义需要实现`Filter`接口并定义`isLoggable(LogRecord record)`方法即可。最后使用`setFiltter`方法安装到记录器或处理器中。

### 5.7 格式化器

可以选择扩展`Formatter`类并覆盖`String format(LogRecord record)`方法。可以根据自己的需要来对信息进行格式化，并返回结果字符串。

```java
String formatMessage(LogRecord record)
```

很多文件格式需要加上头部和尾部就需要覆盖

```java
String getHead(Handler h)
String getTail(Handler h)
```

最后使用`setFormatter`方法将格式化器安装到处理器中。

### 5.8 日志技巧

1. 对于一个简单的应用，选择一个日志记录器。可以把记录器命名为和主应用包名一样的名字。

```java
Logger logger = Logger.getLogger("com.mycompany.myprog");
private static final Logger logger = Logger.getLogger("com.mycompany.myprog");
```

2. 默认的日志配置会把级别大于等于INFO的所有信息记录到控制台。
3. 最好只选择对程序用于有意义的消息显示。

PS：日志这里内容太多了，且很多涉及到后面的知识，暂时没有完全看懂。。。

## 六、调试技巧

1. 可以使用`System.out.println("x="+x);`或`Logger.getGlobal().info("x="+x);`打印或记录任何变量的值；
2. 可以在每个类中放置一个单独的`main`方法。这样就可以提供一个单元测试桩（stub），能够独立测试类；
3. 使用JUnit单元测试框架；
4. 日志代理（logging proxy）是一个子类的对象，可以截获方法调用，记录日志，然后调用超类中的方法。

```java
var generator = new Random() {
    public double nextDouble() {
        double result = super.nextDouble();
        logger.getGlobal().info("nextDouble: " + result);
        return result;
    }
};
```

只要调用了`nextDouble`方法，就会生成一个日志消息。

5. 利用`Throwable`类的`printStackTrace`方法，可以从任何异常对象中获取堆栈轨迹。
6. 堆栈轨迹一般显示在`System.err`上，如果想记录或显示，则可以捕获到字符串中；
7. 通常可以把程序错误记录进文件中。
8. 可以使用`Thread.setDefaultUncaughtExceptionHandler`改变未捕获的处理器。
9. 要想观察类加载过程，启动虚拟机时可使用“`-verbose`”标志。
10. “`-Xlint`”选项可以告诉编译器找出常见的代码问题。
11. Java虚拟机中增加了对Java应用的监控（monitoring）和管理（management）支持，允许在虚拟机中安装代理来跟踪内存消耗、线程使用、类加载等情况。
12. Java任务控制器是一个专业级性能分析和诊断工具，包含在openjdk中，可以免费使用。



