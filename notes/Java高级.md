* ##### 泛型

  开发人员在使用泛型的时候，很容易根据自己的直觉而犯一些错误。比如一个方法如果接收 List&lt;Object&gt; 作为形式 参数，那么如果尝试将一个 List&lt;String&gt; 的对象作为实际参数传进去，却发现无法通过编译。虽然从直觉上来 说， Object 是 String 的父类，这种类型转换应该是合理的。但是实际上这会产生隐含的类型转换问题，因此编译 器直接就禁止这样的行为。

  * ###### 类型擦除

    Java中的泛型基本上都是在编译器这个层次来实现的，在生成的Java字节代码中是不包含泛型中的类型信息的。使用泛型的时候加上的类型参数，会被编译器在编译的时候去掉，这个过程就称为类型擦除。如在代码中定义的 List&lt;Object&gt; 和 List&lt;String&gt; 等类型，在编译之后都会变成 List 。JVM看到的只是List，而由泛型附加的类型信息对JVM来说是不可见的。Java编译器会在编译时尽可能的发现可能出错的地方，但是仍然无法避免在运行时刻出现类型转换异常的情况。  
    很多泛型的奇怪特性都与这个类型擦除的存在有关，包括：

    1. 泛型类并没有自己独有的Class类对象。比如并不存在 List&lt;String&gt;.class 或是 List&lt;Integer&gt;.class ，而只有 List.class

    2. 静态变量是被泛型类的所有实例所共享的。对于声明为 MyClass&lt;T&gt; 的类，访问其中的静态变量的方法仍然是 MyClass.myStaticVar 。不管是通过 new MyClass&lt;String&gt; 还是 new MyClass&lt;Integer&gt; 创建的对象，都是共享一个静 态变量

    3. 泛型的类型参数不能用在Java异常处理的catch语句中。因为异常处理是由 JVM 在运行时刻来进行的。由于 类型信息被擦除， JVM 是无法区分两个异常类型 MyException&lt;String&gt; 和 MyException&lt;Integer&gt; 的。对于 JVM 来 说，它们都是  MyException 类型的。也就无法执行与异常对应的catch语句

    类型擦除的基本过程也比较简单，首先是找到用来替换类型参数的具体类。这个具体类一般是Object。如果指定了 类型参数的上界的话，则使用这个上界。把代码中的类型参数都替换成具体的类。同时去掉出现的类型声明，即去掉&lt;&gt;的内容。比如 T get\(\) 方法声明就变成了 Object get\(\) ； List&lt;String&gt; 就变成了 List 。接下来就可能需要生成 一些桥接方法（bridge method）。这是由于擦除了类型之后的类可能缺少某些必须的方法。比如考虑下面的代码：

    `class MyString implements Comparable<String> {      public int compareTo(String str) {                  return 0;          }  }`  
    当类型信息被擦除之后，上述类的声明变成了 class MyString implements Comparable 。但是这样的话，类 MyString 就 会有编译错误，因为没有实现接口 Comparable 声明的 int compareTo\(Object\) 方法。这个时候就由编译器来动态生成 这个方法。

  * ###### 通配符

    在使用泛型类的时候，既可以指定一个具体的类型，如 List&lt;String&gt; 就声明了具体的类型是 String ；也可以用通配 符 ? 来表示未知类型，如 List&lt;?&gt; 就声明了 List 中包含的元素类型是未知的。 通配符所代表的其实是一组类型， 但具体的类型是未知的。 List&lt;?&gt; 所声明的就是所有类型都是可以的。但是 List&lt;?&gt; 并不等同  
    于 List&lt;Object&gt; 。 List&lt;Object&gt; 实际上确定了 List 中包含的是 Object 及其子类，在使用的时候都可以通 过 Object 来进行引用。而 List&lt;?&gt; 则其中所包含的元素类型是不确定。其中可能包含的是 String ，也可能是 Integer 。如果它包含了 String 的话，往里面添加 Integer 类型的元素就是错误的。正因为类型未知，就不能通过 new ArrayList&lt;?&gt;\(\)的方法来创建一个新的ArrayList对象。因为编译器无法知道具体的类型是什么。但是对于 List&lt;?&gt;中的元素确总是可以用Object来引用的，因为虽然类型未知，但肯定是Object及其子类。考虑下面的代 码：

    `public void wildcard(List<?> list) {    list.add(1);//编译错误 }`

    如上所示，试图对一个带通配符的泛型类进行操作的时候，总是会出现编译错误。其原因在于通配符所表示 的类型是未知的。  
    因为对于 List&lt;?&gt; 中的元素只能用 Object 来引用，在有些情况下不是很方便。在这些情况下，可以使用上下界来限 制未知类型的范围。 如  List&lt;? extends Number&gt; 说明List中可能包含的元素类型是 Number 及其子类。而 List&lt;? super Number&gt; 则说明List中包含的是Number及其父类。当引入了上界之后，在使用类型的时候就可以使用上界类 中定义的方法。

  * ###### 类型系统

    在Java中，大家比较熟悉的是通过继承机制而产生的类型体系结构。比如 String 继承自 Object 。根据 Liskov替换原 则 ，子类是可以替换父类的。当需要 Object 类的引用的时候，如果传入一个 String 对象是没有任何问题的。但是 反过来的话，即用父类的引用替换子类引用的时候，就需要进行强制类型转换。编译器并不能保证运行时刻这种转 换一定是合法的。这种自动的子类替换父类的类型转换机制，对于数组也是适用的。 String\[\]可以替换 Object\[\]。但是泛型的引入，对于这个类型系统产生了一定的影响。正如前面提到的List是不能替换掉List的。

    引入泛型之后的类型系统增加了两个维度：一个是类型参数自身的继承体系结构，另外一个是泛型类或接口自身 的继承体系结构。第一个指的是对于  List&lt;String&gt; 和 List&lt;Object&gt; 这样的情况，类型参数 String 是继承 自 Object 的。而第二种指的是  List 接口继承自 Collection 接口。对于这个类型系统，有如下的一些规则：

    1. 相同类型参数的泛型类的关系取决于泛型类自身的继承体系结构。即 List&lt;String&gt; 是 Collection&lt;String&gt;  的子 类型， List&lt;String&gt; 可以替换 Collection&lt;String&gt; 。这种情况也适用于带有上下界的类型声明。

    2. 当泛型类的类型声明中使用了通配符的时候，其子类型可以在两个维度上分别展开。如对 Collection&lt;? extends Number&gt; 来说，其子类型可以在 Collection 这个维度上展开，即 List&lt;? extends Number&gt; 和 Set&lt;? extends Number&gt; 等；也可以在 Number 这个层次上展开，即 Collection&lt;Double&gt; 和 Collection&lt;Integer&gt; 等。如此循环下 去， ArrayList&lt;Long&gt; 和  HashSet&lt;Double&gt; 等也都算是 Collection&lt;? extends Number&gt; 的子类型。

    3. 如果泛型类中包含多个类型参数，则对于每个类型参数分别应用上面的规则。

