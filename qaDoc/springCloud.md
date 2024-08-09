## Maven 配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.pxai</groupId>
    <artifactId>pxai-cloud</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>auth</module>
        <module>data</module>
        <module>msg</module>
        <module>common</module>
        <module>eureka01</module>
    </modules>

    <properties>
        <spring-boot.version>2.3.12.RELEASE</spring-boot.version>
        <spring-cloud.version>Hoxton.SR12</spring-cloud.version>
        <maven.mybatis>2.2.2</maven.mybatis>
        <maven.mybatis.plus>3.5.1</maven.mybatis.plus>
        <maven.mysql>8.0.28</maven.mysql>
        <maven.redis>2.6.2</maven.redis>
        <maven.gson>2.9.0</maven.gson>
        <maven.poi>4.1.2</maven.poi>
        <maven.junit>4.9</maven.junit>
        <maven.lombok>1.16.18</maven.lombok>
        <maven.sys.common>1.0</maven.sys.common>

        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencyManagement>
        <dependencies>
            <!--spring boot-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--spring cloud-->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--data-->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${maven.mysql}</version>
            </dependency>
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>${maven.mybatis}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-data-redis</artifactId>
                <version>${maven.redis}</version>
            </dependency>
            <!--utils-->
            <dependency>
                <groupId>com.google.code.gson</groupId>
                <artifactId>gson</artifactId>
                <version>${maven.gson}</version>
            </dependency>
            <dependency>
                <groupId>org.apache.poi</groupId>
                <artifactId>poi</artifactId>
                <version>${maven.poi}</version>
            </dependency>
            <dependency>
                <groupId>org.apache.poi</groupId>
                <artifactId>poi-ooxml</artifactId>
                <version>${maven.poi}</version>
            </dependency>
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>${maven.lombok}</version>
                <optional>true</optional>
            </dependency>
            <!--test-->
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>${maven.junit}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <profiles>
        <profile>
            <id>dev</id>
            <properties>
                <profile.active>dev</profile.active>
            </properties>
        </profile>
        <profile>
            <id>release</id>
            <properties>
                <profile.active>release</profile.active>
            </properties>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
        </profile>
    </profiles>
</project>
```



## 注册中心

* eureka 

  1. Maven

  ```xml
  <dependencies>
    <!--eureka server-->
    <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
    </dependency>
  </dependencies>
  ```

  2. springboot yml

     ```yaml
     eureka:
       instance:
         hostname: localhost
       client:
         register-with-eureka: false
         fetch-registry: false
         service-url:
           defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
     ```

## 服务调用

* feign

  1. Maven

  ```xml
  <!--openfeign-->
  <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
  </dependency>
  <!--eureka client-->
  <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
  </dependency>
  ```

  2. springboot yml

     ```yaml
     eureka:
       instance:
         hostname: localhost
       client:
         #false表示不向注册中心注册自己。
         register-with-eureka: true
         #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
         fetch-registry: true
         service-url:
           #集群指向其它eureka,#单机就是自己
           defaultZone: http://${eureka.instance.hostname}:7001/eureka/
     #设置feign客户端超时时间(OpenFeign默认支持ribbon)
     ribbon:
       #指的是建立连接所用的时间，适用于网络状况正常的情况下,两端连接所用的时间
       ReadTimeout: 5000
       #指的是建立连接后从服务器读取到可用资源所用的时间
       ConnectTimeout: 5000
     ```

  3. springboot bean

     ```java
      @Bean
      public RestTemplate restTemplate() {
      	return new RestTemplate();
      }
     ```

## 服务熔断

## 网关

## 分布式配置中心

## 分布式事务