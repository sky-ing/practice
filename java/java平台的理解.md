##谈谈对JAVA平台的理解
---
####Java 语言特性
#####1、面向对象

#####2、反射

#####3、泛型

#####4、Lambda

---
####Java类库
#####1、核心类库
  + 集合
  + IO/NIO
  + 网络
  + 并发
#####2、安全类库

#####3、jdk management等类库

#####4、第三方类库

---
####Java虚拟机
#####1、垃圾收集器
垃圾收集的基本原理，最常见的垃圾收集器，如Serial GC、Parallel GC、CMS、G1等，对于适用于什么样的工作负载最好也心里有数
#####2、运行时
常用版本JDK内嵌的Class-Loader:
+ Bootstrap Class-Loader
+ Application Class-loader：负责loader用户定义的class，这个loader可以通过ClassLoader.getSystemClassLoader()获得
+ Extension Class-loader：负责loader the class from jre/lib/ext 文件下在jar，他负责jdk的扩展类

#####3、动态编译

#####4、辅助功能
如JFR等

---
####工具
#####1、辅助工具
如jlink,jar,jdeps之类

#####2、编译器
javac、sjavac

#####3、诊断工具
jmap、jstack、jconsole、jhsdb、jcmd

#####4、Lambda


---
