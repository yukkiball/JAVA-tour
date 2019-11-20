###  Java复习要点整理



#### 	Java基础

---

**1. JVM、JDK、JRE**

​	JVM是运行Java字节码文件（.class）的虚拟机，JVM有针对不同系统的实现，因此在不同平台下，相同字节码的运行结果相同（一次编译，处处执行）。Java程序通过编译器（javac）编译为字节码文件再经过JVM转变为机器可执行，由于字节码文件到机器码的过程是通过解释器逐行解释执行，所以速度比较慢。JVM引入了JIT编译器，对于热点代码执行编译之后，会将机器码保存，下次可直接使用。对于只执行一次的代码，解释器执行比JIT编译器要快，因此并不是所有地方都使用JIT编译器。

​	JDK是Java开发程序包，用有JRE、编译器（javac）和其他工具，能够创建和编译程序。

​	JRE是Java运行时环境，用来运行已经编译的Java程序，包括JVM、Java类库、Java命令和其他一些基础构建。



**2. Java基本类型和包装类型**

基本类型：

   * byte/8
   * char/16
   * short/16
   * int/32
   * float/32
   * long/64
   * double/64
   * boolean/1

包装类型：

- Byte
- Character
- Short
- Integer
- Float
- Long
- Double
- Boolean

自动装箱：

```java
Integer b = 1;	#执行Integer.valueOf(1);
```

自动拆箱：

```java
Integer a = new Integer(1);
int b = a;	#执行Integer.intValue()
```

不能执行隐式的向下转换（除 += 、*=、-=、\=、++）







**3. 缓存池 **

​	Integer会缓存-128~127的值，调用Integer.valueOf(123)时会从缓存池中取对象

```java
Integer x = new Integer(123);
Integer y = new Integer(123);
System.out.println(x == y);    // false
Integer z = Integer.valueOf(123);
Integer k = Integer.valueOf(123);
System.out.println(z == k);   // true
```

基本类型对应的缓冲池如下：

- boolean values true and false
- all byte values
- short values between -128 and 127
- int values between -128 and 127
- char in the range \u0000 to \u007F



**3.String、StringBuilder、StringBuffer**

​	String是不可变类型，被声明为final不可继承，内部使用char数组存储

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];
}
```

​	StringBuilder可变，线程不安全，性能比StringBuffer要好

​	StringBuffer可变，线程安全，内部用synchronized同步



​    **字符串常量池 **

​	字符串常量池用来保存所有字符串字面量，字面量在编译时已经确定，可以使用intern方法，获取常量池中对象，若常量池中字符串值存在，则返回常量池中对象引用，否则，会先创建对象，在返回该对象引用。

```java
String s1 = new String("aaa");
String s2 = new String("aaa");
System.out.println(s1 == s2);           // false
String s3 = s1.intern();
String s4 = s1.intern();
System.out.println(s3 == s4);           // true

