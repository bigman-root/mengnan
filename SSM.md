[TOC]



### 一、Spring

#### 1.1 配置一个bean的方式?注解/xml

```java
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 定义Bean -->
    <bean id="myBean" class="com.example.MyBean">
        <!-- 设置属性值 -->
        <property name="message" value="Hello, World!"/>
    </bean>

    <!-- 引入其他配置文件 -->
    <import resource="another-config.xml"/>
</beans>
```

#### 1.2 spring 自动装配 bean 有哪些方式?

1. byName自动装配： 当一个Bean的属性名称与其他Bean的名称相匹配时，Spring会自动将相应的Bean注入到该属性中。
2. byType自动装配： 当一个Bean的属性类型与其他Bean的类型相匹配时，Spring会自动将相应的Bean注入到该属性中。如果存在多个符合条件的Bean，会抛出异常。
3. constructor自动装配： 类似于byType自动装配，但是应用于构造函数参数，Spring会自动在容器中查找与构造函数参数类型匹配的Bean，并通过构造函数注入依赖。
4. 使用@Autowired注解： 通过在属性、构造函数或方法上使用@Autowired注解，告诉Spring进行自动装配。Spring会根据注解标记的类型或名称自动查找并注入对应的Bean。

#### 1.3 spring 常用的注入方式有哪些?

1. 构造函数注入（Constructor Injection）： 通过构造函数注入依赖对象。在类的构造函数中声明依赖的参数，并在创建类实例时传入依赖对象。

2. Setter方法注入（Setter Injection）： 通过Setter方法注入依赖对象。在类中定义Setter方法，通过调用Setter方法来设置依赖对象。

   ```java
   public class UserService {
       private UserRepository userRepository;
   
       public void setUserRepository(UserRepository userRepository) {
           this.userRepository = userRepository;
       }
   
       // ...
   }
   ```

   

3. 字段注入（Field Injection）： 通过字段注入依赖对象。在类的字段上使用`@Autowired`或`@Resource`注解来标记依赖对象，Spring会自动将相应的Bean注入到字段中。

   ```java
   public class UserService {
       @Autowired
       private UserRepository userRepository;
   
       // ...
   }
   ```

#### 1.4 @Component和@Bean的区别?

1. `@Component`：
   - `@Component`是一个通用的注解，用于标识一个类为Spring容器的组件（Component）。
   - 通过使用`@Component`注解，我们可以将一个类自动注册为Spring容器中的Bean，并由Spring负责管理该Bean的生命周期。
   - `@Component`注解可以用于任何类，包括自定义类、第三方库类等。
2. `@Bean`：
   - `@Bean`是一个在配置类中使用的注解，用于显式地声明一个方法返回一个Bean对象。
   - 通过在配置类中使用`@Bean`注解，我们可以定制化地创建和配置一个Bean对象，然后将其纳入Spring容器的管理。
   - `@Bean`注解通常与`@Configuration`注解一起使用，用于标识一个类为配置类，其中定义了一个或多个`@Bean`方法。
   - `@Bean`注解的方法可以设置参数、返回值和Bean的作用域等属性。

#### 1.5 spring 事务实现方式有哪些?

1. 编程式事务管理（Programmatic Transaction Management）： 在代码中显式地使用事务管理API来控制事务的开始、提交或回滚。通过`TransactionTemplate`或`PlatformTransactionManager`等接口来编写事务管理代码。

   ```java
   public class UserService {
       private PlatformTransactionManager transactionManager;
   
       public void setTransactionManager(PlatformTransactionManager transactionManager) {
           this.transactionManager = transactionManager;
       }
   
       public void performBusinessLogic() {
           DefaultTransactionDefinition txDefinition = new DefaultTransactionDefinition();
           TransactionStatus txStatus = transactionManager.getTransaction(txDefinition);
   
           try {
               // 执行业务逻辑
               // ...
   
               transactionManager.commit(txStatus);
           } catch (Exception e) {
               transactionManager.rollback(txStatus);
               throw e;
           }
       }
   }
   
   ```

   

