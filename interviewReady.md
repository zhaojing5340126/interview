<!-- TOC -->

- [一、Java集合](#一java集合)
    - [1、Arraylist 、 LinkedList 、Vector 异同？](#1arraylist-与-linkedlist-与-Vector-异同)
    - [3、JDK1.7 和1.8 HashMap的底层实现](#3jdk17-和18-hashmap的底层实现)
    - [4、JDK1.8 HashMap 和 Hashtable 的区别？](#4jdk18-hashmap-和-hashtable-的区别)
    - [5、HashMap 的长度为什么是2的幂次方？](#5hashmap-的长度为什么是2的幂次方)
    - [5、HashMap 多线程操作导致死循环问题【未，周三】](#5hashmap-多线程操作导致死循环问题未周三)
    - [6、HashSet 和 HashMap 区别？](#6hashset-和-hashmap-区别)
    - [7、JDK1.7、 JDK1.8 ConcurrentHashMap 和 Hashtable 的区别](#7jdk17-jdk18-concurrenthashmap-和-hashtable-的区别)
    - [8、集合框架底层数据结构总结](#8集合框架底层数据结构总结)
        - [1） List](#1-list)
        - [2）Set](#2set)
        - [3）Map](#3map)
- [二、Java并发](#二java并发)
    - [1、简述线程，程序、进程的基本概念。以及他们之间关系是什么？](#1简述线程程序进程的基本概念以及他们之间关系是什么)
- [二、杂说](#二杂说)
    - [1、为什么 Java 中只有值传递？](#1为什么-java-中只有值传递)
        - [【答案】：Java程序设计语言只有按值调用。](#答案java程序设计语言只有按值调用)
    - [2、==与equals(重要)](#2与equals重要)
        - [【答案】：](#答案)
    - [3、你重写过 hashcode 和 equals 么，为什么重写equals时必须重写hashCode方法](#3你重写过-hashcode-和-equals-么为什么重写equals时必须重写hashcode方法)
        - [【答案】：](#答案-1)
    - [4、为什么要有hashCode： hashcode 用于缩小查找成本。](#4为什么要有hashcode-hashcode-用于缩小查找成本)
        - [【答案】：](#答案-2)
    - [5、String和StringBuffer、StringBuilder的区别](#5string和stringbufferstringbuilder的区别)
        - [【答案】：](#答案-3)
            - [1) String 对象不可变，StringBuilder和 StringBuffer 是可变的](#1-string-对象不可变stringbuilder和-stringbuffer-是可变的)
            - [2) String 对象是常量不可变，所以线程安全；StringBuffer 对方法加了同步锁，所以是线程安全的。StringBuilder 并没有对方法进行加同步锁，所以是非线程安全的。](#2-string-对象是常量不可变所以线程安全stringbuffer-对方法加了同步锁所以是线程安全的stringbuilder-并没有对方法进行加同步锁所以是非线程安全的)
    - [6、什么是反射机制？](#6什么是反射机制)
    - [7、反射机制的优缺点：](#7反射机制的优缺点)
    - [8、反射机制的应用场景有哪些？【未，第二周】](#8反射机制的应用场景有哪些未第二周)
    - [9、什么是JDK?什么是JRE？什么是JVM？三者之间的联系与区别？](#9什么是jdk什么是jre什么是jvm三者之间的联系与区别)
    - [10、什么是字节码？采用字节码的最大好处是什么？](#10什么是字节码采用字节码的最大好处是什么)
    - [11、java中的编译器和解释器【Java的编译与解释并存】：](#11java中的编译器和解释器java的编译与解释并存)
    - [12、Java和C++的区别？](#12java和c的区别)
    - [13、接口和抽象类的区别是什么？](#13接口和抽象类的区别是什么)
    - [14、重载和重写的区别？](#14重载和重写的区别)

<!-- /TOC -->
# 一、Java集合
## 1、Arraylist 与 LinkedList 与 Vector 异同？
*  1) 从使用方法的角度分析。ArrayList属于非线程安全，而Vector则属于线程安全。如果是开发中没有线程同步的需求，推荐优先使用ArrayList。因此其内部没有synchronized，执行效率会比Vector快很多。 
*  2) 从数据结构的角度分析。ArrayList是一个数组结构（Vector同理），数组在内存中是一片连续存在的片段，在查找元素的时候数组能够很方便的通过内存计算直接找到对应的元素内存。但是它也有很大的缺点。我们假设需要往数组插入或删除数据的位置为i，数组元素长度为n，则需要搬运数据n-i次才能完成插入、删除操作，导致其效率不如LinkedList。 
* 3) LinkedList的底层是一个双向链表结构，在进行查找操作的时候需要花费非常非常多的时间来遍历整个链表（哪怕只遍历一半），这就是LinkedList在查找效率不如ArrayList快的原因。但是由于其链表结构的特殊性，在插入、删除数据的时候，只需要修改链表节点的前后指针就可以完成操作，其的效率远远高于ArrayList。
![8a29da8662acdc29bc3008a6e92567ca.jpeg](evernotecid://D2D8A4E3-5956-4E24-92CA-9D53D605CCCD/appyinxiangcom/17498476/ENResource/p49)


## 3、JDK1.7 和1.8 HashMap的底层实现
* JDK1.7 HashMap 底层是 数组和链表 结合在一起使用【拉链法】。
    * HashMap 通过 key 的 hashCode 经过扰动函数处理过后得到 hash 值，
    * 然后通过 (n - 1) & hash 判断当前元素存放的位置（这里的 n 指的是数组的长度），
    * 如果当前位置存在元素的话，就判断该元素与要存入的元素的 hash 值以及 key 是否相同，如果相同的话，直接覆盖，不相同就通过拉链法解决冲突。
* 补充：
    * 扰动函数指的就是 HashMap 的 hash 方法。使用 hash 方法也就是扰动函数是为了防止一些实现比较差的 hashCode() 方法，换句话说使用扰动函数之后可以减少碰撞
![](https://camo.githubusercontent.com/eec1c575aa5ff57906dd9c9130ec7a82e212c96a/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f332f32302f313632343064626363333033643837323f773d33343826683d34323726663d706e6726733d3130393931)
* JDK1.8 HashMap 底层是 数组和链表/红黑树结合。 JDK1.8在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间。

## 4、JDK1.8 HashMap 和 Hashtable 的区别？
* 1）HashMap 是非线程安全的，HashTable 是线程安全的【HashTable 内部的方法基本都经过 synchronized 修饰】；
* 2）HashMap 要比 HashTable 效率高一点【因为线程安全的问题，切HashTable 基本被淘汰，不要使用】；
* 3）HashMap 中，null 可以作为键，这样的键只有一个，可以有一个或多个键所对应的值为 null；但是在 HashTable 中 put 进的键值只要有一个 null，直接抛出 NullPointerException。
* 4）HashMap 默认的初始化大小为16，之后每次扩充，容量变为原来的2倍，创建时如果给定容量初始值，HashMap会将其扩充为 2 的幂次方大小，即 HashMap 总是使用2的幂作为哈希表的大小；Hashtable 默认的初始大小为11，之后每次扩充，容量变为原来的2n+1，创建时如果给定了容量初始值，那么 Hashtable 会直接使用你给定的大小；
* 5）HashMap 在解决哈希冲突时，当链表长度大于阈值（默认为8）时，会将链表转化为红黑树，以减少搜索时间。Hashtable 没有这样的机制。
## 5、HashMap 的长度为什么是2的幂次方？
* Hash值的范围太大-2147483648到2147483648，内存放不下这样一个40亿长度的数组，所以这个哈希值要先做对数组的长度取模运算，得到的余数才是存放位置【对应的数组下标】。这个数组下标的计算方法是“ (n - 1) & hash ”。（n代表数组长度）。这也就解释了 HashMap 的长度为什么是2的幂次方。
    * 取余(%)操作中如果除数是2的幂次则等价于与其除数减一的与(&)操作（也就是说如果length 是2的 n 次方，则 hash%length==hash&(length-1)等价）。

## 5、HashMap 多线程操作导致死循环问题【未，周三】
jdk1.8已经解决了死循环的问题

## 6、HashSet 和 HashMap 区别？
HashSet 底层就是基于 HashMap 实现的。HashSet 的绝大部分方法都是通过调用 HashMap 的方法来实现的。当调用HashSet的add方法时，实际上是向HashMap中增加了一行(key-value对)，该行的key就是向HashSet增加的那个对象，该行的value就是一个Object类型的常量

## 7、JDK1.7、 JDK1.8 ConcurrentHashMap 和 Hashtable 的区别
* 1）JDK1.7的 ConcurrentHashMap 底层采用 Segment 数组+ HashEntry 数组【分段的数组+链表】；JDK1.8 采用的数据结构是 数组+链表/红黑二叉树。Hashtable 采用 数组+链表；
* 2）JDK1.7 ConcurrentHashMap（分段锁） 对整个桶数组进行了分割分段(Segment)，每一把锁只锁容器其中一部分数据，多线程访问容器里不同数据段的数据，就不会存在锁竞争，提高并发访问率。（默认分配16个Segment，比Hashtable效率提高16倍。） 
    * Segment 实现了 ReentrantLock,所以 Segment 是一种可重入锁，扮演锁的角色。HashEntry 用于存储键值对数据。
    * 一个 ConcurrentHashMap 里包含一个 Segment 数组。一个 Segment 包含一个 HashEntry 数组，每个 HashEntry 是一个链表结构的元素，每个 Segment 守护着一个HashEntry数组里的元素，当对 HashEntry 数组的数据进行修改时，必须首先获得对应的 Segment的锁。
* 2） JDK1.8 摒弃了Segment的概念，而是直接用 Node 数组+链表/红黑树的数据结构，并发控制使用 synchronized 和 CAS 来操作。 整个看起来就像是优化过且线程安全的 HashMap，虽然在JDK1.8中还能看到 Segment 的数据结构，但是已经简化了属性，只是为了兼容旧版本；
    * synchronized只锁定当前链表或红黑二叉树的首节点，这样只要hash不冲突，就不会产生并发，效率又提升N倍。
* 3） Hashtable(同一把锁) :使用 synchronized 来保证线程安全，效率非常低下。
JDK1.7的ConcurrentHashMap
![](https://img-blog.csdn.net/20170706124519839?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvRGF4MW4=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

JDK1.8的ConcurrentHashMap（TreeBin: 红黑二叉树节点 Node: 链表节点）
![](https://camo.githubusercontent.com/2d779bf515db75b5bf364c4f23c31268330a865e/687474703a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f31382d382d32322f39373733393232302e6a7067)

HashTable: 
![](https://camo.githubusercontent.com/b8e66016373bb109e923205857aeee9689baac9e/687474703a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f31382d382d32322f35303635363638312e6a7067)

## 8、集合框架底层数据结构总结
### 1） List
* Arraylist： Object数组
* Vector： Object数组
* LinkedList： 双向循环链表
### 2）Set
* HashSet（无序，唯一）: 基于 HashMap 实现的，底层采用 HashMap 来保存元素
* LinkedHashSet： LinkedHashSet 继承与 HashSet，并且其内部是通过 LinkedHashMap 来实现的。
* TreeSet（有序，唯一）： 红黑树(自平衡的排序二叉树。)
### 3）Map
* HashMap： JDK1.7 HashMap由数组+链表组成的。JDK1.8 在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间
* LinkedHashMap: LinkedHashMap 继承自 HashMap，所以它的底层仍然是基于拉链式散列结构即由数组和链表或红黑树组成。另外，LinkedHashMap 在上面结构的基础上，增加了一条双向链表，使得上面的结构可以保持键值对的插入顺序。同时通过对链表进行相应的操作，实现了访问顺序相关逻辑。
* HashTable: 数组+链表组成的，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的
* TreeMap: 红黑树（自平衡的排序二叉树）

# 二、Java并发
## 1、简述线程，程序、进程的基本概念。以及他们之间关系是什么？
* 1）程序：即静态的代码。
* 2）进程：程序的一次执行过程。进程是资源分配的基本单位，进程中包含线程，线程共享进程的资源。【当程序在执行时，将会被操作系统载入内存中。】
* 3) 线程： 一个线程就是一个顺序执行流，多个就是多线程。线程是操作系统调度的最小单位。一个进程里可以创建多个线程，这些线程都拥有各自的计数器、堆栈和局部变量等属性，并且能够访问共享的内存变量。
* 4）线程上下文的切换比进程上下文切换要快很多【上下文切换就是从当前执行任务切换到另一个任务执行的过程】
    * 进程切换时，涉及到当前进程的CPU环境的保存和新被调度运行进程的CPU环境的设置。
    * 线程切换仅需要保存和设置少量的寄存器内容，不涉及存储管理方面的操作。

## 2、线程有哪些基本状态？这些状态是如何定义的?
* 1）新建(new)：初始状态。此时它和其他java对象一样，仅被分配了内存。
* 2）可运行(runnable)：可运行状态。线程对象创建后，其他线程(比如main线程）调用了该对象的start()方法。该状态的线程位于可运行线程池中，等待被线程调度选中，获 取cpu的使用权。
* 3）运行(running)：运行状态。可运行状态(runnable)的线程获得了cpu时间片（timeslice），执行程序代码。
* 4）阻塞(block)：阻塞状态是指线程因为某种原因放弃了cpu使用权，暂时停止运行。直到线程进入可运行(runnable)状态，才有 机会再次获得cpu时间片转到运行状态。阻塞的情况分三种：
    * (一). 等待阻塞：运行(running)的线程执行o.wait()方法，JVM会把该线程放入等待队列(waiting queue)中。
    * (二). 同步阻塞：运行(running)的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则JVM会把该线程放入锁池(lock pool)中。
    * (三). 其他阻塞: 运行(running)的线程执行Thread.sleep(long ms)或t.join()方法，或者发出了I/O请求时，JVM会把该线程置为阻塞状态。当sleep()状态超时join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入可运行(runnable)状态。
* 5) 死亡(dead)：线程run()、main()方法执行结束，或者因异常退出了run()方法，则该线程结束生命周期。死亡的线程不可再次复生
![](https://camo.githubusercontent.com/5b764ff5af6204f82c7ae6237b20c41f9505aef8/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f382f392f313635316631396437633465393361333f773d38373626683d34393226663d706e6726733d313238303932)


## 3、 何为多线程？
* 一个线程就是一个顺序执行流，多个就是多线程。多线程就是多个线程同时运行或交替运行。单核CPU的话是顺序执行，也就是交替运行。多核CPU的话，因为每个CPU有自己的运算器，所以在多个CPU中可以同时运行。

## 4、 为什么多线程是必要的？【未完】
* 1）能更好的利用处理器上的多个核心。程序使用多线程技术，将计算逻辑分配到多个处理器核心上，就能显著减少程序运行时间，充分利用处理器。
* 2）能得到更快的响应时间。使用多线程，将数据一致性不强的操作派发给其他线程处理（也可以使用消息队列）。让相应用户请求的线程能够尽可能快地处理完成，缩短响应时间，提升用户体验。

* 5、使用多线程常见的三种方式？继承Thread类、实现Runnable接口、使用线程池
```java
//MyThread继承了Thread类
MyThread mythread = new MyThread();
mythread.start();

//MyRunnable实现了Runnable接口
Runnable runnable=new MyRunnable();
Thread thread=new Thread(runnable);
thread.start();

//另外《阿里巴巴Java开发手册》中强制线程池不允许使用 Executors 去创建，
//而是通过 ThreadPoolExecutor 的方式，这样的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险
```

* 线程是一个子任务，CPU以不确定的方式，或者说是以随机的时间来调用线程中的run方法。

## 5、线程的优先级？
* 1）每个线程都具有各自的优先级，线程的优先级可以在程序中表明该线程的重要性，如果有很多线程处于就绪状态，系统会根据优先级来决定首先使哪个线程进入运行状态。
    * 【低优先级的线程运行几率低】
* 2) 线程优先级具有继承特性。
    * 比如A线程启动B线程，则B线程的优先级和A是一样的。
* 3)  线程优先级具有随机性。 
    * 也就是说线程优先级高的不一定每一次都先执行完。

* 补充：
    * Thread.MIN_PRIORITY（常数1），Thread.NORM_PRIORITY（常数5）, Thread.MAX_PRIORITY（常数10）。在默认情况下优先级都是Thread.NORM_PRIORITY（常数5）

## 6、Java多线程分类？
* 1） 用户线程：运行在前台，执行具体的任务；
    * 如程序的主线程、连接网络的子线程等都是用户线程
* 2） 守护线程：运行在后台，为其他前台线程服务.也可以说守护线程是JVM中非守护线程的 “佣人”。
    *  一旦所有用户线程都结束运行，守护线程会随JVM一起结束工作
    * 应用： 数据库连接池中的检测线程，JVM虚拟机启动后的检测线程
    * 最常见的守护线程： 垃圾回收线程
    * 可以通过调用 Thead 类的 setDaemon(true) 方法设置当前的线程为守护线程。注意事项如下
        * 1. setDaemon(true)必须在start（）方法前执行，否则会抛出IllegalThreadStateException异常
        * 2. 在守护线程中产生的新线程也是守护线程
        * 3. 不是所有的任务都可以分配给守护线程来执行，比如读写操作或者计算逻辑

## 7、sleep()方法和wait()方法简单对比？
* 1）两者都可以暂停线程的执行。Wait通常被用于线程间交互/通信，sleep通常被用于暂停执行；
* 2）sleep方法没有释放锁，而wait方法释放了锁 ；
* 3）sleep()方法执行完成后，线程会自动苏醒；wait()方法被调用后，线程不会自动苏醒，需要别的线程调用同一个对象上的notify()或者notifyAll()方法。。

## 8、为什么我们调用start()方法时会执行run()方法，为什么我们不能直接调用run()方法？
* 调用start方法方可启动线程并使线程进入可运行状态，当分配到时间片后就自动执行run()方法的内容，这是多线程工作；而直接调用run方法只是相当于thread的一个普通方法调用，还是在主线程里执行。





# 三、杂说
## 1、为什么 Java 中只有值传递？
* 术语解释：
  * 按值调用：表示方法接收的是调用者提供的值。即方法的得到的是变量的一份拷贝，而非原本的变量。
  * 按引用调用：表示方法接收的是调用者提供的变量地址。方法的得到的是原本的变量的引用。
### 【答案】：Java程序设计语言只有按值调用。
  * 1）方法得到的是所有参数值的一个拷贝，方法不能修改传递给它的任何参数变量的内容。
  * 2）对对象采用的不是引用调用，实际上，对象引用是按值传递的。因为一个方法不能让对象参数引用一个新的对象。
## 2、==与equals(重要)
### 【答案】：
* == : 它的作用是判断两个对象的地址是不是相等。即，判断两个对象是不是同一个对象。(基本数据类型==比较的是值，引用数据类型==比较的是内存地址)

* equals() : 它的作用也是判断两个对象是否相等。但它一般有两种使用情况：
    * 情况1：类没有覆盖equals()方法。则通过equals()比较该类的两个对象时，等价于通过“==”比较这两个对象。
    * 情况2：类覆盖了equals()方法。一般，我们都覆盖equals()方法来两个对象的内容相等；若它们的内容相等，则返回true(即，认为这两个对象相等)。
* String中的equals方法是被重写过的，因为object的equals方法是比较的对象的内存地址，而String的equals方法比较的是对象的值。

## 3、你重写过 hashcode 和 equals 么，为什么重写equals时必须重写hashCode方法
### 【答案】：
* 1）就规定来说：如果两个对象相等，则hashcode一定要相同的 ；两个对象相等,对两个对象分别调用equals方法都返回true
    * 1.1）Object 的 hashcode 方法是本地方法，也就是用 c 语言或 c++ 实现的，该方法通常用来将对象的 内存地址 转换为整数【哈希码】之后返回。【基本只有同一个对象才会有相同的哈希码】
        * 【hashCode() 的作用是获取哈希码，这个哈希码的作用是确定该对象在哈希表中的索引位置】
    * 1.2) Object 的 equals()方法。等价于通过“==”比较这两个对象。即判断两个对象的地址是不是相等。和hashcode方法的内涵一样。
* 2） 因此，equals方法被覆盖过，则hashCode方法也必须被覆盖。因为hashCode()的默认行为是对堆上的对象产生独特值。如果没有重写hashCode()，则该class的两个对象无论如何都不会相等【因为两个对象的hashcode()返回的值肯定不一样，那么根据规定，如果两个对象相等，则hashcode一定要相同的，所以这两个对象就不能相等，即使这两个对象我们重写的equal认为他们相等】

* 附注：hashCode（）与equals（）的相关规定规定：
    * 如果两个对象相等，则hashcode一定也是相同的 **
    * 两个对象相等,对两个对象分别调用equals方法都返回true
    * 两个对象有相同的hashcode值，它们也不一定是相等的
    * 因此，equals方法被覆盖过，则hashCode方法也必须被覆盖

## 4、为什么要有hashCode： hashcode 用于缩小查找成本。
### 【答案】：
* 我们以“HashSet如何检查重复”为例子来说明为什么要有hashCode：
    * 1）当你把对象加入HashSet时，HashSet会先计算对象的hashcode值来判断对象加入的位置，同时也会与其他已经加入的对象的hashcode值作比较，如果没有相符的hashcode，HashSet会假设对象没有重复出现。
    * 2）但是如果发现有相同hashcode值的对象，这时会调用equals（）方法来检查hashcode相等的对象是否真的相同。如果两者相同，HashSet就不会让其加入操作成功。如果不同的话，就会重新散列到其他位置。
    * 3）这样我们就大大减少了equals的次数，相应就大大提高了执行速度。


## 5、String和StringBuffer、StringBuilder的区别
### 【答案】：
#### 1) String 对象不可变，StringBuilder和 StringBuffer 是可变的
* String 类中使用final 修饰的字符数组保存字符串 private　final　char　value[]，所以 String 对象是不可变的。而StringBuilder 与 StringBuffer 都继承自 AbstractStringBuilder 类，在 AbstractStringBuilder 中的字符数组 char[] value 没有用 final 关键字修饰，所以这两种对象都是可变的。
AbstractStringBuilder.java
```java
abstract class AbstractStringBuilder implements Appendable, CharSequence {
    char[] value;
    int count;
    AbstractStringBuilder() {
    }
    AbstractStringBuilder(int capacity) {
        value = new char[capacity];
    }
```
#### 2) String 对象是常量不可变，所以线程安全；StringBuffer 对方法加了同步锁，所以是线程安全的。StringBuilder 并没有对方法进行加同步锁，所以是非线程安全的。

## 6、什么是反射机制？
* 反射：Java反射机制是在运行状态中，
    * 对于任意一个类，都能够知道这个类的所有属性和方法； 
    * 对于任意一个对象，都能够调用他的任意一个方法和属性；
    * 这种动态获取信息以及动态调用对象方法的功能叫Java语言的反射机制
* 反射机制提供了哪些功能？
    * 在运行时判定任意一个对象所属的类
    * 在运行时构造任意一个类的对象；
    * 在运行时判定任意一个类所具有的成员变量和方法；
    * 在运行时调用任意一个对象的方法；
    * 生成动态代理；
## 7、反射机制的优缺点：
* 优点：运行期类型的判断，动态加载类，提高代码的灵活度
* 缺点：性能问题：反射相当于一系列解释操作，通知 JVM 要做的事情，性能比直接的java代码要慢很多。
* 补充说明：
    * 静态加载类：编译时刻加载的类，例如用new关键字创建的对象要通过编译器的静态检查，如果编译时该类不存在，那么使用该对象的类也无法通过编译。
    * 动态加载类：运行时刻加载的类，在编译的时候不会进行判断，只有在运行时才会进行判断，假设该类不存在，在运行时才会报错。
    * 反射使用的前提条件：必须先得到代表的字节码的Class，Class类用于表示.class文件（字节码）
    * 在运行期间，一个类，只有一个Class对象产生。
```java

class Office{
	public static void main(String[] args){
		try{
            //forName()方法时动态加载类，即便编译时args[0]类名的类不存在，编译也是可以通过的，只是在运行时刻会抛出异常，返回的是Class类型
			Class c = Class.forName(args[0]);
			OfficeAble oa = (OfficeAble)c.newInstance();
			oa.start();
		}
		catch(Exception e){
			e.printStackTrace();
		}
		
	}

```


## 8、反射机制的应用场景有哪些？【未，第二周】

## 9、什么是JDK?什么是JRE？什么是JVM？三者之间的联系与区别？
* JDK: java开发工具箱。包括JRE（Java Runtime Environment），和其他供开发者使用的工具包。

* JRE: JRE（Java Runtime Environment）Java运行环境，用于运行Java程序。

* JVM： Java虚拟机，提供了内存管理/垃圾回收和安全机制等。负责将字节码转换为特定机器代码，这种独立于硬件和操作系统，正是java程序可以一次编写多处执行的原因

## 10、什么是字节码？采用字节码的最大好处是什么？
* Java源程序经过编译器编译后变成字节码，字节码由虚拟机解释执行
* 字节码的好处：Java语言通过字节码的方式，在一定程度上解决了传统解释型语言执行效率低的问题，同时又保留了解释型语言可移植的特点。所以Java程序运行时比较高效，而且，由于字节码并不专对一种特定的机器，因此，Java程序无须重新编译便可在多种不同的计算机上运行。
## 11、java中的编译器、解释器和即时编译（JIT）【Java的编译与解释并存】：
* Java源程序经过编译器编译后变成字节码，字节码由虚拟机解释执行，
* 虚拟机将每一条要执行的字节码送给解释器，解释器将其翻译成特定机器上的机器码，然后在特定的机器上运行。
* JVM中还存在着即时编译器优化代码执行，他会检测代码中的热点代码（即多次调用的方法或循环的代码块），这些代码如果每次都通过解释器解释执行无疑大大降低了运行效率，因此Jit编译器将他们编译成本地代码，则下次调用时就不需要解释器再次解释执行。


## 12、Java和C++的区别？
* 都是面向对象的语言，都支持封装、继承和多态
* Java不提供指针来直接访问内存，程序内存更加安全
* Java的类是单继承的，C++支持多重继承；虽然Java的类不可以多继承，但是接口可以多继承。
* Java有自动内存管理和垃圾回收机制

## 13、接口和抽象类的区别是什么？
* 接口的方法默认是public和抽象的，而抽象类可以有非抽象的方法
* 接口中的实例变量默认是final类型的，而抽象类中则不一定
* 一个类可以实现多个接口，但最多只能继承一个抽象类
* 一个类实现接口的话要实现接口的所有方法，而抽象类不一定
* 从设计层面来说，抽象类是对类的抽象，是一种模板设计，接口是行为的抽象，是一种行为的规范。

## 14、重载和重写的区别？
* 重载： 发生在同一个类中，方法名必须相同，其余都可以不同。 
* 重写： 发生在父子类中，方法名、参数列表必须相同，返回值范围小于等于父类，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类；如果父类方法访问修饰符为private则子类就不能重写该方法。

