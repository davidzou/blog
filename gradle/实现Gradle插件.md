
# 实现Gradle插件

### 目录
1. 实践
	1. 使用插件开发插件编写插件
	2. 优先编写和使用自定义任务类型
	3. 受益于增量任务
	4. 建模类DSL的API
	5. 捕获用户输入来配置插件运行时行为
	6. 声明一个DSL配置容器
	7. 反应到插件
	8. 提供插件的默认依赖关系
	9. 分配适当的插件标识符
2. 总结


编写插件代码是高级构建作者的例行活动。该活动通常涉及编写插件实现，创建用于执行所需功能的自定义任务类型，并通过公开声明性和表达性DSL来为最终用户配置运行时行为。在本指南中，您将学习成熟的实践，使您成为一名更好的插件开发人员，以及如何使插件对使用者尽可能易用和有用。在阅读本指南之前，请先考虑完成[设计Gradle插件的指南](https://guides.gradle.org/designing-gradle-plugins/)的阅读。

阅读以下内容前应当具有如下知识基础：

* 对软件工程实践有基本的了解
* Gradle基础知识，如项目组织，任务创建和配置以及Gradle构建生命周期
* 掌握编写Java代码的知识

如果您恰好是Gradle的初学者，请首先参阅[Gradle开发入门指南](https://gradle.org/guides/#getting-started)，同时参考[Gradle用户手册](https://docs.gradle.org/4.3.1/userguide/userguide.html)进一步深入。

### 实践

1. 使用插件开发插件编写插件

	配置一个Gradle插件项目可能的需要少量的样板代码。在[Java Gradle Plugin Development plugin](https://docs.gradle.org/4.3.1/userguide/javaGradle_plugin.html)开发插件提供了这个问题的援助。开始将以下代码添加到您的build.gradle文件：
	
	###### build.gradle
	```
	plugins {
	    id 'java-gradle-plugin'
	}
	```
	通过应用插件，应用必要的插件，并添加相关的依赖关系。它还有助于在将二进制工件发布到Gradle插件门户之前验证插件元数据。每个插件项目都应该应用这个插件。

2. 优先编写和使用自定义任务类型

	Gradle任务可以定义为[ad-hoc tasks](https://guides.gradle.org/writing-gradle-tasks/)，DefaultTask具有一个或多个操作类型的简单任务定义，或者[enhanced tasks](https://docs.gradle.org/4.3.1/userguide/more_about_tasks.html)，这些任务使用自定义任务类型，并在属性帮助下公开其可配置性。一般来说，自定义任务提供了可重用性，可维护性，可配置性和可测试性的手段。当提供任务作为插件的一部分时，同样的原则是成立的。总是比特定任务更喜欢自定义任务类型。如果用户想要将更多的任务添加到构建脚本中，则您的插件的使用者也将有机会重用现有的任务类型。

	让我们说说你去实现一个插件，它通过提供自定义任务类型来进行HTTP调用，从而解决二进制存储库中依赖项的最新版本问题。自定义任务由一个插件提供，负责通过HTTP进行通信，并以XML或JSON等机器可读格式处理响应。
	
	###### LatestArtifactVersion.java
	```
	package com.company.gradle.binaryrepo;
	
	import org.gradle.api.DefaultTask;
	import org.gradle.api.tasks.Input;
	import org.gradle.api.tasks.TaskAction;
	
	public class LatestArtifactVersion extends DefaultTask {
	    private String coordinates;
	    private String serverUrl;
	
	    @Input
	    public String getCoordinates() {
	        return coordinates;
	    }
	
	    public void setCoordinates(String coordinates) {
	        this.coordinates = coordinates;
	    }
	
	    @Input
	    public String getServerUrl() {
	        return serverUrl;
	    }
	
	    public void setServerUrl(String serverUrl) {
	        this.serverUrl = serverUrl;
	    }
	
	    @TaskAction
	    public void resolveLatestVersion() {
	        System.out.println("Retrieving artifact " + coordinates + " from " + serverUrl);
	        // issue HTTP call and parse response
	    }
	}
	```
	该任务的最终用户现在可以使用不同的配置轻松创建该类型的多个任务。所有必要的，潜在复杂的逻辑完全隐藏在自定义任务实现中。
	
	###### build.gradle
	```
	import com.company.gradle.binaryrepo.LatestArtifactVersion
	
	task latestVersionMavenCentral(type: LatestArtifactVersion) {
	    coordinates = 'commons-lang:commons-lang:1.5'
	    serverUrl = 'http://repo1.maven.org/maven2/'
	}
	
	task latestVersionInhouseRepo(type: LatestArtifactVersion) {
	    coordinates = 'commons-lang:commons-lang:2.6'
	    serverUrl = 'http://my.company.com/maven2'
	}
	````

3. 有用的增量任务

	Gradle使用声明的输入和输出来确定任务是否是最新的并且需要执行任何工作。如果没有输入或输出已经改变，Gradle可以跳过这个任务。Gradle将这种机制称为[incremental build support](https://docs.gradle.org/4.3.1/userguide/more_about_tasks.html#sec:up_to_date_checks)。支持增量构建的优点是可以显着提高构建的性能。
	
	Gradle插件引入自定义任务类型是很常见的。作为插件作者，这意味着您必须使用输入或输出注释来标注任务的所有属性。强烈建议为每个任务配备信息以进行最新的检查。记住：为了使最新的检查工作正常，任务需要定义输入和输出。
	
	让我们来考虑下面的示例任务来说明。该任务在输出目录中生成给定数量的文件。写入这些文件的文本由String属性提供。
	
	###### Generate.java
	```
	import java.io.BufferedWriter;
	import java.io.File;
	import java.io.FileWriter;
	import java.io.IOException;
	
	import org.gradle.api.DefaultTask;
	import org.gradle.api.tasks.Input;
	import org.gradle.api.tasks.OutputDirectory;
	import org.gradle.api.tasks.TaskAction;
	
	public class Generate extends DefaultTask {
	    private int fileCount;
	    private String content;
	    private File generatedFileDir;
	
	    @Input
	    public int getFileCount() {
	        return fileCount;
	    }
	
	    public void setFileCount(int fileCount) {
	        this.fileCount = fileCount;
	    }
	
	    @Input
	    public String getContent() {
	        return content;
	    }
	
	    public void setContent(String content) {
	        this.content = content;
	    }
	
	    @OutputDirectory
	    public File getGeneratedFileDir() {
	        return generatedFileDir;
	    }
	
	    public void setGeneratedFileDir(File generatedFileDir) {
	        this.generatedFileDir = generatedFileDir;
	    }
	
	    @TaskAction
	    public void perform() throws IOException {
	        for (int i = 1; i <= fileCount; i++) {
	            writeFile(new File(generatedFileDir, i + ".txt"), content);
	        }
	    }
	
	    private void writeFile(File destination, String content) throws IOException {
	        BufferedWriter output = null;
	        try {
	            output = new BufferedWriter(new FileWriter(destination));
	            output.write(content);
	        } finally {
	            if (output != null) {
	                output.close();
	            }
	        }
	    }
	}
	```
	本指南的第一部分介绍了[Plugin Development plugin](https://guides.gradle.org/implementing-gradle-plugins/#plugin-development-plugin)。作为将插件应用于项目的一个附加好处，该任务将`validateTaskProperties`自动检查定制任务类型实现中定义的每个公共属性的现有输入/输出注释。

4. 类DSL的建模API

	由插件暴露的DSL应该是可读的并且易于理解。为了说明，让我们考虑一下插件提供的以下扩展。在目前的形式中，它提供了用于配置创建网站的“平面”属性列表。
	
	###### build.gradle
	```
	apply plugin: SitePlugin
	
	site {
	    outputDir = file('build/mysite')
	    websiteUrl = 'http://gradle.org'
	    vcsUrl = 'https://github.com/gradle-guides/gradle-site-plugin'
	}
	```
	随着暴露的属性数量增加，您可能需要引入一个嵌套的，更具表现力的结构。以下代码片段添加了一个名为customData扩展的一部分的新配置块。您可能已经注意到它提供了这些属性的意思的更强的指示。
	
	###### build.gradle
	```
	apply plugin: SitePlugin
	
	site {
	    outputDir = file('build/mysite')
	
	    customData {
	        websiteUrl = 'http://gradle.org'
	        vcsUrl = 'https://github.com/gradle-guides/gradle-site-plugin'
	    }
	}
	```
	实现这种扩展的支持对象是相当容易的。首先，你需要引入一个新的数据对象来管理属性websiteUrl和vcsUrl。
	
	###### CustomData.java
	```
	public class CustomData {
	    private String websiteUrl;
	    private String vcsUrl;
	
	    public void setWebsiteUrl(String websiteUrl) {
	        this.websiteUrl = websiteUrl;
	    }
	
	    public String getWebsiteUrl() {
	        return websiteUrl;
	    }
	
	    public void setVcsUrl(String vcsUrl) {
	        this.vcsUrl = vcsUrl;
	    }
	
	    public String getVcsUrl() {
	        return vcsUrl;
	    }
	}
	```
	在扩展中，您需要创建一个CustomData类的实例以及一个可以将捕获的值委托给数据实例的方法。配置底层数据对象定义了[org.gradle.api.Action](https://docs.gradle.org/4.3.1/javadoc/org/gradle/api/Action.html)类型的参数。以下示例演示了Action在扩展定义中的使用。
	
	###### SiteExtension.java
	```
	import java.io.File;
	import org.gradle.api.Action;
	
	public class SiteExtension {
	    private File outputDir;
	    private final CustomData customData = new CustomData();
	
	    public void setOutputDir(File outputDir) {
	        this.outputDir = outputDir;
	    }
	
	    public File getOutputDir() {
	        return outputDir;
	    }
	
	    public CustomData getCustomData() {
	        return customData;
	    }
	
	    public void customData(Action<? super CustomData> action) {
	        action.execute(customData);
	    }
	}
	```
	如果您需要二级或三级嵌套，则还需要添加带有的重载Closure，因为Gradle目前无法嵌套扩展。

	#### 扩展与约定
	一些Gradle核心插件在所谓的“公约”的帮助下提供了可配置性。[Convention](https://docs.gradle.org/4.4/javadoc/org/gradle/api/plugins/Convention.html)是一个扩展的前面的概念，并服务于类似的目的。这两个概念之间的主要区别在于，Convention不允许定义名称空间来模拟类似DSL的API，这使得难以与Gradle核心DSL区分开来。Convention编写新的插件时请避免使用这个概念。长期计划是迁移所有的Gradle核心插件来使用扩展并Convention完全删除这个概念。
	
	有些情况下需要您与使用的Gradle核心插件进行交互`Convention's. You can access the registered convention objects by calling the method Project.getConvention()`。注册插件的特定约定实现可以通过提供约定类来检索`Convention.getPlugin(Class)`。[示例代码](https://guides.gradle.org/implementing-gradle-plugins/#convention-api-usage-example)引用了`JavaPluginConvention`由Java插件公开的检查配置值。

5. 捕获用户输入来配置插件运行时行为

	插件通常会使用默认约定来对消费项目做出合理的假设。例如，Java插件在目录中搜索Java源文件`src/main/java`。默认约定有助于简化项目布局，但在处理自定义项目结构，旧项目需求或不同的用户首选项时不起作用。
	
	插件应该公开一种重新配置默认运行时行为的方式。“[首选编写和使用自定义任务类型](https://guides.gradle.org/implementing-gradle-plugins/#writing-and-using-custom-task-types)”一节介绍了一种实现可配置性的方法：通过为任务属性声明设置方法。解决问题的更复杂的解决方案是公开扩展。扩展通过自定义DSL捕获用户输入，完全混合到由Gradle内核公开的DSL中。
	
	以下示例应用了一个插件，该插件公开名称的扩展名`binaryRepo`以捕获服务器URL：
	
	###### build.gradle
	```
	apply plugin: BinaryRepositoryVersionPlugin
	
	binaryRepo {
	    serverUrl = 'http://my.company.com/maven2'
	}
	```
	我们假设你也想用serverUrl一次捕获的值做一些事情。在许多情况下，暴露的扩展属性直接映射到实际使用该值时执行工作的任务属性。为避免评估顺序问题，您应该使用Gradle 4.0中引入的[公共API](https://docs.gradle.org/4.3.1/userguide/custom_plugins.html#mapExtensionPropertiesToTaskProperties) `Property`。
	
	让我们来看看插件的内部，`BinaryRepositoryVersionPlugin`给你一个更好的主意。该插件创建类型的扩展名，`BinaryRepositoryExtension`并将扩展属性映射`serverUrl`到任务属性`serverUrl`.
	
	####### BinaryRepositoryVersionPlugin.java
	```
	import org.gradle.api.Action;
	import org.gradle.api.Plugin;
	import org.gradle.api.Project;
	
	public class BinaryRepositoryVersionPlugin implements Plugin<Project> {
	    public void apply(Project project) {
	        BinaryRepositoryExtension extension = project.getExtensions().create("binaryRepo", BinaryRepositoryExtension.class, project);
	
	        project.getTasks().create("latestArtifactVersion", LatestArtifactVersion.class, new Action<LatestArtifactVersion>() {
	            public void execute(LatestArtifactVersion latestArtifactVersion) {
	                latestArtifactVersion.setServerUrl(extension.getServerUrlProvider());
	            }
	        });
	    }
	}
	```
	代替使用一个普通的`String`类型，扩展定义的字段`serverUrl`类型`Property<String>`。该字段在类的构造函数中初始化。它的状态可以通过暴露的setter方法来设置。
	
	###### BinaryRepositoryExtension.java
	```
	import org.gradle.api.Project;
	import org.gradle.api.provider.Property;
	import org.gradle.api.provider.Provider;
	
	public class BinaryRepositoryExtension {
	    private final Property<String> serverUrl;
	
	    public BinaryRepositoryExtension(Project project) {
	        serverUrl = project.getObjects().property(String.class);
	    }
	
	    public String getServerUrl() {
	        return serverUrl.get();
	    }
	
	    public Provider<String> getServerUrlProvider() {
	        return serverUrl;
	    }
	
	    public void setServerUrl(String serverUrl) {
	        this.serverUrl.set(serverUrl);
	    }
	}
	```
	任务属性也定义了`serverUrl`包含类型`Property`。它允许映射属性的状态，而不需要实际访问它的值直到需要处理 - 在任务操作中。
	
	###### LatestArtifactVersion.java
	```
	import org.gradle.api.DefaultTask;
	import org.gradle.api.provider.Property;
	import org.gradle.api.provider.Provider;
	import org.gradle.api.tasks.Input;
	import org.gradle.api.tasks.TaskAction;
	
	public class LatestArtifactVersion extends DefaultTask {
	    private final Property<String> serverUrl;
	
	    public LatestArtifactVersion() {
	        serverUrl = getProject().getObjects().property(String.class);
	    }
	
	    @Input
	    public String getServerUrl() {
	        return serverUrl.get();
	    }
	
	    public void setServerUrl(String serverUrl) {
	        this.serverUrl.set(serverUrl);
	    }
	
	    public void setServerUrl(Provider<String> serverUrl) {
	        this.serverUrl.set(serverUrl);
	    }
	
	    @TaskAction
	    public void resolveLatestVersion() {
	        // Access the raw value during the execution phase of the build lifecycle
	        System.out.println("Retrieving latest artifact version from URL " + getServerUrl());
	
	        // do additional work
	    }
	}
	```
	我们鼓励插件开发人员尽快将他们的插件迁移到公共API。不是基于Gradle 4.0的插件可能会继续使用内部的“约定映射”API。请注意，“惯例映射”API没有记录，可能会在更高版本的Gradle中删除。

6. 声明一个DSL配置容器

	有时您可能想要公开一种方式让用户定义多个相同类型的命名数据对象。为了说明的目的，我们考虑下面的构建脚本。
	
	###### build.gradle
	```
	apply plugin: ServerEnvironmentPlugin
	
	environments {
	    dev {
	        url = 'http://localhost:8080'
	    }
	
	    staging {
	        url = 'http://staging.enterprise.com'
	    }
	
	    production {
	        url = 'http://prod.enterprise.com'
	    }
	}
	```
	由插件公开的DSL暴露了定义一组环境的容器。每个由用户配置的环境都有一个任意的声明性名称，并用自己的DSL配置块来表示。上面的例子实例化了一个开发，分期和生产环境，包括其各自的URL。
	
	显然，这些环境中的每一个都需要在代码中具有数据表示来捕获这些值。环境的名称是不可变的，可以作为构造函数参数传入。目前，数据对象存储的唯一其他参数是一个URL。下面`ServerEnvironment`显示的POJO 满足这些要求。
	
	###### ServerEnvironment.java
	```
	public class ServerEnvironment {
	    private final String name;
	    private String url;
	
	    public ServerEnvironment(String name) {
	        this.name = name;
	    }
	
	    public String getName() {
	        return name;
	    }
	
	    public void setUrl(String url) {
	        this.url = url;
	    }
	
	    public String getUrl() {
	        return url;
	    }
	}
	```
	Gradle公开便捷方法[Project.html＃container（java.lang.Class）](https://docs.gradle.org/4.3.1/javadoc/org/gradle/api/Project.html#container(java.lang.Class))来创建一个数据对象的容器。该方法所采用的参数是表示数据的类。所创建的[NamedDomainObjectContainer](https://docs.gradle.org/4.3.1/javadoc/org/gradle/api/NamedDomainObjectContainer.html)类型的实例可以通过将其添加到具有特定名称的扩展容器而暴露给最终用户。
	
	###### ServerEnvironmentPlugin.java
	```
	import org.gradle.api.*;
	
	public class ServerEnvironmentPlugin implements Plugin<Project> {
	    @Override
	    public void apply(Project project) {
	        NamedDomainObjectContainer<ServerEnvironment> serverEnvironmentContainer = project.container(ServerEnvironment.class);
	        project.getExtensions().add("environments", serverEnvironmentContainer);
	
	        serverEnvironmentContainer.all(new Action<ServerEnvironment>() {
	            public void execute(ServerEnvironment serverEnvironment) {
	                String env = serverEnvironment.getName();
	                String capitalizedServerEnv = env.substring(0, 1).toUpperCase() + env.substring(1);
	                String taskName = "deployTo" + capitalizedServerEnv;
	                Deploy deployTask = project.getTasks().create(taskName, Deploy.class);
	
	                project.afterEvaluate(new Action<Project>() {
	                    public void execute(Project project) {
	                        deployTask.setUrl(serverEnvironment.getUrl());
	                    }
	                });
	            }
	        });
	    }
	}
	```
	插件在插件实现中后处理捕获的值是非常普遍的，例如配置任务。在上面的示例中，将为每个由用户配置的环境动态创建部署任务。

7. 反应到插件

	在构建中配置现有插件和任务的运行时行为是Gradle插件实现中的常见模式。例如，一个插件可以假定它被应用到一个基于Java的项目，并自动重新配置标准的源目录。
	
	###### InhouseConventionJavaPlugin.java
	```
	import java.util.Arrays;
	
	import org.gradle.api.Plugin;
	import org.gradle.api.Project;
	import org.gradle.api.plugins.JavaPlugin;
	import org.gradle.api.plugins.JavaPluginConvention;
	import org.gradle.api.tasks.SourceSet;
	
	public class InhouseConventionJavaPlugin implements Plugin<Project> {
	    public void apply(Project project) {
	        project.getPlugins().apply(JavaPlugin.class);
	        JavaPluginConvention javaConvention =
	            project.getConvention().getPlugin(JavaPluginConvention.class);
	        SourceSet main = javaConvention.getSourceSets().getByName(SourceSet.MAIN_SOURCE_SET_NAME);
	        main.getJava().setSrcDirs(Arrays.asList("src"));
	    }
	}
	```
	这种方法的缺点是，它会自动强制项目应用Java插件，因此强加一个强烈的意见。实际上，应用插件的项目甚至可能不处理Java代码。而不是自动应用Java插件，这个插件可以对消费项目应用Java插件的事实做出反应。只有在这种情况下，才会应用某些配置。
	
	######InhouseConventionJavaPlugin.java
	```
	import java.util.Arrays;
	
	import org.gradle.api.Action;
	import org.gradle.api.Plugin;
	import org.gradle.api.Project;
	import org.gradle.api.plugins.JavaPlugin;
	import org.gradle.api.plugins.JavaPluginConvention;
	import org.gradle.api.tasks.SourceSet;
	
	public class InhouseConventionJavaPlugin implements Plugin<Project> {
	    public void apply(Project project) {
	        project.getPlugins().withType(JavaPlugin.class, new Action<JavaPlugin>() {
	            public void execute(JavaPlugin javaPlugin) {
	                JavaPluginConvention javaConvention =
	                    project.getConvention().getPlugin(JavaPluginConvention.class);
	                SourceSet main = javaConvention.getSourceSets().getByName(SourceSet.MAIN_SOURCE_SET_NAME);
	                main.getJava().setSrcDirs(Arrays.asList("src"));
	            }
	        });
	    }
	}
	```
	如果没有充分的理由假设消费项目具有预期的设置，则应该优先考虑对插件进行反应，而不是盲目地应用其他插件。相同的概念适用于任务类型。
	
	###### InhouseConventionWarPlugin.java
	```
	import org.gradle.api.Action;
	import org.gradle.api.Plugin;
	import org.gradle.api.Project;
	import org.gradle.api.tasks.bundling.War;
	
	public class InhouseConventionWarPlugin implements Plugin<Project> {
	    public void apply(Project project) {
	        project.getTasks().withType(War.class, new Action<War>() {
	            public void execute(War war) {
	                war.setWebXml(project.file("src/someWeb.xml"));
	            }
	        });
	    }
	}
	```

8. 提供插件的默认依赖关系

	插件的实现有时需要使用外部依赖。您可能希望使用Gradle的依赖管理机制自动下载工件，并稍后在插件中声明的任务类型的操作中使用它。理想情况下，插件实现不需要询问用户的依赖关系的坐标 - 它可以简单地预先定义一个合理的默认版本。
	
	我们来看一个例子。您编写了一个插件，用于下载包含数据的文件以供进一步处理。插件实现声明了一个自定义配置，允许使用[assigning those external dependencies with default dependency coordinates.](https://docs.gradle.org/4.3.1/userguide/dependency_management.html#sec:configuration_defaults)。
	
	###### DataProcessingPlugin.java
	```
	import org.gradle.api.Action;
	import org.gradle.api.Plugin;
	import org.gradle.api.Project;
	import org.gradle.api.artifacts.Configuration;
	import org.gradle.api.artifacts.DependencySet;
	
	public class DataProcessingPlugin implements Plugin<Project> {
	    public void apply(Project project) {
	        final Configuration config = project.getConfigurations().create("dataFiles")
	            .setVisible(false)
	            .setDescription("The data artifacts to be processed for this plugin.");
	
	        config.defaultDependencies(new Action<DependencySet>() {
	            public void execute(DependencySet dependencies) {
	                dependencies.add(project.getDependencies().create("com.company:data:1.4.6"));
	            }
	        });
	
	        project.getTasks().withType(DataProcessing.class, new Action<DataProcessing>() {
	            public void execute(DataProcessing dataProcessing) {
	                dataProcessing.setDataFiles(config);
	            }
	        });
	    }
	}
	```
	###### DataProcessing.java
	```
	import org.gradle.api.DefaultTask;
	import org.gradle.api.file.ConfigurableFileCollection;
	import org.gradle.api.file.FileCollection;
	import org.gradle.api.tasks.InputFiles;
	import org.gradle.api.tasks.TaskAction;
	
	public class DataProcessing extends DefaultTask {
	    private final ConfigurableFileCollection dataFiles;
	
	    public DataProcessing() {
	        dataFiles = getProject().files();
	    }
	
	    @InputFiles
	    public FileCollection getDataFiles() {
	        return dataFiles;
	    }
	
	    public void setDataFiles(FileCollection dataFiles) {
	        this.dataFiles.setFrom(dataFiles);
	    }
	
	    @TaskAction
	    public void process() {
	        System.out.println(getDataFiles().getFiles());
	    }
	}
	```
	现在，这种方法对于最终用户来说非常方便，因为不需要主动声明一个依赖关系。该插件已经提供了关于这个实现细节的所有知识。但是如果用户想重新定义默认的依赖关系呢？没问题...该插件还公开了可用于分配不同依赖项的自定义配置。有效地，覆盖默认的依赖关系。
	
	###### build.gradle
	```
	apply plugin: DataProcessingPlugin
	
	dependencies {
	    dataFiles 'com.company:more-data:2.6'
	}
	```
	你会发现这个模式适用于需要外部依赖的任务，当任务的动作被实际执行时。该方法主要用于执行外部Ant任务的自定义任务，如许多[Gradle核心静态分析插件](https://docs.gradle.org/4.3.1/userguide/standard_plugins.html#sec:software_development_plugins)，例如`FindBugs`和`Checkstyle`插件。事实上，这些插件甚至通过暴露扩展属性（例如toolVersion 在[JaCoCo](https://docs.gradle.org/4.3.1/dsl/org.gradle.testing.jacoco.plugins.JacocoPluginExtension.html#org.gradle.testing.jacoco.plugins.JacocoPluginExtension:toolVersion)插件中）进一步抽象用于外部依赖的版本。

9. 分配适当的插件标识符

	一个描述性的插件标识符使消费者可以很容易地将该插件应用到项目中。这个ID应该反映一个词的插件的目的。另外，应该添加域名以避免其他具有相似功能的插件之间的冲突。在前面的章节中，代码示例中显示的依赖关系使用组ID com.company。我们可以使用与域名相同的标识符。如果您没有与法律实体合作，或者想要发布开源插件，那么您可以使用托管源代码的域名，例如com.github。
	
	将多个插件作为单个JAR工件的一部分发布时（如“ [功能与约定](https://guides.gradle.org/designing-gradle-plugins/#capabilities-vs-conventions) ”部分中所述）应使用相同的命名约定。对于可以通过标识符注册的插件数量没有限制，并且可以作为将相关插件组合在一起的好方法。为了说明，Gradle Android插件在目录中定义了两个不同的插件`src/main/resources/META-INF/gradle-plugins`。
	
	```
	.
	└──src
	    └── main
	        └── resources
	            └── META-INF
	                └── gradle-plugins
	                    ├── com.android.application.properties
	                    └── com.android.library.properties
	```        
            
2.总结

编写插件并不一定很难。利用正确的技术，您可以轻松克服常见的挑战，并实现可维护，可重复使用，声明式，良好记录和测试的插件。本指南中提供的所有提出的建议和配方可能适用于您的插件或您的使用案例。但是，提出的解决方案应该可以帮助您朝着正确的方向前进。

随着新功能在Gradle内核中的出现，本指南的内容将随着时间的推移而不断扩展。请在[Gradle论坛](https://discuss.gradle.org/)上告知我们，如果您在插件中执行特定用例仍然有困难，或者您想查看本指南中涵盖的其他用例，


[原文源自](https://guides.gradle.org/implementing-gradle-plugins/)

同时可以通过快速工具 [(Koi)](https://github.com/davidzou/koi) 创建插件项目