2. 声明式事务管理（Declarative Transaction Management）： 基于AOP（Aspect-Oriented Programming）的方式，通过在配置文件或注解中声明事务的属性来实现事务管理。可以通过XML配置文件中的`<tx:advice>`元素，或通过在方法上使用`@Transactional`注解来声明事务属性。

   ```java
   <bean id="userService" class="com.example.UserService">
       <property name="userRepository" ref="userRepository" />
   </bean>
   
   <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
       <property name="dataSource" ref="dataSource" />
   </bean>
   
   <tx:advice id="txAdvice" transaction-manager="transactionManager">
       <tx:attributes>
           <tx:method name="*" propagation="REQUIRED" />
       </tx:attributes>
   </tx:advice>
   
   <aop:config>
       <aop:pointcut id="userServicePointcut" expression="execution(* com.example.UserService.*(..))" />
       <aop:advisor advice-ref="txAdvice" pointcut-ref="userServicePointcut" />
   </aop:config>
   
   ```

   

3. 注解驱动事务管理（Annotation-Driven Transaction Management）： 是声明式事务管理的一种形式，通过在配置文件中启用注解驱动的事务管理，使用`@EnableTransactionManagement`注解或`<tx:annotation-driven>`配置元素来启用。然后在需要事务管理的方法上使用`@Transactional`注解来声明事务属性。

   ```java
   @Service
   @Transactional
   public class UserService {
       @Autowired
       private UserRepository userRepository;
   
       public void performBusinessLogic() {
           // 执行业务逻辑
           // ...
       }
   }
   
   ```

   

4. 注入式事务管理（Injection Transaction Management）： 通过将事务管理器注入到需要事务管理的Bean中，在Bean中直接调用事务管理器的方法来控制事务的开始、提交或回滚。

   ```java
   @Service
   public class UserService {
       private PlatformTransactionManager transactionManager;
   
       @Autowired
       public void setTransactionManager(PlatformTransactionManager transactionManager) {
           this.transactionManager = transactionManager;
       }
   
       public void performBusinessLogic() {
           TransactionStatus txStatus = transactionManager.getTransaction(new DefaultTransactionDefinition());
   
           try {
               // 执行业务逻辑
               // ...
   
               transactionManager.commit(txStatus);
           } catch (Exception e) {
               transactionManager.rollback(txStatus);
               throw e;
           }
       }
   }
   
   ```

#### 1.6 spring事务的传播机制?

1. REQUIRED（默认）：
   - 如果当前存在事务，则加入该事务并在事务范围内执行。
   - 如果当前没有事务，则创建一个新的事务并在事务范围内执行。
2. SUPPORTS：
   - 如果当前存在事务，则加入该事务并在事务范围内执行。
   - 如果当前没有事务，则以非事务方式执行。
3. MANDATORY：
   - 如果当前存在事务，则加入该事务并在事务范围内执行。
   - 如果当前没有事务，则抛出异常。
4. REQUIRES_NEW：
   - 创建一个新的事务，并挂起当前事务（如果存在）。
   - 在新的事务中执行，如果有异常则回滚新事务，不会影响当前事务。
5. NOT_SUPPORTED：
   - 以非事务方式执行，如果当前存在事务，则挂起该事务。
6. NEVER：
   - 以非事务方式执行，如果当前存在事务，则抛出异常。
7. NESTED：
   - 如果当前存在事务，则在当前事务的嵌套事务中执行。
   - 如果当前没有事务，则创建一个新的事务并在事务范围内执行。

#### 1.7 spring 的事务隔离?

**1、读未提交** 

Read uncommitted：最低级别，以上情况均无法保证。 

**2、读已提交** 

Read committed：可避免脏读情况发生。 

**4、可重复读** 

