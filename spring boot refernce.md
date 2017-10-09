# Spring Boot Reference

## Part II. Getting-started

- You can use Spring Boot to create Java applications that can be started using `java -jar` or more traditional war deployments. We also provide a command line tool that runs “spring scripts”.

### Servlet Container

| Name         | Servlet Version | Java Version |
| ------------ | --------------- | ------------ |
| Tomcat 8     | 3.1             | Java 7+      |
| Tomcat 7     | 3.0             | Java 6+      |
| Jetty 9.3    | 3.1             | Java 8+      |
| Jetty 9.2    | 3.1             | Java 7+      |
| Jetty 8      | 3.0             | Java 6+      |
| Undertow 1.3 | 3.1             | Java 7+      |

You can also deploy Spring Boot applications to any Servlet 3.0+ compatible container.

### Creating the POM

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>myproject</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.7.RELEASE</version>
    </parent>

    <!-- Additional lines to be added here... -->

</project>
```

### Writing the code

```java
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*;
import org.springframework.stereotype.*;
import org.springframework.web.bind.annotation.*;

@RestController
@EnableAutoConfiguration
public class Example {

    @RequestMapping("/")
    String home() {
        return "Hello World!";
    }

    public static void main(String[] args) throws Exception {
        SpringApplication.run(Example.class, args);
    }

}
```

- @RestController:

  - our class is a web `@Controller` so Spring will consider it when handling incoming web requests
  - The`@RestController` annotation tells Spring to render the resulting string directly back to the caller.(@restcontroller注释告诉Spring将生成的字符串直接返回给调用者。)

- @EnableAutoConfiguration:

  - This annotation tells Spring Boot to “guess” how you will want to configure Spring, based on the jar dependencies that you have added

- `SpringApplication` 

  will bootstrap our application, starting Spring which will in turn start the auto-configured Tomcat web server

  - `Example.class` as an argument to the `run` method to tell `SpringApplication` which is the primary Spring component. 
  - The `args` array is also passed through to expose any command-line arguments

### Running the example

Type `mvn spring-boot:run` from the root project directory to start the application

### Creating an executable jar

The `spring-boot-starter-parent` POM includes `<executions>` configuration to bind the `repackage` goal. If you are not using the parent POM you will need to declare this configuration yourself

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

## Part III. Using Spring Boot

### Maven

#### Inheriting the starter parent

```xml
<!-- Inherit defaults from Spring Boot -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.7.RELEASE</version>
</parent>
```

- 有了这个，添加其他的starter，可以安全省略版本号

#### Override individual dependencies

```xml
<properties>
    <spring-data-releasetrain.version>Fowler-SR2</spring-data-releasetrain.version>
</properties>
```

Check the [`spring-boot-dependencies` pom](https://github.com/spring-projects/spring-boot/tree/v1.5.7.RELEASE/spring-boot-dependencies/pom.xml) for a list of supported properties

#### Using Spring Boot without the parent POM

If you don’t want to use the `spring-boot-starter-parent`, you can still keep the benefit of the dependency management (but not the plugin management) by using a `scope=import` dependency:

```xml
<dependencyManagement>
     <dependencies>
        <dependency>
            <!-- Import dependency management from Spring Boot -->
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>1.5.7.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

That setup does not allow you to override individual dependencies using a property as explained above. 

To achieve the same result, you’d need to add an entry in the`dependencyManagement` of your project **before** the `spring-boot-dependencies` entry. For instance, to upgrade to another Spring Data release train you’d add the following to your `pom.xml`.

```xml
<dependencyManagement>
    <dependencies>
        <!-- Override Spring Data release train provided by Spring Boot -->
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-releasetrain</artifactId>
            <version>Fowler-SR2</version>
            <scope>import</scope>
            <type>pom</type>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>1.5.7.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

#### Changing the Java version

```xml
<properties>
    <java.version>1.8</java.version>
</properties>
```

#### Starters

Starters are a set of convenient dependency descriptors that you can include in your application

可以在参考文档13.5找到各种烦starter

### Structure my code

#### Locating the main application class

强烈推荐main class在定位在其他类之上的根目录中

The `@EnableAutoConfiguration` annotation is often placed on your main class, and it implicitly defines a base “search package” for certain items. For example, if you are writing a JPA application, the package of the`@EnableAutoConfiguration` annotated class will be used to search for `@Entity` items.（如果您正在编写一个JPA应用程序，那么`@enableautoconfiguration`注释类的包将被用于搜索`@entity`项）

Using a root package also allows the `@ComponentScan` annotation to be used without needing to specify a `basePackage` attribute.

You can also use the`@SpringBootApplication` annotation if your main class is in the root package.

Here is a typical layout:

```ini
com
 +- example
     +- myproject
         +- Application.java
         |
         +- domain
         |   +- Customer.java
         |   +- CustomerRepository.java
         |
         +- service
         |   +- CustomerService.java
         |
         +- web
             +- CustomerController.java
