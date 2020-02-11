# Spring概述

![1541056815460](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541056815460.png)

## loC技术

轻量级，一站式，开发框架

Ioc：控制反转

**控制什么？**

对象的依赖         Dependency Injection(DI，依赖注入)

**谁来控制？**

对象的提供者

对象的使用者？            IoC容器

## AOP

AOP：面向切面编程

## 作用

Spring框架是一个**开发工具**，能进行**框架整合**，提升**开发效率**

## 配置代码

需要有web.xml,spring_mvc.xml,applicationContext.xml以及实现类

首先是web.xml

```
<web-app>
    <display-name>SSM Web Application</display-name>

    <!--<error-page>-->
    <!--<error-code>404</error-code>-->
    <!--<location>404.jsp</location>-->
    <!--</error-page>-->
    <!--<error-page>-->
    <!--<error-code>500</error-code>-->
    <!--<location>500.jsp</location>-->
    <!--</error-page>-->

    <!--log4j2-->
    <!--<context-param>-->
        <!--<param-name>log4jContextName</param-name>-->
        <!--<param-value>star</param-value>-->
    <!--</context-param>-->
    <!--<context-param>-->
        <!--<param-name>log4jConfiguration</param-name>-->
        <!--<param-value>classpath:config/log4j2.xml</param-value>-->
    <!--</context-param>-->
    <!--<listener>-->
        <!--<listener-class>org.apache.logging.log4j.web.Log4jServletContextListener</listener-class>-->
    <!--</listener>-->
    <!--<filter>-->
        <!--<filter-name>log4jServletFilter</filter-name>-->
        <!--<filter-class>org.apache.logging.log4j.web.Log4jServletFilter</filter-class>-->
    <!--</filter>-->
    <!--<filter-mapping>-->
        <!--<filter-name>log4jServletFilter</filter-name>-->
        <!--<url-pattern>/*</url-pattern>-->
        <!--<dispatcher>REQUEST</dispatcher>-->
        <!--<dispatcher>FORWARD</dispatcher>-->
        <!--<dispatcher>INCLUDE</dispatcher>-->
        <!--<dispatcher>ERROR</dispatcher>-->
    <!--</filter-mapping>-->


    <!-- 配置 Spring -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!-- 防止Spring内存溢出监听器 -->
    <listener>
        <listener-class>org.springframework.web.util.IntrospectorCleanupListener</listener-class>
    </listener>

    <!-- 配置springmvc -->
    <servlet>
        <servlet-name>springMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring_mvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springMVC</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!-- 字符集过滤器 -->
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

</web-app>
```

spring_mvc.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="
    http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-3.0.xsd
    http://www.springframework.org/schema/mvc
    http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd">

    <!-- 默认的注解映射的支持 -->
    <mvc:annotation-driven/>

    <!-- 自动扫描该包，使SpringMVC认为包下用了@controller注解的类是控制器 -->
    <context:component-scan base-package="com.test"/>

    <!--&lt;!&ndash;&lt;!&ndash; 定义跳转的文件的前后缀 ，视图模式配置 &ndash;&gt;&ndash;&gt;-->
    <!--<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">-->
        <!--<property name="prefix" value="/WEB-INF/views/"/>-->
        <!--<property name="suffix" value=".jsp"/>-->
    <!--</bean>-->

    <!--&lt;!&ndash;避免IE执行AJAX时，返回JSON出现下载文件 &ndash;&gt;-->
    <!--<bean id="mappingJacksonHttpMessageConverter"-->
          <!--class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">-->
        <!--<property name="supportedMediaTypes">-->
            <!--<list>-->
                <!--<value>text/html;charset=UTF-8</value>-->
            <!--</list>-->
        <!--</property>-->
    <!--</bean>-->

    <!-- 启动SpringMVC的注解功能，完成请求和注解model的映射 -->
    <!--<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">-->
        <!--<property name="messageConverters">-->
            <!--<list>-->
                <!--<ref bean="mappingJacksonHttpMessageConverter"/>    &lt;!&ndash; JSON转换器 &ndash;&gt;-->
            <!--</list>-->
        <!--</property>-->
    <!--</bean>-->

    <!-- 添加注解驱动 -->
    <!--<mvc:annotation-driven enable-matrix-variables="true"/>-->

    <!-- 允许对静态资源文件的访问 -->
    <!--<mvc:default-servlet-handler/>-->

    <!--上传 org.springframework.web.multipart.commons.CommonsMultipartResolver-->
    <!--<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">-->
        <!--<property name="defaultEncoding" value="utf-8"/>-->
        <!--<property name="maxUploadSize" value="10485760000"/>-->
        <!--<property name="maxInMemorySize" value="409600"/>-->
    <!--</bean>-->

</beans>
```

applicationContext.xml

```
<!--<?xml version="1.0" encoding="UTF-8"?>-->
<beans xmlns= "http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd ">

    <context:annotation-config/>
    <!-- 自动扫描web包 ,将带有注解的类 纳入spring容器管理 -->
    <context:component-scan base-package="com.test"/>

</beans>
```

实现类

```
package com.test;

import org.springframework.web.bind.annotation.*;
import java.io.IOException;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@RestController
@RequestMapping("/test")
public class SimpleController {
    @RequestMapping(value = "/hello", method = RequestMethod.GET)
    public String spring(HttpServletResponse response) {
        return "askfjklsajflkasklfjlaskj";
    }
}
```

## IOC容器

![1540699604487](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1540699604487.png)

创建对象的工作交给spring

### Bean

这里的bean不是java bean的概念中的bean.在spring中，凡是需要在容器中进行管理的对象都在xml里定义为一个bean.这是个xml的定义规则。
name是定义一个名称，class用于定义对应这个名称的类的名字，property是对这个类中变量的赋值，name是这个变量名，ref是引用这个XML里定义的另外一个bean的名字。

### Bean作用域

#### singleton

```
<bean id = "screwDriver" class="com.test.ScrewDriver"/>
```

单例，即从头到尾保持一致

#### prototype

```
<bean id = "screwDriver" class="com.test.ScrewDriver" scope="prototype"/>
```

每次引用创建一个实例

### Bean生命周期回调

#### 创建

```
public interface InitializingBean{
    void afterPropertiesSet() throws Exception;
}   
```

或者

```
<bean id = "screwDriver" class="com.test.ScrewDriver" init-method="init"/>
```

```
public class ScrewDriver {
    public void init(){
        System.out.println("Init screwdriver");
    }
}
```

#### 销毁

```
public interface DisposableBean{
    void destroy() throws Exception;
}
```

或者

```
<bean id = "screwDriver" class="com.test.ScrewDriver" destroy-method="cleanup"/>
```

```
public class ScrewDriver{
    public void cleanup(){
        System.out.println("Cleanup screwdriver");
    }
}
```

### 依赖注入

注入方式一：

```
<bean id="header" class="com.test.StraightHeader">
    <constructor-arg>
        <map>
            <entry key="color" value="red"/>
            <entry key="size" value="15"/>
        </map>
        <!--<list>-->
            <!--<value>red</value>-->
            <!--<value>15</value>-->
        <!--</list>-->
    </constructor-arg>
    <!--<constructor-arg name = "size" value = "15/>
    <constructor-arg name = "color" value = "red"/>-->
