# Mybatis

环境：

+ JDK
+ MySQL
+ maven
+ IDEA

回顾：

+ JDBC
+ MySQL
+ Java基础
+ Maven
+ Junit

# FirstMybatis

## 搭建环境

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>Mybatis</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>15</maven.compiler.source>
        <maven.compiler.target>15</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.23</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.6</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/junit/junit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>

    </dependencies>

</project>
```

## 创建一个模块

+ ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
      <environments default="development">
          <environment id="development">
              <transactionManager type="JDBC"/>
              <dataSource type="POOLED">
                  <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                  <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&useUnicode=true&characterEncoding=UTF-8"/>
                  <property name="username" value="root"/>
                  <property name="password" value="123456"/>
              </dataSource>
          </environment>
      </environments>
      <mappers>
          <mapper resource="com/Volerde/dao/UserMapper.xml"/>
      </mappers>
  </configuration>
  ```

+ 编写Mybatis工具类
  
  ```java
  //sqlSessionFactory
  public class mybatisUtils {
  
      private static SqlSessionFactory sqlSessionFactory;
  
      static {
          try {
              //获取sqlSessionFactory
              String resource = "mybatis-config.xml";
              InputStream inputStream = Resources.getResourceAsStream(resource);
              SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
          } catch (IOException e) {
              e.printStackTrace();
          }
      }
      //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例。
      //SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。
      public static SqlSession getSqlSession(){
          return sqlSessionFactory.openSession();
      }
  
  }
  ```

## 编写代码

+ 实体类

+ dao接口
  
  ```java
  public interface UserDao {
      List<User> getUserList();
      }
  ```

