从代码去认识SVG（一）
======

### 什么是SVG？
SVG（可缩放矢量图形）是一种用于描述二维矢量图形的语言。它允许您使用简单的XML语法来创建复杂的图形，包括线条、曲线、文本、形状等。

### 动手做
要使用SVG绘制图形，您需要创建一个SVG元素，并在其中定义各种图形元素。例如，要绘制一个矩形，您可以使用以下代码：

```svg
<svg width="100" height="100">
    <rect x="10" y="10" width="80" height="80" fill="blue"/>
</svg>
```
<svg width="100" height="100">
    <rect x="10" y="10" width="80" height="80" fill="blue"/>
</svg>

在上面的代码中，我们创建了一个SVG元素，并在其中定义了一个矩形。我们使用`rect`元素来绘制矩形，并使用`x`、`y`、`width`和`height`属性来指定矩形的位置和大小。我们还使用`fill`属性来指定矩形的填充颜色。

除了矩形之外，SVG还支持许多其他类型的图形元素，包括圆形（`circle`）、椭圆（`ellipse`）、线条（`line`）、折线（`polyline`）、多边形（`polygon`）和路径（`path`）。您可以使用这些元素来绘制各种复杂的图形。


这里有一些简单的SVG代码示例，用于绘制不同类型的图形：

圆形：

```svg
<svg width="100" height="100">
    <circle cx="50" cy="50" r="40" fill="red"/>
</svg>
```
<svg width="100" height="100">
    <circle cx="50" cy="50" r="40" fill="red"/>
</svg>

椭圆：

```svg
<svg width="150" height="100">
    <ellipse cx="75" cy="50" rx="70" ry="40" fill="green"/>
</svg>
```
<svg width="150" height="100">
    <ellipse cx="75" cy="50" rx="70" ry="40" fill="green"/>
</svg>

线条：

```svg
<svg width="100" height="100">
    <line x1="10" y1="10" x2="90" y2="90" stroke="blue" stroke-width="5"/>
</svg>
```

<svg width="100" height="100">
    <line x1="10" y1="10" x2="90" y2="90" stroke="blue" stroke-width="5"/>
</svg>

折线：

```svg
<svg width="100" height="100">
    <polyline points="10,10 90,10 90,90 10,90 10,10" stroke="orange" stroke-width="5" fill="none"/>
</svg>
```
<svg width="100" height="100">
    <polyline points="10,10 90,10 90,90 10,90 10,10" stroke="orange" stroke-width="5" fill="none"/>
</svg>

多边形：

```svg
<svg width="100" height="100">
    <polygon points="50,10 90,90 10,90" fill="purple"/>
</svg>
```
<svg width="100" height="100">
    <polygon points="50,10 90,90 10,90" fill="purple"/>
</svg>

路径：

```svg
<svg width="100" height="100">
    <path d="M10 10 H 90 V 90 H 10 L 50 50 Z" stroke="pink" stroke-width="5" fill="none"/>
</svg>
```
<svg width="100" height="100">
    <path d="M10 10 H 90 V 90 H 10 L 50 50 Z" stroke="pink" stroke-width="5" fill="none"/>
</svg>


您可以将这些代码复制并粘贴到HTML文件中，然后在浏览器中查看结果。您也可以使用SVG编辑器来编辑这些代码，以更改图形的颜色、大小和其他属性。
要使用SVG绘制更复杂的图形，您可以使用多个图形元素并将它们组合在一起。例如，您可以使用多个矩形、圆形和路径来创建一个复杂的图形。

### 如何使用SVG绘制组合的图形

此外，您还可以使用SVG的变换功能来旋转、缩放和移动图形。这可以让您更容易地创建复杂的图形。

下面是一个简单的示例，演示如何使用多个图形元素和变换来创建一个复杂的图形：

```svg
<svg width="200" height="200">
    <rect x="10" y="10" width="50" height="50" fill="red"/>
    <circle cx="70" cy="70" r="40" fill="green"/>
    <path d="M130 10 H 170 V 50 H 130 Z" fill="blue"/>
    <g transform="translate(100,100) rotate(45)">
        <rect x="0" y="0" width="50" height="50" fill="purple"/>
    </g>
</svg>
```
<svg width="200" height="200">
    <rect x="10" y="10" width="50" height="50" fill="red"/>
    <circle cx="70" cy="70" r="40" fill="green"/>
    <path d="M130 10 H 170 V 50 H 130 Z" fill="blue"/>
    <g transform="translate(100,100) rotate(45)">
        <rect x="0" y="0" width="50" height="50" fill="purple"/>
    </g>