Repeatable read：可避免脏读、不可重复读情况的发生。不可以避免虚读。 

**8、串行化读 Serializable**：事务只能一个一个执行，避免了脏读、不可重复读、幻读。执行效率慢，使用时慎重 

### 二、SpringMVC

#### 2.1 SpringlIvc用什么对象从后台向前台传递数据的?

在Spring MVC中，可以使用Model对象从后台向前台传递数据。Model对象是一个接口，可以通过方法参数进行注入或在方法返回值中使用。常见的实现类是`ModelMap`、`ModelAndView`和`Model`。

1. ModelMap：
   - `ModelMap`是Spring MVC提供的一个实现了Model接口的类。
   - 可以通过方法参数进行注入，也可以作为方法的返回值。
   - 用于在后台控制器方法中存储和传递数据。
   - 可以使用`addAttribute()`方法添加属性和值。
2. ModelAndView：
   - `ModelAndView`是一个包含模型数据和视图信息的类。
   - 在后台控制器方法中作为方法的返回值使用。
   - 可以使用`addObject()`方法添加属性和值。
3. Model：
   - `Model`是一个接口，是`ModelMap`的父接口。
   - 可以通过方法参数进行注入，也可以作为方法的返回值。
   - 提供了一系列方法来操作模型数据。

#### 2.2 SpringVC 常用注解都有哪些?

1. `@Controller`：
   - 用于标识控制器类，处理请求并返回响应。
2. `@RequestMapping`：
   - 用于将请求映射到控制器的方法上，指定请求的URL路径和HTTP方法。
3. `@GetMapping`：
   - 缩写形式的`@RequestMapping`，用于处理GET请求。
4. `@PostMapping`：
   - 缩写形式的`@RequestMapping`，用于处理POST请求。
5. `@PutMapping`：
   - 缩写形式的`@RequestMapping`，用于处理PUT请求。
6. `@DeleteMapping`：
   - 缩写形式的`@RequestMapping`，用于处理DELETE请求。
7. `@RequestParam`：
   - 用于获取请求参数的值，可以指定参数的名称、是否必需、默认值等。
8. `@PathVariable`：
   - 用于获取路径变量的值，将URL中的占位符与方法参数进行绑定。
9. `@RequestBody`：
   - 用于将请求体中的数据绑定到方法参数上，常用于处理JSON或XML格式的请求数据。
10. `@ResponseBody`：
    - 用于将方法的返回值直接写入HTTP响应体中，常用于返回JSON或XML格式的响应数据。
11. `@ModelAttribute`：
    - 用于绑定请求参数到模型对象上，常用于表单提交时将表单数据绑定到对象。
12. `@Valid`：
    - 用于在数据绑定时进行校验，常与`javax.validation`包中的注解一起使用。
13. `@ExceptionHandler`：
    - 用于定义全局或局部的异常处理方法，处理特定类型的异常。
14. `@SessionAttributes`：
    - 用于指定控制器中的模型属性与会话属性的映射关系，常用于跨请求共享数据。

1. `@Controller`：
   - 用于标识控制器类，处理请求并返回响应。
2. `@RequestMapping`：
   - 用于将请求映射到控制器的方法上，指定请求的URL路径和HTTP方法。
3. `@GetMapping`：
   - 缩写形式的`@RequestMapping`，用于处理GET请求。
4. `@PostMapping`：
   - 缩写形式的`@RequestMapping`，用于处理POST请求。
5. `@PutMapping`：
   - 缩写形式的`@RequestMapping`，用于处理PUT请求。
6. `@DeleteMapping`：
   - 缩写形式的`@RequestMapping`，用于处理DELETE请求。
7. `@RequestParam`：
   - 用于获取请求参数的值，可以指定参数的名称、是否必需、默认值等。
8. `@PathVariable`：
   - 用于获取路径变量的值，将URL中的占位符与方法参数进行绑定。
