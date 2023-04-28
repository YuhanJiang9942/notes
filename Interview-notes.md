 

# **Java** **基础**

 

#### 基本数据类型 & 引用类型

![](C:\Users\jiang\Pictures\typora\java types.png)

![](C:\Users\jiang\Pictures\typora\float&double.png)

计算时指数要加上偏移量，是为了使指数始终为正

```java
Integer.MAX_VALUE = 0x7fffffff;
Integer.MIN_VALUE = 0x80000000;
```



**基本数据类型** **(primitive type)****：**byte, short, int, long, float, double, char, Boolean

​      存储在**栈****(stack)**上

**引用类型****(reference type)**：其他

​      数据存储在**堆****(heap)**上，栈只存地址

**Collection** 只能储存reference type， 因为：

1. Primitive type 不是object
2. Java 用type-eraser实现generic types, which     requires objects

####  常量池

除Float和Double外缓存

![](C:\Users\jiang\Pictures\typora\integer pooling.png)

jvm用hashTable存储字符串常量池

基本类 -> 包装类： autoboxing

Integer a = 127;    =      Integer a = Integer.valueOf(127); (使用缓存池里的对象)

Public static Integer valueOf(int i){

​      Return new Integer(i);

}

IntegerCache: -128 ~ 127， 不创建新对象，可通过JVM启动参数修改

 

**==**: 检查内存地址

左右都是基本类型是，只比较值

基本数据类型没有equals

 

在java中,**堆**主要用来存放对象,**栈**主要用来存放基本数据类型和对象的引用

 

#### 三大特性

**三大特性**： 封装(encapsulation)，继承(extention)，多态(polymorphism)

·    编译时多态主要指方法的重载（overload）

·    运行时多态指程序中定义的对象引用所指向的具体类型在运行期间才确定

§ 继承, 覆盖（重写）,向上转型



#### Access Levels 

![](C:\Users\jiang\Pictures\typora\access-level.png)

 

#### Abstact vs Interface

·    抽象类不能被实例化 （implement），只能被继承(extend)

·    从设计层面上看，抽象类提供了一种 IS-A 关系，需要满足里式替换原则，即子类对象必须能够替换掉所有父类对象。而接口更像是一种 LIKE-A 关系，它只是提供一种方法实现契约，并不要求接口和实现接口的类具有 IS-A 关系。

·    从使用上来看，一个类可以实现多个接口，但是不能继承多个抽象类。

·    接口的字段只能是 static 和 final 类型的，而抽象类的字段没有这种限制。

·    接口的成员只能是 public 的(java 9以前)，而抽象类的成员可以有多种访问权限。



####  SOLID

