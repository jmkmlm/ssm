# Spring

###  1.依赖注入

​	**控制反转IOC：**对象实例不再由调用者控制，而转移到spring容器，控制权发生了反转

```xml
<bean id="userDao" class="com.itheima.ioc.UserDaoImpl" />
```

​	**依赖注入DI：**从spring角度，spring容器负责将被依赖对象给调用者的成员变量，这相当于为调用者注入了它依赖的实例

```xml
<bean id="userService" class="com.itheima.ioc.UserServiceImpl">
       <property name="userDao" ref="userDao" />
</bean>
```



### 2.Spring两种核心容器

**BeanFactory**是基础类型的ioc容器，管理bean的工厂，负责初始化各种Bean

**ApplicationContext**是BeanFactory的子接口，添加了新功能。

```java
//Spring容器控制
String xmlPath = "beans1.xml";
ApplicationContext appli = new ClassPathXmlApplicationContext(xmlPath);
Bean1 bean =(Bean1) appli.getBean("bean1");
```

​	配置文件

```xml
<!--实例化bean-->
<bean id="bean1" class="com.itheima.instance.constructor.Bean1" />
```

### 3.三种实例化Bean

##### a.构造器实例化

```xml
 <bean id="bean1" class="com.itheima.instance.constructor.Bean1" />
```



##### b.静态工厂实例化

```xml
<bean id="bean2" class="com.itheima.instance.static_factory.MyBean2Factory"
     factory-method="createBean" />
```



##### c.实例工厂实例化

```xml
<bean id="myBean3Factory" class="com.itheima.instance.factory.MyBean3Factory" />
<bean id="bean3Factory" factory-bean ="myBean3Factory" 
      factory-method="createBean" />
```

### 4.Bean作用域

- singleton：单例

- prototype：原型

- request:请求

- session：会话


```xml
 <bean id="bean1" class="com.itheima.instance.constructor.Bean1" scope="singleton"/>
```

### 5.三种Bean装配

##### a.xml装配

​	设值注入：为所有变量设置setter()

```xml
<bean id="user" class="com.itheima.User">
	<constructor-arg index="0" value="tom" />
</bean>
```

​	构造注入:提供含有所有参数的构造方法

```xml
<bean id="user" class="com.itheima.User">
	<property name="username" vlaue="tom" />
</bean>
```

##### b.Annotation装配

> spring-aop-4.3.6.RELEASE.jar
>

类头部

​	@Repository 数据访问层Dao

​	@Service业务层service

​	@Controller控制层

类成员变量

​	**@Autowired** 自动将Bean类安装其类型装配

​	@Resource（name=“userDao”）

```xml
自动扫描包下所有的注解
<context:component-scan base-package="包名" />
```

##### c.自动装配autowire

```xml
并且在类下设置set方法！
<bean id=" " class=" " autowire="byName"
```

### 6.AOP面向切面编程

采取横向抽取机制，将分散在各方法中重复的代码抽取出来，然后将提取出代码编译后运行到需要的地方，而传统的oop只能父子关系纵向重用。

### 7.Aop两种框架

- **spring Aop：**无需专门的加载器，通过代理方式向目标类植入增强的代码
- **AspectJ:**  提供专门编译器，在编译时提供横向代码的植入

### 8.两种动态代理

##### a.JDK动态代理（必须实现接口）

```java
public class JdkProxy implements InvocationHandler {
	//目标类接口
	private UserDao userDao;
    //创建代理方法
	public Object createProxy(UserDao userDao) {
		this.userDao = userDao;
        //类加载器
		ClassLoader classLoader = JdkProxy.class.getClassLoader();
        //被代理对象实现所有接口
		Class[] clazz = userDao.getClass().getInterfaces();
        //使用代理类，增强返回代理后的对象
		return Proxy.newProxyInstance(classLoader, clazz, this);
	}
    /**
    *proxy 被代理的对象
    *method 被执行方法信息
    *args 执行方法参数
    */
	@Override
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
       	//切面类
		MyAspect myAspect = new MyAspect();
		myAspect.check_Permissions();
        //目标类调用方法
		Object obj = method.invoke(userDao, args);
		myAspect.log();
		return obj;
	}

}

```

##### b. CGLIB代理（无需实现接口）

```java
public class CglibProxy implements MethodInterceptor {

	public Object createProxy(Object target) {
        //创建动态类对象
		Enhancer en = new Enhancer();
		en.setSuperclass(target.getClass());
		en.setCallback(this);
		return en.create();
	}
	@Override
	public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
		//创建切面类对象
		MyAspect myAspect = new MyAspect();
		//前增强
		myAspect.check_Permissions();
		//目标方法执行
		Object obj = methodProxy.invokeSuper(proxy, args);
		myAspect.log();
		return obj;
	}

}

```