```

The `Application.java` file would declare the `main` method, along with the basic `@Configuration`.

```java
package com.example.myproject;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableAutoConfiguration
@ComponentScan
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```

### Configuration classes

Spring Boot favors Java-based configuration. 

Although it is possible to call `SpringApplication.run()` with an XML source, we generally **recommend that your primary source is a `@Configuration` class.** 

Usually the class that defines the `main` method is also a good candidate as the primary `@Configuration`

#### Importing additional configuration classes

You don’t need to put all your `@Configuration` into a single class. 

1. The `@Import` annotation can be used to import additional configuration classes. 
2. Alternatively, you can use `@ComponentScan` to automatically pick up all Spring components, including `@Configuration` classes.

#### Importing XML configuration

If you absolutely must use XML based configuration, we recommend that you still start with a `@Configuration` class. 

You can then use an additional `@ImportResource` annotation to load XML configuration files.

### Auto-configuration

Spring Boot auto-configuration attempts to automatically configure your Spring application based on the jar dependencies that you have added. For example, If `HSQLDB`is on your classpath, and you have not manually configured any database connection beans, then we will auto-configure an in-memory database.

**You need to <u>opt-in to auto-configuration</u>(选择自动配置) by adding the `@EnableAutoConfiguration` or `@SpringBootApplication` annotations to one of your `@Configuration`classes.**

| ![[Tip]](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/images/tip.png) |
| ---------------------------------------- |
| You should only ever add one `@EnableAutoConfiguration` annotation. We generally recommend that you add it to your primary `@Configuration` class. |

#### Gradually replacing auto-configuration

If you need to find out what auto-configuration is currently being applied, and why, start your application with the `--debug` switch. This will enable debug logs for a selection of core loggers and log an auto-configuration report to the console.

#### Disabling specific auto-configuration

If you find that specific auto-configure classes are being applied that you don’t want, you can use the exclude attribute of `@EnableAutoConfiguration` to disable them.

```java
import org.springframework.boot.autoconfigure.*;
import org.springframework.boot.autoconfigure.jdbc.*;
import org.springframework.context.annotation.*;

@Configuration
@EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})
public class MyConfiguration {
}
```

If the class is not on the classpath, you can use the `excludeName` attribute of the annotation and specify the fully qualified name instead. <u>Finally, you can also control the list of auto-configuration classes to exclude via the `spring.autoconfigure.exclude` property.</u> （最后，您还可以通过spring.autoconfigure来控制自动配置类的列表。排除属性。）

### Spring Beans and dependency injection

**we often using `@ComponentScan` to find your beans, in combination with `@Autowired` constructor injection works well.**

If you structure your code as suggested above (locating your application class in a root package), you can add `@ComponentScan` without any arguments. All of your application components (`@Component`, `@Service`, `@Repository`, `@Controller` etc.) will be automatically registered as Spring Beans.

Here is an example `@Service` Bean that uses constructor injection to obtain a required `RiskAssessor` bean.

```java
package com.example.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class DatabaseAccountService implements AccountService {

    private final RiskAssessor riskAssessor;

    @Autowired
    public DatabaseAccountService(RiskAssessor riskAssessor) {
        this.riskAssessor = riskAssessor;
    }

    // ...

}
```

And if a bean has one constructor, you can omit(省略) the `@Autowired`.（几个构造方法呢？记得试一波）

```java
@Service
public class DatabaseAccountService implements AccountService {

    private final RiskAssessor riskAssessor;

    public DatabaseAccountService(RiskAssessor riskAssessor) {
        this.riskAssessor = riskAssessor;
    }

    // ...

}
```

| ![[Tip]](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/images/tip.png) |
| ---------------------------------------- |
| Notice how using constructor injection allows the `riskAssessor` field to be marked as `final`, indicating that it cannot be subsequently changed. |

### Using the @SpringBootApplication annotation

The `@SpringBootApplication` annotation is equivalent to using `@Configuration`, `@EnableAutoConfiguration` and `@ComponentScan` with their default attributes

### Developer tools

maven

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
```

Developer tools are automatically disabled when running a fully packaged application

#### Property defaults

**Cache options are usually configured by settings in your `application.properties` file.** For example, Thymeleaf offers the `spring.thymeleaf.cache` property. Rather than needing to set these properties manually, the `spring-boot-devtools` module will automatically apply sensible development-time configuration

#### Automatic restart

Applications that use `spring-boot-devtools` will automatically restart whenever files on the classpath change