</bean>
```

```
public StraightHeader(Map<String,String> paras) {
    this.color = paras.get("color");
    this.size = Integer.valueOf(paras.get("size"));
}
```

注入方式二：

不通过文件注入

```
<bean id="header" class="com.test.StraightHeader">
    <constructor-arg name="color" value="${color}"/>
    <constructor-arg name="size" value="${size}"/>
</bean>

<bean id="headerProperties"
      class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
    <property name="location" value="classpath:header.properties"/>
</bean>
```

header.properties

```
color=green
size=15
```

### 自动装配及Annotation

##### byName：根据Bean名称

##### byType：根据Bean类型

##### constructor：构造函数，根据类型

#### Annotation

XML：繁琐，代码独立                 写到applicationContext里面去

Annotation：简介，代码耦合     写到spring_mvc里面去

##### 具体内容

| 注解        | 含义                                         |
| ----------- | -------------------------------------------- |
| @Component  | 最普通的组件，可以被注入到spring容器进行管理 |
| @Repository | 作用于持久层                                 |
| @Service    | 作用于业务逻辑层                             |
| @Controller | 作用于表现层                                 |

@Component：定义Bean 

@Value：properties注入

@Autowired & Resource：自动装配依赖

@PostConstruct & PreDestroy：生命周期回调

##### 具体实现

```
@Component("header")
public class StraightHeader implements Header {
    @Value("${color}")
    private String color;
    @Value("${size}")
    public int size;

    public StraightHeader() {

    }

    public void doWork() {
        System.out.println("Do work with straight header");
    }

    public String getInfo() {
        return "StraightHeader:color-" + color + ",size:" + size;
    }
}

<context:component-scan base-package="com.test"/>

    <bean id="screwDriver" class="com.test.ScrewDriver"/>

    <bean id="headerProperties"
          class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
        <property name="location" value="classpath:header.properties"/>
    </bean>
    

public class ScrewDriver {
    @Resource
    private Header header;
}
```

## AOP概述

面向切面编程

#### AOP优点

代码重用

解耦业务逻辑与非业务逻辑

#### 通知

前置通知

后置通知

环绕通知  

#### 开发流程 半自动方式

第一步：创建通知类

第二步：创建目标接口和实现类

第三步：创建代理类（Spring提供）  

3.1：整合目标类

3.2：整合通知类

3.3：关联接口

![1541149011775](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541149011775.png)

![1541148929388](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541148929388.png)

代理类返回的对象是被代理的目标对象，不是自己工厂的对象

#### 开发流程 全自动方式

1：创建目标类

2：创建通知类

3：AOP配置编程（让切入点和通知关联形成切面）

![1541160810717](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541160810717.png)

AOP全自动编程，直接获取目标类的对象就可以

#### 开发流程 AspectJ全自动方式

##### 通知

不需要实现接口

方法名称任意

##### 步骤

1：创建目标类

2：创建自定义的通知类（方法自定义）

参数用JoinPoint

```
//编写前置通知
public void myBefore(JoinPoint joinPoint){
    ...
}
//编写后置通知,可以得到返回值，返回值通过res接收
public void myAfter(JoinPoint joinPoint,Object res){
    ...
}
//编写环绕通知，必须手动唤醒目标方法
public Object myAround(ProceedingJoinPoint proceedingJoinPoint){
    System.out.println("环绕前");
    Object proceed = null;
    try{
        proceed = proceedingJoinPoint.proceed();
    } catch (Throwable throwable) {
        throwable.printStackTrace();
    }
    System.out.println("环绕后");
    return proceed;
}
```

3：AOP配置编程（让切入点和通知关联形成切面）

​    （1）Aspect有单独的结点，需要关联通知类

![1541162455209](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541162455209.png)

后置通知：

![1541162739636](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541162739636.png)

配置中开起Aspect注解

```
<aop:aspectj-autoproxy/>
```

#### AOP注解

@Component 加入自定义通知类 

@Service 加入服务层

@Aspect：声明切面，修饰切面类，获得通知，配合Component使用

**通知**

-@Before 前置 加入自定义方法前

-@AfterReturning 后置（value="myPoint()",returning="res")

-@Around 环绕

**切入点**

找到目标类中需要增强的方法

@Pointcut：修饰空方法 private void xxx(){},之后通过"方法名"获得切入点引用

![1540706641886](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1540706641886.png)

![1541161364908](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541161364908.png)

![1540706846235](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1540706846235.png)

Join point：函数执行或者属性访问

Advice：在某个函数执行点上要执行的切面功能

![1541155091379](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541155091379.png) 

#### Advice类型

Before：函数执行之前

After returning：函数正常返回之后

After throwing：函数抛出异常之后

After finally：函数返回之后

Around：函数执行前后

### @AspectJ AOP

AspectJ AOP是Spring本身提供的功能，需要添加依赖包，aspectjweaver.jar

```
http://www.springframework.org/schema/aop
http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
<aop:aspectj-autoproxy/>
```

单独存一个类

```
@Aspect
public class LoggingAspect {
    @Before("execution(* com.aop.Caculator.*(..)) && args(a, ..)")
    private void arithmeticDolog(JoinPoint jp, int a) {
        System.out.println(a + ":" + jp.toString());
    }
}
```

### Schema-based AOP

不需要：

```
<aop:aspectj-autoproxy/>
```

用其他来配置

### 区别

Schema配置相对集中

@AspectJ配置分散，但是兼容AspectJ

因此建议使用@AspectJ

## 数据访问

### DAO

Data Access Object

数据访问相关接口

### ORM

Object Relation Mapping

对象关系映射

### 数据访问

**连接参数**

打开连接

**声明SQL语句以及参数**

执行SQL ，并循环访问结果

**执行业务**

处理异常

关闭连接，语句以及结果集

用户所做的事情只需做图中三个加粗步骤，spring JDBC所做的要去封装其他步骤

#### 具体实现

```
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd ">

    <context:component-scan base-package="com.jdbc"/>
    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource" destroy-method="close">
        <property name="driverClassName" value="${jdbc.driverClassName}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <context:property-placeholder location="db.properties"/>
</beans>
```

```
@Repository
public class jdbcTemplateDao {
    private JdbcTemplate jdbcTemplate;

    @Autowired
    public void setDataSource(DataSource dataSource) {
        this.jdbcTemplate = new JdbcTemplate(dataSource);
    }

    public void createTable() {
        jdbcTemplate.execute("create table user (id integer,first_name varchar(100),last_name varchar(100))");
    }

    public void insertData() {
        this.jdbcTemplate.update("insert into user values(1,?,?)","Meimie","Han");
        this.jdbcTemplate.update("insert into user values (2,?,?)","Lei","Li");
    }

    public int count() {
        return this.jdbcTemplate.queryForObject("select count(*) from user",Integer.class);
    }
}
```

### spring事务管理

统一的事务编程模型

编程式事务及声明式事务（AOP）

### MyBatis整合

定义SqlSessionFactoryBean

```
<context:component-scan base-package="com.jdbc"/>
    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource" destroy-method="close">
        <property name="driverClassName" value="${jdbc.driverClassName}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <mybatis:scan base-package="com.jdbc"/>
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <context:property-placeholder location="db.properties"/>
```

定义Mapper

```
public interface UserMapper{
    @Select("select * from users where id = #{userId}")
    User getUser(@Param("userId") String userId);
}
```

定义MapperBean

```
<bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
    <property name="mapperInterface" value="com.netease.course.UserMapper"/>
    <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
