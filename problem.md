<!-- TOC -->

- [1、Java静态内部类和非静态内部类](#1java静态内部类和非静态内部类)
- [2、java并发之TimeUnit理解](#2java并发之timeunit理解)
- [3、CAS算法](#3cas算法)
- [4、volatile：volatile关键字只保证可见性，不保证原子性](#4volatilevolatile关键字只保证可见性不保证原子性)
- [5、硬盘、内存、CPU缓存：速度由慢到快](#5硬盘内存cpu缓存速度由慢到快)

<!-- /TOC -->
## 1、Java静态内部类和非静态内部类
**1.1 区别：** 
* 静态内部类只能访问静态成员和静态方法，普通的内部类可以访问任意方法。
* 静态内部类可以声明静态方法，但是普通内部类不可以声明静态方法。
* 静态内内部可以定义静态成员变量，但是普通类只能定义final static的静态变量。
* 静态内部类的对象构造不依赖于外部类，而普通内部类在构造器必须先构造所依赖的外部类的对象。

**1.2、为什么java要分内部类和静态内部类**
* 其实个人来看。内部类在很多时候都是为了降低包的复杂度的作用，因为java写着写着程序就会变得非常大。比如匿名内部类，也就通常只用这一次，不用再建个文件专门写。内部类，使用的权限更大，静态内部类，独立性更强，一定的使用权限。

## 2、java并发之TimeUnit理解
转载自 ： https://www.cnblogs.com/shamo89/p/7055094.html
TimeUnit是java.util.concurrent包下面的一个类，TimeUnit提供了可读性更好的线程暂停操作，通常用来替换Thread.sleep()，在很长一段时间里Thread的sleep()方法作为暂停线程的标准方式，几乎所有Java程序员都熟悉它，事实上sleep方法本身也很常用而且出现在很多面试中。如果你已经使用过Thread.sleep()，当然我确信你这样做过，那么你一定熟知它是一个静态方法，暂停线程时它不会释放锁，该方法会抛出InterrupttedException异常（如果有线程中断了当前线程）。但是我们很多人并没有注意的一个潜在的问题就是它的可读性。Thread.sleep()是一个重载方法，可以接收长整型毫秒和长整型的纳秒参数，这样对程序员造成的一个问题就是很难知道到底当前线程是睡眠了多少秒、分、小时或者天。看看下面这个Thread.sleep()方法：

Thread.sleep（2400000）
 
粗略一看，你能计算出当前线程是等待多长时间吗？可能有些人可以，但是对于大多数程序员来说这种写法的可读性还是很差的，你需要把毫秒转换成秒和分，让我们来看看另外一个例子，这个例子比前面那个例子可读性稍微好一点：

Thread.sleep(4*60*1000);
 
这比前面那个例子已经好多了，但是仍然不是最好的，你注意到睡眠时间用毫秒，不容易猜出当前线程将等待4分钟。TimeUnit类解决了这个问题，通过指定DAYS、HOURS、MINUTES,SECONDS、MILLISECONDS和NANOSECONDS。java.utils.concurrent .TimeUnit 是Java枚举应用场景中最好的例子之一，所有TimeUnit都是枚举实例，让我们来看看线程睡眠4分钟用TimeUnit是如何使用的。

TimeUnit.MINUTES.sleep(4);  // sleeping for 4 minutes
 
类似你可以采用秒、分、小时级别来暂停当前线程。你可以看到这比Thread的sleep方法的可读的好多了。记住TimeUnit.sleep()内部调用的Thread.sleep()也会抛出InterruptException。

## 3、CAS算法
原文：https://blog.csdn.net/liubenlong007/article/details/53761730 

* 一个CAS方法包含三个参数CAS(V,E,N)。V表示要更新的变量，E表示预期的值，N表示新值。只有当V的值等于E时，才会将V的值修改为N。如果V的值不等于E，说明已经被其他线程修改了，当前线程可以放弃此操作，也可以再次尝试次操作直至修改成功。基于这样的算法，CAS操作即使没有锁，也可以发现其他线程对当前线程的干扰（临界区值的修改），并进行恰当的处理。

## 4、volatile：volatile关键字只保证可见性，不保证原子性
原文：https://www.zhihu.com/question/49656589/answer/117826278 <br>

* volatile关键字的两层语义
 * 一旦一个共享变量（类的成员变量、类的静态成员变量）被volatile修饰之后，那么就具备了两层语义： 
  * 1）保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。 
  * 2）禁止进行指令重排序。
  
```
volatile 只能保证 “可见性”，不能保证 “原子性”。count++; 这条语句由3条指令组成： 
（1）将 count 的值从内存加载到 cpu 的某个寄存器r 
（2）将 寄存器r 的值 +1，结果存放在 寄存器s 
（3）将 寄存器s 中的值写回内存 
  所以，如果有多个线程同时在执行 count++;，在某个线程执行完第（3）步之前，
其它线程是看不到它的执行结果的。在没有 volatile 的时候，执行完 count++;，
执行结果其实是写到CPU缓存中，没有马上写回到内存中，后续在某些情况下（比如
CPU缓存不够用）再将CPU缓存中的值flush到内存。正因为没有马上写到内存，所以
不能保证其它线程可以及时见到执行的结果。 
  在有 volatile 的时候，执行完 count++;，执行结果写到CPU缓存中，并且同时
写回到内存，因为已经写回内存了，所以可以保证其它线程马上看到执行的结果。
但是，volatile 并没有保证原子性，在某个线程执行（1）（2）（3）的时候，
volatile 并没有锁定 count 的值，也就是并不能阻塞其他线程也执行（1）（2）（3）。
可能有两个线程同时执行（1），所以（2）计算出来一样的结果，然后（3）存回的也是同一个值。 
```

## 5、硬盘、内存、CPU缓存：速度由慢到快
* 硬盘：外设
* 内存：在计算机内部（在主板上）的一些存储器
* CPU缓存：在CPU上的 
* 速度由慢到快

