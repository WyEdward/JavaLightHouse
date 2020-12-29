[TOC]

#### 1、== 和 equals的区别

==的作用的判断两个对象的地址是不是相等。即判断两个对象是不是同一个对象。(**基本数据类型==比较的是值，引用数据类型==比较的是内存地址 **)

equals()的作用也是判断两个对象是否相等，**Object对象的底层默认equals实现是 使用了==** 也就是说如果子类不重写equals 默认比较效果是等同于==的，如果子类重写了equals 就会按照自定义的方法进行比较 比如子类覆盖equals方法来比较两个对象的内容相等的话就返回true。

#### 2、String的equals方法

String类中的equals方法是重写过的，比较的是两个对象的内容。

当创建String类型的对象时，虚拟机会在**常量池 **中查找有没有已经存在的值和要创建的值相同的对象，如果有就把它赋给当前引用。如果没有就在常量池中重新创建一个 String 对象 

#### 3、HashCode介绍

hashcode 的作用是获取哈希码，也称为散列码；它实际上是返回一个 int 整数。这个哈希码的作用是确定该对象在哈希表中的索引位置。 hashcode是定义在JDK的object类中的，object的hashcode方法是本地方法，也就是用 c 语言或 c++ 实现的，该方法通常用来将对象的 内存地址 转换为整数之后返回。 

~~~
public native int hashCode();
~~~

散列表存储的是键值对(key-value)，它的特点是：能根据“键”快速的检索出对应的“值”。这其中就利用到了散列码！（可以快速找到所需要的对象） 

**为什么要用hashcode **

当你把对象加入 `HashSet` 时，`HashSet` 会先计算对象的 hashcode 值来判断对象加入的位置，同时也会与其他已经加入的对象的 hashcode 值作比较，如果没有相符的 hashcode，`HashSet` 会假设对象没有重复出现。但是如果发现有相同 hashcode 值的对象，这时会调用 equals（）方法来检查 hashcode 相等的对象是否真的相同。如果两者相同，`HashSet` 就不会让其加入操作成功。如果不同的话，就会重新散列到其他位置。 

**为什么重写equals的时候要重写hashcode方法 **

因为两个对象比较的时候 首先比较hashcode 然后在默认比较equals判断这两个对象是否真的相等。
如果两个对象相等，hashcode都是相同的，两个对象调用equals方法也会返回true。
但是事实是两个对象可能有相同的hashcode，但是不相等的(这叫hash冲突) 
如果重写了equals  则hashcode也必须被重写 equals假如比较内容 假如两个对象的hashcode是一样的，而且比较内容的时候也是true 那么系统就会误判两个对象是一样的，所以我们必须要重写hashcode

>hascode的默认行为是对堆上的对象产生独特值。
>如果没有重写 hashcode，则该 class 的两个对象无论如何都不会相等（即使这两个对象指向相同的数据） 

为什么两个对象有相同的hashcode值，它们也不一定是相等的

因为 hashcode 所使用的杂凑算法也许刚好会让多个对象传回相同的杂凑值。越糟糕的杂凑算法越容易碰撞，但这也与数据值域分布的特性有关（所谓碰撞也就是指的是不同的对象得到相同的 `hashCode`。

我们刚刚也提到了 hashSet,如果  hashSet 在对比的时候，同样的 hashcode 有多个对象，它会使用 `equals()` 来判断是否真的相同。也就是说  hashSet 只是用来缩小查找成本。

#### 4、Java基本数据类型

Java**中**有8种基本数据类型，分别为：

1. 6种数字类型 ：byte、short、int、long、float、double
2. 1种字符类型：char
3. 1种布尔型：boolean。

这八种基本类型都有对应的包装类分别为：Byte、Short、Integer、Long、Float、Double、Character、Boolean 

| 基本类型 | 位数 | 字节 | 默认值  |
| -------- | ---- | ---- | ------- |
| int      | 32   | 4    | 0       |
| short    | 16   | 2    | 0       |
| long     | 64   | 8    | 0L      |
| byte     | 8    | 1    | 0       |
| char     | 16   | 2    | 'u0000' |
| float    | 32   | 4    | 0f      |
| double   | 64   | 8    | 0d      |
| boolean  | 1    |      | false   |

#### 5、自动装箱与拆箱

装箱：将基本类型用它们对应得引用类型包装起来
拆箱：将包装类型转换未基本数据类型

#### 6、Java的值传递

按值调用(call by value)表示方法接收的是调用者提供的值，而按引用调用（call by reference)表示方法接收的是调用者提供的变量地址。一个方法可以修改传递引用所对应的变量值，而不能修改传递值调用所对应的变量值。

Java 程序设计语言总是采用按值调用。也就是说，方法得到的是所有参数值的一个拷贝，也就是说，方法不能修改传递给它的任何参数变量的内容。

#### 7、重载与重写的区别

> 重载就是同样的一个方法能够根据输入数据的不同，做出不同的处理(比如 多个构造函数 多个参数不同同名的方法)
> 发生在同一个类中，方法名必须相同，参数类型不同、个数不同、顺序不同，方法返回值和访问修饰符可以不同。 

> 重写就是当子类继承自父类的相同方法，输入数据一样，但要做出有别于父类的响应时，你就要覆盖父类方法(子类覆盖父类的方法)
>
> 1. 返回值类型、方法名、参数列表必须相同，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类。
> 2. 如果父类方法访问修饰符为 `private/final/static` 则子类就不能重写该方法，但是被 static 修饰的方法能够被再次声明。
> 3. 构造方法无法被重写

#### 8、深拷贝与浅拷贝

1. **浅拷贝**：对基本数据类型进行值传递，对引用数据类型进行引用传递般的拷贝，此为浅拷贝。
2. **深拷贝**：对基本数据类型进行值传递，对引用数据类型，创建一个新的对象，并复制其内容，此为深拷贝。