### 9.Spring AOP

> spring-aop-4.3.6.RELEASE.jar
>
> aopalliance-1.0.jar
>

```java
基于jdk动态代理实现
public class MyAspect implements MethodInterceptor{

	@Override
	public Object invoke(MethodInvocation mi) throws Throwable {
		check();
		Object obj = mi.proceed();
		log();
		
		return obj;
	}

	public void check() {
		System.out.println("检查");
	}
	
	public void log() {
		System.out.println("日志");
	}

}
```

配置文件

```xml
<!-- 确定bean对象 -->
    <bean id="userDao" class="com.itheima.jdk.UserDaoImpl" />
    <!-- 切面 -->
    <bean id="myAspect" class="com.itheima.factorybean.MyAspect" />


    <!-- 代理 -->
    <bean id="userDaoProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
    <!--代理实现接口  -->
    <property name="proxyInterfaces" value="com.itheima.jdk.UserDao" />
    <!-- 目标对象 -->
    <property name="target" ref="userDao" />
    <!-- 环绕通知，指定切面 -->
    <property name="interceptorNames" value="myAspect" />
    <!-- 指定代理方式，true:cglib-->
     <property name="proxyTargetClass" value="true" />
    </bean>
```

### 10.Spring JDBC

> spring-jdbc-4.3.6.jar
>
> spring-tx-4.3.6.jar

JdbcTemplate核心类

- 1.DataSource 获取连接
- 2.SQLException 对sqlException
- 3.JdbcOperations 执行操作的类

##### xml配置

```java
<!--1.配数据源-->

  <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
  	<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
  	<property name="url" value="jdbc:mysql://localhost/spring"></property>
  	<property name="username" value="root"></property>
  	<property name="password" value="123456"></property>
  </bean>
  
<!-- 2.配置jdbc模板-->
  <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
  <!-- set注入dataSource到jdbcTemplate-->
  	<property name="dataSource" ref="dataSource"></property>
  </bean>
  
 <!--3.注入类-->
  <bean id="accountDao" class="com.itheima.jdbc.AccountDaoImpl">
    <!-- 注入jdbcTemplate到accountDao-->
  	<property name="jdbcTemplate" ref="jdbcTemplate"></property>
  </bean>
```

增删改查：

```java
@Test
	public void addAccountTest() {
		ApplicationContext ap = new ClassPathXmlApplicationContext("applicationContext.xml");
		AccountDao accountDao =(AccountDao) ap.getBean("accountDao");
		Account ac = new Account();
		ac.setUsername("jack");
		ac.setBalance(1000.00);
		int num = accountDao.addAccount(ac);
		if(num>0) {
			System.out.println("插入了"+num);
		}else {
			System.out.println("失败！");
		}
	}
	
	@Test
	public void updateAccountTest() {
		ApplicationContext ap = new ClassPathXmlApplicationContext("applicationContext.xml");
		AccountDao accountDao =(AccountDao) ap.getBean("accountDao");
		Account ac = new Account();
		ac.setId(1);
		ac.setUsername("tom");
		ac.setBalance(2000.00);
		int num = accountDao.updateAccount(ac);
		if(num>0) {
			System.out.println("修改了"+num);
		}else {
			System.out.println("修改失败");
		}
	}
	@Test
	public void findAccountByIdTest() {
		ApplicationContext ap = new ClassPathXmlApplicationContext("applicationContext.xml");
		AccountDao accountDao =(AccountDao) ap.getBean("accountDao");
		Account ac = accountDao.findAccountByid(1);
		System.out.println(ac);
	}
	@Test
	public void findAllAccount() {
		ApplicationContext ap = new ClassPathXmlApplicationContext("applicationContext.xml");
		AccountDao accountDao =(AccountDao) ap.getBean("accountDao");
		List<Account> ac = accountDao.findAllAccount();
		for(Account act:ac) {
			System.out.println(act);
		}
	}
```

### 11.Spring事务管理

> spring-tx-4.3.6RELEASE.jar

```xml
<!--4. 事务配置 -->
	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"></property>
	</bean>
```



#### 三大接口作用：

##### a.接口PlatformTransactionManager 事务管理器

> 两个实现类
>
> DataSOurceTransactionManager 配置事务管理
>
> HibemateTransacitonManger：配置Hibernate事务管理

