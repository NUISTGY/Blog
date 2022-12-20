---
title: Spring中注册Bean的方式有哪些？
tags: [Spring, Java]
date: 2022-12-19
categories: [Spring]
---
## 一、概述

我们知道，在使用Spring框架后，Bean统一都交给IOC容器去管理和创建，那么将一个对象加入到Spring容器中有哪些方式呢，今天我们结合具体的代码来总结一下。

## 二、第一种方式： XML配置方式

XML配置的方式，是Spring最早支持的方式，不过现在XML方式已经用的比较少了，基本上都是用后面的配置方式替代了。

```java
public class Student {
}
 
<beans xmlns="http://www.springframework.org/schema/beans"
	   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	   xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	<bean id="student" class="com.wsh.injectbean.method_01.Student"/>
</beans>
 
/**
 * 第一种方式： XML文件配置
 */
public class Client {
	public static void main(String[] args) {
		ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("classpath:spring-config.xml");
		System.out.println(applicationContext.getBean("student"));
	}
}
```

## 三、第二种方式： 使用@Component注解 + @ComponentScan包扫描方式

为了解决bean太多时，XML文件过大，从而导致膨胀不好维护的问题。在Spring2.5中开始支持：@Component、@Repository、@Service、@Controller等注解定义bean。@Component放在类名上面，然后通过@ComponentScan指定一个路径，Spring进行扫描带有@Componet注解的bean，然后加至容器中。

**注意，这种方式不单单指的是@Component注解，还包括@Controler、@Service、@Repository等派生的注解。**

```java
@Component
public class UserHandler {
}
 
@Service
public class UserService {
}
 
@Repository
public class UserDao {
}
 
@Controller
public class UserController {
}
 
@ComponentScan("com.wsh.injectbean.method_02")
@Configuration
public class AppConfig {
}
 
/**
 * 第二种方式： 使用@Component注解 + @ComponentScan包扫描方式
 * 包括@Controler、@Service、@Repository等派生的注解。
 * 为了解决bean太多时，xml文件过大，从而导致膨胀不好维护的问题。在spring2.5中开始支持：@Component、@Repository、@Service、@Controller等注解定义bean。
 * 其实本质上@Controler、@Service、@Repository也是使用@Component注解修饰的。
 * <p>
 * 通常情况下：
 *
 * @Controller：一般用在控制层
 * @Service：一般用在业务层
 * @Repository：一般用在持久层
 * @Component：一般用在公共组件上
 */
public class Client {
	public static void main(String[] args) {
		AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
		System.out.println(applicationContext.getBean("userDao"));
		System.out.println(applicationContext.getBean("userService"));
		System.out.println(applicationContext.getBean("userController"));
		System.out.println(applicationContext.getBean("userHandler"));
	}
}
```

## 四、第三种方式：@Configuration + @Bean方式
这种方式其实也是我们最常用的方式之一，@Configuration用来声明一个配置类，然后使用 @Bean 注解声明一个bean，将其加入到Spring容器中。

**通常情况下，如果项目中有使用到第三方类库中的工具类的话，我们都是采用这种方式注册Bean。**

具体代码如下：

```java
public class Student {
}
 
@Configuration
public class AppConfig {
 
	@Bean
	public Student student() {
		return new Student();
	}
 
}
 
/**
 * 第三种方式：@Configuration + @Bean方式
 * <p>
 * 通常情况下，如果项目中有使用到第三方类库中的工具类的话，我们都是采用这种方式注册Bean
 */
public class Client {
	public static void main(String[] args) {
		AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
		System.out.println(applicationContext.getBean("student"));
	}
}
```

## 五、第四种方式：FactoryBean方式
FactoryBean是一个Bean，它允许我们自定义Bean的创建，主要有三个方法：

1. getObject()：自定义Bean如何创建；
2. getObjectType()：要注册的Bean的类型；
3. isSingleton()：是否单例；

```java
public class User {
}
 
@Component
public class UserFactoryBean implements FactoryBean<User> {
	@Override
	public User getObject() throws Exception {
		return new User();
	}
 
	@Override
	public Class<?> getObjectType() {
		return User.class;
	}
 
	@Override
	public boolean isSingleton() {
		return true;
	}
}
 
@Configuration
@ComponentScan("com.wsh.injectbean.method_04")
public class AppConfig {
 
}
 
/**
 * 第四种方式：使用FactoryBean
 *
 * FactoryBean在一些框架整合上用的比较多，如Mybatis与Spring的整合中：MapperFactoryBean、SqlSessionFactoryBean等
 */
public class Client {
	public static void main(String[] args) {
		AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
		System.out.println(applicationContext.getBean("userFactoryBean"));
		System.out.println(applicationContext.getBean("&userFactoryBean"));
	}
}
```

