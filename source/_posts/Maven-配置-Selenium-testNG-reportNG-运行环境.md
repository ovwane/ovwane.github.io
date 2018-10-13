---
title: Maven 配置 Selenium + testNG + reportNG 运行环境
date: 2018-10-12 10:23:34
tags:
---



```xml
 <!-- maven 参数配置，这里引用不同的testng.xml -->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <xmlFileName>testng.xml</xmlFileName>
    </properties>
 
    <!-- maven 引用依赖不同的jar -->
    <dependencies>
 
        <!-- 依赖testNg -->
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>6.9.10</version>
            <scope>test</scope>
        </dependency>
 
        <!-- 依赖reportNg 关联testNg -->
        <dependency>
            <groupId>org.uncommons</groupId>
            <artifactId>reportng</artifactId>
            <version>1.1.4</version>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.testng</groupId>
                    <artifactId>testng</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
 
        <!-- 依赖Guice -->
        <dependency>
            <groupId>com.google.inject</groupId>
            <artifactId>guice</artifactId>
            <version>3.0</version>
            <scope>test</scope>
        </dependency>
 
        <!-- 依赖Selenium驱动包 -->
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>2.52.0</version>
            <scope>compile</scope>
            <!--
            scope标签中对应值的解释：
                * compile，缺省值，适用于所有阶段，会随着项目一起发布。
                * provided，类似 compile，期望 JDK、容器或使用者会提供这个依赖。如 servlet.jar。
                * runtime，只在运行时使用，如 JDBC 驱动，适用运行和测试阶段。
                * test，只在测试时使用，用于编译和运行测试代码。不会随项目发布。
                * system，类似 provided，需要显式提供包含依赖的 jar， Maven 不会在 Repository 中查找它。
             -->
        </dependency>
 
    </dependencies>
 
 
    <build>
        <plugins>
            <!-- 添加插件 关联testNg.xml -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.17</version>
                <configuration>
                    <suiteXmlFiles>
                        <suiteXmlFile>res/${xmlFileName}</suiteXmlFile>
                    </suiteXmlFiles>
                </configuration>
            </plugin>
 
            <!-- 添加插件,添加ReportNg的监听器，修改最后的TestNg的报告 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <properties>
                        <property>
                            <name>usedefaultlisteners</name>
                            <value>false</value>
                        </property>
                        <property>
                            <name>listener</name>
                            <value>org.uncommons.reportng.HTMLReporter</value>
                        </property>
                    </properties>
                    <workingDirectory>target/</workingDirectory>
                    <!-- <forkMode>always</forkMode> -->
                </configuration>
            </plugin>
        </plugins>
    </build>
```

```
【注意：】上面配置如果报错，需要在项目下新建一个目录"res/testng.xml"，他会去这个目录读取指定的XML
```

testng.xml配置如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
<suite name="Default suite">
    <test verbose="2" name="Default test">
        <classes>
            <class name="com.jase.test.BaiduTest" />
        </classes>
    </test> <!-- Default test -->
</suite> <!-- Default suite -->
```



## 参考

[Maven 配置 Selenium + testNG + reportNG 运行环境 - ﹏猴子请来的救兵 - 博客园](https://www.cnblogs.com/yyhh/p/5226955.html)

[一、搭建TestNG+Maven+IDEA接口测试环境 - 简书](https://www.jianshu.com/p/f76d04de982b)