三个方法

- TransactionStatus getTransaction();获取事务状态
- void commit(status)；提交事务
- void rollback(status); 回滚事务

##### b.接口TransactionDefinition 事务规则

四个方法

- getName();获取事务对象名称

- getIsolation level();获取事务隔离级别

- getPropagationBehavior();获取事务传播行为

  ​	传播行为可以控制是否需要创建事务以及如何创建事务，默认行为REQUIRED

- getTimeout();获取事务超时时间

##### c.接口Transaction Status 事务状态

六个方法：

- void flush();

- boolean hasSavepoint();
- boolean isCompleted();
- boolean isNewTransaction();
- boolean isRollbackOnly();
- void setRollbackOnly();

#### 声明式事务管理

##### 1.基于xml：提供tx命名空间来配置事务

```xml
<!--5. 通知 -->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
		<tx:attributes>
			<tx:method name="*" propagation="REQUIRED" isolation="DEFAULT" read-only="false"/>
		</tx:attributes>
</tx:advice>
<!--6. aop -->
<aop:config>
  	<aop:pointcut expression="execution(* com.itheima.jdbc.*.*(..))" id="txPointCut"/>
  	<aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
</aop:config>
```

##### 2.基于Annotation

```xml
1.配置文件中 扫描事务
<tx:annotation-driven transaction-manager="transactionManager"/>
2.Bean类或方法上添加@interface
@Transactional(propagation=Propagation.REQUIRED,isolation=Isolation.DEFAULT,readOnly=false)
```

## MyBatis

> MyBatis框架也称为ORM框架（对象关系映射）--解决面向对象与关系型数据库中数据类型不匹配的技术。。
>
> ----实现
>
> 将java对象与数据库表之间映射关系，自动将java对象持久化到关系型数据库的表中。
>
> ---即
>
> 使用ORM框架胡，应用程序不再直接访问底层数据库，而是以面向对象来操作持久化对象po，而ORM会将面向对象操作转为底层的sql操作。
>
> ---导入jar包
>
> mybatis-3.4.2.jar

##### 1.log4j.properties

为了查看控制台输出的sql语句

```properties
# Global logging configuration
log4j.rootLogger=ERROR, stdout
# 这句需要自己写，其余在pdf手册中找...
log4j.logger.com.itheima=DEBUG
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

##### 2.持久化类Customer与数据库字段对应

即Javabean

##### 3.映射文件CustomerMapper.xml

将对数据库的操作（增删改查）与持久化类映射到一起

```xml
<mapper namespace="com.itheima.mapper.CustomerMapper">
	<select id="findCustomerById" parameterType="Integer"
		resultType="com.itheima.po.Customer">
		select * from t_customer where id = #{id}
	</select>
	<select id="findCustomerByName" parameterType="String" resultType="com.itheima.po.Customer">
		select * from t_customer where username like concat('%',#{value},'%');
	</select>
	<insert id="addCustomer" parameterType="com.itheima.po.Customer">
		insert into t_customer(username,jobs,phone) values(#{username},#{jobs},#{phone});
	</insert>
</mapper>
```

##### 4.mybatis-config.xml

mybatis核心配置文件

> 1.配置mysql链接
>
> 2.配置映射文件mapper

```xml
<configuration>
    <!--1.mysql链接 -->
    	 <!--1.1mysql外部文件db.properties -->
	<properties resource="db.properties" />
	<environments default="mysql">
		<environment id="mysql">
			<transactionManager type="JDBC" />
			<dataSource type="POOLED">
				<property name="driver" value="${jdbc.driver}" />
				<property name="url" value="${jdbc.url}" />
				<property name="username" value="${jdbc.username}" />
				<property name="password" value="${jdbc.password}" />
			</dataSource>
		</environment>
	</environments>
     <!--2.mapper-->
	<mappers>
		<mapper resource="com/itheima/mapper/CustomerMapper.xml" />
	</mappers>
</configuration>
```

db.properties外部文件

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis
jdbc.username=root
jdbc.password=123456
```

##### 5.test

```java
//a.读取mybatis配置文件
InputStream inputStream = Resources.getResourceAsStream(resource);
//b.通过配置文件构建SqlSessionFactory
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
//c.通过SqlSessionFactory打开SQLSession
SqlSession sqlSession = sqlSessionFactory.openSession();
//d.SQLSession执行映射文件中sql
Customer customer = sqlSession.selectOne("com.itheima.mapper"+".CustomerMapper.findCustomerById", 1);
//e.关闭SQLSession
sqlSession.close();
```

