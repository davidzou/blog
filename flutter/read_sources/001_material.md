读源码，学Flutter
=========

## 起因
一直有读源码的习惯，但是重来就没有归类过，其中也会涉及到其他的很多知识点，所以也重来就只知有这么回事，而不能很好的系统的说出个所以然来。故此决定一边写一边归类总结。

## Material

位置 ：$FLUTTER_HOME/packages/flutter/lib/src/material/material.dart

这里我们来看注释怎么说（原文见附录1）

解释 ： 一种材料，一个元素。

辩证 : 很抽象的解释，那么就直接认为他就是原子，分子不就好了。原子和分子是更细化的分类，那么Material是原材料的意思，即是我们看到的实物，即所见即所得的东西，原子和分子是我们看不到的。

解释 ： Material widget提共了3种功能， 第一种裁剪，第二种叫提升（鬼话，不理解），第三种叫墨迹效应（更鬼话）

辩证 ： 怎么理解呢，比方说，有一张A4纸，你可以裁剪它吧，好了第一种来了，剪裁的手段方式方法最终导致的结果也会不一样的。
第二种提升也就是用制图的话说，它有正视图，俯视图，侧视图。那么从俯视图的角度看即从Z轴的方向去看它，对方在一起的A4纸是不是会有出入，当然不可能完全叠加的。
第三种墨迹效果，其实学美术的会更好理解，他就是一个物体的阴影面，不对，应该是光照效果，当一个物体被光照射时，就会出现亮面和暗面，就是这个效果。

解释 ： 材料的比喻

辩证 ： Material 作为 Material Design的中心抽象层。每种现有的材料都有一个立面，这会影响该材质与其他材质在视觉上的关系以及该材质投射阴影的方式
大多数用户界面元素要么概念性地印在一块材料上，要么本身由材料制成。材质使用[InkSplash]和[InkHighlight]效果对用户输入做出反应。要触发材料上的反应，请使用通过[material.of]获得的[MaterialInkController]。

通常，[材料]的特征不应随时间变化（例如，[材料]不应改变其[颜色]、[阴影颜色]或[类型]）。对[elevation]、[shadowColor]和[SurfaceTitColor]的更改在[animationDuration]中设置动画。
如果[type]不是[MaterialType.transparency]，并且支持上一个和下一个[shape]值之间的[ShapeOrder.lerp]，则对[shape]进行动画更改。形状更改也为[animationDuration]设置动画。

解释 ：形状

辩证 ：材料的形状由[形状]、[类型]和[边界半径（倒角）]决定。
            - 如果[形状]非空，则确定形状。
            - 如果[shape]为null且[borderRadius]为非null，则该形状为圆角矩形，角点由[Borders Radius]指定。
            - 如果[shape]和[borderRadius]为空，[type]确定形状如下：
                - [MaterialType.canvas]: 默认材质形状为矩形。
                - [MaterialType.card]: 默认材质形状是一个具有圆角边缘的矩形。边缘半径由[kMaterialEdges]指定。
                - [MaterialType.circle]: 默认材质形状为圆形。
                - [MaterialType.button]: 默认材质形状是一个具有圆角边缘的矩形。边缘半径由[kMaterialEdges]指定。
                - [MaterialType.transparency]: 默认材质形状为矩形。


解释 ：边框

辩证 ：如果[形状]不为null，则也将绘制其边界（如果有）。

解释 ：绘画

辩证 ：材质小部件通常会在其最近的材质上触发反应。例如，[ListTile.hoverColor]触发当指针悬停在其上时，平铺的材质。这些反应将是如果在它们和材料之间有任何小部件以这种方式绘制，
则会被遮挡模糊材料的方式（例如设置[BoxEdition.color]一个[装饰盒]。为了避免这种行为，请使用[InkDecoration]进行装饰材料本身。

相关类
[Ink][InkFeature][InkSplash][InkHighlight]

