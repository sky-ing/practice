
####JVM核心

#####类加载过程
1. 类加载机制
    + JVM把class文件加载到内存，并对数据进行校验、解析和初始化，最总形成JVM可以直接使用的Java类型的过程
    + 加载：将class文件字节码内容加载到内存中，并将这些静态数据转换成方法区中的运行时数据结构，在堆中生成一个代表这个类的java.lang.Class对象，最为方法区类数据的访问入口
    + 链接：将Java类的二进制代码合并到JVM的运行状态之中的过程
        + 验证：确保加载的类信息符合JVM贵方，没有安全方面的问题
        + 准备：正式为类变量（static变量）分配内存并设置类变量初始值的阶段，这些内存都将在方法区中进行分配
        + 解析：虚拟机常量池内的符号引用替换为直接引用的过程
    + 初始化
        + 初始化阶段是执行类构造器<clinit>()方法的过程。类构造器<clinit>()方法是由编译器自动收集类中的所有类变量的赋值动作和静态语句块(static块)中的语句合并产生的
        + 当初始化一个类的时候，如果发现其父类还没有进行过初始化、则需要先初始化其父类的初始化
        + 虚拟机会暴躁一个类的<clinit>()方法在多线程环境中被正确加锁和同步
        + 当访问一个Java类的静态域的类才会被初始化

#####类的引用
1. 类的主动引用
    + new一个类的对象
    + 调用类的静态成员（除了final常量）和静态方法
    + 使用java.lang.reflect包的方法对类进行反射调用
    + 当虚拟机启动，java Hello,则一定会初始化Hello类，说白了就是先启动main方法锁在的类
    + 当初始化一个类，如果其父类没有被初始化，则先会初始化他的父类
2. 类的被动引用（不会发生类的初始化）
    + 当访问一个静态域时，只有真正声明这个域的类才会被初始化。（通过自雷引用父类的静态变量，不会导致子类初始化）
    + 通过数组定义类引用，不会触发此类的初始化
    + 引用常量不会触发此类的初始化（常量在编译阶段就存入调用类的常量池了）

#####类缓存
标准的Java SE类加载器可以按要求查找类，但一旦摸个类被加载到类加载器中，它将维持加载（缓存）一段时间。不过，JVM垃圾收集器可以回收这些Class对象。

#####类加载器的层次结构（树状结构）
1. 引导类加载器（bootstrap class loader）（C写的，其他事JAVA写得）
    + 它用来加载Java的核心库（JAVA_HOME/jre/lib/rt.jar,或sun.boot.class.path路径下的内容），是用原生代码来实现的，并不继承自java.lang.ClassLoader
    + 加载扩展类和应用程序类加载器。并制定他们的父类加载器
2. 扩展类加载器（extensions class loader）
    + 用来加载Java的扩展库（JAVA_HOME/jre/ext/*.jar,或java.ext.dirs路径下的内容）Java虚拟机的实现会提供一个扩展库目录。该类加载器在此目录里面查找并加载Java类
    + 由sun.misc.Launcher$ExtClassLoader实现
3. 应用程序类加载器(application class loader)
    + 它根据Java应用的类路径（classpath,java.class.path路径）的类
    + 一般来说，Java应用的类都是由她来完成加载的
    + 由sun.misc.Launcher$AppClassLoader实现
4. 自定义加载器
    + 开发人员可以通过继承java.lang.ClassLoader类的方式实现自己的类加载器，以满足一些特殊的需求

#####ClassLoader
1. 作用
    + java.lang.ClassLoader类的基本职责就是根据一个指定的类的名称，找到或者生成其对应的字节代码，然后从这些字节代码中定义出一个Java类，即java.lang.Class类的一个实例
    + 除此之外，ClassLoader还辅助加载Java应用所需的资源，如图像文件和配置文件等
2. 相关方法
    + getParent() 返回该类加载器的父类加载器。
    + loadClass(String name) 加载名称为name的类，返回的结果是java.lang.Class类的实例。
    + findClass(String name) 查找名称为name的类，返回的结果是java.lang.Class类的实例。
    + findLoadedClass(String name)  查找名称为name的已经被加载过的类，返回的结果是 java.lang.Class类的实例。
    + defineClass(String name, byte[] b, int off, int len)  把字节数组b中的内容转换成Java类，返回的结果是java.lang.Class类的实例。这个方法被声明为final的。
    + resolveClass(Class<?> c)链接指定的 Java 类。
    + 对于以上给出的方法，表示类名称的 name参数的值是类的二进制名称。需要注意的是内部类的表示，如 com.example.Sample$1和com.example.Sample$Inner等表示方式。

#####类加载器的代理模式
1. 代理模式 交给其他加载器来加载指定的类
2. 双亲委托机制
    + 就是摸个特定的类加载器在接到加载类的请求时，首先将加载任务委托给父类加载器，依次追溯，直到最高的爷爷辈的，如果父类加载器可以完成类加载任务，就成功返回；只有父类加载器无法完成此加载任务时，才自己去加载
    + 双亲委托机制是为了保证Java核心库的类型安全。（这种机制就保证不会出现用户自己能定义java.lang.Object类的情况）
    + 类加载器除了用于加载类，也是安全的最基本的屏障
3. 双亲委托机制是代理模式的一种
    + 并不是所有的类加载器都采用双亲委托机制
    + tomcat服务器类加载器也使用代理模式，所不同的是它是首先尝试去加载某个类，如果找不到再代理给父类加载器。这与一般类加载器的顺序是相反的

#####自定义类加载器
1. 自定义类加载器的流程
    + 继承：java.lang.ClassLoader
    +
