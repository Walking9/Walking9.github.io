---
title: SpringMVC+IDEA
date: 2019-12-27 12:40:22
tags: [IDEA, SpringMVC, Maven, Hibernate, MySql]
---

# SpringMVC环境搭建

OS：ubuntu16.04LTS

## 准备工作

### Java环境配置

java环境配置网上的教程很多，但我觉得说的都不够清楚，简单说下自己的方法：

1、下载jdk，在usr创建jdk8目录，并将jdk移动到该目录下，使用mkdir，mv等命令，不细说；

2、配置系统环境变量（用户环境变量也是同样的道理），在文件的末尾添加信息：

```
export JAVA_HOME=/usr/jdk1.8.0_101
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib:$JRE_HOME/lib
export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
```

解释:

```
export JAVA_HOME=/usr/jdk-8是配置jdk的主目录
export JRE_HOME=$JAVA_HOME/jre是配置jre的目录
export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib:$JRE_HOME/lib是配置的CLASSPATH目录
export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin将jdk的可执行文件目录添加到系统系统环境目录中
```

所以这里需要根据自己的实际情况（路径、命名等）做相应的修改

3、使用

`$ source /etc/profile`

命令使刚才的配置生效（备注：该命令针对系统环境变量的配置）

4、用

`$ java -version`

命令验证是否正确：

![java](/images/java.png)