+ 接口实现类
  
  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="com.Volerde.dao.UserDao">
      <select id="getUserList" resultType="com.Volerde.pojo.User">
          select * from mybatis.user
      </select>
  </mapper>
  ```

+ Junit测试
  
  ```java
      @Test
      public void test(){
          SqlSession sqlSession = MybatisUtils.getSqlSession();
          UserDao userDao = sqlSession.getMapper(UserDao.class);
          List<User> userList = userDao.getUserList();
          for (User user : userList) {
              System.out.println(user);
          }
          sqlSession.close();
      }
  ```
  
   可能问题：
  
  1. 配置文件未注册
  2. 绑定接口错误
  3. 方法名错误
  4. 返回类型错误
  5. Maven导出资源问题
     
     # CRUD

## namespace

namespace中的包名要和Dao/Mappper接口的包名一致

## select

选择，查询语句

+ id:对应的namespace中的方法名
+ resultType：SQL语句执行的返回值
+ parameterType：参数类型
1. 编写接口
   
   ```java
       //通过id查询用户
       User getUserById(int id);
   ```

2. 编写对应的Mapper中的SQL语句
   
   ```xml
       <select id="getUserById" parameterType="int" resultType="com.Volerde.pojo.User">
           select * from mybatis.user where id = #{id}
       </select>
   ```

3. 测试
   
   ```java
   @Test
   public void getUserById(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
   
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       User user = mapper.getUserById(1);
       System.out.println(user);
   
       sqlSession.close();
   }
   ```

## Insert

1. 编写接口
   
   ```java
   //insert user
   int insertUser(User user);
   ```

2. 编写对应的Mapper中的SQL语句
   
   ```xml
   <insert id="insertUser" parameterType="com.Volerde.pojo.User">
       insert into mybatis.user (id,name,password) values (#{id},#{name},#{password});
   </insert>
   ```

3. 测试
   
   ```java
   //增删改需要提交事务
   @Test
   public void insertUser(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       mapper.insertUser(new User(1,"Volunteer","123456"));
   
       //提交事务
       sqlSession.commit();
       sqlSession.close();
   }
   ```

## update

1. 编写接口
   
   ```java
   //修改用户
   int updateUser(User user);
   ```

2. 编写对应的Mapper中的SQL语句
   
   ```xml
   <update id="updateUser" parameterType="com.Volerde.pojo.User">
       update mybatis.user
       set name = #{name},password=#{password}
       where id = #{id};
   </update>
   ```

3. 测试
   
   ```java
   //增删改需要提交事务
   @Test
   public void updateUser(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       mapper.updateUser(new User(1,"nameless","654321"));
   
       sqlSession.commit();
       sqlSession.close();
   }
   ```

## delete

1. 编写接口
   
   ```java
   //删除一个用户
   int deleteUser(int id);
   ```

2. 编写对应的Mapper中的SQL语句
   
   ```xml
   <delete id="deleteUser" parameterType="int">
       delete from mybatis.user where id =#{id}
   </delete>
   ```

3. 测试
   
   ```java
   //增删改需要提交事务
   @Test
   public void deleteUser(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       mapper.deleteUser(1);
   
       sqlSession.commit();
       sqlSession.close();
   }
   ```

## Map（了解）

当实体类或者数据库中的表，字段或者参数过多时，可以使用Map

# 配置解析

## 核心配置文件

+ mybatis-config.xml
+ mybatis的配置文件包含了会深深影响Mybatis行为的设置和属性信息

配置文档的顶层结构如下：

+ configuration（配置）
  + [properties（属性）](https://mybatis.org/mybatis-3/zh/configuration.html#properties)
  + [settings（设置）](https://mybatis.org/mybatis-3/zh/configuration.html#settings)
  + [typeAliases（类型别名）](https://mybatis.org/mybatis-3/zh/configuration.html#typeAliases)
  + [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
  + [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
  + [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)
  + environments（环境配置）
    + environment（环境变量）
      + transactionManager（事务管理器）
      + dataSource（数据源）
  + [databaseIdProvider（数据库厂商标识）](https://mybatis.org/mybatis-3/zh/configuration.html#databaseIdProvider)
  + [mappers（映射器）](https://mybatis.org/mybatis-3/zh/configuration.html#mappers)

## 环境配置(Environments)

 Mybatis可以配置成适应多种环境

**尽管可以配置成多种环境，但每个sqlSessionFactory实例只能选择一种环境**

Mybatis默认的事务管理器就是JDBC，连接池：POOLED

## 属性(Properties)

 通过properties属性来实现引用配置文件

这些属性都是可外部配置且可动态替换的，既可以在典型的java属性文件中配置，亦可以通过properties元素的子元素来传递

编写一个配置文件

db.properties

```properties
driver = com.mysql.cj.jdbc.Driver
url = jdbc:mysql://localhost:3306/mybatis?useSSL=true&useUnicode=true&characterEncoding=UTF-8
username = root
password = 123456
```

在配置文件中引入

```xml
<!--引入外部文件-->
    <properties resource="db.properties"/>
```

+ 可以直接引入外部文件
+ 可以在其中增加一些属性配置
+ 如果两个文件有同一个字段，优先使用外部配置文件的字段

## 类型别名(TypeAliases)

+ 类型别名是为Java类型设置一个短的名字

+ 存在的意义在于减少类完全限定名的冗余
  
  ```xml
  <!--    可以给实体类起别名-->
  <typeAliases>
      <typeAlias type="com.Volerde.pojo.User" alias="user"/>
  </typeAliases>
  ```

+ 也可以指定一个包名，Mybatis会在包名下面搜索需要的Java Bean，比如：扫描实体类的包，它的默认别名就为这个类的类名 的首字母小写
  
  ```xml
  <typeAliases>
      <package name="com.Volerde.pojo"/>
  </typeAliases>
  ```
  
  实体类少，第一种方式
  
  实体类多，第二种方式
  
  ```java
  //方式二之注解版，
  @Alias("whatever")
  public class User {
  ```

## 设置(Settings)

会改变Mybatis的运行时行为

[详情参见官网](https://mybatis.org/mybatis-3/zh/configuration.html)

## 其他配置

+ TypeHandlers(类型处理器)
+ objectFactory(对象工厂)
+ plugins插件
  + Mybatis-generator-core
  + Mybatis-plus
  + 通用Mapper

## 映射器(Mappers)

MapperRegistry:注册绑定Mapper文件;

方式一:`推荐使用`

```xml
<!-- 每一个Mapper.xml都需要在Mybatis核心配置文件中注册 -->
<mappers>
    <mapper resource="com/Volerde/dao/UserMapper.xml"/>
</mappers>
```

方式二: 使用class文件绑定注册

```xml
<mappers>
    <mapper class="com.Volerde.dao.UserMapper"/>
</mappers>
```

注意点:

+ 接口和它的Mapper配置文件必须同名
+ 接口和它的Mapper配置文件必须在同一个包下

方式三:使用扫描包进行注入绑定

```xml
<mappers>
    <mapper name="com.Volerde.dao"/>
</mappers>
```

注意点:

+ 接口和它的Mapper配置文件必须同名
+ 接口和它的Mapper配置文件必须在同一个包下

## 生命周期和作用域

生命周期和作用域的错误的使用会导致非常严重的并发问题。

**SqlSessionFactoryBuilder**：

+ 一旦创建了，就不在需要
+ 局部变量

**SqlSessionFactory**:

+ 可以想象为：数据库连接池
+ SqlSessionFactory一旦被创建就英爱在应用的运行期间一直存在，没有任何理由丢弃它或者重新创建另一个实例。
+ 因此SqlSessionFactory的最佳作用域就是应用作用域
+ 最简单的就是使用单例模式或者静态单例模式

**SqlSession：**

+ 连接到连接池的一个请求
+ SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。
+ 用完后需要赶紧关闭，否则资源被占用

<img src="http://lsky.volerde.space/i/2022/05/16/62823e38b3814.png" alt="image-20210419162213568.png" style="zoom: 50%;" />

每一个Mapper都代表一个业务

# 解决属性名和字段名不一致的问题

## 问题

数据库中的字段

![image-20210419162355017.png](http://lsky.volerde.space/i/2022/05/16/62823e3a6c45a.png)![image-20210419164245178.png](http://lsky.volerde.space/i/2022/05/16/62823e382bcd2.png)

测试出现的问题

<img src="http://lsky.volerde.space/i/2022/05/16/62823e397978c.png" alt="image-20210419164322301.png" style="zoom:80%;" />

```xml
//select * from mybatis.user where id = #{id}
//类型处理器
//select id,name,pwd from mybatis.user where id = #{id}
```

解决方法

+ 起别名

```xml
<select id="getUserById" resultType="com.VOlerde.pojo.user">
    select id,name,pwd as password from mybatis.user where id = #{id}
</select>
```

resultMap

结果集映射

```
id        name        pwd
id        name        password
```

```xml
<!--结果集映射-->
<resultMap id="UserMap" type="User">
    <!--column数据库中的字段,property实体类中的属性-->
    <result column="pwd" property="password"/>
</resultMap>
```

+ `resultMap` 元素是 MyBatis 中最重要最强大的元素。
+ ResultMap 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。

# 日志

## 日志工厂

如果数据库出现异常，我们需要排错。日志就是对好的助手

曾经：sout、debug

现在：日志工厂

![image-20210419183159743.png](http://lsky.volerde.space/i/2022/05/16/62823e3a0e345.png)

+ SLF4J
+ LOG4J【掌握】
+ LOG4J2
+ JDK_LOGGING
+ COMMONS_LOGGING
+ STDOUT_LOGGING【掌握】
+ NO_LOGGING

```xml
<settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

## Log4j

什么是Log4j？

+ Log4j是[Apache](https://baike.baidu.com/item/Apache/8512995)的一个开源项目，通过使用Log4j，我们可以控制日志信息输送的目的地是[控制台](https://baike.baidu.com/item/控制台/2438626)、文件、[GUI](https://baike.baidu.com/item/GUI)组件

+ 可以控制每一条日志的输出格式

+ 通过定义每一条日志信息的级别，我们能够更加细致的控制日志的生成过程

+ 通过一个配置文件来灵活地进行配置，而不需要修改应用的代码
1. 先导入Log4j的包

2. log4j.properties
   
   ```properties
   #将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
   log4j.rootLogger=DEBUG,console,file
   
   #控制台输出的相关设置
   log4j.appender.console = org.apache.log4j.ConsoleAppender
   log4j.appender.console.Target = System.out
   log4j.appender.console.Threshold=DEBUG
   log4j.appender.console.layout = org.apache.log4j.PatternLayout
   log4j.appender.console.layout.ConversionPattern=[%c]-%m%n
   
   #文件输出的相关设置
   log4j.appender.file = org.apache.log4j.RollingFileAppender
   log4j.appender.file.File=./log/Volerde.log
   log4j.appender.file.MaxFileSize=10mb
   log4j.appender.file.Threshold=DEBUG
   log4j.appender.file.layout=org.apache.log4j.PatternLayout
   log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n
   
   #日志输出级别
   log4j.logger.org.mybatis=DEBUG
   log4j.logger.java.sql=DEBUG
   log4j.logger.java.sql.Statement=DEBUG
   log4j.logger.java.sql.ResultSet=DEBUG
   log4j.logger.java.sql.PreparedStatement=DEBUG
   ```

3. 配置log4j为日志的实现
   
   ```xml
   <settings>
       <setting name="logImpl" value="LOG4J"/>
   </settings>
   ```

4. log4j的使用，直接测试运行
   
   ```
   D:\Java\jdk-15.0.1\bin\java.exe -ea -Didea.test.cyclic.buffer.size=1048576 -javaagent:C:\Users\123\AppData\Local\JetBrains\Toolbox\apps\IDEA-U\ch-0\211.6693.111\lib\idea_rt.jar=7846:C:\Users\123\AppData\Local\JetBrains\Toolbox\apps\IDEA-U\ch-0\211.6693.111\bin -Dfile.encoding=UTF-8 -classpath C:\Users\123\AppData\Local\JetBrains\Toolbox\apps\IDEA-U\ch-0\211.6693.111\lib\idea_rt.jar;C:\Users\123\AppData\Local\JetBrains\Toolbox\apps\IDEA-U\ch-0\211.6693.111\plugins\junit\lib\junit5-rt.jar;C:\Users\123\AppData\Local\JetBrains\Toolbox\apps\IDEA-U\ch-0\211.6693.111\plugins\junit\lib\junit-rt.jar;D:\Programming\Java_Programming\IDEA\Mybatis\Mybatis-03\target\test-classes;D:\Programming\Java_Programming\IDEA\Mybatis\Mybatis-03\target\classes;C:\Users\123\.m2\repository\mysql\mysql-connector-java\8.0.23\mysql-connector-java-8.0.23.jar;C:\Users\123\.m2\repository\com\google\protobuf\protobuf-java\3.11.4\protobuf-java-3.11.4.jar;C:\Users\123\.m2\repository\org\mybatis\mybatis\3.5.6\mybatis-3.5.6.jar;C:\Users\123\.m2\repository\junit\junit\4.13.2\junit-4.13.2.jar;C:\Users\123\.m2\repository\org\hamcrest\hamcrest-core\1.3\hamcrest-core-1.3.jar;C:\Users\123\.m2\repository\log4j\log4j\1.2.17\log4j-1.2.17.jar com.intellij.rt.junit.JUnitStarter -ideVersion5 -junit4 com.Volerde.dao.UserMapperTest,getUserById
   [org.apache.ibatis.logging.LogFactory]-Logging initialized using 'class org.apache.ibatis.logging.log4j.Log4jImpl' adapter.
   [org.apache.ibatis.logging.LogFactory]-Logging initialized using 'class org.apache.ibatis.logging.log4j.Log4jImpl' adapter.
   [org.apache.ibatis.io.VFS]-Class not found: org.jboss.vfs.VFS
   [org.apache.ibatis.io.JBoss6VFS]-JBoss 6 VFS API is not available in this environment.
   [org.apache.ibatis.io.VFS]-Class not found: org.jboss.vfs.VirtualFile
   [org.apache.ibatis.io.VFS]-VFS implementation org.apache.ibatis.io.JBoss6VFS is not valid in this environment.
   [org.apache.ibatis.io.VFS]-Using VFS adapter org.apache.ibatis.io.DefaultVFS
   [org.apache.ibatis.io.DefaultVFS]-Find JAR URL: file:/D:/Programming/Java_Programming/IDEA/Mybatis/Mybatis-03/target/classes/com/Volerde/pojo
   [org.apache.ibatis.io.DefaultVFS]-Not a JAR: file:/D:/Programming/Java_Programming/IDEA/Mybatis/Mybatis-03/target/classes/com/Volerde/pojo
   [org.apache.ibatis.io.DefaultVFS]-Reader entry: User.class
   [org.apache.ibatis.io.DefaultVFS]-Listing file:/D:/Programming/Java_Programming/IDEA/Mybatis/Mybatis-03/target/classes/com/Volerde/pojo
   [org.apache.ibatis.io.DefaultVFS]-Find JAR URL: file:/D:/Programming/Java_Programming/IDEA/Mybatis/Mybatis-03/target/classes/com/Volerde/pojo/User.class
   [org.apache.ibatis.io.DefaultVFS]-Not a JAR: file:/D:/Programming/Java_Programming/IDEA/Mybatis/Mybatis-03/target/classes/com/Volerde/pojo/User.class
   [org.apache.ibatis.io.DefaultVFS]-Reader entry: ����   ; ?
   [org.apache.ibatis.io.ResolverUtil]-Checking to see if class com.Volerde.pojo.User matches criteria [is assignable to Object]
   [org.apache.ibatis.datasource.pooled.PooledDataSource]-PooledDataSource forcefully closed/removed all connections.
   [org.apache.ibatis.datasource.pooled.PooledDataSource]-PooledDataSource forcefully closed/removed all connections.
   [org.apache.ibatis.datasource.pooled.PooledDataSource]-PooledDataSource forcefully closed/removed all connections.
   [org.apache.ibatis.datasource.pooled.PooledDataSource]-PooledDataSource forcefully closed/removed all connections.
   [org.apache.ibatis.transaction.jdbc.JdbcTransaction]-Opening JDBC Connection
   [org.apache.ibatis.datasource.pooled.PooledDataSource]-Created connection 75470648.
   [org.apache.ibatis.transaction.jdbc.JdbcTransaction]-Setting autocommit to false on JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@47f9738]
   [com.Volerde.dao.UserMapper.getUserById]-==>  Preparing: select * from mybatis.user where id = ?
   [com.Volerde.dao.UserMapper.getUserById]-==> Parameters: 1(Integer)
   [com.Volerde.dao.UserMapper.getUserById]-<==      Total: 1
   User{id=1, name='Volunteer', password='123456'}
   [org.apache.ibatis.transaction.jdbc.JdbcTransaction]-Resetting autocommit to true on JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@47f9738]
   [org.apache.ibatis.transaction.jdbc.JdbcTransaction]-Closing JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@47f9738]
   [org.apache.ibatis.datasource.pooled.PooledDataSource]-Returned connection 75470648 to pool.
   
   Process finished with exit code 0
   ```

简单使用

1. 在要使用log4j的类中，导入包import org.apache.log4j.Logger;

2. 日志对象，参数为当前类的class
   
   ```java
   static Logger logger = Logger.getLogger(UserMapperTest.class);
   ```

3. 日志级别
   
   ```java
   @Test
   public void log4j(){
       logger.info("this is a msg by info");
       logger.debug("this is a msg by debug");
       logger.error("this is a msg by error");
   }
   ```

# 分页

为什么要分页？

+ 减少数据的处理量

使用limit分页

```sql
语法：select * from user limit startIndex,pageSize;
```

使用Mybatis实现分页，核心SQL

1. 接口
   
   ```java
   //分页
   List<User> getUserLimit(Map<String,Integer> map);
   ```

2. Mapper.xml
   
   ```xml
   <select id="getUserLimit" resultType="user" parameterType="map">
       select * from mybatis.user limit #{startIndex},#{pageSize};
   </select>
   ```

3. 测试
   
   ```java
   @Test
   public void getUserLimit(){
       SqlSession sqlSession = MybatisUtils.getSqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       HashMap<String,Integer> map = new HashMap<String,Integer>();
       map.put("startIndex",0);
       map.put("pageSize",2);
   
       List<User> userLimit = mapper.getUserLimit(map);
       for (User user : userLimit) {
           System.out.println(user);
       }
       sqlSession.close();
   }
   ```

# 使用注解开发

## 面向接口编程

在真正开发中，很多时候我们会选择面向接口编程

+ **根本原因：解耦，可拓展，提高复用，分层开发中，上层不用管具体的实现，遵守共同的标准，使得开发变得容易，规范性高**

关于接口

+ 接口从更深层次理解，影视定义（规范，约束）与实现（名实分离的的原则）的分离
+ 接口的本身反映了系统设计人员对系统的抽象理解
+ 接口应有两类：
  + 对一个个体的抽象，可对应为一个抽象体（abstract class）
  + 对一个个体某一方面的抽象，即形成一个抽象面（interface）
+ 一个个体可能有多个抽象面，抽象体与抽象面是有区别的

三个面向区别

+ 面向对象：考虑问题时，以对象为单位，考虑它的属性及方法
+ 面向过程：考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现
+ 接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题。更多的体现就是对系统整体的架构

## 使用注解开发

1. 注解在接口上实现
   
   ```java
   @Select("select * from user")
   List<User> getUsers();
   ```

2. 需要在核心文件中绑定接口
   
   ```xml
   <mappers>
       <mapper class="com.Volerde.dao.UserMapper"/>
   </mappers>
   ```

3. 测试

本质：反射机制实现

底层：动态代理