9. `@RequestBody`：
   - 用于将请求体中的数据绑定到方法参数上，常用于处理JSON或XML格式的请求数据。
10. `@ResponseBody`：
    - 用于将方法的返回值直接写入HTTP响应体中，常用于返回JSON或XML格式的响应数据。
11. `@ModelAttribute`：
    - 用于绑定请求参数到模型对象上，常用于表单提交时将表单数据绑定到对象。
12. `@Valid`：
    - 用于在数据绑定时进行校验，常与`javax.validation`包中的注解一起使用。
13. `@ExceptionHandler`：
    - 用于定义全局或局部的异常处理方法，处理特定类型的异常。
14. `@SessionAttributes`：
    - 用于指定控制器中的模型属性与会话属性的映射关系，常用于跨请求共享数据。

#### 2.3 spring mvc 有哪些组件?

1. DispatcherServlet（派发器Servlet）：
   - 是Spring MVC框架的核心组件，负责接收所有的HTTP请求，并将请求分发给相应的处理器。
2. HandlerMapping（处理器映射）：
   - 用于确定请求应该由哪个处理器来处理，根据请求的URL或其他条件进行匹配。
3. HandlerAdapter（处理器适配器）：
   - 负责将请求分发给处理器，并将处理器的执行结果封装为ModelAndView对象返回给DispatcherServlet。
4. Handler（处理器）：
   - 是实际处理请求的组件，可以是控制器（Controller）类或其他类型的处理器。
5. ViewResolver（视图解析器）：
   - 用于将逻辑视图名称解析为具体的视图对象，可以是JSP、Thymeleaf、Freemarker等视图技术。
6. View（视图）：
   - 负责渲染模型数据并生成响应内容，通常是一个模板或页面。
7. Model（模型）：
   - 用于封装处理器处理后的数据，可以是Java对象、Map、ModelAndView等。
8. HandlerInterceptor（处理器拦截器）：
   - 在请求处理过程中的特定时机进行拦截，用于实现一些通用的处理逻辑，如日志记录、权限验证等。
9. LocaleResolver（区域解析器）：
   - 用于确定请求的区域信息，以便国际化和本地化的处理。
10. ThemeResolver（主题解析器）：
    - 用于确定请求的主题信息，以便在视图渲染时应用不同的主题样式。
11. HandlerExceptionResolver（异常解析器）：
    - 用于处理处理器执行过程中抛出的异常，将异常转换为合适的响应或错误页面。

