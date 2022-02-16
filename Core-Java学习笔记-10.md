---
title: Core-Java学习笔记-10
categories:
  - Java
tags:
  - Java
  - backend
  - 计算机基础
abbrlink: b4ae475e
date: 2021-07-20 15:44:40
---

> Core-Java第12章并发

多任务是操作系统的一种能力，看起来在同一时刻运行多个程序。多线程在更低一层扩展了多任务的概念：单个程序看起来同时完成多个任务。每个任务在一个线程（thread）中运行。

## 一、什么是线程

线程是一个程序的组成部分。

不要调用Thread类或者Runnable对象的run方法。直接调用只会在一个线程中执行这个任务。应该启动线程的start方法。

## 二、线程状态

- New(新建)
- Runnable(可运行)
- Blocked(阻塞)
- Waiting(等待)
- Timed waiting(计时等待)
- Terminated(终止)

getState获取线程状态

### 2.1 新建线程

new创建一个新线程，还没有开始就一直处于新建状态

### 2.2 可运行线程

一旦调用start方法，就进入可运行状态。一个可运行状态可能在运行也可能没有在运行，要由OS为线程提供运行时间。一个线程开始运行，不一定始终保持运行，运行中的线程可能会暂停，让其他线程有机会运行。线程调度的细节依赖OS提供的服务。抢占式调度系统给每一个可运行线程一个时间片执行任务。时间片用完，OS剥夺线程的运行权，并让下一个优先级高的线程运行。

### 2.3 阻塞和等待线程

当线程处于阻塞或等待状态，线程暂时不活动。不运行任何代码，消耗最少的资源，由线程调度器重新激活这个线程。

- 当一个线程视图获取一个**内部的对象锁**，而这个锁目前被其他线程占有，这个线程就会被阻塞。当其他线程释放了这把锁，并且线程调度器允许线程持有这把锁时，它将变成非阻塞状态。
- 当线程在等待另一个线程通知调度器出现一个条件时，这个线程会进入等待状态。如调用Object.wait方法或Thread.join方法，或是等待Lock或Condition时，就会出现这种情况。
- 有几个方法有超时参数，调用这些方法会让线程进入计时等待状态。这一状态会一直保持到超时期满或接收到适当的通知。如Thread.sleep、计时版的Object.wait、Thread.join、Lock.tryLock以及Condition.await。

![image-20210720161936464](https://gitee.com/cao_ziqiang/img/raw/master/20210723094258.png)

### 2.4 终止线程

- run方法正常退出，线程自然终止；
- 一个没有捕获的异常终止了run方法，使线程意外终止

stop方法已经废弃，不要在自己的代码中调用废弃的方法。

Thread类：

- Thread(Runnable target)

构造一个新线程，调用指定目标的run方法。

- void start()

启动线程，从而调用run方法。立即返回，新线程并发执行。

- void run()

调用相关Runnable的run方法。

- static void sleep(long millis)

休眠指定的毫秒数

- static void yield)

使当前正在执行的线程向另一个线程交出运行权。

- void join()

等待终止指定的线程

- void join(long millis)

等待指定的线程终止或者等待经过指定的毫秒数

- Thread.State getState()

得到这个线程的状态；取值为NEW、RUNNABLE、BLOCKED、WAITING、TIMED_WAITING或TERMINATED。

- void stop

停止该线程。（已废弃）

- void suspend()

暂停这个线程的执行。（已废弃）

- void resume()

恢复线程。只能在调用suspend后调用。（已废弃）

## 三、线程属性

### 3.1 中断线程

run执行完毕、return或出现异常都会是线程中断。stop方法现已废弃。除了stop方法，**没有办法强制终止一个线程**。

interrupt方法可以请求终止一个线程。当线程调用interrupt方法时，就会设置中断状态。

- void interrupt()

向线程发送中断请求。线程的中断状态将被设置为true。如果当前线程被一个sleep调用阻塞，抛出InterruptedException。