![Graphical user interface, application, table  Description automatically generated](file:///C:/Users/jiang/AppData/Local/Temp/msohtmlclip1/01/clip_image004.gif)

 

#### Override:

- 子类方法的访问权限必须大于等于父类方法； (public>protected)
- 子类方法的返回类型必须是父类方法返回类型或为其子类型。
- 子类方法抛出的异常类型必须是父类抛出异常类型或为其子类型。

this.func(this)

super.func(this)

this.func(super)

super.func(super)

 

#### **final**：

   1.如果一个类被声明为final，就意味着它不能再派生出新的子类，不能作为父类被继承。因此，一个类不能同时被声明为absrtact抽象类的和final的类。

​    2.如果将变量或者方法声明为final，可以保证它们在使用中不被改变.

​         2.1 被声明为final的变量必须在声明时给定初值，而在以后的引用中只能读取，不可修改。 

​         2.2被声明final的方法只能使用，不能重载。



#### Finalize

Java技术使用finalize()方法在垃圾收集器将对象从内存中清除出去前，做必要的清理工作。这个方法是由垃圾收集器在确定这个对象没有被引用时对这个对象调用的。它是在Object类中定义的，因此所有的类都继承了它。子类覆盖finalize()方法以整理系统资源或者执行其他清理工作。finalize()方法是在垃圾收集器删除对象之前对这个对象调用的。



#### 反射

**反射**优点：可拓展性（使用来自外部的用户自定义类）， 类浏览器和可视化环境， 调试器和测试



#### 拷贝

##### 浅拷贝

两个不同的对象， 即只克隆基本类型的字段， 所引用的对象不克隆，指向同一个

##### 深拷贝

包括引用的对象全部拷贝



#### 创建对象

1. new

2. clone()

   浅拷贝

3. 类派发一个对象（反射）

   ```java
   GirlFriend girlFriend = GirlFriend.class.newInstance();
   ```

4. 动态加载一个对象（反射）

   ```java
   GirlFriend girlFriend = (GirlFriend)Class.forName("cn.javastack.test.jdk.core.GirlFriend").newInstance();
   ```

5. 构造一个对象（反射）

   ```java
   GirlFriend girlFriend = GirlFriend.class.getConstructor().newInstance();
   ```

6. 反序列化

   ```java
   // 序列化一个女朋友
       ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream("gf.obj"));
       objectOutputStream.writeObject(girlFriend1);
       objectOutputStream.close();
   
       // 反序列化出来
       ObjectInputStream objectInputStream = new ObjectInputStream(new FileInputStream("gf.obj"));
       GirlFriend girlFriend2 = (GirlFriend) objectInputStream.readObject();
       objectInputStream.close();
   ```

   

 

#### Collection

**Collection.sort** 优先使用timsort, 若用户指定，可使用mergesort

·    Timsort is the combination of mergesort and quicksort. 在真实案例中，数据往往不是随机分部，所以找到严格递增的片段反转，如片段长度小于minrun, 用快排扩充 O(nlogn)

**PriorityQueue**：堆结构

**Collection:**

![](C:\Users\jiang\Pictures\typora\collections.png)

 

![](C:\Users\jiang\Pictures\typora\map.png)

 

##### modcount (fail-fast)

**Modcount**记录对象的被修改的次数

我们知道 java.util.HashMap 不是线程安全的，因此如果在使用迭代器的过程中有其他线程修改了map，那么将抛出**ConcurrentModificationException**，这就是所谓**fail-fast**策略。这一策略在源码中的实现是通过 modCount 域，modCount 顾名思义就是修改次数，对HashMap 内容的修改都将增加这个值，那么在迭代器初始化过程中会将这个值赋给迭代器的 expectedModCount。在迭代过程中，判断 modCount 跟 expectedModCount 是否相等，如果不相等就表示已经有其他线程修改了 Map。

JDK7以后不再申明modcount为volatile, 因为concurrentModificationException不是保证会被触发的 (**best-effort**)，而volatile既不能保证原子性，且消耗过大

Also for some collections such as CopyOnWriteArrayList, ConcurrentHashMap, the interator works on the copy of the collection, thus it does not throw CME, also called **fail-Save**

##### ArrayList vs LinkedList

![img](file:///C:/Users/jiang/AppData/Local/Temp/msohtmlclip1/01/clip_image010.gif)

·    ArrayList: 应减少扩容， 删除消耗大， 只序列化有数的那部分

·    Vector和array list相似，但使用了synchronized

·    CopyOnWriteArrayList: 读写分离， 写在一个新复制数组里，适用读多写少，不适用实时性和内存敏感. 写操作用reentrant lock. 读写，读读不互斥

·    LinkedList：存储first，last指针，不支持随机访问，但删除快

 

##### LinkedList & ArrayDeque

都可实现双端队列

ArrayDeque:

·    一个环形array（防止假溢出）

·    计算新索引：（tail + 1）& （length - 1）这个计算方式是长度只能是2 的n次方的原因

·    当head == tail时，数组已满，扩容一倍

·    不允许null元素，LinkedList支持

LinkedList

·    双向链表（可以选择从头或从尾遍历）

·    Clear时清除所有节点间的引用关系，帮助GC

ArrayDeque效率较高，因为LinkedList需要创造node节点对象

LinkedList实现了List接口，可以通过索引操作数据

##### HashSet

Is just an instance of Hashmap, store in Key, and value is set to PRESENT

##### TreeSet

·    Based on TreeMap

·    Implements NavigableSet

·    BST

##### String & StringBuilder & StringBuffer

·    String:

底层是一个由final修饰的char[], String对象赋值后会在字符串常量池（String Pool）缓存

String s = new String(“cat”);

首先在字符串常量池寻找“cat”, 没有则创建，然后再堆内存中创建“cat”字符串，并将其地址存入常量池

String s’ = “cat”;

s’ 则是字符串常量池地址，所以s != s’.

·    StringBuilder & StringBuffer

 

都继承了AbstractStringBuilder，底层是没有用final修饰的char[]

 

StringBuffer用Synchronized关键词修饰，线程安全，但是效率低，因为每次操作都要验证有无加锁

 

 

 

##### HashMap

Java object中hashcode 默认返回内存地址，大部分class会override hashcode(), eg. Integer is the value itself, String is sum(s[i] * 31^(n-1)). 选择31的原因：不能时偶数会导致信息损失，素数，2^5 – 1, 位运算高效

https://zhuanlan.zhihu.com/p/340688573

https://zhuanlan.zhihu.com/p/63142005

put(key,val)流程：

1. Hash(key)

Hashcode高16位和低16位异或操作（扰动函数），以尽可能高效地降低哈希碰撞(hash collision)

2. 决定index

取模， hash & (len - 1), len应为2的整数幂，这样（len - 1）可作为低位掩码，截取最低四位的值，即为取模

######  Hash函数

1. 直接寻址法： a * key + b

2. 除留余数法：mod为素数时效果最好

3. 数学分析法：使用多个关键字，不适用于设计通用哈希表

4. 其他： 平方取中法， 折叠法，随机函数（伪随机函数）

###### 解决哈希冲突

两个元素：

1. 填装因子 α = n/m （n is the number of elements）
2. 哈希函数

 

1. 开放定址法  H（key）=（H（key）+ d）MOD m
   * 线性探测
   * 二次探测 d = 12, -12, 22, -22
   * 伪随机数探测

2. 链表法 (拉链法)

3. 再哈希法

![Diagram  Description automatically generated](file:///C:/Users/jiang/AppData/Local/Temp/msohtmlclip1/01/clip_image012.jpg)

###### 使用hash进行分布式存储（路由到某台机器）

使用单纯的取余方法，当增加或减少一台机器是需要重哈希，缓存中数据失效，所有流量打到数据库上



 

 

##### 红黑树 

放弃完全平衡， 只需三次旋转就可达到平衡，实现简单，平均最坏皆为O(logn)， 与AVL相同

##### LinkedHashMap

HashMap + doubly-linked list

·    基于hashMap, 维护一个双向列表

·    主要类属性：头/尾节点引用，accessOrder == true ? 访问顺序迭代 ：插入顺序迭代

·    如果accessOrder == true, 每次访问节点后触发afterNodeAccess方法，把该节点移到尾部

·    可用于实现LRU （继承LinkedHashMap, 并重写removeEldestEntry）

```java
Map m = Collections.synchronizedMap(new LinkedHashMap(...));
```

###### SynchronizedCollection 

Use Synchronized (mutex){//logic} in every method.

Object mutex 可传入，或mutex = this 

##### HashTable

线程安全，用synchronized修饰方法

##### SynchronizedTable

线程安全，使用对象锁，synchronized锁住非静态变量 mutex

和HashTable性能接近



##### ConcurrentHashMap

1.7:

·    使用分段锁，多个segment，二次哈希，第一次选择一个segment，最多可有number of segment的并发数

·    使用reentrant lock, segment extends ReentrantLock. 

·    Put时首先尝试自旋获取锁， 重试次数超过一定量直接lock（）；

·    写加锁，读无锁 （volatile修饰节点，volatile修饰数组指数组地址可见，为了在数组扩容时保证可见）

·    严格来说读取操作不能反映最新的更新，eg.putAll 放入大量数据

·    查询遍历链表效率低

1.8

JDK1.8版本中synchronized+CAS+HashEntry+红黑树。

放弃segment，改为node.

**Node：保存key，value及key的hash值的数据结构。其中value和next都用volatile修饰，保证并发的可见性。**

锁粒度变小，大量使用cas提升了性能

##### WeakReferenceHashMap

·    放弃分段锁，缩小锁的粒度为一个节点，使用CAS + Synchronized

·    空bin直接CAS put，否则Synchronize（头节点）

 

#### Transient

不需要的被序列化的对象（安全考虑，或可由其他对象推导出来的）

"When JVM comes across **transient** keyword, it ignores original value of the variable and save default value of that variable data type."

序列化：Java对象 -> 字节序列

####  版本

##### java8

1. lambda
2. stream
3. default, 不用实现
4. optional

# 多线程

#### 实现

- 实现 Runnable 接口；

![runnable](C:\Users\jiang\Pictures\typora\Picture1.png)

- 实现 Callable 接口；

![](C:\Users\jiang\Pictures\typora\callable.png)

- 继承 Thread 类。

![](C:\Users\jiang\Pictures\typora\Thread.png)

 

#### 中断线程

interrupt() - >interruptedException

​          Interrupted()

​          executorService.shutdownNow();

await(), signal()

 

#### 同步方案

##### **互斥锁**： 

- synchronized (JVM) 

  pessimitic locking

- reentrantLock (java.util.concurrent)

  pessimitic locking, 可以是公平的，可中断，

  可绑定多个condition：Condition condition = lock.newCondition()

  trylock() : non-blocking attempt to acquire a lock (lock polling)

  lockInterruptibly(): and attemp to acquire the lock that can be interrupted

  tryLock(long, timeunit) : and attempt to acquire the lock taht can timeout 

  more flexible to use

ReentrantReadWriteLock：两个锁

"This is typically worthwhile only when the collections are expected to be **large**, **accessed by more reader threads than writer threads**, and entail operations with overhead that outweighs synchronization overhead."



```java
Lock lock = new ReentrantLock();
public void sync(){
    lock.lock();
    // some logic
    lock.unlock();
}
```



##### **非阻塞同步**

：CAS(compare and swap): 当内存地址等于旧的预期值时

​           AtomicInteger

##### **无同步方案**：

栈封闭（局部变量，在栈上），线程本地存储（threadLocal）, 可重入代码（中断返回后原代码不出差）

###### ThreadLocal

类中有1个Map（称：ThreadLocalMap）：用于存储每个线程 & 该线程设置的存储在ThreadLocal变量的值

1、ThreadLocalMap的键Key = 当前ThreadLocal实例、值value = 该线程设置的存储在ThreadLocal变量的值

2、该key是 ThreadLocal对象的弱引用；当要抛弃掉ThreadLocal对象时，垃圾收集器会忽略该 key的引用而清理掉ThreadLocal对象

#### LOCK

more flexibility comes additional responsibility. the locks will not be atomatically removed, thus, must unlock in the finally part. 

This lock is not **monitor lock / intrinsic lock**, it provide more behav

ior and sementics

***condition***: 

```java
    private ReentrantLock lock = new ReentrantLock();
    // 为线程A注册一个Condition
    public Condition conditionA = lock.newCondition();

    public void awaitA() {
        try {
            lock.lock();
            long timeBefore = System.currentTimeMillis();
            conditionA.await();
            long timeAfter = System.currentTimeMillis();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
```

Java condition basically has two method: **await(). signal()**

##### Reentrant Lock

**fairness**

"Programs using fair locks accessed by many threads may display **lower overall throughput** (i.e., are slower; often much slower) than those using the default setting, but have **smaller variances** in times to obtain locks and **guarantee lack of starvation**. "

"Note however, that fairness of locks does not guarantee **fairness of thread scheduling**."

**implementation**

use an inner class Sync

```java
abstract static class Sync extends AbstractQueuedSynchronizer {
    // CAS
    final boolean nonfairTryAcquire(int acquires) {
            final Thread current = Thread.currentThread();
            int c = getState();
            if (c == 0) {
                if (compareAndSetState(0, acquires)) {
                    setExclusiveOwnerThread(current);
                    return true;
                }
            }
            else if (current == getExclusiveOwnerThread()) {
                int nextc = c + acquires;
                if (nextc < 0) // overflow
                    throw new Error("Maximum lock count exceeded");
                setState(nextc);
                return true;
            }
            return false;
        }
}
```

and for fair and unfair:

```java
static final class NonfairSync extends Sync {}
static final class FairSync extends Sync {
    // use a linkedList structure
    // node -> node ->... 
}
```

##### AQS

Abstract queued Synchronizer

是一个接口， 独占模式实现tryAcquire 和 tryRelease, 共享模式实现tryAcquiredShared 和 tryReleasedShared

![](C:\Users\jiang\Pictures\typora\AQS.png)

state:

volatile int

- getState
- setState
- compareAndSetState

methods tryAcquire should be override (exclusive mode)

#### Synchronized

"but forces all lock acquisition and release to occur in a **block-structured** way: when multiple locks are acquired they must be released in the **opposite order**, and all locks must be released in the **same lexical scope** in which they were acquired."

implemented in java with **monitors**. All object owns a monitor. and only one thread can enter the monitor.

no lock -> biased lock -> lightweight lock (self-spin, non-blocking) -> heavyweight lock

#### 线程生命周期

**线程协作**：join()；wait(); notify(); condition.await()

![](C:\Users\jiang\Pictures\typora\thread lifecycle.png)

#### pessimistic & optimistic locking

##### pessimitive

check whether the recorded is modified while commiting, using version number/timestamp/checksums.

if the record is dirty, abort.

Most applicable to high-volumn systems and three-tier architectures where the users do not necessarily maintain conncetion to the database. 

##### optimistic

lock the record when using it. 



#### Synchronized

##### 对象锁

锁住：非静态变量，非静态方法，this对象

##### 类锁

锁住：静态变量，静态方法，XXX.class

#### Volatile

保证可见性和顺序性，加上CAS可保证原子性

1.保证此变量对所有的线程的可见性，这里的“可见性”，如本文开头所述，当一个线程修改了这个变量的值，volatile 保证了新值能立即同步到主内存，以及每次使用前立即从主内存刷新。但普通变量做不到这点，普通变量的值在线程间传递均需要通过主内存（详见：[Java内存模型](https://link.segmentfault.com/?enc=19cXjG%2FwgWCAZ2Ynf1yYMg%3D%3D.rkAvLJa%2BeFzh%2FjT60m%2BkAvfaDqxK7MmCxW%2BvIxb2Gd1zfLNbGHINftQud%2Fc%2F8C6E)）来完成。

2.禁止指令重排序优化。有volatile修饰的变量，赋值后多执行了一个“load addl $0x0, (%esp)”操作，这个操作相当于一个**内存屏障**（指令重排序时不能把后面的指令重排序到内存屏障之前的位置），只有一个CPU访问内存时，并不需要内存屏障；

#### wait() & sleep()

- wait() 是 Object 的方法，而 sleep() 是 Thread 的静态方法；
- wait() 会释放锁，sleep() 不会。



#### 锁优化

自旋锁，锁消除（逃逸分析），锁粗化， 轻量锁（CAS不行再用互斥量），偏向锁

![Diagram  Description automatically generated](file:///C:/Users/jiang/AppData/Local/Temp/msohtmlclip1/01/clip_image026.jpg)

#### 死锁

四个条件：互斥，请求与保持，不可剥夺，环路

#### 线程池

1. If the number of threads is less than the `corePoolSize`, create a new Thread to run a new task.
2. If the number of threads is equal (or greater than) the `corePoolSize`, put the task into the queue.
3. If the queue is full, and the number of threads is less than the `maxPoolSize`, create a new thread to run tasks in.
4. If the queue is full, and the number of threads is greater than or equal to `maxPoolSize`, reject the task. The long and the short of it is that new threads are only created when the queue fills up, so if you're using an unbounded queue then the number of threads will not exceed `corePoolSize`.

This method tries to keep the size of the pool small.

reason to use:

1. easy to manage

2. creating and destroying the threads frequently costs too much.

   eg. allocate a thread stack, system create a real thread correponding to the java thread

 

 

# **计算机系统**

#### CPU Structure

1. arithmetic and logic unit (ALU)

2. control Unit
3. memory unit



registers

- Accumulator

  Most frequently used, store data taken from memory

- Memory Address Register (MAR)

- Memory Data Register (MDR) 

  MAR and MDR together facilitate the communication of CPU and main memory

- General Purpose Register

- Program Counter

- Instruction Register

- Condition code register

  contain different flags, such as "Carry C", "Overflow V"

#### Von Neumann Architecture

![](C:\Users\jiang\Pictures\typora\Von-neumann.png)

##### Bus

- Data bus
- Address bus (seperate to Data address bus and Instruction Address bus in Harverd Architecture)
- Instruction bus

#### CPU 

##### 指令周期（instruction cycle）

1. Fetch (get the instruction from the memory to instruction register )
2. Decode
3. Execution (the control unit passed decoded information as a sequence of control signals to the relevent functional units)
4. Store

##### 存储

1. 寄存器

   速度最快，空间小

2. CPU cache

   使用SRAM (static random-access memory), 不断电数据就不丢失

   - L1

     通常分为“数据缓存” 和 “指令缓存”

     最接近CPU，最快最小

   - L2

     大多数CPU中， l1 & l2 is inside the core of the CPU

   - L3

     Largest and Slowest

   ![](C:\Users\jiang\Pictures\typora\CPU cache.png)

3. 内存

   use DRAM (dynamic Random Access Memory), need constant refresh to keep the data 

4. 硬盘

   SSD(Solid-state disk) & HHD (Hard Disk Drive)

   ![](C:\Users\jiang\Pictures\typora\storage.png)

CPU get a block data from memory at once, which is called **Cache Line** 

How the CPU knows whether a data is stored in the cache?

1. Direct Mapped Cache

   Map the address of memory block to cache line, using mod. CPU line also stotr a tag to identify which memory block it is storing now. Thus:

   CPU line contains

   1.  data

   2. valid bit

      To identify wether the data stored is valid

   3. tag

   CPU use a **offset** to read a **word** from a cache line, thus an address contains **tag, index and offset**

   ![](C:\Users\jiang\Pictures\typora\access cache.webp)

2. Fully Associative cache
3. Set Associative Cache

How to increase the chances of a cache hit?

1. For data cache, try to access the data in order

2. For instruction cache, make the if/else result more predictable, eg. make the array in order

   In c/c++, we can use macro to say if a condition is likely or unlikely to happen

For multi-core CPU, a thread may be run by different core, which will decrease the chance of a cache hit. 

##### Cache Coherence

1. Write Through

   ![](C:\Users\jiang\Pictures\typora\write through.webp)

   data will be write to memory every time, time-costing

1. Write Back

   ![](C:\Users\jiang\Pictures\typora\write back.webp)

   回写式（write back）即 CPU 只向 Cache 写入，并用标记加以注明，直到 Cache 中被写过的块要被进入的信息块取代时，才一次写入主存。这种方式考虑到写入的往往是中间结果，每次写入主存速度慢而且不必要。其特点是速度快，避免了不必要的冗余写操作，但结构上较复杂。

   Deal with cache coherency between cores

   1. write propagation

   2. Transaction Serialization

      ensuring the order

   Thus:

   **Bus Snooping**

   CPU monitor the bus

   And to ensuring Transaction Serialization, based on Bus Snooping, we have 

   **MESI protocol**

   four tags:
   
   - Modified
   - Exclusive
   - shared
   - Invalidated
   
   ![](C:\Users\jiang\Pictures\typora\MESI.webp)

##### **CPU调度算法**

First come first serve

Shortest job first

Highest response ratio next （响应时间/执行时间）

Round robin

Highest priority first

Multilevel feedback queue



##### False Sharing

eg. A and B in a same cache line, shared by two cores. Core1 modifies A. The state in core1 is Modified, in core2 is invalidated. Then core2 tries to change B, which needs core1 to write back to memory, core1 state change to invalidated, core2 state change to Modifies. In this case, the cache does not do its job. 

To avoid false sharing:

- use Macro __cacheline_aligned_in_smp, which will place two variable in different lines.
- use final variables to fill the position

This is actually space for time.

##### Task

In linux, both process and thread are represented as **task_struct**, thus thread is also called light-weighted process

In linux, tasks can be roughly divided as:

1.  real-time task (priority 0~99)
2. conventional tasks (priority 100~139)

###### Scheduler

![](C:\Users\jiang\Pictures\typora\linux scheduling.webp)

Deadline and Realtime scheduling classed are for real-time tasks.

###### CFS (Completely Fair Scheduling)

virtual runtime of a task is its actual runtime normalized to the total number of running tasks

vruntime += 实际运行时间（time process run) * 1024 / 进程权重(load weight of this process)

权重： 根据任务的nice值进行索引。nice值可以理解为是我们事先为任务分配的优先级

![](C:\Users\jiang\Pictures\typora\CFS.jpeg)

###### Running Queue

![](C:\Users\jiang\Pictures\typora\running queue.webp)

priority: deadline > realtime > cfs

###### Modify priority

1. Nice: Modify priority of conventional tasks
2. Change scheduling strategy

##### Soft Interrupts

中断应快且短，为了解决这个问题，Linux 将中断分为上下两部分

1. 上半部分快速处理中断，暂时关闭中断亲切，处理硬件相关或time sensitive事件
2. 下半部分处理其余工作，以内核线程的方式运行

软中断也发生于内核调度，RCU锁等

#### Kernel

1. Manage processes and threads
2. Manage memory
3. Manage hardware devices
4. offer system calls

![](C:\Users\jiang\Pictures\typora\systemcall.webp)

##### Types

1. Monolithic Kernel

   Linux

2. Micro Kernel

   Dynamically load needed components

   Each component is isolated, will not affect each other, thus more stable and reliable. 

   However, there will be more swapping bettween kernel mode and user mode. 

3. Hybrid Kernel

   Windows. like a Monolithic kernel wrap a micro kernel

#### 上下文

进程上下文：CPU的所有寄存器中的值、进程的状态以及堆栈上的内容

用户级上下文: 正文、数据、用户堆栈以及共享存储区；

寄存器上下文: 通用寄存器、程序寄存器(IP)、处理器状态寄存器(EFLAGS)、栈指针(ESP)；

系统级上下文: 进程控制块task_struct、内存管理信息(mm_struct、vm_area_struct、pgd、pte)、内核栈。

 



#### **进程** && **线程** && **协程**

线程是程序执行的最小单位，而进程是操作系统分配资源的最小单位；

进程的切换涉及页表的切换

进程之间数据很难共享

协程：**是单线程下的并发，又称微线程，纤程。英文名Coroutine。一句话说明什么是线程：\**协程是一种用户态的轻量级线程，即协程是由用户程序自己控制调度的。***

 

**页表**记录了物理内存和虚拟内存的映射关系, 每个进程都有自己的页表. 页表查找速度很慢, 所以用TLB (Translation Lookaside Buffer) 缓存常用的地址映射. 进程切换导致TLB失效, 查找速度变慢, 程序也变慢

#### DMA (零拷贝)

Before DMA：

![](C:\Users\jiang\Pictures\typora\IO before dma.webp)

Direct memery access:

![](C:\Users\jiang\Pictures\typora\DMA IO.webp)

CPU不直接参与数据搬运

##### 文件传输

###### 传统文件传输 (PIO)

![](C:\Users\jiang\Pictures\typora\传统文件传输.webp)

两次系统调用，read(), write()

四次用户态和内核态的上下文切换

四次数据拷贝

要想提高文件传输的性能，就需要减少「**用户态与内核态的上下文切换**」和「**内存拷贝**」的次数

###### 优化

- 减少「**用户态与内核态的上下文切换**」

  减少系统调用的次数

- 和「**内存拷贝**」

  数据不用搬运到用户空间，用户缓冲区没有必要存在

##### 实现零拷贝

###### mmap + write()

![](C:\Users\jiang\Pictures\typora\mmap + write 零拷贝.webp)

减少一次数据拷贝， 系统调用没有减少

###### SendFile

```c
#include <sys/socket.h>
ssize_t sendfile(int out_fd, int in_fd, off_t *offset, size_t count);
```

file descriptor: uniquely identifies an open file in a computer's operating system.

![](C:\Users\jiang\Pictures\typora\senfile-3次拷贝.webp)

三次拷贝，两次切换

###### SG-DMA （Scatter-Gather DMA）

![](C:\Users\jiang\Pictures\typora\sgdma.webp)

实现了零拷贝（zero-copy）， 没有在内存层面拷贝数据， CPU没有参与搬运数据。 

两次切换和搬运

##### PageCache

内核缓冲区即为pageCache（磁盘高速缓存）

- 缓存最近被访问的数据

  程序运行时的**局部性**

- 预读

  减少磁头旋转到数据所在扇区的耗时的影响，预读一部分数据

GB级别大文件不应该使用零拷贝技术，因为会使PageCache被大文件占据，导致热点小文件无法利用到PageCache. 

解决方法：异步I/O+直接I/O

![](C:\Users\jiang\Pictures\typora\异步 IO 的过程.webp)

- 直接I/O

  不使用PageCache，对磁盘来说异步I/O只支持直接I/O

  应用场景

  1. 应用程序已经实现了磁盘数据的缓存
  2. 传输大文件

- 缓存I/O

  使用PageCache

  优点：

  1. 缓存尽可能多的I/O请求在PageCache，合并成一个大的请求发给磁盘，减少磁盘寻址
  2. 内核预读I/O请求放在PageCache中， 减少对磁盘的操作

#### 

##### 阻塞I/O

![clipboard.png](https://segmentfault.com/img/bVm1c3)

##### 非阻塞I/O

 ![clipboard.png](https://segmentfault.com/img/bVm1c4)

##### I/O 多路复用

I/O multiplexing

1. select

   ```c++
   int select(int nfds, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout);
   ```

   block until the given file descriptors are ready to perform I/O, or until an optionally specified timeout has elapsed

   需无差别轮询所有流，用定长数组，数组中的每一位代表一个文件句柄（file handle）的掩码，上限1024

   select 实现多路复用的方式是，将已连接的 Socket 都放到一个**文件描述符集合**，然后调用 select 函数将文件描述符集合**拷贝**到内核里，让内核来检查是否有网络事件产生，检查的方式很粗暴，就是通过**遍历**文件描述符集合的方式，当检查到有事件产生后，将此 Socket 标记为可读或可写， 接着再把整个文件描述符集合**拷贝**回用户态里，然后用户态还需要再通过**遍历**的方法找到可读或可写的 Socket，然后再对其处理。

2. poll

   ```c++
   int poll (struct pollfd *fds, unsigned int nfds, int timeout);
   ```

   和select相似，但用了用户自定义数组长度，动态数组，链表形式，在接口上无限制

   select由于使用的是位运算，所以select需要分别设置read/write/error fds的掩码。而poll是通过设置数据结构中fd和event参数来实现read/write，比如读为POLLIN，写为POLLOUT，出错为POLLERR

   缺点：

   （1）每次调用select，都需要把fd集合从用户态拷贝到内核态，这个开销在fd很多时会很大

   （2）同时每次调用select都需要在内核遍历传递进来的所有fd，这个开销在fd很多时也很大

   （3）select支持的文件描述符数量太小了，默认是1024

3. epoll

   提供三个函数：epoll_create,epoll_ctl和epoll_wait，epoll_create是创建一个epoll句柄；epoll_ctl是注册要监听的事件类型；epoll_wait则是等待事件的产生。

   ```c++
   int epoll_create(int size)；//创建一个epoll的句柄，size用来告诉内核这个监听的数目一共有多大
   int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event)；
   int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);
   ```

   对于第一个缺点，epoll的解决方案在epoll_ctl函数中。每次注册新的事件到epoll句柄中时（在epoll_ctl中指定EPOLL_CTL_ADD），会把所有的fd拷贝进内核，而不是在epoll_wait的时候重复拷贝。epoll保证了每个fd在整个过程中只会拷贝一次。**保存在内核里的红黑树**。无重复拷贝，O（logn）.

   　　对于第二个缺点，epoll的解决方案不像select或poll一样每次都把current轮流加入fd对应的设备等待队列中，而只在epoll_ctl时把current挂一遍（这一遍必不可少）并为每个fd指定一个**回调函数**，当设备就绪，唤醒等待队列上的等待者时，就会调用这个回调函数，而这个回调函数会把就绪的fd加入一个**就绪链表**）。epoll_wait的工作实际上就是在这个就绪链表中查看有没有就绪的fd（利用schedule_timeout()实现睡一会，判断一会的效果，和select实现中的第7步是类似的）。

   　　对于第三个缺点，epoll没有这个限制，它所支持的FD上限是最大可以打开文件的数目，这个数字一般远大于2048,举个例子,在1GB内存的机器上大约是10万左右，具体数目可以cat /proc/sys/fs/file-max察看,一般来说这个数目和系统内存关系很大。
   
   ![](C:\Users\jiang\Pictures\typora\epoll.webp)



epoll 支持两种事件触发模式，分别是**边缘触发（edge-triggered，ET）和水平触发（level-triggered，LT）**。

- 使用边缘触发模式时，当被监控的 Socket 描述符上有可读事件发生时，**服务器端只会从 epoll_wait 中苏醒一次**，即使进程没有调用 read 函数从内核读取数据，也依然只苏醒一次，因此我们程序要保证一次性将内核缓冲区的数据读取完；应搭配non-blocking I/O
- 使用水平触发模式时，当被监控的 Socket 上有可读事件发生时，**服务器端不断地从 epoll_wait 中苏醒，直到内核缓冲区数据被 read 函数读完才结束**，目的是告诉我们有数据需要读取

####  内存泄漏

Memory leak， 程序在申请内存后，无法释放，最终导致OOM,

#####  原因

1. 指针重复赋值

   ```c
   char * p = (char *)malloc(10);
   char * np = (char *)malloc(10);
   
   p = np // p所指向的内存位置变为了孤立的内存，无法释放
   ```

2. 错误的内存释放

   如一指针包含某个动态分配的内存位置的指针时， 应先删除指向的

3. 返回值的不正确处理

   ```C
   char *f(){
   	return (char *)malloc(10);
   }
   void f1(){
   	f();
   }
   ```

   调用f()时不对返回值进行处理，导致内存泄漏

4. 内存分配后不进行释放

#### LRU

传统的 LRU 算法法无法避免下面这两个问题：

- 预读失效导致缓存命中率下降；

- 缓存污染导致缓存命中率下降；

  Redis 的缓存淘汰算法则是通过**实现 LFU 算法**来避免「缓存污染」而导致缓存命中率下降的问题（Redis 没有预读机制）。

  MySQL （buffer pool） 和 Linux 操作系统是通过**改进 LRU 算法**来避免「预读失效和缓存污染」而导致缓存命中率下降的问题。

1. 预读失效：预读无用

   预读停留在内存时间尽量短，冷热数据分离

   Linux： 两个链表

   MySQL：young区old区

2. 缓存污染

   大量热点数据被淘汰，导致缓存命中率降低

   提高进入活跃LRU链表的门槛

   Linux：第二次被访问时，从inactive list到active List

   MySQL： 

   - 在内存页被访问**第二次**的时候，并不会马上将该页从 old 区域升级到 young 区域，因为还要进行**停留在 old 区域的时间判断**：

   - - 如果第二次的访问时间与第一次访问的时间**在 1 秒内**（默认值），那么该页就**不会**被从 old 区域升级到 young 区域；
     - 如果第二次的访问时间与第一次访问的时间**超过 1 秒**，那么该页就**会**从 old 区域升级到 young 区域；

#### 进程间通信方式

管道，信号量，信号，消息队列，socket

#### Linux常用指令

1. free

   显示系统内存的使用情况，包括物理内存、交换内存(swap)和内核缓冲区内存。

2. top

   当前正在运行的进程及其使用的系统资源

3. du

   查看磁盘使用情况

4. ps

   ps查看正处于Running的进程，ps aux查看所有的进程。

5. chmod

   改变文件权限

6. grep

7. sed

   Stream EDitor

   Sed是一种非交互式的流编辑器，可动态编辑文件；流编辑器则会在编辑器处理数据之前基于预先提供的一组 规则来编辑数据流 。
          Sed本身是一个管道命令，可以分析 standard input 的，主要是用来分析关键字的使用、统计等，此外还可 以将数据进行替换、删除、选中、选取特定行等功能

8. awk

   awk的功能复杂，对列处理的功能比较强大。可以用于匹配。

#### Security

##### kerbores

![](C:\Users\jiang\Pictures\typora\kerberos.png)

![](C:\Users\jiang\Pictures\typora\Kerberos_protocol.png)

Step A: client encrypt partial message with its password, server find password based on its id and decrypt message. send (ticket)K_TGS

Step B: TGS send a (token)K_Server

 

 

 

# **JVM**



![](C:\Users\jiang\Pictures\typora\类的生命周期.jpg)

- 检查类是否加载过；
- 分配内存；

#### new

**1. 检查类是否加载过：**

在之前 JVM 系列文章中说过，类通过 ClassLoader 生成一个模板，这个模板放在方法区(1.7的实现叫永久代，1.8的实现叫元空间)，这个模板就包含了类的结构信息，包括方法、属性、常量池等。 **new 一个对象的时候，首先会检查是否已经生成了类的模板。如果有，就直接拿来用；如果没有，就先加载类生成类的模板。**

**2. 分配内存：**

经过了第一步之后，就要为对象分配内存，这个过程在堆中进行。分配内存有两种方式，一个叫**指针碰撞**，一个叫**空闲列表**。至于具体用哪种方式，取决于堆内存是否连续。之前的 JVM 垃圾回收文章中说到过，如果采用标记清除算法进行垃圾回收，就会产生内存碎片，如果是用标记整理，就不会有内存碎片。如果没有内存碎片，就用指针碰撞，否则就用空闲列表。

- 指针碰撞：用过的内存放一边，没用过的放另一边，中间有个指针作为分界线，采用该方式为对象分配内存时，只需要将指针向未用过的内存方向移动对象所需内存大小即可。
- 空闲列表：有内存碎片的时候，虚拟机会维护一个列表，列表记录了哪些位置的内存是可用的，给对象分配内存时就会找一块够大的内存去分配，然后更新列表记录。

**3. 初始化零值：**

什么叫初始化零值？你有没有发现，我们在类中定义的成员变量，是不需要赋初始值也可以使用的，而局部变量，没进行初始化去使用就会报错。这是为什么呢？就是因为在对象的创建过程中有“初始化零值”这一步。比如定义了一个 int 类型的成员变量，拿来用的时候，默认值是0，而不是null，这就是初始化零值。

**4. 设置对象头：**

什么是对象头？JVM 在存储对象时，增加的一些标记字段，用于增强对象的功能，这就是对象头。java 对象头包括：

- Mark word：存储对象自身的一些数据，比如 hashCode，gc 分代年龄等；
- Klass pointer：存储指针，JVM 通过这个指针来确定该对象是哪个类的实例；
- array length：如果对象是数组，那么还会存储数组的长度。

**5. 执行init方法：**

经过上面四个步骤，一个新的 java 对象就已经产生了，最后就是执行 init 方法，让对象按照程序猿的意愿，进行初始化。什么叫按照程序猿的意愿初始化？就是你 new 对象的时候传了哪些参数，属性值是什么。

------

内存分配的过程中，如何保证线程安全呢？JVM 采用 TLAB + CAS 的方式保证线程安全。

TLAB 就是为每个线程预先在伊甸园区分配一块内存，JVM 要给对象分配内存的时候，首先会用 TLAB，即预先分配的这块内存，如果不够，就用 CAS 一直重试。



#### 运行时数据区域

![](C:\Users\jiang\Pictures\typora\jvm.png)

##### Java 虚拟机栈

每个 Java 方法在执行的同时会创建一个栈帧用于存储局部变量表、操作数栈、常量池引用等信息。从方法调用直至执行完成的过程，对应着一个栈帧在 Java 虚拟机栈中入栈和出栈的过程。

该区域可能抛出以下异常：

- 当线程请求的栈深度超过最大值，会抛出 StackOverflowError 异常；
- 栈进行动态扩展时如果无法申请到足够内存，会抛出 OutOfMemoryError 异常。

##### 堆

所有对象都在这里分配内存，是垃圾收集的主要区域（"GC 堆"）。

现代的垃圾收集器基本都是采用分代收集算法，其主要的思想是针对不同类型的对象采取不同的垃圾回收算法。可以将堆分成两块：

- 新生代（Young Generation）
- 老年代（Old Generation）

堆不需要连续内存，并且可以动态增加其内存，增加失败会抛出 OutOfMemoryError 异常。

##### 方法区

用于存放已被加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。

和堆一样不需要连续的内存，并且可以动态扩展，动态扩展失败一样会抛出 OutOfMemoryError 异常。

对这块区域进行垃圾回收的主要目标是对常量池的回收和对类的卸载，但是一般比较难实现。

HotSpot 虚拟机把它当成永久代来进行垃圾回收。但很难确定永久代的大小，因为它受到很多因素影响，并且每次 Full GC 之后永久代的大小都会改变，所以经常会抛出 OutOfMemoryError 异常。为了更容易管理方法区，从 JDK 1.8 开始，移除永久代，并把方法区移至元空间，它位于本地内存中，而不是虚拟机内存中。

方法区是一个 JVM 规范，永久代与元空间都是其一种实现方式。在 JDK 1.8 之后，原来永久代的数据被分到了堆和元空间中。**元空间存储类的元信息，静态变量和常量池等放入堆中。**

##### 运行时常量池

运行时常量池是方法区的一部分。

Class 文件中的常量池（编译器生成的字面量和符号引用）会在类加载后被放入这个区域。

除了在编译期生成的常量，还允许动态生成，例如 String 类的 intern()。

##### 直接内存

在 JDK 1.4 中新引入了 NIO 类，它可以使用 Native 函数库直接分配堆外内存，然后通过 Java 堆里的 DirectByteBuffer 对象作为这块内存的引用进行操作。这样能在一些场景中显著提高性能，因为避免了在堆内存和堆外内存来回拷贝数据。



#### **虚拟内存**

https://zhuanlan.zhihu.com/p/171784987

#### 四种引用

1. 强引用，不会被回收

2. 软引用，内存不够时被回收，常用于缓存技术

3. 弱引用，只要开始垃圾回收，就会被回收，weakHashMap(键对被GC)

4. 虚引用，等于没有引用，随时会被回收

 

#### **垃圾回收**

如何确定无效对象：

1. 引用计数法（无法解决环形引用）

2. 可达性分析 （分析过程中需stop the world）

##### GC root

- 虚拟机栈（栈帧中的本地变量表）中引用的对象。
- 方法区中类静态属性引用的对象。static
- 方法区中常量引用的对象。final
- 本地方法栈中JNI（即一般说的Native方法）引用的对象。

##### 垃圾收集算法

1. 标记-清除法 （产生内存碎片）

2. 复制算法 （牺牲一半内存空间， 不适用于old generation因大部分存活）

​      Eden : survivor0 : s1 = 8 : 1 : 1 (hotspot虚拟机)

​      对象首先分配于eden区， 一次新生代回收后，如存活，进入survivor区

​      每次GC年龄加一，最后进入老年区

3. 标记-整理 （向一端移动，清除边界外，为old generation设计）

4. 分代收集

 

##### 垃圾回收器

串行收集器：Serial和Serial Old

只能有一个垃圾回收线程执行，用户线程暂停。 适用于内存比较小的嵌入式设备 。

并行收集器[吞吐量优先]：Parallel Scanvenge和Parallel Old

多条垃圾收集线程并行工作，但此时用户线程仍然处于等待状态。 适用于科学计算、后台处理等若交互场景 。

并发收集器[停顿时间优先]：CMS和G1。

用户线程和垃圾收集线程同时执行(但并不一定是并行的，可能是交替执行的)，垃圾收集线程在执行的时候不会停顿用户线程的运行。 适用于对时间有要求的场景，比如Web应用。

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

# 网络

![](C:\Users\jiang\Pictures\typora\network.jpg)

#### 七层模型

##### 物理层

​      中继器（repeater）

##### 数据链路层（data link）

物理地址寻址、数据的成帧、流量控制、数据的检错、重发  

* 数据链路层为网络层提供可靠的数据传输
* 基本数据单位为帧；
* 主要的协议：以太网协议；
* 两个重要设备名称：网桥和交换机。

##### 网络层

两个主机系统之间的数据透明传送， 路径选择、路由及逻辑寻址

* 网络层负责对子网间的数据包进行路由选择。此外，网络层还可以实现拥塞控制、网际互连等功能；
* 基本数据单位为IP数据报；
* 包含的主要协议：
  * IP协议（Internet Protocol，因特网互联协议）;
  * ICMP协议（Internet Control Message Protocol，因特网控制报文协议）
  * ARP协议（Address Resolution Protocol，地址解析协议）可看成是跨网络层和链路层的协议;
  * RARP协议（Reverse Address Resolution Protocol，逆地址解析协议）。

##### 传输层

为应用进程之间提供端到端的逻辑通信。

网络层只是根据网络地址将源结点发出的数据包传送到目的结点，而传输层则负责将数据可靠地传送到相应的端口。

* 传输层负责将上层数据分段并提供端到端的、可靠的或不可靠的传输以及端到端的差错控制和流量控制问题；
* 包含的主要协议：
  * TCP协议（Transmission Control Protocol，传输控制协议）
  * UDP协议（User Datagram Protocol，用户数据报协议）
* 重要设备：网关。

 

##### 会话层 (session layer)

管理主机之间的会话进程，即负责建立、管理、终止进程之间的会话

 

##### 表示层

**对上层数据或信息进行变换以保证一个主机应用层信息可以被另一个主机的应用程序理解**。表示层的数据转换包括**数据的加密、压缩、格式转换**等。

 

##### 应用层

**为用户的应用程序提供网络服务的接口**。**将用户的操作通过应用程序转换成为服务，并匹配一个相应的服务协议发送给传输层**。

数据传输基本单位为报文

 

#### 各层协议

物理层：RJ45、CLOCK、IEEE802.3   （中继器，集线器，网关）

数据链路：PPP、FR、HDLC、VLAN、MAC  （网桥，交换机）

网络层：IP、ICMP、ARP、RARP、OSPF (open shortest path first)、IPX、RIP、IGRP、 （路由器）

传输层：TCP、UDP、SPX

会话层：NFS、SQL、NETBIOS、RPC

表示层：JPEG、MPEG、ASCII

应用层：FTP、DNS、Telnet、SMTP、HTTP、WWW、NFS



![](C:\Users\jiang\Pictures\typora\internet.jpeg)

#### 常用端口

![Text, letter  Description automatically generated](C:\Users\jiang\Pictures\typora\ports.png)



#### 状态码

![Text, table  Description automatically generated](C:\Users\jiang\Pictures\typora\状态码.png)



#### HTTP request

·   GET: 发送请求，获取服务器数据

·   POST：向 URL 指定的资源提交数据

·   PUT：向服务器提交数据，以修改数据

·   HEAD: 请求页面的首部，获取资源的元信息

·   DELETE：删除服务器上的某些资源。

·   CONNECT：建立连接隧道，用于代理服务器；

·   OPTIONS：列出可对资源实行的请求方法，常用于跨域

·   TRACE：追踪请求 - 响应的传输路径

 

![Chart, bar chart  Description automatically generated](C:\Users\jiang\Pictures\typora\layer.png)

 

 

#### HTTPS

**HTTPS** = HTTP + SSL/TLS （secure socket layer/transport layer secure）

目的：1. Privacy 2. Integrity(完整性) 3. Identification

现在多用TLS

![](C:\Users\jiang\Pictures\typora\TLS.jpg)

![](C:\Users\jiang\Pictures\typora\TLS detail.png)

TLS is consisted of two layers: Handshake layer and TLS record protocol layer.

TLS record protocal layer: implement a secure channel

- Dividing outgoing messages into manageable blocks, and reassembling incoming messages.
- Compressing outgoing blocks and decompressing incoming blocks (optional).
- Applying a [*Message Authentication Code*](https://docs.microsoft.com/en-us/windows/win32/secgloss/m-gly) (MAC) to outgoing messages, and verifying incoming messages using the MAC.
- Encrypting outgoing messages and decrypting incoming messages.

MAC: also known as a tag, is used for authentication a message



![](C:\Users\jiang\Pictures\typora\TLS layer.png)

![](C:\Users\jiang\Pictures\typora\TLS steps.png)

#### **HTTP**

1.0 vs 1.1

1. 1.1 支持persistentConnection

2. 增加host字段，因为虚拟机的存在

3. 增加状态码100 continue，先发request head试探，收到100，再发request body,节约带宽

4. 更多的缓存处理策略

5. 请求头加入range, 可请求资源的一部分，可节约带宽，返回206 partial content

 

1.1 vs 2.0

![](C:\Users\jiang\Pictures\typora\http1.1 vs 2.png)



#### **TCP vs UDP**

##### TCP

 ![](C:\Users\jiang\Pictures\typora\tcp1.jpg)

使用random sequence number 防止segment injection

![](C:\Users\jiang\Pictures\typora\tcp2.jpg)

TCP协议的主要特点

（1）TCP是面向连接的运输层协议；

（2）每一条TCP连接只能有两个端点（即两个套接字），只能是点对点的；

（3）TCP提供可靠的传输服务。传送的数据无差错、不丢失、不重复、按序到达；

（4）TCP提供全双工通信。允许通信双方的应用进程在任何时候都可以发送数据，因为两端都设有发送缓存和接受缓存；

（5）面向字节流。虽然应用程序与TCP交互是一次一个大小不等的数据块，但TCP把这些数据看成一连串无结构的字节流，它不保证接收方收到的数据块和发送方发送的数据块具有对应大小关系，例如，发送方应用程序交给发送方的TCP10个数据块，但就受访的TCP可能只用了4个数据块久保收到的字节流交付给上层的应用程序，但字节流完全一样。

 

###### TCP fast open

![](C:\Users\jiang\Pictures\typora\TCP fast open.png)

减少1RTT (round trip time)



###### **TCP的可靠性原理**

可靠传输有如下两个特点:

a.传输信道无差错,保证传输数据正确;

b.不管发送方以多快的速度发送数据,接收方总是来得及处理收到的数据;

（1）首先，采用三次握手来建立TCP连接，四次握手来释放TCP连接，从而保证建立的传输信道是可靠的。

（2）其次，TCP采用了连续ARQ(Automatic Repeat reQuest)协议（回退N，Go-back-N；超时自动重传）来保证数据传输的正确性，使用滑动窗口协议来保证接方能够及时处理所接收到的数据，进行流量控制。

（3）最后，TCP使用**慢开始**（指数增加）**、拥塞避免、快重传和快恢复**来进行拥塞控制，避免网络拥塞。

![Diagram  Description automatically generated](C:\Users\jiang\Pictures\typora\tcp3.jpg)

######  Limitations

1. 升级工作困难

   TCP是在内核中实现的，而服务器的升级比较保守和缓慢，且需双方同时支持

2. 建立连接的延迟

   在https的情况下，需要三次握手 + TLS四次握手才能开始传输数据

   TCP握手是在内核中完成，TLS握手在应用层完成，无法结合，且TLS无法对TCP头加密，序列号明文传输，存在安全问题。只要序列号处于接收方的滑动窗口内，就会被认为合法。

   针对这个问题，TCP在三次握手时要同步序列号，初始序列号采取随机的方式，以减少被攻击的风险。

3. 队头阻塞 （head-of-line blocking）

   也是导致HTTP/2 head-of-line blocking的原因

4. 网络迁移需要重新建立连接

   TCP连接基于双方IP和端口，网络变化时需重新建立连接，如WiFi to 4G



##### UDP协议特点

（１）UDP是无连接的传输层协议；

（２）UDP使用尽最大努力交付，不保证可靠交付；

（３）UDP是面向报文的，对应用层交下来的报文，不合并，不拆分，保留原报文的边界；

（４）UDP没有拥塞控制，因此即使网络出现拥塞也不会降低发送速率；

（５）UDP支持一对一　一对多　多对多的交互通信；

（６）UDP的首部开销小，只有８字节．

可靠传输

1. 在应用层实现TCP可靠传输的特性（序列号，确认应答，超时重传，流量控制，拥塞控制）

2.  QUIC

   类似TCP的设计，解决了几个TCP的痛点，因为运营商的问题难以推广

##### TCP和UDP的区别

\3.  TCP是可靠传输,UDP是不可靠传输;

\4.  TCP面向连接,UDP无连接;

\5.  TCP传输数据有序,UDP不保证数据的有序性;

\6.  TCP不保存数据边界,UDP保留数据边界;

\7.  TCP传输速度相对UDP较慢;

\8.  TCP有流量控制和拥塞控制,UDP没有;

\9.  TCP是重量级协议,UDP是轻量级协议;

\10. TCP首部较长２０字节,UDP首部较短８字节;

 

#### **ARQ:**

Atomatic repeat request

\1. Stop-and-wait (等待ACK)

\2. Continuous ARQ (滑动窗口，接收方返回的ACK中包含窗口大小)

![A picture containing text  Description automatically generated](C:\Users\jiang\Pictures\typora\arq.jpg)

\3. Go-back-n (接收方丢弃所有第一个没有收到的之后的包，发送方收到NACK, 从指明的包开始重传)

\4. Selective Repeat (连续发送，对每个包设有计时器，)

#### Reactor模式

高性能IO, Netty, Redis, 都在使用

* 网络编程原始思路：while不断监听。问题：无法并发，效率低。请求会被阻塞，吞吐量太低。

* 进阶: connection per thread (tomcat早期版本) 。问题： 资源要求太高，线程反复创建-销毁（线程池可缓解）。线程粒度太大，每一个线程要完成连接，读取和写入，限制了吞吐量。

Reactor将一次操作分为更细的粒度或过程（handler, 每一种handler处理一种event）。全局的管理者selector在channel上检测已注册的时间，调用handler处理

对于多个CPU机器，为充分利用系统资源，将reactor拆为两部分。MainReactor负责监听连接，subReactor accept连接。因为建立连接的过程也要耗费时间和资源 eg.TCP三次握手。

 

#### PING

 发送ICMP请求报文，发回ICMP应答报文

ICMP (Internet Control Message Protocol), 是[TCP/IP协议族](http://baike.baidu.com/view/2221037.htm) 的一个子协议，用于在IP[主机](http://baike.baidu.com/view/23880.htm) 、[路由](http://baike.baidu.com/view/18655.htm) 器之间传递控制消息。控制消息是指[网络通](http://baike.baidu.com/view/8079702.htm) 不通、[主机](http://baike.baidu.com/view/23880.htm) 是否可达、[路由](http://baike.baidu.com/view/18655.htm) 是否可用等网络本身的消息。这些控制消息虽然并不传输用户数据，但是对于用户数据的传递起着重要的作用。

#### DNS

default interative.

Have recursive option, can be used in domain that limit client access to their DNS information for security reasons. But can be attacked. Amplification Attack. Lots of other users amplify as you, send DNS request. You will be flooded with request. Cause performance issue and can be added to blacklist. 

# **设计模式**

**代理**

![Diagram  Description automatically generated with medium confidence](file:///C:/Users/jiang/AppData/Local/Temp/msohtmlclip1/01/clip_image054.jpg)

动态：

JDK: ![preview](file:///C:/Users/jiang/AppData/Local/Temp/msohtmlclip1/01/clip_image056.jpg)

代理类实现invocationHandler，并override invoke方法

Proxy用于关联目标对象和代理对象

 

 

# **数据库**

#### ACID & 隔离级别

原子性（Atomicity）
原子性是指事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生。
一致性（Consistency）
事务前后数据的完整性必须保持一致。
隔离性（Isolation）
事务的隔离性是多个用户并发访问数据库时，数据库为每一个用户开启的事务，不能被其他事务的操作数据所干扰，多个并发事务之间要相互隔离。
持久性（Durability）
持久性是指一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来即使数据库发生故障也不应该对其有任何影响

![](C:\Users\jiang\Pictures\typora\database.png)

一级封锁协议： X锁，解决丢失修改

二级封锁协议： X+S， 读取完立即释放S   脏读

三级：          事务结束释放S  不可重复读



修改丢失

脏读

幻读

不可重复读

**未提交读（READ UNCOMMITTED）**

事务中的修改，即使没有提交，对其它事务也是可见的。

### 提交读（READ COMMITTED）

一个事务只能读取已经提交的事务所做的修改。换句话说，一个事务所做的修改在提交之前对其它事务是不可见的。

### 可重复读（REPEATABLE READ）

保证在同一个事务中多次读取同一数据的结果是一样的。

### 可串行化（SERIALIZABLE）

强制事务串行执行，这样多个事务互不干扰，不会出现并发一致性问题。

该隔离级别需要加锁实现，因为要使用加锁机制保证同一时间只有一个事务执行，也就是保证事务串行执行。

#### Next-key lock

Next-key lock = record lock + gap lock

Next-key lock + MVCC 解决幻读

####  MVCC

多版本并发控制（Multi-Version Concurrency Control, MVCC）是 MySQL 的 InnoDB 存储引擎实现隔离级别的一种具体方式，用于实现提交读和可重复读这两种隔离级别。而未提交读隔离级别总是读取最新的数据行，要求很低，无需使用 MVCC。可串行化隔离级别需要对所有读取的行都加锁，单纯使用 MVCC 无法实现。

### 基本思想

在封锁一节中提到，加锁能解决多个事务同时执行时出现的并发一致性问题。在实际场景中读操作往往多于写操作，因此又引入了读写锁来避免不必要的加锁操作，例如读和读没有互斥关系。读写锁中读和写操作仍然是互斥的，而 MVCC 利用了多版本的思想，写操作更新最新的版本快照，而读操作去读旧版本快照，没有互斥关系，这一点和 CopyOnWrite 类似。

在 MVCC 中事务的修改操作（DELETE、INSERT、UPDATE）会为数据行新增一个版本快照。

脏读和不可重复读最根本的原因是事务读取到其它事务未提交的修改。在事务进行读取操作时，为了解决脏读和不可重复读问题，MVCC 规定只能读取已经提交的快照。当然一个事务可以读取自身未提交的快照，这不算是脏读。

### 版本号

- 系统版本号 SYS_ID：是一个递增的数字，每开始一个新的事务，系统版本号就会自动递增。
- 事务版本号 TRX_ID ：事务开始时的系统版本号。

### Undo 日志

MVCC 的多版本指的是多个版本的快照，快照存储在 Undo 日志中，该日志通过回滚指针 ROLL_PTR 把一个数据行的所有快照连接起来。

例如在 MySQL 创建一个表 t，包含主键 id 和一个字段 x。我们先插入一个数据行，然后对该数据行执行两次更新操作。

```
INSERT INTO t(id, x) VALUES(1, "a");
UPDATE t SET x="b" WHERE id=1;
UPDATE t SET x="c" WHERE id=1;
```

因为没有使用 `START TRANSACTION` 将上面的操作当成一个事务来执行，根据 MySQL 的 AUTOCOMMIT 机制，每个操作都会被当成一个事务来执行，所以上面的操作总共涉及到三个事务。快照中除了记录事务版本号 TRX_ID 和操作之外，还记录了一个 bit 的 DEL 字段，用于标记是否被删除。

[![img](https://camo.githubusercontent.com/648b989516b41055bbd1f39ee4b61a6b0a00e0d0114632dceb1c6b91ec39dca6/68747470733a2f2f63732d6e6f7465732d313235363130393739362e636f732e61702d6775616e677a686f752e6d7971636c6f75642e636f6d2f696d6167652d32303139313230383136343830383231372e706e67)](https://camo.githubusercontent.com/648b989516b41055bbd1f39ee4b61a6b0a00e0d0114632dceb1c6b91ec39dca6/68747470733a2f2f63732d6e6f7465732d313235363130393739362e636f732e61702d6775616e677a686f752e6d7971636c6f75642e636f6d2f696d6167652d32303139313230383136343830383231372e706e67)



INSERT、UPDATE、DELETE 操作会创建一个日志，并将事务版本号 TRX_ID 写入。DELETE 可以看成是一个特殊的 UPDATE，还会额外将 DEL 字段设置为 1。

##### ReadView

MVCC 维护了一个 ReadView 结构，主要包含了当前系统未提交的事务列表 TRX_IDs {TRX_ID_1, TRX_ID_2, ...}，还有该列表的最小值 TRX_ID_MIN 和 TRX_ID_MAX。

[![img](https://camo.githubusercontent.com/1fe81e9af90e3b9256a607920773eab0c685bda03fbb947a2c1a4d26ff55c21e/68747470733a2f2f63732d6e6f7465732d313235363130393739362e636f732e61702d6775616e677a686f752e6d7971636c6f75642e636f6d2f696d6167652d32303139313230383137313434353637342e706e67)](https://camo.githubusercontent.com/1fe81e9af90e3b9256a607920773eab0c685bda03fbb947a2c1a4d26ff55c21e/68747470733a2f2f63732d6e6f7465732d313235363130393739362e636f732e61702d6775616e677a686f752e6d7971636c6f75642e636f6d2f696d6167652d32303139313230383137313434353637342e706e67)



在进行 SELECT 操作时，根据数据行快照的 TRX_ID 与 TRX_ID_MIN 和 TRX_ID_MAX 之间的关系，从而判断数据行快照是否可以使用：

- TRX_ID < TRX_ID_MIN，表示该数据行快照时在当前所有未提交事务之前进行更改的，因此可以使用。
- TRX_ID > TRX_ID_MAX，表示该数据行快照是在事务启动之后被更改的，因此不可使用。
- TRX_ID_MIN <= TRX_ID <= TRX_ID_MAX，需要根据隔离级别再进行判断：
  - 提交读：如果 TRX_ID 在 TRX_IDs 列表中，表示该数据行快照对应的事务还未提交，则该快照不可使用。否则表示已经提交，可以使用。
  - 可重复读：都不可以使用。因为如果可以使用的话，那么其它事务也可以读到这个数据行快照并进行修改，那么当前事务再去读这个数据行得到的值就会发生改变，也就是出现了不可重复读问题。

在数据行快照不可使用的情况下，需要沿着 Undo Log 的回滚指针 ROLL_PTR 找到下一个快照，再进行上面的判断。

#### 设计范式

1NF: 属性不可分

2NF：每个非主属性完全函数依赖于键码

3NF: 每个非主属性不传递依赖于键码

####  

#### B-树

![](C:\Users\jiang\Pictures\typora\B-tree.png)

B-tree is a **balanced search** tree. It is often used when the data is too large that they cannot be kept in the main memory. Most of the tree operation requires **O(h) disk access** where h is the height of the tree. Since B-tree is a **fat tree**, there will be **less disk access**. Generally,  the **B-tree node size** if kept equal to the **disk block size**.

#### B+树

compare to B-tree, non-leaf node does not store data, and leaf node store all information of a record.

非叶节点只存储其子树的最大/最小值。

叶子节点中的关键字**按大小顺序排列**，之间用指针链接

算法的实际运行时间，往往取决于数据在不同存储级别之间的I/O次数。

磁盘读取数据以block为基本单位， 一页里的数据会被同时读出来（磁盘预读）

B+树可以减小I/O次数

MySQL是一种关系型数据库，**区间访问**常见，链指针加强了区间访问性。

1）B+树的磁盘读写代价更低

　　B+树的内部结点并没有指向关键字具体信息的指针。因此其内部结点相对B 树更小。如果把所有同一内部结点的关键字存放在同一盘块中，那么盘块所能容纳的关键字数量也越多。一次性读入内存中的需要查找的关键字也就越多。相对来说IO读写次数也就降低了；

2）B+树查询效率更加稳定

　　由于非终结点并不是最终指向文件内容的结点，而只是叶子结点中关键字的索引。所以任何关键字的查找必须走一条从根结点到叶子结点的路。所有关键字查询的路径长度相同，导致每一个数据的查询效率相当；

3）B+树便于范围查询（最重要的原因，范围查找是数据库的常态）

　　B树在提高了IO性能的同时并没有解决元素遍历的我效率低下的问题，正是为了解决这个问题，B+树应用而生。B+树只需要去遍历叶子节点就可以实现整棵树的遍历。而且在数据库中基于范围的查询是非常频繁的，而B树不支持这样的操作或者说效率太低；



#### 分布式锁

单服务器时可以用本地锁来解决线程安全问题，但本地锁无法在多个服务器之间生效

#####  mysql

1. 利用unique

```jsx
CREATE TABLE `methodLock` (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键',
  `method_name` varchar(64) NOT NULL DEFAULT '' COMMENT '锁定的方法名',
  `desc` varchar(1024) NOT NULL DEFAULT '备注信息',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '保存数据时间，自动生成',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uidx_method_name` (`method_name `) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='锁定中的方法';
```

insert 想要锁住的资源，insert成功则是获得了锁

删除以释放锁

问题：

* 这把锁强依赖数据库的可用性，数据库是一个单点，一旦数据库挂掉，会导致业务系统不可用。

* 这把锁没有失效时间，一旦解锁操作失败，就会导致锁记录一直在数据库中，其他线程无法再获得到锁。

* 这把锁只能是非阻塞的，因为数据的insert操作，一旦插入失败就会直接报错。没有获得锁的线程并不会进入排队队列，要想再次获得锁就要再次触发获得锁操作。

* 这把锁是不可重入的，同一个线程在没有释放锁之前无法再次获得该锁。因为数据中数据已经存在了。

解决方法：

* 两个数据库，数据之前双向同步。一旦挂掉快速切换到备库上。

* 只要做一个定时任务，每隔一定时间把数据库中的超时数据清理一遍

* 搞一个while循环，直到insert成功再返回成功。

* 在数据库表中加个字段，记录当前获得锁的机器的主机信息和线程信息，那么下次再获取锁的时候先查询数据库，如果当前机器的主机信息和线程信息在数据库可以查到的话，直接把锁分配给他就可以了。



2. 使用数据库排他锁

   ```csharp
   result = select * from methodLock where method_name=xxx for update;
   ```

 		for update -> 排他锁

​		 innoDB只有通过索引进行检索的时候才会用行级锁，否则会使用表级锁

```cpp
public void unlock(){
    connection.commit();
}
```

 解锁

问题：mySql会对查询进行优化，不一定会使用索引而是全表扫，加表锁 

##### Redis

SETNX key value



#### 索引

索引存储在**磁盘**

mysql每个表有两个文件，**frm**存储表结构， **ibd**存储数据和索引 （innodb存储引擎， 在mysam中数据和索引分两个文件存储）

##### **聚簇索引和非聚簇索引**

聚簇：

数据和索引一起存储，innodb中数据插入必须和索引放在一起，主键 -> 唯一值 -> rowid，所以一定有聚簇索引

但数据不应重复存储，所以其他索引存储聚簇索引的key值，需要一个回表操作

非聚簇

myisam只有非聚簇

#### sql优化

1. 避免全表扫描，在where 和order by上涉及的列建立索引
2. 避免导致索引失效的操作：
   1. where sth = null
   2. where sth != sth
   3. in/ not in -> between ..  and ..
   4. like "%sth" (通配符在右边会导致失效)
   5. where中使用表达式
   6. 对索引所在列进行操作
3. 索引不要太多， 因为insert和update可能需要重建索引
4. varchar代替char
5. 不要用select *



#### sql执行过程

![](C:\Users\jiang\Pictures\typora\sql.png)

 

 MyIsam and Innobd 区别

1. MySQL默认采用的是MyISAM。
2. MyISAM不支持事务，而InnoDB支持。InnoDB的AUTOCOMMIT默认是打开的，即每条SQL语句会默认被封装成一个事务，自动提交，这样会影响速度，所以最好是把多条SQL语句显示放在begin和commit之间，组成一个事务去提交。
3. InnoDB支持数据行锁定，MyISAM不支持行锁定，只支持锁定整个表。即MyISAM同一个表上的读锁和写锁是互斥的，MyISAM并发读写时如果等待队列中既有读请求又有写请求，默认写请求的优先级高，即使读请求先到，所以MyISAM不适合于有大量查询和修改并存的情况，那样查询进程会长时间阻塞。因为MyISAM是锁表，所以某项读操作比较耗时会使其他写进程饿死。
4. InnoDB支持外键，MyISAM不支持。
5. InnoDB的主键范围更大，最大是MyISAM的2倍。
6. InnoDB不支持全文索引，而MyISAM支持。全文索引是指对char、varchar和text中的每个词（停用词除外）建立倒排序索引。MyISAM的全文索引其实没啥用，因为它不支持中文分词，必须由使用者分词后加入空格再写到数据表里，而且少于4个汉字的词会和停用词一样被忽略掉。
7. MyISAM支持GIS数据，InnoDB不支持。即MyISAM支持以下空间数据对象：Point,Line,Polygon,Surface等。
8. 没有where的count(*)使用MyISAM要比InnoDB快得多。因为MyISAM内置了一个计数器，count(*)时它直接从计数器中读，而InnoDB必须扫描全表。所以在InnoDB上执行count(*)时一般要伴随where，且where中要包含主键以外的索引列。为什么这里特别强调“主键以外”？因为InnoDB中primary index是和raw data存放在一起的，而secondary index则是单独存放，然后有个指针指向primary key。所以只是count(*)的话使用secondary index扫描更快，而primary key则主要在扫描索引同时要返回raw data时的作用较大。

 

 

 

 

 

 

 

 

 

 

 

 

# Redis



#### 单线程

redis在4.0之前是完全单线程的，4.0之后引入多线程用于后台处理，eg.删除对象，核心流程（接收命令、解析命令、执行命令、返回结果等）仍为单线程。

6.0引入多线程，主要用于网络I/O阶段（接受命令，写回结果），同时读或同时写

![img](https://filescdn.proginn.com/8343358e2c0276206e707a6b4b3ec319/ebf16948ee1ac6b169952f61cc5f5c5d.webp)

 

 redis是完全基于内存操作的，通常情况下CPU不会是redis的瓶颈，而最有可能是内存大小或者网络带宽。多线程会带来额外的性能消耗。

1、基于内存的操作

2、使用了 I/O 多路复用模型，select、epoll 等，基于 reactor 模式开发了自己的网络事件处理器

3、单线程可以避免不必要的上下文切换和竞争条件，减少了这方面的性能消耗。

4、以上这三点是 redis 性能高的主要原因，其他的还有一些小优化，例如：对数据结构进行了优化，简单动态字符串、压缩列表等。

#### 使用场景

缓存（核心）、分布式锁（set + lua 脚本）、排行榜（zset）、计数（incrby）、消息队列（stream）、地理位置（geo）、访客统计（hyperloglog）等。

#### 数据结构

| 数据类型 | 可以存储的值           | 操作                                                         |
| -------- | ---------------------- | ------------------------------------------------------------ |
| STRING   | 字符串、整数或者浮点数 | 对整个字符串或者字符串的其中一部分执行操作</br> 对整数和浮点数执行自增或者自减操作 |
| LIST     | 列表                   | 从两端压入或者弹出元素 </br> 对单个或者多个元素进行修剪，</br> 只保留一个范围内的元素 |
| SET      | 无序集合               | 添加、获取、移除单个元素</br> 检查一个元素是否存在于集合中</br> 计算交集、并集、差集</br> 从集合里面随机获取元素 |
| HASH     | 包含键值对的无序散列表 | 添加、获取、移除单个键值对</br> 获取所有键值对</br> 检查某个键是否存在 |
| ZSET     | 有序集合               | 添加、获取、删除元素</br> 根据分值范围或者成员来获取元素</br> 计算一个键的排名 |

#####  String

动态字符串，采用预分配冗余空间方式来减少内存的频繁分配

常使用命令：

```javascript
set    - 设置key的值；若key存在，覆盖旧值无视类型，成功返回OK(注意大写OK，很多框架封装返回的值改变了，不要被误导了)
get    - 获取指key值；若key不存在，返回nil(不是null,不要搞错)
mset   - 同set一致，批量设置键值对，减少网络开销
mget   - 同get一致，批量获取键值对，减少网络开销
incr   - key值+1，不存在则先set后incr，返回integer切记值必须能标识为数字
incrby - 同incr一致，多出一个指定数字增量值
decr   - 同incr原理一致，操作方式变为了减法
decrby - 同incrby原理一致，操作方式变为了减法
strlen - 获取长度len，若不存在返回0，类型为integer
setnx  - 设置key值，若key不存在，则返回1，否则返回0。(若场景需要设置过期时间，不推荐使用这个命令，网络波动情况下，有可能setnx成功，expire却失败了，不是原子操作)
setex  - 设置key值及过期时间，若key已存在，则替换旧值覆盖。(若需要set值且需设置过期时间且要求较高必须要有过期时间，推荐使用这个命令，设置key值+过期时间是原子操作，要么成功要么失败)
append - 对key值进行末尾追加数据，返回值是字符串长度
```

redis 每次对 以前的值覆盖时，会 清空 TLL 值。（TTL 是过期时间）

- EX second：设置键的过期时间为 second 秒。 SET key value EX second 效果等同于 SETEX key second value 。

- PX millisecond ：设置键的过期时间为 millisecond 毫秒。 SET key value PX millisecond 效果等同于 PSETEX key millisecond value 。

- NX ：只在键不存在时，才对键进行设置操作。 SET key value NX 效果等同于 SETNX key value 。

- XX ：只在键已经存在时，才对键进行设置操作。

  在开发过程中，用 redis 来实现锁是很常用的操作。结合 NX 以及 EX 来实现。

  ```shell
  > set hello world NX EX 10     # 成功加锁，过期时间是 10s
  OK
  > set hello wolrd NX EX 10     # 在10s内执行这个命令返回错误，因为上一次的锁还没有释放
  (nil)
  > del hello                    # 释放了锁
  OK
  > set hello world NX EX 10     # 成功加锁，过期时间是 10s
  OK
  > setnx hello world            # 也可以这么写
  > setex hello 10 wolrd
  ```

应用：**计数**、**缓存基础数据**、**限制请求次数**、**分布式下共享session**、**签到**

https://cloud.tencent.com/developer/article/1828767

##### List

- 列表对象的编码可以是 ziplist 或者 双向链表 。
- 列表对象保存的所有字符串元素的长度都小于 64 字节并且保存的元素数量小于 512 个，使用 ziplist 编码；否则使用 linkedlist；
- 3.0后用quickList

###### ZipList

类似于array, 但是每个entry大小不一样，属于用时间换空间，是一个特殊的双向链表

每个节点由三部分组成：prevlength、encoding、data

- prevlengh: 记录上一个节点的长度，为了方便反向遍历ziplist
- encoding: 当前节点的编码规则，下文会详细说
- data: 当前节点的值，可以是数字或字符串

连锁更新问题：假如插入一个节点，后一个节点存储其长度的存储长度可能变化（超过254字节），并连锁变化，每一次变化都需重新分配空间

###### ZipList 和LinkedList优缺点

- 双向链表linkedlist便于在表的两端进行push和pop操作，在插入节点上复杂度很低，但是它的内存开销比较大。首先，它在每个节点上除了要保存数据之外，还要额外保存两个指针；其次，双向链表的各个节点是单独的内存块，地址不连续，节点多了容易产生内存碎片。
- ziplist存储在一段连续的内存上，所以存储效率很高。但是，它不利于修改操作，插入和删除操作需要频繁的申请和释放内存。特别是当ziplist长度很长的时候，一次realloc可能会导致大批量的数据拷贝。

###### QuickList

结合了zipList和LinkedList, 是时间和效率的折中

![img](https://pic4.zhimg.com/80/v2-800dbf77bba29897de1ad769d0149f8f_1440w.jpg)

应用：

异步队列，将待执行的任务结构体序列化（json）成字符串加入列表，另一个线程处理

但是这种方法是不安全的，因为bpop取出元素，元素只存在于客户端上下文中，不存在于redis服务器中，如果客户端崩溃会导致任务丢失。

解决方法：使用两个队列。但仍存在问题，

pub/sub同样存在消息无法持久化的问题

5.0 后引入了stream，借鉴了很多Kafka的设计思想

- 提供了消息的持久化和主备复制功能，可以让任何客户端访问任何时刻的数据，并且能记住每一个客户端的访问位置，还能保证消息不丢失。
- 引入了消费者组的概念，不同组接收到的数据完全一样（前提是条件一样），但是组内的消费者则是竞争关系。

但仍存在尚未经过大量验证、成本较高、不支持分区（partition）、无法支持大规模数据等问题。

使用kafka或rocketMQ等更为合适





##### Set

inset或hashtable

intset是一个有序数组，用二分法查找

hashtable的key存储值，value为空，实际就是通过计算hash的方式来快速排重的，这也是set能提供判断一个成员是否在集合内的原因。

应用：相比于list, 可以自动去重，可以判断某个元素是否存在，可以求交集并集等（共同关注）



##### Hash

ZipList 和 Hashtable, 当：

1. 哈希对象保存的所有键值对的键和值的字符串长度都小于64字节
2. 哈希对象保存的键值对数量小于512个

使用ziplist

redis hashtable是标准hashtable结构，使用拉链法解决哈希冲突

![img](http://redisbook.com/_images/graphviz-d2641d962325fd58bf15d9fffb4208f70251a999.png)

用两个dictht完成渐进式的rehash。rehashidx记录操作的次数，每进行一次操作，rehash一次。同时有定时辅助系统，防止长时间保持两个dictht。

应用：相比于普通存储信息的方式，不需要序列化，也没有并发控制的问题

##### ZSet

使用zipList, when:

保存元素数量小于128，并且每个元素长度小于64字节

OR

字典 + 跳跃表

dict用来查询数据到分数的对应关系，而skiplist用来根据分数查询数据（可能是范围查找）。

单独使用字典：在执行范围型操作，比如 zrank、zrange，字典需要进行排序，至少需要 O(NlogN) 的时间复杂度及额外 O(N) 的内存空间。

单独使用跳跃表：根据成员查找分值操作的复杂度从 O(1) 上升为 O(logN)。

跳跃表可作为红黑树的替代，更容易实现和调试。范围查找时更简单

##### Advance

- HyperLogLog：通常用于基数统计。使用少量固定大小的内存，来统计集合中唯一元素的数量。统计结果不是精确值，而是一个带有0.81%标准差（standard error）的近似值。所以，HyperLogLog适用于一些对于统计结果精确度要求不是特别高的场景，例如网站的UV统计。
- Geo：redis 3.2 版本的新特性。可以将用户给定的地理位置信息储存起来， 并对这些信息进行操作：获取2个位置的距离、根据给定地理位置坐标获取指定范围内的地理位置集合。
- Bitmap：位图。
- Stream：主要用于消息队列，类似于 kafka，可以认为是 pub/sub 的改进版。提供了消息的持久化和主备复制功能，可以让任何客户端访问任何时刻的数据，并且能记住每一个客户端的访问位置，还能保证消息不丢失。

##### 总结：

String: 动态字符串

List： zipList/linkedList -> quickList

Set: 数组/hashTable

Hash： zipList / HashTable

Zset: ZipList / 字典+跳表

#### 网络事件处理器

基于reactor模式，提供 select、epoll、evport、kqueue 的实现，会根据当前系统自动选择最佳的方式。

时间事件又分为：

- 定时事件：是让一段程序在指定的时间之内执行一次；
- 周期性事件：是让一段程序每隔指定时间就执行一次。

Redis 将所有时间事件都放在一个无序链表中，通过遍历整个链表查找出已到达的时间事件，并调用相应的事件处理器。

![img](https://filescdn.proginn.com/87f3706e04b975d24c2dd0c58e2f6553/88dc8f2ad1a6af417bfbd9d641ceba51.webp)

##### Reactor

![](C:\Users\jiang\Pictures\typora\reactor.png)

1. handle 

   句柄或描述符， 本质上表示一种资源，这里表示一个个事件

2. Synchronous Event Demultiplexer

   一个系统调用，如Linux中poll, epoll.

3. Initialization dispatcher

   Initiation Dispatcher会通过Synchronous Event Demultiplexer来等待事件的发生。一旦事件发生，Initiation Dispatcher首先会分离出每一个事件，然后调用事件处理器，最后调用相关的回调方法来处理这些事件。

#### 删除过期键策略

定时删除：在设置键的过期时间的同时，创建一个定时器，让定时器在键的过期时间来临时，立即执行对键的删除操作。对内存最友好，对 CPU 时间最不友好。

惰性删除：放任键过期不管，但是每次获取键时，都检査键是否过期，如果过期的话，就删除该键；如果没有过期，就返回该键。对 CPU 时间最优化，对内存最不友好。

定期删除：每隔一段时间，默认100ms，程序就对数据库进行一次检査，删除里面的过期键。至 于要删除多少过期键，以及要检査多少个数据库，则由算法决定。前两种策略的折中，对 CPU 时间和内存的友好程度较平衡。

Redis 使用惰性删除和定期删除。



#### 内存淘汰策略（maxmemory-policy）

- noeviction：默认策略，不淘汰任何 key，直接返回错误
- allkeys-lru：在所有的 key 中，使用 LRU (least recently used) 算法淘汰部分 key
- allkeys-lfu：在所有的 key 中，使用 LFU (least frequently used) 算法淘汰部分 key，该算法于 Redis 4.0 新增
- allkeys-random：在所有的 key 中，随机淘汰部分 key
- volatile-lru：在设置了过期时间的 key 中，使用 LRU 算法淘汰部分 key
- volatile-lfu：在设置了过期时间的 key 中，使用 LFU 算法淘汰部分 key，该算法于 Redis 4.0 新增
- volatile-random：在设置了过期时间的 key 中，随机淘汰部分 key
- volatile-ttl：在设置了过期时间的 key 中，挑选 TTL（time to live，剩余时间）短的 key 淘汰



##### LRU 实现

在object里保存一个unsigned的字段（LRU_BITS), 用来存储最后一次被访问的时间

具体算法：

版本一：随机取n（默认5）个，移除一个

版本二：引入缓冲池，随机取五个，放入pool, 去除pool中空闲时间最长的一个



#### 持久化机制

RDB, AOF, 混合

##### RDB

类似于快照。在某个时间点，将 Redis 在内存中的数据库状态（数据库的键值对等信息）保存到磁盘里面。RDB 持久化功能生成的 RDB 文件是经过压缩的二进制文件。

**SAVE**：生成 RDB 快照文件，但是会阻塞主进程，服务器将无法处理客户端发来的命令请求，所以通常不会直接使用该命令。

**BGSAVE**：fork 子进程来生成 RDB 快照文件，阻塞只会发生在 fork 子进程的时候，之后主进程可以正常处理请求，详细过程如下图：

![img](https://filescdn.proginn.com/f67b9d043c644c5021065232d206a67e/a7c9ae17def00aacaf8a6b10fe92c5d4.webp)



**RDB 的优点**

1）RDB 文件是是经过压缩的二进制文件，占用空间很小，它保存了 Redis 某个时间点的数据集，很适合用于做备份。 比如说，你可以在最近的 24 小时内，每小时备份一次 RDB 文件，并且在每个月的每一天，也备份一个 RDB 文件。这样的话，即使遇上问题，也可以随时将数据集还原到不同的版本。

2）RDB 非常适用于灾难恢复（disaster recovery）：它只有一个文件，并且内容都非常紧凑，可以（在加密后）将它传送到别的数据中心。

3）RDB 可以最大化 redis 的性能。父进程在保存 RDB 文件时唯一要做的就是 fork 出一个子进程，然后这个子进程就会处理接下来的所有保存工作，父进程无须执行任何磁盘 I/O 操作。

4）RDB 在恢复大数据集时的速度比 AOF 的恢复速度要快。

**RDB 的缺点**

1）RDB 在服务器故障时容易造成数据的丢失。RDB 允许我们通过修改 save point 配置来控制持久化的频率。但是，因为 RDB 文件需要保存整个数据集的状态， 所以它是一个比较重的操作，如果频率太频繁，可能会对 Redis 性能产生影响。所以通常可能设置至少5分钟才保存一次快照，这时如果 Redis 出现宕机等情况，则意味着最多可能丢失5分钟数据。

2）RDB 保存时使用 fork 子进程进行数据的持久化，如果数据比较大的话，fork 可能会非常耗时，造成 Redis 停止处理服务N毫秒。如果数据集很大且 CPU 比较繁忙的时候，停止服务的时间甚至会到一秒。

3）Linux fork 子进程采用的是 copy-on-write 的方式。在 Redis 执行 RDB 持久化期间，如果 client 写入数据很频繁，那么将增加 Redis 占用的内存，最坏情况下，内存的占用将达到原先的2倍。刚 fork 时，主进程和子进程共享内存，但是随着主进程需要处理写操作，主进程需要将修改的页面拷贝一份出来，然后进行修改。极端情况下，如果所有的页面都被修改，则此时的内存占用是原先的2倍。



##### AOF

保存 Redis 服务器所执行的所有写操作命令来记录数据库状态，并在服务器启动时，通过重新执行这些命令来还原数据集。

AOF 持久化功能的实现可以分为三个步骤：命令追加、文件写入、文件同步。

命令追加：当 AOF 持久化功能打开时，服务器在执行完一个写命令之后，会将被执行的写命令追加到服务器状态的 aof 缓冲区（aof_buf）的末尾。

文件写入与文件同步：Linux 操作系统中为了提升性能，使用了页缓存（page cache）。当我们将 aof_buf 的内容写到磁盘上时，此时数据并没有真正的落盘，而是在 page cache 中，为了将 page cache 中的数据真正落盘，需要执行 fsync / fdatasync 命令来强制刷盘。

**AOF 的优点**

1. AOF 比 RDB可靠。你可以设置不同的 fsync 策略：no、everysec 和 always。默认是 everysec，在这种配置下，redis 仍然可以保持良好的性能，并且就算发生故障停机，也最多只会丢失一秒钟的数据。
2. AOF文件是一个纯追加的日志文件。即使日志因为某些原因而包含了未写入完整的命令（比如写入时磁盘已满，写入中途停机等等）， 我们也可以使用 redis-check-aof 工具也可以轻易地修复这种问题。
3. 当 AOF文件太大时，Redis 会自动在后台进行重写：重写后的新 AOF 文件包含了恢复当前数据集所需的最小命令集合。整个重写是绝对安全，因为重写是在一个新的文件上进行，同时 Redis 会继续往旧的文件追加数据。当新文件重写完毕，Redis 会把新旧文件进行切换，然后开始把数据写到新文件上。
4. AOF 文件有序地保存了对数据库执行的所有写入操作以 Redis 协议的格式保存， 因此 AOF 文件的内容非常容易被人读懂， 对文件进行分析（parse）也很轻松。如果你不小心执行了 FLUSHALL 命令把所有数据刷掉了，但只要 AOF 文件没有被重写，那么只要停止服务器， 移除 AOF 文件末尾的 FLUSHALL 命令， 并重启 Redis ， 就可以将数据集恢复到 FLUSHALL 执行之前的状态。

**AOF 的缺点**

1. 对于相同的数据集，AOF 文件的大小一般会比 RDB 文件大。
2. 根据所使用的 fsync 策略，AOF 的速度可能会比 RDB 慢。通常 fsync 设置为每秒一次就能获得比较高的性能，而关闭 fsync 可以让 AOF 的速度和 RDB 一样快。
3. AOF 在过去曾经发生过这样的 bug ：因为个别命令的原因，导致 AOF 文件在重新载入时，无法将数据集恢复成保存时的原样。（举个例子，阻塞命令 BRPOPLPUSH 就曾经引起过这样的 bug ） 。虽然这种 bug 在 AOF 文件中并不常见， 但是相较而言， RDB 几乎是不可能出现这种 bug 的。



###### 重写

当AOF文件过大时，重建文件：遍历键值对并用set命令记录

AOF后台重写使用子线程，但主线程仍在处理数据，所以可能出现数据不一致的情况

为了解决上述问题，Redis 引入了 AOF 重写缓冲区（aof_rewrite_buf_blocks），这个缓冲区在服务器创建子进程之后开始使用，当 Redis 服务器执行完一个写命令之后，它会同时将这个写命令追加到 AOF 缓冲区和 AOF 重写缓冲区。

这样不仅可以保证现有AOF文件的安全，也能解决数据不一致的问题。



##### 混合持久化

在AOF重写的时候使用RDB，重写后的文件前半段是RDB，后半段是AOF

同时使用RDB和AOF是，redis优先使用AOF，因为一般来说，AOF的数据更完整



#### 高可用

##### 主从复制

redis 6.0中，流程如下：

1. 开启主从复制

2. 建立socket连接

   Slave向master发起socket连接

3. 发送PING命令

   slave 向 master 发送一个 PING 命令，以检査套接字的读写状态是否正常、 master 能否正常处理命令请求。

4. 身份验证

   slave 向 master 发送 AUTH password 命令来进行身份验证。

5. 发送端口信息

   在身份验证通过后后， slave 将向 master 发送自己的监听端口号， master 收到后记录在 slave 所对应的客户端状态的 slave_listening_port 属性中。

6. 发送IP地址

   如果配置了 slave_announce_ip，则 slave 向 master 发送 slave_announce_ip 配置的 IP 地址， master 收到后记录在 slave 所对应的客户端状态的 slave_ip 属性。

   该配置是用于解决服务器返回内网 IP 时，其他服务器无法访问的情况。可以通过该配置直接指定公网 IP。

7. 发送CAPA

   CAPA 全称是 capabilities，这边表示的是同步复制的能力。slave 会在这一阶段发送 capa 告诉 master 自己具备的（同步）复制能力， master 收到后记录在 slave 所对应的客户端状态的 slave_capa 属性。

8. 数据同步

   slave 将向 master 发送 PSYNC 命令， master 收到该命令后判断是进行部分重同步还是完整重同步，然后根据策略进行数据的同步。

   * 完整重同步：处理初次复制情况，从服务器（Slave）先让主服务器（Master）创建并发送RDB文件，然后再将主服务器的缓冲区里的命令发送给从服务器，以此完成同步；
   * 部分重同步：用于处理断线后复制的情况，在断线重连后，只要将断线期间主服务器产生的命令发送给从服务器，就可以完成数据同步了。

9. 命令传播

   当完成了同步之后，就会进入命令传播阶段，这时 master 只要一直将自己执行的写命令发送给 slave ，而 slave 只要一直接收并执行 master 发来的写命令，就可以保证 master 和 slave 一直保持一致了。



以部分重同步为例，主从复制的核心步骤流程图如下：

![img](https://filescdn.proginn.com/0dc49ce8283b3e570232ce4ed7884fcd/fc86d7f18d7e90a7229cc1ec2b24a192.webp)

问题：master节点下线后，需手动设置新master



##### 哨兵 Sentinel

一个或多个sentinel监视多个主服务器，及其下属服务器。当主服务器下线时，可替换新主服务器。

###### 故障检测

* 主观下线：

  sentinel每秒一次发送ping命令判断服务器状态，如果在限定时间内没有收到有效回复，主观下线

* 客观下线

  询问其他sentinel，若超过一定数量sentinel认定下线，客观下线，进行故障转移

###### 故障转移

1. 选举出领头sentinel

   监视该主节点的所有哨兵都有可能被选为领导者，选举使用的算法是Raft算法；Raft算法的基本思路是先到先得

2. 领头sentinel选出新主服务器

   选择的原则是，首先过滤掉不健康的从节点；然后选择优先级最高的从节点(由slave-priority指定)；如果优先级无法区分，则选择复制偏移量最大的从节点；如果仍无法区分，则选择runid最小的从节点。

3. 领头sentinel将其他从服务器设置为复制新主服务器

4. 领头sentinel更改配置，当原来主服务器上线时，成为新主服务器的从服务器

问题： 没有实现数据分片存储，每个redis实例里存储的都是全量数据, qps上限低



##### 集群

高可用 + 数据分区

###### 步骤

1. 启动节点

   该阶段节点没有主从关系

2. 节点握手

   每个节点都刚知道其他节点

3. 分配槽

   共16384个，必须都分配上节点

4. 指定主从关系

###### 集群设计方案

（1）高可用要求：根据故障转移的原理，至少需要3个主节点才能完成故障转移，且3个主节点不应在同一台物理机上；每个主节点至少需要1个从节点，且主从节点不应在一台物理机上；因此高可用集群至少包含6个节点。

（2）数据量和访问量：估算应用需要的数据量和总访问量(考虑业务发展，留有冗余)，结合每个主节点的容量和能承受的访问量(可以通过benchmark得到较准确估计)，计算需要的主节点数量。

（3）节点数量限制：Redis官方给出的节点数量限制为1000，主要是考虑节点间通信带来的消耗。在实际应用中应尽量避免大集群；如果节点数量不足以满足应用对Redis数据量和访问量的要求，可以考虑：(1)业务分割，大集群分为多个小集群；(2)减少不必要的数据；(3)调整数据过期策略等。

（4）适度冗余：Redis可以在不影响集群服务的情况下增加节点，因此节点数量适当冗余即可，不用太大。

###### 数据分区

redis使用哈希方法，对数据的特征值进行哈希。

衡量数据分区方法的好坏主要有两个元素：

1. 数据分布是否均匀 （哈希法天然满足）
2. 增加或删减节点对数据分布的影响

几个哈希法：

1. 取余

   对节点数量取余，当节点数量变化时，变动较大

2. 一致性哈希分区

   ![img](https://img2018.cnblogs.com/blog/1174710/201810/1174710-20181025213424713-1246878063.png)

​		将增减节点的影响限制在相邻节点，问题时节点数量较少时，节点数量变化，对单个节点影响可能会很大



3. 带虚拟节点的一致性哈希分区

   在使用了槽的一致性哈希分区中，槽是数据管理和迁移的基本单位。槽解耦了数据和实际节点之间的关系，增加或删除节点对系统的影响很小。

   ![img](https://img2018.cnblogs.com/blog/1174710/201908/1174710-20190802191100758-1110624103.png)

   在线扩容时，向访问源节点，如源节点未找到信息，返回一个ASK错误，指引客户端寻找正确的节点

###### 节点通信

* 端口

  集群中每个节点既是数据节点，也参与集群的维护，所以有两个端口。普通端口负责和客户端的通信，集群端口用于和集群通信

* 通信协议

  节点间通信，按照通信协议可以分为几种类型：单对单、广播、Gossip协议等。重点是广播和Gossip的对比。

  * 广播是指向集群内所有节点发送消息；优点是集群的收敛速度快(集群收敛是指集群内所有节点获得的集群信息是一致的)，缺点是每条消息都要发送给所有节点，CPU、带宽等消耗较大。

  * Gossip协议的特点是：在节点数量有限的网络中，每个节点都“随机”的与部分节点通信（并不是真正的随机，而是根据特定的规则选择通信的节点），经过一番杂乱无章的通信，每个节点的状态很快会达到一致。Gossip协议的优点有负载(比广播)低、去中心化、容错性高(因为通信有冗余)等；缺点主要是集群的收敛速度慢。

* 消息类型

  - MEET消息：在节点握手阶段，当节点收到客户端的CLUSTER MEET命令时，会向新加入的节点发送MEET消息，请求新节点加入到当前集群；新节点收到MEET消息后会回复一个PONG消息。
  - PING消息：集群里每个节点每秒钟会选择部分节点发送PING消息，接收者收到消息后会回复一个PONG消息。PING消息的内容是自身节点和部分其他节点的状态信息；作用是彼此交换信息，以及检测节点是否在线。PING消息使用Gossip协议发送，接收节点的选择兼顾了收敛速度和带宽成本，具体规则如下：(1)随机找5个节点，在其中选择最久没有通信的1个节点(2)扫描节点列表，选择最近一次收到PONG消息时间大于cluster_node_timeout/2的所有节点，防止这些节点长时间未更新。
  - PONG消息：PONG消息封装了自身状态数据。可以分为两种：第一种是在接到MEET/PING消息后回复的PONG消息；第二种是指节点向集群广播PONG消息，这样其他节点可以获知该节点的最新信息，例如故障恢复后新的主节点会广播PONG消息。
  - FAIL消息：当一个主节点判断另一个主节点进入FAIL状态时，会向集群广播这一FAIL消息；接收节点会将这一FAIL消息保存起来，便于后续的判断。
  - PUBLISH消息：节点收到PUBLISH命令后，会先执行该命令，然后向集群广播这一消息，接收节点也会执行该PUBLISH命令。

#### 事件

redis服务器是事件驱动程序

##### 文件事件

套接字操作的抽象， 基于reactor的模式

##### 时间事件

定时操作的抽象，分为定时事件和周期性事件

Redis 将所有时间事件都放在一个无序链表中，通过遍历整个链表查找出已到达的时间事件，并调用相应的事件处理器。

##### 事件的调度和执行

```python
def aeProcessEvents():
    # 获取到达时间离当前时间最接近的时间事件
    time_event = aeSearchNearestTimer()
    # 计算最接近的时间事件距离到达还有多少毫秒
    remaind_ms = time_event.when - unix_ts_now()
    # 如果事件已到达，那么 remaind_ms 的值可能为负数，将它设为 0
    if remaind_ms < 0:
        remaind_ms = 0
    # 根据 remaind_ms 的值，创建 timeval
    timeval = create_timeval_with_ms(remaind_ms)
    # 阻塞并等待文件事件产生，最大阻塞时间由传入的 timeval 决定
    aeApiPoll(timeval)
    # 处理所有已产生的文件事件
    procesFileEvents()
    # 处理所有已到达的时间事件
    processTimeEvents()
```



#### 客户端

##### Jedis：

比较全面的提供了 Redis的操作特性

##### Redisson：

促使使用者对 Redis 的关注分离，提供很多分布式相关操作服务，例如，分布式锁，分布式集台，可通过 Redis 支持延退队列

##### Lettuce：

基于 Netty 框架的事件驱动的通信层，其方法调用是异步的。Lettuce 的 API 是程安全的，所以可以操作单个 Lettuce 连接来完成各种操作

```java
RedisClient redisClient = RedisClient.create("redis://localhost/0");
StatefulRedisConnection<String, String> connection = redisClient.connect();

System.out.println("Connected to Redis");
connection.sync().set("key", "Hello World");

connection.close();
redisClient.shutdown(); 
```



```java
interface MyCommandInterface extends Commands {

  String get(String key);

  @Command("GET")
  byte[] getAsByteArray(String key);

  @Command("NR.RUN net")
  double neuralRun(int from, int to);
}
```



优先使用Lettuce，如果需要分布式锁，分布式集合等分布式的高级特性，结合redisson使用，因为redisson对字符串的操作支持很差。



#### 缓存

##### Cache Penetration（缓存穿透）

request data that does not exist in either the cache or the database. eg. impossible data. Can be a kind of attack, can lead to excessive pressure on the database. 

Solution:

1. filter illegal request
2. cache empty/null result, set expiration time short

##### Cache Breakdown

Data not in cache but in database.

Solutions:

1. hot data no expiration. Use an asynchronized thread to rewrite the cache when the data is updated, or refresh regularly.
2. Adopt a mutex approach. one thread access the data from the database and updated the cache.

##### Cache Avalanche

cache is empty, such as set the same expiration time.

Solution:

1. hot data never expire
2. use difference expiration time
3. Double caching. Only A set expiration time. if data not found in A, read form B and update A asynchronously.

#### Distributed Lock with Redis

Requirements:

1. Safety property : mutual exclusion
2. Liveness property A: Deadlock free
3. Liveness property B: Fault tolerance

Solutions:

1. setnx

   setnx will only set value to key that does not exist. Return 1 if success

   problem will happen if the Redis master goes down:

   ![](C:\Users\jiang\Pictures\typora\redis key.png)

2. **Single instance**: Suggested by Redis

   https://redis.io/docs/reference/patterns/distributed-locks/

   ```java
   SET resource_name random_value NX PX 30000
   ```

   ![](C:\Users\jiang\Pictures\typora\redis key java.jpg)

   NX: set the key only if it does not exist

   PX: set expiration time

   random_value: must be unique across all clients and lock request. This is to release the lock in a save way

   ```lua
   if redis.call("get",KEYS[1]) == ARGV[1] then
       return redis.call("del",KEYS[1])
   else
       return 0
   end
   ```

   This is to avoid release other client's key. eg. when a client perform some operations for longer than the lock validity time, and later trying to romove the lock.

   **Distributed Algorithm**

   ![](C:\Users\jiang\Pictures\typora\redis distributed lock.png)

   Mutual exclusive is ensured because only one client can get N/2+1 keys

















# Kafka

#### 消息队列

1. 解耦

2. 可恢复性

   处理消息的进程挂掉，队列中消息不会丢失

3. 缓冲

   解决生产消息和消费消息处理速度不一致的问题

4. 削峰填谷

5. 异步通信

#### Kafka 优势

1. 高吞吐量，低延迟
2. 可扩展性， 支持热扩展
3. 持久性，可靠性： 消息被持久化到本地磁盘
4. 容错性：多副本
5. 高并发

#### 架构

![](C:\Users\jiang\Pictures\typora\kafka.png)

为实现**可扩展性**， 并提高并发， 每个**topic**可分部到多个**broker**， 分为多个**partition** 

#### 分区策略

1. DefaultPartioner

   - 如果消息中指定了分区，则使用它
   - 如果未指定分区但存在key，则根据序列化key使用murmur2哈希算法对分区数取模。
   - 如果不存在分区或key，则会使用**粘性分区策略**，关于粘性分区请参阅 KIP-480。

   **Sticky Partioner**

   发送消息时，先放入batch，batch满后一起发送，减少请求数量

   如果使用round robin 的话会存在分区迟迟不满，延迟增大的问题，所以使用粘性分区，优先填满一个batch

   ![](C:\Users\jiang\Pictures\typora\sticky.jpg)

2. UniformStickyPartitioner

   有key也使用sticky

3. Round Robin

#### 可靠性

##### ACK应答机制

三种可靠性级别， acks参数配置

1. 0： producer 不等待broker的ack, 延迟最低，但有可能丢失数据
2. 1：等待ack， partition的leader落盘成功后返回ack, 也有可能丢失数据，比如在follower同步之前leader故障
3. -1： 等待，复制完成后返回ACK， 但是在同步完成后，发送ack之前， leader发生故障，会导致follower中数据重复

##### 副本数据同步策略：

1. 半数完成同步，发送ACK， 延迟低，选举新的leader时，容忍n台节点的故障，需要2n+1个副本
2. 全部完成，延迟高，更可靠

选择二的原因是会造成大量数据的冗余， 网络延迟对Kafka影响较小

##### IRS (in-sync replica set)

如果一个follower长时间不能完成同步 (延迟时间和延迟条数任意一个超过)，将其剔除set，发生故障时，从IRS中选举新的

所有副本： **Assigned Replicas (AR)**

#### 故障处理

![](C:\Users\jiang\Pictures\typora\hw.png)

**HW (High WaterMark):** 只有start到HW之间可读

**LEO (Log End offset)**: 下一条要写入消息的offset

**分区 ISR 集合中的每个副本都会维护自己的 LEO，而 ISR 集合中最小的LEO 即为分区的 HW**

#### Offset

信息以**log文件**形式存在broker， 消费后不会消失。 使用offset来**唯一标识**一个segment中的数据。

消费者消费一条消息后，需要**提交offset**表示自己消费到哪儿了。

**kafka 0.9**前，offset维护在**zookeeper**中，但zookeeper不适合大量写入， 现在维护在**_consumer_offsets**这个topic下。采用key-value形式，key为group.id+topic+partitonid

##### auto.commit

默认自动提交offset，可以设置自动提交的时间间隔，默认5s

##### 手动提交

1. consumer.commit(offset)

   会产生阻塞，影响吞吐量

2. consumer.StoreOffset(offset)

   允许Kafka在后台线程帮我们自动提交，但offset更新手动控制，兼顾性能和可靠性

#### 漏消费和重复消费

##### 漏消费

如果先提交offset再消费，就可能出现漏消费（消费者挂掉）

解决方法：先消费再提交

##### 重复消费

如果使用自动提交，可能出现重复消费：消息已消费，未提交，consumer挂掉

或处理数据耗时超过了Kafka的session timeout， 会导致**re-balance**（重新分配partition给group中的每个consumer）

解决方法：使用手动消费，但不能保证不会重复消费，所以需要幂等性处理：eg. 落表（主键或唯一索引）， 业务逻辑处理（redis）

#### 消息积压

1. Kafka消费能力不足

   增加partition，并同步增加consumer实例数

2. Consumer数据处理不及时

   提高每批次拉取的数量

#### 有序性

Kafka只保证partition内消息有序，可以指定key，将消息发到同一个partition中保证有序



#### 分区和吞吐量的关系

在一定条件下分区和吞吐量成正比， 但超过一定限度会对性能产生影响

1. 服务端和客户端需要使用的内存增多

   生产者batch更多

   broker有分区级别的缓存

   消费者线程更多

2. 文件句柄的开销

   每个 partition 都会对应磁盘文件系统的一个目录。在 Kafka 的数据日志文件目录中，每个日志数据段都会分配两个文件，一个索引文件和一个数据文件。每个 broker 会为每个日志段文件打开一个 index 文件句柄和一个数据文件句柄。因此，随着 partition 的增多，所需要保持打开状态的文件句柄数也就越多，最终可能超过底层操作系统配置的文件句柄数量限制。

3. 增加端对端的延迟

   需要更多的同步

4. 降低刚可用性

   re-balance所用时间增加



#### 速度快的原因

1. 顺序读写

   利用磁盘的顺序读写

2. page cache

   直接利用操作系统的Page Cache

3. 零拷贝

   直接将数据从内核空间的读缓冲区拷贝到内核空间的socket缓冲区

4. 分区分段 + 索引

5. 批量读写

6. 批量压缩



# K8S

Kubernetes（k8s）是自动化容器操作的开源平台。这些容器操作包括：部署、调度和节点集群间扩展。



- 自动化容器部署和复制。

- 实时弹性收缩容器规模。

- 容器编排成组，并提供容器间的负载均衡

  

  

  

  ![](C:\Users\jiang\Pictures\typora\k8s.jpg)

#### 架构

##### **Master节点**

Master节点指的是集群控制节点，管理和控制整个集群，基本上k8s的所有控制命令都发给它，它负责具体的执行过程。在Master上主要运行着：

1. Kubernetes Controller Manager（kube-controller-manager）：k8s中所有资源对象的自动化控制中心，维护管理集群的状态，比如故障检测，自动扩展，滚动更新等。
2. Kubernetes Scheduler（kube-scheduler）： 负责资源调度，按照预定的调度策略将Pod调度到相应的机器上。
3. etcd：保存整个集群的状态。

##### **Node节点**

除了master以外的节点被称为Node或者Worker节点，可以在master中使用命令 `kubectl get nodes`查看集群中的node节点。每个Node都会被Master分配一些工作负载（Docker容器），当某个Node宕机时，该节点上的工作负载就会被Master自动转移到其它节点上。在Node上主要运行着：

1. kubelet：负责Pod对应的容器的创建、启停等任务，同时与Master密切协作，实现集群管理的基本功能
2. kube-proxy：实现service的通信与负载均衡
3. docker（Docker Engine）：Docker引擎，负责本机的容器创建和管理

#### 两地三中心

本地生产中心、本地灾备中心、异地灾备中心。

两地三中心要解决的一个重要问题就是数据一致性问题。（RAFT）



#### 四层服务发现

k8s提供了两种方式进行服务发现：

环境变量：当创建一个Pod的时候，kubelet会在该Pod中注入集群内所有Service的相关环境变量。需要注意的是，要想一个Pod中注入某个Service的环境变量，则必须Service要先比该Pod创建。这一点，几乎使得这种方式进行服务发现不可用。 搜索公众号互联网架构师复“2T”，送你一份惊喜礼包 。

DNS：可以通过cluster add-on的方式轻松的创建KubeDNS来对集群内的Service进行服务发现。

以上两种方式，一个是基于tcp，众所周知，DNS是基于UDP的，它们都是建立在四层协议之上。

#### 五种pod共享资源

Pod是K8s最基本的操作单元，包含一个或多个紧密相关的容器，一个Pod可以被一个容器化的环境看作应用层的“逻辑宿主机”；一个Pod中的多个容器应用通常是紧密耦合的，Pod在Node上被创建、启动或者销毁；每个Pod里运行着一个特殊的被称之为Volume挂载卷，因此他们之间通信和数据交换更为高效，在设计时我们可以充分利用这一特性将一组密切相关的服务进程放入同一个Pod中。

同一个Pod里的容器之间仅需通过localhost就能互相通信。

一个Pod中的应用容器共享五种资源：

- PID命名空间：Pod中的不同应用程序可以看到其他应用程序的进程ID。
- 网络命名空间：Pod中的多个容器能够访问同一个IP和端口范围。
- IPC命名空间：Pod中的多个容器能够使用SystemV IPC或POSIX消息队列进行通信。
- UTS命名空间：Pod中的多个容器共享一个主机名。
- Volumes(共享存储卷)：Pod中的各个容器可以访问在Pod级别定义的Volumes。



#### 六个CNI常用插件

CNI(Container Network Interface)容器网络接口，是Linux容器网络配置的一组标准和库，用户需要根据这些标准和库来开发自己的容器网络插件。CNI只专注解决容器网络连接和容器销毁时的资源释放，提供一套框架，所以CNI可以支持大量不同的网络模式，并且容易实现。

![](C:\Users\jiang\Pictures\typora\CNI.jpeg)

#### 七层负载均衡

- 二层负载均衡：基于MAC地址的二层负载均衡。

- 三层负载均衡：基于IP地址的负载均衡。

- 四层负载均衡：基于IP+端口的负载均衡。

- 七层负载均衡：基于URL等应用层信息的负载均衡。

  上面四层服务发现讲的主要是k8s原生的kube-proxy方式。K8s关于服务的暴露主要是通过NodePort方式，通过绑定minion主机的某个端口，然后进行pod的请求转发和负载均衡，但这种方式有下面的缺陷：

  - Service可能有很多个，如果每个都绑定一个node主机端口的话，主机需要开放外围的端口进行服务调用，管理混乱。
  - 无法应用很多公司要求的防火墙规则。

  理想的方式是通过一个外部的负载均衡器，绑定固定的端口，比如80，然后根据域名或者服务名向后面的Service ip转发，Nginx很好的解决了这个需求，但问题是如果有的心得服务加入，如何去修改Nginx的配置，并且加载这些配置？Kubernetes给出的方案就是Ingress。这是一个基于7层的方案。

  ![](C:\Users\jiang\Pictures\typora\ingress.png)

#### 八种隔离维度

![](C:\Users\jiang\Pictures\typora\八种隔离维度.jpeg)

#### 九个网络模型原则



#### Raft

Leader选举（Leader election）、日志同步（Log replication）、安全性（Safety）、日志压缩（Log compaction）、成员变更（Membership change）

- **Leader**：接受客户端请求，并向Follower同步请求日志，当日志同步到大多数节点上后告诉Follower提交日志。
- **Follower**：接受并持久化Leader同步的日志，在Leader告之日志可以提交之后，提交日志。
- **Candidate**：Leader选举过程中的临时角色。

##### Leader选举

如果Follower在选举超时时间内没有收到Leader的heartbeat，就会等待一段随机的时间后发起一次Leader选举。

Follower将其当前term加一然后转换为Candidate。它首先给自己投票并且给集群中的其他服务器发送 RequestVote RPC （RPC细节参见八、Raft算法总结）。结果有以下三种情况：

- 赢得了多数的选票，成功选举为Leader；
- 收到了Leader的消息，表示有其它服务器已经抢先当选了Leader；
- 没有服务器赢得多数的选票，Leader选举失败，等待选举时间超时后发起下一次选举。



##### 日志同步

Leader选出后，就开始接收客户端的请求。Leader把请求作为日志条目（Log entries）加入到它的日志中，然后并行的向其他服务器发起 AppendEntries RPC （RPC细节参见八、Raft算法总结）复制日志条目。当这条日志被复制到大多数服务器上，Leader将这条日志应用到它的状态机并向客户端返回执行结果。

某些Followers可能没有成功的复制日志，Leader会无限的重试 AppendEntries RPC直到所有的Followers最终存储了所有的日志条目。

日志由有序编号（log index）的日志条目组成。每个日志条目包含它被创建时的任期号（term），和用于状态机执行的命令。如果一个日志条目被复制到大多数服务器上，就被认为可以提交（commit）了

Raft日志同步保证如下两点：

- 如果不同日志中的两个条目有着相同的索引和任期号，则它们所存储的命令是相同的。
- 如果不同日志中的两个条目有着相同的索引和任期号，则它们之前的所有条目都是完全一样的。

第一条特性源于Leader在一个term内在给定的一个log index最多创建一条日志条目，同时该条目在日志中的位置也从来不会改变。

第二条特性源于 AppendEntries 的一个简单的一致性检查。当发送一个 AppendEntries RPC 时，Leader会把新日志条目紧接着之前的条目的log index和term都包含在里面。如果Follower没有在它的日志中找到log index和term都相同的日志，它就会拒绝新的日志条目。

一般情况下，Leader和Followers的日志保持一致，因此 AppendEntries 一致性检查通常不会失败。然而，Leader崩溃可能会导致日志不一致：旧的Leader可能没有完全复制完日志中的所有条目。

Leader和Followers上日志不一致

上图阐述了一些Followers可能和新的Leader日志不同的情况。一个Follower可能会丢失掉Leader上的一些条目，也有可能包含一些Leader没有的条目，也有可能两者都会发生。丢失的或者多出来的条目可能会持续多个任期。

Leader通过强制Followers复制它的日志来处理日志的不一致，Followers上的不一致的日志会被Leader的日志覆盖。

Leader为了使Followers的日志同自己的一致，Leader需要找到Followers同它的日志一致的地方，然后覆盖Followers在该位置之后的条目。

Leader会从后往前试，每次AppendEntries失败后尝试前一个日志条目，直到成功找到每个Follower的日志一致位点，然后向后逐条覆盖Followers在该位置之后的条目。

##### 安全性

Raft增加了如下两条限制以保证安全性：

- 拥有最新的已提交的log entry的Follower才有资格成为Leader。

这个保证是在RequestVote RPC中做的，Candidate在发送RequestVote RPC时，要带上自己的最后一条日志的term和log index，其他节点收到消息时，如果发现自己的日志比请求中携带的更新，则拒绝投票。日志比较的原则是，如果本地的最后一条log entry的term更大，则term大的更新，如果term一样大，则log index更大的更新。

- Leader只能推进commit index来提交当前term的已经复制到大多数服务器上的日志，旧term日志的提交要等到提交当前term的日志来间接提交（log index 小于 commit index的日志被间接提交）。

之所以要这样，是因为可能会出现已提交的日志又被覆盖的情况

##### 日志压缩

在实际的系统中，不能让日志无限增长，否则系统重启时需要花很长的时间进行回放，从而影响可用性。Raft采用对整个系统进行snapshot来解决，snapshot之前的日志都可以丢弃。

每个副本独立的对自己的系统状态进行snapshot，并且只能对已经提交的日志记录进行snapshot。

Snapshot中包含以下内容：

- 日志元数据。最后一条已提交的 log entry的 log index和term。这两个值在snapshot之后的第一条log entry的AppendEntries RPC的完整性检查的时候会被用上。
- 系统当前状态。

当Leader要发给某个日志落后太多的Follower的log entry被丢弃，Leader会将snapshot发给Follower。或者当新加进一台机器时，也会发送snapshot给它。发送snapshot使用InstalledSnapshot RPC（RPC细节参见八、Raft算法总结）。

做snapshot既不要做的太频繁，否则消耗磁盘带宽， 也不要做的太不频繁，否则一旦节点重启需要回放大量日志，影响可用性。推荐当日志达到某个固定的大小做一次snapshot。

做一次snapshot可能耗时过长，会影响正常日志同步。可以通过使用copy-on-write技术避免snapshot过程影响正常日志同步。

##### 成员变更

成员变更是在集群运行过程中副本发生变化，如增加/减少副本数、节点替换等。

成员变更也是一个分布式一致性问题，既所有服务器对新成员达成一致。但是成员变更又有其特殊性，因为在成员变更的一致性达成的过程中，参与投票的进程会发生变化。

如果将成员变更当成一般的一致性问题，直接向Leader发送成员变更请求，Leader复制成员变更日志，达成多数派之后提交，各服务器提交成员变更日志后从旧成员配置（Cold）切换到新成员配置（Cnew）。

因为各个服务器提交成员变更日志的时刻可能不同，造成各个服务器从旧成员配置（Cold）切换到新成员配置（Cnew）的时刻不同。

成员变更不能影响服务的可用性，但是成员变更过程的某一时刻，可能出现在Cold和Cnew中同时存在两个不相交的多数派，进而可能选出两个Leader，形成不同的决议，破坏安全性。

我们在原有集群中有Server1, Server2, Server3三个服务器。现在想加入Server4和Server5。若Server3首先进行了更新，这个时候就出现了问题。

在Cnew新的配置中，5个服务器的大多数为3，在Cold旧的配置文件中，3个服务器的大多数为2。而现在就可能出现有2个Leader的情况。Server1请求Server2给自己投票，便获得了大多数投票。Server4请求Server3和Server5投票，也获得了自己配置文件中的大多数投票。

由于成员变更的这一特殊性，成员变更不能当成一般的一致性问题去解决。

为了解决这一问题，Raft提出了两阶段的成员变更方法。集群先从旧成员配置Cold切换到一个过渡成员配置，称为共同一致（joint consensus），共同一致是旧成员配置Cold和新成员配置Cnew的组合Cold U Cnew，一旦共同一致Cold U Cnew被提交，系统再切换到新成员配置Cnew。

1. Leader收到成员变更请求从Cold切成Cold,new；
2. Leader在本地生成一个新的log entry，其内容是Cold∪Cnew，代表当前时刻新旧成员配置共存，写入本地日志，同时将该log entry复制至Cold∪Cnew中的所有副本。在此之后新的日志同步需要保证得到Cold和Cnew两个多数派的确认；
3. Follower收到Cold∪Cnew的log entry后更新本地日志，并且此时就以该配置作为自己的成员配置；
4. 如果Cold和Cnew中的两个多数派确认了Cold U Cnew这条日志，Leader就提交这条log entry并切换到Cnew；
5. 接下来Leader生成一条新的log entry，其内容是新成员配置Cnew，同样将该log entry写入本地日志，同时复制到Follower上；
6. Follower收到新成员配置Cnew后，将其写入日志，并且从此刻起，就以该配置作为自己的成员配置，并且如果发现自己不在Cnew这个成员配置中会自动退出；
7. Leader收到Cnew的多数派确认后，表示成员变更成功，后续的日志只要得到Cnew多数派确认即可。Leader给客户端回复成员变更执行成功。

##### 与Multi-paxos

![](C:\Users\jiang\Pictures\typora\raft&paxos.png)

# 算法



1. 排序

![Table  Description automatically generated](file:///C:/Users/jiang/AppData/Local/Temp/msohtmlclip1/01/clip_image062.jpg)

1. Bubble Sort

遍历交换顺序错误的元素，知道一次遍历不再需要交换

2. Selection Sort

选出最小的元素放在开头

3. Insertion Sort

类似于整理手上的牌，把每一张插入应在的位置

4. Shell Sort （复杂度由gap决定）

非稳定，insertion sort的提升，不再一一交换而是可以远距离交换元素，交换距离从大到小直到1

稳定性：相同值的元素顺序不改变

5. # Merge sort https://www.geeksforgeeks.org/merge-sort/      

 

####  树

![](C:\Users\jiang\Pictures\typora\tree.png)

 

 

 

 

# **Spring**



特点： 轻量，松耦合 （loose coupling），可集成其他框架

1.4. Spring Framework 有哪些不同的功能？

 

轻量级 - Spring 在代码量和透明度方面都很轻便。

IOC - 控制反转

AOP - 面向切面编程可以将应用业务逻辑和系统服务分离，以实现高内聚。

容器 - Spring 负责创建和管理对象（Bean）的生命周期和配置。

MVC - 对 web 应用提供了高度可配置性，其他框架的集成也十分方便。

事务管理 - 提供了用于事务管理的通用抽象层。Spring 的事务支持也可用于容器较少的环境。

JDBC 异常 - Spring 的 JDBC 抽象层提供了一个异常层次结构，简化了错误处理策略。

 

ORM (Object Relational Mapping), 通过使用描述对象与数据库之间映射的元数据，将程序中的对象自动持久化到关系数据库中。如Hibernate, ibatis, speedframework.



### InitializingBean

实现此接口为bean提供初始化方法的方式，它只包括afterPropertiesSet方法，在初始化bean时执行该方法

### 优点

（1）spring属于低侵入式设计，代码的污染极低；

（2）spring的DI机制将对象之间的依赖关系交由框架处理，减低组件的耦合性；

（3）Spring提供了AOP技术，支持将一些通用任务，如安全、事务、日志、权限等进行集中式管理，从而提供更好的复用。

（4）spring对于主流的应用框架提供了集成支持。

### IOC

（1）**什么是IOC**：

IOC，Inversion of Control，控制反转，指将对象的控制权转移给Spring框架，由 Spring 来负责控制对象的生命周期（比如创建、销毁）和对象间的依赖关系。

最直观的表达就是，以前创建对象的时机和主动权都是由自己把控的，如果在一个对象中使用另外的对象，就必须主动通过new指令去创建依赖对象，使用完后还需要销毁（比如Connection等），对象始终会和其他接口或类耦合起来。

而 IOC 则是由专门的容器来帮忙创建对象，将所有的类都在 Spring 容器中登记，当需要某个对象时，不再需要自己主动去 new 了，只需告诉 Spring 容器，然后 Spring 就会在系统运行到适当的时机，把你想要的对象主动给你。

也就是说，对于某个具体的对象而言，以前是由自己控制它所引用对象的生命周期，而在IOC中，所有的对象都被 Spring 控制，控制对象生命周期的不再是引用它的对象，而是Spring容器，由 Spring 容器帮我们创建、查找及注入依赖对象，而引用对象只是被动的接受依赖对象，所以这叫控制反转。

（2）**什么是DI**：

IoC 的一个重点就是在程序运行时，动态的向某个对象提供它所需要的其他对象，这一点是通过DI（Dependency Injection，依赖注入）来实现的，即应用程序在运行时依赖 IoC 容器来动态注入对象所需要的外部依赖。而 Spring 的 DI 具体就是通过反射实现注入的，反射允许程序在运行的时候动态的生成对象、执行对象的方法、改变对象的属性

OOP面向对象，允许开发者定义纵向的关系，但并不适用于定义横向的关系，会导致大量代码的重复，而不利于各个模块的重用。

### AOP

![](C:\Users\jiang\Pictures\typora\AOP.png)

AOP，一般称为面向切面，作为面向对象的一种补充，用于将那些与业务无关，但却对多个对象产生影响的公共行为和逻辑，抽取并封装为一个可重用的模块，这个模块被命名为“切面”（Aspect），减少系统中的重复代码，降低了模块间的耦合度，提高系统的可维护性。可用于权限认证、日志、事务处理。

AOP实现的关键在于 代理模式，AOP代理主要分为静态代理和动态代理。静态代理的代表为AspectJ；动态代理则以Spring AOP为代表。

（1）AspectJ是静态代理，也称为编译时增强，AOP框架会在编译阶段生成AOP代理类，并将AspectJ(切面)织入到Java字节码中，运行的时候就是增强之后的AOP对象。

（2）Spring AOP使用的动态代理，所谓的动态代理就是说AOP框架不会去修改字节码，而是每次运行时在内存中临时为方法生成一个AOP对象，这个AOP对象包含了目标对象的全部方法，并且在特定的切点做了增强处理，并回调原对象的方法。

### 循环依赖

#### 单例setter注入

spring自己解决

![](C:\Users\jiang\Pictures\typora\循环依赖.jpg)

##### 三级缓存

- singletonObjects 一级缓存，用于保存实例化、注入、初始化完成的bean实例
- earlySingletonObjects 二级缓存，用于保存实例化完成的bean实例
- singletonFactories 三级缓存，用于保存bean创建工厂，以便于后面扩展有机会创建代理对象。

#### 非单例setter注入

即@prototype

无法解决，因为spring不对此类对象进行缓存

#### 构造器注入

无法解决。**抛出BeanCurrentlyInCreationException**

### Spring容器启动流程

（1）初始化Spring容器，注册内置的BeanPostProcessor的BeanDefinition到容器中：

- ① 实例化BeanFactory【DefaultListableBeanFactory】工厂，用于生成Bean对象
- ② 实例化BeanDefinitionReader注解配置读取器，用于对特定注解（如@Service、@Repository）的类进行读取转化成 BeanDefinition 对象，（BeanDefinition 是 Spring 中极其重要的一个概念，它存储了 bean 对象的所有特征信息，如是否单例，是否懒加载，factoryBeanName 等）
- ③ 实例化ClassPathBeanDefinitionScanner路径扫描器，用于对指定的包目录进行扫描查找 bean 对象

（2）将配置类的BeanDefinition注册到容器中：

（3）调用refresh()方法刷新容器：

- ① prepareRefresh()刷新前的预处理；

- ② obtainFreshBeanFactory()：获取在容器初始化时创建的BeanFactory；

- ③ prepareBeanFactory(beanFactory)：BeanFactory的预处理工作，向容器中添加一些组件；

- ④ postProcessBeanFactory(beanFactory)：子类重写该方法，可以实现在BeanFactory创建并预处理完成以后做进一步的设置；

- ⑤ invokeBeanFactoryPostProcessors(beanFactory)：在BeanFactory标准初始化之后执行BeanFactoryPostProcessor的方法，即BeanFactory的后置处理器：

- ⑥ registerBeanPostProcessors(beanFactory)：向容器中注册Bean的后置处理器BeanPostProcessor，它的主要作用是干预Spring初始化bean的流程，从而完成代理、自动注入、循环依赖等功能

- ⑦ initMessageSource()：初始化MessageSource组件，主要用于做国际化功能，消息绑定与消息解析：

- ⑧ initApplicationEventMulticaster()：初始化事件派发器，在注册监听器时会用到：

- ⑨ onRefresh()：留给子容器、子类重写这个方法，在容器刷新的时候可以自定义逻辑

- ⑩ registerListeners()：注册监听器：将容器中所有的ApplicationListener注册到事件派发器中，并派发之前步骤产生的事件：

- ⑪ finishBeanFactoryInitialization(beanFactory)：初始化所有剩下的单实例bean，核心方法是preInstantiateSingletons()，会调用getBean()方法创建对象；

- ⑫ finishRefresh()：发布BeanFactory容器刷新完成事件。

  

  

  ### BeanFactory和ApplicationContext有什么区别？

  BeanFactory和ApplicationContext是Spring的两大核心接口，都可以当做Spring的容器。

  （1）BeanFactory是Spring里面最底层的接口，是IoC的核心，定义了IoC的基本功能，包含了各种Bean的定义、加载、实例化，依赖注入和生命周期管理。

  ApplicationContext接口作为BeanFactory的子类，除了提供BeanFactory所具有的功能外，还提供了更完整的框架功能：

  - 继承MessageSource，因此支持国际化。
  - 资源文件访问，如URL和文件（ResourceLoader）。
  - 载入多个（有继承关系）上下文（即同时加载多个配置文件） ，使得每一个上下文都专注于一个特定的层次，比如应用的web层。
  - 提供在监听器中注册bean的事件。

  （2）①BeanFactroy采用的是延迟加载形式来注入Bean的，只有在使用到某个Bean时(调用getBean())，才对该Bean进行加载实例化。这样，我们就不能提前发现一些存在的Spring的配置问题。

  如果Bean的某一个属性没有注入，BeanFacotry加载后，直至第一次使用调用getBean方法才会抛出异常。

  ②ApplicationContext，它是在容器启动时，一次性创建了所有的Bean。这样，在容器启动时，我们就可以发现Spring中存在的配置错误，这样有利于检查所依赖属性是否注入。 

  ③ApplicationContext启动后预载入所有的单实例Bean，所以在运行的时候速度比较快，因为它们已经创建好了。相对于BeanFactory，ApplicationContext 唯一的不足是占用内存空间，当应用程序配置Bean较多时，程序启动较慢。

  （3）BeanFactory和ApplicationContext都支持BeanPostProcessor、BeanFactoryPostProcessor的使用，但两者之间的区别是：BeanFactory需要手动注册，而ApplicationContext则是自动注册。

  （4）BeanFactory通常以编程的方式被创建，ApplicationContext还能以声明的方式创建，如使用ContextLoader。

### Spring Bean的生命周期？

简单来说，Spring Bean的生命周期只有四个阶段：实例化 Instantiation --> 属性赋值 Populate  --> 初始化 Initialization  --> 销毁 Destruction



### 双亲委派

JVM 并不是在启动时就把所有的.class文件都加载一遍，而是程序在运行过程中用到了这个类才去加载。除了启动类加载器外，其他所有类加载器都需要继承抽象类ClassLoader，这个抽象类中定义了三个关键方法，理解清楚它们的作用和关系非常重要。

```java
public abstract class ClassLoader {



//每个类加载器都有个父加载器

private final ClassLoader parent;

public Class<?> loadClass(String name) {

//查找一下这个类是不是已经加载过了

Class<?> c = findLoadedClass(name);

//如果没有加载过

if( c == null ){

//先委派给父加载器去加载，注意这是个递归调用

if (parent != null) {

c = parent.loadClass(name);

}else {

// 如果父加载器为空，查找Bootstrap加载器是不是加载过了

c = findBootstrapClassOrNull(name);

}

}

// 如果父加载器没加载成功，调用自己的findClass去加载

if (c == null) {

c = findClass(name);

}

return c；

}

protected Class<?> findClass(String name){

//1. 根据传入的类名name，到在特定目录下去寻找类文件，把.class文件读入内存

...

//2. 调用defineClass将字节数组转成Class对象

return defineClass(buf, off, len)；

}

// 将字节码数组解析成一个Class对象，用native方法实现

protected final Class<?> defineClass(byte[] b, int off, int len){

...

}

}
```



JVM 的类加载器是分层次的，它们有父子关系，而这个关系不是继承维护，而是组合，每个类加载器都持有一个 parent字段，指向父加载器。（AppClassLoader的parent是ExtClassLoader，ExtClassLoader的parent是BootstrapClassLoader，但是ExtClassLoader的parent=null。）

defineClass方法的职责是调用 native 方法把 Java 类的字节码解析成一个 Class 对象。

findClass方法的主要职责就是找到.class文件并把.class文件读到内存得到字节码数组，然后调用 defineClass方法得到 Class 对象。子类必须实现findClass。

loadClass方法的主要职责就是实现双亲委派机制：首先检查这个类是不是已经被加载过了，如果加载过了直接返回，否则委派给父加载器加载，这是一个递归调用，一层一层向上委派，最顶层的类加载器（启动类加载器）无法加载该类时，再一层一层向下委派给子类加载器加载。

双亲委派保证类加载器，自下而上的委派，又自上而下的加载，保证每一个类在各个类加载器中都是同一个类。



一个非常明显的目的就是保证java官方的类库<JAVA_HOME>\lib和扩展类库<JAVA_HOME>\lib\ext的加载安全性，不会被开发者覆盖。



例如类java.lang.Object，它存放在rt.jar之中，无论哪个类加载器要加载这个类，最终都是委派给启动类加载器加载，因此Object类在程序的各种类加载器环境中都是同一个类。



如果开发者自己开发开源框架，也可以自定义类加载器，利用双亲委派模型，保护自己框架需要加载的类不被应用程序覆盖。



四、破坏双亲委派



如果想自定义类加载器，就需要继承ClassLoader，并重写findClass，如果想不遵循双亲委派的类加载顺序，还需要重写loadClass。如下是一个自定义的类加载器，并重写了loadClass破坏双亲委派：



### Autowired

Autowired 的意思是自动装配，它将Spring容器中的bean自动的和我们 需要这个bean的类 组装在一起，它通过反射将成员变量赋值为期望的类实例。
当 Spring 容器启动时，AutowiredAnnotationBeanPostProcessor 将扫描 Spring 容器中所有的 Bean，当发现 Bean 中拥有@Autowired 注解时就找到和其匹配（默认按类型匹配）的 Bean 并注入到对应的地方。

#### 和Resource对比：

Autowired 由spring提供，Resource 由JavaEE提供。
Autowired 按类型装配依赖对象，Resource 默认是按 name 进行装配，如果没有指定 name 属性，就按类型进行装配。（这里的 name 指的是变量名）
一般用 Resource 注解多一点，因为它不是由 spring 提供的，可以减少项目与spring 的耦合。



# **项目**



**![Graphical user interface, text  Description automatically generated](file:///C:/Users/jiang/AppData/Local/Temp/msohtmlclip1/01/clip_image064.gif)**

数据库：

Spring data jpa

\1. 创建实体类

\2. repository extend jpaRepository/jpaSpecificationExecutor

 

 

Controller:

@getMapping/ @PostMapping

接受一个dto

 

Service:

Use mapper map to 实体类，用repository中的方法操作数据库



# TO DO

磁盘及网络IO工作方式解析https://segmentfault.com/a/1190000007692223?hmsr=toutiao.io

数据从网卡到磁盘的流程https://www.csdn.net/tags/MtTaMgxsMjQzMTA5LWJsb2cO0O0O.html

 

 

 

 

 

 

 

 

 

 

#  面试记录

08/12/2021 字节

https原理

B+树与B树对比

StringBuffer 和 StringBuilder在无竞争的情况下，效率有区别吗

 

12/13/2021

Concurrent hashmap

Collection中哪些是线程安全的

Java版本



单点登录其他实现方案

jwt是对称加密还是非对称加密



sql优化

内存管理

怎么判断线程是不是出现了死循环

Arraylist的扩容

sql join和where执行顺序

 

乐观锁悲观锁应用场景

乐观锁失败怎么办

k8s



蓄水池抽样

ipv7转10位数



系统监控指标

kafka 节点宕机对生产者和消费者的影响



redis一台机器能有多少qps,多少核