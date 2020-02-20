### 如何上传一个独立的文件至Nexus3

有时我们需要统一管理一些zip文件，以作为环境配置，或者其他应用。如Gradle distribution的下载就会很慢，那么就需要添加自己的镜像服务。

> curl

```
curl -v -u <用户名>：<密码> \
   上传文件artifact.zip \
   https：// <服务器> /repository/maven-releases/com/example/artifact/1.0.0/artifact-1.0.0.zip

```

以上的命令方式仅使用文件方式，其存在着2点问题：

1. 这个文件不会生成POM文件
2. 此文件相关联的元数据不会被更新

在这种情况下使用Maven Deploy插件的[deploy-file-mojo](http://maven.apache.org/plugins/maven-deploy-plugin/deploy-file-mojo.html)将帮助我们满足上述要求。mojo能够运行在任意的目录下，而不需要pom.xml存在.

> maven

```
mvn deploy:deploy-file \
    -DgroupId=com.example \
    -DartifactId=artifact \
    -Dversion=1.0.0 \
    -Dpackaging=zip \
    -Dfile=artifact.zip \
    -DgeneratePom=true \
    -DupdateReleaseInfo=true \
    -Durl="https://${NEXUS_USERNAME}:${NEXUS_PASSWORD}@<nexus-server>/repository/maven-releases/"
```