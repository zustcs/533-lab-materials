# Tomcat入门介绍

## 简介

Tomcat是开源的 Java Web 应用服务器，实现了 Java EE(Java Platform Enterprise Edition)的部 分技术规范，比如 Java Servlet、Java Server Page、JSTL、Java WebSocket。Java EE 是 Sun 公 司为企业级应用推出的标准平台，定义了一系列用于企业级开发的技术规范，除了上述的之外，还有 EJB、Java Mail、JPA、JTA、JMS 等，而这些都依赖具体容器的实现。

### 其他的一些Web服务器

![1569239843152](tomcat.assets/1569239843152.png)

## 目录结构

这些是一些关键的tomcat目录：

- **/ bin-**Startup, shutdown和其他脚本。windows为`*.bat`文件，linux为 `*.sh`文件。
- **/ conf-**配置文件和相关的DTDs。这里最重要的文件是server.xml。它是容器的主要配置文件。
- **/ logs-**日志文件默认位于此处。
- **/ webapps-**这是您的webapp所在的位置。



![1569239187061](tomcat.assets/1569239187061.png)

## 工作流程

当客户请求某个资源时，Servlet 容器使用 ServletRequest 对象把客户的请求信息封装起 来，然后调用 Java Servlet API 中定义的 Servlet 的一些生命周期方法，完成 Servlet 的执行， 接着把 Servlet 执行的要返回给客户的结果封装到 ServletResponse 对象中，最后 Servlet 容 器把客户的请求发送给客户，完成为客户的一次服务过程。

## 组织架构

Tomcat是一个基于组件的服务器，它的构成组件都是可配置的，其中最外层的是Catalina servlet容器，其他组件按照一定的格式要求配置在这个顶层容器中。 

Tomcat的各种组件都是在Tomcat安装目录下的/conf/server.xml文件中配置的。

```
<Server>                                     //顶层类元素，可以包括多个Service   
    <Service>                                //顶层类元素，可包含一个Engine，多个Connecter
        <Connector>                          //连接器类元素，代表通信接口
                <Engine>                     //容器类元素，为特定的Service组件处理客户请求，要包含多个Host
                        <Host>               //容器类元素，为特定的虚拟主机组件处理客户请求，可包含多个Context
                                <Context>    //容器类元素，为特定的Web应用处理所有的客户请求
                                </Context>
                        </Host>
                </Engine>
        </Connector>
    </Service>
</Server>
```

所以，tomcat的体系结构如下：

![1569400549940](tomcat.assets/1569400549940.png)

由上图可看出Tomca的心脏是两个组件：Connecter和Container。一个Container可以选择多个Connecter，多个Connector和一个Container就形成了一个Service。Service可以对外提供服务，而Server服务器控制整个Tomcat的生命周期。

## Lifecycle

Lifecycle，其实就是一个状态机，对组件的由生到死状态的管理。我们来看看其总的状态转换图，如下图所示：

