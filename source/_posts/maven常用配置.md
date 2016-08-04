---
title: maven常用配置
date: 2015-06-28 12:00:00
category: maven
---
配置属性

``` xml
<properties>
    <project.build.sourceJDK>1.7</project.build.sourceJDK>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    ...
</properties>
```

<!-- more -->

依赖管理

``` xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
        </dependency>
        ...
    </dependencies>
</dependencyManagement>
```

构建

``` xml
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <filtering>true</filtering>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <filtering>true</filtering>
            <includes>
                <include>**/*.dtd</include>
            </includes>
        </resource>
    </resources>
    <pluginManagement>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>${maven.compiler.plugin}</version>
                <configuration>
                    <source>${project.build.sourceJDK}</source>
                    <target>${project.build.sourceJDK}</target>
                    <encoding>${project.build.sourceEncoding}</encoding>
                </configuration>
            </plugin>
        </plugins>
    </pluginManagement>
    ...
</build>
```