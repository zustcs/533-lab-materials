# pom.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>io.ilss</groupId>
    <artifactId>web-demo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>
    <!--版本号-->
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <!-- 单元测试 -->
        <junit.version>4.12</junit.version>
        <!--1：日志-->
        <!--实现slf4j接口并整合-->
        <logback.classic.version>1.2.2</logback.classic.version>
        <!--2：数据库-->
        <mysql.connector.java.version>8.0.13</mysql.connector.java.version>
        <druid.version>1.1.1</druid.version>
        <c3p0.version>0.9.5</c3p0.version>
        <!-- DAO: MyBatis -->
        <mybatis.version>3.4.6</mybatis.version>
        <spring.mybatis.version>1.3.2</spring.mybatis.version>
        <!-- 3.Servlet web -->
        <taglibs.standard.impl.version>1.2.3</taglibs.standard.impl.version>
        <jstl.version>1.1.2</jstl.version>
        <jackson.core.version>2.9.7</jackson.core.version>
        <jackson.databind.version>2.9.7</jackson.databind.version>
        <jackson.annotation.version>2.9.7</jackson.annotation.version>
        <javax.servlet-api.version>4.0.1</javax.servlet-api.version>
        <!-- 4.Spring -->
        <!-- 1)Spring核心 -->
        <spring.context.version>5.1.0.RELEASE</spring.context.version>
        <spring.core.version>5.1.0.RELEASE</spring.core.version>
        <spring.beans.version>5.1.0.RELEASE</spring.beans.version>
        <spring.aop.version>5.1.0.RELEASE</spring.aop.version>
        <spring.context.support.version>5.1.0.RELEASE</spring.context.support.version>
        <spring.aspects.version>5.1.0.RELEASE</spring.aspects.version>
        <spring.jms.version>5.1.0.RELEASE</spring.jms.version>
        <spring.messaging.version>5.1.0.RELEASE</spring.messaging.version>
        <spring.orm.version>5.1.0.RELEASE</spring.orm.version>
        <spring.oxm.version>5.1.0.RELEASE</spring.oxm.version>
        <spring.websocket.version>5.1.0.RELEASE</spring.websocket.version>
        <!-- 2)Spring DAO层 -->
        <spring.jdbc.version>5.1.1.RELEASE</spring.jdbc.version>
        <spring.tx.version>5.1.0.RELEASE</spring.tx.version>
        <!-- 3)Spring web -->
        <spring.web.version>5.1.0.RELEASE</spring.web.version>
        <spring.webmvc.version>5.1.0.RELEASE</spring.webmvc.version>
        <spring.instrument.version>5.1.0.RELEASE</spring.instrument.version>
        <!-- 4)Spring test -->
        <spring.test.version>5.1.0.RELEASE</spring.test.version>
        <!-- redis客户端:Jedis -->
        <jedis.version>2.9.0</jedis.version>
        <protostuff.core.version>1.6.0</protostuff.core.version>
        <protostuff.runtime.version>1.6.0</protostuff.runtime.version>
        <!-- 分页-->
        <pageHelper.version>5.1.8</pageHelper.version>
        <pageFilter.version>1.9.1-RC1</pageFilter.version>
        <!-- Map工具类 -->
        <commons.collections4.version>4.2</commons.collections4.version>
        <!-- lombok -->
        <lombok.version>1.16.20</lombok.version>
        <!-- ajax -->
        <ajax.version>0.7.3</ajax.version>
        <jquery.version>3.3.1</jquery.version>
        <!-- 闲置 -->
        <freemarker.version>2.3.23</freemarker.version>
        <aspectjweaver.version>1.9.2</aspectjweaver.version>
        <commons.dbcp2.version>2.5.0</commons.dbcp2.version>
        <commons.pool2.version>2.6.0</commons.pool2.version>
        <commons.logging.version>1.2</commons.logging.version>
        <alibaba.fastjson.version>1.2.49</alibaba.fastjson.version>
        <fileupload.version>1.3.3</fileupload.version>
        <commons-io.version>2.6</commons-io.version>
    </properties>
    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.context.version}</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.core.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-beans -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.beans.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-web -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.web.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.webmvc.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-tx -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>${spring.tx.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-instrument</artifactId>
            <version>${spring.instrument.version}</version>
        </dependency>
        <!--Spring JMS：为简化jms api的使用而做的简单封装-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jms</artifactId>
            <version>${spring.jms.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-messaging</artifactId>
            <version>${spring.messaging.version}</version>
        </dependency>
        <!--Spring orm：整合第三方的orm实现，如hibernate，ibatis，jdo以及spring 的jpa实现-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>${spring.orm.version}</version>
        </dependency>
        <!--Spring oxm：Spring对于object/xml映射的支持，可以让JAVA与XML之间来回切换-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-oxm</artifactId>
            <version>${spring.oxm.version}</version>
        </dependency>
        <!--Spring websocket：提供 Socket通信， web端的推送功能-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-websocket</artifactId>
            <version>${spring.websocket.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>${javax.servlet-api.version}</version>
            <scope>provided</scope>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>${aspectjweaver.version}</version>
        </dependency>
        <!--Spring Aspects：Spring提供的对AspectJ框架的整合-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>${spring.aspects.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-aop -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>${spring.aop.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${spring.jdbc.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-test -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.test.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>${mysql.connector.java.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.apache.commons/commons-dbcp2 -->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-dbcp2</artifactId>
            <version>${commons.dbcp2.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.apache.commons/commons-pool2 -->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-pool2</artifactId>
            <version>${commons.pool2.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/commons-logging/commons-logging -->
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>${commons.logging.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>${mybatis.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>${spring.mybatis.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-context-support -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
            <version>${spring.context.support.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.freemarker/freemarker -->
        <dependency>
            <groupId>org.freemarker</groupId>
            <artifactId>freemarker</artifactId>
            <version>${freemarker.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>${jackson.databind.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>${jackson.core.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-annotations -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-annotations</artifactId>
            <version>${jackson.annotation.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/ch.qos.logback/logback-classic -->
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>${logback.classic.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.apache.taglibs/taglibs-standard-impl -->
        <dependency>
            <groupId>org.apache.taglibs</groupId>
            <artifactId>taglibs-standard-impl</artifactId>
            <version>${taglibs.standard.impl.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.servlet/jstl -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>${jstl.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>${jedis.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/io.protostuff/protostuff-core -->
        <dependency>
            <groupId>io.protostuff</groupId>
            <artifactId>protostuff-core</artifactId>
            <version>${protostuff.core.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/io.protostuff/protostuff-runtime -->
        <dependency>
            <groupId>io.protostuff</groupId>
            <artifactId>protostuff-runtime</artifactId>
            <version>${protostuff.runtime.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.apache.commons/commons-collections4 -->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-collections4</artifactId>
            <version>${commons.collections4.version}</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>${lombok.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/cljs-ajax/cljs-ajax -->
        <dependency>
            <groupId>cljs-ajax</groupId>
            <artifactId>cljs-ajax</artifactId>
            <version>${ajax.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.webjars.bower/jquery -->
        <dependency>
            <groupId>org.webjars.bower</groupId>
            <artifactId>jquery</artifactId>
            <version>${jquery.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>${druid.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.mchange/c3p0 -->
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>${c3p0.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.github.pagehelper/pagehelper -->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>${pageHelper.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.scalatra.scalate/scalate-page -->
        <dependency>
            <groupId>org.scalatra.scalate</groupId>
            <artifactId>scalate-page_2.13.0-M5</artifactId>
            <version>${pageFilter.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.0</version>
        </dependency>
        <!--Alibaba JSON-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>${alibaba.fastjson.version}</version>
        </dependency>
        <!--文件上传-->
        <dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
            <version>${fileupload.version}</version>
        </dependency>
        <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>${commons-io.version}</version>
        </dependency>
    </dependencies>

    <build>
        <finalName>SSMStandard</finalName>
        <pluginManagement>
            <plugins>
                <plugin>
                    <artifactId>maven-clean-plugin</artifactId>
                    <version>3.0.0</version>
                </plugin>
                <plugin>
                    <artifactId>maven-resources-plugin</artifactId>
                    <version>3.0.2</version>
                </plugin>
                <!-- 编译 设置 -->
                <plugin>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.7.0</version>
                    <configuration>
                        <source>${maven.compiler.source}</source>
                        <target>${maven.compiler.target}</target>
                        <encoding>${project.build.sourceEncoding}</encoding>
                    </configuration>
                </plugin>
                <plugin>
                    <artifactId>maven-surefire-plugin</artifactId>
                    <version>2.20.1</version>
                </plugin>
                <plugin>
                    <artifactId>maven-war-plugin</artifactId>
                    <version>3.2.0</version>
                </plugin>
                <plugin>
                    <artifactId>maven-install-plugin</artifactId>
                    <version>2.5.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-deploy-plugin</artifactId>
                    <version>2.8.2</version>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
</project>
```

# Java Web基础入门

## JSP入门

### 课程简介及Java Web开发环境搭建

#### Java Web开发所需技术及应用场景

Java Web是用Java技术来结局相关web互联网领域的技术总称

需要在特定的web服务器上运行，分为web服务器和web客户端两部分

跨平台，能够在多个不同平台下部署与运行

## JSP基本语法

### JSP声明语法

语法格式：

```
<%!String str = "hello world";%>
```

定义成员变量，以及成员方法

不能直接包含程序语句

**输出**

```
<%!
    String str = "hello world";

    String getStr() {
        return "hello world2";
    }
%>
<hr>
<%out.println(this.str);%>
<hr>
<%= this.getStr()%>
```

### jSP注释

语法格式：

```
<%-- Java脚本、jsp中其他代码 --%>
```

**Java语言所固有的注释语法依然使用**

### JSP内容输出表达式

语法格式：

```
<%! int i = 10;%>
<% = i %>
```

输出的变量名称不需要添加分号

```
<%-- 这是一个带有返回值的方法 --%>
<% = getInfo() %>
```

### JSP包引入语法

语法格式：

```
<%@ page import = "java.util.Date" %>
```

不同的包引用被逗号隔开，作为一个整体字符串

```
<%@ page import = "java.util.Date,java.io.*" %>
```

## JSP内置对象

### 概述

#### HTTP协议

HTTP协议————超文本传输协议

##### HTTP请求

客户端在输入某个网址访问web叫做一个请求

##### HTTP相应

服务器通过超文本传输协议将资源以文本的形式传输给我们的客户端

#### JSP内置对象简介

##### 请求与响应模式

##### JSP内置对象

内置对象（又叫隐含对象，有9个内置对象）：不需要预先声明就可以在脚本代码和表达式中随意使用

##### 作用域

作用域：pageContext、request、session、application

### request、response和out对象

#### request、response和out对象简介

request：封装了由WEB浏览器或其他客户端生成HTTP请求的细节（参数，属性，头标和数据）

out：代表输出流的对象

response：封装了返回到HTTP客户端的输出，向页面作者提供设置响应头标和状态码的方式

request作用域：用户的请求周期

#### request应用

```
logon.jsp
<form action="control.jsp">
...
<input type = "text" name = "account" />
...
<input type = "password" name = "password" />
...
 <input type = "submit" value = "登录" />
 control.jsp
 <%...
String account = request.getParameter("account");
String password = request.getParameter("password");
...%>
```

获取logon.jsp中的account和password值

#### request作用域

request作用域在相邻两个web资源之间共享同一个

request请求对象时使用

```
request.setAttribute("name","嘿嘿嘿");
request.getRequestDispatcher("result.jsp").forward(request,response);
```

将其转发到`result.jsp`页面

**若出现乱码**

```
request.setCharacterEncoding("UTF-8");
```



### `page`和`pageContex`对象

#### `pageContext`内置对象

`pageContext`：提供了转发请求到其他资源和包含其他资源的方法，提供获取其他内置对象的方法

1：forward方法来完成请求的转发

```
index.jsp
<% pageContext.forward("a.jsp?name = 嘿嘿");%>
a.jsp
<% request.getParameter("name")%>
```

2：include方法

```
<% pageContext.include("header.jsp");%>
```

可以进入`header.jsp`

3：pageContext可以来获取其他的内置对象

#### `page`内置对象

`page`:代表了正在运行的由JSP文件产生的类对象

#### `pageContext`作用域

它的作用域是当前执行页面

### `session`、`config`、`exception`对象应用

#### session对象及session作用域

**session：主要用于跟踪会话**

会话是代表用户第一次进入当前系统直到退出系统或关闭浏览器，在此期间与服务器的一系列交互

**session作用域：会话期间**

#### config

**config：获取配置信息**

#### exception

**exception：异常对象**

1：exception只能在错误页面中使用

在page设置里添加：

```
isErrorPage="true"
```

异常捕获

```
<% out.print(exception.getMessage());%>
```

2：有一个页面出现了异常，在页面中指定一个错误处理的页面，`page`指令单中的`errorpage`来制定

在测试的`jsp`文件的page中添加

```
errorPage="error.jsp"
```

#### application对象应用

application：提供了关于服务器版本，应用级初始化参数和应用内资源绝对路径方式

application作用域：web容器的生命周期

```
//application之访问量的设置
Object o = application.getAttribute("count");
if (o == null) {
    application.setAttribute("count", 1);
} else {
    //将字符串强转为整型
    int count = Integer.parseInt(o.toString());  
    application.setAttribute("count", count + 1);
}
//输出
out.print(application.getAttribute("count"));
```

#### 信息修改

相邻两个JSP页面传递数据的时候，通过URL参数的方式来传递数据

规则：

```
//传参 不能有空格！！！！！！！
资源?key = value & key= value
```

```
//修改
<%
    Map<String,emp> map = DBUTil.map;
    emp emp0 = map.get(request.getParameter("account"));
    emp0.setName(request.getParameter("name"));
    emp0.setEmail(request.getParameter("email"));
%>
```

 ## XML入门

### XML文档结构

#### XML的概念与用途

XML的全称是E**X**tensible **M**arkup **L**anguage,可扩展标记语言

编写XML就是编写**标签**，与HTML非常类似，扩展名.xml

良好的人机可读性

**XML与 HTML的比较**

1：XML与HTML非常相似，都是编写标签

2：XML没有预定义标签，HTML存在大量预定义标签

3：XML重在保存与传输数据，HTML用于显示信息

**XML的用途**

1：Java程序的配置描述文件

```
<servlet>
    <servlet-name>InitTest</servlet-name>
    <servlet-class>moreservlets.InitServlet<servlet-class/>
      <init-param>
          <param-name>param1</param-name>
          <param-value>value1</param-value>
      </init-param>
      <init-param>
          <param-name>param2</param-name>
          <param-value>2</param-value>
      </init-param>
</servlet>
```

2：用于保存程序的产生的数据

3：网络间的数据传输

#### XML文档结构

第一行必须是XML声明

有且只有一个根节点

XML标签的书写规则与HTML相同

XML声明说明XML文档的基本信息，包括版本号与字符集，写在XML第一行

```
<?xml version = "1.0" encoding = "UTF-8"?>
```

#### XML标签书写规则

**合法的标签名**

1：标签名要有意义

2：建议使用英文，小写字母，单词之间使用“-”分隔

3：建议多级标签之间不要存在重名的情况

**合理使用属性**

1：标签属性用于描述标签不可或缺的信息

2：对标签分组或者为标签设置Id时常用属性表示

**处理特殊字符**

1：标签体中，出现"<"、">"特殊字符，会破坏文档结构

**解决方法1**：使用实体引用

XML支持五种实体引用

| 实体引用 | 对应符号 | 说明   |
| -------- | -------- | ------ |
| `&lt;`   | <        | 小于   |
| `&gt;`   | >        | 大于   |
| `&amp;`  | &        | 和号   |
| `&apos;` | '        | 单引号 |
| `&quot;` | "        | 双引号 |

**解决方法2**：使用`CDATA`标签（批量处理）

`CDATA`指的是不应由XML解析器进行解析的文本数据

 从“`<![CDATA[`"开始，到"`]]>`"结束

```
<![CDATA[
<body>
    <a href = "index.html">首页</a>
</body>
]]>
```

**有序的子元素**

在XML多层嵌套的子元素中，标签前后顺序应保持一致

### XML语义约束

#### XML语意约束之DTD

XML文档结构正确，但可能不是有效的

XML语义约束有两种定义方式：DTD与XML Schema

**DTD**：**D**ocument **T**ype **D**efinition

DTD是一种简单易用的语义约束方式

DTD文件的扩展名为`.dtd`

**DTD定义节点**

利用DTD中的`<!element>`标签，我们可以定义XML文档中允许出现的节点及数量，以hr.xml为例：

```
定义hr节点下只允许出现1个employee子节点
<!ELEMENT hr (employee)>
employee节点下必须包含以下四个节点，且按顺序出现
<!ELEMENT employee (name,age,salary,department)>
定义name标签体只能是文本，#PCDATA代表文本元素
<!ELEMENT name (#pcdata)>
```

**DTD定义节点数量**

如某个子节点需要多次重复出现，则需要在子节点后增加相应的描述符

```
hr节点下最少出现1个employee子节点
<!ELEMENT hr (employee+)>
hr节点下可出现0...n个employee子节点
<!ELEMENT hr (employee*)>
hr节点下最多出现1个employee子节点
<!ELEMENT hr (employee?)>
```

**XML引用DTD文件**

在XML中使用<！DOCTYPE>标签来引用DTD文件

```
书写格式：
<!DOCTYPE 根节点 SYSTEM "dtd文件路径">
示例：
<!DOCTYPE hr SYSTEM "hr.dtd">
```

#### 创建DTD文件

```
<?xml version = '1.0' encoding = "UTF-8"
<!ELEMENT hr (employee)>
<!ELEMENT employee (name,age,salary,department)>
//针对employee里面的no属性
<!ATTLIST employee no CDATA "">
<!ELEMENT name (#PCDATA)>
<!ELEMENT age (#PCDATA)>
<!ELEMENT salary (#PCDATA)>
<!ELEMENT department (dname,address)>
<!ELEMENT dname (#PCDATA)>
<!ELEMENT address (#PCDATA)>
```

employee后面一定要加空格

#### XML Schema

XML Schema比DTD更为复杂，提供了更多功能

XML Schema提供了数据类型、格式限定、数据范围等特性

XML Schema是W3C标准

### XML文档解析及XPth语言

#### DOM模型与Dom4j

DOM定义了访问和操作XML文档的标准方法，DOM把XML文档作为树结构来查看，能够通过DOM树来读写所有元素

**Dom4j**

Dom4j是一个易用的、开源的库，用于解析XML。它应用于Java平台，具有性能优异、功能强大和极其易使用的特点

Dom4j将XML视为Document对象

XML标签被Dom4j定义为Element对象

## Servlet入门

### Servlet简介

Servlet是Java Servlet的简称，称为小服务程序或服务连接器，用Java编写的服务器端程序，主要功能在于交互式地浏览和修改数据，生成动态Web内容

**若没有servlet**

1：ctrl+alt+shift+s

2：选中libraries

3：点击顶端的一个类似加号”+“的图标

4：选择Java

5：在弹出的窗口中选择tomcat所在的目录，进入lib目录，寻找servlet-api.jar这个包

**格式**

```
xml
<servlet>
    <servlet-name>LoginServlet1</servlet-name>
    <servlet-class>servlet.LoginServlet1</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>LoginServlet1</servlet-name>
    <url-pattern>/login</url-pattern>
</servlet-mapping>
```

**在网页显示文字**

方法一：

```
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    response.getWriter().print("输入文字");    }
```

方法二：

```
在page里面导入包
import="java.io.PrintWriter"

protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    PrintWriter out=response.getWriter();
    out.append("<html>");
    out.append("<body>");
    out.append("<h1>Hello,world!</h1>");
    out.append("</body>");
    out.append("</html>");
}
```

### Servlet的生命周期

servlet生命周期分为三个阶段：

**1：初始化阶段调用**`init()`**方法**

Servlet第一次被访问的时候调用

**2：响应客户请求阶段调用**`service()`**方法**

每次请求都会被调用

**3：终止阶段调用**`destroy()`**方法**

当Servlet被销毁的时候调用

**Servlet主要用于业务逻辑处理，JSP用于展示内容**

### 请求与响应

浏览器对服务器的一次访问称之为一次请求，请求用`HttpServletRequest`对象来表示

服务器给浏览器的一次反馈称之为一次响应，响应用`HttpServletResponse`对象来表示

### `ServletContext`与`ServletConfig`

java是一门面向对象的语言，万事万物皆对象。整个`JavaWeb`工程也可以用一个对象来表示，这个对象就是`ServletContext`类型

我们可以在web.xml文件中给某一个servlet配置一些配置信息，当服务器被启动的时候，这些配置信息就会被封装到某一个`ServletConfig`对象中去。因此`ServletConfig`表示的是某一个Servlet的配置文件

### 重定向与转发的区别

实现转发调用的是`HttpServletRequest`对象中的方法，实现重定向调用的是`HttpServletResponse`对象中的方法

转发时浏览器中的`url`地址栏不会发生改变，重定向时浏览器中的`url`地址会发生改变

转发时浏览器只请求一次服务器，重定向时浏览器请求两次服务器

**跳转**

```
//跳转
        if ("admin".equals(username) && "123".equals(password)) {
            //转发，url显示.../login
            request.getRequestDispatcher("/success.jsp").forward(request,response);
        }else{
        //重定向，url显示.../fail
            response.sendRedirect("/fail.jsp");
        }
```

**跳转并传递数据**

```
java
//转发带数据给某个jsp页面
//      request.setAttribute("username", "王二麻子");
//      request.getRequestDispatcher("/Demo.jsp").forward(request,response);
//通过重定向带数据过去
        ServletContext sc = request.getServletContext();
        sc.setAttribute("goods","娃娃");
        response.sendRedirect("/Demo.jsp");
jsp
        String goods = (String) application.getAttribute("goods");
        PrintWriter out1=response.getWriter();
                out1.append(goods);
```

## MVC概述

### MVC的概述

SUN公司推出JSP技术后，同时也推荐了两种web应用程序的开发模式，一种是JSP+JavaBean模式，一种是Servlet+JSP+JavaBean模式

**JSP+JavaBean**

其中，JSP+JavaBean模式用于处理不太复杂的web程序，其中JSP处理用户请求，JavaBean用于封装和处理数据

**Servlet+JSP+JavaBean**

Servlet+JSP+JavaBean(MVC)用于处理复杂的web程序，其中Servlet用于处理用户请求，JSP用于数据显示，JavaBean用于数据封装

M：Model模型层——JavaBean

V：View视图层——JSP

C：Controller控制层——Servlet

用户的请求都提交到Servlet(C)，由C统一调度M封装和处理数据，由V进行数据的显示

## JSTL和EL表达式

### EL表达式介绍

Expression Language(表达式语言)，目的是替代JSP页面中的复杂代码

#### 语法

```
${变量名}
```

```
<body>
    姓名：${username}<br>
    年龄：${age}
</body>
```

### JSTL

JSP标准标签库

#### 与EL表达式关系

JSTL通常会与EL表达式合作实现JSP页面的编码

#### 作用

在JSP中不建议直接书写java代码

EL表达式对于复杂的数据取值会很麻烦

JSTL配合EL能简化代码书写

#### JSTL开发准备

在JSP页面添加`taglib`指令：

`<%@taglib uri = "http://java.sun.com/jsp/jstl/core" prefix = "c"%>`

#### JSTL常用标签介绍

通用标签：set、out、remove

条件标签：if、choose

迭代标签：forEach

#### 通用标签

##### set标签

将值保存到指定范围里

——将value值存储到范围为scope的变量variable中

`<c:set var="username" value="张三" scope=" scope"/>`

##### out标签

将结果输出显示

`<c:out value="value"/>`

##### remove标签

删除指定域内数据

`<c:remove var="username" scope="session"/>`

#### 条件标签

**if标签**

```
<c:if test="${example==12}">
....
</c:if>
```

**choose标签**

```
<c:choose>
    <c:when test="${age==12}">
        您的年龄为12岁
    </c:when>
    <c:otherwise>
        您的年龄不为12岁
    </c:otherwise>
</c:choose>
```

相当于if，else

#### 迭代标签

**forEach标签**

```
<c:forEach items="${lists}" var="Map">
    ${Map.shopName}:${Map.address}:${Map.price}<br>
</c:forEach>
```

其中，items为遍历对象，var为变量名

## Ajax入门

###  Ajax概述

Ajax是一种用于创建快速动态网页的技术

**特点**

通过在后台与服务器进行少量数据交换，Ajax可以使网页实现异步更新

传统的网页如果需要更新内容，必须重载整个网页页面

**basePath配置**

```
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<%
    String path = request.getContextPath();
    String basePath = request.getScheme()+".//"+request.getServerName()+request.getServerPort()+path+"/";
%>
```

**语法总结**

url：请求的地址

type：请求时数据的传递方式（常用的有get/post）

data：用来传递的数据

success：交互成功后要执行的方法

dataType：ajax接收后台数据的类型

注意事项：ajax和后台交互时，后台是不能够直接跳转到其他页面的

## Java正则表达式

### 正则表达式介绍

使用特殊的符号来作校验，目标是操作字符串。例如：手机号码、邮箱、身份证的校验等

### 正则表达式语法规则

`[abc]`        a、b或c（简单类）一个位置上或者a或者b或者c

`[a-zA-Z]`  a到z或A到Z，两头的字母包括在内（范围）

`\d`              数字：[0-9]

`X{n}`          X，恰好n次          [0-9]{2}

`X{n,}`        X，至少n次

`X{n,m}`      X，至少n次，但是不超过m次

在input属性中定义`pattern="[a-zA-Z]{6-12}"`

手机号可以定义为`pattern = "1[3578]\d{9}"`

`\D`              非数字

`\s`              空白字符

`\S`              非空白字符

`\w`              单词字符：`[a-zA-Z_0-9]`

`\W`              非单词字符

在正则表达式中"^''表示正则的起始标记，"$"表示结束标记（可以不写）

用户名只能为字母，长度为6-12位：`[a-zA-Z]{6-12}`

密码只能为数字，长度至少6位：`[0-9]{6,},\\d{6}`

手机号校验：`[1][3578]\\d{9}`

邮箱校验：`[a-zA-Z_0-9]{3,}@([a-zA-Z]+|\\d+)(\\.[a-zA-Z]+)+`

### 前台校验局限性

**前台校验优点**

1：能够对数据进行初步的筛选，减少后台服务器的压力

2：使用html5校验，比较简单易用

**前台校验弊端**

1：可以通过一些手段绕过前端的校验

**前后端混合校验**

```
doget
String username = request.getParameter("username");
...
String usernameRegex = "[a-zA-Z]{6,12}";
/*matches方法的含义是将获取过来的username和usernameRegex这个规则进行比对，满足要求返回true否则返回flase*/
boolean flag1 = username.matches(usernameRegex);
```

## 过滤器

### Java过滤器概述

#### 概述

**作用**

实现对web资源请求的拦截，完成特殊的操作，尤其是对请求的预处理

**过滤器的应用场景**

Web资源权限访问控制

请求字符集编码处理

内容敏感字符词汇过滤

响应信息压缩

#### 常见概念 

**过滤器的生命周期**

web应用程序启动时，web服务器创建Filter的实例对象，以及对象的初始化

当请求访问与过滤器关联的web资源时，过滤器拦截请求，完成指定功能

Filter对象创建后会驻留在内存，在web应用移除或服务器停止时才销毁

过滤器的创建和销毁由web服务器负责

**实现步骤**

1：编写java类实现Filter接口，并实现doFilter方法

2：在web.xml文件中对filter类进行注册，并设置所拦截的资源

**过滤器链**

在一个web应用中，多个过滤器组合起来称之为一个过滤器链

过滤器的调用顺序取决于过滤器在web.xml文件中的注册顺序

**xml配置**

```
<filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>systemName</param-name>
        <param-value>filter Encoding</param-value>
    </init-param>
    <init-param>
        <param-name>version</param-name>
        <param-value>1.0</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

**过滤器接口的定义**

```
public class CharacterEncodingFilter implements Filter {
    public void destroy() {
        System.out.println("characterEncoding Filter destroy...");
    }

    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("characterEncoding Filter doFilter...");
        //配置并获取过滤器的初始化参数
        String systemName = config.getInitParameter("systemName");
        String version = config.getInitParameter("version");
        System.out.println("systemName:" + systemName);
        System.out.println("version:" + version);
        chain.doFilter(request, response);
    }

    public void init(FilterConfig config) throws ServletException {
        this.config = config;
        System.out.println("characterEncoding Filter init...");
    }
}
```

#### 实现留言板防止中文出现乱码的设置

```
xml
<filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>charset</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
接口类
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        request.setCharacterEncoding(config.getInitParameter("charset"));
        chain.doFilter(request, response);
    }
```

## 监听器

### 常见应用场景

统计在线人数

页面访问量的统计

应用启动时完成信息初始化工作

与Spring结合

### 监听器的实现步骤

1：编写java类实现监听器接口，并实现其接口方法

2：在web.xml文件中对实现的监听器类进行注册

### Java监听器分类

#### 按监听对象

1：ServletContext对象监听器

2：HttpSession对象监听器

3：ServletRequest对象监听器

#### 按监听事件

1：域对象自身的创建和销毁事件监听器

2：域对象中属性的创建、替换和消除事件监听器

3：绑定到session中的某个对象的状态事件监听器

# Linux

## Linux基础

### 关机/重启命令

关机：shutdown -h now

重启：shutdown -r now 或 reboot

### Linux目录结构

使用ls命令查看目录结构

| /bin      | 命令存放目录         | /opt     | 第三方软件放置目录       |
| --------- | -------------------- | -------- | ------------------------ |
| **/boot** | **启动目录**         | /root    | 超级用户家目录           |
| /dev      | 设备文件存放目录     | /tmp     | 临时目录                 |
| **/etc**  | **配置文件存放目录** | /sbin    | 命令存放目录             |
| /lib      | 函数库存放目录       | /proc    | 放置数据到内存           |
| **/home** | **普通用户家目录**   | /srv     | 服务存放数据目录         |
| /mnt      | 系统挂载目录         | **/usr** | **系统软件资源目录**     |
| /media    | 媒体设备挂载目录     | **/var** | **系统相关文档内容目录** |

#### 目录管理命令

目录查看：`ls [-al][文件或目录名称]`

目录切换：`cd [目录名称]`

显示当前目录：`pwd`

##### 路径格式

绝对路径：

​     从根目录/开始写起

相对路径：

​    当前目录          .

​    上级目录          ..

​    家目录              ~

##### 目录创建、删除

目录创建：mkdir [-p] 目录名称

目录删除：rmdir [-p] 目录名称 

-p可以依次创建目录

#### 文件操作命令

##### 文件创建

创建文件：`touch 文件名`

##### 文件编辑

vi编辑器

​    命令模式

​    编辑模式

​    最后行模式

![1540113974299](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1540113974299.png)

##### 文件查看

`cat/more/less/head/tail`

##### 具体操作

vi 文件名称进入后，首先是命令模式，

命令模式下的移动靠h  j  k   l

​                                  左下上右

按a进入编辑模式

| a    | 在光标后插入     | A    | 在当前行末插入 |
| ---- | ---------------- | ---- | -------------- |
| i    | 在光标前插入     | I    | 在当前行首插入 |
| o    | 在当前行之下插入 | O    | 在上一行插入   |
| dd   | 剪切当前行       | yy   | 复制           |
| p    | 下一行黏贴       | P    | 上一行黏贴     |

使用:转到最末行模式

| :w   | 保存       | :wq     | 保存并退出 |
| ---- | ---------- | ------- | ---------- |
| :q!  | 不保存退出 | :set nu | 显示行号   |

#### 目录及文件管理命令

##### 复制、移动、删除

复制：  `cp [-r] 来源文件 目标文件`

移动：  `mv 来源文件 目标文件`

删除‘： `rm [rf] 文件或目录`

##### 查找

查找命令：         `which 命令名`

特定目录查找： `whereis 文件或目录`

查找                    `find 目录(/) [-name/user/size] 参数`

#### 用户管理

查看：       `who`

创建用户：`useradd [-g群组] 用户名`

设置密码：`passwd 用户名`

密码要超过8个字符

删除用户：`userdel[-r] 用户名`

#### 群组管理

查看群组：         `groups [用户名]`

创建群组：         `groupadd 群组名`

删除群组：         `groupdel 群组名`

用户群组修改： `usermod [-g 群组名] 用户名`

#### 权限、角色的作用

| 符号 | 权限     | 对文件的含义 | 对目录的含义 |
| ---- | -------- | ------------ | ------------ |
| r    | 读权限   | 查看文件     | 查看目录     |
| w    | 写权限   | 修改文件     | 修改目录内容 |
| x    | 执行权限 | 执行文件     | 进入目录     |

#### 权限、角色的设置

修改所有者：             `chown [-R] 用户名 文件或目录`

修改所有者和组：     `chown [-R] 用户名：组名 文件或目录`

修改所属组：             `chgrp [-R] 组名 文件或目录`

权限修改：                 `chmod [-R] xyz 文件或目录`

##### 1

x所有者权限       y所属组权限     z其他用户权限

r:4     w:2    x:1   rwx数字总和即权限

##### 2

x 角色   u g o a分别代表所有者、所属组、其他用户、所有角色

y 设置   + - = 分别代表增加、减少、设置

z 权限   r w x

#### 压缩命令、饥饿压缩命令

```
tar [-ctxzjJvf] 压缩文件 [源文件]
c打包压缩 t查看内容 x解打包解压缩
z使用gzip方式 j使用bzip2方式 J使用xz方式
v显示过程 f指定压缩包名
```

#### 软件的安装与卸载

##### 源码包安装

下载源码包（curl、wget）

解压（tar）

进入到该目录（cd）

编译前配置（./configure）

编译（make）

编译安装（make install）

##### 删除

make clean 然后直接删除目录

##### rpm包安装

下载rpm安装包

`rpm -ivh 软件包`

-i安装  -v显示详细信息   -h显示进度

查询是否安装 `rpm -q 安装包`

查询包信息  `rpm -qi 安装包`

查询安装位置 `rpm -ql 安装包`

卸载 `rpm - e 安装包`

##### yum安装管理rpm包

查询可以安装的软件包 `yum list 名称`

安装 `yum [-y] install 软件包`

-y 自动回答yes

更新 `yum [-y] update 软件包`

卸载 `yum [-y] remove 软件包`

yum安装软件包来自yum源

##### 总结

源码包同一平台都可以安装

rpm安装管理方便

yum省时省力

### redis登录

`redis-cli -h 127.0.0.1 -p 6379 -a password`

#### 打开进程 关闭进程

`ps -ef | grep redis`

`netstat -napt |grep redis`

`kill -9 1671`

#### 关闭防火墙

`systemctl stop firewalld`

#### 卸载防火墙

`yum remove firewalld`

