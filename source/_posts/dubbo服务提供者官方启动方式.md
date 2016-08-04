---
title: dubbo服务提供者官方启动方式
date: 2016-03-28 12:00:00
category: dubbo
---
公司之前的dubbo项目为了省事，服务提供者直接由web容器启动，虽然dubbo项目理论上也只是需要加载其配置文件即可启动，但依赖容器运行会启动多余的web服务造成资源浪费，查看官方例子中有使用单独启动方式，逐按照dubbo-demo-provider的例子进行配置。

<!-- more -->

首先在pom文件中加入插件

``` xml
<build>
    <plugins>
        <plugin>
            <artifactId>maven-dependency-plugin</artifactId>
            <executions>
                <execution>
                    <id>unpack</id>
                    <phase>package</phase>
                    <goals>
                        <goal>unpack</goal>
                    </goals>
                    <configuration>
                        <artifactItems>
                            <artifactItem>
                                <groupId>com.alibaba</groupId>
                                <artifactId>dubbo</artifactId>
                                <version>${project.parent.version}</version>
                                <outputDirectory>${project.build.directory}/dubbo</outputDirectory>
                                <includes>META-INF/assembly/**</includes>
                            </artifactItem>
                        </artifactItems>
                    </configuration>
                </execution>
            </executions>
        </plugin>
        <plugin>
            <artifactId>maven-assembly-plugin</artifactId>
            <configuration>
                <descriptor>src/main/assembly/assembly.xml</descriptor>
            </configuration>
            <executions>
                <execution>
                    <id>make-assembly</id>
                    <phase>package</phase>
                    <goals>
                        <goal>single</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

maven-dependency-plugin插件把dubbo包中的文件（如启动脚本），解压到outputDirectory指定的目录中，然后使用assembly方式打包，assembly文件配置：

``` xml
<assembly>
	<id>assembly</id>
	<formats>
		<format>tar.gz</format>
	</formats>
	<includeBaseDirectory>true</includeBaseDirectory>
	<fileSets>
		<fileSet>
			<directory>${project.build.directory}/dubbo/META-INF/assembly/bin</directory>
			<outputDirectory>bin</outputDirectory>
			<fileMode>0755</fileMode>
		</fileSet>
		<fileSet>
			<directory>src/main/assembly/conf</directory>
			<outputDirectory>conf</outputDirectory>
			<fileMode>0644</fileMode>
			<!-- 过滤 -->
			<excludes>
				<exclude>*.xml</exclude>
			</excludes>
		</fileSet>
	</fileSets>
	<dependencySets>
		<dependencySet>
			<outputDirectory>lib</outputDirectory><!-- 将scope为runtime的依赖包打包到lib目录下。 -->
		</dependencySet>
	</dependencySets>
</assembly>
```

第一个fileSet把dubbo的启动脚本（bin目录下）输出到当前目录的bin目录下，第二个fileSet把dubbo配置文件输出到conf目录下，dependencySets把依赖的jar包输出到lib目录下。

运行maven打包命令生成.tar.gz压缩文件，之后在服务器上解压该文件，运行bin目录下的start脚本启动即可。

另外本地测试直接运行时，可以调用官方main方法启动

``` java
public static void main(String[] args) {
    com.alibaba.dubbo.container.Main.main(args);
}
```
