# demo2.github.io
个人博客

    String为啥不可变	
  1.字符串常量池的需要
(1)当创建一个String对象时，假如此字符串值已经存在于常量池中，则不会创建一个新的对象，而是引用已经存在的对象
(2)假如说字符串允许改变的话，那么就会有各种逻辑错误，比如改变一个对象会影响到另一个独立的对象，所以从严格的角度来说，这种常量池的思想，它是一种优化手段
  2.允许String对象缓存HashCode
String不可变保证了Hash码的唯一性，就可以放心的进行缓存，意味着不用每次都去计算新的哈希码。
  3.安全性
String被许多的java类用来当参数，比如链接地址url，文件路径path，还有反射机制所需要的String参
数，假如String是可变的，就会引起很多隐患。

    String不可变有什么好处?
因为字符串是不可变的，所以是多线程安全的，同一个字符串实例可以被多个线程共享。
类加载器要用到字符串，不可变性提供了安全性，确保正确的类被加载。譬如你想加载java.sql.Connection类，但这个值被改成了myhacked.Connection，那么会对你的数据库造成不可知的破坏。

    String的底层原理
String是被final修饰的，然后还有个成员属性value的 并且是一个char类型的数组
This.value = original.value; 通过构造方法知道，传递的参数是直接复制给了value，就说明String是一个不可变的字符串，底层是一个char类型的数组。

    HashMap的底层原理
1.7的时候是数组+链表组成的。当创建hashmap的时候首先会先创建一个数组，当用put方法存储数据的时候，会先根据key的hashcode值计算出这个Hash值，然后用这个hash值计算出位置，把value值放进去，如果这个位置上没有数据的话，就会直接存入，如果有数据的话，就会在这个位置上建立一个链表，把它放入到这个链表的头部。当我们用get方法取值的时候，也是根据key的hashcode值计算出这个hash值，然后在根据equals方法从这个位置上把value取出来，当容量超过当前容量的0.75倍之后，就会自动扩容到原来容量的两倍，这个0.75就是负载因子。
1.8是数组+链表+红黑树。因为1.7的时候，链表的长度不固定，当key的hashcode值重复之后，对应链表的数据长度就没有办法掌控了。Get数据的时间复杂度就取决于链表的长度，为了提高这一部分的性能，加入了红黑树，如果链表长度超过8位以后，就将链表转为红黑树，极大的降低了时间复杂度



    Hashmap和hashtable的区别
相同点：两者都是键值对的双列集合
底层都是通过数组＋链表的方式实现数据存储
不同点：继承的类不同
Hashtable继承Dictionary类，而hashmap继承abstractmap类，但是两者都是现实了map接口

    线程安全性不同
Hashtable中的方法是synchronize的，而hashmap中的方法在缺省的情况下是非synchronize的，在多线程并发的环境下，可以直接使用hashtable，不需要自己为他的方法实现同步，但使用hashmap的话就需要自己增加同步处理

Hashmap允许null键和null值，只能有一个。但是hashtable不允许。
Hashmap是java开发的常用类，但是hashtable和vector一样，成为了废弃类
Hashmap是不安全的，如果想安全怎么办，是如何保证的安全的
使用concurrentHashMap（肯可润t）。它是内部采用了分段锁的策略，它的主干是个segment数组。Segment是通过继承ReentrantLock(润ten)来进行加锁，所以每次需要加锁的操作锁住的是一个segment，这样的话只要保证每个segment是线程安全的，也就实现了全局的线程安全。一个segment就是一个子哈希表，segment里维护了一个HashEntry数组。默认有16个segment，所以理论上，最多支持16个线程并发写，只要它们的操作分布在不同的segment上

    什么泛型
泛型就是一种程序语言的一种特性。允许程序员在强类型程序设计语言中编写代码时定义一些可变的部分，那些部分在使用前边必须做出指明。是将类型参数化以达到代码重复以提高软件开发工作的一种类型语言。泛型类是引用类型，是堆对象，主要是引入了类型参数这个概念。

    泛型的好处
1.不会强行对值类型进行装箱和拆箱，或者对引用类型进行向下强转，所以性能会提高。
2.泛型能提高程序的类型安全
3.它为开发者提供了一种高性能的编程方式，提高了代码的重用性

    定义分类
主要有两种：
1.在程序编程中一些包含类型参数的类型，也就是说泛型的参数只可以代表类，不能代表个别对象。
2.在程序编码中一些包含参数的类。其参数可以代表类或对象等。不管是哪个定义，泛型的参数在真正使用的时候都必须做出指明。

    类型擦除以及使用泛型擦除引起的问题
类型擦除：java泛型都是在编译器上实现的，在生成的字节码中是不包括泛型中的类型信息的，使用泛型的时候加上类型参数，在编译器编译的时候去掉，这就是类型擦除。



协变、不变、逆变 是c语言里面的概念 但是不是某一个语言独有的 是所有语言都有的

Java的泛型默认就是不变的，不管是设置long、int，不管啥类型，最后都是Object，所以java泛型是不变的。

协变：就比如 设置一个数组String 里面只能是字符串类型，比如int 只能是int类型，不会有其他类型。为了实现协变 ，用的通配符 一颗ten zi，
不变就是 比方说java泛型就是不变的，就是刚刚举得哪个例子 不管设置成什么类型 编译之后都是Object类型，所以说泛型是不变的
