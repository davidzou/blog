## Android Gradle Plugin 3.6.0 (2020 二月)

### 这个版本的Android插件需要:

[Gradle 5.6.4](https://docs.gradle.org/5.6.4/release-notes.html). 了解更多关于[Gradle升级](https://developer.android.com/studio/releases/gradle-plugin#updating-gradle)部分。

SDK Build Tools 28.0.3或者更高.

## 新功能
此版本的Android Gradle插件包含以下新功能。

#### View Binding

在代码中引用视图时，View binging提供了编译时安全性。现在可以用自动生成的绑定类引用替换findViewById（）。要开始使用View binding，请在<font color=red>每个模块的build.gradle</font>文件中包含以下内容：

``` 
android {
    viewBinding.enabled = true
}
```
想了解更多, 请看[View Binding文档](https://developer.android.com/topic/libraries/view-binding).

#### Maven Publish plugin 支持

Android Gradle插件包括对[Maven Publish Gradle](https://docs.gradle.org/current/userguide/publishing_maven.html)插件的支持，它允许您将构建工件发布到apachemaven仓库。Android Gradle插件为应用程序或库模块中的每个构建变体工件创建一个[组件](https://docs.gradle.org/current/userguide/dependency_management_terminology.html#sub:terminology_component)，您可以使用该组件将自定义[发布](https://docs.gradle.org/current/userguide/publishing_maven.html#publishing_maven:publications)到Maven仓库。

要了解更多信息，请转到有关如何[使用Maven Publish插件](https://developer.android.com/studio/build/maven-publish-plugin)的页面。

#### 新的默认打包工具

在构建应用程序的调试版本时，插件使用名为zipflinger的新打包工具来构建APK。这个新工具应该可以提高构建速度。如果新的打包工具不能按预期工作，请[报告一个错误](https://developer.android.com/studio/report-bugs)。要恢复旧的打包工具只需在gradle.properties文件中包含以下内容，

```
android.useNewApkCreator=false
```

#### 原生构建属性

现在可以确定Clang在项目中建立和链接每个C/C++文件所需的时间长度。Gradle可以输出一个Chrome跟踪，其中包含这些编译器事件的时间戳，这样您就可以更好地理解构建项目所需的时间。要输出此生成属性文件，请执行以下操作：

执行Gradle构建命令时添加属性 

```
 -Pandroid.enableProfileJson=true   
```

如：

```
gradlew assembleDebug -Pandroid.enableProfileJson=true
```

打开Chrome浏览器并输入chrome://tracing

单击加载按钮并导航到project-root/build/android-profile以查找文件。文件名为profile-timestamp.json.gz。

您可以在查看器顶部附近看到本机生成属性数据：

![](https://developer.android.com/studio/images/releases/native-build-attribution.png)

## 习惯改变
使用此版本的插件时，可能会遇到以下行为的变化。

#### 默认NDK版本
如果您下载了多个版本的NDK，Android Gradle插件现在将选择一个默认版本来编译源代码文件。此前，插件选择了NDK的最新下载版本。使用模块build.gradle文件中的```android.ndkVersion```属性覆盖所选插件的默认值。

#### 简化R类的生成
Android Gradle插件通过为项目中的每个库模块仅生成一个R类并与其他模块依赖项共享这些R类，简化了编译类路径。这种优化应该会导致更快的生成，但它要求您记住以下几点：

1. 因为编译器与上层模块依赖项共享R类，所以项目中的每个模块使用唯一的包名称是很重要的
2.  一个库的R类对其他项目的可见依赖是由将该库作为依赖项包含在内的配置决定。例如，如果库A包含库B作为“api”依赖项，则库A和其他依赖于库A的库可以访问库B的R类。但是，如果库A使用实现依赖项配置，则其他库可能无法访问库B的R类。要了解更多信息，请阅读[依赖配置](https://developer.android.com/studio/build/dependencies#dependency_configurations)。

#### 移除默认配置中的资源
对于库模块，如果包含的语言资源未包含在默认资源集中（例如，如果将hello_world作为字符串资源包含在/values-es/strings.xml中，但未定义） /values/strings.xml中的资源-编译项目时，Android Gradle插件不再包含该资源。 此行为更改应导致更少的“找不到资源”运行时异常，并提高了构建速度。

#### D8现在遵守CLASS注释保留策略
现在在编译你的应用程序时，D8会考虑注释何时应用CLASS保留策略，并且这些注释在运行时不再可用。 将应用程序的Target SDK设置为API级别23时，也会发生此行为，该行为以前允许在运行时使用较旧版本的Android Gradle插件和D8编译应用程序时访问这些批注。

#### 其他变化
* ```aaptOptions.noCompress``` 在所有平台上都不再区分大小写（对于APK和Bundle），并遵守使用大写字符的路径。

* 默认情况下，数据绑定现在是增量的。要了解更多信息，请参阅[问题#110061530](https://issuetracker.google.com/110061530).

* 所有单元测试，包括Roboelectric单元测试，现在都是完全可缓存的。要了解更多信息，请参阅[问题#115873047](https://issuetracker.google.com/115873047).

## 已知问题

#### 丢失Manifest类文件
如果您的应用程序在其清单中定义了自定义权限，Android Gradle插件通常会以字符串常量的方式定义这些自定义权限并生成manifest.java类。插件将这个类与你的应用程序打包在一起，这样你就可以更容易地在运行时引用这些权限。

生成清单类在Android Gradle插件3.6.0中已损坏。 如果使用此版本的插件构建应用程序，并且该应用程序引用了清单类，则可能会看到ClassNotFoundException异常。 要解决此问题，请执行以下任一操作：

* 通过标准名称引用您的自定义权限. 如: ``` "com.example.myapp.permission.DEADLY_ACTIVITY".```
* 定义自己的常量，如下所示：

```
public final class CustomPermissions {
  public static final class permission {
    public static final String DEADLY_ACTIVITY="com.example.myapp.permission.DEADLY_ACTIVITY";
  }
```

## 附录
### 版本更新对应表

|Plugin version|Required Gradle version|
|:-----|:----|
|1.0.0 - 1.1.3	| 2.2.1 - 2.3|
|1.2.0 - 1.3.1 | 2.2.1 - 2.9
|1.5.0 | 2.2.1 - 2.13
|2.0.0 - 2.1.2 | 2.10 - 2.13
|2.1.3 - 2.2.3 | 2.14.1+
|2.3.0+ | 3.3+
|3.0.0+ | 4.1+
|3.1.0+ | 4.4+
|3.2.0 - 3.2.1 | 4.6+
|3.3.0 - 3.3.2 | 4.10.1+
|3.4.0 - 3.4.1 | 5.1.1+
|3.5.0-3.5.3 | 5.4.1+
|3.6.0+ | 5.6.4+