## 六、第五种方式：@Import方式

```java
public class Student {
}
 
@Import({Student.class})
@Configuration
public class AppConfig {
 
}
 
/**
 * 第五种方式：@Import方式
 */
public class Client {
	public static void main(String[] args) {
		AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
		for (String beanDefinitionName : applicationContext.getBeanDefinitionNames()) {
			System.out.println(beanDefinitionName);
		}
		System.out.println("================");
		System.out.println(applicationContext.getBean("com.wsh.injectbean.method_05.Student"));
		System.out.println(applicationContext.getBean("student"));
	}
}
```

## 七、第六种方式：@Import + ImportSelector方式

首先介绍一下ImportSelector接口的好处，主要有以下两点：

1. 把某个功能的相关类放到一起，方面管理和维护。

2. 重写selectImports方法时，能够根据条件判断某些类是否需要被实例化，或者某个条件实例化这些bean，其他的条件实例化那些bean等，我们能够非常灵活的定制化bean的实例化。

这种方式我们需要实现ImportSelector接口，并重写selectImports()方法，然后将我们要导入的类的全限定名写在里面即可。

**指定需要定义bean的类名，注意要包含完整路径，而非相对路径。**

```java
public class Product {
}
 
public class User {
}
 
public class MyImportSelector implements ImportSelector {
 
	// 指定需要定义bean的类名，注意要包含完整路径，而非相对路径
	@Override
	public String[] selectImports(AnnotationMetadata importingClassMetadata) {
		return new String[]{"com.wsh.injectbean.method_06.Product", "com.wsh.injectbean.method_06.User"};
	}
 
}
 
@Import({MyImportSelector.class})
@Configuration
public class AppConfig {
 
}
 
/**
 * 第六种方式：@Import + ImportSelector方式
 * <p>
 * ImportSelector接口的好处主要有以下两点：
 * <p>
 * 1.把某个功能的相关类放到一起，方面管理和维护。
 * 2.重写selectImports方法时，能够根据条件判断某些类是否需要被实例化，或者某个条件实例化这些bean，其他的条件实例化那些bean等,我们能够非常灵活的定制化bean的实例化。
 */
public class Client {
	public static void main(String[] args) {
		AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
		for (String beanDefinitionName : applicationContext.getBeanDefinitionNames()) {
			System.out.println(beanDefinitionName);
		}
		System.out.println("================");
		System.out.println(applicationContext.getBean("com.wsh.injectbean.method_06.Product"));
		try {
			System.out.println(applicationContext.getBean("product"));
		} catch (Exception e) {
			e.printStackTrace();
		}
		System.out.println("================");
		System.out.println(applicationContext.getBean("com.wsh.injectbean.method_06.User"));
		try {
			System.out.println(applicationContext.getBean("user"));
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

## 八、第七种方式：@Import + ImportBeanDefinitionRegistrar方式?

这种方式我们需要实现ImportBeanDefinitionRegistrar接口，并重写registerBeanDefinitions()方法，然后定义我们需要注册的Bean的定义信息，然后registry.registerBeanDefinition()方法注册即可。**这种方式比ImportSelector更加灵活，可以自定义bean的名称、作用域等很多参数。** 像我们常见的Spring Cloud中的Feign，就使用了ImportBeanDefinitionRegistrar，具体可以参考FeignClientsRegistrar类。

```java
public class User {
}
 
 
public class Product {
}
 
public class CustomImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {
	@Override
	public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
		// 可以自定义bean的名称、作用域等很多参数
		registry.registerBeanDefinition("user", new RootBeanDefinition(User.class));
		RootBeanDefinition rootBeanDefinition = new RootBeanDefinition(Product.class);
		rootBeanDefinition.setScope(BeanDefinition.SCOPE_SINGLETON);
		registry.registerBeanDefinition("product", rootBeanDefinition);
	}
}
 
@Import({CustomImportBeanDefinitionRegistrar.class})
@Configuration
public class AppConfig {
 
}
 
/**
 * 第七种方式：@Import + ImportBeanDefinitionRegistrar方式
 *
 * 可以自定义bean的名称、作用域等很多参数, 像我们常见的Spring Cloud中的Feign，就使用了ImportBeanDefinitionRegistrar
 * class FeignClientsRegistrar implements ImportBeanDefinitionRegistrar, ResourceLoaderAware, EnvironmentAware {
 * 		public void registerBeanDefinitions(AnnotationMetadata metadata, BeanDefinitionRegistry registry) {
 *         this.registerDefaultConfiguration(metadata, registry);
 *         this.registerFeignClients(metadata, registry);
 *     }
 * }
 */
public class Client {
	public static void main(String[] args) {
		AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
		System.out.println(applicationContext.getBean("product"));
		System.out.println(applicationContext.getBean("user"));
	}
}
```

