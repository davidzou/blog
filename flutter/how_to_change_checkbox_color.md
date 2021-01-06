如何改变Flutter中CheckBox未选中状态下的默认颜色
===========

### 寻找

如下根据Checkbox类的定义，并未找到相对应的值

```
  const Checkbox({
    Key key,
    @required this.value,
    this.tristate = false,
    @required this.onChanged,
    this.mouseCursor,
    this.activeColor,
    this.checkColor,
    this.focusColor,
    this.hoverColor,
    this.materialTapTargetSize,
    this.visualDensity,
    this.focusNode,
    this.autofocus = false,
  }) : assert(tristate != null),
       assert(tristate || value != null),
       assert(autofocus != null),
       super(key: key); 
```

### 设置

在全局的样式中找到unselectedWidgetColor这个值。

```
    return MaterialApp(
      title: 'Color List',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        visualDensity: VisualDensity.adaptivePlatformDensity,
        unselectedWidgetColor: Colors.white,  // 这里设置颜色
      ),
    );
 
```