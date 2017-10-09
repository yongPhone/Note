# Spring Boot 实战

### 开启第一个Spring Boot程序

### 查看初始化的Spring boot项目

#### 启动引导Spring Boot

- Spring的@Configuration：标明该类使用Spring基于Java的配置。（与基于xml的配置相区别）
- Spring的@ConponentScan：启动组件扫描，这样为我们写的控制类和其他组件才能自动发现和注册为Spring应用上下文里的Bean
- Spring Boot的@EnableAutoConfiguration：（也可以称为@Abracadabra）开启Spring Boot自动配置

#### 测试Spring Boot应用程序

在测试类的上面添加三个注解

- @RunWith(SpringJunit4ClassRunner.class)

- @SpringApplicationConfiguration(classes = ReadingListApplication.class)

  表示从ReadingListApplication类加载Spring应用上下文

- @WebAppConfiguration

#### 覆盖起步依赖引入的传递依赖

- 在Maven里，可以对某个依赖使用\<exclusions>来排除传递依赖

  for example:

  ```xml
  <dependency>
  	<groupId>org.springframework.boot</groupId>
    	<artifactId>spring-boot-starter-web</artifactId>
    	<exclusiond>
    		<exclusion>
        		<groupId>org.fasterxml.jackson.core</groupId>
        	</exclusion>
    	</exclusiond>
  </dependency>
  ```

- 如果需要指定使用的版本，可以直接在pom.xml中表达诉求

#### Spring的条件化配置

（Spring4.0引入的新特性）

条件化配置允许配置存在与应用程序中，在满足某些特定的条件之前都要忽略这个配置

##### 在Spring中编写自己的条件

我们要做的就是实现Condition接口，覆盖它的Matches()方法

```java
public class MyCondition implements Condition{
    @Override
    public boolean matches(ConditionContext conditionContext, AnnotatedTypeMetadata annotatedTypeMetadata) {
        System.out.println("满足我的条件了，返回true你就能通过");
        return true;
    }
}
```

```java
@SpringBootConfiguration
public class ConditionBeanConfig {
    @Bean
    @Conditional(MyCondition.class)
    public UseBean useBean1() {
        //MyCondition只有在MyCondition的matches方法返回true时候，useBean1才会被创建
        return new UseBean();
    }

    @Bean
    public UseBean useBean2() {
        return new UseBean();
    }
}
```

- @Bean注解，会自动配置一个和方法名字一样的Bean
- `@Conditianal`不仅可以写在方法上，还可以写在configuration.class上，这样该类中的所有bean都需要满足自定义的Condition才能被自动加载

#####  自动化配置中使用的条件注解

| 条件化注解                          | 配置生效条件                                   |
| ------------------------------ | ---------------------------------------- |
| @ConditionalOnBean             | 配置了某个特定的Bean                             |
| @ConditionalOnMissingBean      | 没有配置特定的Bean                              |
| @ConditionalOnClass            | Classpath中有指定的类                          |
| @ConditionalOnMissingClass     | Classpath中缺少指定的类                         |
| @ConditionalOnExpression       | 给定的Spring Expression Language (SpEL) 表达式计算结果为true |
| @ConditionalOnJava             | Java版本匹配指定值或者一个范围值                       |
| @ConditionalOnJudi             | 参数中给定的JNDI位置必须存在一个，如果没有给定参数，则要有JNDI InitialContext |
| @ConditionalOnProperty         | 指定的配置属性要有一个明确的值                          |
| @ConditionalOnResource         | Classpath里有指定的资源                         |
| @ConditionalOnWebApplication   | 这是一个WEB应用程序                              |
| @ConditionalOnNoWebApplicaiton | 这不是一个WEB应用程序                             |

## 自定义配置

### 覆盖Spring Boot的自动配置

#### 保护应用程序

1. 添加依赖

   ```xml
   		<dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-security</artifactId>
           </dependency>
   ```

   - Classpath里有Spring Security之后，自动配置就能介入一个基本的Spring Security配置

