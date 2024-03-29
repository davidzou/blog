Flutter BackdropFilter扩展
=======

BackdropFilter是Flutter中一个非常有用的组件，它允许开发人员在应用程序中创建模糊效果。本文将基于前篇的内容更详细的介绍一些常见用例。

### 模糊滤镜
BackdropFilter是一个Flutter组件，它可以让您在应用程序中创建模糊效果。它通过使用ImageFilter类来实现这一点。

要使用BackdropFilter，您需要导入dart:ui库，并创建一个ImageFilter对象。然后，您可以使用ImageFilter.blur方法来创建一个模糊效果，并将其应用于BackdropFilter。

下面是一个简单的示例，演示如何使用BackdropFilter来创建模糊效果：

```dart
import 'dart:ui';

class BackdropFilterExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Stack(
      children: <Widget>[
        Positioned.fill(child: Image.asset('image.png')),
        Positioned.fill(
          child: BackdropFilter(
            filter: ImageFilter.blur(sigmaX: 6, sigmaY: 6),
          ),
        ),
        Positioned.fill(child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'Hello, World!',
              style: TextStyle(
                fontSize: 32,
                color: Colors.white,
                fontWeight: FontWeight.bold,
              ),
            ),
            SizedBox(height: 16),
            Image.asset('logo.png', width: 100),
          ],
        )),
      ],
    );
  }
}
```

在上面的代码中，我们使用Stack将一个Image和一个BackdropFilter叠加在一起。然后，我们使用ImageFilter.blur方法创建一个模糊效果，并将其应用于BackdropFilter。最后添加一个文字和logo图片置于最顶层一次展现模糊背景的效果。

___运行结果如下___

![](../images/backdropfilter.png)

BackdropFilter通常与Stack组件一起使用，以便在其他组件的上层添加模糊效果。例如，您可以在一个图像的上层添加一个BackdropFilter，以创建一个模糊背景。

### 矩阵变换滤镜

除了模糊效果之外，ImageFilter类还提供了其他类型的滤镜，如矩阵变换和颜色滤镜。您可以使用这些滤镜来创建各种复杂的视觉效果。

例如，您可以使用`ImageFilter.matrix`方法来创建一个矩阵变换滤镜。这种滤镜允许您对图像进行旋转、缩放、倾斜和其他类型的变换，我们平时对图片的滤镜都是源于matrix的操作原理。

下面是一个简单的示例，演示如何使用`ImageFilter.matrix`方法来创建一个矩阵变换滤镜：

```dart
import 'dart:ui';

class MatrixFilterExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final matrix = Matrix4.identity()
      ..rotateZ(0.9)
      ..scale(1.2, 1.2);

    return Stack(
      children: <Widget>[
        Positioned.fill(child: FlutterLogo()),
        Positioned.fill(
          child: BackdropFilter(
            filter: ImageFilter.matrix(matrix.storage),
            child: Container(
              width: 100,
              height: 100,
              // color: Colors.black54,
            ),
          ),
        ),
      ],
    );
  }
}
```

___运行结果如下___
![](../images/backdropfilter_extra_1.png)

在上面的代码中，我们使用`Matrix4`类来创建一个变换矩阵。然后，我们使用`rotateZ`和`scale`方法来旋转和缩放图像。最后，我们将这个矩阵传递给`ImageFilter.matrix`方法，以创建一个矩阵变换滤镜。

### 组合滤镜
除了矩阵变换滤镜之外，您还可以使用`ImageFilter.compose`方法来组合多个滤镜。这允许您创建复杂的滤镜效果，例如同时应用模糊和颜色滤镜。

```dart
import 'dart:ui';

class ComposeFilterExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final matrix = Matrix4.identity()
      ..rotateZ(0.5)
      ..scale(1.2, 1.2);

    final matrixFilter = ImageFilter.matrix(matrix.storage);
    final blurFilter = ImageFilter.blur(sigmaX: 6, sigmaY: 6);
    final combinedFilter = ImageFilter.compose(outer: matrixFilter, inner: blurFilter);

    return Stack(
      children: <Widget>[
        Positioned.fill(child: FlutterLogo()),
        Positioned.fill(
          child: BackdropFilter(
            filter: combinedFilter,
            child: Container(
              width: 100,
              height: 100,
            ),
          ),
        ),
      ],
    );
  }
}
```

___运行结果如下___
![](../images/backdropfilter_extra_2.png)

在上面的代码中，我们分别创建了一个矩阵变换滤镜和一个模糊滤镜。然后，我们使用`ImageFilter.compose`方法将这两个滤镜组合在一起，以创建一个复合滤镜。

您可以尝试运行这些代码并查看效果。希望这些示例能帮助您了解如何使用Flutter创建不同类型的滤镜效果。


参考文献：
[1] Flutter API文档：[BackdropFilter class](https://api.flutter.dev/flutter/widgets/BackdropFilter-class.html)
[2] Flutter API文档：[ImageFilter class](https://api.flutter.dev/flutter/dart-ui/ImageFilter-class.html)
