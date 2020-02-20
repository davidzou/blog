### 配置自定义的Gradle wrapper distributionUrl

当使用AndroidStudio第一次创建项目的时候，或者改变wrapper版本的时候，需要从Gradle服务器去下载相应版本的的执行文件。但是由于网络的问题，导致长时间卡顿，无法直接进入的尴尬情形。

解决此种的方式不外呼2种，本地文件和网络文件（良好的网络为前提）

1. 本地文件

	事先将gradle文件下载，可以是gradle-x.x-all.zip, 也可以是gradle-x.x-bin.zip, 将下载好的文件拷贝到gradle/wrapper/目录下, 
	
	###### gradle-wrapper.properties
	```
	distributionBase=GRADLE_USER_HOME
	distributionPath=wrapper/dists
	zipStoreBase=GRADLE_USER_HOME
	zipStorePath=wrapper/dists
	distributionUrl=gradle-4.2-bin.zip
	```
	
2. 使用网络文件方式添加。（即设置Gradle distribution镜像）

	###### gradle-wrapper.properties
	```
	distributionBase=GRADLE_USER_HOME
	distributionPath=wrapper/dists
	zipStoreBase=GRADLE_USER_HOME
	zipStorePath=wrapper/dists
	distributionUrl=http\://localhost:8081/nexus/content/repositories/gradle-distribution/gradle-4.2-bin.zip
	```
	
	<font color=#f00>注：冒号前面需要添加转义符'\' </font>
	
### 如何在Sonartype Nexus上配置Gradle distribution镜像
1. Add --> Proxy repsitorynew

	![](images/sonartype_nexus_gradle_mirror.jpg)

2. 设置 gradle-wrapper.properties 

	```
		distributionUrl=http\://localhost:8081/nexus/content/repositories/gradle-distribution/gradle-4.2-bin.zip
	```
	
3. 使用命令行执行

	```
	gradlew --gradle-distribution-url "http\://localhost:8081/nexus/content/repositories/gradle-distribution/gradle-4.2-bin.zip" clean build
	```