- static boolean interrupted()

测试当前线程是否被中断。会将当前线程的状态设置为false。

- boolean isInterrupted()

测试线程是否被中断。与static interrupted方法不同，这个调用不改变中断状态。

- static Thread currentThread()

返回表示当前正在执行的线程的Thread对象。

### 3.2 守护线程

setDaemon(true)；将一个线程转换为守护线程。

当只剩下守护线程时，虚拟机退出。

### 3.3 线程名

setName为线程设置名称

### 3.4 未捕获异常的处理器

**run方法不能抛出任何检查型异常**。但是非检查型异常可能会使线程终止。这种情况下，线程死亡。对于可以传播的异常，没有catch子句。可以用setUncaughtExceptionHandler为所有线程安装一个默认的处理器。

### 3.5 线程优先级

每个线程有优先级。默认情况下，线程会继承构造它的线程的优先级。可以用setPriority方法提高或降低优先级。

优先级为1-10之间的数字，默认为5。

## 四、同步

两个或两个以上线程需要共享同一数据的存取，取决于线程访问数据的次序，可能会导致对象被破坏，这种情况称为竞态条件。

### 4.1 一个例子

为避免多线程破坏共享数据，需要实现**同步存取**。

### 4.2 竞态条件

两个线程同时试图更新同一个账户时，同时执行指令accounts[to] += amount；

这不是原子操作，如

1. 将accounts[to]加载到寄存器；
2. 增加amount；
3. 将结果写回accounts[to]；

若线程1执行完1，2后被抢占执行权，线程2执行1，2，3后线程1被唤醒完成第3步就存在问题。

如果能确保线程失去控制前方法已经被完成就能够保证线程执行不会破坏。

### 4.3 锁🔒对象

有两种机制可以防止并发访问代码块。Java语言提供了synchronized关键字来确保并发访问。另外Java5引入了ReentrantLock类。synchronized关键字会自动提供一个锁以及相关的“条件”，对于大多数需要显示提供锁的情况，这种机制都很方便。

java.util.concurrent框架为并发提供了单独的类。

使用ReentrantLock保护代码块的基本结构如下：

```java
myLock.lock();
try {
	//critical section;
}finally {
	myLock.unLock();
}
```

这个结构确保任何时刻都只有一个线程进入临界区。一旦有线程锁定了锁对象，其他线程就无法通过lock语句，其他线程调用lock时，它们会暂停，直到第一个线程释放这把锁。

每个Bank对象都有自己ReentrantLock对象。如果两个线程尝试访问同一个Bank对象，那么这把锁可以确保串行化访问。如果两个线程访问不同的对象，两个线程会得到不同的锁对象，那么线程不会堵塞。因为线程在操纵不同对象实例时，线程之间不会相互影响。

这把锁称为重入（reentrant）锁，因为线程可以反复获得已经拥有的锁。锁有一个持有计数（hold count）来跟踪对lock方法的嵌套调用。线程每一次调用lock后都需要unlock来释放锁。由于这个特性，被一个锁保护的代码可以调用另一个使用相同锁的方法。

```java
void lock();
//获得这个锁;如果锁当前被另一个线程占有,则堵塞
void unlock();
//释放这个锁
ReentrantLock();
//构造一个重入锁,可以用来保护临界区
ReentrantLock(boolean fair);
//构造一个采用公平策略的锁。一个公平锁倾向于等待时间最长的线程。不过,这种公平可能影响性能。所以默认情况下不要求锁是公平的。
```

### 4.4 条件对象

通常线程进入临界区是因为只有满足了某个条件才可以执行，这时可以使用一个条件遍历管理那些获得了锁却不能做工作的线程。

例如一个账户没有足够的资金转账，我们就不希望这样的账户转出资金。

在线程再次运行前，账户的余额可能已经低于提款金额，此时必须确保在检查余额到转账之间没有其他线程修改余额。为此，需要提供一个锁来保护这个测试和转账操作：