</bean>
```

## SpringMVC工作原理

![1541164031240](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541164031240.png)

开发角度：

只需开发传统的MVC部分，其他的组件由SpringMVC提供

### 开发流程  配置方式一

#### 第一步：配置前端控制器（中央控制器）

##### 附带：配置过滤器

```
<!-- 如果是用mvn命令生成的xml，需要修改servlet版本为3.1 -->
<!-- 配置过滤器 -->
<filter>
        <filter-name>myFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <!-- 指定编码 -->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>myFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
<!-- 配置DispatcherServlet -->
<servlet>
    <servlet-name>seckill-dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!-- 配置springMVC需要加载的配置文件
        spring-dao.xml,spring-service.xml,spring-web.xml
        Mybatis - > spring -> springmvc
     -->
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring/spring-*.xml</param-value>
    </init-param>
</servlet>
<servlet-mapping>
    <servlet-name>seckill-dispatcher</servlet-name>
    <!-- 过滤器拦截do -->
    <url-pattern>*.do</url-pattern>
</servlet-mapping>
```

第二步：配置处理器映射器 name作为url查找

![1541426414538](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541426414538.png)

第三步：配置处理器适配器

![1541426754898](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541426754898.png)

写在applicationcontext里

第四步：配置视图解析器

![1541427074866](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541427074866.png)

第五步：开发功能控制器

即controller

第六步：开发模型层

整合mybatis

第七步：开发view 

![1541479072991](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541479072991.png)

### 配置方式二 注解方式

#### 第一步：上同

#### 第二步：注解映射器和适配器

![1541481272578](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541481272578.png)

替换上面的第二种方式

![1541482036649](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541482036649.png)

#### 第三步：配置视图解析器

![1541481310323](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541481310323.png)

#### 第四步：开发注解的Action

编写自定义的Action,不需要实现接口

方法自定义，通过@RequestMapping指定请求路径

![1541481824414](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541481824414.png)

扫描注解

![1541481886408](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541481886408.png)

扫描的目的是容器帮助我们创建对象，不需要自己编id,class

### @RequestMapping

可以加在类上：给请求加入根路径

可以加在方法上：具体方法的请求路径

可以每个方法都加入一个url和方法对应，也就是这种注解的方式，一个模块一个类就可以了。类中可以有多个方法

### @Controller

**@RequestMapping**

name：名称

value & path：路径，如"/hello"

method：请求方法，如"GET"

params：请求参数

headers：请求头

consumes：请求的媒体类型，“Content-Type"

produces：响应的媒体类型，“Accept"

### 功能业务之间的跳转

![1541483347412](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541483347412.png)

方式一：ModelAndView

方式二：String   return字符串

### 传值

#### 功能方法之间（C-C）

1：Request 

2：Session

3：Cache(分布式缓存)

#### 具体的功能方法到页面（C-V）

1：ModelAndView

2：Model

![1541486196896](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541486196896.png)

#### 从页面到功能方法（V-C）

1：request.getParamerter

2：指定具体的类型接收具体的参数

![1541485072274](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1541485072274.png)

### freemarker

#### 指令

开始标签：<#directivename parameters>

结束标签：</#directivename>

#### if指令

`<welcome ${user} <#if user == "Big Joe">,our beloved leader</#if>!`

#### list指令

```
<#list animals as animal>
<tr><td>${animal.name}<td>${animal.price} Euros
</#list>
代替
<tr><td>mouse<td>50 Euros
<tr><td>elephant<td>5000 Euros
<tr><td>python<td>4999 Euros
```

# SpringBoot

#### 使用场景

有spring的地方都行

j2EE/web项目

**微服务**

```
spring.datasource.driverClassName=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost/example2?serverTimezone=GMT%2B8&amp;useCursorFetch=true&amp;characterEncoding=utf8&&useSSL=true
spring.datasource.username=root
spring.datasource.password=root
```

### jackson注解

**字段重命名**

@JsonProperty(value="xxxx")

**字段可见性**

`@JsonIgnore`

**时间格式化**

@JsonFormat(pattern="yyyy-MM-dd hh:mm:ss a",locale="zh",timezone="GMT+8")

**省去null值元素**

@JsonInclude(Include.NON_NULL) 

### devtools热部署

#### 架包

![1542529595980](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1542529595980.png)

#### 配置

![1542529729604](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1542529729604.png)

### 资源文件属性配置

#### 架包

![1542530026326](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1542530026326.png)

#### 配置

![1542530038659](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1542530038659.png)

yml：

```
resource:
  name: lamar
  website: www.lamar.com
  language: java
  grantType: authorization_code
```

### 实体类

![1542530072808](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1542530072808.png)

这样就能在配置中进行值的传输了

### freemarker

**jsp**是大家最熟悉的技术
**优点**：
1、功能强大，可以写java代码
2、支持jsp标签（jsp tag）
3、支持表达式语言（el）
4、官方标准，用户群广，丰富的第三方jsp标签库
5、性能良好。jsp编译成class文件执行，有很好的性能表现
**缺点**：
jsp没有明显缺点，非要挑点骨头那就是，由于可以编写java代码，如使用不当容易破坏mvc结构。

**velocity**是较早出现的用于代替jsp的模板语言
**优点**：
1、不能编写java代码，可以实现严格的mvc分离
2、性能良好，据说比jsp性能还要好些
3、使用表达式语言，据说jsp的表达式语言就是学velocity的
**缺点**：
1、不是官方标准
2、用户群体和第三方标签库没有jsp多。
3、对jsp标签支持不够好

**freemarker**
**优点**：
1、不能编写java代码，可以实现严格的mvc分离
2、性能非常不错
3、对jsp标签支持良好
4、内置大量常用功能，使用非常方便
5、宏定义（类似jsp标签）非常方便
6、使用表达式语言
**缺点**：
1、不是官方标准
2、用户群体和第三方标签库没有jsp多

**选择freemarker的原因**：
1、性能。velocity应该是最好的，其次是jsp，普通的页面freemarker性能最差（虽然只是几毫秒到十几毫秒的差距）。但是在复杂页面上（包含大量判断、日期金额格式化）的页面上，freemarker的性能比使用tag和el的jsp好。
2、宏定义比jsp tag方便
3、内置大量常用功能。比如html过滤，日期金额格式化等等，使用非常方便
4、支持jsp标签

5、可以实现严格的mvc分离

#### 架包

![1542533367027](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1542533367027.png)

### 配置

![1542533472987](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1542533472987.png)

### 设置静态文件

spring.mvc.static-path-pattern=/static/**

### spring redis

配置

```
redis:
  database: 0
  host: 192.168.3.50
  port: 6379
  password: 123456
  timeout: 5000
  jedis:
    pool:
      max-active: 8
      max-wait: -1
      max-idle: 8
      min-idle: 0
```

**存储**

```
strRedis.opsForValue().set("json:user",“xxxx”);
```

**取值**

```
strRedis.opsForValue().get("json:user");
```

#### Redis封装

```
package com.example.demo111.utils;

import org.springframework.stereotype.Component;
import redis.clients.jedis.BinaryClient;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;
import redis.clients.jedis.exceptions.JedisConnectionException;

import java.util.List;
import java.util.Map;
import java.util.Set;

/**
 * className: RedisOperator
 * description: TODO
 *
 * @author hasee
 * @version 1.0
 * @date 2018/11/22 16:31
 */
