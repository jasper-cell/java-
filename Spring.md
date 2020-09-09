## 组件相关的注解:

~~~java
	@component

​	@Service

​	@Repository

​	@Controller
~~~

## 配置文件：

​	**application.xml**

​	在xml配置文件中配置scan组件

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">

	<!--设置一个基础包：spring会扫描该包及其子包下的所有类，如果有注解就给其创建对象  -->
	<context:component-scan base-package="com.atguigu.demo" use-default-filters="false">
        <!--按照注解类型排除  -->
    	<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Component"/>
        <!--按照接口类型排除 -->
       
        <!--包含：只有配置的类(注解和接口)，可以在IOC容器创建的时候创建对象 use-default-filters="false"-->
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Component"/>
    </context:component-scan>

</beans>
~~~

其中的expression主要是写相关组件注解的全类名称

## IOC容器：

~~~java
ApplicationContext cxt = new ClassPathXmlApplcationContext("application.xml");
// 获取Bean的三种方式
UserView view = cxt.getBean("userView");
UserView view = cxt.getBean(UserView.class);
UserView view = cxt.getBean("userView",UserView.class);
~~~

​	**IOC控制反转**

IOC是通过依赖注入实现的，IOC中存放着对象的地址值

## 自动写入接口类：

​	@Autowire(required=true)  类似的注解还有:@Resource    @Inject

		1. false：可以注入失败，同时也不会报错
  		2. true：一旦注入失败，就报错

​	@Qualifier("Implname")

​	**利用接口和多态原理实现松耦合的概念**

​	没有Qulifier时，默认先匹配类型，类型不匹配required不起任何作用。

​	加上Qulifier时，按名字匹配，required可以对名字不匹配起作用，使其不报错

## 防止重名错误的产生：

​	@Component(value="aaa")  // 此处可以起到给相关组件在IOC容器中起别名的作用

## 手动配置Bean：

~~~xml
<beans>
	<bean id="student" class="com.atguigu.demo.beans.Student">
    	<property name="lastName" value="jasper"></property>
    </bean>
</beans>
~~~

## 配置bean产生类型：

**@Scope("prototype")**

**@Scope("singleton")**



## SpringMVC:

类似于一个servlet的框架，相当于一个转发器

**转发界面**  => **SpringMVC框架中所定义的servlet** => **@RequestMapping("/hello") 所注释的方法** => **返回方法中的字符串的值** => **到达试图解析器** => **最终返回页面**

设置status码：为1的时候则采用tomcat一启动就创建

**@RequestMapping(value={"/hello", "/hi"}, method=RequestMethod.GET)**

不仅仅可以将@RequestMapping放在方法上，还可以将其放在类上，相当于给方法一个前缀的限制URL，不让其非得与其他所有类中的方法不同

类上的路径+方法上的路径

1. 参数定了几个，那么请求参数的数据就必须有几个

2. **@RequestParam(value = "username",defaultValue="ccc")**映射给相应的形参名

3. 使用**POJO**作为参数,POJO类似于一个javabean。表单中的name值会自动填充到以pojo作为参数的属性值当中.

4. 使用servlet原生API作为参数使用。

响应：

1. 只能使用**@controller**注解
2. 创建ModelAndView：**ModelAndView mv = new ModelAndView();**
3. 放置数据: **mv.addObject("age", 18);**
4. 设置视图：**mv.setViewName("index");**

第二种响应方式,入参为map:

​	map.put("age", 100);

实现转发和重定向:

- 一旦我么自己定义重定向或者转发，那么将不再调用视图解析器。需要注意路径问题。

- 转发：return "**forward:hello.jsp**";
- 重定向: return "**redirect: hello.jsp**";  重定向不能访问WEB-INF目录下的文件



## Mybatis简介:

Herbnet是早期的框架

MyBatis与之前的主要区别在于sql语句需要自己去写。

支持定制化SQL，存储过程以及高级映射的优秀的持久层框架。

Mybatis避免了几乎所有的JDBC代码和手动设置参数以及获取结果集

利用xml和注解，将接口和java的pojo映射成数据库中的数据。

半自动的ORM框架。ORM：对象关系映射

**建议**：尽量不要使用基本类型

对象属性的非空判断：

包装类型：默认为null

基本类型：int-》0





在pom.xml中导入Mybatis的依赖：

1. mybatis

   1. <groupId>org.mybatis</groupId>

       	 	<artifactId>mybatis</artifactId>

       	<version>3.4.1</version>

       	</dependency>

2. mysql-connector-java

   1.  	<dependency>

           	  <groupId>mysql</groupId>

      ​	  <artifactId>mysql-connector-java</artifactId>

      ​	  <version>5.1.37</version>

       	</dependency>

3. log4j

   1.  	<dependency>

           		<groupId>log4j</groupId>

      ​		<artifactId>log4j</artifactId>

      ​		<version>1.2.14</version>

       	</dependency>

4. 创建javaBean

5. Mybatis全局配置文件

6. mybatis中的Mapper接口就是以前的Dao接口

7. mapper的namespace必须指定为接口的全类名，id指定为接口中的方法名称

   resultType：返回类型的全类名

8. setting将下划线映射为驼峰命名

   ### 总结

   1. 准备测试表(tbl_employee), 准备测试类(Employee)
   2. 参考mybatis的官方文档，拷贝示例的全局配置文件
   3. 编写mappper接口中的CRUD方法，准备mapper映射文件
   4. 完成两个绑定：mapper接口与mapper映射； mapper接口与mapper映射文件的sql语句的绑定。
   5. 在mybatis-xml中通过<mapper>标签将mapper映射文件引入
   6. 获取sqlsession对象
   7. 通过sqlsession对象执行getEmployee对象
   8. 再编写接口类，执行新的CRUD操作

   

   @ComponentScan(basePackage="com.atguigu.springBoot")  // 组件扫描

   视图解析器直接使用web-starter进行替代

   基本上是传输数据，比如json格式的数据

   采用ajax异步请求而不是采用jsp

   - @ResponseBody  ： 会通过Jackson技术将java对象转换成Json字符串响应给客户端
   - @RquestMapping("/hello")
   - @RestController

   ## SpringBoot工作流程

   1. 创建maven工程
   2. 再maven工程中的pom.xml中配置Spring-boot-starter-parent等必须的依赖
   3. 创建主要的main运行类，在main运行类中添加@SpringBootApplication注解
      1. 若要想修改spring-boot-starter-web中的tomcat的起动端口号，可以在properties文件中对其进行相应的修改。
   4. 在main类上添加@componentScan(basepackage="com.atguigu")的注解，从而自动扫描相应的包、
   5. 创建controller类，添加@Contorller的注解和相应的@RequestMapping("/member")的注解
   6. 同时在Controller的方法中或者类上，均可以加上@ResponseBody的注解使得其自动将方法返回的java对象封装为json字符串
   7. @RestController可以相当于@ResponseBody和@Controller两者的结合体
   8. 添加@Autowired到方法中所用到的所有接口上，使其能自动根据接口创建对象

   

   

   

   

   





