> **Restart vs Reload**
>
> The restart technology provided by Spring Boot works by **using two classloaders**. Classes that don’t change (for example, those from third-party jars) are loaded into a *base* classloader. Classes that you’re actively developing are loaded into a *restart* classloader. When the application is restarted, the *restart* classloader is thrown away and a new one is created. This approach means that application restarts are typically much faster than “cold starts” since the *base* classloader is already available and populated.
>
> If you find that restarts aren’t quick enough for your applications, or you encounter classloading issues, you could consider reloading technologies such as [JRebel](http://zeroturnaround.com/software/jrebel/)from ZeroTurnaround. These work by **rewriting classes** as they are loaded to make them more amenable to reloading. [Spring Loaded](https://github.com/spring-projects/spring-loaded) provides another option, however it doesn’t support as many frameworks and it isn’t commercially supported.

#### Excluding resources

By default changing resources in `/META-INF/maven`, `/META-INF/resources`, `/resources`, `/static`, `/public` or `/templates` will not trigger a restart but will trigger a [live reload](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using-boot-devtools-livereload). 

If you want to customize these exclusions you can use the `spring.devtools.restart.exclude` property. For example, to exclude only `/static` and `/public` you would set the following:

```properties
spring.devtools.restart.exclude=static/**,public/**
```

| ![[Tip]](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/images/tip.png) |
| ---------------------------------------- |
| if you want to keep those defaults and *add* additional exclusions, use the `spring.devtools.restart.additional-exclude` property instead. |

## Part IV. Spring Boot features

### SpringApplication

- SpringApplication类提供了快速启动spring应用的方式，使得我们可以从main方法启动这个应用。通常，我们只要通过SpringApplicaiton.run方法就可以启动应用。

#### 启动失败

如果应用启动失败，注册的`FailureAnalyzers`就有机会提供一个特定的错误信息，及具体的解决该问题的动作。

例如，如果在`8080`端口启动一个web应用，而该端口已被占用，那你应该可以看到类似如下的内容：

```
***************************
APPLICATION FAILED TO START
***************************

Description:

Embedded servlet container failed to start. Port 8080 was already in use.

Action:

Identify and stop the process that's listening on port 8080 or configure this application to listen on another port.

```

**注** Spring Boot提供很多的`FailureAnalyzer`实现，你[自己实现](http://docs.spring.io/spring-boot/docs/1.4.1.RELEASE/reference/htmlsingle/#howto-failure-analyzer)也很容易。

如果没有可用于处理该异常的失败分析器（failure analyzers），你需要展示完整的auto-configuration报告以便更好的查看出问题的地方，因此你需要启用`org.springframework.boot.autoconfigure.logging.AutoConfigurationReportLoggingInitializer`的debug属性，或开启DEBUG日志级别。

例如，使用`java -jar`运行应用时，你可以通过如下命令启用`debug`属性：

```
$ java -jar myproject-0.0.1-SNAPSHOT.jar --debug
```

#### 自定义Banner

里面有

- 如何启动Banner

  - 通过在classpath下添加一个`banner.txt`或设置`banner.location`来指定相应的文件可以改变启动过程中打印的banner。
  - 如果这个文件有特殊的编码，你可以使用`banner.encoding`设置它（默认为UTF-8）。
  - 除了文本文件，你也可以添加一个`banner.gif`，`banner.jpg`或`banner.png`图片，或设置`banner.image.location`属性。图片会转换为字符画（ASCII art）形式，并在所有文本banner上方显示

- Banner中可用的占位符

  其实主要是一些和版本号有关的东西

  | 变量                                       | 描述                                       |
  | ---------------------------------------- | ---------------------------------------- |
  | ${application.version}                   | MANIFEST.MF中声明的应用版本号，例如`Implementation-Version: 1.0`会打印`1.0` |
  | ${application.formatted-version}         | MANIFEST.MF中声明的被格式化后的应用版本号（被括号包裹且以v作为前缀），用于显示，例如(`v1.0`) |
  | ${spring-boot.version}                   | 当前Spring Boot的版本号，例如`1.4.1.RELEASE`      |
  | ${spring-boot.formatted-version}         | 当前Spring Boot被格式化后的版本号（被括号包裹且以v作为前缀）, 用于显示，例如(`v1.4.1.RELEASE`) |
  | ${Ansi.NAME}（或${AnsiColor.NAME}，${AnsiBackground.NAME}, ${AnsiStyle.NAME}） | NAME代表一种ANSI编码，具体详情查看[AnsiPropertySource](https://github.com/spring-projects/spring-boot/tree/v1.4.1.RELEASE/spring-boot/src/main/java/org/springframework/boot/ansi/AnsiPropertySource.java) |
  | ${application.title}                     | `MANIFEST.MF`中声明的应用title，例如`Implementation-Title: MyApp`会打印`MyApp` |

- 如何禁用Banner

  YAML会将`off`映射为`false`，如果想在应用中禁用banner，你需要确保`off`添加了括号：

  +

  ```
  spring:
      main:
          banner-mode: "off"
  ```

- 如何实现自己的Banner

  如果想以编程的方式产生一个banner，可以使用`SpringBootApplication.setBanner(…)`方法，并实现`org.springframework.boot.Banner`接口的`printBanner()`方法。

- 用于打印的Banner是单例的

  你也可以使用`spring.main.banner-mode`属性决定将banner打印到何处，`System.out`（`console`），配置的logger（`log`）或都不输出（`off`)。