附录1

```
/// A piece of material.
///
/// The Material widget is responsible for:
///
/// 1. Clipping: If [clipBehavior] is not [Clip.none], Material clips its widget
///    sub-tree to the shape specified by [shape], [type], and [borderRadius].
///    By default, [clipBehavior] is [Clip.none] for performance considerations.
///    See [Ink] for an example of how this affects clipping [Ink] widgets.
/// 2. Elevation: Material elevates its widget sub-tree on the Z axis by
///    [elevation] pixels, and draws the appropriate shadow.
/// 3. Ink effects: Material shows ink effects implemented by [InkFeature]s
///    like [InkSplash] and [InkHighlight] below its children.
///
/// ## The Material Metaphor
///
/// Material is the central metaphor in material design. Each piece of material
/// exists at a given elevation, which influences how that piece of material
/// visually relates to other pieces of material and how that material casts
/// shadows.
///
/// Most user interface elements are either conceptually printed on a piece of
/// material or themselves made of material. Material reacts to user input using
/// [InkSplash] and [InkHighlight] effects. To trigger a reaction on the
/// material, use a [MaterialInkController] obtained via [Material.of].
///
/// In general, the features of a [Material] should not change over time (e.g. a
/// [Material] should not change its [color], [shadowColor] or [type]).
/// Changes to [elevation], [shadowColor] and [surfaceTintColor] are animated
/// for [animationDuration]. Changes to [shape] are animated if [type] is
/// not [MaterialType.transparency] and [ShapeBorder.lerp] between the previous
/// and next [shape] values is supported. Shape changes are also animated
/// for [animationDuration].
///
/// ## Shape
///
/// The shape for material is determined by [shape], [type], and [borderRadius].
///
///  - If [shape] is non null, it determines the shape.
///  - If [shape] is null and [borderRadius] is non null, the shape is a
///    rounded rectangle, with corners specified by [borderRadius].
///  - If [shape] and [borderRadius] are null, [type] determines the
///    shape as follows:
///    - [MaterialType.canvas]: the default material shape is a rectangle.
///    - [MaterialType.card]: the default material shape is a rectangle with
///      rounded edges. The edge radii is specified by [kMaterialEdges].
///    - [MaterialType.circle]: the default material shape is a circle.
///    - [MaterialType.button]: the default material shape is a rectangle with
///      rounded edges. The edge radii is specified by [kMaterialEdges].
///    - [MaterialType.transparency]: the default material shape is a rectangle.
///
/// ## Border
///
/// If [shape] is not null, then its border will also be painted (if any).
///
/// ## Layout change notifications
///
/// If the layout changes (e.g. because there's a list on the material, and it's
/// been scrolled), a [LayoutChangedNotification] must be dispatched at the
/// relevant subtree. This in particular means that transitions (e.g.
/// [SlideTransition]) should not be placed inside [Material] widgets so as to
/// move subtrees that contain [InkResponse]s, [InkWell]s, [Ink]s, or other
/// widgets that use the [InkFeature] mechanism. Otherwise, in-progress ink
/// features (e.g., ink splashes and ink highlights) won't move to account for
/// the new layout.
///
/// ## Painting over the material
///
/// Material widgets will often trigger reactions on their nearest material
/// ancestor. For example, [ListTile.hoverColor] triggers a reaction on the
/// tile's material when a pointer is hovering over it. These reactions will be
/// obscured if any widget in between them and the material paints in such a
/// way as to obscure the material (such as setting a [BoxDecoration.color] on
/// a [DecoratedBox]). To avoid this behavior, use [InkDecoration] to decorate
/// the material itself.
///
/// See also:
///
///  * [MergeableMaterial], a piece of material that can split and re-merge.
///  * [Card], a wrapper for a [Material] of [type] [MaterialType.card].
///  * <https://material.io/design/>
```

***以上理解仅代表个人经验，有问题请指出。***