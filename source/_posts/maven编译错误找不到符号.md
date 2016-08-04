---
title: maven编译错误找不到符号
date: 2015-10-22 11:00:00
category: maven
---
今天从svn更新项目以后，发现一直编译不过，提示"错误: 找不到符号"

``` bash
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:2.3.2:compile (default-compile) on project v1-common-util: Compilation failure: Compilation failure:
[ERROR] \workspace\idea\v1_java\v1_new_site\v1_common\common_util\src\main\java\cn\v1\tech\common\util\HttpUtil.java:[80,12] 错误: 找不到符号
...
```

<!-- more -->

排查过依赖包的问题，也依照网上说法利用工具clean和mvn clean都无效，经过几番尝试，最后发现跟编码有关，通过配置插件解决

``` xml
<build>
    <plugins>
        <plugin>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.3</version>
            <configuration>
                <source>1.7</source>
                <target>1.7</target>
                <encoding>UTF-8</encoding>
            </configuration>
        </plugin>
    </plugins>
</build>
```
