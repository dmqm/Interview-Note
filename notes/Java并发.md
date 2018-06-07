* ##### 底层原理

  * ###### volatile

    volatile是轻量级的synchronized，保证了共享变量的可见性  
    volatile执行写操作的时候会多出第二行带有lock前缀的汇编代码，将处理器缓存数据写入系统内存

    * Lock前缀指令会引起处理器缓存回写到内存

    * 一个处理器的缓存回写到内存会导致其他处理器的缓存无效

  * ###### synchronized

    Java 1.6中引入偏向锁和轻量级锁，以及锁的存储结构和升级过程  
    JVM基于进入和推出Monitor对象来实现方法同步和代码块同步

    * [Java对象头![](/assets/Java-object-header.png)](https://blog.csdn.net/zhoufanyang_china/article/details/54601311)对象在内存中存储的布局可以分为三块区域：对象头（Header）、实例数据（Instance Data）和对齐填充（Padding）  
      HotSpot虚拟机的对象头\(Object Header\)包括两部分信息，第一部分用于存储对象自身的运行时数据， 如哈希码（HashCode）、GC分代年龄、锁状态标志、线程持有的锁、偏向线程ID、偏向时间戳等等，这部分数据的长度在32位和64位的虚拟机（暂 不考虑开启压缩指针的场景）中分别为32个和64个Bits，官方称它为“Mark Word”

    * ###### 锁的升级和对比

      锁的状态从低到高：无锁，偏向锁，轻量级锁，重量级锁

  * ###### 原子操作
* ##### 内存模型

  * ###### 重排序
  * ###### 顺序一致性
  * ###### happens-before
* ##### 并发编程基础

  * ###### 线程

    ![](/assets/java-thread-states.png)  
     Java线程一共有七个状态，分别是新建，可运行，运行中，睡眠，阻塞，等待，死亡

  * ###### 线程间通信

    * ###### volatile

      每次从共享内存读取，保证所有进程对该变量的可见性

    * ###### synchronized

      同步，同一个时刻，只有一个进程处于方法或者同步块中  
      对象监视器的排他性使得同一时刻只有一个对象可以获得这个对象的监视器

    * ###### 等待/通知

      object对象的notify\(\)和wait\(\)方法，通过某个对象来完成交互  
      wait\(\)会释放锁

      * ###### 等待方

        获取对象的锁  
         如果条件不满足，那么调用锁的wait\(\)方法，使该线程进入waiting，被通知后依然要检查条件  
         条件满足则执行对应的逻辑

      * ###### 通知方

        获得对象的锁  
        改变条件  
        通知索引等待在对象上的进程

    * ###### 管道输入/输出流
    * ###### thread.join\(\)

      当前进程等待thread线程终止之后才从thread.join\(\)返回

    * ###### ThreadLocal

      线程变量，以ThreadLocal为键、任意对象为值的存储结构
  * ###### 等待超时模式

    在等待通知基础上添加超时控制，针对昂贵资源的一种自我保护机制

    * 数据库连接池

    * 线程池
* ##### Java中的锁

  * ###### Lock接口
  * ###### 队列同步器（AQS）

    FIFO同步队列  
    超时，中断

  * ###### 重入锁（ReentrantLock）

    通过组合自定义同步器来实现锁的获取与释放

  * ###### 读写锁

    分离出一个读锁一个写锁

  * ###### Condition接口

    依赖Lock对象，等待队列（AQS）
* ##### 并发容器和框架

  * ###### ConcurrentHashMap
  * ###### ConcurrentLinkedQueue
  * ###### 阻塞队列

    * ###### ArrayBlockingQueue
    * ###### LinkedBlockingQueue
    * ###### PriorityBlockingQueue

      优先级排序，无界

    * ###### DelayQueue

      PriorityBlockingQueue实现，延时

    * ###### SynchronousQueue

      不存储元素，每个put操作要等待一个take操作

    * ###### LinkedTransferQueue

      无界

    * ###### LinkedBlockingDeque

      无界，双向
  * ###### Fork/Join框架
* ##### 原子操作类

  * ###### 原子更新基本类型类

    AtomicBoolean，AtomicInteger，AtomicLong，Unsafe类的CAS方法实现

  * ###### 原子更新数组

    AtomicIntegerArray，AtomicLongArray，AtomicReferenceArray
* ##### 并发工具类

  * ###### CountDownLatch

    等待多线程完成，直到计数器为0

  * ###### CyclicBarrier

    同步屏障，可用于多线程计算器最后合并结果

  * ###### Semaphore

    信号量，acquire\(\),release\(\)

  * ###### Exchanger

    交换者，线程间交换数据

  * ##### 线程池

    降低资源消耗，提高响应速度，提高线程的可管理性

    * ###### 策略

      如果当前运行进程少于corePoolSize，创建新线程执行任务  
      如果运行的线程等于或多于corePoolSize，将任务加入BlockingQueue  
      如果无法将任务加入BlockingQueue（队列满），创建新线程执行 任务  
      如果创建新线程将使当前运行的线程超过maximumPoolSize，任务将被拒绝  
      注意创建新线程需获取全局锁  
      ![](/assets/threadpoolexecutor-policy.png)

    * ###### 线程池的使用

      new ThreadPoolExecutor\(corePoolSize,maximumPoolSize,keepAliveTime,millseconds,runnableTaskQueue,handle\)

      * ###### execute\(\)，执行Runnable实例
      * ###### submit\(\)，返回future对象

  * ##### Executor框架

    工作单元Runnable，Callable，执行机制Executor  
    ![](/assets/executor-arch.png)

  * ###### ThreadPoolExector

    SingleThreadExecutor，单个线程  
    FixedThreadExecutor，固定线程数  
    CachedThreadExecutor，根据需要创建新线程

  * ###### ScheduledThreadPoolExecutor

    ScheduledThreadPoolExecutor，若干线程  
    SingleThreadPoolExecutor，一个线程

  * ###### Future

    接口Future和实现类FutureTask来表示异步计算的结果