2. 创建自定义安全配置

   ```java
   @Configuration
   @EnableWebSecurity
   public class SecurityConfig extends WebSecurityConfigurerAdapter{

       @Override
       protected void configure(AuthenticationManagerBuilder auth) throws Exception {
           auth.userDetailsService(new UserDetailsService() {
               @Override
               public UserDetails loadUserByUsername(String s) throws UsernameNotFoundException {

                   return (UserDetails) new User("name", "password");
               }
           });
       }

       @Override
       protected void configure(HttpSecurity http) throws Exception {
           http.authorizeRequests().antMatchers("/").access("hasRole('READER')")
                   .antMatchers("/**").permitAll().and().formLogin().loginPage("/login").failureUrl("/login?error=true");
       }
   }
   ```

   - 扩展WebSecurityConfigurerAdapter的配置类可以覆盖两个不同的configure()方法。
   - 在dispatcherServlet启动之后就会依此调用者两个方法分别对AuthenticationManagerBuilder和HttpSecurity进行配置

#### 覆盖自动配置的整个类

大部分情况下，@ConditionalOnMissingBean注解是覆盖自动配置的关键，这种情况下，我们一般只要自己实现了@ConditionalOnMissingBean指定的Bean就可以使用自己的Bean覆盖自动配置的Bean

#### 通过属性文件外置配置

很多时候不需要完全掌控Bean，只需要对自动配置的Bean中的属性进行微调

Spring Boot获得属性的地方（优先级同序号）

1. 命令行参数
2. java:comp/env里的JNDI属性
3. JVM系统属性
4. 操作系统环境变量
5. 随机生成的带random.*前缀的属性（在设置其他属性时，可以引用它们，比如${random.long}）
6. 应用程序以外的application.properties或者application.yml文件
7. 打包在应用程序内的application.properties或者application.yml文件
8. 通过@PropertySource标注的属性源
9. 默认属性

- applicaion.properties和application.pml文件可以放在以下四个位置（优先级同序号）

  1. 外置，在相对与应用程序运行目录的/config子目录里面
  2. 外置，在应用程序运行的目录里
  3. 内置，在config包内
  4. 内置，在Classpath根目录

  注：同一个优先级位置同时有application.properties和applicaion.yml，那么applicaion.yml里的属性会覆盖applicaiton.properties中的属性

##### 自动配置微调

###### 1. 禁用模版缓存

将spring.thymeleaf.cache设置为false就能禁用Thymeleaf模板缓存。

>  作为开发者，在修改模板时始终关闭缓存实在太方便了。为此，可以通过环境变量来禁用
>
>  Thymeleaf缓存：$ export spring_thymeleaf_cache=false 
>
>  此处使用Thymeleaf作为应用程序的视图，Spring Boot支持的其他模板也能关闭模板缓存，
>
>  设置这些属性就好了：
>
>   spring.freemarker.cache（Freemarker）
>
>   spring.groovy.template.cache（Groovy模板）
>
>   spring.velocity.cache（Velocity）
>
>  默认情况下，这些属性都为true，也就是开启缓存。将它们设置为false即可禁用缓存

###### 2. 配置嵌入式服务

- 设置端口，直接设置server.port

