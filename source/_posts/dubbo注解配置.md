---
title: dubbo注解配置
date: 2016-03-28 12:01:00
category: dubbo
---
### 服务提供者

使用Service注解，注意引入的是com.alibaba.dubbo.config.annotation包下的

``` java
@Service(version="1.0.0")
public class LoginServiceImpl implements LoginService {
    ...
}
```

<!-- more -->

配置文件中开启注解扫描
``` xml
<!-- 扫描注解包路径，多个包用逗号分隔，不填pacakge表示扫描当前ApplicationContext中所有的类 -->
<dubbo:annotation package="com.feijl.provider" />
```

### 服务消费者

使用Reference注解引用，其它照旧spring方式，比如spring中的Service

``` java
@Service
public class WebLoginService {

    @Reference(version = "1.0.0")
    private LoginService loginService;
    
    ...

}
```

配置文件中开启注解扫描
``` xml
<!-- 扫描注解包路径，多个包用逗号分隔，不填pacakge表示扫描当前ApplicationContext中所有的类 -->
<dubbo:annotation package="com.feijl.consumer" />
```



