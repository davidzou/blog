### 如何创建Fatjar(将library jar打入可执行jar中)

1. Fatjar可执行的jar 首先是要可执行，那么必须在MANIFEST.MF中声明Main-Class项，结果如下:
	
	###### META-INF/MANIFEST.MF
	```
	Manifest-Version: 1.0
  	Implementation-Title: Gradle Jar File Example
  	Implementation-Version: 1.0.0
  	Main-Class: com.example.Main
	```

2. jar可以执行了，但是依赖的jar还没有包含在可执行文件中，那么资源打包的时候必须将resoures下的jar一并打入

	```
	jar {
		// 包含所有的jar文件
		from { configurations.compile.collect { it.isDirectory() ? it : zipTree(it) } }
	}
	```

#### 配置文件如下

###### build.gradle
```
  1 apply plugin: 'java'
  2 
  3 repositories{
  4    mavenCentral()
  5    jcenter()
  6 }
  7 
  8 [compileJava,compileTestJava,javadoc]*.options*.encoding = 'UTF-8'
  9 
 10 archivesBaseName="demo"
 11 
 12 version="1.0.0"
 13 
 14 jar {
 15         manifest {
 16         attributes 'Implementation-Title': 'Gradle Jar File Example',
 17                 'Implementation-Version': version,
 18                 'Main-Class': 'com.example.Main'
 19     }
 20     // 包含所有的jar文件
 21     from { configurations.compile.collect { it.isDirectory() ? it : zipTree(it) } }
 22 }
 23 
 24 dependencies {
 25         compile files("libs/extra.jar")
 26         compile "org.apache.logging.log4j:log4j:2.2"
 27         compile "com.google.code.gson:gson:2.3.1"
 28         compile "org.slf4j:slf4j-api:1.7.8"
 29         compile "org.slf4j:slf4j-log4j12:1.7.9"
 30         compile "com.googlecode.json-simple:json-simple:1.1.1"
 31         compile "org.yaml:1.3"
 32 }
 
 ```