参考链接: [http://blog.csdn.net/oh_mourinho/article/details/52691398](http://blog.csdn.net/oh_mourinho/article/details/52691398)


### Tomcat配置

1、[官网](http://tomcat.apache.org/download-80.cgi#8.5.9)上下载对应的Tomcat，在/usr下新建Tomcat文件夹，将Tomcat移动到目录下

2、进入Tomcat的bin目录，编辑文件startup.sh，在最后一行之前加入信息：

```
#set java environment
export JAVA_HOME=/usr/java/jdk1.8.0_101
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:%{JAVA_HOME}/lib:%{JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH

#tomcat
export TOMCAT_HOME=/usr/tomcat/apache-tomcat-8.5.9
```

备注：由于Tomcat依赖java包，所以需要加入java环境变量，这个同样需要在java环境配置好的情况下，根据实际情况做对应修改，而不是简单的复制。

3、运行startup.sh

`$ sudo ./startup.sh`

然后在浏览器中输入 localhost:8080，出现一下界面，则配置正确：

![Tomcat](/images/Tomcat.png)

参考链接: [http://www.linuxidc.com/Linux/2017-06/144809.htm](http://www.linuxidc.com/Linux/2017-06/144809.htm)

## 

## 方法一：使用Maven完成搭建

> 1.maven是一个跨平台的项目管理工具。
>
> 2.它是Apache的一个开源项目，主要服务于基于Java平台的项目构建、依赖管理和项目信息管理。不重复发明轮子。
>
> 3.简单、交流与反馈、测试驱动开发(TDD)、十分钟构建、持续集成(CI)、富有信息的工作区。Maven几乎友好的支持任何软件开发方法；Maven帮助快速发布项目。

Maven的主要功能是：**项目构建；项目构建；项目依赖管理；软件项目持续集成；版本管理；项目的站点描述信息管理；**

### Maven配置

Maven 3.3.9下载地址：[http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.zip](http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.zip)

将Maven放到/opt/Maven下，再配置它的系统环境变量：

在/etc/profile文件末尾添加信息：

```
#set maven environment
export MAVEN_HOME=/opt/Maven
export PATH=$MAVEN_HOME/bin:$PATH
```

用

`$ source /etc/profile`

使之生效

用

`$ mvn -version`

验证

### 打开IDEA创建Demo

这里参考了博主TK-Xiong的博客，[这篇文章](http://blog.tk-xiong.com/archives/1227)给了我很大帮助，有关图文教程，可以直接参考这篇文章，但最后一点有个bug，我在下面进行了补充：

**理清整个流程以及背后的作用：**

1、打开IDEA，创建Maven项目；

2、由于使用了Maven创建项目，所以需要通过修改pom.xml文件里的依赖项dependencies来配置外部拓展库External Libraries；

查拓展库的网站：[http://mvnrepository.com/](http://mvnrepository.com/)

需要添加的拓展库有：

- spring-webmvc
- junit   (java单元测试框架)
- jstl  (Tomcat依赖包)

3、编码前，改变工程的目录结构；

4、修改web.xml文件：

```
- 增加了XML版本头部
- 修改了 web-app 头部
- 增加了 servlet
- 增加了 servlet-mapping
- 为了能够处理中文的post请求，再配置一个encodingFilter，以避免post请求中文出现乱码情况
```

其中，

url-pattern的值为/，用于拦截请求，并交由Spring MVC的后台控制器来处理

servlet的值是你对应的文件名，即xxx-servlet.xml

5、添加Controllers（文章MainController类）

```java
package project.controller;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

/**
 * Created by Walking on 2017/12/26.
 */

@Controller
public class MainController {

    @RequestMapping(value = "/", method = RequestMethod.GET)
    public String index() {
        model.addAttribute("param", "The is the home page");
        return "index";
    }
}
```

这段java代码是一段基本的处理路由的代码，当我们输入url为**[http://localhost:8080/home](http://localhost:8080/home)**的时候controller就回去渲染响应的**index.jsp**，并且做了一个页面传参，也就是把命名为**param**的字符串"The is the home page"传给了页面。

备注:这里的return与参考文章不同，我认为这是博主的一个错误。参照上下文就可以看出这里错在了哪：我们在xxx-servlet.xml文件中bean标签已经指明了SpringMVC去**/WEB-INF/views/**目录下去寻找jsp文件进行渲染，所以这里的return只需指明渲染这个目录下哪个文件就行了。

6、创建xxx-servlet.xml文件：

其中，

`<property name="prefix" value="/WEB-INF/pages/"/>`

是指定一下SpringMVC需要扫描的package，也就是我们等下需要新建的放置controllers的包。

```xml
<bean id="jspViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
        <property name="prefix" value="/WEB-INF/views/"/>
        <property name="suffix" value=".jsp"/>
</bean>
```

这段内容是用来支持jsp的解析，也就是说我们在controllers里面如果用return进行返回，那么SpringMVC就回去**/WEB-INF/views/**目录下去寻找**index.jsp**的文件并且进行渲染。

7、添加Views（文章index.jsp文件）：

```
<%--
    Create by IntelliJ IDEA
    User: Walking
    Date: 2017/12/16
    Time: 20:18
    To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <title>SpringMVCDemo</title>
</head>
<body>
    <h1>这里是SpringMVC Demo首页</h1>
  	<h2>${param}</h2>
    <h3>出现此页面，说明配置成功</h3>
</body>
</html>
```

其中的**${param}**就是我们在jsp文件里面获取的之前controller传过来的参数。

**–至此项目配置完成**

8、Tomcat查看项目；


## 方法二： 直接用SpringMVC框架完成搭建

参考博客：[http://blog.csdn.net/industriously/article/details/52851588](http://blog.csdn.net/industriously/article/details/52851588)

- 使用体会：

  ​        这种方法，是在使用IDEA在创建工程时，就选择使用Spring框架，而不是方法二中的选择Maven，这样有个好处就是，新建工程后，会自动添加与Spring项目开发有关的所有库，实现起来比方法一更简单，所以在此就不再罗列步骤了。

  ​    缺点：在用到Spring以外的东西不怎么灵活，除非对整个框架和其运作方式有足够的了解，显然，我就是了解的不够，准备在工程的实现过程中深入了解这方面的知识。

## + Hibernate连接MySql

参考博客：[http://blog.csdn.net/mr_sk/article/details/68947018](http://blog.csdn.net/mr_sk/article/details/68947018)

–（以上内容日后继续补充完善）

哎呀突然发现这里还给自己留了个任务，课设都熬夜做完了，在让我实现一遍截个图，好麻烦的说，博客里写的很清楚啦，我就不弄了吧^v^