@Component
public class RedisUtils {
    //Redis服务器IP
    private static String ADDR = "192.168.3.50";

    //Redis的端口号
    private static int PORT = 6379;

    //访问密码
    private static String AUTH = "123456";

    //可用连接实例的最大数目，默认值为8；
    //如果赋值为-1，则表示不限制；如果pool已经分配了maxActive个jedis实例，则此时pool的状态为exhausted(耗尽)。
    private static int MAX_ACTIVE = 1024;

    //控制一个pool最多有多少个状态为idle(空闲的)的jedis实例，默认值8。
    private static int MAX_IDLE = 200;

    //等待可用连接的最大时间，单位毫秒，默认值为-1，表示永不超时。如果超过等待时间，则直接抛出JedisConnectionException；
    private static int MAX_WAIT = 10000;

    private static int TIMEOUT = 10000;

    //在borrow一个jedis实例时，是否提前进行validate操作；如果为true，则得到的jedis实例均是可用的；
    private static boolean TEST_ON_BORROW = true;

    private static JedisPool pool = null;

    /**
     * 初始化Redis连接池
     */
    static {
        try {
            JedisPoolConfig config = new JedisPoolConfig();
            config.setMaxTotal(MAX_ACTIVE);
            config.setMaxIdle(MAX_IDLE);
            config.setMaxWaitMillis(MAX_WAIT);
            config.setTestOnBorrow(TEST_ON_BORROW);
            Integer port = new Integer(PORT);
            pool = new JedisPool(config, ADDR, port, TIMEOUT, AUTH);

            try {
                pool.getResource();
            } catch (JedisConnectionException e) {
                System.out.println("redis没有设置密码");
                pool = new JedisPool(config, ADDR, port, TIMEOUT);
                //System.out.println("再次获取资源："+pool.getResource());
                e.printStackTrace();
            }
            //需要认证
            // pool = new JedisPool(config, ADDR, PORT, TIMEOUT,AUTH);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }


    /**
     * 返还到连接池
     *
     * @param pool
     * @param jedis
     */
    public static void returnResource(JedisPool pool, Jedis jedis) {
        if (jedis != null) {
            if (jedis != null) {
                jedis.close();
            }
        }
    }

    /**
     * <p>通过key获取储存在redis中的value</p>
     * <p>并释放连接</p>
     *
     * @param key
     * @return 成功返回value 失败返回null
     */
    public static String get(String key) {
        Jedis jedis = null;
        String value = null;
        try {
            jedis = pool.getResource();
            value = jedis.get(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return value;
    }

    /**
     * <p>向redis存入key和value,并释放连接资源</p>
     * <p>如果key已经存在 则覆盖</p>
     *
     * @param key
     * @param value
     * @return 成功 返回OK 失败返回 0
     */
    public static String set(String key, String value) {
        Jedis jedis = null;
        try {
            jedis = pool.getResource();
            return jedis.set(key, value);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
            return "0";
        } finally {
            returnResource(pool, jedis);
        }
    }


    /**
     * <p>删除指定的key,也可以传入一个包含key的数组</p>
     *
     * @param keys 一个key  也可以使 string 数组
     * @return 返回删除成功的个数
     */
    public static Long del(String... keys) {
        Jedis jedis = null;
        try {
            jedis = pool.getResource();
            return jedis.del(keys);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
            return 0L;
        } finally {
            returnResource(pool, jedis);
        }
    }

    /**
     * <p>通过key向指定的value值追加值</p>
     *
     * @param key
     * @param str
     * @return 成功返回 添加后value的长度 失败 返回 添加的 value 的长度  异常返回0L
     */
    public static Long append(String key, String str) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.append(key, str);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
            return 0L;
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>判断key是否存在</p>
     *
     * @param key
     * @return true OR false
     */
    public static Boolean exists(String key) {
        Jedis jedis = null;
        try {
            jedis = pool.getResource();
            return jedis.exists(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
            return false;
        } finally {
            returnResource(pool, jedis);
        }
    }

    /**
     * <p>设置key value,如果key已经存在则返回0,nx==> not exist</p>
     *
     * @param key
     * @param value
     * @return 成功返回1 如果存在 和 发生异常 返回 0
     */
    public static Long setnx(String key, String value) {
        Jedis jedis = null;
        try {
            jedis = pool.getResource();
            return jedis.setnx(key, value);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
            return 0L;
        } finally {
            returnResource(pool, jedis);
        }
    }

    /**
     * <p>设置key value并制定这个键值的有效期</p>
     *
     * @param key
     * @param value
     * @param seconds 单位:秒
     * @return 成功返回OK 失败和异常返回null
     */
    public static String setex(String key, String value, int seconds) {
        Jedis jedis = null;
        String res = null;
        try {
            jedis = pool.getResource();
            res = jedis.setex(key, seconds, value);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }


    /**
     * <p>通过key 和offset 从指定的位置开始将原先value替换</p>
     * <p>下标从0开始,offset表示从offset下标开始替换</p>
     * <p>如果替换的字符串长度过小则会这样</p>
     * <p>example:</p>
     * <p>value : bigsea@zto.cn</p>
     * <p>str : abc </p>
     * <P>从下标7开始替换  则结果为</p>
     * <p>RES : bigsea.abc.cn</p>
     *
     * @param key
     * @param str
     * @param offset 下标位置
     * @return 返回替换后  value 的长度
     */
    public static Long setrange(String key, String str, int offset) {
        Jedis jedis = null;
        try {
            jedis = pool.getResource();
            return jedis.setrange(key, offset, str);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
            return 0L;
        } finally {
            returnResource(pool, jedis);
        }
    }


    /**
     * <p>通过批量的key获取批量的value</p>
     *
     * @param keys string数组 也可以是一个key
     * @return 成功返回value的集合, 失败返回null的集合 ,异常返回空
     */
    public static List<String> mget(String... keys) {
        Jedis jedis = null;
        List<String> values = null;
        try {
            jedis = pool.getResource();
            values = jedis.mget(keys);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return values;
    }

    /**
     * <p>批量的设置key:value,可以一个</p>
     * <p>example:</p>
     * <p>  obj.mset(new String[]{"key2","value1","key2","value2"})</p>
     *
     * @param keysvalues
     * @return 成功返回OK 失败 异常 返回 null
     */
    public static String mset(String... keysvalues) {
        Jedis jedis = null;
        String res = null;
        try {
            jedis = pool.getResource();
            res = jedis.mset(keysvalues);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>批量的设置key:value,可以一个,如果key已经存在则会失败,操作会回滚</p>
     * <p>example:</p>
     * <p>  obj.msetnx(new String[]{"key2","value1","key2","value2"})</p>
     *
     * @param keysvalues
     * @return 成功返回1 失败返回0
     */
    public static Long msetnx(String... keysvalues) {
        Jedis jedis = null;
        Long res = 0L;
        try {
            jedis = pool.getResource();
            res = jedis.msetnx(keysvalues);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>设置key的值,并返回一个旧值</p>
     *
     * @param key
     * @param value
     * @return 旧值 如果key不存在 则返回null
     */
    public static String getset(String key, String value) {
        Jedis jedis = null;
        String res = null;
        try {
            jedis = pool.getResource();
            res = jedis.getSet(key, value);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过下标 和key 获取指定下标位置的 value</p>
     *
     * @param key
     * @param startOffset 开始位置 从0 开始 负数表示从右边开始截取
     * @param endOffset
     * @return 如果没有返回null
     */
    public static String getrange(String key, int startOffset, int endOffset) {
        Jedis jedis = null;
        String res = null;
        try {
            jedis = pool.getResource();
            res = jedis.getrange(key, startOffset, endOffset);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key 对value进行加值+1操作,当value不是int类型时会返回错误,当key不存在是则value为1</p>
     *
     * @param key
     * @return 加值后的结果
     */
    public static Long incr(String key) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.incr(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key给指定的value加值,如果key不存在,则这是value为该值</p>
     *
     * @param key
     * @param integer
     * @return
     */
    public static Long incrBy(String key, Long integer) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.incrBy(key, integer);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>对key的值做减减操作,如果key不存在,则设置key为-1</p>
     *
     * @param key
     * @return
     */
    public static Long decr(String key) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.decr(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>减去指定的值</p>
     *
     * @param key
     * @param integer
     * @return
     */
    public static Long decrBy(String key, Long integer) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.decrBy(key, integer);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key获取value值的长度</p>
     *
     * @param key
     * @return 失败返回null
     */
    public static Long serlen(String key) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.strlen(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key给field设置指定的值,如果key不存在,则先创建</p>
     *
     * @param key
     * @param field 字段
     * @param value
     * @return 如果存在返回0 异常返回null
     */
    public static Long hset(String key, String field, String value) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.hset(key, field, value);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key给field设置指定的值,如果key不存在则先创建,如果field已经存在,返回0</p>
     *
     * @param key
     * @param field
     * @param value
     * @return
     */
    public static Long hsetnx(String key, String field, String value) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.hsetnx(key, field, value);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key同时设置 hash的多个field</p>
     *
     * @param key
     * @param hash
     * @return 返回OK 异常返回null
     */
    public static String hmset(String key, Map<String, String> hash) {
        Jedis jedis = null;
        String res = null;
        try {
            jedis = pool.getResource();
            res = jedis.hmset(key, hash);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key 和 field 获取指定的 value</p>
     *
     * @param key
     * @param field
     * @return 没有返回null
     */
    public static String hget(String key, String field) {
        Jedis jedis = null;
        String res = null;
        try {
            jedis = pool.getResource();
            res = jedis.hget(key, field);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key 和 fields 获取指定的value 如果没有对应的value则返回null</p>
     *
     * @param key
     * @param fields 可以使 一个String 也可以是 String数组
     * @return
     */
    public static List<String> hmget(String key, String... fields) {
        Jedis jedis = null;
        List<String> res = null;
        try {
            jedis = pool.getResource();
            res = jedis.hmget(key, fields);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key给指定的field的value加上给定的值</p>
     *
     * @param key
     * @param field
     * @param value
     * @return
     */
    public static Long hincrby(String key, String field, Long value) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.hincrBy(key, field, value);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key和field判断是否有指定的value存在</p>
     *
     * @param key
     * @param field
     * @return
     */
    public static Boolean hexists(String key, String field) {
        Jedis jedis = null;
        Boolean res = false;
        try {
            jedis = pool.getResource();
            res = jedis.hexists(key, field);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key返回field的数量</p>
     *
     * @param key
     * @return
     */
    public static Long hlen(String key) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.hlen(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;

    }

    /**
     * <p>通过key 删除指定的 field </p>
     *
     * @param key
     * @param fields 可以是 一个 field 也可以是 一个数组
     * @return
     */
    public static Long hdel(String key, String... fields) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.hdel(key, fields);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key返回所有的field</p>
     *
     * @param key
     * @return
     */
    public static Set<String> hkeys(String key) {
        Jedis jedis = null;
        Set<String> res = null;
        try {
            jedis = pool.getResource();
            res = jedis.hkeys(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key返回所有和key有关的value</p>
     *
     * @param key
     * @return
     */
    public static List<String> hvals(String key) {
        Jedis jedis = null;
        List<String> res = null;
        try {
            jedis = pool.getResource();
            res = jedis.hvals(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key获取所有的field和value</p>
     *
     * @param key
     * @return
     */
    public static Map<String, String> hgetall(String key) {
        Jedis jedis = null;
        Map<String, String> res = null;
        try {
            jedis = pool.getResource();
            res = jedis.hgetAll(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key向list头部添加字符串</p>
     *
     * @param key
     * @param strs 可以使一个string 也可以使string数组
     * @return 返回list的value个数
     */
    public Long lpush(String key, String... strs) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.lpush(key, strs);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key向list尾部添加字符串</p>
     *
     * @param key
     * @param strs 可以使一个string 也可以使string数组
     * @return 返回list的value个数
     */
    public static Long rpush(String key, String... strs) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.rpush(key, strs);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key在list指定的位置之前或者之后 添加字符串元素</p>
     *
     * @param key
     * @param where LIST_POSITION枚举类型
     * @param pivot list里面的value
     * @param value 添加的value
     * @return
     */
    public static Long linsert(String key, BinaryClient.LIST_POSITION where,
                               String pivot, String value) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.linsert(key, where, pivot, value);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key设置list指定下标位置的value</p>
     * <p>如果下标超过list里面value的个数则报错</p>
     *
     * @param key
     * @param index 从0开始
     * @param value
     * @return 成功返回OK
     */
    public static String lset(String key, Long index, String value) {
        Jedis jedis = null;
        String res = null;
        try {
            jedis = pool.getResource();
            res = jedis.lset(key, index, value);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key从对应的list中删除指定的count个 和 value相同的元素</p>
     *
     * @param key
     * @param count 当count为0时删除全部
     * @param value
     * @return 返回被删除的个数
     */
    public static Long lrem(String key, long count, String value) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.lrem(key, count, value);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key保留list中从strat下标开始到end下标结束的value值</p>
     *
     * @param key
     * @param start
     * @param end
     * @return 成功返回OK
     */
    public static String ltrim(String key, long start, long end) {
        Jedis jedis = null;
        String res = null;
        try {
            jedis = pool.getResource();
            res = jedis.ltrim(key, start, end);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key从list的头部删除一个value,并返回该value</p>
     *
     * @param key
     * @return
     */
    public static String lpop(String key) {
        Jedis jedis = null;
        String res = null;
        try {
            jedis = pool.getResource();
            res = jedis.lpop(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key从list尾部删除一个value,并返回该元素</p>
     *
     * @param key
     * @return
     */
    public static String rpop(String key) {
        Jedis jedis = null;
        String res = null;
        try {
            jedis = pool.getResource();
            res = jedis.rpop(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key从一个list的尾部删除一个value并添加到另一个list的头部,并返回该value</p>
     * <p>如果第一个list为空或者不存在则返回null</p>
     *
     * @param srckey
     * @param dstkey
     * @return
     */
    public static String rpoplpush(String srckey, String dstkey) {
        Jedis jedis = null;
        String res = null;
        try {
            jedis = pool.getResource();
            res = jedis.rpoplpush(srckey, dstkey);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key获取list中指定下标位置的value</p>
     *
     * @param key
     * @param index
     * @return 如果没有返回null
     */
    public static String lindex(String key, long index) {
        Jedis jedis = null;
        String res = null;
        try {
            jedis = pool.getResource();
            res = jedis.lindex(key, index);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key返回list的长度</p>
     *
     * @param key
     * @return
     */
    public static Long llen(String key) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.llen(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key获取list指定下标位置的value</p>
     * <p>如果start 为 0 end 为 -1 则返回全部的list中的value</p>
     *
     * @param key
     * @param start
     * @param end
     * @return
     */
    public static List<String> lrange(String key, long start, long end) {
        Jedis jedis = null;
        List<String> res = null;
        try {
            jedis = pool.getResource();
            res = jedis.lrange(key, start, end);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key向指定的set中添加value</p>
     *
     * @param key
     * @param members 可以是一个String 也可以是一个String数组
     * @return 添加成功的个数
     */
    public static Long sadd(String key, String... members) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.sadd(key, members);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key删除set中对应的value值</p>
     *
     * @param key
     * @param members 可以是一个String 也可以是一个String数组
     * @return 删除的个数
     */
    public static Long srem(String key, String... members) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.srem(key, members);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key随机删除一个set中的value并返回该值</p>
     *
     * @param key
     * @return
     */
    public static String spop(String key) {
        Jedis jedis = null;
        String res = null;
        try {
            jedis = pool.getResource();
            res = jedis.spop(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key获取set中的差集</p>
     * <p>以第一个set为标准</p>
     *
     * @param keys 可以使一个string 则返回set中所有的value 也可以是string数组
     * @return
     */
    public static Set<String> sdiff(String... keys) {
        Jedis jedis = null;
        Set<String> res = null;
        try {
            jedis = pool.getResource();
            res = jedis.sdiff(keys);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key获取set中的差集并存入到另一个key中</p>
     * <p>以第一个set为标准</p>
     *
     * @param dstkey 差集存入的key
     * @param keys   可以使一个string 则返回set中所有的value 也可以是string数组
     * @return
     */
    public static Long sdiffstore(String dstkey, String... keys) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.sdiffstore(dstkey, keys);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key获取指定set中的交集</p>
     *
     * @param keys 可以使一个string 也可以是一个string数组
     * @return
     */
    public static Set<String> sinter(String... keys) {
        Jedis jedis = null;
        Set<String> res = null;
        try {
            jedis = pool.getResource();
            res = jedis.sinter(keys);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key获取指定set中的交集 并将结果存入新的set中</p>
     *
     * @param dstkey
     * @param keys   可以使一个string 也可以是一个string数组
     * @return
     */
    public static Long sinterstore(String dstkey, String... keys) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.sinterstore(dstkey, keys);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key返回所有set的并集</p>
     *
     * @param keys 可以使一个string 也可以是一个string数组
     * @return
     */
    public static Set<String> sunion(String... keys) {
        Jedis jedis = null;
        Set<String> res = null;
        try {
            jedis = pool.getResource();
            res = jedis.sunion(keys);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key返回所有set的并集,并存入到新的set中</p>
     *
     * @param dstkey
     * @param keys   可以使一个string 也可以是一个string数组
     * @return
     */
    public static Long sunionstore(String dstkey, String... keys) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.sunionstore(dstkey, keys);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key将set中的value移除并添加到第二个set中</p>
     *
     * @param srckey 需要移除的
     * @param dstkey 添加的
     * @param member set中的value
     * @return
     */
    public static Long smove(String srckey, String dstkey, String member) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.smove(srckey, dstkey, member);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key获取set中value的个数</p>
     *
     * @param key
     * @return
     */
    public static Long scard(String key) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.scard(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key判断value是否是set中的元素</p>
     *
     * @param key
     * @param member
     * @return
     */
    public static Boolean sismember(String key, String member) {
        Jedis jedis = null;
        Boolean res = null;
        try {
            jedis = pool.getResource();
            res = jedis.sismember(key, member);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key获取set中随机的value,不删除元素</p>
     *
     * @param key
     * @return
     */
    public static String srandmember(String key) {
        Jedis jedis = null;
        String res = null;
        try {
            jedis = pool.getResource();
            res = jedis.srandmember(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key获取set中所有的value</p>
     *
     * @param key
     * @return
     */
    public static Set<String> smembers(String key) {
        Jedis jedis = null;
        Set<String> res = null;
        try {
            jedis = pool.getResource();
            res = jedis.smembers(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key向zset中添加value,score,其中score就是用来排序的</p>
     * <p>如果该value已经存在则根据score更新元素</p>
     *
     * @param key
     * @param scoreMembers
     * @return
     */
    public static Long zadd(String key, Map<String, Double> scoreMembers) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.zadd(key, scoreMembers);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key向zset中添加value,score,其中score就是用来排序的</p>
     * <p>如果该value已经存在则根据score更新元素</p>
     *
     * @param key
     * @param score
     * @param member
     * @return
     */
    public static Long zadd(String key, double score, String member) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.zadd(key, score, member);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key删除在zset中指定的value</p>
     *
     * @param key
     * @param members 可以使一个string 也可以是一个string数组
     * @return
     */
    public static Long zrem(String key, String... members) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.zrem(key, members);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key增加该zset中value的score的值</p>
     *
     * @param key
     * @param score
     * @param member
     * @return
     */
    public static Double zincrby(String key, double score, String member) {
        Jedis jedis = null;
        Double res = null;
        try {
            jedis = pool.getResource();
            res = jedis.zincrby(key, score, member);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key返回zset中value的排名</p>
     * <p>下标从小到大排序</p>
     *
     * @param key
     * @param member
     * @return
     */
    public static Long zrank(String key, String member) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.zrank(key, member);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key返回zset中value的排名</p>
     * <p>下标从大到小排序</p>
     *
     * @param key
     * @param member
     * @return
     */
    public static Long zrevrank(String key, String member) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.zrevrank(key, member);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key将获取score从start到end中zset的value</p>
     * <p>socre从大到小排序</p>
     * <p>当start为0 end为-1时返回全部</p>
     *
     * @param key
     * @param start
     * @param end
     * @return
     */
    public static Set<String> zrevrange(String key, long start, long end) {
        Jedis jedis = null;
        Set<String> res = null;
        try {
            jedis = pool.getResource();
            res = jedis.zrevrange(key, start, end);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key返回指定score内zset中的value</p>
     *
     * @param key
     * @param max
     * @param min
     * @return
     */
    public static Set<String> zrangebyscore(String key, String max, String min) {
        Jedis jedis = null;
        Set<String> res = null;
        try {
            jedis = pool.getResource();
            res = jedis.zrevrangeByScore(key, max, min);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key返回指定score内zset中的value</p>
     *
     * @param key
     * @param max
     * @param min
     * @return
     */
    public static Set<String> zrangeByScore(String key, double max, double min) {
        Jedis jedis = null;
        Set<String> res = null;
        try {
            jedis = pool.getResource();
            res = jedis.zrevrangeByScore(key, max, min);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>返回指定区间内zset中value的数量</p>
     *
     * @param key
     * @param min
     * @param max
     * @return
     */
    public static Long zcount(String key, String min, String max) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.zcount(key, min, max);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key返回zset中的value个数</p>
     *
     * @param key
     * @return
     */
    public static Long zcard(String key) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.zcard(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key获取zset中value的score值</p>
     *
     * @param key
     * @param member
     * @return
     */
    public static Double zscore(String key, String member) {
        Jedis jedis = null;
        Double res = null;
        try {
            jedis = pool.getResource();
            res = jedis.zscore(key, member);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key删除给定区间内的元素</p>
     *
     * @param key
     * @param start
     * @param end
     * @return
     */
    public static Long zremrangeByRank(String key, long start, long end) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.zremrangeByRank(key, start, end);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key删除指定score内的元素</p>
     *
     * @param key
     * @param start
     * @param end
     * @return
     */
    public static Long zremrangeByScore(String key, double start, double end) {
        Jedis jedis = null;
        Long res = null;
        try {
            jedis = pool.getResource();
            res = jedis.zremrangeByScore(key, start, end);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>返回满足pattern表达式的所有key</p>
     * <p>keys(*)</p>
     * <p>返回所有的key</p>
     *
     * @param pattern
     * @return
     */
    public static Set<String> keys(String pattern) {
        Jedis jedis = null;
        Set<String> res = null;
        try {
            jedis = pool.getResource();
            res = jedis.keys(pattern);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }

    /**
     * <p>通过key判断值得类型</p>
     *
     * @param key
     * @return
     */
    public static String type(String key) {
        Jedis jedis = null;
        String res = null;
        try {
            jedis = pool.getResource();
            res = jedis.type(key);
        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            e.printStackTrace();
        } finally {
            returnResource(pool, jedis);
        }
        return res;
    }
}
```

测试

```
RedisUtils.set("json:user",JSONUtils.toJSONString(user));
return RedisUtils.get("json:user");
```

### 异步操作

1：写上@EnableAsync![1542889477743](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1542889477743.png)

2:服务层编写相应的服务![1542889520940](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1542889520940.png)

3：Web层实现

![1542889618279](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1542889618279.png)

### 整合拦截器

#### 过滤器，拦截器，监听器三者的对比

**1.过滤器**
  依赖于servlet容器。在实现上基于函数回调，可以对几乎所有请求进行过滤，但是缺点是一个过滤器实例只能在容器初始化时调用一次。使用过滤器的目的是用来做一些过滤操作，获取我们想要获取的数据，比如：在过滤器中修改字符编码；在过滤器中修改HttpServletRequest的一些参数，包括：过滤低俗文字、危险字符等。



**2.拦截器**
  依赖于web框架，在SpringMVC中就是依赖于SpringMVC框架。在实现上基于Java的反射机制，属于面向切面编程（AOP）的一种运用。由于拦截器是基于web框架的调用，因此可以使用Spring的依赖注入（DI）进行一些业务操作，同时一个拦截器实例在一个controller生命周期之内可以多次调用。但是缺点是只能对controller请求进行拦截，对其他的一些比如直接访问静态资源的请求则没办法进行拦截处理。 



**3.监听器**
  web监听器是一种Servlet中的特殊的类，它们能帮助开发者监听web中的特定事件，实现了javax.servlet.ServletContextListener 接口的服务器端程序，它也是随web应用的启动而启动，只初始化一次，随web应用的停止而销毁。主要作用是：感知到包括request(请求域)，session(会话域)和applicaiton(应用程序)的初始化和属性的变化。

##### 所依赖的支持

拦截器需要Spring的支持；过滤器、监听器需要servlet的支持。

##### 应用场景

拦截器：拦截未登录、审计日志等；

过滤器：设置字符编码、URL级别的权限访问控制、过滤敏感词汇、压缩响应信息等；

监听器：统计在线人数、清除过期session。

#### 配置步骤

1：使用注解@Configuration配置拦截器

2：继承WebMvcConfigurerAdapter

3：重写addInterceptors添加需要的拦截器地址



# spring工具

### lombok

通过注解来实现对getter和setter等方法的简化

```xml
<lombok.version>1.16.20</lombok.version>

<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>${lombok.version}</version>
            <scope>provided</scope>
        </dependency>
```

```java
@Getter
@Setter
@ToString
//@data包含了@ToString、@Getter、@Setter、@EqualsAndHashCode、@NoArgsConstructor
@Data  
@AllArgsConstructor
@NoArgsConstructor
```

### Swagger



### JSONUtils

```
package com.example.demo111.utils;

import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.*;

import java.util.Iterator;
import java.util.List;
import java.util.Map;

import com.opensymphony.xwork2.ActionContext;
import net.sf.json.JSONArray;
import net.sf.json.JSONObject;
import org.apache.commons.beanutils.BeanUtils;
import org.apache.struts2.ServletActionContext;
/**
 * className: JSONUtils
 * description: TODO
 *
 * @author hasee
 * @version 1.0
 * @date 2018/11/22 11:48
 */
public class JSONUtils {

/**
 *
 * @author wangwei JSON工具类
 * @param <T>
 *
 */

    /***
     * 将List对象序列化为JSON文本
     */
    public static <T> String toJSONString(List<T> list) {
        JSONArray jsonArray = JSONArray.fromObject(list);
        return jsonArray.toString();
    }

    /***
     * 将对象序列化为JSON文本
     * @param object
     * @return
     */
    public static String toJSONString(Object object) {
        JSONArray jsonArray = JSONArray.fromObject(object);
        return jsonArray.toString();
    }

    /***
     * 将JSON对象数组序列化为JSON文本
     * @param jsonArray
     * @return
     */
    public static String toJSONString(JSONArray jsonArray) {
        return jsonArray.toString();
    }

    /***
     * 将JSON对象序列化为JSON文本
     * @param jsonObject
     * @return
     */
    public static String toJSONString(JSONObject jsonObject) {
        return jsonObject.toString();
    }

    /***
     * 将对象转换为List对象
     * @param object
     * @return
     */
    public static List toArrayList(Object object) {
        List arrayList = new ArrayList();
        JSONArray jsonArray = JSONArray.fromObject(object);
        Iterator it = jsonArray.iterator();
        while (it.hasNext()) {
            JSONObject jsonObject = (JSONObject) it.next();
            Iterator keys = jsonObject.keys();
            while (keys.hasNext()) {
                Object key = keys.next();
                Object value = jsonObject.get(key);
                arrayList.add(value);
            }
        }
        return arrayList;
    }

    /***
     * 将对象转换为Collection对象
     * @param object
     * @return
     */
    public static Collection toCollection(Object object) {
        JSONArray jsonArray = JSONArray.fromObject(object);
        return JSONArray.toCollection(jsonArray);
    }

    /***
     * 将对象转换为JSON对象数组
     * @param object
     * @return
     */
    public static JSONArray toJSONArray(Object object) {
        return JSONArray.fromObject(object);
    }

    /***
     * 将对象转换为JSON对象
     * @param object
     * @return
     */
    public static JSONObject toJSONObject(Object object) {
        return JSONObject.fromObject(object);
    }

    /***
     * 将对象转换为HashMap
     * @param object
     * @return
     */
    public static HashMap toHashMap(Object object) {
        HashMap<String, Object> data = new HashMap<String, Object>();
        JSONObject jsonObject = JSONUtils.toJSONObject(object);
        Iterator it = jsonObject.keys();
        while (it.hasNext()) {
            String key = String.valueOf(it.next());
            Object value = jsonObject.get(key);
            data.put(key, value);
        }
        return data;
    }

    /***
     * 将对象转换为List<Map<String,Object>>
     * @param object
     * @return
     */
// 返回非实体类型(Map<String,Object>)的List
    public static List<Map<String, Object>> toList(Object object) {
        List<Map<String, Object>> list = new ArrayList<Map<String, Object>>();
        JSONArray jsonArray = JSONArray.fromObject(object);
        for (Object obj : jsonArray) {
            JSONObject jsonObject = (JSONObject) obj;
            Map<String, Object> map = new HashMap<String, Object>();
            Iterator it = jsonObject.keys();
            while (it.hasNext()) {
                String key = (String) it.next();
                Object value = jsonObject.get(key);
                map.put((String) key, value);
            }
            list.add(map);
        }
        return list;
    }

    /***
     * 将JSON对象数组转换为传入类型的List
     * @param <T>
     * @param jsonArray
     * @param objectClass
     * @return
     */
    public static <T> List<T> toList(JSONArray jsonArray, Class<T> objectClass) {
        return JSONArray.toList(jsonArray, objectClass);
    }

    /***
     * 将对象转换为传入类型的List
     * @param <T>
     * @param jsonArray
     * @param objectClass
     * @return
     */
    public static <T> List<T> toList(Object object, Class<T> objectClass) {
        JSONArray jsonArray = JSONArray.fromObject(object);
        return JSONArray.toList(jsonArray, objectClass);
    }

    /***
     * 将JSON对象转换为传入类型的对象
     * @param <T>
     * @param jsonObject
     * @param beanClass
     * @return
     */
    public static <T> T toBean(JSONObject jsonObject, Class<T> beanClass) {
        return (T) JSONObject.toBean(jsonObject, beanClass);
    }

    /***
     * 将将对象转换为传入类型的对象
     * @param <T>
     * @param object
     * @param beanClass
     * @return
     */
    public static <T> T toBean(Object object, Class<T> beanClass) {
        JSONObject jsonObject = JSONObject.fromObject(object);
        return (T) JSONObject.toBean(jsonObject, beanClass);
    }

    /***
     * 将JSON文本反序列化为主从关系的实体
     * @param <T> 泛型T 代表主实体类型
     * @param <D> 泛型D 代表从实体类型
     * @param jsonString JSON文本
     * @param mainClass 主实体类型
     * @param detailName 从实体类在主实体类中的属性名称
     * @param detailClass 从实体类型
     * @return
     */
    public static <T, D> T toBean(String jsonString, Class<T> mainClass,
                                  String detailName, Class<D> detailClass) {
        JSONObject jsonObject = JSONObject.fromObject(jsonString);
        JSONArray jsonArray = (JSONArray) jsonObject.get(detailName);
        T mainEntity = JSONUtils.toBean(jsonObject, mainClass);
        List<D> detailList = JSONUtils.toList(jsonArray, detailClass);
        try {
            BeanUtils.setProperty(mainEntity, detailName, detailList);
        } catch (Exception ex) {
            throw new RuntimeException("主从关系JSON反序列化实体失败！");
        }
        return mainEntity;
    }

    /***
     * 将JSON文本反序列化为主从关系的实体
     * @param <T>泛型T 代表主实体类型
     * @param <D1>泛型D1 代表从实体类型
     * @param <D2>泛型D2 代表从实体类型
     * @param jsonString JSON文本
     * @param mainClass 主实体类型
     * @param detailName1 从实体类在主实体类中的属性
     * @param detailClass1 从实体类型
     * @param detailName2 从实体类在主实体类中的属性
     * @param detailClass2 从实体类型
     * @return
     */
    public static <T, D1, D2> T toBean(String jsonString, Class<T> mainClass,
                                       String detailName1, Class<D1> detailClass1, String detailName2,
                                       Class<D2> detailClass2) {
        JSONObject jsonObject = JSONObject.fromObject(jsonString);
        JSONArray jsonArray1 = (JSONArray) jsonObject.get(detailName1);
        JSONArray jsonArray2 = (JSONArray) jsonObject.get(detailName2);
        T mainEntity = JSONUtils.toBean(jsonObject, mainClass);
        List<D1> detailList1 = JSONUtils.toList(jsonArray1, detailClass1);
        List<D2> detailList2 = JSONUtils.toList(jsonArray2, detailClass2);
        try {
            BeanUtils.setProperty(mainEntity, detailName1, detailList1);
            BeanUtils.setProperty(mainEntity, detailName2, detailList2);
        } catch (Exception ex) {
            throw new RuntimeException("主从关系JSON反序列化实体失败！");
        }
        return mainEntity;
    }

    /***
     * 将JSON文本反序列化为主从关系的实体
     * @param <T>泛型T 代表主实体类型
     * @param <D1>泛型D1 代表从实体类型
     * @param <D2>泛型D2 代表从实体类型
     * @param jsonString JSON文本
     * @param mainClass 主实体类型
     * @param detailName1 从实体类在主实体类中的属性
     * @param detailClass1 从实体类型
     * @param detailName2 从实体类在主实体类中的属性
     * @param detailClass2 从实体类型
     * @param detailName3 从实体类在主实体类中的属性
     * @param detailClass3 从实体类型
     * @return
     */
    public static <T, D1, D2, D3> T toBean(String jsonString,
                                           Class<T> mainClass, String detailName1, Class<D1> detailClass1,
                                           String detailName2, Class<D2> detailClass2, String detailName3,
                                           Class<D3> detailClass3) {
        JSONObject jsonObject = JSONObject.fromObject(jsonString);
        JSONArray jsonArray1 = (JSONArray) jsonObject.get(detailName1);
        JSONArray jsonArray2 = (JSONArray) jsonObject.get(detailName2);
        JSONArray jsonArray3 = (JSONArray) jsonObject.get(detailName3);
        T mainEntity = JSONUtils.toBean(jsonObject, mainClass);
        List<D1> detailList1 = JSONUtils.toList(jsonArray1, detailClass1);
        List<D2> detailList2 = JSONUtils.toList(jsonArray2, detailClass2);
        List<D3> detailList3 = JSONUtils.toList(jsonArray3, detailClass3);
        try {
            BeanUtils.setProperty(mainEntity, detailName1, detailList1);
            BeanUtils.setProperty(mainEntity, detailName2, detailList2);
            BeanUtils.setProperty(mainEntity, detailName3, detailList3);
        } catch (Exception ex) {
            throw new RuntimeException("主从关系JSON反序列化实体失败！");
        }
        return mainEntity;
    }

    /***
     * 将JSON文本反序列化为主从关系的实体
     * @param <T> 主实体类型
     * @param jsonString JSON文本
     * @param mainClass 主实体类型
     * @param detailClass 存放了多个从实体在主实体中属性名称和类型
     * @return
     */
    public static <T> T toBean(String jsonString, Class<T> mainClass,
                               HashMap<String, Class> detailClass) {
        JSONObject jsonObject = JSONObject.fromObject(jsonString);
        T mainEntity = JSONUtils.toBean(jsonObject, mainClass);
        for (Object key : detailClass.keySet()) {
            try {
                Class value = (Class) detailClass.get(key);
                BeanUtils.setProperty(mainEntity, key.toString(), value);
            } catch (Exception ex) {
                throw new RuntimeException("主从关系JSON反序列化实体失败！");
            }
        }
        return mainEntity;
    }

    /**
     * 封装json数据从后台传输
     *
     * @param obj
     */
    public static void outPutJson(Object obj) {
        ActionContext context = ActionContext.getContext();
        HttpServletResponse response = (HttpServletResponse) context.get(ServletActionContext.HTTP_RESPONSE);
        try {
            response.getWriter().print(obj);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```