- 设置HTTPS服务

  [贼优秀的参考教程](http://blog.csdn.net/RO_wsy/article/details/51319963)

  1. 使用keytool生成ssl证书

     `keytool -genkey -alias tomcat  -storetype PKCS12 -keyalg RSA -keysize 2048  -keystore keystore.p12 -validity 3650`

  2. 设置属性

     ```properties
     server.port=8443
     server.ssl.key-store=classpath:keystore.p12
     server.ssl.key-store-password=123456
     server.ssl.keyStoreType=PKCS12
     server.ssl.keyAlias=tomcat1
     ```

  3. 把ssl证书弄到设置的路径下就可以了

  4. 如果要兼容http协议或者其他端口，可以将请求重定向到https

     - application.properties中不能同时配置两个connector，所以要以编程的方式配置HTTP connector，然后重定向到HTTPS connector

     需要在配置类中配置一个TomcatEmbeddedServletContainerFactory bean：

     ```java
       @Bean
       public EmbeddedServletContainerFactory servletContainer() {

         TomcatEmbeddedServletContainerFactory tomcat = new TomcatEmbeddedServletContainerFactory() {

             @Override
             protected void postProcessContext(Context context) {

               SecurityConstraint securityConstraint = new SecurityConstraint();
               securityConstraint.setUserConstraint("CONFIDENTIAL");
               SecurityCollection collection = new SecurityCollection();
               collection.addPattern("/*");
               securityConstraint.addCollection(collection);
               context.addConstraint(securityConstraint);
             }
         };
         tomcat.addAdditionalTomcatConnectors(initiateHttpConnector());
         return tomcat;
       }

       private Connector initiateHttpConnector() {

         Connector connector = new Connector("org.apache.coyote.http11.Http11NioProtocol");
         connector.setScheme("http");
         connector.setPort(8080);
         connector.setSecure(false);
         connector.setRedirectPort(8443);
         return connector;
       }
     ```

###### 3. 配置日志

默认情况下：Spring Boot会用[Logback](http://logback.qos.ch)来记录日志，并用INFO级别输出到控制台。

> **用其他日志替换LogBack：**
>
> 如果要用Log4j或者Log4j2，只需要修改依赖，引入对应该日志实现的起步依赖，同时排除掉Logback
>
> 排除Logback：
>
> ```xml
> <dependency> 
>  	<groupId>org.springframework.boot</groupId> 
>  	<artifactId>spring-boot-starter</artifactId> 
>  	<exclusions> 
>  		<exclusion> 
>  			<groupId>org.springframework.boot</groupId> 
> 		 	<artifactId>spring-boot-starter-logging</artifactId> 
>  		</exclusion> 
>  	</exclusions> 
> </dependency> 
> ```
>
> 引入Log4j：
>
> ```xml
> <dependency> 
>  	<groupId>org.springframework.boot</groupId> 
>  	<artifactId>spring-boot-starter-log4j</artifactId> 
>   	<version>1.3.8.RELEASE</version>
> </dependency> 
> ```

- 如果想要完全掌控日志配置，可以在Classpath的根目录里创建logback.xml文件

  for example:

  ```xml
  <configuration>
      <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
          <encoder>
              <pattern>
                  %d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n
              </pattern>
          </encoder>
      </appender>
      <logger name="root" level="INFO"/>
      <root level="INFO">
          <appender-ref ref="STDOUT" />
      </root>
  </configuration>
  ```

- 使用Spring Boot配置属性

  - 设置日志级别：创建logging.level开头的属性，后面是要日志名称

    ```properties
    logging: 
     level: 
     root: WARN 
     org: 
     	springframework: 
     		security: DEBUG 
    ```

  - 设置日志输出路径

    设置logging.path和logging.file属性

    ```properties
    logging: 
     path: /var/logs/ 
     file: BookWorm.log 
     level: 
     	root: WARN 
     	org: 
     		springframework: 
     			security: DEBUG 
    ```

  -  默认情况下，日志文件大小达到10MB时就会切分一次

  - 如果希望对日志完全掌控，但是有不想用logback.xml作为Logback的名字，可以通过logging.config属性自定义的名字

    `loggingn.config.classpath:logging-config.xml`

  ###### 4. 配置数据源

  通常无需指定JDBC驱动，Spring  Boot会根据数据库的URL识别出要的驱动,但是如果自动识别出现问题，也手动设置spring.datadourcce.driver-class-name：

  ```properties
  spring: 
   	datasource: 
   		url: jdbc:mysql://localhost/readinglist
   		username: dbuser 
   		password: dbpass 
  		driver-class-name: com.mysql.jdbc.Driver 
  ```

  - 在自动配置DataSource Bean的时候，Spring会使用这里的连接数据。DataSource Bena是一个连接池，如果Classpath里有Tomcat的连接池DataSource，那么么就会使用这个连接池；否则，Spring Boot就会在Classpath里查找以下连接池

    - HikariCP
    - Commons DBCP
    - Commons DBCP 2

    这些只是自动配置支持的连接池，我们可以配置DataSource Bean，使用自己喜欢的连接池

##### 应用程序Bean的配置外置

1. application.properties

   ```properties
   amazon.first=this is first properties
   amazon.second=this is second properties
   ```

2. Controller中直接使用这些属性

   ```java
   @Controller
   @ConfigurationProperties(prefix="amazon")
   public class PropertiesTest {
       private String first;
       private String second;

       public void setFirst(String first) {
           this.first = first;
       }

       public void setSecond(String second) {
           this.second = second;
       }

       @ResponseBody
       @RequestMapping("/properties")
       public String properties() {
           return "first:" + first + "second:" + second;
       }
   }
   ```

   - `@ConfigurationProperties(prefix="properties")`指定要使用配置文件中的属性，并且指定前缀是properties的

   - 一定要有`setter` 方法，因为这些属性都是通过setter注入的

     > **开启属性配置：**
     >
     > 从技术上来说，@ConfigurationProperties注解不会生效，除非先向Spring配置类添加@EnableConfigurationProperties注解。但是通常不需要这么做，因为Spring Boot自动配置的后面的全部配置类都已经加上了@EnableConfigurationProperties注解（除非你完全不使用自动配置）

     > Spring Boot的属性解析器非常智能，它自动把驼峰规则的属性和使用连字符或下划线的同名属性关联起来：即是说，amazon.assodiateId这个属性和amazon.associate_id和amazon.assocoate-id都是等价的

###### 在一个类里面收集属性

上面那种直接通过amazon前缀获取属性的方式不够优雅，因为这个类看起来和amazon的关系并不大，只是要用一些属性而已，所以我们可以把amazon前缀的属性都收集到一个Bean中

```java
@Component
@ConfigurationProperties(prefix = "amazon")
public class AmazonProperties {
    private String first;
    private String second;

    public String getFirst() {
        return first;
    }

    public void setFirst(String first) {
        this.first = first;
    }

    public String getSecond() {
        return second;
    }

    public void setSecond(String second) {
        this.second = second;
    }
}
```

```java
@Controller
@ComponentScan
public class PropertiesTest {
    @Autowired
    private AmazonProperties amazonProperties;

    @ResponseBody
    @RequestMapping("/properties")
    public String properties() {
        return "first:" + amazonProperties.getFirst() + "second:" + amazonProperties.getSecond();
    }
}
```

#### 使用profile进行配置

当应用程序要部署到不同的环境（比如开发和生产环境）时，一些细节配置会有所不同。

Profile是一种条件化配置，基于运行时激活的profile，会使用或者不同的Bean或配置类。

- 在某个类上用@Profile("部署环境")，可以运行时激活该部署环境的profile，这样才能应用该配置。如果该环境的profile没有激活，就会忽略该配置。

- 设置`spring.profiles.active=部署环境`属性就能激活profile

  - 使用提供特定于profile的属性文件

    - 如果正在使用application.properties，就可以创建额外的属性文件，命名格式为`application-{profile}.properties` ，具体用法同application.properties

      eg:`application-development.properties`

  - 使用多Profile YAML文件进行配置

    - 可以像application.properties进行分文件配置

    - 也可以整合到一个文件配置

      for example:

      ```properties
      logging: 
       level: 
       root: INFO 
      --- 
      spring: 
       profiles: development 
      logging: 
       level: 
       root: DEBUG 
      --- 
      spring: 
       profiles: production 
      logging: 
       path: /tmp/ 
       file: BookWorm.log 
       level: 
       root: WARN 
      ```


## 测试

### 集成测试自动配置

依赖：

```xml
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <version>1.5.6.RELEASE</version>
    <scope>test</scope>
</dependency>
```

测试类：

在测试类头加上如下两个注解

```java
@RunWith(SpringJUnit4ClassRunner.class)
//指定如何加载加载应用程序上下文
@ContextConfiguration(classes = TestController.class)
```

- `@ContextConfiguration` 并没有能够加载完整的Spring Boot，Spring Boot最终是由SpringApplication加载的，SpringApplication不仅加载应用程序上下文，还会开启日志、加载外部属性（application.properties或 application.yml），以及其他的Spring Boot属性

  > 因此，可以将@ContextConfiguration替换为Spring Boot的@SpringApplicationConfiguration

- @SpringApplicationConfiguration：和@ContextConfiguratioin的用法大致相同

  - 可能由于版本原因`@SpringApplicationConfiguration` 用不了，使用`@SpringBootTest`代替

### 测试Web应用

要恰当地测试一个Web应用程序，需要投入一些实际的HTTP请求，确认它能正确处理这些请求

- （方案一）Spring Mock MVC： 能在一个近似真实的模拟Servlet容器里测试控制类，而不用实际启动应用服务器
- （方案二）Web集成测试：在嵌入式Servlet容器（比如tomcat或jetty）里启动应用程序，在真正的应用服务器里执行测试

#### Spring Mock MVC

```java
@RunWith(SpringJUnit4ClassRunner,class)
@SpringApplicationConfiguration(classes = ReadingListApplication.class)
@WebAppConfiguration
public class MockMvcWebTests {
	@Autowired
	private WebApplicationContext webContext;
	private MockMvc mockMvc;
  
	@Before
	public void setupMockMvc() {
		mockMvc = MockMvcBuilders
			.webAppContextSetup(webContext)
			.build();
	}
}
```

- @WebAppConfiguration注解声明，优SpringJUnit4ClassRunner创建的应用程序上下文应该是一个WebApplicationContext（相对于非基本的WebApplicaitionContext）

测试：

```java
@Test
public void homePage() throws Exception {
  	//首先向/readingList发起一个GET请求
    mockMvc.perform(MockMvcRequestBuilders.get("/readingList"))
      	//接下来希望该请求处理成功（isOk()会判断HTTP 200响应码）
    	.andExpect(MockMvcResultMatchers.status().isOk())
    	.andExpect(MockMvcResultMatchers.view().name("readingList"))
    	.andExpect(MockMvcResultMatchers.model().attributeExists("books"))
    	.andExpect(MockMvcResultMatchers.model().attribute("books",
   	 	Matchers.is(Matchers.empty())));
}
```

##### 测试Web安全

依赖：

```xml
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-test</artifactId>
            <scope>test</scope>
        </dependency>
```

在测试类中创建MockMvc实例使运用Spring Security配置器

```java
    @Before
    public void setupMockMvc() {
        mockMvc = MockMvcBuilders.webAppContextSetup(webApplicationContext).apply(SecurityMockMvcConfigurers.springSecurity()).build();
    }
```

- 只需像上面这样运用就行了， Spring Security会介入MockMvc上执行的每个请求 

- 开启了Spring Security之后，在请求主页的时候，我们便不能只期待HTTP 200响应。如果请
  求未经身份验证，我们应该期待重定向到登录页面：

  ```java
  @Test
  public void homePage_unauthenticatedUser() throws Exception {
  	mockMvc.perform(get("/"))
          .andExpect(status().is3xxRedirection())
          .andExpect(header().string("Location",
          "http://localhost/login"));
  }
  ```

#### 测试运行中的应用程序

将@WebAppConfiguration注解换成@WebIntegrationTest，声明不仅希望Spring Boot为测试创建应用程序上下文，还要启动一个嵌入式的Servlet容器（@WebIntegrationTest在1.5之后就没有了）

- 使用Spring的RestTemplete向应用程序发HTTP请求

其他功能看书好了，测试看不下去了了了