![img](https://img-blog.csdnimg.cn/d89681c54ca9476c9f00539f94bb2387.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 三、Mybatis

#### 3.1 # 和$ 的区别是什么?

1. ＃符号：

   - `#`符号用于预编译的语句，也称为安全的占位符。

   - 当使用`#`符号时，MyBatis会将参数值转义并将其作为预编译的参数传递给数据库，可以有效防止SQL注入攻击。

   - `#`符号可以用于任何参数类型，包括基本类型、包装类型和自定义类型。

     ```java
     <select id="getUserById" resultType="User">
       SELECT * FROM users WHERE id = #{id}
     </select>
     //在上面的示例中，#{id}是一个预编译的占位符，MyBatis会将传入的id参数值安全地转义并插入到SQL语句中。
     ```

2. 

3. $符号：

   - `$`符号用于拼接字符串，也称为非安全的占位符。

   - 当使用`$`符号时，MyBatis会直接将参数值插入到SQL语句中，不会进行预编译或转义处理。

   - `$`符号主要用于动态拼接SQL语句的部分，如表名、列名等，而不适合用于传递参数值。

     ```java
     <select id="getUserByName" resultType="User">
       SELECT * FROM users WHERE username = '${username}'
     </select>
     //在上面的示例中，${username}是一个非安全的占位符，传入的username参数值会直接插入到SQL语句中，可能存在SQL注入的风险。
     ```

#### 3.2 MyBatis 有几种分页方式?

1. 基于数据库的分页：
   - 在SQL语句中使用数据库的分页功能，如MySQL的`LIMIT`关键字、Oracle的`ROWNUM`等。
   - 通过在SQL语句中指定起始行和查询行数来实现分页。
2. 基于物理分页插件（如PageHelper）：
   - 使用第三方分页插件，如MyBatis的PageHelper插件。
   - 集成分页插件后，可以通过在代码中设置分页参数（页码、每页记录数）来实现分页查询。
3. 基于拦截器的分页：
   - 使用自定义拦截器拦截执行的SQL语句，在执行前后对结果进行分页处理。
   - 在拦截器中截取查询结果的指定范围，返回分页后的结果。
4. 基于游标的分页（如MySQL的游标方式）：
   - 使用数据库游标来实现分页查询，适用于大数据量分页查询的场景。
   - 通过在SQL语句中声明游标并进行游标操作，实现分批获取数据。

#### 3.3 逻辑分页和物理分页的区别是什么?

1. 定义：
   - 逻辑分页（也称为应用分页）是在应用程序层面进行分页处理，通过查询结果集的部分来实现分页。在查询时获取所有数据，然后在应用程序中根据页码和每页记录数对结果进行截取，返回指定页的数据。
   - 物理分页（也称为数据库分页）是在数据库层面进行分页处理，通过在SQL语句中指定起始行和查询行数来实现分页。在查询时只获取指定页的数据，减少了数据的传输和处理量。
2. 执行方式：
   - 逻辑分页是先获取全部数据，然后在应用程序中进行截取和处理，因此对于大量数据的分页查询可能会占用较多的内存和计算资源。
   - 物理分页是通过数据库的分页机制，在查询时只获取指定页的数据，减少了数据的传输和处理量，对于大数据量的分页查询更为高效。
3. 数据库压力：
   - 逻辑分页会将全部数据加载到应用程序中进行处理，对数据库的压力相对较小。
   - 物理分页只获取指定页的数据，减少了数据传输和处理量，对数据库的压力较大数据量分页查询更加合适。
4. 使用场景：
   - 逻辑分页适用于数据量较小的情况，或者需要对查询结果进行额外的处理和计算的场景。
   - 物理分页适用于大数据量的分页查询，能够减少数据传输和处理的开销，提高查询效率。

#### 3.4 常见标签?

1. `<select>`：用于定义查询语句。
2. `<insert>`：用于定义插入语句。
3. `<update>`：用于定义更新语句。
4. `<delete>`：用于定义删除语句。
5. `<resultMap>`：用于定义结果映射关系，将查询结果映射到Java对象。
6. `<result>`：用于定义单个属性的映射关系。
7. `<if>`：用于条件判断，根据条件动态拼接SQL语句。
8. `<where>`：用于生成动态的WHERE子句。
9. `<foreach>`：用于遍历集合或数组，并将元素应用于SQL语句中。
10. `<choose>`、`<when>`、`<otherwise>`：用于实现类似于Java中的switch-case语句的逻辑。
11. `<include>`：用于包含其他SQL片段，可以重复使用已定义的SQL片段。
12. `<trim>`：用于修剪SQL语句，删除开头或结尾的多余空格。
13. `<set>`：用于生成动态的SET子句。
14. `<bind>`：用于将一个参数绑定到一个变量上。
15. `<sql>`：用于定义可重用的SQL片段。
16. `<group>`：用于将多个SQL语句分组，并将它们作为一个单元进行操作。

#### 3.5 9种动态sql标签

1. `<if>`：条件判断标签，根据条件动态拼接SQL语句。

   ```xml
   <select id="getUserList" resultType="User">
     SELECT * FROM users
     <where>
       <if test="name != null">
         AND name = #{name}
       </if>
       <if test="age != null">
         AND age = #{age}
       </if>
     </where>
   </select>
   
   ```

2. `<choose>`、`<when>`、`<otherwise>`：类似于Java中的switch-case语句的逻辑。 

   ```xml
   <select id="getUserList" resultType="User">
     SELECT * FROM users
     <where>
       <choose>
         <when test="name != null">
           AND name = #{name}
         </when>
         <when test="age != null">
           AND age = #{age}
         </when>
         <otherwise>
           AND status = 1
         </otherwise>
       </choose>
     </where>
   </select>
   
   ```

3. `<trim>`：修剪SQL语句，删除开头或结尾的多余空格。

   ```xml
   <update id="updateUser" parameterType="User">
     UPDATE users
     <set>
       <trim suffixOverrides=",">
         <if test="name != null">
           name = #{name},
         </if>
         <if test="age != null">
           age = #{age},
         </if>
       </trim>
     </set>
     WHERE id = #{id}
   </update>
   
   ```

4. `<where>`：生成动态的WHERE子句，自动添加WHERE关键字。

   ```xml
   <select id="getUserList" resultType="User">
     SELECT * FROM users
     <where>
       <if test="name != null">
         AND name = #{name}
       </if>
       <if test="age != null">
         AND age = #{age}
       </if>
     </where>
   </select>
   
   ```

5. `<set>`：生成动态的SET子句，用于更新语句中的动态赋值。

   ```xml
   <update id="updateUser" parameterType="User">
     UPDATE users
     <set>
       <if test="name != null">
         name = #{name},
       </if>
       <if test="age != null">
         age = #{age},
       </if>
     </set>
     WHERE id = #{id}
   </update>
   
   ```

6. `<foreach>`：用于遍历集合或数组，并将元素应用于SQL语句中。

   ```xml
   <select id="getUserList" resultType="User">
     SELECT * FROM users
     WHERE id IN
     <foreach collection="ids" item="id" open="(" close=")" separator=",">
       #{id}
     </foreach>
   </select>
   
   ```

7. `<bind>`：将一个参数绑定到一个变量上，可以在后续的标签中使用该变量。

   ```xml
   <select id="getUserList" resultType="User">
     <bind name="searchName" value="'%' + name + '%'"/>
     SELECT * FROM users
     WHERE name LIKE #{searchName}
   </select>
   
   ```

8. `<sql>`：定义可重用的SQL片段，可以在其他语句中使用。

   ```xml
   <sql id="commonColumns">
     id, name, age
   </sql>
   
   <select id="getUserList" resultType="User">
     SELECT <include refid="commonColumns"/> FROM users
   </select>
   ```

9. `<include>`：包含其他SQL片段，可以重复使用已定义的SQL片段。

   ```xml
   <sql id="commonConditions">
     WHERE status = 1
   </sql>
   
   <select id="getUserList" resultType="User">
     SELECT * FROM users
     <include refid="commonConditions"/>
   </select>
   ```

#### 3.6 不同的xml映射文件，id是否可以重复?

在MyBatis中，每个XML映射文件对应一个命名空间（namespace），并且每个SQL语句都需要使用一个唯一的ID来标识。因此，同一个XML映射文件中的ID必须是唯一的，不允许重复。

如果在同一个XML映射文件中出现了相同的ID，会导致MyBatis解析时发生冲突，无法确定具体使用哪个SQL语句，从而抛出异常。

```xml
<!-- UserMapper.xml -->

<mapper namespace="com.example.UserMapper">
  
  <select id="getUserById" resultType="User">
    SELECT * FROM users WHERE id = #{id}
  </select>
  
  <insert id="insertUser" parameterType="User">
    INSERT INTO users (name, age) VALUES (#{name}, #{age})
  </insert>
  
</mapper>
<!--在上述示例中，getUserById和insertUser是两个不同的ID，用于标识不同的SQL语句。如果这两个ID重复，就会引发异常。因此，在编写MyBatis的XML映射文件时，需要确保每个ID都是唯一的。 -->
```



 

