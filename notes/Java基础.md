* ##### 数据类型

  * ###### 内置数据类型

    byte，short，int，long，float，double，boolean，char

    | 简单类型 | boolean | byte | char | short | Int | long | float | double | void |
    | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
    | 二进制位数 | 1 | 8 | 16 | 16 | 32 | 64 | 32 | 64 | -- |
    | 封装器类 | Boolean | Byte | Character | Short | Integer | Long | Float | Double | Void |

  封装类的value字段都是final的

  * ###### 引用数据类型

    数组，对象

  * ###### 类型转换

    低-----------------------------------------------------------------&gt;高  
    byte,short,char—&gt; int —&gt; long—&gt; float —&gt; double

    * 自动类型转换

    * 强征类型转换

  * ###### String

    String对象的value是final的  
    在Java中将String设计成不可变的是综合考虑到各种因素的结果,如内存,同步,数据结构以及安全等方面的考虑

    * StringBuffer 和 StringBuilder  
      由于String是不可变类，当涉及String的大量更改时，使用String会创建大量的对象，此时可以使用StringBuilder来进行字符串的修改  
      StringBuffer是线程安全的，StringBuilder是非线程安全，单线程先速度快于StringBuffer

* ##### 关键字

  * ###### [final](http://www.cnblogs.com/dolphin0520/p/3736238.html)

  Java中final表最终，不可变，天然线程安全

  * ###### [static](http://www.cnblogs.com/dolphin0520/p/3736238.html)

  Java中static表静态，只有一个副本

* ##### 继承

  * ###### [抽象类和接口](https://blog.csdn.net/hupoling/article/details/52447582)

    接口是"has - a "，抽象类是"is -a "  
    Java 提供和支持创建抽象类和接口。它们的实现有共同点，不同点在于：  
    1. 接口中所有的方法隐含的都是抽象的。而抽象类则可以同时包含抽象和非抽象的方法  
    2. 类可以实现很多个接口，但是只能继承一个抽象类  
    3. 类如果要实现一个接口，它必须要实现接口声明的所有方法。但是，类可以不实现抽象类声明的所有方法，当 然，在这种情况下，类也必须得声明成是抽象的  
    4. 抽象类可以在不提供接口方法实现的情况下实现接口。  
    5. Java 接口中声明的变量默认都是 final 的。抽象类可以包含非 final 的变量。  
    6. Java 接口中的成员函数默认是 public 的。抽象类的成员函数可以是 private，protected 或者是 public 。  
    7. 接口是绝对抽象的，不可以被实例化。抽象类也不可以被实例化，但是，如果它包含 main 方法的话是可以被调用的
* ##### 多态

  * ###### 重写和重载

    Overload 是重载的意思，Override 是覆盖的意思，也就是重写
* ##### 错误和异常

  Java中有Error和Exception，它们都是继承自Throwable类。  
  二者的不同之处  
  Exception：  
  可以是可被控制\(checked\) 或不可控制的\(unchecked\)。  
  表示一个由程序员导致的错误。  
  应该在应用程序级被处理。  
  Error：  
  总是不可控制的\(unchecked\)。  
  经常用来用于表示系统错误或低层资源的错误。  
  如何可能的话，应该在系统级被捕捉。

  * ###### Exception

    * ###### Checked exception

      这类异常都是Exception的子类。异常的向上抛出机制进行处理，假如子类可能产生A异 常，那么在父类中也必须throws A异常。可能导致的问题：代码效率低，耦合度过高

    * ###### Unchecked exception

      这类异常都是RuntimeException的子类，虽然RuntimeException同样也是 Exception的子类，但是它们是非凡的，它们不能通过client code来试图解决，所以称为Unchecked exception 。



