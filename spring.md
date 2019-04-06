# Spring

###  1.依赖注入

​	**依赖注入DI：**从spring角度，spring容器负责将被依赖对象给调用者的成员变量，这相当于为调用者注入了它依赖的实例

​	**控制反转IOC：**对象实例不再由调用者控制，而转移到spring容器，控制权发生了反转

### 2.Spring两种核心容器

**BeanFactory**是基础类型的ioc容器，管理bean的工厂，负责初始化各种Bean

**ApplicationContext**是BeanFactory的子接口，添加了新功能。

```java
Spring容器控制
public static void main(String[] args) {
		String xmlPath = "beans1.xml";
		ApplicationContext appli = new ClassPathXmlApplicationContext(xmlPath);
		Bean1 bean =(Bean1) appli.getBean("bean1");
		System.out.println(bean);
	}
```

​	配置文件

```xml
实例化bean
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

singleton：单例

prototype：原型

request:请求

session：回话

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
	<property name="username" vlaue="tom"></property>
</bean>
```

##### b.Annotation装配

spring-aop-4.3.6.RELEASE.jar

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

**spring Aop：**无需专门的加载器，通过代理方式向目标类植入增强的代码

**AspectJ:**提供专门编译器，在编译时提供横向代码的植入

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

##### b. CGCIB代理（无需实现接口）

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

spring-aop-4.3.6.RELEASE.jar

aopalliance-1.0.jar

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

