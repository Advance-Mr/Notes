#                                       **MyBatis**

## 介绍

###    JDBC不足

​		· sql***\*语句写死\****，如果拓展功能，可能整个sql语句作废

​		***\*set操作繁琐\****，如果sql语句有一百个?可能要写一百个set

### 	MyBatis概念

- MyBatis是对jdbc的封装

- MyBatis之前叫做ibatis，后来才改名

- MyBatis是持久层框架

  

  Java的持久层框架有：HIbernate、Mybatise、JOOQ、ActiviteJDBC、GUZZ

 

***\**Rom的思想：对象关系的映射，通过mybatis这个框架实现了。\**\***

 

![img](file:///C:\Users\Administrator\AppData\Local\Temp\ksohtml8604\wps1.jpg) 

 

​	**注意：mybatis是一个半自动化的ORM框架，Hibernate是一个全自动化的框架，自动生成sql语句。**

​    	       **这个类对象叫pojo（普通java类）、javabean、domain叫法**

------



## First程序

​		**mybatis程序从mybatis-config.xml文件获取一个流，从而加载sqlsessionfactory这个类，通过这个来加载一个sqlsession会话。进行数据库的具体操作。**



​		1.	mybatis中有两个配置文件

​				配置mybatis-config.xml核心配置文件

​				配置xxxMapper.xml配置文件用来编写sql语句（一个表一个）

​		2.

```
 //获取sqlSessionFactoryBuilder
SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();

//通过流的方式获取核心配置文件，在通过sqlSessionFactoryBuilder.build就会生成一个会话
InputStream flood = Resources.getResourceAsStream("mybatis-config.xml");
SqlSessionFactory sqlSessionFactory= sqlSessionFactoryBuilder.build(flood);

//获取sqlSession
SqlSession sqlSession = sqlSessionFactory.openSession();

//执行sql语句
int count = sqlSession.insert("insert111");
System.out.println("插入了几条记录"+ count);


//事务管理：提交
sqlSession.commit();
```

3.获取流

​		获取流的时候也可以通过java内置的类加载器来加载流，如下语法：

![image-20231106210312911](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20231106210312911.png)

​       当配置文件在其他文件下（不建议，违背了OCP开闭原则）

![image-20231106210503715](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20231106210503715.png)

也可以在mybatis的核心配置文件里通过url来加载xxxMapper.xml文件

![image-20231106210705649](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20231106210705649.png)

4.集成

关于mybatis集成日志组件。让我们调试起来更加方便。
* mybatis常见的集成的日志组件有哪些呢?
  SLF4J (沙拉风)：logback就是继承沙拉疯实现的
  L0G4J
  L0G4J2
  STDOUT_LOGGING：**MyBatis**内部实现的日志组件
  

其中STDOUT_LOGGING是标准日志，mybatis已经实现了这种标准日志。mybatis框架本身已经实现了这种标准。
只要开启即可。怎么开启呢? 在mybatis-config.xml文件中使用settings标签进行配置开启。

4.1集成log4j

​		设置配置文件：log4j.properties------>导入maven依赖 （只是导入一个log4j）



​			1.导包：org.apache.log4j.Logger

​			2.设置日志对象  ：Static Logger logger =Logger.getLogger(类.class)

​			3.日记实现：logger.info("实现日志")

4.2



5.连接池

数据库的连接类型，即设置数据库的连接池。<dataSource type="POOLED">

```
UNPOOLED：不使用数据库连接池，每次获取Connection都要创建一个新对象（耗时且容易宕机）
POOLED：使用MyBatis自带的连接池
JNDI：连接第三方的数据源（如果是javaweb项目，可以使用感tomcat）
```

问：使用连接池有什么好处？
答：1.	迟内的Connection数量是固定的，比如池子大小是5，如果5个连接都被占用，第6个要获取连接就先等待，数量固定

2. 假如有人一直F5刷新，没有用连接池的话，就会一直创建Connection对象，如果实例化对象过多，可能会导致服务器宕机，数量固定
3. 有新请求，第一反应去池中查找，可以增加效率

## 遇到问题

### Tomcat10安装教程

[win10安装Tomcat10详细教程_tomcat10 windows哪个系统-CSDN博客](https://blog.csdn.net/qq_44697728/article/details/114476174)

注意：Tomcat10.x 版本和之前的版本有所区别，就是在使用 Java API 的主要包 javax.* 时要换成 jakarta.*。

拓展：SpringBoot2 尚不兼容 jakarta 命名空间更改。

​         目前最新 SpringBoot2 ------Tomcat9 

​        所以得使用 SpringBoot3，SpringBoot3 将迁移到 Java 17 和 Jakarta 命名空间，推动生态系统超越 Java 8。这是一个重大修订将基于Spring框架6.0，并且spring6.0框架最低使用Java 17或更高版本。它也将是第一个使用 Jakarta EE 9 APIs（jakarta.*）而不是EE 8（javax.*）的Spring Boot版本。最最最重要的，预计是本月（2022.11）就会发布。

### tomcat、servelt 、mybatis版本问题

由于我使用的是Tomcat10.0.12的版本，而我使用Maven导入的依赖如下

```
<dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
</dependency>
<dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>javax.servlet.jsp-api</artifactId>
      <version>2.3.2-b01</version>
</dependency>
```

​			这将导致Tomcat版本无法兼容Servlet版本（tomcat10将JavaEE更换成了Jakarta® EE，所以原Servlet包"javax.servlet.“更改成		了"jakarta.servlet.”），我们在500的图中根本原因的第一行也可以看到：Register cannot be cast to class Jakarta.servlet.Servlet，		即我使用的是Servlet是javax包里的，而Tomcat能够识别（适配）的是Jakarta包里的，而javax包里的Servlet类无法转换为Jakarta包		里的Servlet类，所以显示了例外情况里的第一行：类cn.ken.controller.Register不是Servlet。

解决方法
		既然是由于Tomcat版本与Servlet版本不匹配造成，那么自然就有对应的两种解决方法

  	1、降低Tomcat版本
  			因为tomcat10才改成Jakarta，故只需将Tomcat版本降低到10以下，例如Tomcat9等。

​			当然还要结合具体情况进行分析，让使用的Tomcat版本能够匹配自己使用的JSP或JDK等的版本

​			除了通过下载安装新的Tomcat（这显然比较麻烦），我们也可以通过Maven安装低版本的Tomcat插件来运行，具体操作可见			Maven继承Tomcat插件

​	2、提高Servlet版本
​			将pom.xml中的Servlet更换为最新的依赖，与使用的Tomcat匹配，如下

```
		`<dependency>`
      `<groupId>org.apache.tomcat</groupId>`
      `<artifactId>tomcat-servlet-api</artifactId>`
      `<version>10.0.12</version>`
		`</dependency>`
		`<dependency>`
      `<groupId>org.apache.tomcat</groupId>`
      `<artifactId>tomcat-jsp-api</artifactId>`
      `<version>10.0.12</version>`
		`</dependency>
```



				重新加载Maven变更后即可成功访问Sevrlet

​	----------相关内容----------
​		对于 JDK，高版本的Tomcat是可以正常使用低版本的JDK，反之则不行

​	JAVA servlet 2.x规范：
　　项目目录结构必须要有WEB-INF，web.xml等文件夹和文件
　　在web.xml中配置servlet,filter,listener，以web.xml为java web项目的统一入口

​	JAVA servlet 3.x规范：
　　项目中可以不需要WEB-INF，web.xml等文件夹和文件
　　在没有web.xml文件的情况下，通过注解实现servlet，filter, listener的声明，例如
　　@WebServlet, @WebFilter,@WebListener，当使用注解时，容器自动进行扫描。

原文链接：https://blog.csdn.net/qq_25046827/article/details/121490243

### Tomcat乱码问题

1、改正tomcat中的UTF-8为GBK不推荐

链接：https://www.cnblogs.com/alenblue/p/12491969.html

2、修改idean中的文件Help-->Edit Custom VM Options --->最后一行添加-Dfile.encoding=UTF-8（重启idea即可）

### 自动装箱问题

[【精选】Java之自动装箱与拆箱_java方法实现自动装箱功能_王小二_Leon的博客-CSDN博客](https://blog.csdn.net/qq_44085437/article/details/124028654)

[java八种基本数据类型及包装类详解_java基础数据类型-CSDN博客](https://blog.csdn.net/qq_37688023/article/details/85106894)

### java中引用数据类型

Java中有俩种数据类型,其中主要有8中基本数据类型和引用数据类型，除了8中基本数据类型以外都是引用数据类型,8中基本数据类型分别是byte,short,int,long,char,boolean,float,double，具体如下：

1、boolean：数据值只有true或false，适用于逻辑计算。

2、char：char型（字符型）数据在内存中占用2个字节。char型数据用来表示通常意义上的字符，每个字符占2个字节，Java字符采用Unicode编码，它的前128字节编码与ASCII兼容字符的存储范围在\u0000~\uFFFF，在定义字符型的数据时候要注意加' '，比如 '1'表示字符'1'而不是数值1， 

3、byte：byte型（字节型）数据在内存中占用1个字节，表示的存储数据范围为：-128~127。

4、short：short型（短整型）数据在内存中占用2个字节。

5、int：int型（整型）数据在内存中占用4个字节。

6、long：long型（长整型）数据在内存中占用8个字节。

7、float：float型（单精度浮点型）数据在内存中占用4个字节。（float精度为7-8位）

8、double：double型（双精度浮点型）数据在内存中占用8个字节。

引用数据类型分3种：类，接口，数组；

#### 类Class引用 

可以是我们创建的，这里我不多讲，主要是讲解几个java库中的类 Object ：Object是一个很重要的类，Object是类层次结构的根类，每个类都使用Object作为超类，所有对象（包括数组）都实现这个类的方法。用Object可以定义所有的类 

​             如： 

​         Object object= new Integer(1); 来定义一个Interger类 

​               Integer i=(Integer) object;     在来把这个Object强制转换成Interger类 

String ：String类代表字符串，Java 程序中的所有字符串字面值（如"abc"）都作为此类的实例来实现。检查序列的单个字符、比较字符串、搜索字符串、提取子字符串、创建字符串副本、在该副本中、所有的字符都被转换为大写或小写形式。 

Date ：Date表示特定的瞬间，精确到毫秒。Date的类一般现在都被Calendar 和GregorianCalendar所有代替 

Void ：Void 类是一个不可实例化的占位符类，它保持一个对代表 Java 关键字 void 的 Class 对象的引用。 

同时也有对应的Class如：Integer  Long  Boolean  Byte  Character  Double  Float   Short 

#### 接口interface引用 

​	可以是我们创建的，这里我不多讲，主要是讲解几个java库中的接口interface
List<E>：列表 ，此接口的用户可以对列表中每个元素的插入位置进行精确地控制。用户可以根据元素的整数索引 （在列表中的位置）访问元素，并搜索列表中的元素。List 接口提供了两种搜索指定对象的方法。从性能的观点来看，应该小心使用这些方法。在很多实现中，它们将执行高开销的线性搜索。 List 接口提供了两   种在列表的任意位置高效插入和移除多个元素的方法。 
add() : 在列表的插入指定元素。 
remove()：移除列表中指定位置的元素。 
get(int index)：返回列表中指定位置的元素。
Map<K,V>： 
K - 此映射所维护的键的类型 
V - 映射值的类型 将键映射到值的对象。一个映射不能包含重复的键；每个键最多只能映射到一个值。 
put(K key,V value)：将指定的值与此映射中的指定键关联（可选操作）。如果此映射以前包含一个该键的映射关系，则用指定值替换旧值（当且仅当，返回 true 时，才能说映射 m 包含键 k 的映射关系）。  remove(Object key)如果存在一个键的映射关系，则将其从此映射中移除（可选操作）。更确切地讲，如果此 映射包含从满足(key==null ? k==null :key.equals(k))的键 k 到值 v 的映射关系，则移除该映射关系。（该映射最多只能包含一个这样的映射关系.） get(Object key)：返回指定键所映射的值；如果此映射不包含该键的映射关系，则返回 null。
这里我们主要是用String List Map Object 是最常用Number ArrayList<E> Arrays等 

#### 数组引用

数组：存储在一个连续的内存块中的相同数据类型（引用数据类型）的元素集合。

数组中的每一个数据称之为数组元素，数组中的元素以索引来表示其存放的位置，索引（下标）从0开始。

数组的定义

第一种方式：类型[] 数组名; 如 int[] nums; 
第二种方式：类型数组名[]; 如 int nums[];
大多数Java程序员喜欢使用第一种风格,因为它把数据类型int[],和变量名num分开了.
数组的初始化
Java中数组必先初始化后才能使用.
初始化就是给数组元素分配内存，并为每个元素赋初始值。
初始化数组的两种方式：
- 静态初始化:
语法格式:类型[] 数组名 = new 数组类型[]{元素1,元素2,元素3,...元素n};
简化语法：类型[] 数组名 = {元素1,元素2,元素3...元素n};
- 动态初始化:
如果我们事先不知道数组里存储哪些数据，只知道需要存储数据的个数，此时可以使用动态初始化方式。
动态初始化：初始化时由我们指定数组的长度，系统自动为数组元素分配初始值。
格式：类型[] 数组名 = new 数组类型[数组长度];
注意:无论，以哪种方式初始化数组，一旦初始化完成，数组的长度就固定了，不能改变，除非重新初始化。也就是说数组是定长的。











mybatis的反射机制能读取字节码，但是javaasist可以操作字节码，从而实现接口的动态代理。

![image-20231118132149987](./MyBatis.assets/image-20231118132149987.png)

[MyBatis (yuque.com)](https://www.yuque.com/dujubin/ltckqu/pozck9?#l3AZu)

rs4n
