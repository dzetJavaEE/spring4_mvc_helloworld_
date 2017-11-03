# spring4_mvc_helloworld_

---

来源：https://www.java2blog.com/spring-mvc-hello-world-example/

---
开发环境：intellij idea
构建方式：maven
本地服务器：tomcat 7

代码：https://github.com/dzetJavaEE/spring4_mvc_helloworld_

> 实测，可行

![SpringHelloWorldExample2.png](http://upload-images.jianshu.io/upload_images/3858745-25611ee8aced802d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


---

#### 1 maven pom.xml

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>test</groupId>
  <artifactId>spring4_mvc_helloworld_java2blog_</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>spring4_mvc_helloworld_java2blog_ Maven Webapp</name>
  <url>http://maven.apache.org</url>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                    <source>${jdk.version}</source>
                    <target>${jdk.version}</target>
                </configuration>
            </plugin>
        </plugins>

    </build>
    <properties>
        <spring.version>4.2.1.RELEASE</spring.version>
        <jdk.version>1.7</jdk.version>
    </properties>

</project>

```

---

#### 2 controller and view

```

package controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class HelloWorldController {
 
 @RequestMapping("/helloworld")
 public ModelAndView hello() {
 
  String helloWorldMessage = "Hello world from java2blog!";
  return new ModelAndView("hello", "message", helloWorldMessage);
 }
}

```

> http请求途径的第一个位置是 web.xml中的dispatcherServlet，[转发控制器]

> dispatcher 判断当前该去哪个 @Controller

> 中的哪个 @RequestMapping("/xxxxxxxxxxx")

====

> url 访问 上面创建好的 HelloWorldController
```

http://localhost:8080/SpringMVCHelloWorldExample/helloworld
// 或者
http://localhost:8080/SpringMVCHelloWorldExample/helloworld.html

```

![ModelAndViewContructor.jpg](http://upload-images.jianshu.io/upload_images/3858745-e124e14d1fa0ce4f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> 修改index.jsp

```

<%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>HelloWorld</title>
</head>
<body>
<a href="helloworld.html">点击这里去阅读 hello message </a>
</body>
</html>

```
> 创建 hello.jsp ，在 /WEB-INF/ 文件夹

> hello.jsp

```

<%@ page language="java" contentType="text/html; charset=UTF-8"
 pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Hello</title>
</head>
<body>
${message}
</body>
</html>

```

#### 3 配置 web.xml xxx-servlet.xml

> 注意这里的DispatcherServlet，对应的 servlet-name

> web.xml
```

<web-app xmlns="http://java.sun.com/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
 http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">
    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>

    <!-- spring 转发控制器 -->
    <servlet>
        <servlet-name>spring4mvc</servlet-name>
        <servlet-class>
            org.springframework.web.servlet.DispatcherServlet
        </servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>spring4mvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>

```

> 在 /WEB-INF 文件夹下面，创建 spring4mvc-servlet.xml

> /WEB-INF 是受服务器保护的，，，

> 注意上面的DispatcherServlet，对应的 servlet-name

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <context:component-scan base-package="controller"/>

    <bean
            class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix">
            <value>/WEB-INF/</value>
        </property>
        <property name="suffix">
            <value>.jsp</value>
        </property>
    </bean>
    <mvc:annotation-driven/>

</beans>
```

> context:component-scan 追踪并加载 spring 注解类。 
> [这里加载的就是HelloWorldController]

> InternalResourceViewResolver 是资源视图解析器类 , 前缀和后缀 会添加至 ModelViewObject 中 viewName 参数.
>  "hello" 是一个 viewName 在 ModelAndView 对象中. 
> 它会在视图中搜索 /WEB-INF/hello.jsp.

![SpringPrefixSuffix.png](http://upload-images.jianshu.io/upload_images/3858745-33ecd650535f3b57.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


---

#### 4 运行在服务器


![SpringHelloWorldExample1.png](http://upload-images.jianshu.io/upload_images/3858745-016c42c2e2c0d16a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![SpringHelloWorldExample2.png](http://upload-images.jianshu.io/upload_images/3858745-0554adb830b8856e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---

end