```java
public void transfer(int from, int to, int amount) {
	bankLock.lock();
    try {
        while(accounts[from] < amount) {
            // wait
        }
        // taansfer funds
    } finally {
        bankLock.unlock();
    }
}
```

当账户中没有足够的资金时，需要等待，直到另一个线程往其中加入了资金。但是这个线程刚才获得了bankLock锁，因此别的线程没有存款的机会。这里就需要引入条件变量。

一个锁对象可以有一个或多个相关联的对象。可以使用newCondition方法获得一个条件对象。

**以下创建了一个条件变量表示“资金充足”条件。**

```java
private Condition sufficientFunds;
sufficientFunds = bankLock.newCondition();
```

如果transfer发现资金不足，则调用await暂停当前线程并放弃锁。

调用await后，线程仍然不会和正常等待锁的线程一样，线程进入这个条件的等待集（wait set）。

当锁可用时，该线程不会变为可运行状态。仍然保持非活动状态，直到另一个线程在同一条件上调用singalAll方法。

当另一个线程完成转账后，应该调用

```java
sufficientFunds.singalAll();
```

这个调用会重新激活等待这个条件的所有线程。当这些线程从等待集中移出时，再次称为可运行的线程，调度器最终再次将它们激活。同时，它们会尝试重新进入该对象。一旦锁可用，它们中的某个线程就从await调用返回，得到锁，并从之前暂停的地方继续执行。

此时，线程应该继续测试条件。不能保证现在一定满足条件，singalAll只是通知等待的线程：现在有可能满足条件，可以再次检查条件。

注意一旦线程调用了await方法后，就没有办法重新自行激活，只能等待其他线程来singalAll。

一旦对象的状态发生变化，就应该调用singalAll方法。

### 4.5 synchronized关键字

- 锁用来保护代码片段，一次只能有一个线程执行被保护的代码。
- 锁用来管理试图进入被保护代码段的线程。
- 一把锁可以有一个或多个相关联的条件对象。
- 每个条件对象管理那些已经进入被保护代码段但还不能运行的线程。

Java语言内置了一种机制，**每个对象都有一个内部锁**。

如果一个方法声明为synchronized，那么对象的锁将会保护整个方法。**即要调用这个方法，线程必须获得内部对象锁。**

```java
public synchronized void method() {
	// method body
}
```

等价于

```java
public void method() {
	this.intrinsicLock.lock();
	try {
		// method body
	} finally {
		this.intrinsicLock.unLock();
	}
}
```

内部锁只有一个关联条件。wait方法将一个线程增加到等待集中，notifyAll或notify方法可以解除等待线程的阻塞。

由于wait、notify、notifyAll都是Object类的final方法。所以Condition必须重命名解决冲突。

将静态方法声明为同步也是合法的。**如果调用这个方法，它会获得相关类对象的内部锁。**

**例如，Bank类内部有一个静态同步方法，调用这个方法时，Bank.class对象的锁会锁定。**

- 不能中断一个正在尝试获得锁的线程。
- 不能指定尝试获得锁时的超时时间。
- 每个锁仅有一个条件可能是不够的。

建议：

- 最好既不使用Lock/Condition也不使用synchronized关键字。在很多情况下，可以使用java.util.concurrent包中的某种机制，会处理所有的锁定。

### 4.6 同步块

线程除调用同步方法获得锁之外。还可以进入同步块获得对象内部锁。

```java
synchronized (obj) {
	//critical section
}
```

它会获得obj的锁🔒。

有时程序员使用一个对象的锁来实现额外的原子操作，这种做法称为客户端锁定（client side locking）。

### 4.7 监视器概念

锁🔒和条件是实现线程同步的强大工具，但是它们不是面向对象的。监视器（monitor），也成为管程。

