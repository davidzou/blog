Null Safety in Dart --- Introduction （Dart空安全介绍）
========

### 历史
2020年11月空安全进入Beta测试阶段, 自2.12及Flutter2.0之后开始全面支持`Sound null safety`。

### 介绍

一般来说，变量储存了某些值。例如整数可以是0, 42, -2；一个String可以是 hello world，或者其他什么的内容。

```
// In null-safe Dart, none of these can ever be null.
var i = 42; // Inferred to be an int.
String name = getFileName();
final b = Foo();
```
但是对大部分包括Dart在内的编程语言而言，还有一种空值的概念，也就是没有值。
例如，我喜欢的颜色这个变量，可以是空值，这是合理的。有些人就是没有喜欢的颜色。

```
    setBackground(my.favoriteColor); 
```

这就产生了一个问题，如果变量既可以是一种类型。例如整数或者颜色，又可以是空，那么你每次使用的时候，都要想一想，
这个颜色变量到底存储了真的颜色值，还是空的颜色值？这一串整数数列里的某一个值，到底是整数，还是空值？ 😫 呀哒

事实证明，程序员开发时不会一天到晚想着这个问题，常常制造出跟空值有关的错误。NullPointException可不是好东西。 你的应用会因此而奔溃，用户可不喜欢这样。


在默认设定中，Dart假设你的变量都不是空值。例如，年龄变量不可为空，你不能将它设为空值。这有点像双重否定的概念（Filip Hracek说的）。

```
    my.age = null;  // 我靠这是什么, 你的年龄竟然是空。 🤔️何者じゃ？
```

换个说法，年龄变量一定是某个数值，不可以是空值。这是默认设定，所有类型的默认都是不可为空。 


不过你还是可以使用空值，但是你要明确的说明。 如果要容许favoriteColor使用空值，就要加上问好。这个变量就可以为空。它既可以是颜色值，也可以是空值。因为我们已经明确标识了。

```
    setBackground(my?.favoriteColor); 
```

现在Dart可以帮助我们避免刚才提到的问题，如果你使用favoriteColor为color，忘记它可能是空值，Dart就会提醒你，这样你的代码就更安全了。

另外，Dart的空安全页很靠谱。（Filip Hracek说的）。 

如果Dart判断某个变量不可为空，那个变量就从头到尾就都不可为空。IDE中已经会通过上下文做出判断检测，不仅仅检测一行代码哦。正所谓编译时报错，而非运行时报错了。


许多其他的程序语言并没有靠谱的空安全机制。经常要在运行期间进行检查。

||NullSafety|Sound NullSafety|
|:---:|:---:|:---:|
|C#|X| |
|Dart|X|X|
|Java|X| |
|Kotlin|X| |
|Swift|X|X|
|TypeScript|X| |

而且Dart与Swift共享靠谱的空安全机制，其他语言可说不准了。由于Dart的空安全机制很靠谱，它还可以令你的程序运行得更快。不但更安全，而且更迅速。
各位程序员，这对你来说代表什么呢？🚀


我前面说过，大部分变量一定有某种类型的值，绝不会没有值，也就是说，他们不可为空。这是比较好懂的说法。比"它们不可以有一个什么都没有的值"好一点。


因此，你大部分的代码应该看起来像这样： 没有问好(`?.`)，没有空值判断（`!`），什么都没有。

```
int fibonacci(int n) {
    if (n <= 1) return 1;
    return fibonacci(n - 1) + fibonacci(n - 2);
} 

void main() {
    print(fibonacci(10));
}
```


如果你觉得某个变量可能会出现空值，你就必须明确的标识它可以为空。方法就是加上问好。

```
Color? favoriteColor 

void setBackground(Color? color) {
    // ...
}

Color? chooseFavorite() {
    // ...
}
List<Color?> favorites;
```

它不但可以用在声明变量的时候，还可以用在函数参数、返回值，范型等等。


Dart还会帮助你避免跟空值有关的错误。基本上他会要你在适当的地方加上空值检查，并且处理空值出现的情况。

```
void setBackground(Color? color) {
    // ...
}

Color definitelyColor = Color(0xffff00ff);
setBackground(difinitelyColor);  // 这是对的

```

你可能先前忘记处理了。如果是介于两者之间的情况呢？如果你将一个不可能为空的变量，传给可空的字段，是没有问题的。
一个不可为空的颜色变量，可以传给一个接受空值参数的函数，可是反过来的话，就没有那么简单了。一个可为空的颜色变量，就不可以传给参数不可为空的函数。

```
void setBackground(Color color) {
    // ...
} 

Color? definitelyColor = null;
setBackground(definitelyColor);   // 这是错误的
```


换句话说，如果你的函数说，我不接受空值，我的参数都不可为空，你就不能将可能为空值的东西丢给它。这种情况怎么处理？🤔️


### 实践 

> 有几种可能的做法。

1. 完全不要调用这个函数，如果你知道变量是空值。
2. 你知道变量是空值的，就改用别的变量。提供默认值。
3. 使用感叹号，强迫使用空值。这跟写一段空值检查很类似。并在失败时抛出异常。这个类虽可为空值，但你只有在100%确定这个变量不是空值的情况下才可以使用这个方法。

``` 
setBackground(definitelyColor ?? black);

setBackground(definitelyColor！);
```

比如说，你知道这个程序的运作背景，而Dart却无从得知。不过大部分情况下，你应该尽量避免使用感叹号。就这样，空安全令你的代码更安全，运行的更快速，对你编写方式的影响也降到最小。


> 旧项目迁移

如果有兴趣可以看下Dart提供了命令行工具，来转化你的旧代码到NullSafety的迁移。

```
dart migrate 
```

请到[dart.dev/null-safety](https://dart.dev/null-safety)查阅更多资料