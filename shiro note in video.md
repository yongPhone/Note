### 授权认证

1. 授权需要继承 AuthorizingRealm 类, 并实现其 doGetAuthorizationInfo 方法
2. AuthorizingRealm 类继承自 AuthenticatingRealm, 但没有实现 AuthenticatingRealm 中的 doGetAuthenticationInfo, 所以认证和授权只需要继承 AuthorizingRealm 就可以了. 同时实现他的两个抽象方法.

> doGetAuthorizationInfo：每次用户访问一个Url时都会被调用
>
> doGetAuthenticationInfo：用户登陆是会被shiro回调

- 单Realm授权：
- 多Realm授权：

### 从数据表中初始化资源和权限

```xml
    <!--
    6. 配置 ShiroFilter.
    6.1 id 必须和 web.xml 文件中配置的 DelegatingFilterProxy 的 <filter-name> 一致.
                  若不一致, 则会抛出: NoSuchBeanDefinitionException. 因为 Shiro 会来 IOC 容器中查找和 <filter-name> 名字对应的 filter bean.
    -->
    <bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
        <property name="securityManager" ref="securityManager"/>
        <property name="loginUrl" value="/login.jsp"/>
        <property name="successUrl" value="/list.jsp"/>
        <property name="unauthorizedUrl" value="/unauthorized.jsp"/>

        <property name="filterChainDefinitionMap" ref="filterChainDefinitionMap"></property>

        <!--
        	配置哪些页面需要受保护.
        	以及访问这些页面需要的权限.
        	1). anon 可以被匿名访问
        	2). authc 必须认证(即登录)后才可能访问的页面.
        	3). logout 登出.
        	4). roles 角色过滤器
        -->
        <!--<property name="filterChainDefinitions">-->
            <!--<value>-->
                <!--/login.jsp = anon-->
                <!--/anon.jsp = anon-->
                <!--/login/in = anon-->
                <!--/login/out = logout-->

                <!--/user.jsp = roles[user]-->
                <!--/admin.jsp = roles[admin]-->

                <!--# everything else requires authentication:-->
                <!--/** = authc-->
            <!--</value>-->
        <!--</property>-->
    </bean>

    <!-- 配置一个 bean, 该 bean 实际上是一个 Map. 通过实例工厂方法的方式 -->
    <bean id="filterChainDefinitionMap"
          factory-bean="filterChainDefinitionMapBuilder" factory-method="buildFilterChainDefinitionMap"></bean>

    <bean id="filterChainDefinitionMapBuilder"
          class="com.shangguigu.shiro.factory.FilterChainDefinitionMapBuilder"></bean>
```

```java
public class FilterChainDefinitionMapBuilder  {
    public LinkedHashMap<String, String> buildFilterChainDefinitionMap() {
        LinkedHashMap<String, String> map = new LinkedHashMap<>();
      	//可以在这里将数据库表中的资源的权限对应关系储存进map
        map.put("/login.jsp", "anon");
        map.put("/login/in", "anon");
        map.put("/**", "authc");
        return map;
    }
}
```

### 会话管理

与java web中的HttpSession是一致的

- 获取会话：`Subject.getSession()`
- 在controller层建议使用原生的HttpSession，但是在Service中可以通过Shiro获得Session

### SessionDao

### 会话验证

Shiro提供了会话验证调度器，用于定期地验证会话是否已过期，如果过期将停止会话；

出于性能考虑，一般情况下都是获取会话时来验证会话是否过期并停止会话的

- 在Web环境中，如果用户不主动退出是不知道会话是否过期，因此需要定期的检测会话是否过期，Shiro提供了会话验证调度器

  SessionValidationScheduler

- Shiro也提供了使用Quartz会话验证调度器：QuartzSessionValidationScheduler

### 缓存

- CacheManagerAware接口：Shiro内部对应的组件（DefaultSecurityManager）会自动检测相应的对象（如Realm）是否实现了CacheManagerAware并自动注入相应的CacheManager
- Realm缓存：
  - Shiro提供了CachingRelam，其实现了CacheManagerAware接口，提供了缓存的一些基础实现；
  - AuthenticatingRealm及AuthorizingRealm也分别提供了对AuthenticationInfo 和 AuthorizationInfo 信息的缓存

### RememberMe

认证和记住我：

两者二选一，即 subject.isAuthenticated()==true，则subject.isRemembered()==false；反之一样