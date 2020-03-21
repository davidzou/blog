JDK14新特性介绍
====

***

### 描述

OpenJDK14 [^ openjdk] 是Java SE平台的版本14的开源参考实现，由Java社区流程中的[JSR 389](https://openjdk.java.net/projects/jdk/14/)指定 。2020年3月17日JDK14达到一般可用性, 基于GPL协议的构建许可文件可从[Oracle](https://jdk.java.net/14/)获得; 其他供应商的二进制文件将在不久之后。已通过JEP 2.0提案修订的[JEP流程](https://openjdk.java.net/jeps/0)提出并跟踪了此版本的功能和时间表。该发行版是使用JDK发行流程（[JEP 3](https://openjdk.java.net/jeps/3)）制作的。

此次的更新包含了很多令人兴奋的新功能。总共有非常令人印象深刻的16个JDK增强建议（JEP）和69个新的API元素。

***

### 新功能

#### *Records*

Java是一种面向对象的语言。您可以创建类来保存数据，并使用封装来控制如何访问和修改该数据。对象的使用使操作复杂的数据类型变得简单而直接。这是Java如此流行作为平台的原因之一。缺点（到现在为止）是，创建数据类型非常冗长，即使在最直接的情况下也需要大量代码。让我们看一下基本二维点所需的代码：

```
public class Point {
  private final double x;
  private final double y;

  public Point(double x, double y) {
    this.x = x;
    this.y = y;
  }

  public double getX() {
    return x;
  }

  public double getY() {
    return y;
  }
}
```
这14行代码仅仅表示了2个值元。

JDK 14引入了记录（records）作为预览功能。预览功能是一个新概念，使Java平台的开发人员可以在不使其成为Java SE标准一部分的情况下包括新的语言功能。通过这样做，开发人员可以尝试这些功能，并提供反馈，以便在必要时在标准中设置功能之前进行更改（甚至删除功能）。要使用预览功能，必须为编译和运行时指定命令行标志`--enable-preview`。对于编译，还必须指定`-source`标志。

记录是表示数据类的一种简单得多的方法。如果以Point为例，代码可以简化为一行：

```
public record Point(double x, double y) { }
```

这与代码的可读性无关。我们立即意识到，我们现在有了一个包含两个称为x和y的双精度值的类，我们可以使用getX和getY的标准方法名称进行访问。

让我们检查一下记录的一些细节。

首先，记录是一种新的类型，它是类的受限形式，就像枚举一样。记录具有名称和状态描述，用于定义记录的组成部分。在上面的Point示例中，状态描述是double和x和y。记录是为了简化而设计的，因此它们不能扩展任何其他类或定义其他实例变量。记录中的所有状态都是最终状态，因此不提供set方法。如果您需要任何一个，则需要使用成熟的类。

记录确实具有灵活性。

通常，构造函数除了提供值外还需要提供其他行为。如果是这种情况，我们可以提供构造函数的替代实现：

```
record Range(int min, int max) {
  Range {
    if (min > max)
      throw new IllegalArgumentException(“Max must be >= min”);
  }
}
```
请注意，由于指定参数是多余的，因此构造方法的定义仍被缩写。也可以声明从状态描述自动派生的任何成员，因此，例如，您可以提供toString （）或hashCode （）实现的替代方法。

#### *instanceof*

在某些情况下，您不知道对象的确切类型。为了解决这个问题，Java有instanceof运算符，可用于针对不同类型进行测试。这样做的缺点是确定了对象的类型。如果要使用显式类型转换，则必须使用显式类型转换：

```
if (o instanceof String) {
  String s = (String)o;
  System.out.println(s.length);
}
```
在JDK 14中，对instanceof运算符进行了扩展，以允许除了类型之外还指定变量名。然后可以使用该变量而无需显式强制转换：

```
if (o instanceof String s)
  System.out.println(s.length);
```
变量的范围限于在逻辑上正确使用的地方，因此：
```
if (o instanceof String s)
  System.out.println(s.length);
else
  // s在这里超出范围
```

该范围也可以在条件语句中应用，因此我们可以执行以下操作：

```
if (o instanceof String s && s.length() > 4) ...
```
这是有意义的，因为length()方法将只是被称为如果 o为一个字符串。逻辑或运算符不一样：

```
if (o instanceof String s || s.length() > 4) ...
```

在这种情况下，无论o是否为String的结果，都需要对s.length（）进行求值。从逻辑上讲，这不起作用，因此会导致编译错误。

使用逻辑非运算符可以产生一些有趣的作用域效果：

```
if (!(o instanceof String s && s.length() > 3)
  return;
System.out.println(s.length());  // s 在这里
```
我已经看到了对变量的范围划分方式的一些负面反馈，但是，鉴于所有作用域都是完全合乎逻辑的，因此我认为它非常有效。

已经有计划提供此功能的第二个预览版本，该版本将扩展模式匹配以与记录一起使用，并提供实现解构模式的简单方法。可以在[JEP 375](https://openjdk.java.net/jeps/375)中找到更多详细信息。

#### *有帮助的NullPointException*

任何编写了多行Java代码的人都将在某个时候遇到NullPointerException。未能初始化对象引用（或将其错误地显式设置为null），然后尝试使用该引用将导致引发此异常。

在简单的情况下，找出问题的原因很简单。如果我们尝试运行以下代码：

```
public class NullTest {
  List<String> list;
  public NullTest() {
    list.add("foo");
  }
}
```

生成的错误是：

```
Exception in thread "main" java.lang.NullPointerException
            at jdk14.NullTest.<init>(NullTest.java:16)
            at jdk14.Main.main(Main.java:15)
```

由于我们在第16行上引用列表，因此很明显，列表是罪魁祸首，我们可以快速解决问题。
但是，如果我们在这样的行中使用链接引用：

```
a.b.c.d = 12;
```

运行此命令时，我们可能会看到如下错误：

```
Exception in thread "main" java.lang.NullPointerException
    at Prog.main(Prog.java:5)
```

问题在于我们无法由此确定异常是由于a为null，b为null还是c为null的结果。我们要么需要使用IDE中的调试器，要么更改代码，将引用分隔到不同的行上。两者都不是理想的。

在JDK 14中，如果我们运行相同的代码，我们将看到类似以下内容：

```
Exception in thread "main" java.lang.NullPointerException:
    Cannot read field "c" because "a.b" is null
    at Prog.main(Prog.java:5)
```

马上，我们可以看到ab是问题所在，并着手进行纠正。我敢肯定，这将使许多Java开发人员的生活更加轻松，绝对拍手叫好的，更好的定位才是解决bug的最佳途径。

***

### 新API

#### *java.io*
PrintStream有两个新方法，write（byte [] buf）和writeBytes（byte [] buf）。这些有效地做同样的事情，等效于write（buf，0，buf.length）。之所以拥有两种不同的方法，是因为将write定义为抛出IOException（但是，奇怪的是，从不抛出），而writeBytes没有。因此，选择使用哪种方法取决于您是否希望使用try-catch块包围该调用。

有一个新的注释类型，`Serial `。它旨在用于对序列化的编译器检查。特别是，此类型的注释应应用于与序列化相关的方法和声明为可序列化的类中的字段。（它在某些方面与Override注释类似）。

#### *java.lang*

`Class`类为新的`Record`功能提供了两种方法：`isRecord()`和`getRecordComponents()`。`getRecordComponents()`方法返回一个`RecordComponent`对象数组。`RecordComponent`是`java.lang.reflect`包中的新类，它具有11种方法来检索内容，例如注释的详细信息和泛型。

`Record`是一个简单的新类，它重写Object的equals，hashCode和toString方法。

`NullPointerException`现在作为有帮助的NullPointerExceptions功能的一部分覆盖了Throwable中的getMessage方法。

`StrictMath`类具有六个新方法，这些方法补充了需要检测溢出错误时使用的现有精确方法。新方法是decrementExact，incrementExact和negateExact（所有方法都有int和long参数的两个重载版本）。

#### *java.lang.annotation*

`ElementType`枚举为`Records`添加了一个新的常量RECORD_TYPE。

#### *java.lang.invoke*

MethodHandles.Lookup类具有两个新方法：

* hasFullPrivilegeAccess，如果要查找的方法同时具有PRIVATE和MODULE访问权，则返回true。
* previousLookupClass，它报告此查找对象先前从其传送过来的另一个模块中的查找类，或者为null。以前，我从未听说过在Java上下文中进行隐形传送（仅在Doctor Who和Minecraft中）。查找可能导致模块之间的传送。

#### *java.lang.runtime*

这是JDK 14中的新软件包，只有一个类ObjectMethods。这是记录功能的低级部分，它具有单个方法（引导程序），该方法生成对象等于，hashCode和toString方法。

#### *java.util.text*

CompactNumberFormat类具有一个新的构造函数，该构造函数为pluralRules添加了一个参数。这是一个String，表示将Count关键字（例如“ one”）与实际整数关联的多个规则。它的语法在Unicode Consortium的复数规则语法中定义。

#### *java.util*

HashSet类具有一个新方法toArray，该方法返回一个数组，其运行时组件类型为Object，包含此集合中的所有元素。

#### *java.util.concurrent.locks*

`LockSupport`类具有一个新方法setCurrentBlocker。`LockSupport`提供了暂存和取消暂存线程的功能（与不赞成使用的Thread.suspend和Thread.resume方法没有相同的问题）。现在可以设置getBlocker将返回的Object。从非公共对象调用无参数方法时，这很有用。

#### *javax.lang.model.element*

`ElementKind`枚举具有三个新常量，用于记录和模式匹配实例的特征，即`BINDING_VARIABLE`，`RECORD`和`RECORD_COMPONENT`。

#### *javax.lang.model.util*

该软件包提供实用程序，以协助处理程序元素和类型。通过添加记录，添加了一组新的抽象类和具体类来支持此功能。（示例是AbstractTypeVisitor14，ElementScanner14和TypeKindVisitor14）。

#### *org.xml.sax*

一种新方法已添加到SAX XML解析器的ContentHandler接口。声明方法接收XML声明的通知。在默认实现的情况下，此操作无效。

#### *JEP 370：外部内存访问API*

这是作为孵化器模块引入的，以允许更广泛的Java社区进行测试，并在其成为Java SE标准的一部分之前集成反馈。它旨在替代`sun.misc.Unsafe`和`java.io.MappedByteBuffer`。

外部存储器访问API引入了三个主要抽象：

* MemorySegment：提供对具有给定范围的连续内存区域的访问。
* MemoryAddress：提供到MemorySegment的偏移量（基本上是一个指针）。
* MemoryLayout：提供一种描述内存段布局的方法，该方法大大简化了使用var句柄访问MemorySegment的过程。使用此功能，无需根据内存的使用方式来计算偏移量。例如，一个整数或长整数数组的偏移量将有所不同，但将使用MemoryLayout透明地对其进行处理。

***

### JVM改变

* [JEP 345](https://openjdk.java.net/jeps/345): G1的NUMA感知内存分配。这样可以提高使用非均匀内存体系结构（NUMA）的大型计算机的性能。

* [JEP 363](https://openjdk.java.net/jeps/363): 删除并发标记扫描（CMS）垃圾收集器。从JDK 9开始，G1已经成为默认的收集器，大多数（但不是全部）都认为G1是CMS的高级收集器。考虑到维护两个相似的配置文件收集器所需的资源，Oracle决定弃用[CMS](https://openjdk.java.net/jeps/291)（在JDK 9中也是如此），现在将其删除。如果您正在寻找一款高性能，低延迟替代，为什么不尝试 Zing JVM的[C4](http://go.azul.com/continuously-concurrent-compacting-collector)？

* [JEP 349](https://openjdk.java.net/jeps/349): Java Flight Recorder事件流。通过启用工具以异步方式订阅Java Flight Recorder事件，这可以对JVM进行更实时的监视。

* [JEP 364](https://openjdk.java.net/jeps/364): macOS上的ZGC和 [JEP 365](https://openjdk.java.net/jeps/365)：Windows上的ZGC。ZGC是实验性的低延迟收集器，最初仅在Linux上受支持。现在，它已扩展到macOS和Windows操作系统。

* [JEP 366](https://openjdk.java.net/jeps/366): 弃用ParallelScavenge和SerialOld GC组合。Oracle指出，很少有人会使用这种组合，并且维护开销相当大。期望在不久的将来某个时候可以删除此组合。

***

### 其它功能
有许多与OpenJDK的不同部分相关的JEP：

* [JEP 343](https://openjdk.java.net/jeps/343)：打包工具。这是一个简单的打包工具，基于从JDK 11中的Oracle JDK中删除的JavaFX javapackager工具。它有很多功能，因此我将在单独的博客文章中详细介绍。

* [JEP 352](https://openjdk.java.net/jeps/352)：非易失性映射字节缓冲区。这添加了新的特定于JDK的文件映射模式，以便可以使用FileChannel API创建引用非易失性存储器的MappedByteBuffer实例。添加了一个新模块jdk.nio.mapmode，以允许将MapMode设置为READ_ONLY_SYNC或WRITE_ONLY_SYNC。

* [JEP 361](https://openjdk.java.net/jeps/361)：开关表达式标准。开关表达式是JDK 12中添加到OpenJDK的第一个预览功能。在JDK 13中，反馈导致将break值语法更改为yield 值。 在JDK 14中，开关表达式不再是预览功能，并且已包含在Java SE标准中。

* [JEP 362](https://openjdk.java.net/jeps/362)：弃用Solaris和SPARC端口。由于Oracle不再开发Solaris操作系统或SPARC芯片体系结构，因此他们不再需要继续维护这些端口。这些可能会被Java社区中的其他人所接受。

* [JEP 367](https://openjdk.java.net/jeps/367)：删除Pack 200工具和API。JDK 11中不推荐使用的另一个功能现在已从JDK 14中删除。此压缩格式的主要用途是用于applet使用的jar文件。鉴于浏览器插件已从Oracle JDK 11中删除，这似乎是一个合理的功能。

* [JEP 368](https://openjdk.java.net/jeps/368)：文本块。这些已作为预览功能包含在JDK 13中，并且在JDK 14中继续进行第二次迭代（仍在预览中）。文本块提供了对多行字符串文字的支持。所做的更改是添加了两个新的转义序列。第一个通过在行的末尾添加\来抑制换行符的包含（这在软件开发的许多其他地方很常见）。第二个是\ s，表示单个空格。这对于防止从文本块中的行尾剥离空白很有用。


***

[^ openjdk]: [OpenJDK14官方信息](https://openjdk.java.net/projects/jdk/14/)
