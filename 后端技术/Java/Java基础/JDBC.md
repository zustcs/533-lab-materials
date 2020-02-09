# JDBC基础

## JDBC基础（上）

### JDBC优势

简单 快捷 移植性 框架

### JDBC类

#### Driver & DriverManager

Driver：接口，定义各个驱动程序实现的功能，使其变得抽象，可以实现对各个驱动程序的操作

DriverManager：是Java的管理类，通过`Class.forname(DriverName)`的方式就可以向其注册一个驱动程序，通过`getConnection`可以调用该驱动程序，建立后端数据库的物理连接。

#### DBURL

是应用程序的唯一标识符

`jdbc:mysql://10.164.172.20:3306/cloud_study`

jbdc：协议

mysql：自协议

10.164.172.20:3306/cloud_study：子名称

其中

10.164.172.20：主机

3306：端口

cloud_study：数据库

#### 常用方法

`Statement stmt = conn.createStatement();` 创建一个Statement对象

`ResultSet rs = stmt.executeQuery("select userName from user");` 得到查询结果

#### 所需架包

```
spring-jdbc
mysql-connector-java
commons-dbcp2
commons-pool2
commons-logging
```

## JDBC基础(下)

### 构建步骤

1：装载驱动程序

2：建立数据库连接

3：执行SQL语句

4：获取执行结果

5：清理环境

### 具体操作

```
注意：新版为cj.jbdc
static final String JDBC_DRIVER = "com.mysql.cj.jdbc.Driver";
static final String DB_URL = "jdbc:mysql://localhost/jbdc"+"?serverTimezone=GMT%2B8";
static final String USER = "root";
static final String PASSWORD = "root";
public static void helloword() throws ClassNotFoundException {
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;

        //1：装载驱动程序
        Class.forName(JDBC_DRIVER);
        //2：建立数据库连接
        try {
            conn = DriverManager.getConnection(DB_URL, USER, PASSWORD);
            //3：执行SQL语句
            stmt = conn.createStatement();
            rs = stmt.executeQuery("select username from cloud_study");
            //4：获取执行结果
            while (rs.next()) {
                System.out.println("Hello" + rs.getString("username"));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            try {
                if (conn != null) {
                    conn.close();
                }
                if (stmt != null) {
                    stmt.close();
                }
                if (rs != null) {
                    rs.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
```

## JDBC进阶

### 游标

游标提供一种客户端读取部分服务器端结果集的机制

#### 如何使用游标

DB_URL的参数设置为true

`jdbc:mysql://<IP>:<Port>/<database>?useCursorFetch=true`

用`PreparedStatement`代替`Statement`，并运用`SetFetchSize()`方法

#### 具体实现

```
//设置DB_URL
 static final String DB_URL = "jdbc:mysql://localhost/jbdc"+"?serverTimezone=GMT%2B8"+"&&useCursorFetch=true";
//执行sql语句
ptmt = conn.prepareStatement("select  username from cloud_study");
ptmt.setFetchSize(1);
rs = ptmt.executeQuery();
```

### 流方式

针对大字段时，以二进制流的方式进行存储，每次读取一个区间的内容

#### 具体实现

![1540350080104](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1540350080104.png)

![1540350093034](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1540350093034.png)

### 大量数据插入

#### 批处理

一次提交多条SQL语句，节省网络开销

```
stmt = conn.createStatement();
for (String user : users) {
    stmt.addBatch("insert into cloud_study values('4'," + user +")");
}
stmt.executeBatch();
stmt.clearBatch();
```

### 防止乱码

在DB_URL中插入`characterEncoding=utf8`

# 数据库连接池

它是一个Java架包，用户可以进行创建，管理，限流，销毁

![1540383987246](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1540383987246.png)

![1540384005986](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1540384005986.png)

![1540384013409](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1540384013409.png)

![1540384020723](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1540384020723.png)

## SQL注入与防范

数据库注入指用户用SQL命令，如张三';--，欺骗服务器，通过恶意SQL获取数据库内信息

#### 根源

根源在于SQL语句由用户动态拼接而成，用户如果写下SQL命令，可能导致原有语意改变

### 解决方案

`Connection.preparedStatement(sql)`→`select * from user where userName = ? And password = ?`

用？来代替参数，防止语意改变

密码需要加密处理，用`AES_ENCRYPT/ AES_DECRYPT`加密和解密

## 事物原理与开发

原子性：整体

一致性

隔离性

持久性

### 事物

事物是并发控制的基本单位，指作为单个逻辑工作单元执行的一系列操作，而这些逻辑工作单元需要满足ACID特性，即原子性，一致性，隔离性，持久性

#### JDBC事务控制

`Connection.setAutoCommit() `      开启事务

`Connection.commit()`                    提交事务

`Connection.rollback()`                回滚事务

#### 脏读

读取一个事务未提交的更新

#### 不可重复读

同一个事物中两次读取相同的记录，结果不一样

#### 幻读

两次读取的结果包含的行记录不一样

### 事务隔离级别

1：读未提交：允许脏读

