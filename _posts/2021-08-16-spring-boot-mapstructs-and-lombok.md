---
layout: post
ads: true
comments: true
published: true
title: Spring boot mapstruct and lombok
categories:
  - linux
  - program
  - tooltips
tags:
  - springboot
  - lombok
  - mapstruct
---
### Ref:

- https://stackoverflow.com/questions/65112406/intellij-idea-mapstruct-java-internal-error-in-the-mapping-processor-java-lang
- https://stackoverflow.com/questions/47676369/mapstruct-and-lombok-not-working-together
- https://github.com/mapstruct/mapstruct/issues/2271

### Issues

- IntelliJ Idea mapstruct java: Internal error in the mapping processor: java.lang.NullPointerException

```
The solution is to update MapStruct to 1.4.1.Final or later version, see this issue for more details.

You can also add -Djps.track.ap.dependencies=false at File | Settings (Preferences on macOS) | Build, Execution, Deployment | Compiler | Build process VM options as a workaround.
```

- MapStruct and Lombok not working together

	- Opt1: using lombok version older than 1.18.14 
	- Opt2: lombok-mapstruct-binding


```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.12.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <artifactId>spring-boot-jpa</artifactId>

    <properties>
        <lombok.version>1.18.12</lombok.version>
        <mapstruct.version>1.3.1.Final</mapstruct.version>
        <lombok-mapstruct.version>0.2.0</lombok-mapstruct.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- mapstruct dependencies-->
        <dependency>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct</artifactId>
            <version>${mapstruct.version}</version>
        </dependency>
        <dependency>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct-processor</artifactId>
            <version>${mapstruct.version}</version>
            <scope>provided</scope>
        </dependency>
<!--        <dependency>-->
<!--            <groupId>org.projectlombok</groupId>-->
<!--            <artifactId>lombok-mapstruct-binding</artifactId>-->
<!--            <version>${lombok-mapstruct.version}</version>-->
<!--        </dependency>-->

        <!--spring jpa-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>
</project>
```
