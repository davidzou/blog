从代码去认识SV
======

### 如何使用SVG绘制交互式图形

您可以使用SVG的交互功能来创建交互式图形。这些功能允许您在图形上添加事件监听器，以便在用户与图形交互时执行特定的操作。

例如，您可以使用`onclick`属性来添加一个鼠标单击事件监听器。当用户单击图形时，将执行指定的JavaScript代码。

下面是一个简单的示例，演示如何使用`onclick`属性来创建一个交互式图形：

```html
<svg width="100" height="100">
    <rect x="10" y="10" width="80" height="80" fill="red" onclick="changeColor(evt)"/>
</svg>

<script>
    function changeColor(evt) {
        var rect = evt.target;
        rect.setAttribute("fill", "green");
    }
</script>
```

在上面的代码中，我们创建了一个矩形，并为其添加了一个`onclick`属性。当用户单击矩形时，将调用`changeColor`函数。在此函数中，我们使用`setAttribute`方法来更改矩形的填充颜色。

这里有一个简单的交互式SVG图形示例，它使用`onclick`属性来添加一个鼠标单击事件监听器：

```html
<svg width="100" height="100">
    <rect x="10" y="10" width="80" height="80" fill="red" onclick="changeColor(evt)"/>
</svg>

<script>
    function changeColor(evt) {
        var rect = evt.target;
        var color = rect.getAttribute("fill") === "red" ? "green" : "red";
        rect.setAttribute("fill", color);
    }
</script>
```

在上面的代码中，我们创建了一个矩形，并为其添加了一个`onclick`属性。当用户单击矩形时，将调用`changeColor`函数。在此函数中，我们使用`setAttribute`方法来更改矩形的填充颜色。

要使用SVG绘制更复杂的交互式图形，您可以使用多个事件监听器并将它们组合在一起。例如，您可以使用`onclick`、`onmouseover`和`onmouseout`属性来添加鼠标单击、悬停和离开事件监听器。

此外，您还可以使用JavaScript来动态更改图形的属性和内容。这可以让您更容易的创建复杂的交互式图形。

下面是一个简单的示例，演示如何使用多个事件监听器和JavaScript来创建一个复杂的交互式图形：

```html
<svg width="200" height="200">
    <rect id="myRect" x="10" y="10" width="80" height="80" fill="red"
          onclick="changeColor()" onmouseover="grow()" onmouseout="shrink()"/>
</svg>

<script>
    function changeColor() {
        var rect = document.getElementById("myRect");
        var color = rect.getAttribute("fill") === "red" ? "green" : "red";
        rect.setAttribute("fill", color);
    }

    function grow() {
        var rect = document.getElementById("myRect");
        rect.setAttribute("width", "100");
        rect.setAttribute("height", "100");
    }

    function shrink() {
        var rect = document.getElementById("myRect");
        rect.setAttribute("width", "80");
        rect.setAttribute("height", "80");
    }
</script>
```

在上面的代码中，我们创建了一个矩形，并为其添加了多个事件监听器。当用户单击矩形时，将调用`changeColor`函数；当用户将鼠标悬停在矩形上时，将调用`grow`函数；当用户将鼠标移开矩形时，将调用`shrink`函数。在这些函数中，我们使用`setAttribute`方法来动态更改矩形的属性。

这里有一个更复杂的交互式SVG图形示例，它使用多个事件监听器和JavaScript来创建一个复杂的动画序列：

```html
<svg width="200" height="200">
    <rect id="myRect" x="10" y="10" width="80" height="80" fill="red"
          onclick="changeColor()" onmouseover="grow()" onmouseout="shrink()"/>
    <circle id="myCircle" cx="150" cy="50" r="40" fill="#ff00ff"
            onclick="changeColor()" onmouseover="grow()" onmouseout="shrink()"/>
</svg>

<script>
    function changeColor() {
        var rect = document.getElementById("myRect");
        var circle = document.getElementById("myCircle");
        var color = rect.getAttribute("fill") === "red" ? "green" : "red";
        rect.setAttribute("fill", color);
        circle.setAttribute("fill", color);
    }

    function grow() {
        var rect = document.getElementById("myRect");
        var circle = document.getElementById("myCircle");
        rect.setAttribute("width", "100");
        rect.setAttribute("height", "100");
        circle.setAttribute("r", "50");
    }

    function shrink() {
        var rect = document.getElementById("myRect");
        var circle = document.getElementById("myCircle");
        rect.setAttribute("width", "80");
        rect.setAttribute("height", "80");
        circle.setAttribute("r", "40");
    }
</script>
```

在上面的代码中，我们创建了一个矩形和一个圆形，并为它们添加了多个事件监听器。当用户单击图形时，将调用`changeColor`函数；当用户将鼠标悬停在图形上时，将调用`grow`函数；当用户将鼠标移开图形时，将调用`shrink`函数。在这些函数中，我们使用`setAttribute`方法来动态更改图形的属性。


### 总结

上面这些示例都介绍了在html中如何使用script创建一些SVG变化，您可以尝试运行这些代码并查看效果。G（二）


[//]: # (这里有一个简单的SVG动画示例，它使用`animate`元素在多个`path`之间切换：)

[//]: # ()
[//]: # (```svg)

[//]: # (<svg width="200" height="200">)

[//]: # (    <path d="M10 10 H 90 V 90 H 10 Z" fill="red">)

[//]: # (        <animate attributeName="d")

[//]: # (                 values="M10 10 H 90 V 90 H 10 Z; M30 30 H 70 V 70 H 30 Z; M50 50 H 70 V 70 H 50 Z; M10 10 H 90 V 90 H 10 Z")

[//]: # (                 dur="4s")

[//]: # (                 repeatCount="indefinite"/>)

[//]: # (    </path>)

[//]: # (</svg>)

[//]: # (```)

[//]: # ()
[//]: # (在上面的代码中，我们创建了一个`path`元素，并在其中定义了一个`animate`元素。我们使用`attributeName`属性来指定要更改的属性（在这种情况下为`d`），并使用`values`属性来指定动画的起始和结束值。我们还使用`dur`属性来指定动画的持续时间。)

[//]: # ()
[//]: # (您可以尝试运行此代码并查看效果。希望这个示例能帮助您了解如何使用SVG在多个`path`之间切换动画。)