String s5 = "bbb";
String s6 = "bbb";
System.out.println(s5 == s6);  // true
```



   **String a = new String("abc")创建几个对象**

​	若常量池中没有字面量为“abc”的字符串对象，会创建2个对象，分别是常量池中字符串对象和堆中的字符串对象。若常量中存在，则只会创建堆中的字符串对象。



**4. 继承**

**抽象类**

​	抽象类中的抽象方法用abstract修饰，抽象类不可以被实例化，抽象类是类的模板设计，is a的关系

**接口**

​	接口中方法和字段默认为public修饰（不可以用private、protected修饰），字段默认都是static final，jdk1.8中允许接口中拥有默认的方法实现（default修饰），一个类只能继承一个父类，但可以实现多个接口。接口累死与一种like a的关系，是一种行为的抽象。优先使用接口。



**重载和重写**

​	重载发生在同一个类中， 参数类型不同、个数不同、顺序不同，方法返回值和访问修饰符可以不同，发生在编译时。  

​	重写发生在父类的子类中， 参数列表必须相同，返回值范围小于等于父类，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类；如果父类方法访问修饰符为 private 则子类就不能重写该方法。 

**super**

- 调用父类构造函数

  当父类只有有参数的构造函数时，子类构造函数中必须显示调用父类的有参构造函数。

- 调用父类方法

   super.xxx()
   

**this**

* 类的当前实例

* 内部类用外部类的成员时

  外部类名.this.xxx

**static**

- **修饰成员变量和成员方法:** 被 static 修饰的成员属于类，不属于单个这个类的某个对象，被类中所有对象共享，可以并且建议通过类名调用。被static 声明的成员变量属于静态成员变量，静态变量 存放在 Java 内存区域的方法区。调用格式：`类名.静态变量名` `类名.静态方法名()`
- **静态代码块:** 静态代码块定义在类中方法外, 静态代码块在非静态代码块之前执行(静态代码块—>非静态代码块—>构造方法)。 该类不管创建多少对象，静态代码块只执行一次.
- **静态内部类（static修饰类的话只能修饰内部类）：** 静态内部类与非静态内部类之间存在一个最大的区别: 非静态内部类在编译完成之后会隐含地保存着一个引用，该引用是指向创建它的外围类，但是静态内部类却没有。没有这个引用就意味着：1. 它的创建是不需要依赖外围类的创建。2. 它不能使用任何外围类的非static成员变量和方法。
- **静态导包(用来导入类中的静态资源，1.5之后的新特性):** 格式为：`import static` 这两个关键字连用可以指定导入某个类中的指定静态资源，并且不需要使用类名调用类中静态成员，可以直接使用类中静态成员变量和成员方法。

**final**

- 修饰数据，则为编译时常量，对于基本类型，数值不变，对于引用类型，不能指向其他对象

- 修饰方法，不能被子类重写

- 修饰类，不能被继承



**5. equals()和hashCode()**

- 对于基本类型，==判断值是否相等，无equals()。

- 对于引用类型，==判断两个变量是否引用同一对象，equals()判断引用的对象是否等价（若没有重写和==一样）

- 重写equals()，必须重写hashCode()
      equals()的对象，散列值一定相等，反之不一定。
      在set中判断是否重复首先比较hashCode()，若相等再调用equals()。
  



**6. Java序列化**

将对象转换为字节序列便于存储传输，需要实现Serializable接口

   - 序列化：ObjectOutputStream.writeObject()

   - 反序列化：ObjectInputStream.readObject()

使用transient可以使一些关键字不被序列化



**7. 反射**

​	获取运行时的类信息，在运行时获取类的所有字段和方法，对象类型在编译期时是未知的，在运行时可以通过反射机制直接创建对象。





**8. 异常**

  Throwable分为两类，Error和Exception，Error表示不能被JVM处理，Exception可以被捕获并处理



**9. 注解**

​	附加在代码中的一些元信息，用于一些工具在编译、运行时进行解析和使用，起到说明、配置的功能。注解不会也不能影响代码的实际逻辑，仅仅起到辅助性的作用。 



**10. jdk8新特性**

- Lambda表达式

- Stream函数式操作流元素集合

- 接口新增：默认方法与静态方法

- 方法引用,与Lambda表达式联合使用

- 引入重复注解

- 类型注解

- 最新的Date/Time API (JSR 310)

- 新增base64加解密API

- 数组并行（parallel）操作

- JVM的PermGen空间被移除：取代它的是Metaspace（JEP 122）元空间



#### 	Java容器

---

##### Collection

**1. Set**

- TreeSet:基于红黑树实现，支持有序性，查找插入均为O(logN)

- HashjSet:基于哈希表实现，不支持有序性，丢失插入顺序，查找效率O(1)

- LinkedHashSet:具有(1)查找效率，内部使用双向链表维护插入顺序


**2.List**
- ArrayList:基于动态数组实现，支持随机访问

- Vector:与ArrayList类似，线程安全

- LinkedList:基于双向链表实现，只能顺序访问，插入删除元素快


**3.Queue**
- LinkedList: 用它做双向队列
- PriorityQueue:基于堆结构实现，优先级队列

##### Map

- HashMap:基于数组+链表（红黑树）实现
- TreeMap:基于红黑树实现
- LinkedHashMap:使用双向链表维护插入顺序
- HashTable:线程安全，使用同一把锁，应用CocurrentHashMap替代



##### 源码分析



###### List

**1.ArrayList**

- 实现了RandomAccess接口支持随机访问

- 数组的默认大小为10

- 添加元素时使用 ensureCapacityInternal() 方法来保证容量足够 ，不够时，使用grow()进行扩容，使用Arrays.copyOf()进行复制，新容量是旧容量的1.5倍。

- Fail-Fast迭代器，用modCount记录结构发生变化的次数（添加或删除元素），在迭代时需要比较操作前后的modCount是否改变，若改变抛出异常。



**2.Vector**

- 内部所有方法用synchronized同步，每次扩容容量为原来的2倍



**3.CopyOnWriteArrayList**

- 读写分离，写时复制原数组，写操作需要加锁，写入完成后指向新数组

- 读操作不能读取到实时性的数据，写操作的数据还未同步

- 内存占用大，每次写都需要复制原数组

- 适用于读多写少的场景



**4.LinkedList**

- 内部使用双向链表实现
- 不支持随机访问，插入删除元素快



###### Map

**1.HashMap**

- 内部基于数组（Node类型）+链表实现，jdk8之后当链表长度大于等于8之后会转变为红黑树

- HashMap数组长度为2的次幂，默认为16，因为桶下标为hashCode % length，若length为2的次幂则可以转变为位与运算x & (length - 1)，效率大大提升

- 当元素个数大于threshold时，需要进行扩容，新容量为旧容量的2倍，建立新数组，复制

- jdk8之前是链表是使用头插法，会造成并发插入导致扩容的情况下会造成死循环导致CPU100%，jdk8之后使用尾插法解决了这个问题
- 可以有null键


**2.CocurrentHashMap**
- jdk8之前使用分段锁，每个锁维护一部分桶，多个线程可以访问不同分段锁上的桶，默认并发级别是16， segment锁继承自Reentrantlock
- 执行size操作时会尝试不加锁，若两次不加锁得到结果一致则正确，若尝试次数超过3次，则对每个Segment加锁
- jdk8 之后使用CAS加内部synchronized（只锁定当前链表和红黑树首节点）实现

**3.HashTable**
- 内部使用数组加链表实现
- 使用的是同一把锁，效率低
- 不能插入null键
- 初始容量为11， 每次扩容2n + 1





#### Java IO

---

java IO内部使用了装饰者模式。

- InputStream 是抽象组件；
- FileInputStream 是 InputStream 的子类，属于具体组件，提供了字节流的输入操作；
- FilterInputStream 属于抽象装饰者，装饰者用于装饰组件，为组件提供额外的功能。例如 BufferedInputStream 为 FileInputStream 提供缓存的功能。

  ![img](https://camo.githubusercontent.com/d650ccc4ec1a0c99171582d9ccc9a5003155496f/68747470733a2f2f63732d6e6f7465732d313235363130393739362e636f732e61702d6775616e677a686f752e6d7971636c6f75642e636f6d2f39373039363934622d646230352d346363652d386432662d3163386230396634643932312e706e67) 

实例化一个具有缓存功能的字节流对象时，只需要在 FileInputStream 对象上再套一层 BufferedInputStream 对象即可。

```java
FileInputStream fileInputStream = new FileInputStream(filePath);
BufferedInputStream bufferedInputStream = new BufferedInputStream(fileInputStream);
```

 DataInputStream 装饰者提供了对更多数据类型进行输入的操作，比如 int、double 等基本类型。 



**BIO**

同步阻塞IO，数据的读取写入必须阻塞在一个线程内等待其完成，一请求一应答，在接收到客户端的连接请求之后为每个客户端创建一个新的线程进行链路处理，处理完成之后，通过输出流返回应答给客户端，线程销毁。采用 线程池实现，可以改善线程创建和回收的成本，有效控制线程最大数量。



**NIO**

同步非阻塞IO， 对应 java.nio 包，提供了 Channel , Selector，Buffer等抽象。  NIO提供了与传统BIO模型中的 `Socket` 和 `ServerSocket` 相对应的 `SocketChannel` 和 `ServerSocketChannel` 两种不同的套接字通道实现,两种通道都支持阻塞和非阻塞两种模式。阻塞模式使用就像传统中的支持一样，比较简单，但是性能和可靠性都不好；非阻塞模式正好与之相反。对于低负载、低并发的应用程序，可以使用同步阻塞I/O来提升开发速率和更好的维护性；对于高负载、高并发的（网络）应用，应使用 NIO 的非阻塞模式来开发。 



**NIO与BIO的区别**

- BIO是阻塞的，NIO是非阻塞的
- BIO是面向流的，NIO是面向缓冲区的（NIO读取写入都要通过Buffer）
- NIO通过Channel进行读写，Channel是双向的，而流是单向的
- NIO有选择器，IO没有，选择器用于单线程处理多个通道



**AIO**
异步非阻塞IO，基于事件和回调机制，应用不广泛







#### JVM

---

**1.Java内存区域**



##### 线程私有：

程序计数器：计数器的值用于选取下一条要执行的字节码指令

本地方法栈：为Naive方法服务

JVM栈：存放局部变量，调用方法时形成栈帧压入栈中

##### 线程共享：

堆：存放对象实例

方法区：类信息、类静态变量、常量、JIT编译后的代码，jdk8之后放入元空间（使用直接内存）

运行时常量池：在方法区中，存放Class文件中的常量池（编译器生成的字面量和符号引用）

##### 其他：

直接内存：不是运行时数据区一部分，NIO使用Native函数直接分配堆内存，避免了在Java堆和Native堆中来回复制数据



**2. 创建对象**



- 类加载检查：检查常量池中是否有该类的符号引用，检查该类是否被加载解析初始化，若没有进行类加载过程

- 分配内存：内存规整用指针碰撞，不规整用空闲列表
- 初始化零值：保证实例字段不赋值就可以直接使用（不包括对象头）
- 设置对象头：对象头中存放类的元数据信息，哈希吗、GC分代年龄、偏向锁、类型指针等
- 执行init方法：执行类的构造器方法

**3. 对象的内存布局**



- 对象头
- 实例数据
- 对齐填充
- 对象访问定位：使用句柄和直接指针

**4. Java垃圾收集**



##### 判断对象是否可以回收

- 引用计数（由于循环引用不使用）

- 可达性分析（以GC Roots为起点进行搜索，能够到达的对象是存活的）

  GC Roots:

  1. JVM栈中变量引用的对象

  2. 本地方法栈中引用的对象
  3. 方法区中静态变量引用的对象
  4. 方法区中常量引用的对象

##### 引用
1. 强引用：不会被回收
2. 软引用：若内存足够不会被回收
3. 弱引用：在下一次垃圾收集时被回收
4. 虚引用：在对象被回收时收到一个通知

对象回收前进行两次标记，第一次是可达性分析，第二是finalize方法中，若可以与引用链上对象建立关联，则存活

##### 回收方法区

1. 回收常量：没有任何对象引用常量池中字面量
2. 回收无用的类：不存在任何类的实例、Classloader被回收、Class对象没有任何引用，无法通过反射机制访问



**5.垃圾回收算法**

- 标记-清除算法：对需要回收的对象进行标记，然后进行回收，会产生碎片
- 标记-整理算法：标记后，将存活对象移动至一端，直接清除之后的内存
- 复制算法：分为eden和survivor，每次使用其中一块，将存活对象复制至另外一块，交换。现在常用eden:survivor:survivor = 8:1:1，分配担保机制保证当survivor空间不足时，直接进入老年代
- 分代收集算法：对于新生代，只有少量对象存活，采用复制算法，对于老年代，存活率高，没有额外空间担保，采用标记-清除和标记-整理算法。



**6. 垃圾收集器**

###### Serial收集器

单线程，新生代复制，老年代标记-整理，暂停所有用户线程

###### ParNew收集器

多线程，新生代复制，老年代标记-整理，暂停所有用户线程

###### Paraller Scavenge收集器

多线程，注重吞吐量（CPU用于用户程序运行的时间占总时间的比值）

###### Serial Old收集器

Serial的老年代版本，作为CMS的后备方案，在并发收集发生Cocurrent Mode Failure时使用

###### Parallel Old收集器

Paraller Scavenge的老年代版本

###### CMS收集器

- 初始标记：暂停所有线程，快速标记

- 并发标记：同时开启GC和用户线程

- 重新标记：标记那些并发标记过程中因为用户线程变动的对象，需要停顿

- 并发清理：开启用户线程和GC线程进行清理
优点：并发收集，停顿时间短
缺点：对CPU资源敏感（吞吐量低），无法处理浮动垃圾（并发清除阶段产生的垃圾，因此需要预留一部分内存，若内存，标记-清除算法会产生碎片


###### G1收集器
收集范围是整个老年代和新生代，把堆划分为大小相等的独立区域，新生代和老年代一起回收，记录每个区域垃圾回收时间和回收所获得的空间，维护一个优先列表，每次根据允许的收集时间，回收价值最大的区域
- 初始标记
- 并发标记
- 最终标记：记录在并发标记中产生变动的部分，记录在remember set logs中，合并到Remember Set中，需要停顿线程，可并行执行
- 筛选回收：对各Region回收成本和价值排序，根据用户期望GC停顿时间制定回收计划
空间整合：整体上看是标记-清理，局部来看是复制算法，运行期间不产生碎片
可预测的停顿：能让使用者明确指定一个长度为M毫秒的时间片段内，消耗在GC上的时间不超过N秒。



**7. 内存分配策略**

- 对象有现在eden分配
- 大对象直接进入老年代
- 长期存活的对象进入老年代
- 当相同年龄的对象大于survivor空间一半，大于等于该年龄的进入老年代
- 空间分配担保



**8. Full GC的触发条件**
- System.gc()
- 老年代空间不足
- Cocurrent Mode Failure
- 空间担保失败



**9.常用的JDK命令行工具**

- jps：查看Java进程
- jstat：监视运行时信息
- jinfo：实时查看虚拟机参数
- jmap：生成堆存储快照
- jhat：分析heapdump文件
- jstack：生成当前时刻的线程快照

**10.类加载机制**
###### 类加载过程
- 加载：获取二进制字节流、将静态存储结构转变为方法区运行时数据结构，内存中生成Class对象，作为访问入口
- 验证：验证该二进制字节流是否符合虚拟机标准
- 准备：类变量分配内存并初始化为零值（常量直接为该值）
- 解析：将常量池中符号引用转变为直接饮用
- 初始化：执行clinit，类变量赋予初始值，执行静态语句块

执行初始化的情况：
- 遇到new，读取（修改）静态字段、调用静态方法
- 反射调用时
- 子类初始化时，先初始化父类
- 虚拟机启动时，初始化主类
- jdk1.7，如果一个MethodHandle实例解析后的句柄未初始化，则需要先触发初始化

**11.类加载器**
- 启动类加载器：加载lib下的jar包
- 扩展类加载器：加载lib/ext下的jar包和类
- 应用类加载器：加载classpath下的jar包和类

###### 双亲委托模型
当进行类加载时，先委托父类加载器进行类加载，若父类加载无法进行类加载，再自己进行类加载

优点：
使得类具有类加载器的优先级的层次结构，基础类得到统一，避免类的重复加载，保证核心API不被篡改

破坏：

自定义类加载器，重写loadclass()方法