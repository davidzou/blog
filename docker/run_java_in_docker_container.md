如何在docker容器中运行java程序
====

随着Java版本的更新，现在还有很多人在使用不同版本，7，8，9，11，13乃至14。新的语法的添加也导致了一些变化。我们的开发环境也变得更加的复杂，导致了很多集成安装软件的出现。如：sdkman。又类似python的virtualenv，使用对应的开发环境以支持不断更新的版本变化。docker的出现让我们的开发环境的结构上出现了变化，整个将开发的方式转向了轻量级，模块化，高度的灵活性。

***

### 安装Docker

* Mac和Windows请下载Docker Desktop并安装 [请参考](https://www.docker.com/products/docker-desktop)

***

### 创建Java代码

```
    public class Main {
        public static final void main(String[] args) {
            System.out.println("Hello World! It's in Docker Container");
        }
    }
```

***

### 编写Dockerfile [^source]

这里使用openjdk，历史的原因现在openjdk和oraclejdk基本一致了。

```
FROM openjdk:7
COPY ./src /user/src
WORKDIR /user/src
RUN cd /user/src
RUN ls
RUN javac Main.java
CMD ["java", "Main"]
```

***

### 构建docker image

* ` docker build -t my-java-app .`

    创建一个名为custom-java-app的docker镜像。

    ```
    zzw:run_java_in_docker zzw$ tree .
    .
    ├── Dockerfile
    └── src
        └── Main.java

    1 directory, 2 files
    zzw:run_java_in_docker zzw$ docker build -t my-java-app .
    Sending build context to Docker daemon  3.584kB
    Step 1/7 : FROM openjdk:7
     ---> d735a2057e60
    Step 2/7 : COPY ./src /user/src
     ---> 2f73db0048b2
    Step 3/7 : WORKDIR /user/src
     ---> Running in 253f6ab329bc
    Removing intermediate container 253f6ab329bc
     ---> 4556a7649c61
    Step 4/7 : RUN cd /user/src
     ---> Running in 2ef0d5fc33af
    Removing intermediate container 2ef0d5fc33af
     ---> 2c3bc60d26ef
    Step 5/7 : RUN ls
     ---> Running in 35b1d1d275b7
    Main.java
    Removing intermediate container 35b1d1d275b7
     ---> b33876781109
    Step 6/7 : RUN javac Main.java
     ---> Running in 82a1391878d1
    Removing intermediate container 82a1391878d1
     ---> fba21944e1af
    Step 7/7 : CMD ["java", "Main"]
     ---> Running in 5a6d886b0b01
    Removing intermediate container 5a6d886b0b01
     ---> 48170726b7d4
    Successfully built 48170726b7d4
    Successfully tagged my-java-app:latest
    zzw:run_java_in_docker zzw$ di
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    my-java-app         latest              48170726b7d4        24 seconds ago      475MB
    ```

***

### 在容器中执行程序

* `docker run -it --rm --name my-running-app my-java-app`

    ```
    zzw:run_java_in_docker zzw$ docker run -it --rm --name my-running-app my-java-app
    Hello World! It's in Docker Container
    ```

***

[^source]: [相关代码](https://github.com/davidzou/WonderingWall/tree/master/challenge/run_java_in_docker)