- 监视器是只包含私有字段的类。
- 监视器类的每个对象有一个关联的锁。
- 所有的方法都由这个锁锁定。即客户端调用obj.method()。那么obj对象的锁在方法调用开始时就自动获得，并且当方法返回时自动释放该锁。因为所有字段都是私有的，也确保了一个线程在处理字段时，没有其他的线程可以访问这些字段。
- 锁可以有任意多个相关联的条件。

但是Java的设计者并没有严格遵守管程的概念：

- Java中的每一个对象都有一个内部锁和一个内部条件。
- 如果一个方法用synchronized声明，那么，它就表现得像是一个监视器方法。
- 可以通过调用wait、notify、notifyAll来访问条件变量。

但是Java实际上确实这样实现的：

- 字段不要求是private
- 方法不要求是synchronized
- 内部锁对用户可见

### 4.8 volatile字段

- 有多处理器的计算机能够暂时在寄存器或本地内存缓存中保存内存值.但是,运行在不同处理器上的线程可能看到同一个内存位置有不同的值。
- 编译器可以改变指令的执行顺序以使吞吐量达到最大化。

volatile关键字为实例字段的同步访问提供了一种免锁机制。如果一个字段被声明为volatile，那么编译器和虚拟机就知道该字段可能被另一个线程并发更新。编译器会插入适当的代码，以确保如果一个线程对volatile变量做了修改，这个修改对读取这个变量的所有其他线程都可见。

**但是volatile不能保证原子性。**

### 4.9 final变量

将字段声明为final时，也可以保证变量的可见性，但是不能保证原子性。有多个线程需要更改和读取这个变量时，仍然需要进行同步。

### 4.10 原子性

假设对共享变量除赋值以外不做其他操作，那么可以将这些共享变量声明为volatile。

java.util.concurrent.atomic包中有很多类使用了很高效的机器级指令（没有使用锁）来保证其他操作的原子性。

如AtomicInteger类提供了incrementAndGet和decrementAndGet，将以原子性的方式将一个整数进行自增或自减。

### 4.11 死锁

因为一组线程竞争资源不当引起所有线程都被阻塞，且不能自我终止的状态称为死锁。

### 4.12 线程局部变量

ThreadLocal辅助类为各个线程提供各自的实例。

线程局部变量的构造方法如下：

```java
public static final ThreadLocal<SimpleDateFormat> dateFormat = ThreadLocal.withInitial(() -> new SimpleDateFormat("yyyy-MM-dd"));
```

要访问具体的格式化方法，可以调用：

```java
String dateStamp = dateFormat.get().format(new Date());
```

在一个给定线程中首次调用get时，会调用构造器中的lambda表达式。在此之后，get方法会返回属于当前线程的实例。

- T get()

得到这个线程的当前值。如首次调用get,会调用initialize来得到这个值。

- void set(T t)

为这个线程设置一个新值

- void remove()

删除对应这个线程的值。

- `static  <S> ThreadLocal <S> withInitial(Supplier<? extends S> supplier)`

创建一个线程局部变量，其初始值通过调用给定的提供者生成

### 4.13 为什么废弃stop和suspend方法

stop和suspend都试图控制一个给定的线程行为，没有线程的互操作。

stop终止所有未结束的方法，包括run方法，线程终止后，会立即释放被它锁定的所有对象的锁。会导致对象处于不一致的状态。

suspend不会破坏对象，但是如果挂起一个持有锁的线程后，运行suspend的方法试图获得**同一个锁**，程序就会进入死锁：被挂起的线程等待被恢复，而将其挂起的线程等待获得锁。

## 五、线程安全的集合

### 5.1 阻塞队列

很多线程问题可以用一个或多个队列描述。如生产者线程向队列插入元素，消费者线程从队列获取元素。使用队列可以安全地从一个线程向另一个线程传递数据。

当试图向队列添加元素而队列满或试图从队列移出元素而队列为空时，阻塞队列将导致线程阻塞。