* ##### Java集合

  Java集合框架提供了数据持有对象的方式，提供了对数据集合的操作。Java集合框架位于 java.util 包下，主要有 三个大类： Collection 、 Map 接口以及对集合进行操作的工具类

  * ###### Collection

    ![](/assets/java-collection-arch.png)

    * ArrayList ：线程不同步。默认初始容量为10，当数组大小不足时增长率为当前长度的 50% 。

    * Vector ：线程同步。默认初始容量为10，当数组大小不足时增长率为当前长度的 100% 。它的同步是通 过 Iterator 方法加 synchronized 实现的。

    * LinkedList ：线程不同步。双端队列形式。

    * Stack ：线程同步。继承自 Vector ，添加了几个方法来完成栈的功能。

    * Set ：Set是一种不包含重复元素的Collection，Set最多只有一个null元素。

    * HashSet ：线程不同步，内部使用 HashMap 进行数据存储，提供的方法基本都是调用 HashMap 的方法，所以两者 本质是一样的。集合元素可以为 NULL 。

    * NavigableSet ：添加了搜索功能，可以对给定元素进行搜索：小于、小于等于、大于、大于等于，放回一个符 合条件的最接近给定元素的 key。

    * TreeSet ：线程不同步，内部使用 NavigableMap 操作。默认元素“自然顺序”排列，可以通过 Comparator 改变排 序。

    * EnumSet ：线程不同步。内部使用Enum数组实现，速度比 HashSet 快。只能存储在构造函数传入的枚举类的 枚举值。

  * ###### Map

    ![](/assets/java-map-arch.png)

    * HashMap ：线程不同步。根据 key 的 hashcode 进行存储，内部使用静态内部类 Node 的数组进行存储，默认初 始大小为16，每次扩大一倍。当发生Hash冲突时，采用拉链法（链表）。可以接受为null的键值\(key\)和值 \(value\)。JDK 1.8中：当单个桶中元素个数大于等于8时，链表实现改为红黑树实现；当元素个数小于6时，变 回链表实现。由此来防止hashCode攻击。

    * LinkedHashMap ：保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的. 也可以在构造时用带参数，按照应用次数排序。在遍历的时候会比HashMap慢，不过有种情况例外，当 HashMap容量很大，实际数据较少时，遍历起来可能会比LinkedHashMap慢，因为LinkedHashMap的遍历速 度只和实际数据有关，和容量无关，而HashMap的遍历速度和他的容量有关。

    * TreeMap ：线程不同步，基于 红黑树 （Red-Black tree）的NavigableMap 实现，能够把它保存的记录根据键 排序,默认是按键值的升序排序，也可以指定排序的比较器，当用Iterator 遍历TreeMap时，得到的记录是排 过序的。

    * HashTable ：线程安全，HashMap的迭代器\(Iterator\)是 fail-fast 迭代器。HashTable不能存储NULL的key和 value。

  * ###### 工具类

    * Collections 、 Arrays ：集合类的一个工具类\/帮助类，其中提供了一系列静态方法，用于对集合中元素进行 排序、搜索以及线程安全等各种操作。

    * Comparable 、 Comparator ：一般是用于对象的比较来实现排序，两者略有区别。

      > 类设计者没有考虑到比较问题而没有实现Comparable接口。这时我们就可以通过使用 Comparator，这种情况下，我们是不需要改变对象的。  
      > 一个集合中，我们可能需要有多重的排序标准，这时候如果使用Comparable就有些捉襟见肘了， 可以自己继承Comparator提供多种标准的比较器进行排序。

* ##### Java反射

  [https://blog.csdn.net/sinat\_38259539/article/details/71799078](https://blog.csdn.net/sinat_38259539/article/details/71799078)

* ##### Java NIO

  [https://www.cnblogs.com/geason/p/5774096.html](https://www.cnblogs.com/geason/p/5774096.html)



