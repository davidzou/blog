Flutter Wrap 详解
======

Flutter Wrap是Flutter框架中的一个非常实用的布局组件，它可以帮助我们在移动端应用中实现自适应的、灵活的布局效果。本文将为大家详解Flutter Wrap的使用方法和应用场景。

### Flutter Wrap参数

* `direction`    子控件的排列方向，默认为水平方向，可选垂直方向。
* `alignment`    子控件的对齐方式，默认为start，可选center、end、spaceBetween、spaceAround、spaceEvenly。
* `spacing`    子控件之间的间距，默认为0。
* `runAlignment`    子控件所在行或列的对齐方式，默认为start，可选center、end、spaceBetween、spaceAround、spaceEvenly。
* `runSpacing`    子控件所在行或列之间的间距，默认为0。
* `crossAxisAlignment`    子控件在其所在行或列中的对齐方式，默认为start，可选center、end、baseline、stretch。
* `textDirection`    子控件的文本方向，默认为从左到右，可选从右到左。
* `verticalDirection`    子控件的垂直方向排列方式，默认为down，可选up。

### Flutter Wrap使用方法

Flutter Wrap的使用非常简单，只需要将需要排列的子控件放入Wrap组件中即可。例如：

```
Wrap(
  direction: Axis.horizontal,
  alignment: WrapAlignment.start,
  spacing: 10.0,
  runAlignment: WrapAlignment.center,
  runSpacing: 20.0,
  crossAxisAlignment: WrapCrossAlignment.center,
  children: <Widget>[
    Container(
      width: 100.0,
      height: 100.0,
      color: Colors.red,
    ),
    Container(
      width: 50.0,
      height: 50.0,
      color: Colors.green,
    ),
    Container(
      width: 80.0,
      height: 80.0,
      color: Colors.blue,
    ),
    Container(
      width: 120.0,
      height: 120.0,
      color: Colors.yellow,
    ),
  ],
)
```

上述代码中，我们创建了一个水平方向的Wrap组件，子控件之间的间距为10.0，行之间的间距为20.0。子控件分别是四个不同颜色的Container组件，它们的宽度和高度也不同。通过设置Wrap组件的各个参数，我们可以实现不同的布局效果。

### Flutter Wrap应用场景

1. 流式标签

Wrap组件非常适合实现流式标签效果。我们可以将标签作为Wrap组件的子控件，通过设置各个参数来达到自适应、灵活排列的效果。

2. 图片墙

在图片墙中，我们通常需要将图片按照一定规则排列，而且每张图片的大小可能不一样。这时候，Wrap组件就能够派上用场了。我们只需要将图片作为Wrap组件的子控件，通过设置各个参数来实现自适应排列。

3. 动态表单

在移动端应用中，我们经常需要实现各种表单页面。如果表单项比较多，而且每个表单项的大小和位置都不一样，那么使用Wrap组件就可以轻松实现动态表单了。

### 结论
总之，Flutter Wrap是一个非常实用的布局组件，可以帮助我们实现自适应、灵活排列的效果。无论是流式标签、图片墙还是动态表单，都可以通过Wrap组件来实现。希望本文能够对大家理解Flutter Wrap有所帮助。

更多范例可以查看我的[Challenge关于Wrap的例子](https://github.com/davidzou/flutter_challenge/tree/master/f_002_widgets_wrap)