</svg>

在上面的代码中，我们创建了一个矩形、一个圆形和一个路径。然后，我们使用`g`元素来创建一个组，并在其中定义一个矩形。我们使用`transform`属性来移动并旋转这个矩形。

### 如何使用SVG绘制动画

您可以使用SVG的`animate`和`animateTransform`元素来创建动画。这些元素允许您在一段时间内更改图形的属性，从而创建动画效果。

下面是一个简单的示例，演示如何使用`animateTransform`元素来创建一个旋转动画：

```svg
<svg width="100" height="100">
    <rect x="10" y="10" width="80" height="80" fill="red">
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="rotate"
                          from="0 50 50"
                          to="360 50 50"
                          dur="10s"
                          repeatCount="indefinite"/>
    </rect>
</svg>
```

<svg width="100" height="100">
    <rect x="10" y="10" width="80" height="80" fill="red">
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="rotate"
                          from="0 50 50"
                          to="360 50 50"
                          dur="10s"
                          repeatCount="indefinite"/>
    </rect>
</svg>

在上面的代码中，我们创建了一个矩形，并在其中定义了一个`animateTransform`元素。我们使用`attributeName`属性来指定要更改的属性（在这种情况下为`transform`），并使用`type`属性来指定变换类型（在这种情况下为`rotate`）。我们还使用`from`和`to`属性来指定动画的起始和结束值，以及使用`dur`属性来指定动画的持续时间。

这里有一些简单的SVG代码示例，用于创建不同类型的动画：

旋转动画：

```svg
<svg width="100" height="100">
    <rect x="10" y="10" width="80" height="80" fill="pink">
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="rotate"
                          from="0 50 50"
                          to="360 50 50"
                          dur="2s"
                          repeatCount="indefinite"/>
    </rect>
</svg>
```

<svg width="100" height="100">
    <rect x="10" y="10" width="80" height="80" fill="pink">
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="rotate"
                          from="0 50 50"
                          to="360 50 50"
                          dur="2s"
                          repeatCount="indefinite"/>
    </rect>
</svg>

缩放动画：

```svg
<svg width="100" height="100">
    <rect x="10" y="10" width="80" height="80" fill="green">
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="scale"
                          values="1;2;1"
                          dur="2s"
                          repeatCount="indefinite"/>
    </rect>
</svg>
```
<svg width="100" height="100">
    <rect x="10" y="10" width="80" height="80" fill="green">
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="scale"
                          values="1;2;1"
                          dur="2s"
                          repeatCount="indefinite"/>
    </rect>
</svg>

移动动画：

```svg
<svg width="100" height="100">
    <circle cx="30" cy="50" r="20" fill="blue">
        <animate attributeName="cx"
                 values="30;70;30"
                 dur="2s"
                 repeatCount="indefinite"/>
    </circle>
</svg>
```
<svg width="100" height="100">
    <circle cx="30" cy="50" r="20" fill="blue">
        <animate attributeName="cx"
                 values="30;70;30"
                 dur="2s"
                 repeatCount="indefinite"/>
    </circle>
</svg>

颜色动画：

```svg
<svg width="100" height="100">
    <circle cx="50" cy="50" r="40" fill="#ff0000">
        <animate attributeName="fill"
                 values="#ff0000;#00ff00;#0000ff;#ff0000"
                 dur="4s"
                 repeatCount="indefinite"/>
    </circle>
</svg>
```

<svg width="100" height="100">
    <circle cx="50" cy="50" r="40" fill="#ff0000">
        <animate attributeName="fill"
                 values="#ff0000;#00ff00;#0000ff;#ff0000"
                 dur="4s"
                 repeatCount="indefinite"/>
    </circle>
</svg>

您可以将这些代码复制并粘贴到HTML文件中，然后在浏览器中查看结果。您也可以使用SVG编辑器来编辑这些代码，以更改动画的持续时间、重复次数和其他属性。

### 如何使用SVG绘制更复杂的动画

要使用SVG绘制更复杂的动画，您可以使用多个`animate`和`animateTransform`元素并将它们组合在一起。例如，您可以使用多个动画来创建一个复杂的动画序列。

此外，您还可以使用SVG的`set`元素来控制动画的播放顺序。这可以让您更容易地创建复杂的动画。

下面是一个简单的示例，演示如何使用多个动画和`set`元素来创建一个复杂的动画：