2：读提交：不允许脏读，允许不可重复读

3：重复读：不会出现不可重复读，但可以出现幻读

4：串行化：不可以出现幻读，数据库性能会变差



MySQL默认事务隔离级别为重复读

事务隔离界别越高，数据库性能越差，难度越低

### 设置隔离界别

`Connection.getTranactionlsolation()`        获取事务隔离级别

`Connection.setTransactionlsolation()`      设置事务隔离级别

### 死锁分析与解决

死锁是指两个或者两个以上的事务在执行过程中，因争夺资源而造成的一种**互相等待**的现象

#### 产生必要条件

1：互斥

并发执行的事务为了进行必要的隔离保证执行正确，在事务结束前，需要对修改的数据库记录持锁，保证多个事务对相同数据库记录串行修改。

对于大型并发系统无法避免

2：请求和保持

已经持有一个资源锁，等待另一个资源锁

死锁仅发生在请求两个或者两个以上的锁对象

由于应用需要，难以消除

3：不剥夺

已经获得锁资源的事务，在未执行前，不能被强制剥夺，只能在使用完时，由事务自己释放

一般用于已经出现死锁时，通过破坏该条件达到解除死锁的目的

数据库系统通常通过一定的死锁检测机制发现死锁，强制回滚代价相对较小的事务，达到解除死锁的目的

4：环路等待

发生死锁时，必然存在一个事务——锁的环形链

按照同一顺序获取锁，可以破坏该条件

通过分析死锁事务之间的锁竞争关系，调整SQL的顺序，达到消除死锁的目的

#### 加锁方式

##### 外部加锁

由应用程序添加，锁依赖关系较容易分析

共享锁（S）：`select * from table lock in share mode`

排他锁（X）：`select * from table for update`

##### 内部加锁

为了实现ACID特性，由数据库系统内部自动添加

加锁规则繁琐，与SQL执行计划、事务隔离级别、表索引结构有关

## MyBatis

### ORM

完成持久化类对数据库表之间的映射关系

对持久化对象的操作自动转换成对关系数据库操作

关系数据库的每一行映射为每一个对象

关系数据库的每一列映射为对象的每个属性

### 所需架包

```
mybatis
mysql-connector-java
```

### 配置

在resources下配置xml文件

xml1：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="jdbc"/>
            <!-- 配置数据库连接信息 -->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost/jbdc?serverTimezone=GMT%2B8&amp;useCursorFetch=true&amp;characterEncoding=utf8"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="userMapper.xml"/>
    </mappers>
</configuration>
```

xml2：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="mybatis.GetUserInfo">
    <select id="getUser" parameterType="int"
            resultType="mybatis.User">
        select id,username from cloud_study where id = #{id}
    </select>
</mapper>
```

### Java对象

构造对象 

构建接口

```
package mybatis;

import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.InputStream;

public class HelloMyBatis {
    public static void main(String[] args) {
        //1：声明配置文件的目录渎职
        String resource = "mybatis-config.xml";
        //2：加载应用配置文件
        InputStream is = HelloMyBatis.class.getClassLoader().getResourceAsStream(resource);
        //3：创建SqlSessionFactory
        SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(is);
        //4：获取Session
        SqlSession session = sessionFactory.openSession();
        try {
            //5：获取操作类
            GetUserInfo getUserInfo = session.getMapper(GetUserInfo.class);
            //6：完成查询操作
            User user = getUserInfo.getUser(1);
            System.out.println(user.getId() + " " + user.getUserName());
        } finally {
            //7：关闭Session
            session.close();
        }
    }
}
```

#### 优势

入门门槛低

更加灵活，SQL优化

#### 劣势

需要自己编写SQL，工作量大

数据库移植性差

### 不写Mapper的方法

**新建接口**

```
public interface GetUserInfoAnnotation {
    @Select("select * from cloud_study where id = #{id}")
    public User getUser(String id);
}
```

Java类：

```
package mybatis;

import org.apache.ibatis.session.Configuration;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.InputStream;

public class HelloMyBatis {
    public static void main(String[] args) {
        //1：声明配置文件的目录渎职
        String resource = "mybatis-config.xml";
        //2：加载应用配置文件
        InputStream is = HelloMyBatis.class.getClassLoader().getResourceAsStream(resource);
        //3：创建SqlSessionFactory
        SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(is);
        //注解方式
        Configuration conf = sessionFactory.getConfiguration();
        conf.addMapper(GetUserInfoAnnotation.class);
        //4：获取Session
        SqlSession session = sessionFactory.openSession();
        try {
            //5：获取操作类
            GetUserInfoAnnotation getUserInfo = session.getMapper(GetUserInfoAnnotation.class);
            //6：完成查询操作
            User user = getUserInfo.getUser("1");
            System.out.println(user.getId() + " " + user.getUserName());
        } finally {
            //7：关闭Session
            session.close();
        }
    }
}
```

### 增删改

![1540484477602](C:\Users\hasee\AppData\Roaming\Typora\typora-user-images\1540484477602.png)

