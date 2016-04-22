## 第 5 章 Docker + Spring Boot: 快速搭建和部署Java Web应用

**0、你需要：**

* JDK 1.8 : java -version
* Maven 3.0+ : mvn -v
* Git : git --version
* Source Code : https://github.com/bingoHuang/spring-boot-docker
* Docker : docker version
    * docker-machine ls
    * docker-machine start
    * docker-machine env
    * eval $(docker-machine env)

**1、Maven编译工程**

下载源码到本地，进入工程目录，执行maven编译

    git clone https://github.com/bingoHuang/spring-boot-docker.git
    cd spring-boot-docker
    tree

```
项目结构：
├── README.md
├── pom.xml
└── src
    ├── main
    │   ├── docker
    │   │   ├── Dockerfile
    │   │   └── gs-spring-boot-docker-0.1.0.jar
    │   ├── java
    │   │   └── hello
    │   │       └── Application.java
    │   └── resources
    │       └── application.yml
    └── test
        └── java
            └── hello
                └── HelloWorldConfigurationTests.java
```

    mvn package

**2、测试Jar包执行**

执行生成的jar包，运行spring boot应用

    java -jar target/gs-spring-boot-docker-0.1.0.jar

**3、验证本地运行是否可以访问成功**

* 命令行下访问：curl http://127.0.0.1:8080/
* 浏览器中访问：http://127.0.0.1:8080/

---

**4、编写Dockerfile文件**

进入到源码的docker目录下，编写Dockerfile文件

    mkdir spring-boot-docker
    cd spring-boot-docker
    拷贝编译好的gs-spring-boot-docker-0.1.0.jar到当前目录，和Dockerfile放在同一目录

    # 编写Dockerfile文件
    FROM hub.c.163.com/xbingo/jdk8:latest
    ADD gs-spring-boot-docker-0.1.0.jar app.jar
    CMD ["java","-jar","/app.jar"]

**5、构建Dockerfile**

    docker build -t cloudcomb/springbootdocker:1.0 .

**6、查看构建的镜像**

    docker images

    REPOSITORY                       TAG                 IMAGE ID            CREATED              SIZE
    cloudcomb/springbootdocker       1.0                 c5a57ce057e7        About a minute ago   180.8 MB

**7、运行docker容器**

    docker run -p 8081:8080 -t cloudcomb/springbootdocker:1.0
    docker ps

**8、验证Docker容器运行是否可以访问成功**

* 新建一个命令行tag：command+T
* 命令行下访问：curl http://192.168.99.100:8081
* 浏览器中访问：http://192.168.99.100:8081