```svg
<svg width="100" height="100">
    <rect x="10" y="10" width="80" height="80">
        <set attributeName="fill" to="red" begin="0s"/>
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="rotate"
                          from="0 50 50"
                          to="360 50 50"
                          dur="2s"
                          begin="0s"/>
        <set attributeName="fill" to="green" begin="2s"/>
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="scale"
                          values="1;2;1"
                          dur="2s"
                          begin="2s"/>
        <set attributeName="fill" to="blue" begin="4s"/>
        <animate attributeName="x" values="10;30;10" dur="2s" begin="4s"/>
        <animate attributeName="y" values="10;30;10" dur="2s" begin="4s"/>
    </rect>
</svg>
```

<svg width="100" height="100">
    <rect x="10" y="10" width="80" height="80">
        <set attributeName="fill" to="red" begin="0s"/>
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="rotate"
                          from="0 50 50"
                          to="360 50 50"
                          dur="2s"
                          begin="0s"/>
        <set attributeName="fill" to="green" begin="2s"/>
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="scale"
                          values="1;2;1"
                          dur="2s"
                          begin="2s"/>
        <set attributeName="fill" to="blue" begin="4s"/>
        <animate attributeName="x" values="10;30;10" dur="2s" begin="4s"/>
        <animate attributeName="y" values="10;30;10" dur="2s" begin="4s"/>
    </rect>
</svg>

在上面的代码中，我们创建了一个矩形，并在其中定义了多个动画和`set`元素。我们使用`set`元素来更改矩形的填充颜色，并使用`begin`属性来指定动画的开始时间。我们还定义了多个`animateTransform`和`animate`元素来创建旋转、缩放和移动动画。


这里有一个更复杂的SVG动画示例，它使用了多个动画和`set`元素来创建一个复杂的动画序列：

```svg
<svg width="200" height="200">
    <rect x="10" y="10" width="80" height="80">
        <set attributeName="fill" to="red" begin="0s"/>
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="rotate"
                          from="0 50 50"
                          to="360 50 50"
                          dur="2s"
                          begin="0s"/>
        <set attributeName="fill" to="green" begin="2s"/>
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="scale"
                          values="1;2;1"
                          dur="2s"
                          begin="2s"/>
        <set attributeName="fill" to="blue" begin="4s"/>
        <animate attributeName="x" values="10;30;10" dur="2s" begin="4s"/>
        <animate attributeName="y" values="10;30;10" dur="2s" begin="4s"/>
    </rect>
    <circle cx="150" cy="50" r="40">
        <set attributeName="fill" to="#ff00ff" begin="0s"/>
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="translate"
                          values="-20,0;20,0;-20,0"
                          dur="6s"
                          begin="0s"/>
        <set attributeName="fill" to="#00ffff" begin="6s"/>
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="rotate"
                          from="-90 150 50"
                          to="-450 150 50"
                          dur= "6s"
                          begin= "6s"/>
    </circle>
</svg>
```

<svg width="200" height="200">
    <rect x="10" y="10" width="80" height="80">
        <set attributeName="fill" to="red" begin="0s"/>
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="rotate"
                          from="0 50 50"
                          to="360 50 50"
                          dur="2s"
                          begin="0s"/>
        <set attributeName="fill" to="green" begin="2s"/>
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="scale"
                          values="1;2;1"
                          dur="2s"
                          begin="2s"/>
        <set attributeName="fill" to="blue" begin="4s"/>
        <animate attributeName="x" values="10;30;10" dur="2s" begin="4s"/>
        <animate attributeName="y" values="10;30;10" dur="2s" begin="4s"/>
    </rect>
    <circle cx="150" cy="50" r="40">
        <set attributeName="fill" to="#ff00ff" begin="0s"/>
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="translate"
                          values="-20,0;20,0;-20,0"
                          dur="6s"
                          begin="0s"/>
        <set attributeName="fill" to="#00ffff" begin="6s"/>
        <animateTransform attributeName="transform"
                          attributeType="XML"
                          type="rotate"
                          from="-90 150 50"
                          to="-450 150 50"
                          dur= "6s"
                          begin= "6s"/>
    </circle>
</svg>

在上面的代码中，我们创建了一个矩形和一个圆形，并在其中定义了多个动画和`set`元素。我们使用`set`元素来更改图形的填充颜色，并使用`begin`属性来指定动画的开始时间。我们还定义了多个`animateTransform`和`animate`元素来创建旋转、缩放、移动和平移动画。

您可以尝试运行此代码并查看效果。希望这个示例能帮助您了解如何使用SVG创建更复杂的动画。

