---
title: jsp常用标签
date: 2015-06-04 15:05:03
category: jsp
tags:
---
### 标签

c标签引入

``` jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
```

<!-- more -->

静态include

``` jsp
<%@ include file="page.jsp" %>
```

### 语句

forEach

``` jsp
<c:forEach items="${list}" var="v" varStatus="status">
    ...
</c:forEach>
```

switch

``` jsp
<c:choose>
    <c:when test="${result == 1}">
        ...
    </c:when>
    <c:otherwise>
        ...
    </c:otherwise>
</c:choose>
```

设置变量

``` jsp
<c:set var="str" value="abc" scope="request"/>
```