| 方法    | 正常动作               | 特殊情况下的动作                       |
| ------- | ---------------------- | -------------------------------------- |
| add     | 添加一个元素           | 队列若满，则抛出IllegalStateException  |
| offer   | 添加一个元素并返回true | 队列若满，则返回false                  |
| put     | 添加一个元素           | 队列若满，则堵塞                       |
| remove  | 移除并返回队头元素     | 队列若空，则抛出IllegalStateException  |
| poll    | 移除并返回队头元素     | 队列若空，则返回null                   |
| take    | 移除并返回队头元素     | 队列若空，则阻塞                       |
| element | 返回队头元素           | 队列若空，则抛出NoSuchElementException |
| peek    | 返回队头元素           | 队列若空，则返回null                   |

还有带超时时间的offer方法和poll方法。

LinkedBlockingQueue的容量没有上界，但是也可以指定一个最大容量。

LinkedBlockingDeque是一个双端队列。

ArrayBlockingQueue再构造时需要指定容量，并且有一个可选参数来指定是否需要公平性。

如果设置了公平参数，那么等待了最长时间的线程将会被最先得到处理。

PriorityBlockingQueue是一个优先队列，并不是先进先出队列。元素按照它们的优先级顺序移除。

DelayQueue包含了Delayed接口的对象：

```java
interface Delayed extends Comparable<Delayed> {
	long getDelay(TimeUnit unit);
}
```

getDelay方法返回对象的剩余延迟。负值表示延迟已经结束。元素只有在延迟结束的情况下才能从DelayQueue中移除。还需要实现compareTo方法，DelayQueue使用该方法对元素进行排序。

Java7增加了一个TransferQueue接口，允许生产者线程等待，直到消费者准备就绪可以接收元素。

如果生产者调用q.tarnsfer(item)，将会被阻塞，直到另一个线程将元素（item）删除。LinkedTransferQueue实现了接口。

### 5.2 高效的映射、集和队列

java.util.concurrent包提供了映射、有序集和队列的高效实现：ConcurrentHashMap、ConcurrentSkipListMap、ConcurrentSkipListSet和ConcurrentLinkedQueue。

这些集合使用复杂的算法，通过允许并发地访问数据结果的不同部分尽可能减少竞争。

与大多数集合不一样，这些类的size方法不一定在常量时间内返回集合的大小。确定这些集合的大小需要遍历。

集合返回弱一致性的迭代器。这意味着迭代器不一定能反映出它们构造之后的所有更改，但是它们不会返回同一个值两次，也不会抛出ConcurrentModificationException异常。

### 5.3 映射条目的原子性更新

get和put方法是原子性操作，永远不会破坏数据结构。

### 5.4 对并发散列映射的批操作

Java为并发散列映射提供了批操作，即使有其他线程在处理映射，这些操作也能安全的执行。

批操作会遍历映射，处理遍历过程中找到的元素。

- search（搜索）为每个键或值应用一个函数，直到函数生成一个非null的对象。然后搜索终止，返回这个函数的结果
- reduce（规约）组合所有的键或值，这里要使用所提供的一个累加函数。
- forEach为所有的键或值应用一个函数。

每个操作都有4个版本：

- operationKeys：处理键
- operationValues：处理值
- operation：处理键和值
- operationEntries：处理Map.entry对象

### 5.5 并发集试图

ConcurrentHashMap.newKeySet方法会生成一个Set<K>，这实际上是ConcurrentHashMap<K, Boolean>的一个包装器。所有值都被映射为Boolean.TRUE，不过因为只要把它当成一个集处理，所以不关心它的映射值。

也可以直接使用keySet方法，返回映射的键集。这个键集可以更改，如果删除键集里的元素，映射中的键以及值也会相应删除。但是向其中添加元素没有意义，因为没有相应的值。

还有一个keySet方法可以设置默认值，为集添加元素时可以使用这个方法。

```java
Set<String> words = map.keySet(1L);
words.add("Java");
```

