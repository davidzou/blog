## 开场白
[GraalVM](https://www.graalvm.org/)正成为JVM生态系统中最流行的话题之一。它承诺以尽可能高的速度运行基于JVM的程序（编译为本机映像时），同时占用较小的内存。听起来很有趣，可以试一试。今天，我将在编译成独立的本机映像后运行简单的Groovy程序。
这篇博客文章记录了运行编译成GraalVM本机映像的Groovy代码的非常简单的用例。它会让你很好地理解如何开始，以及如何解决当你开始玩自己的例子时，你可能会面临的问题。

## 环境安装
我将使用以下工具:
GraalVM (20.0.0)
Groovy (2.5.9)

让我们确保使用正确的GraalVM Java发行版。
* 检查java版本
```
zzw:Home zzw$ java -version
openjdk version "1.8.0_242"
OpenJDK Runtime Environment (build 1.8.0_242-b06)
OpenJDK 64-Bit Server VM GraalVM CE 20.0.0 (build 25.242-b06-jvmci-20.0-b02, mixed mode)
```
从19.0.0版本开始，GraalVM不包含native-image工具作为发行版的默认部分，因此我们需要使用GraalVM组件更新程序安装它。

* 使用gu（GraalVM发行版附带的工具）安装native-image工具
```
zzw:Home zzw$ gu install native-image
Downloading: Component catalog from www.graalvm.org
Processing Component: Native Image
Downloading: Component native-image: Native Image  from github.com
Installing new component: Native Image (org.graalvm.native-image, version 20.0.0)
```

* 检查groovy版本
```
rzzw:blog zzw$ groovy -v
Groovy Version: 2.5.9 JVM: 1.8.0_242 Vendor: Oracle Corporation OS: Mac OS X
```

具体请查看[GraalVM安装文档](https://www.graalvm.org/docs/getting-started/#install-graalvm)

## 实例
在这个实验中，我们将使用一个简单的Groovy程序来对数字进行求和和和和乘法：

###### RandomNumber.groovy
```
class RandomNumber {
    static void main(String[] args) {
        def random = new Random().nextInt(1000)

        println "The random number is: $random"

        def sum = (0..random).sum { int num -> num * 2 }

        println "The doubled sum of numbers between 0 and $random is $sum"
    }
}
```
GraalVM更喜欢静态编译[2]，因此我们需要将Groovy从动态编译切换到静态编译。我们可以简单地将@groovy.transform.CompileStatic注释放在RandomNumber.groovy文件中的类定义上，或者创建一个编译器配置文件，将此注释添加到所有类中。

###### compiler.groovy
```
withConfig(configuration) {
    ast(groovy.transform.CompileStatic)
}
```

#### 我们使用groovyc来编译代码：
```
zzw:groovy_and_graalvm zzw$ ls
RandomNumber.groovy compiler.groovy
zzw:groovy_and_graalvm zzw$ groovyc --configscript compiler.groovy RandomNumber.groovy
```

让我们查看下执行结果
```
zzw:groovy_and_graalvm zzw$ groovy RandomNumber
The random number is: 507
The doubled sum of numbers between 0 and 507 is 257556
```

它跑得很平稳！您可能注意到了一个小延迟-结果没有立即打印到控制台上。我们可以再次运行程序，但这次添加了time命令来测量执行时间。

```
zzw:groovy_and_graalvm zzw$ time groovy RandomNumber
The random number is: 81
The doubled sum of numbers between 0 and 81 is 6642

real	0m2.334s
user	0m4.612s
sys	    0m0.512s
```
花了2334毫秒才完成。是慢的还是快的？这要看情况。如果我们将它与同一个程序进行比较，但是在Groovy解释器命令行工具中执行，Java程序大约快2.5倍。

```
zzw:groovy_and_graalvm zzw$ time java -cp ".:$GROOVY_HOME/lib/groovy-2.5.9.jar" RandomNumber
The random number is: 545
The doubled sum of numbers between 0 and 545 is 297570

real	0m0.782s
user	0m1.589s
sys	    0m0.201s
```

让我们看看GraalVM的native-image是否能做得更好。

## 创建native-image
GraalVM最有趣的特性之一是它能够从给定的Java字节码（Java.class或.jar文件）创建独立的本机二进制文件。

在JVM中运行我们的示例很好，但是GraalVM提供了更多。我们可以创建一个独立的native-image，它将消耗更少的内存，并在一瞬间执行。我们试试看：

编译大约需要60秒，这是我们应该期望的输出。
```
zzw:groovy_and_graalvm zzw$ native-image --allow-incomplete-classpath --report-unsupported-elements-at-runtime --initialize-at-build-time --initialize-at-run-time=org.codehaus.groovy.control.XStreamUtils,groovy.grape.GrapeIvy --no-fallback --no-server -cp ".:$GROOVY_HOME/lib/groovy-2.5.9.jar" RandomNumber
[randomnumber:6194]    classlist:  11,048.19 ms,  1.42 GB
[randomnumber:6194]        (cap):   6,565.97 ms,  1.42 GB
[randomnumber:6194]        setup:  10,826.18 ms,  1.42 GB
[randomnumber:6194]   (typeflow):  43,086.61 ms,  2.24 GB
[randomnumber:6194]    (objects):  38,040.58 ms,  2.24 GB
[randomnumber:6194]   (features):   1,546.67 ms,  2.24 GB
[randomnumber:6194]     analysis:  84,102.69 ms,  2.24 GB
[randomnumber:6194]     (clinit):     845.98 ms,  2.24 GB
[randomnumber:6194]     universe:   3,034.29 ms,  2.24 GB
[randomnumber:6194]      (parse):  11,970.45 ms,  2.55 GB
[randomnumber:6194]     (inline):   9,875.36 ms,  2.56 GB
[randomnumber:6194]    (compile):  77,254.46 ms,  4.28 GB
[randomnumber:6194]      compile: 102,050.59 ms,  4.28 GB
[randomnumber:6194]        image:   7,570.92 ms,  4.28 GB
[randomnumber:6194]        write:   2,697.79 ms,  4.28 GB
[randomnumber:6194]      [total]: 222,450.30 ms,  4.28 GB
```

### 运行独立的native-image文件
查看编译后的文件，24M的大小
```
zzw:groovy_and_graalvm zzw$ ls -alh
total 48752
drwxr-xr-x  7 zzw  staff   224B  3  1 00:49 .
drwxr-xr-x  6 zzw  staff   192B  3  1 00:05 ..
-rw-r--r--  1 zzw  staff   1.6K  3  1 00:34 RandomNumber$_main_closure1.class
-rw-r--r--  1 zzw  staff   2.7K  3  1 00:34 RandomNumber.class
-rw-r--r--  1 zzw  staff   298B  2 23 00:00 RandomNumber.groovy
-rw-r--r--  1 zzw  staff    70B  2 23 00:02 compiler.groovy
-rwxr-xr-x  1 zzw  staff    24M  3  1 00:49 randomnumber
```

直接执行
```
zzw:groovy_and_graalvm zzw$ ./randomnumber
The random number is: 114
Exception in thread "main" groovy.lang.MissingMethodException: No signature of method: RandomNumber$_main_closure1.doCall() is applicable for argument types: (Integer) values: [0]
Possible solutions: findAll(), findAll(), isCase(java.lang.Object), isCase(java.lang.Object)
	at org.codehaus.groovy.runtime.metaclass.ClosureMetaClass.invokeMethod(ClosureMetaClass.java:255)
	at groovy.lang.MetaClassImpl.invokeMethod(MetaClassImpl.java:1041)
	at groovy.lang.Closure.call(Closure.java:405)
	at org.codehaus.groovy.runtime.DefaultGroovyMethods.sum(DefaultGroovyMethods.java:6648)
	at org.codehaus.groovy.runtime.DefaultGroovyMethods.sum(DefaultGroovyMethods.java:6548)
	at RandomNumber.main(RandomNumber.groovy:7)
```

有错误被中断了。随机数的第一行是：114被正确打印，但在尝试调用RandomNumber$\_main_closure1.doCall（int）时失败。怎么回事？
此方法表示传递给（0..random）.sum（）方法的闭包。问题是doCall（int）方法查找的过程使用反射。而且，尽管native-image支持运行时反射，但在某些情况下，它无法正确地找到它，因此需要用户提供额外的配置。

### 反射配置
GraalVM本机映像的手动反射配置相当简单。我们所要做的就是创建一个JSON配置文件并将-H:ReflectionConfigurationFiles=…添加到命令行。我们可以使用帮助器选项（如allDeclaredMethods）配置将被反射使用的类，也可以手动提供希望使用反射调用的方法（及其参数）的列表。为了使这个例子简单，我们将使用第一种方法。
###### reflections.json
```
[
  {
    "name": "RandomNumber$_main_closure1",
    "allDeclaredConstructors": true,
    "allPublicConstructors": true,
    "allDeclaredMethods": true,
    "allPublicMethods": true
  }
]
```
反射配置可以参考[这里](https://github.com/oracle/graal/blob/master/substratevm/REFLECTION.md#manual-configuration)

重新编译native-iamge
```
zzw:groovy_and_graalvm zzw$ native-image --allow-incomplete-classpath --report-unsupported-elements-at-runtime --initialize-at-build-time --initialize-at-run-time=org.codehaus.groovy.control.XStreamUtils,groovy.grape.GrapeIvy --no-fallback --no-server -cp ".:$GROOVY_HOME/lib/groovy-2.5.9.jar" -H:ReflectionConfigurationFiles=reflections.json RandomNumber
[randomnumber:6555]    classlist:  12,353.97 ms,  1.56 GB
[randomnumber:6555]        (cap):   3,546.12 ms,  1.56 GB
[randomnumber:6555]        setup:   8,947.81 ms,  1.56 GB
[randomnumber:6555]   (typeflow):  40,152.46 ms,  2.47 GB
[randomnumber:6555]    (objects):  36,473.16 ms,  2.47 GB
[randomnumber:6555]   (features):   1,347.55 ms,  2.47 GB
[randomnumber:6555]     analysis:  79,091.59 ms,  2.47 GB
[randomnumber:6555]     (clinit):     950.82 ms,  2.47 GB
[randomnumber:6555]     universe:   2,944.74 ms,  2.47 GB
[randomnumber:6555]      (parse):  12,665.35 ms,  2.50 GB
[randomnumber:6555]     (inline):   9,530.42 ms,  2.81 GB
[randomnumber:6555]    (compile):  73,834.22 ms,  3.39 GB
[randomnumber:6555]      compile:  99,606.00 ms,  3.39 GB
[randomnumber:6555]        image:   7,761.15 ms,  3.41 GB
[randomnumber:6555]        write:   2,626.08 ms,  3.41 GB
[randomnumber:6555]      [total]: 214,830.88 ms,  3.41 GB
```

再次执行
```
zzw:groovy_and_graalvm zzw$ ./randomnumber
The random number is: 558
java.lang.ClassNotFoundException: org.codehaus.groovy.runtime.dgm$521
	at com.oracle.svm.core.hub.ClassForNameSupport.forName(ClassForNameSupport.java:60)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:229)
	at org.codehaus.groovy.reflection.GeneratedMetaMethod$Proxy.createProxy(GeneratedMetaMethod.java:101)
	at org.codehaus.groovy.reflection.GeneratedMetaMethod$Proxy.proxy(GeneratedMetaMethod.java:93)
	at org.codehaus.groovy.reflection.GeneratedMetaMethod$Proxy.isValidMethod(GeneratedMetaMethod.java:78)
	at groovy.lang.MetaClassImpl.chooseMethodInternal(MetaClassImpl.java:3226)
	at groovy.lang.MetaClassImpl.chooseMethod(MetaClassImpl.java:3188)
	at groovy.lang.MetaClassImpl.getNormalMethodWithCaching(MetaClassImpl.java:1399)
	at groovy.lang.MetaClassImpl.getMethodWithCaching(MetaClassImpl.java:1314)
	at groovy.lang.MetaClassImpl.getMetaMethod(MetaClassImpl.java:1229)
	at groovy.lang.MetaClassImpl.invokeMethod(MetaClassImpl.java:1082)
	at groovy.lang.MetaClassImpl.invokeMethod(MetaClassImpl.java:1041)
	at org.codehaus.groovy.runtime.DefaultGroovyMethods.sum(DefaultGroovyMethods.java:6655)
	at org.codehaus.groovy.runtime.DefaultGroovyMethods.sum(DefaultGroovyMethods.java:6548)
	at RandomNumber.main(RandomNumber.groovy:7)
Exception in thread "main" groovy.lang.GroovyRuntimeException: Failed to create DGM method proxy : java.lang.ClassNotFoundException: org.codehaus.groovy.runtime.dgm$521
	at org.codehaus.groovy.reflection.GeneratedMetaMethod$Proxy.createProxy(GeneratedMetaMethod.java:106)
	at org.codehaus.groovy.reflection.GeneratedMetaMethod$Proxy.proxy(GeneratedMetaMethod.java:93)
	at org.codehaus.groovy.reflection.GeneratedMetaMethod$Proxy.isValidMethod(GeneratedMetaMethod.java:78)
	at groovy.lang.MetaClassImpl.chooseMethodInternal(MetaClassImpl.java:3226)
	at groovy.lang.MetaClassImpl.chooseMethod(MetaClassImpl.java:3188)
	at groovy.lang.MetaClassImpl.getNormalMethodWithCaching(MetaClassImpl.java:1399)
	at groovy.lang.MetaClassImpl.getMethodWithCaching(MetaClassImpl.java:1314)
	at groovy.lang.MetaClassImpl.getMetaMethod(MetaClassImpl.java:1229)
	at groovy.lang.MetaClassImpl.invokeMethod(MetaClassImpl.java:1082)
	at groovy.lang.MetaClassImpl.invokeMethod(MetaClassImpl.java:1041)
	at org.codehaus.groovy.runtime.DefaultGroovyMethods.sum(DefaultGroovyMethods.java:6655)
	at org.codehaus.groovy.runtime.DefaultGroovyMethods.sum(DefaultGroovyMethods.java:6548)
	at RandomNumber.main(RandomNumber.groovy:7)
Caused by: java.lang.ClassNotFoundException: org.codehaus.groovy.runtime.dgm$521
	at com.oracle.svm.core.hub.ClassForNameSupport.forName(ClassForNameSupport.java:60)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:229)
	at org.codehaus.groovy.reflection.GeneratedMetaMethod$Proxy.createProxy(GeneratedMetaMethod.java:101)
	... 12 more
```
又失败了。这次它找不到org.codehaus.groovy.runtime.dgm$521类。这是一个表示Groovy动态方法的类——用新方法扩展JDK类的方法。这个类也可以通过reflection访问，添加到reflection.json配置文件中。

#### reflections.json
```
[
  {
    "name": "RandomNumber$_main_closure1",
    "allDeclaredConstructors": true,
    "allPublicConstructors": true,
    "allDeclaredMethods": true,
    "allPublicMethods": true
  },
  {
    "name": "org.codehaus.groovy.runtime.dgm$521",
    "allDeclaredConstructors": true,
    "allPublicConstructors": true,
    "allDeclaredMethods": true,
    "allPublicMethods": true
  }
]
```
再次编译执行看看是否有效果

```
zzw:groovy_and_graalvm zzw$ ./randomnumber
The random number is: 689
java.lang.ClassNotFoundException: org.codehaus.groovy.runtime.dgm$1180
	at com.oracle.svm.core.hub.ClassForNameSupport.forName(ClassForNameSupport.java:60)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:229)
	at org.codehaus.groovy.reflection.GeneratedMetaMethod$Proxy.createProxy(GeneratedMetaMethod.java:101)
	at org.codehaus.groovy.reflection.GeneratedMetaMethod$Proxy.proxy(GeneratedMetaMethod.java:93)
	at org.codehaus.groovy.reflection.GeneratedMetaMethod$Proxy.isValidMethod(GeneratedMetaMethod.java:78)
	at groovy.lang.MetaClassImpl.chooseMethodInternal(MetaClassImpl.java:3226)
	at groovy.lang.MetaClassImpl.chooseMethod(MetaClassImpl.java:3188)
	at groovy.lang.MetaClassImpl.getNormalMethodWithCaching(MetaClassImpl.java:1399)
	at groovy.lang.MetaClassImpl.getMethodWithCaching(MetaClassImpl.java:1314)
	at groovy.lang.MetaClassImpl.getMetaMethod(MetaClassImpl.java:1229)
	at groovy.lang.MetaClassImpl.invokeMethod(MetaClassImpl.java:1082)
	at groovy.lang.MetaClassImpl.invokeMethod(MetaClassImpl.java:1041)
	at org.codehaus.groovy.runtime.DefaultGroovyMethods.sum(DefaultGroovyMethods.java:6655)
	at org.codehaus.groovy.runtime.DefaultGroovyMethods.sum(DefaultGroovyMethods.java:6548)
	at RandomNumber.main(RandomNumber.groovy:7)
Exception in thread "main" groovy.lang.GroovyRuntimeException: Failed to create DGM method proxy : java.lang.ClassNotFoundException: org.codehaus.groovy.runtime.dgm$1180
	at org.codehaus.groovy.reflection.GeneratedMetaMethod$Proxy.createProxy(GeneratedMetaMethod.java:106)
	at org.codehaus.groovy.reflection.GeneratedMetaMethod$Proxy.proxy(GeneratedMetaMethod.java:93)
	at org.codehaus.groovy.reflection.GeneratedMetaMethod$Proxy.isValidMethod(GeneratedMetaMethod.java:78)
	at groovy.lang.MetaClassImpl.chooseMethodInternal(MetaClassImpl.java:3226)
	at groovy.lang.MetaClassImpl.chooseMethod(MetaClassImpl.java:3188)
	at groovy.lang.MetaClassImpl.getNormalMethodWithCaching(MetaClassImpl.java:1399)
	at groovy.lang.MetaClassImpl.getMethodWithCaching(MetaClassImpl.java:1314)
	at groovy.lang.MetaClassImpl.getMetaMethod(MetaClassImpl.java:1229)
	at groovy.lang.MetaClassImpl.invokeMethod(MetaClassImpl.java:1082)
	at groovy.lang.MetaClassImpl.invokeMethod(MetaClassImpl.java:1041)
	at org.codehaus.groovy.runtime.DefaultGroovyMethods.sum(DefaultGroovyMethods.java:6655)
	at org.codehaus.groovy.runtime.DefaultGroovyMethods.sum(DefaultGroovyMethods.java:6548)
	at RandomNumber.main(RandomNumber.groovy:7)
Caused by: java.lang.ClassNotFoundException: org.codehaus.groovy.runtime.dgm$1180
	at com.oracle.svm.core.hub.ClassForNameSupport.forName(ClassForNameSupport.java:60)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:229)
	at org.codehaus.groovy.reflection.GeneratedMetaMethod$Proxy.createProxy(GeneratedMetaMethod.java:101)
	... 12 more
```

又失败了。这次找不到org.codehaus.groovy.runtime.dgm$1180类。将其添加到reflections.json配置文件中。

```
[
  {
    "name": "RandomNumber$_main_closure1",
    "allDeclaredConstructors": true,
    "allPublicConstructors": true,
    "allDeclaredMethods": true,
    "allPublicMethods": true
  },
  {
    "name": "org.codehaus.groovy.runtime.dgm$521",
    "allDeclaredConstructors": true,
    "allPublicConstructors": true,
    "allDeclaredMethods": true,
    "allPublicMethods": true
  },
  {
    "name": "org.codehaus.groovy.runtime.dgm$1180",
    "allDeclaredConstructors": true,
    "allPublicConstructors": true,
    "allDeclaredMethods": true,
    "allPublicMethods": true
  }
]
```

更新配置文件后，使用与之前相同的命令重新编译native-image。完成后，是运行程序的时候了。

```
zzw:groovy_and_graalvm zzw$ ./randomnumber
The random number is: 515
The doubled sum of numbers between 0 and 515 is 265740
```

<font color=red>成功了！</font>您还注意到，与以前的尝试（将Groovy代码作为Java程序运行）相比，反应时间要好得多。让我们来测量本机映像的执行时间。

```
zzw:groovy_and_graalvm zzw$ time ./randomnumber
The random number is: 833
The doubled sum of numbers between 0 and 833 is 694722

real	0m0.027s
user	0m0.010s
sys	0m0.010s
```

我靠，太令人兴奋了，才27毫秒。

![](https://github.com/davidzou/blog/tree/master/java/excutiontime.png)
