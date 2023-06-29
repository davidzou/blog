如何改变Flutter中CheckBox未选中状态下的颜色
=======

### 起因
很早在做Demo的时候就想改变下CheckBox的未选中状态的颜色，但是多是设置全局`ThemeData.unselectedWidgetColor`的颜色来设置。
不过可以想到的就是一个App中如果有不同颜色的变态需求的话，😂。

### 解决方法

一. 当然是自己重写Widget就好了，不过稍有点复杂。放弃了，等后面有时间来弄下吧。

二. 想想觉得不会设计的这么差吧，就追了下源码。

* 既然可以通过`ThemeData.unselectedWidgetColor`设置，那么CheckBox中肯定会用到咯。

    ```
    // 第一处搜索到的
    /// {@template flutter.material.checkbox.fillColor}
      /// The color that fills the checkbox, in all [MaterialState]s.
      ///
      /// Resolves in the following states:
      ///  * [MaterialState.selected].
      ///  * [MaterialState.hovered].
      ///  * [MaterialState.focused].
      ///  * [MaterialState.disabled].
      /// {@endtemplate}
      ///
      /// If null, then the value of [activeColor] is used in the selected
      /// state. If that is also null, the value of [CheckboxThemeData.fillColor]
      /// is used. If that is also null, then [ThemeData.disabledColor] is used in
      /// the disabled state, [ThemeData.toggleableActiveColor] is used in the
      /// selected state, and [ThemeData.unselectedWidgetColor] is used in the
      /// default state.
      final MaterialStateProperty<Color?>? fillColor;
  
    // 第二处搜索到的
     MaterialStateProperty<Color> get _defaultFillColor {
        final ThemeData themeData = Theme.of(context);
        return MaterialStateProperty.resolveWith((Set<MaterialState> states) {
          if (states.contains(MaterialState.disabled)) {
            return themeData.disabledColor;
          }
          if (states.contains(MaterialState.selected)) {
            return themeData.toggleableActiveColor;
          }
          return themeData.unselectedWidgetColor;   // 就是这里返回了
        });
      }

    ```
  
* 既然`fillColor`中默认状态下使用了`unselectedWidgetColor`,那么直接设置其为自定义颜色不就完了。

* 但是很显然`fillColor`返回的对象是`MaterialStateProperty<Color?>?`, 看名字不就是一个配置文件嘛。只要按照`_defalutFilllColor`写不就好了。

```
    Checkbox(
        value: false,
        fillColor: MaterialStateProperty.resolveWith((Set<MaterialState> states) {
          const Set<MaterialState> interactiveStates = <MaterialState>{
            MaterialState.pressed,
            MaterialState.hovered,
            MaterialState.focused,
          };
          if (states.contains(MaterialState.disabled)) {
            return ThemeData.from(colorScheme: ColorScheme.light()).disabledColor;
          }
          if (states.contains(MaterialState.selected)) {
            return ThemeData().toggleableActiveColor;
          }
          if (states.any(interactiveStates.contains)) {
            return Colors.red;
          }
          // 最终返回
          return Colors.blue;
        }),
        onChanged: (value) {
        },
    ),
```

* 完工，这样就不要去设置全局变量，可以在每个CheckBox设置不同的颜色了。🚀