如果“Java”在words中不存在，现在就会有一个默认值1。

### 5.6 写数组的拷贝

CopyOnWriteArrayList和CopyOnWriteArraySet是线程安全的集合。其中所有的更改器都会建立底层数组的一个副本。如果迭代访问集合的线程超过更改集合的线程数，就很有必要。

### 5.7 并行数组算法

静态Arrays.parallelSort方法可以对一个基本数据类型或对象数组进行排序。对对象排序，需要提供Comparator。

parallelSetAll方法会用一个由一个函数计算得到的值填充一个数组。这个函数接收元素索引，然后计算相应位置上的值。

parallelPrefix方法会用一个给定结合操作的相应前缀和的累加结果替换各个数组元素。

```java
Arrays.parallelPrefix(values, (x, y) -> x*y);
```

数组将包括

```java
[1,1*2,1*2*3,1*2*3*4,...]
```

### 5.8 较早的线程安全集合

Vector和HashTable类提供了线程安全的实现。底层通过使用同步包装器变成线程安全的。

```java
List<E> synchArrayList = Collections.synchronizedList(new ArrayList());
Map<K,V> synchHashMap = collections.synchronizedMap(new HashMap());
```

结果集合的方法使用锁加以保护，可以提供线程安全的访问。

## 六、任务和线程池

构造线程的花销很大,如果需要大量线程,应该使用线程池。线程池包括许多准备运行的线程，为线程池提供一个Runnable，就会有一个线程调用run方法。

### 6.1 Callable与Feture

Runnable封装一个异步运行任务，可以将其想象成一个没有参数和返回值的异步方法。Callable与Runnable类似，但是有返回值。Callable接口是一个参数化类型，只有一个方法call。

```java
public interface Callable<V> {
    V call() throws Exception;
	// 运行一个产生结果的任务
}
```

```java
public interface Future<V> {
	V get();
	V get(long time, TimeUnit unit);
	//获取结果，这个方法会阻塞，直到结果可用或超过了指定的时间。如果不成功，第二个方法会抛出TimeoutException
    boolean calcel(boolean mayInterrupt);
    // 尝试取消任务的运行。如果任务已经开始，并且参数为true，就中断。如果成功中断，返回true
    boolean isCalcelled();
    // 如果任务在完成前被取消，则返回true
    boolean isDone();
    // 如果任务结束，无论是正常完成、中途取消、还是发生异常，都返回true
}
```

```java
public class FetureTask<V> implements Future, Runnable {
	FetureTask(Callable<V> task);
	FetureTask(Runnable task, V result);
}
```

### 6.2 执行器

执行器（Executors）类有许多静态工厂方法，用来构造线程池。

| 方法                             | 描述                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| newCachedThreadPool              | 必要时创建线程；空闲时线程会保留60s                          |
| newFixedThreadPool               | 池中包含固定数目的线程；空闲线程会一直保留                   |
| newWorkStealingPool              | 一种适合“fork-join”任务的线程池，其中复杂的任务会分解为更简单的任务，空闲线程会“密取”较简单的任务 |
| newSingleThreadExecutor          | 只有一个线程的“池”，会顺序地执行所提交的任务                 |
| newScheduleThreadPool            | 用于调度执行的固定线程池                                     |
| newSingleThreadScheduledExecutor | 用于调度执行的单线程“池”                                     |

1. 调用Executors类的静态工厂方法来创建线程池；
2. 调用submit提交Runnable或Callable对象；
3. 保存好返回的Future对象，以便得到结果或者取消任务；
4. 当不想提交任何任务时，调用shundown；

### 6.3 控制任务组

### 6.4 fork-join框架

有些应用使用了大量的线程，但是其中大部分是空闲的。fork-jon框架专门用来处理许多子任务的程序。

在后台，fork-join框架使用了一种有效的智能方式来平衡可用线程的工作负载。

## 七、异步计算

这里的内容太多了，暂时先跳过，也看不懂。。。