![deep and shallow copy](https://camo.githubusercontent.com/d6d8355e9c0cbde0bdf8a53d34b0e2b46bbaa5e7/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d372f6a6176612d646565702d616e642d7368616c6c6f772d636f70792e6a7067) 

#### 9、成员变量与局部变量的区别

1. 从语法形式上看:成员变量是属于类的，而局部变量是在方法中定义的变量或是方法的参数；成员变量可以被 public,private,static 等修饰符所修饰，而局部变量不能被访问控制修饰符及 static 所修饰；但是，成员变量和局部变量都能被 final 所修饰。
2. 从变量在内存中的存储方式来看:如果成员变量是使用`static`修饰的，那么这个成员变量是属于类的，如果没有使用`static`修饰，这个成员变量是属于实例的。而对象存在于堆内存，局部变量则存在于栈内存。
3. 从变量在内存中的生存时间上看:成员变量是对象的一部分，它随着对象的创建而存在，而局部变量随着方法的调用而自动消失。
4. 成员变量如果没有被赋初值:则会自动以类型的默认值而赋值（一种情况例外:被 final 修饰的成员变量也必须显式地赋值），而局部变量则不会自动赋值。

#### 10、面向对象三大特性

1、封装性 2、继承性 3、多态性

#### 11、接口和抽象类的区别

接口(Interface)就是特殊的抽象类，只有方法和常量 
抽象类就有抽象方法的类 抽象方法是abstact修饰。可以有
如果普通的类要继承(extends)抽象类 就要重写抽象类中的所有抽象方法   **类是单继承 **
如果普通的类要继承(implements)接口 就要重写接口中的所有抽象方法   **接口可以多实现**

~~~
public interface 接口名称 {
	// 抽象方法
	// 默认方法
	// 静态方法
	// 私有方法
}
抽象方法：使用 abstract 关键字修饰，可以省略，没有方法体。该方法供子类实现使用。
默认方法：使用 default 修饰，不可省略，供子类调用或者子类重写。
静态方法：使用 static 修饰，供接口直接调用。
私有方法：使用 private 修饰，供接口中的默认方法或者静态方法调用。
public interface InterFaceName {
	public abstract void method();
	public default void method() {
    	// 执行语句
    }
    public static void method2() {
    	// 执行语句
    }
    private void method() {
    	// 执行语句
    }
}
~~~

#### 12、引用类型转换

多态的转型分为向上转型与向下转型两种 
向上转型：多态本身是子类类型向父类类型向上转换的过程，这个过程是默认的。 

~~~
父类类型 变量名 = new 子类类型();
~~~

向下转型：父类类型向子类类型向下转换的过程，这个过程是强制的。 
一个已经向上转型的子类对象，将父类引用转为子类引用，可以使用强制类型转换的格式，便是向下转型。 

~~~
子类类型 变量名 = (子类类型) 父类变量名
~~~



#### 13、super和this

**父类空间优先于子类对象产生 **
在每次创建子类对象时，先初始化父类空间，再创建其子类对象本身。目的在于子类对象中包含了其对应的父类空 间，便可以包含其父类的成员，如果父类成员非private修饰，则子类可以随意使用父类成员。代码体现在子类的构 造方法调用时，一定先调用父类的构造方法。理解图解如下： 

super:  代表父类的存储空间标识(可以理解为父亲的引用)。 
this: 代表当前对象的引用(谁调用就代表谁)。 

~~~
this.成员变量 ‐‐ 本类的
super.成员变量 ‐‐ 父类的
this.成员方法名() ‐‐ 本类的
super.成员方法名() ‐‐ 父类的

this(...) ‐‐ 本类的构造方法
super(...) ‐‐ 父类的构造方法
~~~

子类的每个构造方法中均有默认的super()，调用父类的空参构造。
手动调用父类构造会覆盖默认的super()。 **super() 和 this() 都必须是在构造方法的第一行，所以不能同时出现。 **

#### 14、抽象类

**抽象类 即 含有抽象方法的类 **

1. 抽象类不能创建对象，如果创建，编译无法通过而报错。只能创建其非抽象子类的对象。 

   > 理解：假设创建了抽象类的对象，调用抽象的方法，而抽象方法没有具体的方法体，没有意义 

2. 抽象类中，可以有构造方法，是供子类创建对象时，初始化父类成员使用的。 

   > 理解：子类的构造方法中，有默认的super()，需要访问父类构造方法。 

3. 抽象类中，不一定包含抽象方法，但是有抽象方法的类必定是抽象类。 

   > 理解：未包含抽象方法的抽象类，目的就是不想让调用者创建该类对象，通常用于某些特殊的类结构设 计。 

4. 抽象类的子类，必须重写抽象父类中所有的抽象方法，否则，编译无法通过而报错。除非该子类也是抽象 类。 

   > 理解：假设不重写所有抽象方法，则类中可能包含抽象方法。那么创建对象后，调用抽象的方法，没有 意义。 

#### 15、static关键词

关于 static 关键字的使用，它可以用来修饰的成员变量和成员方法，被修饰的成员是属于类的，而不是单单是属 于某个对象的。也就是说，既然属于类，就可以不靠创建对象来调用了。 

**类变量** 

使用 static关键字修饰的成员变量。 

该类的每个对象都共享同一个类变量的值。任何对象都可以更改 该类变量的值，但也可以在不创建该类的对象的情况下对类变量进行操作。 

~~~
static 数据类型 变量名；
~~~

**静态方法 **

使用 static关键字修饰的成员方法，习惯称为静态方法。 

~~~
修饰符 static 返回值类型 方法名 (参数列表){
  	// 执行语句
}
~~~

**静态方法调用的注意事项： **

1、静态方法可以直接访问类变量和静态方法。
2、静态方法不能直接访问普通成员变量或成员方法。反之，成员方法可以直接访问类变量或静态方法。
3、静态方法中，不能使用this关键字。 因为静态方法是在类加载的时候创建的  不是在创建对象的时候初始化的对象还没有出来呢

**访问方式**

static.成员  staic.方法名

**原理 **

static 修饰的内容： 

1、是随着类的加载而加载的，且只加载一次。 
2、存储于一块固定的内存区域（静态区），所以，可以直接被类名调用。 
3、它优先于对象存在，所以，可以被所有对象共享。 

**静态代码块 **

定义在成员位置，使用static修饰的代码块{}

位置：类中方法外。 

执行：随着类的加载而执行且执行一次，优先于main方法和构造方法的执行。 

~~~
public class ClassName{
    static {
    // 执行语句
    }
}

~~~

#### 16、多态性

多态： 是指同一行为，具有多个不同表现形式。 

表现形式

1. 继承或者实现【二选一】 

2. 方法的重写【意义体现：不重写，无意义】

3. 父类引用指向子类对象【格式体现】 

   ~~~
   父类类型 变量名 = new 子类对象；
   变量名.方法名();
   ~~~

多态的好处，体现在，可以使程序编写的更简单，并有良好的扩展。 

#### 17、final

final： 不可改变。可以用于修饰类、方法和变量。 

类：被修饰的类，不能被继承。 
方法：被修饰的方法，不能被重写。 
变量：被修饰的变量，不能被重新赋值。 
	* 基本类型的局部变量，被final修饰后，只能赋值一次，不能再更改。
 	* 引用类型的局部变量，被final修饰后，只能指向一个对象，地址不能再更改。但是不影响对象内部的成员变量值的 修改，  

~~~
final class 类名 {
}

修饰符 final 返回值类型 方法名(参数列表){
//方法体
}

final int a = 10;
final User u = new User();

~~~

#### 18、权限修饰符

public：公共的。 
protected：受保护的 
default：默认的 
private：私有的 

|                        | public | protected | default(空的) | private |
| ---------------------- | ------ | --------- | ------------- | ------- |
| 同一类中               | y      | y         | y             | y       |
| 同一包中(子类与无关类) | y      | y         | y             |         |
| 不同包的子类           | y      | y         |               |         |
| 不同包中的无关类       | y      |           |               |         |

#### 19、内部类

将一个类A定义在另一个类B里面，里面的那个类A就称为内部类，B则称为外部类。 

~~~
class 外部类 {
    class 内部类{
    }
} 
~~~

**访问特点**

1、内部类可以直接访问外部类的成员，包括私有成员。 
2、外部类要访问内部类的成员，必须要建立内部类的对象。 

创建内部类对象格式 

~~~
外部类名.内部类名 对象名 = new 外部类型().new 内部类型()；

~~~

#### 20、匿名内部类

是内部类的简化写法。它的本质是一个 带具体实现的 父类或者父接口的 匿名的 子类对象。 
开发中，最常用到的内部类就是匿名内部类了。
以接口举例，当你使用一个接口时，似乎得做如下几步操作，

  1. 定义子类 
  2. 重写接口中的方法 
  3. 创建子类对象 
  4. 调用重写后的方法 

匿名内部类必须继承一个父类或者实现一个父接口 

~~~
new 父类名或者接口名(){
	// 方法重写
    @Override
    public void method() {
    	// 执行语句
    }
};	
~~~

#### 21、 String StringBuffer 和 StringBuilder 的区别是什么? String 为什么是不可变的

`String` 类中使用 final 关键字修饰字符数组来保存字符串，`private final char value[]`，所以` String` 对象是不可变的。 
在 Java 9 之后，String 类的实现改用 byte 数组存储字符串 `private final byte[] value`; 

而 `StringBuilder` 与 `StringBuffer` 都继承自 `AbstractStringBuilder` 类，在 `AbstractStringBuilder` 中也是使用字符数组保存字符串`char[]value` 但是没有用 `final` 关键字修饰，所以这两种对象都是可变的。 

**线程安全性**

`String` 中的对象是不可变的，也就可以理解为常量，线程安全。`AbstractStringBuilder` 是 `StringBuilder` 与 `StringBuffer` 的公共父类，定义了一些字符串的基本操作，如 `expandCapacity`、`append`、`insert`、`indexOf` 等公共方法。`StringBuffer` 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。`StringBuilder` 并没有对方法进行加同步锁，所以是非线程安全的。 

**性能**

每次对 `String` 类型进行改变的时候，都会生成一个新的 `String` 对象，然后将指针指向新的 `String` 对象。`StringBuffer` 每次都会对 `StringBuffer` 对象本身进行操作，而不是生成新的对象并改变对象引用。相同情况下使用 `StringBuilder` 相比使用 `StringBuffer` 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。 

**使用场景**

1. 操作少量的数据: 适用 `String`
2. 单线程操作字符串缓冲区下操作大量数据: 适用 `StringBuilder`
3. 多线程操作字符串缓冲区下操作大量数据: 适用 `StringBuffer`

#### 22、Object的常见方法

Object 类是一个特殊的类，是所有类的父类。它主要提供了以下 11 个方法： 

~~~
public final native Class<?> getClass()//native方法，用于返回当前运行时对象的Class对象，使用了final关键字修饰，故不允许子类重写。

public native int hashCode() //native方法，用于返回对象的哈希码，主要使用在哈希表中，比如JDK中的HashMap。
public boolean equals(Object obj)//用于比较2个对象的内存地址是否相等，String类对该方法进行了重写用户比较字符串的值是否相等。

protected native Object clone() throws CloneNotSupportedException//naitive方法，用于创建并返回当前对象的一份拷贝。一般情况下，对于任何对象 x，表达式 x.clone() != x 为true，x.clone().getClass() == x.getClass() 为true。Object本身没有实现Cloneable接口，所以不重写clone方法并且进行调用的话会发生CloneNotSupportedException异常。

public String toString()//返回类的名字@实例的哈希码的16进制的字符串。建议Object所有的子类都重写这个方法。

public final native void notify()//native方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。

public final native void notifyAll()//native方法，并且不能重写。跟notify一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。

public final native void wait(long timeout) throws InterruptedException//native方法，并且不能重写。暂停线程的执行。注意：sleep方法没有释放锁，而wait方法释放了锁 。timeout是等待时间。

public final void wait(long timeout, int nanos) throws InterruptedException//多了nanos参数，这个参数表示额外时间（以毫微秒为单位，范围是 0-999999）。 所以超时的时间还需要加上nanos毫秒。

public final void wait() throws InterruptedException//跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念

protected void finalize() throws Throwable { }//实例被垃圾回收器回收的时候触发的操作

~~~

#### 23、序列化和反序列

序列化就是指把Java对象转换为字节序列的过程  一般用于保存到磁盘 或者 传输数据

反序列化就是指把字节序列恢复为Java对象的过程。 

#### 24、线程任务的创建

**概念 **

并发：指两个或多个事件在同一个时间段内发生。
并行：指两个或多个事件在同一时刻发生（同时发生）。

 单核处理器的计算机肯定是不能并行的处理多个任务的，只能是多个任务在单个CPU上并发运行。同理,线程也是一样的，从宏观角度上理解线程是并行运行的，但是从微观角度上分析却是串行运行的，即一个线程一个线程的去运行，当系统只有一个CPU时，线程会以某种顺序执行多个线程，我们把这种情况称之为线程调度。 

进程：是指一个内存中运行的应用程序，每个进程都有一个独立的内存空间，一个应用程序可以同时运行多个进程；进程也是程序的一次执行过程，是系统运行程序的基本单位；系统运行一个程序即是一个进程从创建、运行到消亡的过程。

 线程：线程是进程中的一个执行单元，负责当前进程中程序的执行，一个进程中至少有一个线程。一个进程中是可以有多个线程的，这个应用程序也可以称之为多线程程序。简而言之：一个程序运行后至少有一个进程，一个进程中可以包含多个线程 

**常用的线程任务创建的几种方式 **

1、一个类继承Thread  然后重写里面的run()方法  

~~~
Thread类的构造方法和常用方法
构造方法
public Thread() :分配一个新的线程对象。 
public Thread(String name) :分配一个指定名字的新的线程对象。 
public Thread(Runnable target) :分配一个带有指定目标新的线程对象。 
public Thread(Runnable target,String name) :分配一个带有指定目标新的线程对象并指定名字。

常用方法
public String getName() :获取当前线程名称。 
public void start() :导致此线程开始执行; Java虚拟机调用此线程的run方法。 
public void run() :此线程要执行的任务在此处定义代码。 
public static void sleep(long millis) :使当前正在执行的线程以指定的毫秒数暂停（暂时停止执行）。 
public static Thread currentThread() :返回对当前正在执行的线程对象的引用。
~~~

2、定义Runnable接口重写run()方法 创建Runnable的接口的实现类 然后Thread传实现类参数 

1） 定义Runnable接口的实现类，并重写该接口的run()方法，该run()方法的方法体同样是该线程的线程执行体。 
2）创建Runnable实现类的实例，并以此实例作为Thread的target来创建Thread对象，该Thread对象才是真正 的线程对象。 
3）调用线程对象的start()方法来启动线程。 

Thread类实际上也是实现了Runnable接口的类。 

**注意**
所有的多线程代码都是通过运行Thread的start()方法来运行的。因此，不管是继承Thread类还是实现 Runnable接口来实现多线程，最终还是通过Thread的对象的API来控制线程的，熟悉Thread类的API是进行多线程 编程的基础。
**Runnable对象仅仅作为Thread对象的target，Runnable实现类里包含的run()方法仅作为线程执行体。 而实际的线程对象依然是Thread实例，只是该Thread线程负责执行其target的run()方法。** 

#### 25、锁的代码实现

**1、同步代码块** 

在Runable接口实现类的Run()方法中加入 同步代码块
然后new几个Thread调用同样的任务 

~~~
在Runable接口实现类的Run()方法中加入 同步代码块
然后new几个Thread调用同样的任务 

synchronized(同步锁){
 需要同步操作的代码 
}
~~~

同步锁
对象的同步锁只是一个概念,可以想象为在对象上标记了一个锁

1. 锁对象 可以是任意类型。 
2. 多个线程对象 要使用同一把锁。

在任何时候,最多允许一个线程拥有同步锁,谁拿到锁就进入代码块,其他的线程只能在外等着



**2、同步方法**

同步方法:使用synchronized修饰的方法,就叫做同步方法,保证A线程执行该方法的时候,其他线程只能在方法外 等着。 
在Runable接口实现类的Run()方法中加入 同步方法然后new几个Thread调用同样的任务 

~~~

~~~

同步锁是谁? 
对于非static方法,同步锁就是this。
对于static方法,我们使用当前方法所在类的字节码对象(类名.class)。 

**3、锁机制**

Lock锁机制提供了比synchronized代码块和synchronized方法更广泛的锁定操作, 同步代码块/同步方法具有的功能Lock都有,除此之外更强大,更体现面向对象。
在Runable接口实现类的Run()方法中加入 Locak锁然后new几个Thread调用同样的任务 

 ~~~
Lock锁也称同步锁，加锁与释放锁方法化了，
public void lock() :加同步锁。 
public void unlock() :释放同步锁。

Lock lock = new ReentrantLock();
在run()方法中
运行开始前lock.lock();
运行完毕后lock.unlock();
 ~~~

26、线程状态

![捕获](C:\Users\lenovo\AppData\Local\Temp\chrome_drag4936_29673\捕获.PNG)

![捕获](C:\Users\lenovo\AppData\Local\Temp\chrome_drag4936_17942\捕获.PNG)

#### 26、异常

Java异常处理的五个关键字：try、catch、finally、throw、throws 

**1、抛出异常throw(用在方法内)** 

**使用步骤**1.创建一个异常对象。封装一些提示信息(信息可以自己编写)。2.需要将这个异常对象告知给调用者。怎么告知呢？怎么将这个异常对象传递到调用者处呢？通过关键字throw就可以完成。throw 异常对象。
**throw用在方法内**，用来抛出一个异常对象，将这个异常对象传递到调用者处，并结束当前方法的执行。 

~~~
使用格式
throw new 异常类名(参数);

举例：
throw new NullPointerException("要访问的arr数组不存在");
throw new ArrayIndexOutOfBoundsException("该索引在数组中不存在，已超出范围");
~~~

**2、声明异常throws(关键字throws运用于方法声明之上) **

声明异常：将问题标识出来，报告给调用者。如果方法内通过throw抛出了编译时异常，而没有捕获处理（稍后讲解该方式），那么必须通过throws进行声明，让调用者去处理。
**关键字throws运用于方法声明之上**,用于表示当前方法不处理异常,而是提醒该方法的调用者来处理异常(抛出异常). 

声明异常格式修饰符 返回值类型 方法名(参数) throws 异常类名1,异常类名2…{   }     

throws用于进行异常类的声明，若该方法可能有多种异常情况产生，那么在throws后面可以写多个异常类，用逗号隔开。 

~~~
public class ThrowsDemo {
    public static void main(String[] args) throws FileNotFoundException {
        read("a.txt");
    }
    // 如果定义功能时有问题发生需要报告给调用者。可以通过在方法上使用throws关键字进行声明
    public static void read(String path) throws FileNotFoundException {
        if (!path.equals("a.txt")) {//如果不是 a.txt这个文件
            // 我假设  如果不是 a.txt 认为 该文件不存在 是一个错误 也就是异常  throw
            throw new FileNotFoundException("文件不存在");
        }
    }
}


public class ThrowsDemo2 {
    public static void main(String[] args) throws IOException {
        read("a.txt");
    }
    public static void read(String path)throws FileNotFoundException, IOException {
        if (!path.equals("a.txt")) {//如果不是 a.txt这个文件
            // 我假设  如果不是 a.txt 认为 该文件不存在 是一个错误 也就是异常  throw
            throw new FileNotFoundException("文件不存在");
        }
        if (!path.equals("b.txt")) {
            throw new IOException();
        }
    }
}
~~~

**3、捕获异常try...catch**

如果异常出现的话,会立刻终止程序,所以我们得处理异常:

1.该方法不处理,而是声明抛出,由该方法的调用者来处理(throws)。
2.在方法中使用try-catch的语句块来处理异常。 

 **try-catch的方式就是捕获异常。**捕获异常：Java中对异常有针对性的语句进行捕获，可以对出现的异常进行指定方式的处理。 

~~~
捕获异常语法
try{
     编写可能会出现异常的代码
}catch(异常类型  e){
     处理异常的代码
     //记录日志/打印异常信息/继续抛出异常
}
语法解释
try：该代码块中编写可能产生异常的代码。
catch：用来进行某种异常的捕获，实现对捕获到的异常进行处理。
注意:try和catch都不能单独使用,必须连用。



如何获取异常信息：
Throwable类中定义了一些查看方法:

public String getMessage():获取异常的描述信息,原因(提示给用户的时候,就提示错误原因。
public String toString():获取异常的类型和异常描述信息(不用)。
public void printStackTrace():打印异常的跟踪栈信息并输出到控制台。
        包含了异常的类型,异常的原因,还包括异常出现的位置,在开发和调试阶段,都得使用printStackTrace。
~~~

~~~
代码举例
public class TryCatchDemo {
    public static void main(String[] args) {
        try {// 当产生异常时，必须有处理方式。要么捕获，要么声明。
            read("b.txt");
        } catch (FileNotFoundException e) {// 括号中需要定义什么呢？
              //try中抛出的是什么异常，在括号中就定义什么异常类型
            System.out.println(e);
        }
        System.out.println("over");
    }
    /*
     *
     * 我们 当前的这个方法中 有异常  有编译期异常
     */
    public static void read(String path) throws FileNotFoundException {
        if (!path.equals("a.txt")) {//如果不是 a.txt这个文件
            // 我假设  如果不是 a.txt 认为 该文件不存在 是一个错误 也就是异常  throw
            throw new FileNotFoundException("文件不存在");
        }
    }
}
~~~

**4、finally代码块**

finally：有一些特定的代码无论异常是否发生，都需要执行。另外，因为异常会引发程序跳转，导致有些语句执行不到。而finally就是解决这个问题的，在finally代码块中存放的代码都是一定会被执行的。 

什么时候的代码必须最终执行？ 当我们在try语句块中打开了一些物理资源(磁盘文件/网络连接/数据库连接等),我们都得在使用完之后,最终关闭打开的资源。 

finally的语法:
	try...catch....finally:自身需要处理异常,最终还得关闭资源。
finally不能单独使用。 

当只有在try或者catch中调用退出JVM的相关方法,此时finally才不会执行,否则finally永远会执行。 

~~~
代码举例

public class TryCatchDemo4 {
    public static void main(String[] args) {
        try {
            read("a.txt");
        } catch (FileNotFoundException e) {
            //抓取到的是编译期异常  抛出去的是运行期
            throw new RuntimeException(e);
        } finally {
            System.out.println("不管程序怎样，这里都将会被执行。");
        }
        System.out.println("over");
    }
    /*
     *
     * 我们 当前的这个方法中 有异常  有编译期异常
     */
    public static void read(String path) throws FileNotFoundException {
        if (!path.equals("a.txt")) {//如果不是 a.txt这个文件
            // 我假设  如果不是 a.txt 认为 该文件不存在 是一个错误 也就是异常  throw
            throw new FileNotFoundException("文件不存在");
        }
    }
}
~~~

**5、异常捕获注意**

多个异常使用捕获又该如何处理呢？
    1.多个异常分别处理。
    2.多个异常一次捕获，多次处理。
    3.多个异常一次捕获一次处理。

一般我们是使用一次捕获多次处理方式，

~~~

格式如下：
try{
     编写可能会出现异常的代码
}catch(异常类型A  e){  当try中出现A类型异常,就用该catch来捕获.
     处理异常的代码
     //记录日志/打印异常信息/继续抛出异常
}catch(异常类型B  e){  当try中出现B类型异常,就用该catch来捕获.
     处理异常的代码
     //记录日志/打印异常信息/继续抛出异常
}
注意:这种异常处理方式，要求多个catch中的异常不能相同，并且若catch中的多个异常之间有子父类异常的关系，那么子类异常要求在上面的catch处理，父类异常在下面的catch处理。
~~~

运行时异常被抛出可以不处理。即不捕获也不声明抛出。
如果finally有return语句,永远返回finally中的结果,避免该情况.
如果父类抛出了多个异常,子类重写父类方法时,抛出和父类相同的异常或者是父类异常的子类或者不抛出异常。
父类方法没有抛出异常，子类重写父类该方法时也不可抛出异常。此时子类产生该异常，只能捕获处理，不能声明抛出

**6、自定义异常 **

什么是自定义异常类:
在开发中根据自己业务的异常情况来定义异常类.
自定义一个业务逻辑异常: RegisterException。一个注册异常类。


异常类如何定义:
1.自定义一个编译期异常: 自定义类 并继承于java.lang.Exception。
2.自定义一个运行时期的异常类:自定义类 并继承于java.lang.RuntimeException。

~~~
代码举例

要求：我们模拟注册操作，如果用户名已存在，则抛出异常并提示：亲，该用户名已经被注册。
1、首先定义一个登陆异常类RegisterException：

// 业务逻辑异常
public class RegisterException extends Exception {
    /**
     * 空参构造
     */
    public RegisterException() {
    }

    /**
     *
     * @param message 表示异常提示
     */
    public RegisterException(String message) {
        super(message);
    }
}

2、 模拟登陆操作，使用数组模拟数据库中存储的数据，并提供当前注册账号是否存在方法用于判断。

public class Demo {
    // 模拟数据库中已存在账号
    private static String[] names = {"bill","hill","jill"};
    public static void main(String[] args) {     
        //调用方法
        try{
              // 可能出现异常的代码
            checkUsername("nill");
            System.out.println("注册成功");//如果没有异常就是注册成功
        }catch(RegisterException e){
            //处理异常
            e.printStackTrace();
        }
    }

    //判断当前注册账号是否存在
    //因为是编译期异常，又想调用者去处理 所以声明该异常
    public static boolean checkUsername(String uname) throws LoginException{
        for (String name : names) {
            if(name.equals(uname)){//如果名字在这里面 就抛出登陆异常
                throw new RegisterException("亲"+name+"已经被注册了！");
            }
        }
        return true;
    }
}
~~~

#### 27、IO流

IO流分为几种？

- 按照流的流向分，可以分为输入流和输出流；
- 按照操作单元划分，可以划分为字节流和字符流；
- 按照流的角色划分为节点流和处理流。

Java I0 流的 40 多个类都是从如下 4 个抽象类基类中派生出来的。 

- InputStream/Reader: 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
- OutputStream/Writer: 所有输出流的基类，前者是字节输出流，后者是字符输出流

按操作方式分类结构图 



![IO-操作方式分类](https://camo.githubusercontent.com/639ec442b39898de071c3e4fd098215fb48f11e9/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f494f2d2545362539332538442545342542442539432545362539362542392545352542432538462545352538382538362545372542312542422e706e67)

按操作对象分类结构图： 

![IO-操作对象分类](https://camo.githubusercontent.com/4a44e49ab13eacac26cbb0e481db73d6d11181b7/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f494f2d2545362539332538442545342542442539432545352541462542392545382542312541312545352538382538362545372542312542422e706e67)

##### BIO,NIO,AIO 有什么区别?

- **BIO (Blocking I/O):** 同步阻塞 I/O 模式，数据的读取写入必须阻塞在一个线程内等待其完成。在活动连接数不是特别高（小于单机 1000）的情况下，这种模型是比较不错的，可以让每一个连接专注于自己的 I/O 并且编程模型简单，也不用过多考虑系统的过载、限流等问题。线程池本身就是一个天然的漏斗，可以缓冲一些系统处理不了的连接或请求。但是，当面对十万甚至百万级连接的时候，传统的 BIO 模型是无能为力的。因此，我们需要一种更高效的 I/O 处理模型来应对更高的并发量。
- **NIO (Non-blocking/New I/O):** NIO 是一种同步非阻塞的 I/O 模型，在 Java 1.4 中引入了 NIO 框架，对应 java.nio 包，提供了 Channel , Selector，Buffer 等抽象。NIO 中的 N 可以理解为 Non-blocking，不单纯是 New。它支持面向缓冲的，基于通道的 I/O 操作方法。 NIO 提供了与传统 BIO 模型中的 `Socket` 和 `ServerSocket` 相对应的 `SocketChannel` 和 `ServerSocketChannel` 两种不同的套接字通道实现,两种通道都支持阻塞和非阻塞两种模式。阻塞模式使用就像传统中的支持一样，比较简单，但是性能和可靠性都不好；非阻塞模式正好与之相反。对于低负载、低并发的应用程序，可以使用同步阻塞 I/O 来提升开发速率和更好的维护性；对于高负载、高并发的（网络）应用，应使用 NIO 的非阻塞模式来开发
- **AIO (Asynchronous I/O):** AIO 也就是 NIO 2。在 Java 7 中引入了 NIO 的改进版 NIO 2,它是异步非阻塞的 IO 模型。异步 IO 是基于事件和回调机制实现的，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。AIO 是异步 IO 的缩写，虽然 NIO 在网络操作中，提供了非阻塞的方法，但是 NIO 的 IO 行为还是同步的。对于 NIO 来说，我们的业务线程是在 IO 操作准备好时，得到通知，接着就由这个线程自行进行 IO 操作，IO 操作本身是同步的。查阅网上相关资料，我发现就目前来说 AIO 的应用还不是很广泛，Netty 之前也尝试使用过 AIO，不过又放弃了。

#### 28、ConcurrentHashMap的数据结构（必考）

HashMap是线程不安全的 HashTable虽然线程安全，但是性能很差。![这里写图片描述](https://img-blog.csdn.net/20180521100148713?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDA5NjE3Ng==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

在ConcurrentHashMap中有个重要的概念就是**Segment**。我们知道HashMap的结构是数组+链表形式(或红黑树)，从图中我们可以看出其实每个segment就类似于一个HashMap。Segment包含一个HashEntry数组，数组中的每一个HashEntry既是一个键值对，也是一个链表的头节点。在ConcurrentHashMap中有2的N次方个Segment，共同保存在一个名为segments的数组当中。可以说，**ConcurrentHashMap是一个二级哈希表**。**在一个总的哈希表下面，有若干个子哈希表**。 

为什么说ConcurrentHashMap的性能要比HashTable好，HashTables是用全局同步锁，而CconurrentHashMap采用的是锁分段，每一个Segment就好比一个自治区，读写操作高度自治，Segment之间互不干扰。 

1、**不同Segment的并发写入 ** 

![这里写图片描述](https://img-blog.csdn.net/20180521102004201?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDA5NjE3Ng==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

不同Segment的写入是可以并发执行的。 

2、**同一Segment的一写一读** 

![这里写图片描述](https://img-blog.csdn.net/20180521102237419?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDA5NjE3Ng==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

同一Segment的写和读是可以并发执行的。 

3、**同一Segment的并发写入** 

![这里写图片描述](https://img-blog.csdn.net/2018052110232651?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDA5NjE3Ng==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

Segment的写入是需要上锁的，因此对同一Segment的并发写入会被阻塞。

由此可见，ConcurrentHashMap当中每个Segment各自持有一把锁。在保证线程安全的同时降低了锁的粒度，让并发操作效率更高。 上面我们说过，每个Segment就好比一个HashMap，其实里面的操作原理都差不多，只是Segment里面加了锁。 

~~~
Get方法：
1.为输入的Key做Hash运算，得到hash值。
2.通过hash值，定位到对应的Segment对象
3.再次通过hash值，定位到Segment当中数组的具体位置。

public V get(Object key) {
        Segment<K,V> s; // manually integrate access methods to reduce overhead
        HashEntry<K,V>[] tab;
        int h = hash(key);
        long u = (((h >>> segmentShift) & segmentMask) << SSHIFT) + SBASE;
        if ((s = (Segment<K,V>)UNSAFE.getObjectVolatile(segments, u)) != null &&
            (tab = s.table) != null) {
            for (HashEntry<K,V> e = (HashEntry<K,V>) UNSAFE.getObjectVolatile
                     (tab, ((long)(((tab.length - 1) & h)) << TSHIFT) + TBASE);
                 e != null; e = e.next) {
                K k;
                if ((k = e.key) == key || (e.hash == h && key.equals(k)))
                    return e.value;
            }
        }
        return null;
    }
~~~

~~~
Put方法：
1.为输入的Key做Hash运算，得到hash值。
2.通过hash值，定位到对应的Segment对象
3.获取可重入锁
4.再次通过hash值，定位到Segment当中数组的具体位置。
5.插入或覆盖HashEntry对象。
6.释放锁。
final V put(K key, int hash, V value, boolean onlyIfAbsent) {
            HashEntry<K,V> node = tryLock() ? null :
                scanAndLockForPut(key, hash, value);
            V oldValue;
            try {
                HashEntry<K,V>[] tab = table;
                int index = (tab.length - 1) & hash;
                HashEntry<K,V> first = entryAt(tab, index);
                for (HashEntry<K,V> e = first;;) {
                    if (e != null) {
                        K k;
                        if ((k = e.key) == key ||
                            (e.hash == hash && key.equals(k))) {
                            oldValue = e.value;
                            if (!onlyIfAbsent) {
                                e.value = value;
                                ++modCount;
                            }
                            break;
                        }
                        e = e.next;
                    }
                    else {
                        if (node != null)
                            node.setNext(first);
                        else
                            node = new HashEntry<K,V>(hash, key, value, first);
                        int c = count + 1;
                        if (c > threshold && tab.length < MAXIMUM_CAPACITY)
                            rehash(node);
                        else
                            setEntryAt(tab, index, node);
                        ++modCount;
                        count = c;
                        oldValue = null;
                        break;
                    }
                }
            } finally {
                unlock();
            }
            return oldValue;
        }
~~~

~~~
可以看出get方法并没有加锁，那怎么保证读取的正确性呢，这是应为value变量用了volatile来修饰，
static final class HashEntry<K,V> {
        final int hash;
        final K key;
        volatile V value;
        volatile HashEntry<K,V> next;

        HashEntry(int hash, K key, V value, HashEntry<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }
        .....
~~~

~~~
size方法
既然每一个Segment都各自加锁，那么我们在统计size()的时候怎么保证解决一直性的问题，比如我们在计算size时，有其他线程在添加或删除元素。

  ConcurrentHashMap的Size方法是一个嵌套循环，大体逻辑如下：
1.遍历所有的Segment。
2.把Segment的元素数量累加起来。
3.把Segment的修改次数累加起来。
4.判断所有Segment的总修改次数是否大于上一次的总修改次数。如果大于，说明统计过程中有修改，重新统计，尝试次数+1；如果不是。说明没有修改，统计结束。
5.如果尝试次数超过阈值，则对每一个Segment加锁，再重新统计。
6.再次判断所有Segment的总修改次数是否大于上一次的总修改次数。由于已经加锁，次数一定和上次相等。
7.释放锁，统计结束。
可以看出JDK1.7的size计算方式有点像乐观锁和悲观锁的方式，为了尽量不锁住所有Segment，首先乐观地假设Size过程中不会有修改。当尝试一定次数，才无奈转为悲观锁，锁住所有Segment保证强一致性。
~~~

JDK1.7和JDK1.8中ConcurrentHashMap的区别

~~~
1、在JDK1.8中ConcurrentHashMap的实现方式有了很大的改变，在JDK1.7中采用的是Segment + HashEntry，而Sement继承了ReentrantLock，所以自带锁功能，而在JDK1.8中则取消了Segment，作者认为Segment太过臃肿，采用Node + CAS + Synchronized（ps:Synchronized一直以来被各种吐槽性能差，但java一直没有放弃Synchronized，也一直在改进，既然作者在这里采用了Synchronized，可见Synchronized的性能应该是有所提升的，当然只是猜想哈哈哈。。。）

2、在上篇HashMap中我们知道，在JDK1.8中当HashMap的链表个数超过8时，会转换为红黑树，在ConcurrentHashMap中也不例外。这也是新能的一个小小提升。

3、在JDK1.8版本中，对于size的计算，在扩容和addCount()时已经在处理了。JDK1.7是在调用时才去计算。
为了帮助统计size，ConcurrentHashMap提供了baseCount和counterCells两个辅助变量和CounterCell辅助类，1.8中使用一个volatile类型的变量baseCount记录元素的个数，当插入新数据或则删除数据时，会通过addCount()方法更新baseCount。

//ConcurrentHashMap中元素个数,但返回的不一定是当前Map的真实元素个数。基于CAS无锁更新
~~~

#### 29、volatile作用(必考)

**1 保证内存可见性** 

1.1 基本概念

可见性是指线程之间的可见性，一个线程修改的状态对另一个线程是可见的。也就是一个线程修改的结果，另一个线程马上就能看到。

1.2 实现原理

当对非volatile变量进行读写的时候，每个线程先从主内存拷贝变量到CPU缓存中，如果计算机有多个CPU，每个线程可能在不同的CPU上被处理，这意味着每个线程可以拷贝到不同的CPU cache中。

volatile变量不会被缓存在寄存器或者对其他处理器不可见的地方，保证了每次读写变量都从主内存中读，跳过CPU cache这一步。当一个线程修改了这个变量的值，新值对于其他线程是立即得知的。

![img](https://picb.zhimg.com/80/v2-0a097560b7216a8b25522df4e7168096_720w.jpg)

**2 禁止指令重排**

**2.1 基本概念 **

指令重排序是JVM为了优化指令、提高程序运行效率，在不影响单线程程序执行结果的前提下，尽可能地提高并行度。指令重排序包括编译器重排序和运行时重排序。

volatile变量禁止指令重排序。针对volatile修饰的变量，在读写操作指令前后会插入内存屏障，指令重排序时不能把后面的指令重排序到内存屏

```
示例说明：
double r = 2.1; //(1) 
double pi = 3.14;//(2) 
double area = pi*r*r;//(3)
```

虽然代码语句的定义顺序为1->2->3，但是计算顺序1->2->3与2->1->3对结果并无影响，所以编译时和运行时可以根据需要对1、2语句进行重排序。

**2.2 指令重排带来的问题**

如果一个操作不是原子的，就会给JVM留下重排的机会。

```
线程A中
{
    context = loadContext();
    inited = true;
}

线程B中
{
    if (inited) 
        fun(context);
}
```

如果线程A中的指令发生了重排序，那么B中很可能就会拿到一个尚未初始化或尚未初始化完成的context,从而引发程序错误。

**2.3 禁止指令重排的原理**

volatile关键字提供内存屏障的方式来防止指令被重排，编译器在生成字节码文件时，会在指令序列中插入内存屏障来禁止特定类型的处理器重排序。

JVM内存屏障插入策略：

每个volatile写操作的前面插入一个StoreStore屏障；

在每个volatile写操作的后面插入一个StoreLoad屏障；

在每个volatile读操作的后面插入一个LoadLoad屏障；

在每个volatile读操作的后面插入一个LoadStore屏障。

**2.4 指令重排在双重锁定单例模式中的影响**

基于双重检验的单例模式(懒汉型)

```
public class Singleton3 {
    private static Singleton3 instance = null;

    private Singleton3() {}

    public static Singleton3 getInstance() {
        if (instance == null) {
            synchronized(Singleton3.class) {
                if (instance == null)
                    instance = new Singleton3();// 非原子操作
            }
        }

        return instance;
    }
}
```

instance= new Singleton()并不是一个原子操作，其实际上可以抽象为下面几条JVM指令：

```
memory =allocate();    //1：分配对象的内存空间 
ctorInstance(memory);  //2：初始化对象 
instance =memory;     //3：设置instance指向刚分配的内存地址
```

上面操作2依赖于操作1，但是操作3并不依赖于操作2。所以JVM是可以针对它们进行指令的优化重排序的，经过重排序后如下：

```
memory =allocate();    //1：分配对象的内存空间 
instance =memory;     //3：instance指向刚分配的内存地址，此时对象还未初始化
ctorInstance(memory);  //2：初始化对象
```

指令重排之后，instance指向分配好的内存放在了前面，而这段内存的初始化被排在了后面。在线程A执行这段赋值语句，在初始化分配对象之前就已经将其赋值给instance引用，恰好另一个线程进入方法判断instance引用不为null，然后就将其返回使用，导致出错。

**解决办法**

用volatile关键字修饰instance变量，使得instance在读、写操作前后都会插入内存屏障，避免重排序。

```
public class Singleton3 {
    private static volatile Singleton3 instance = null;

    private Singleton3() {}

    public static Singleton3 getInstance() {
        if (instance == null) {
            synchronized(Singleton3.class) {
                if (instance == null)
                    instance = new Singleton3();
            }
        }
        return instance;
    }
}
```

3 适用场景

（1）volatile是轻量级同步机制。在访问volatile变量时不会执行加锁操作，因此也就不会使执行线程阻塞，是一种比synchronized关键字更轻量级的同步机制。

（2）volatile**无法同时保证内存可见性和原子性。加锁机制既可以确保可见性又可以确保原子性，而volatile变量只能确保可见性**。

（3）volatile不能修饰写入操作依赖当前值的变量。声明为volatile的简单变量如果当前值与该变量以前的值相关，那么volatile关键字不起作用，也就是说如下的表达式都不是原子操作：“count++”、“count = count+1”。

（4）当要访问的变量已在synchronized代码块中，或者为常量时，没必要使用volatile；

（5）volatile屏蔽掉了JVM中必要的代码优化，所以在效率上比较低，因此一定在必要时才使用此关键字。

#### 30、Atomic类如何保证原子性(高频)

Atomic包是java.util.concurrent下的另一个专门为线程安全设计的java的包，包含多个原子性操作的类。 

**CAS**

每一个CAS操作都包含三个运算符：内存地址V ， 一个期望值A和新值B，操作的时候如果这个地址上存放的值等于期望的值A，那么就赋值为B，如果没有，那么就不做任何操作。简而言之，你的值和我的期望值相等，就给你新值，否则不干活了。 

##### ABA问题

CAS算法是取出内存中某时刻的数据并在当下时刻比较并替换，这中间会有一个时间差导致数据变化。比如说有两个线程，一快一慢，同时从主内存中取变量A，快线程将变量改为B并写入主内存，然后又将B从主内存中取出再改为A写入主内存，这时候慢线程刚完成了工作，使用CAS算法，发现预期值还是A，然后慢线程将自己的结果写入主内存。虽然慢线程操作成功，但是这个过程可能是有问题的

#### 31、ArrayList 和 LinkedList的区别

主要区别：ArrayList是Array(动态数组)的数据结构，而LinkedList是Link(双向链表)的数据结构。随机访问（get和set）时，ArrayList优于LinkedList；新增和删除操作，LinedList比较占优势 

#### 32、抽象类能用final修饰吗

不能，抽象类是被用于继承的，final修饰代表不可修改、不可继承的。 

#### 33、HashSet和HashMap的区别

hashset存的是对象 在存之前需要重写一下hashcode和equal方法 不允许对象的值有重复
hashMap存的是键值对 键不重复 