---
title: REST Assured 学习笔记
date: 2019-01-10 10:19:28
tags:
---

# REST Assured

>REST Assured 3.2.0

[REST Assured](http://rest-assured.io/)

https://github.com/rest-assured/rest-assured

[REST Assured 3.2.0 API](http://www.javadoc.io/doc/io.rest-assured/rest-assured/3.2.0)



- [Getting Started](https://github.com/rest-assured/rest-assured/wiki/GettingStarted)
- [Usage](https://github.com/rest-assured/rest-assured/wiki/Usage)

[pom.xml](https://mvnrepository.com/artifact/io.rest-assured/rest-assured/3.2.0)

```xml
<dependency>
    <groupId>io.rest-assured</groupId>
    <artifactId>rest-assured</artifactId>
    <version>3.2.0</version>
    <scope>test</scope>
</dependency>
```



导入包

```java
import static io.restassured.RestAssured.*
import static io.restassured.matcher.RestAssuredMatchers.*
import static org.hamcrest.Matchers.*
import static io.restassured.module.jsv.JsonSchemaValidator.*
import static io.restassured.module.mockmvc.RestAssuredMockMvc.*
```



## 参考

[Testing RESTful Services in Java: Best Practices](https://blog.philipphauer.de/testing-restful-services-java-best-practices/)