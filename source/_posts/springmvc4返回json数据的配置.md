---
title: springmvc4返回json数据的配置
date: 2015-09-20 12:00:00
category: springmvc
---
升级到springmvc4后，发现照3.x的配置方式会报

`Java.lang.NoClassDefFoundError: com/fasterxml/jackson/core/JsonProcessingException`

需要更换依赖包

<!-- more -->

``` xml
<dependencies>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-annotations</artifactId>
        <version>2.5.0</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-core</artifactId>
        <version>2.5.0</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.5.0</version>
    </dependency>
</dependencies>
```

dispatcherServlet.xml配置

``` xml
<bean id="stringConverter" class="org.springframework.http.converter.StringHttpMessageConverter">
    <property name="supportedMediaTypes">
        <list>
            <value>text/plain;charset=UTF-8</value>
        </list>
    </property>
</bean>
<bean id="jsonConverter" class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"></bean>
<bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter">
    <property name="messageConverters">
        <list>
            <ref bean="stringConverter" />
            <ref bean="jsonConverter" />
        </list>
    </property>
</bean>
```

由于默认使用jackson，也可以省略上面相关bean配置，在controller中需要返回json数据的方法上加上@ResponseBody注解就可以了

顺便提一下，3.x可以使用fastjson包，springmvc4中没有找到相应使用方式，贴一下3.x的配置

``` xml
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.4</version>
    </dependency>
</dependencies>
```

``` xml
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="false">
        <bean id="fastJsonHttpMessageConverter" class="com.alibaba.fastjson.support.spring.FastJsonHttpMessageConverter">
            <property name="supportedMediaTypes">
                <list>
                    <value>text/html;charset=UTF-8</value>
                    <value>application/json;charset=UTF-8</value>
                </list>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```