![img](https://upload-images.jianshu.io/upload_images/845143-cd70b24b04d6fba4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

### 方法

在Lifecycle中有以下的方法：

```
public interface Lifecycle {
    // 添加监听器
    public void addLifecycleListener(LifecycleListener listener);
    // 获取所以监听器
    public LifecycleListener[] findLifecycleListeners();
    // 移除某个监听器
    public void removeLifecycleListener(LifecycleListener listener);
    // 初始化方法
    public void init() throws LifecycleException;
    // 启动方法
    public void start() throws LifecycleException;
    // 停止方法，和start对应
    public void stop() throws LifecycleException;
    // 销毁方法，和init对应
    public void destroy() throws LifecycleException;
    // 获取生命周期状态
    public LifecycleState getState();
    // 获取字符串类型的生命周期状态
    public String getStateName();
}
```

### LifecycleBase

LifecycleBase是Lifecycle的实现类，重点关注几个方法：

#### 增加、删除和获取监听器

```
/**
 * {@inheritDoc}
 */
@Override
public void addLifecycleListener(LifecycleListener listener) {
    lifecycleListeners.add(listener);
}


/**
 * {@inheritDoc}
 */
@Override
public LifecycleListener[] findLifecycleListeners() {
    return lifecycleListeners.toArray(new LifecycleListener[0]);
}


/**
 * {@inheritDoc}
 */
@Override
public void removeLifecycleListener(LifecycleListener listener) {
    lifecycleListeners.remove(listener);
}
```

1）生命周期监听器保存在一个线程安全的List中，`CopyOnWriteArrayList`。所以add和remove都是直接调用此List的相应方法。

2）findLifecycleListeners返回的是一个数组，为了线程安全，所以这儿会生成一个新数组。

#### init()

```
@Override
public final synchronized void init() throws LifecycleException {
    if (!state.equals(LifecycleState.NEW)) {
        invalidTransition(Lifecycle.BEFORE_INIT_EVENT);
    }

    try {
        setStateInternal(LifecycleState.INITIALIZING, null, false);
        initInternal();
        setStateInternal(LifecycleState.INITIALIZED, null, false);
    } catch (Throwable t) {
        handleSubClassException(t, "lifecycleBase.initFail", toString());
    }
}
```

首先判断是否已为NEW状态，接着进行初始化。并且更改状态。

#### start()

```
@Override
public final synchronized void start() throws LifecycleException {
    if (LifecycleState.STARTING_PREP.equals(state) || LifecycleState.STARTING.equals(state) ||
            LifecycleState.STARTED.equals(state)) {
        if (log.isDebugEnabled()) {
            Exception e = new LifecycleException();
            log.debug(sm.getString("lifecycleBase.alreadyStarted", toString()), e);
        } else if (log.isInfoEnabled()) {
            log.info(sm.getString("lifecycleBase.alreadyStarted", toString()));
        }
        return;
    }
    if (state.equals(LifecycleState.NEW)) {
        init();
    } else if (state.equals(LifecycleState.FAILED)) {
        stop();
    } else if (!state.equals(LifecycleState.INITIALIZED) &&
            !state.equals(LifecycleState.STOPPED)) {
        invalidTransition(Lifecycle.BEFORE_START_EVENT);
    }
    try {
        setStateInternal(LifecycleState.STARTING_PREP, null, false);
        startInternal();
        if (state.equals(LifecycleState.FAILED)) {
            // This is a 'controlled' failure. The component put itself into the
            // FAILED state so call stop() to complete the clean-up.
            stop();
        } else if (!state.equals(LifecycleState.STARTING)) {
            // Shouldn't be necessary but acts as a check that sub-classes are
            // doing what they are supposed to.
            invalidTransition(Lifecycle.AFTER_START_EVENT);
        } else {
            setStateInternal(LifecycleState.STARTED, null, false);
        }
    } catch (Throwable t) {
        // This is an 'uncontrolled' failure so put the component into the
        // FAILED state and throw an exception.
        handleSubClassException(t, "lifecycleBase.startFail", toString());
    }
}
```

与init()类似，也是判断条件后更改state状态。除此之外，还有stop()与destroy()方法等，都类似，所以LifecycleBase是使用了状态机与模板模式来实现的。

# 容器

Servlet容器处理客户端的请求并填充response对象。Servlet容器实现了Container接口。在Tomcat中有4种级别的容器：Engine，Host，Context和Wrapper。

Engine：表示整个Catalina Servlet引擎；

Host：包含一个或多个Context容器的虚拟主机；

Context：表示一个Web应用程序，可以包含多个Wrapper；

Wrapper：表示一个独立的Servlet；

4个层级接口的标准实现分别是：StandardEngine类，StandardHost类，StandardContext类和StandardWrapper类。它们在org.apache.catalina.core包下。

![1569401058596](tomcat.assets/1569401058596.png)

## Engine

### Engine

Engine是表示整个Catalina Servlet引擎的容器。它在以下类型的场景中很有用：

1）希望使用拦截器来查看整个引擎的每个处理请求。

2）希望使用独立的http连接器运行catalina，但仍希望支持多个虚拟主机。

通常，在部署连接到web服务器（如apache）的catalina时，您不会使用引擎，因为连接器将利用web服务器的功能来确定应该使用哪个上下文（甚至可能是哪个Wrapper）来处理此请求。

附加到Engine的子容器通常是Host（表示虚拟Host）或上下文（表示单个servlet上下文）的实现，具体取决于Engine实现。

如果使用，Engine始终是catalina层次结构中的顶级容器。因此，实现的setParent（）方法应该抛出IllegalArgumentException。

### Host

Host是一个容器，表示Catalina Servlet引擎中的虚拟Host。它在以下类型的场景中很有用：

1）希望使用Interceptors来查看此特定虚拟Host处理的每个请求。

2）希望使用独立的http连接器运行catalina，但仍希望支持多个虚拟主机。

通常，在部署连接到web服务器（如apache）的catalina时，您不会使用Host，因为连接器将利用web服务器的功能来确定应该使用哪个上下文（甚至可能是哪个Wrapper）来处理此请求。

Host的父容器通常是一个Engine，但可能是其他一些实现，或者在不需要时可以省略。

主机的子容器通常是Context（表示单个servlet上下文）。

### Context

Context是一个容器，表示catalina servlet引擎中的一个servlet上下文，因此是一个单独的web应用程序。

因此，它在catalina的几乎所有部署中都很有用（即使连接到web服务器（如apache）的连接器使用web服务器的工具来标识处理此请求的适当Wrapper也是如此。

它还提供了一种使用拦截器的方便机制，拦截器可以查看这个特定web应用程序处理的每个请求。

上下文的父容器通常是Host，但可能是其他一些实现，或者在不需要时可以省略。

上下文的子容器通常是Wrapper的实现（表示单个servlet定义）。

### Wrapper

Wrapper是一个容器，它表示来自web应用程序的部署描述符的一个单独的servlet定义。它提供了一种方便的机制来使用拦截器，拦截器可以看到这个定义所表示的对servlet的每个请求。

wrapper的实现负责管理其底层servlet类的servlet生命周期，包括在适当的时候调用init（）和destroy（），以及考虑servlet类本身是否存在单线程模型声明。

Wrapper的父容器通常是context的实现，表示这个servlet在其中执行的servlet上下文（因此是web应用程序）。

Wrapper实现上不允许使用子容器，因此<code>addChild（）</code>方法应该抛出<code>illegalargumentexception</code>。

## Pipeline

每个管道上面都有阀门，`Pipeline`和`Valve`关系也是一样的。`Valve`代表管道上的阀门，可以控制管道的流向，当然每个管道上可以有多个阀门。如果把`Pipeline`比作公路的话，那么`Valve`可以理解为公路上的收费站，车代表`Pipeline`中的内容，那么每个收费站都会对其中的内容做一些处理(收费，查证件等)。

Pipeline描述在调用<code>invoke（）</code>方法时应按顺序执行的阀集合的接口。要求管道中的某个阀门（通常是最后一个）必须处理请求并创建相应的响应，而不是试图传递请求。

通常有一个单独的管道实例与每个容器相关联。容器的正常请求处理功能通常封装在容器特定的阀门中，该阀门应始终在管道的末端执行。为了实现这一点，官方提供了<code>setbasic（）</code>方法来设置总是最后执行的valve实例。在执行基础阀之前，将按照添加顺序执行其他阀门。

基础阀的作用是连接当前容器的下一个容器(通常是自己的自容器),可以说基础阀是两个容器之间的桥梁。运行图如下：

![1569409979371](tomcat.assets/1569409979371.png)

可以看到在同一个`Pipeline`上可以有多个`Valve`,每个`Valve`都可以做一些操作，无论是`Pipeline`还是`Valve`操作的都是`Request`和`Response`。而在容器之间`Pipeline`和`Valve`则起到了桥梁的作用。

### 源码剖析

#### 1）Valve

```
package org.apache.catalina;

import java.io.IOException;

import javax.servlet.ServletException;

import org.apache.catalina.connector.Request;
import org.apache.catalina.connector.Response;

public interface Valve {

    public Valve getNext();
    
    public void setNext(Valve valve);

    public void backgroundProcess();

    public void invoke(Request request, Response response)
        throws IOException, ServletException;

    public boolean isAsyncSupported();
}
```

方法并不是很多，首先，Pipeline上有许多valve，这些valve存放的方式更像是链表，当获取到一个valve实例时，可以通过getNext()获取下一个。setNext则是设置当前valve的下一个valve实例。

#### 2）Pipeline

```
package org.apache.catalina;

import java.util.Set;

public interface Pipeline extends Contained {

    public Valve getBasic();

    public void setBasic(Valve valve);

    public void addValve(Valve valve);

    public Valve[] getValves();

    public void removeValve(Valve valve);

    public Valve getFirst();

    public boolean isAsyncSupported();

    public void findNonAsyncValves(Set<String> result);
}
```

可以看出`Pipeline`中很多的方法都是操作`Valve`的，包括获取，设置，移除`Valve`,`getFirst()`返回的是`Pipeline`上的第一个`Valve`,而`getBasic()`,`setBasic()`则是获取/设置基础阀,我们都知道在`Pipeline`中，每个`pipeline`至少都有一个阀门，叫做基础阀，而`getBasic()`,`setBasic()`则是操作基础阀的。

接下来 ，我们看实现类**StandardPipeline**的几个重要方法。

##### 1：startInternal

```
protected synchronized void startInternal() throws LifecycleException {

    // Start the Valves in our pipeline (including the basic), if any
    Valve current = first;
    if (current == null) {
        current = basic;
    }
    while (current != null) {
        if (current instanceof Lifecycle)
            ((Lifecycle) current).start();
        current = current.getNext();
    }
 
    setState(LifecycleState.STARTING);
}
```

组件的`start()`方法，将`first`(第一个阀门)赋值给`current`变量，如果`current`为空，就将`basic`(也就是基础阀)赋值给`current`,接下来遍历单向链表，调用每个对象的`start()`方法，最后将组件(`pipeline`)状态设置为`STARTING`(启动中)。

##### 2：setBasic

```
public void setBasic(Valve valve) {

    // 只有必要时才会改变
    Valve oldBasic = this.basic;
    if (oldBasic == valve)
        return;

    // 条件符合的话，停止旧的基础阀
    if (oldBasic != null) {
        if (getState().isAvailable() && (oldBasic instanceof Lifecycle)) {
            try {
                ((Lifecycle) oldBasic).stop();
            } catch (LifecycleException e) {
                log.error(sm.getString("standardPipeline.basic.stop"), e);
            }
        }
        if (oldBasic instanceof Contained) {
            try {
                ((Contained) oldBasic).setContainer(null);
            } catch (Throwable t) {
                ExceptionUtils.handleThrowable(t);
            }
        }
    }

    // 条件符合的话，开启valve
    if (valve == null)
        return;
    if (valve instanceof Contained) {
        ((Contained) valve).setContainer(this.container);
    }
    if (getState().isAvailable() && valve instanceof Lifecycle) {
        try {
            ((Lifecycle) valve).start();
        } catch (LifecycleException e) {
            log.error(sm.getString("standardPipeline.basic.start"), e);
            return;
        }
    }

    // 更新pipeline的阀
    Valve current = first;
    while (current != null) {
        if (current.getNext() == oldBasic) {
            current.setNext(valve);
            break;
        }
        current = current.getNext();
    }

    this.basic = valve;

}
```

这是用来设置基础阀的方法，这个方法在每个容器的构造函数中调用，代码逻辑也比较简单，稍微注意的地方就是阀门链表的遍历。

##### 3：addValve

```
public void addValve(Valve valve) {

    // 验证Valve 关联Container
    if (valve instanceof Contained)
            ((Contained) valve).setContainer(this.container);
            
    // 如果符合条件，就启动valve
    if (getState().isAvailable()) {
        if (valve instanceof Lifecycle) {
            try {
                ((Lifecycle) valve).start();
            } catch (LifecycleException e) {
                log.error(sm.getString("standardPipeline.valve.start"), e);
            }
        }
  
    // 如果first为空，就将valve赋值给first，并将下个valve设置为基础阀（因为为空说明只有一个基础阀）
    if (first == null) {
        first = valve;
        valve.setNext(basic);
    } else {
    	// 遍历阀门链表，将valve设置在基础阀之前
        Valve current = first;
        while (current != null) {
            if (current.getNext() == basic) {
                current.setNext(valve);
                valve.setNext(basic);
                break;
            }
            current = current.getNext();
        }
        
  	//触发添加事件
    container.fireContainerEvent(Container.ADD_VALVE_EVENT, valve);
}
```

这方法是像容器中添加`Valve`，在`server.xml`解析的时候也会调用该方法。

##### 4：getValves

```
public Valve[] getValves() {

    List<Valve> valveList = new ArrayList<>();
    Valve current = first;
    if (current == null) {
        current = basic;
    }
    while (current != null) {
        valveList.add(current);
        current = current.getNext();
    }

    return valveList.toArray(new Valve[0]);

}
```

获取所有的阀门,其实就是将阀门链表添加到一个集合内，最后转成数组返回。

##### 5：removeValve

```
public void removeValve(Valve valve) {

    Valve current;
    
    // 如果first是要被删除的结点，那么将first指向下一位，current置空
    if(first == valve) {
        first = first.getNext();
        current = null;
    } else {
    	// 将current指向first
        current = first;
    }
    while (current != null) {
    	// 类似链表删除，将要被删除的valve前一位指向它的后一位（valve不会是first）
        if (current.getNext() == valve) {
            current.setNext(valve.getNext());
            break;
        }
        current = current.getNext();
    }

	// 如果first==basic，first置空。first严格定义是 除了基础阀的第一个阀门。
    if (first == basic) first = null;

    if (valve instanceof Contained)
        ((Contained) valve).setContainer(null);

	// 停用并销毁valve
    if (valve instanceof Lifecycle) {
        // Stop this valve if necessary
        if (getState().isAvailable()) {
            try {
                ((Lifecycle) valve).stop();
            } catch (LifecycleException e) {
                log.error(sm.getString("standardPipeline.valve.stop"), e);
            }
        }
        try {
            ((Lifecycle) valve).destroy();
        } catch (LifecycleException e) {
            log.error(sm.getString("standardPipeline.valve.destroy"), e);
        }
    }

	// 触发container的移除valve事件
    container.fireContainerEvent(Container.REMOVE_VALVE_EVENT, valve);
}
```

它用来删除指定的valve，在destroyInternal方法中被调用。

##### 6：getFirst

```
public Valve getFirst() {
    if (first != null) {
        return first;
    }

    return basic;
}
```

在方法5中我们也看到了，`first`指向的是容器第一个非基础阀门的阀门，从方法6中也可以看出来，`first`在只有一个基础阀的时候并不会指向基础阀，因为如果指向基础阀的话就不需要判断非空然后返回基础阀了，这是个需要注意的点！

## AccessLog

用于valve以指示valve提供访问日志记录。tomcat内部使用它来标识记录访问请求的阀门，以便在处理链的早期被拒绝的请求仍然可以添加到访问日志中。

此接口的实现应该是健壮的，以防提供的request和response对象为空、具有空属性或任何其他“异常”，这些“异常”可能是由于试图记录一个几乎肯定被拒绝的请求，因为该请求的格式不正确。

其中，AccessLog的配置可以在server.xml的Host中找到：

```
<Host name="localhost"  appBase="webapps"
      unpackWARs="true" autoDeploy="true">

  <!-- SingleSignOn valve, share authentication between web applications
       Documentation at: /docs/config/valve.html -->
  <!--
  <Valve className="org.apache.catalina.authenticator.SingleSignOn" />
  -->

  <!-- Access log processes all example.
       Documentation at: /docs/config/valve.html
       Note: The pattern used is equivalent to using pattern="common" -->
  <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
         prefix="localhost_access_log" suffix=".txt"
         pattern="%h %l %u %t &quot;%r&quot; %s %b" />

</Host>
```

Access Log Valve用来创建日志文件，它可以与任何Catalina容器关联，记录该容器处理的所有请求。输出文件将放在由directory属性指定的目录中。文件名由配置的前缀、时间戳和后缀的串联组成。文件名中时间戳的格式可以使用filedateformat属性设置。如果通过将rotatable设置为false来关闭文件旋转，则将省略此时间戳。pattern项的修改，可以改变日志输出的内容。

参数/选项说明：

| 参数           | 含义                                                         |
| -------------- | ------------------------------------------------------------ |
| className      | 实现的java类名，必须设置成org.apache.catalina.valves.AccessLogValve |
| directory      | 存放日志文件的目录，如果指定了相对路径，则会将其解释为相对于$catalina_base。如果未指定目录属性，则默认值为“logs”（相对于$catalina_base）。 |
| pattern        | 需要记录的日志信息的格式布局，如果是”common”或者”combined”，说明是使用的标准记录格式，也有自定义的格式，下面会详细说明 |
| prefix         | 日志文件名的前缀，如果没有指定，缺省值是”access_log.；（要注意后面有个小点） |
| resolveHosts   | 将远端主机的IP通过DNS查询转换成主机名，设为true。如果为false，忽略DNS查询，报告远端主机的IP地址 |
| sufix          | 日志文件的后缀名。（sufix=”.log”）；也需要注意有个小点       |
| rotatable      | 缺省值为true，决定日志是否要翻转，如果为false则永不翻转，并且忽略fileDateFormat，谨慎使用。 |
| condition      | 打开条件日志                                                 |
| fileDateFormat | 允许在日志文件名称中使用定制的日期格式。日志的格式也决定了日志文件翻转的频率。 |

Pattern：

​	%a    远端IP
​    %A    本地IP
​    %b    发送的字节数，不包含HTTP头，如果为0，使用”-”
​    %B    发送的字节数，不包含HTTP头
​    %h    远端主机名（如果resolveHosts=false），远端的IP
​    %H    请求协议
​    %l      从identd返回的远端逻辑用户名，总是返回’-’
​    %m   请求的方法
​    %p     收到请求的本地端口号
​    %q     查询字符串
​    %r      请求的第一行
​    %s　 响应的状态码
​    %S     用户的sessionID
​    %t     日志和时间，使用通常的log格式
​    %u    认证以后的远端用户（如果存在的话，否则为’-’）
​    %U    请求的ＵＲＩ路径
​    %v    本地服务器的名称
​    %D　处理请求的时间，以毫秒为单位
​    %T    处理请求的时间，以秒为单位

## Realm

首先说一下什么是Realm，可以把它理解成“域”，也可以理解成“组”，因为它类似 类Unix系统 中组的概念。

Realm域提供了一种用户密码与web应用的映射关系。

因为tomcat中可以同时部署多个应用，因此并不是每个管理者都有权限去访问或者使用这些应用，因此出现了用户的概念。但是想想，如果每个应用都去配置具有权限的用户，那是一件很麻烦的事情，因此出现了role这样一个概念。具有某一角色，就可以访问该角色对应的应用，从而达到一种域的效果。

每个用户我们可以设置不同的角色（在tomcat-users.xml中配置）。

每个应用中会设定可以访问的角色（在web.xml中配置）。

当tomcat启动后，就会通过Realm进行验证（在server.xml中配置），通过验证才可以访问该应用，从而达到角色安全管理的作用。

### server.xml

```
<Realm className="org.apache.catalina.realm.LockOutRealm">
  <!-- This Realm uses the UserDatabase configured in the global JNDI
       resources under the key "UserDatabase".  Any edits
       that are performed against this UserDatabase are immediately
       available for use by the Realm.  -->
  <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
         resourceName="UserDatabase"/>
</Realm>
```

默认情况下，Realm的配置位置是在Engine标签内部，并且使用的是UserDatabase的方式。其他的方式会在下面部分说明。

其中Realm的不同位置也会影响到它作用的范围。

>1 、在<Engine>元素内部 —— Realm将会被所有的虚拟主机上的web应用共享，除非它被<Host>或者<Context>元素内部的Realm元素重写。
>
>2 、在<Host>元素内部 —— 这个Realm将会被本地的虚拟主机中的所有的web应用共享，除非被<Context>元素内部的Realm元素重写。
>
>3 、在<Context>元素内部 —— 这个Realm元素仅仅被该Context指定的应用使用。

### 配置

#### 1）配置server.xml

![img](https://images0.cnblogs.com/blog2015/449064/201506/041927237389043.png)

上图中的代码配置了UserDatabase的目录文件，为conf/tomcat-users.xml。

#### 2）在tomcat-users.xml中配置用户密码以及分配角色

![1569418487374](tomcat.assets/1569418487374.png)

#### 3）在应用的web.xml中配置访问角色以及安全限制的内容

````
<security-constraint>
    <web-resource-collection>
      <web-resource-name>HTML Manager interface (for humans)</web-resource-name>
      <url-pattern>/html/*</url-pattern>
    </web-resource-collection>
    <auth-constraint>
       <role-name>manager-gui</role-name>
    </auth-constraint>
  </security-constraint>
  <security-constraint>
    <web-resource-collection>
      <web-resource-name>Text Manager interface (for scripts)</web-resource-name>
      <url-pattern>/text/*</url-pattern>
    </web-resource-collection>
    <auth-constraint>
       <role-name>manager-script</role-name>
    </auth-constraint>
  </security-constraint>
  <security-constraint>
    <web-resource-collection>
      <web-resource-name>JMX Proxy interface</web-resource-name>
      <url-pattern>/jmxproxy/*</url-pattern>
    </web-resource-collection>
    <auth-constraint>
       <role-name>manager-jmx</role-name>
    </auth-constraint>
  </security-constraint>
  <security-constraint>
    <web-resource-collection>
      <web-resource-name>Status interface</web-resource-name>
      <url-pattern>/status/*</url-pattern>
    </web-resource-collection>
    <auth-constraint>
       <role-name>manager-gui</role-name>
       <role-name>manager-script</role-name>
       <role-name>manager-jmx</role-name>
       <role-name>manager-status</role-name>
    </auth-constraint>
  </security-constraint>

  <!-- Define the Login Configuration for this Application -->
  <login-config>
    <auth-method>BASIC</auth-method>
    <realm-name>Tomcat Manager Application</realm-name>
  </login-config>

  <!-- Security roles referenced by this web application -->
  <security-role>
    <description>
      The role that is required to access the HTML Manager pages
    </description>
    <role-name>manager-gui</role-name>
  </security-role>
  <security-role>
    <description>
      The role that is required to access the text Manager pages
    </description>
    <role-name>manager-script</role-name>
  </security-role>
  <security-role>
    <description>
      The role that is required to access the HTML JMX Proxy
    </description>
    <role-name>manager-jmx</role-name>
  </security-role>
  <security-role>
    <description>
      The role that is required to access to the Manager Status pages
    </description>
    <role-name>manager-status</role-name>
  </security-role>
````

这是manager项目中的web.xml中的内容，其中,role-name定义了可以访问的角色。其他内容中上面定义了限制访问的资源，下面的Login-config比较重要。

它定义了验证的方式，BASIC就是基本的弹出对话框输入用户名密码。还是DIGEST方式，这种方式会对网络中的传输信息进行加密，更安全。

# Jasper解析器

Jasper负责jsp页面的解析，jsp属性的验证，同时也负责将jsp页面动态转换为java代码并编译成类文件。

...有待完善

# Connector

Connector 用于接收请求并将请求封装成Request 和Response 来具体处理，最底层是使用Socket 来进行连接的， Request 和Response 是按照HTTP 协议来封装的，所以Connector 同时实现了TCP/IP 协议和HTTP 协议， Request 和Response 封装完之后交给Container 进行处理，Container 就是Servlet 的容器， Container 处理完之后返回给Connector，最后Connector 使用Socket 将处理结果返回给客户端，这样整个请求就处理完了。

## 结构

Connector主要包含三个模块：`Http11NioProtocol(Endpoint+processor)`, `Mapper`, `CoyoteAdapter`，http请求在Connector中的流程如下

![1570626040676](tomcat.assets/1570626040676.png)

Connector 中具体是用ProtocolHandler 来处理请求的，不同的ProtocolHandler 代表不同的连接类型，比如， Http11Protocol 使用的是普通Socket 来连接的， Http 11 NioProtocol 使用的是NioSocket 来连接的。

ProtocolHandler 里面有2 个非常重要的组件： Endpoint 、Processor。

> Endpoint：用于处理底层Socket 的网络连接。
> Processor：用于将Endpoint 接收到的Socket 封装成Request。

也就是说Endpoint用来实现TCP/IP 协议， Processor 用来实现HTTP 协议。

Endpoint 的抽象实现AbstractEndpoint 里面定义的Acceptor 和AsyncTimeout 两个内部类和一个Handler 接口。Acceptor 用于监昕请求， AsyncTimeout 用于检查异步request 的超时，Handler 用于处理接收到的Socket，在内部调用了Processor 进行处理。

## Connector类

Connector 类本身的作用主要是在其创建时创建ProtocolHandler，然后在生命周期的相关方法中调用了ProtocolHandler 的相关生命周期方法。

Connector 的使用方法是通过Connector 标签配置在conf/server.xml 文件中，所以Connector 是在Catalina 的load 方法中根据conf/server.xml 配置文件创建Server对象时创建的。Connector 的生命周期方法是在Service 中调用的。如下：

```xml
<Service name="Catalina">
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
</Service>
```

### Connector的创建

Connector 的创建过程主要是初始化ProtocolHandler。server.xrnl 配置文件中Connector 标签的protocol 属性会设置到Connector 构造函数的参数中，它用于指定ProtocolHandler 的类型，Connector 的构造函数代码如下：

```java
public Connector(String protocol) {
    boolean aprConnector = AprLifecycleListener.isAprAvailable() &&
            AprLifecycleListener.getUseAprConnector();
    if ("HTTP/1.1".equals(protocol) || protocol == null) {
        if (aprConnector) {
            protocolHandlerClassName = "org.apache.coyote.http11.Http11AprProtocol";
        } else {
            protocolHandlerClassName = "org.apache.coyote.http11.Http11NioProtocol";
        }
    } else if ("AJP/1.3".equals(protocol)) {
        if (aprConnector) {
            protocolHandlerClassName = "org.apache.coyote.ajp.AjpAprProtocol";
        } else {
            protocolHandlerClassName = "org.apache.coyote.ajp.AjpNioProtocol";
        }
    } else {
        protocolHandlerClassName = protocol;
    }
    // Instantiate protocol handler
    ProtocolHandler p = null;
    try {
        Class<?> clazz = Class.forName(protocolHandlerClassName);
        p = (ProtocolHandler) clazz.getConstructor().newInstance();
    } catch (Exception e) {
        log.error(sm.getString(
                "coyoteConnector.protocolHandlerInstantiationFailed"), e);
    } finally {
        this.protocolHandler = p;
    }
    // Default for Connector depends on this system property
    setThrowOnFailure(Boolean.getBoolean("org.apache.catalina.startup.EXIT_ON_INIT_FAILURE"));
}
```

Apr 是Apache Portable Runtime 的缩写，是Apache 提供的一个运行时环境，如果要使用Apr 需要先安装，安装后Tomcat 可以自己检测出来。如果安装了Apr, 方法会根据配置的HTTP/1.1 属性对应地将protocolHandlerClassName 设置为org.apache.coyote.http11.Http11.AprProtocol ，如果没有安装Apr，会根据配置的HTTP/1.1 属性将protocoHandlerClassName设置为com..apache.coyote.http11.Http11NioProtocol，然后就会根据protocolHandlerClassName 来创建ProtocolHandler。

## ProtocolHandler

ProtocolHandler 有一个抽象实现类AbstractProtocol, AbstractProtocol 下面分了三种类型： Ajp 、HTTP 和Spdy 。

Ajp是Apache JServ Protocol 的缩写， Apache 的定向包协议，主要用于与前端服务器（如Apache ）进行通信，它是长连接，不需要每次通信都重新建立连接，这样就节省了开销； Spdy 协议Google开发的协议，作用类似HTTP ，比HTTP 效率高，不过这只是Google 制定的企业级协议，使用并不广泛，而且在HTTP/2 协议中已经包含了Spdy 所提供的优势，所以Spdy 协议平常很少使用，不过Tomcat 提供了支持。

以默认配置中的Http11NioProtocol为例来分析，它使用HTTP11协议， TCP 层使用NioSocket 来传输数据。构造方法如下：

```java
public Http11NioProtocol() {    super(new NioEndpoint());}
```

```java
public AbstractHttp11Protocol(AbstractEndpoint<S,?> endpoint) {
    super(endpoint);
    setConnectionTimeout(Constants.DEFAULT_CONNECTION_TIMEOUT);
    ConnectionHandler<S> cHandler = new ConnectionHandler<>(this);
    setHandler(cHandler);
    getEndpoint().setHandler(cHandler);
}
```

可见在构造方法中创建了endpoint。

### Endpoint

Endpoint 用于处理具体连接和传输数据， NioEndpoint 继承自org.apache.tomcat. util.net.AbstractEndpoint，在NioEndpoint 中新增了Poller 和SocketProcessor 内部类， NioEndpoint 中处理请求的具体流程如图:

![](https://user-gold-cdn.xitu.io/2019/10/10/16db4a5737734335?w=704&h=107&f=png&s=40103)

其主要调用bind()方法进行初始化：

```java
public void bind() throws Exception {
    initServerSocket();

    setStopLatch(new CountDownLatch(1));

    // Initialize SSL if needed
    initialiseSsl();

    selectorPool.open(getName());
}
```

```java
protected void initServerSocket() throws Exception {
    if (!getUseInheritedChannel()) {
    	// 初始化socket
        serverSock = ServerSocketChannel.open();
        socketProperties.setProperties(serverSock.socket());
        InetSocketAddress addr = new InetSocketAddress(getAddress(), getPortWithOffset());
        serverSock.socket().bind(addr,getAcceptCount());
    } else {
        // Retrieve the channel provided by the OS
        Channel ic = System.inheritedChannel();
        ...
    }
    serverSock.configureBlocking(true); //mimic APR behavior
}
```

初始化主要是建立socket连接，接着看生命周期的startInternal方法：

```java
public void startInternal() throws Exception {

	// 创建socketProcessor
	if (socketProperties.getProcessorCache() != 0) {
    processorCache = new SynchronizedStack<>(SynchronizedStack.DEFAULT_SIZE,
            socketProperties.getProcessorCache());
	}
	if (socketProperties.getEventCache() != 0) {
    	eventCache = new SynchronizedStack<>(SynchronizedStack.DEFAULT_SIZE,
            	socketProperties.getEventCache());
	}
	if (socketProperties.getBufferPool() != 0) {
    	nioChannels = new SynchronizedStack<>(SynchronizedStack.DEFAULT_SIZE,
            	socketProperties.getBufferPool());
	}
    ...
    
    // 启动poller线程
    poller = new Poller();
    Thread pollerThread = new Thread(poller, getName() + "-ClientPoller");
    pollerThread.setPriority(threadPriority);
    pollerThread.setDaemon(true);
    pollerThread.start();
    startAcceptorThread();
    
}
```

可以发现它创建了一个Poller以及启动Poller线程与Acceptor线程来处理请求。同时它还初始化了SocketProcessor，这是它的内部类，Poller收到请求后就会交由它处理。而SocketProcessor又会将请求传递到Handler，如下：

```java
state = getHandler().process(socketWrapper, SocketEvent.OPEN_READ);
```

最后由handler将请求传递给processor。

### Processor

Processor的结构如下：

![](https://user-gold-cdn.xitu.io/2019/10/10/16db4a5a30fb6cc4?w=719&h=288&f=png&s=276228)

当Handler建立好socket连接后，请求将会交由Processor处理，AbstractProcessor构造方法如下：

```java
protected AbstractProcessor(Adapter adapter, Request coyoteRequest, Response coyoteResponse) {
    this.adapter = adapter;
    asyncStateMachine = new AsyncStateMachine(this);
    request = coyoteRequest;
    response = coyoteResponse;
    response.setHook(this);
    request.setResponse(response);
    request.setHook(this);
    userDataHelper = new UserDataHelper(getLog());
}
```

之后它将调用Adapter 将请求传递到Container 中，最后对处理的结果进行了处理，如有没有启动异步处理、处理过程巾有没有抛出异常等。主要体现在Service方法中：

```java
public SocketState service(SocketWrapperBase<?> socketWrapper)
    throws IOException {
    
    ...
    
    // Process the request in the adapter
    if (getErrorState().isIoAllowed()) {
    
        rp.setStage(org.apache.coyote.Constants.STAGE_SERVICE);
        getAdapter().service(request, response);
        
     	...
    }
    
    // Finish the handling of the request
    ...
}
```

## Adaper

Adapter 只有一个实现类，那就是org.apache.catalina.connector 包下的CoyoteAdapter 类。

Processor 在其process 方法中会调用Adapter 的service 方法来处理请求， Adapter 的service 方法主要是调用Container 管道中的invoke方法来处理请求，在处理之前对Request和Response做了处理，将原来创建的org.apache.coyote 包下的Request 和Response 封装成了org.apache.catal ina.connector 的Request 和Response ，并在处理完成后判断再启动了Comet（长连接推模式）和是否启动了异步请求，并作出相应处理。调用Container 管道的相应代码片段如下：

```java
connector.getService().getContainer().getPipeline().getFirst().invoke(        request, response);
```

我们知道，tomcat有四个Container，采用了责任链的设计模式，每一个Container定义了一个Pipeline，每一个Pipeline又定义了多个Valve，代表需要处理的任务。Pipeline就像是每个容器的逻辑总线，在Pipeline上按照配置的顺序，加载各个Valve。通过Pipeline完成各个Valve之间的调用，各个Valve实现具体的应用逻辑。

有了责任链设计模式的概念，http请求由Connector转发至Container，在Container中的流程就比较清晰了，如下：

![](https://user-gold-cdn.xitu.io/2019/10/10/16db4a5d0a134d2d?w=566&h=432&f=png&s=88246)
最终将Response返回给Connector完成一次http的请求。

## Mapper

在Tomcat中，当一个请求到达时，该请求最终由哪个Servlet来处理是靠Mapper路由映射器完成的。Mapper由Service管理。

### 存储结构

![](https://user-gold-cdn.xitu.io/2019/10/10/16db4a5eb522a3ff?w=451&h=270&f=png&s=33047)

### MapElement

```java
protected abstract static class MapElement<T> {

    public final String name;   // 名字

    public final T object;      // 对应的对象，如host, context, wrapper

}
```

### MappedHost

```java
protected static final class MappedHost extends MapElement<Host> {

    // host包含的context列表，即MappedContext数组的包装
    public volatile ContextList contextList;
}
```

### MappedContext

```java
protected static final class MappedContext extends MapElement<Void> {

    // 一个Context可能会对应许多个不同的版本的context，一般情况下是1个
    public volatile ContextVersion[] versions;

}
```

其中ContextVersion包含了Context下的所有Servlet，有多种映射方式，如精确的map，通配符的map，扩展名的map，如下：

```java
protected static final class ContextVersion extends MapElement<Context> {

    // 对wrapper的精确的map
    public MappedWrapper[] exactWrappers = new MappedWrapper[0];

    // 基于通配符的map
    public MappedWrapper[] wildcardWrappers = new MappedWrapper[0];

    // 基于扩展名的map
    public MappedWrapper[] extensionWrappers = new MappedWrapper[0];
}
```

### MappedWrapper

```java
protected static class MappedWrapper extends MapElement<Wrapper> {
    public final boolean jspWildCard;
    public final boolean resourceOnly;
}
```

### Mapper类

```java
public final class Mapper {

    // host数组，host里面又包括了context和wrapper数组
    volatile MappedHost[] hosts = new MappedHost[0];

    // 下面三个是添加host, context, wrapper的函数，都是同步的
    // 而且保证添加后是有序的
    public synchronized void addHost(String name, String[] aliases, Host host) { }

    public void addContextVersion(String hostName, Host host, String path,
            String version, Context context, String[] welcomeResources,
            WebResourceRoot resources, Collection<WrapperMappingInfo> wrappers) {

    }

    protected void addWrapper(ContextVersion context, String path,
            Wrapper wrapper, boolean jspWildCard, boolean resourceOnly) {

    }

    // 根据name，查找一个MapElement（host, context, 或者wrapper)
    private static final <T> int find(MapElement<T>[] map, CharChunk name,
            int start, int end) {

        // ...省略

        // 核心是二分法
        while (true) {
            i = (b + a) >>> 1;
            int result = compare(name, start, end, map[i].name);
            if (result == 1) {
                a = i;
            } else if (result == 0) {
                return i;
            } else {
                b = i;
            }
            if ((b - a) == 1) {
                int result2 = compare(name, start, end, map[b].name);
                if (result2 < 0) {
                    return a;
                } else {
                    return b;
                }
            }
        }
    }
}
```

### 体现

下面介绍在一次http请求中，哪个环节调用了Mapper作路由映射，已经路由映射的过程。

http请求经过`http11Processor`解析之后，会调用`CoyoteAdapter`的`service()`函数转发给`Container`，我们来详细看一下这一步。

```java
public void service(org.apache.coyote.Request req, org.apache.coyote.Response res)
        throws Exception {
    ...
    
    try {
        // 解析并配置请求参数
        postParseSuccess = postParseRequest(req, request, res, response);
        
        if (postParseSuccess) {
            //check valves if we support async
            request.setAsyncSupported(
                 connector.getService().getContainer().getPipeline().isAsyncSupported());
            // 调用container
            connector.getService().getContainer().getPipeline().getFirst().invoke(
                    request, response);
        }
    }catch{
    	....
    }
    
    ...
}
```

我们进入postParseRequest方法：

```java
protected boolean postParseRequest(org.apache.coyote.Request req, Request request,
        org.apache.coyote.Response res, Response response) throws IOException, ServletException {
    
    ...
    
    while (mapRequired) {
        // This will map the the latest version by default
        connector.getService().getMapper().map(serverName, decodedURI,
                version, request.getMappingData());
		
		...
    }
    
    ...
}
```

`CoyoteAdapter`会获取`Connector`中的`Service`中的`Mapper`，然后调用`map()`方法。

```java
public final class Mapper {

    public void map(MessageBytes host, MessageBytes uri, String version,
            MappingData mappingData) throws IOException {
        
        ...
        
        // 调用私有方法internalMap，传入host, uri, version, 结果将会保存在mappingData
        internalMap(host.getCharChunk(), uri.getCharChunk(), version,
                mappingData);
    }

    private final void internalMap(CharChunk host, CharChunk uri,
            String version, MappingData mappingData) throws IOException {

        ...

        // 查找映射的host
        MappedHost[] hosts = this.hosts;
        // 跟mapper的find方法一样，采用二分法查找
        MappedHost mappedHost = exactFindIgnoreCase(hosts, host);
        mappingData.host = mappedHost.object;
        
        ...
        
         // 查找映射的context
        contextVersion = exactFind(contextVersions, version);


        // 查找映射的wrapper
        if (!contextVersion.isPaused()) {
            internalMapWrapper(contextVersion, uri, mappingData);
        }
    }
}
```

# 线程模型

## 种类

tomcat一共有四种线程模型，如下：

| 名称 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| BIO  | 阻塞式IO，采用传统的java IO进行操作，该模式下每个请求都会创建一个线程，适用于并发量小的环境 |
| NIO  | 同步非阻塞，tomcat8.0后的默认模式                            |
| APR  | tomcat以JNI形式调用http服务器的核心动态链接库来处理文件读取和网络传输操作，需要编译安装APR库 |
| AIO  | 异步非阻塞，tomcat8.0后支持                                  |

## 配置方式

默认的配置文件：

```xml
<Service name="Catalina">
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
</Service>
```

只要将protocol替换为以下几种即可：

**BIO**:  protocol =" org.apache.coyote.http11.Http11Protocol"   (tomcat9不支持)

**NIO**: protocol ="org.apache.coyote.http11.Http11NioProtocol"

**AIO**: protocol ="org.apache.coyote.http11.Http11Nio2Protocol"

**APR**: protocol ="org.apache.coyote.http11.Http11AprProtocol"

## 概念介绍

### 同步、异步

同步：自己亲自去买饭。（Java自己处理IO读写）

异步：点外卖，自己用走路去饭馆的时间去干别的事（Java将IO读写委托给操作系统处理，需要将数据缓冲区地址和大小传给操作系统（送餐地址，数量）

### 阻塞、非阻塞

阻塞：食堂排队，只能等待（使用阻塞IO是，Java调用会一直阻塞到读写完成才返回）。

非阻塞：取号，坐在位置上休息，等阿姨喊，同时可以询问是否轮到自己了（使用非阻塞IO时，如果不能读写，Java调用会马上返回，当IO事件分发器会通知可读写时再继续读写 ，不断循环到读写完成）。

### Java对BIO、NIO、AIO的支持

Java BIO ： 同步并阻塞，服务器实现模式为一个连接一个线程，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销，当然可以通过线程池机制改善。

Java NIO ： 同步非阻塞，服务器实现模式为一个请求一个线程，即客户端发送的连接请求都会注册到多路复用器上，多路复用器轮询到连接有I/O请求时才启动一个线程进行处理。

Java AIO(NIO.2) ： 异步非阻塞，服务器实现模式为一个有效请求一个线程，客户端的I/O请求都是由OS先完成了再通知服务器应用去启动线程进行处理

### 使用场景

BIO方式适用于连接数目比较小且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4以前的唯一选择，但程序直观简单易理解。

NIO方式适用于连接数目多且连接比较短（轻操作）的架构，比如聊天服务器，并发局限于应用中，编程比较复杂，JDK1.4开始支持。

AIO方式使用于连接数目多且连接比较长（重操作）的架构，比如相册服务器，充分调用OS参与并发操作，编程比较复杂，JDK7开始支持。

## 线程模型

### BIO

![1570697894653](tomcat.assets/1570697894653.png)

启动时，JIoEndpoint组件内部的Acceptor组件将启动某个端口的监听，一个请求到来后将被扔进线程池Executor，线程池进行任务处理处理，处理过程中将通过Http11Processor组件对HTTP协议解析并传递到Engine容器继续处理。

#### LimitLatch

Tomcat的流量控制是通过AQS（AbstractQueuedSynchronizer）并发框架实现的，通过AQS实现更有灵活性和定制型。

LimitLatch用来控制tomcat的流量，每接收一个套接字(Socket)那么count+1，反之则减少 。如果超过最大限制，AQS将接收线程阻塞，停止接收套接字，直到某些套接字处理完关闭后重新唤起接收线程往下接收套接字。

Tomcat默认同时接收的客户端连接数为200，但可以通过server.xml中的`<Connector>`节点的`maxConnections`进行调节，BIO模式下，最大连接数与最大线程数比例为1:1。

![1570699101285](tomcat.assets/1570699101285.png)

同时，与最大连接数相关的还有`acceptCount`参数，默认值为100。可以参考一下情形：

> 1）接受一个请求，此时tomcat起动的线程数没有到达maxThreads，tomcat会起动一个线程来处理此请求。
>
> 2）接受一个请求，此时tomcat起动的线程数已经到达maxThreads，tomcat会把此请求放入等待队列，等待空闲线程。
>
> 3）接受一个请求，此时tomcat起动的线程数已经到达maxThreads，等待队列中的请求个数也达到了acceptCount，此时tomcat会直接拒绝此次请求，返回connection refused。



### NIO

![1570697978959](tomcat.assets/1570697978959.png)

与BIO相比，NIO多了Poller组件，它可以在非阻塞I/O方式下轮询多个客户端连接，不断检测，处理各种事件，例如不断检测各个连接是否可读，对于可读的客户端连接尝试进行读取并解析消息报文。

![1570699420758](tomcat.assets/1570699420758.png)

Poller池的大小应为：`Math.min(2,Runtime.getRuntime().availableProcessors())`

## 压测并发测试

### Jmeter工具下载与使用

#### 1）下载

打开Jmeter官网，进行工具的下载，http://jmeter.apache.org/download_jmeter.cgi

![1571046532330](tomcat.assets/1571046532330.png)

#### 2）解压

解压到相应的目录后，以管理员的身份打开bin目录下的jmeter.bat。

![1571046592361](tomcat.assets/1571046592361.png)

启动后等待一段时间会弹出图形化界面：

![1571046732930](tomcat.assets/1571046732930.png)

#### 3）使用

启动一个java web程序，开放端口localhost:8080，使用Jmeter对其进行压测，步骤如下：

##### 1：新建线程组

![1571047346233](tomcat.assets/1571047346233.png)

##### 2：设置线程组参数

![1571047464141](tomcat.assets/1571047464141.png)

##### 3：新增http请求默认值

![1571047584713](tomcat.assets/1571047584713.png)

![1571047672240](tomcat.assets/1571047672240.png)

##### 4：添加要压测的http请求

![1571047917793](tomcat.assets/1571047917793.png) 

下图第一个红框内的协议、IP、端口不需要设置，会使用步骤3中设置的默认值，只需设置请求路径`Path`即可，这里填入`/task2_1_war_exploded/hello`

![1571048071960](tomcat.assets/1571048071960.png)

##### 5：新增监听器，用于查看压测结果。

![1571048238722](tomcat.assets/1571048238722.png)

##### 6：运行

![1571048544979](tomcat.assets/1571048544979.png)

> 1）Label：每个 JMeter 的 element（例如 HTTP Request）都有一个 Name 属性，label显示的就是 Name 属性的值 。
>
> 2）Samples：表示你这次测试中一共发出了多少个请求。  
>
> 3）Average：平均响应时间——默认情况下是单个 Request 的平均响应时间，当使用了 Transaction Controller 时，也可以以Transaction 为单位显示平均响应时间。（单位ms）   
>
> 4）Median：中位数，也就是 50％ 用户的响应时间。   
>
> 5）90% Line：90％ 用户的响应时间。
>
> 6）Min：最小响应时间。   
>
> 7）Max：最大响应时间。   
>
> 8）Error%：本次测试中出现错误的请求的数量/请求的总数。   
>
> 9）Throughput：吞吐量——默认情况下表示每秒完成的请求数（Request per Second），当使用了 Transaction Controller 时，也可以表示类似 LoadRunner 的 Transaction per Second 数。   
>
> 10）KB/Sec：每秒从服务器端接收到的数据量，相当于LoadRunner中的Throughput/Sec 。   

| tomcat模式 | 并发数 | 样本数 | 平均响应时间 | 吞吐量   | 错误率 | 接收Kb/s | 发送Kb/s |
| ---------- | ------ | ------ | ------------ | -------- | ------ | -------- | -------- |
| nio        | 200    | 20000  | 147          | 1176.3/s | 18.52% | 817.18   | 134.78   |
| nio        | 300    | 30000  | 236          | 1029.1/s | 45.02% | 1278.89  | 79.57    |
| nio        | 500    | 50000  | 493          | 842.3/s  | 70.38% | 1490.38  | 35.09    |
| aio        | 200    | 20000  | 114          | 1357.1/s | 18.70% | 947.83   | 155.15   |
| aio        | 300    | 30000  | 254          | 949.5/s  | 59.03% | 1455.15  | 54.71    |
| aio        | 500    | 50000  | 401          | 1020.9/s | 64.70% | 1687.59  | 50.68    |

可以发现两者性能差不多。

### linux压测

#### 1）解压

去官网上下载tgz包，上传至服务器，进行解压：

![1571140906216](tomcat.assets/1571140906216.png)

#### 2）导出

将配置好的文件保存：

![1571141221550](tomcat.assets/1571141221550.png)

#### 3）上传至服务器

将保存的jmx文件上传至服务器目录下。

#### 4）运行

使用命令：

`./jmeter.sh  -n -t ../../jmx/test.jmx -l ../../jmx/result.jtl`

将结果保存为result.jtl。

#### 5）导入Windows

将生成的jtl导入windows下：

![1571142013083](tomcat.assets/1571142013083.png)

#### 6）结果

| tomcat模式 | 并发数 | 样本数 | 平均响应时间 | 吞吐量  | 错误率 | 接收Kb/s | 发送Kb/s |
| ---------- | ------ | ------ | ------------ | ------- | ------ | -------- | -------- |
| nio        | 200    | 20000  | 193          | 894.8/s | 0.00%  | 278.74   | 125.83   |

可以发现比windows的好很多。

| tomcat模式 | 并发数 | 样本数 | 平均响应时间 | 吞吐量 | 错误率 | 接收Kb/s | 发送Kb/s |
| ---------- | ------ | ------ | ------------ | ------ | ------ | -------- | -------- |
| nio        | 200    | 600000 | 99           | 1925.2 | 0.00%  | 599.76   | 270.74   |
|            |        |        |              |        |        |          |          |
|            |        |        |              |        |        |          |          |
|            |        |        |              |        |        |          |          |
|            |        |        |              |        |        |          |          |

