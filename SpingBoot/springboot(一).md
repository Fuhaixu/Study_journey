# SpringBoot

B站视频导学链接<https://www.bilibili.com/video/av38657363?from=search&seid=17759923419767264330>

### 一、springboot入门

- 简化spring应用开发，约定大于配置
- 使用嵌入式的Servlet容器，应用无需打成WAR包

- starters自动依赖和版本控制

- 大量的自动配置，简化开发，也可修改默认值

- 无需配置XML，无代码生成，开箱即用

> SpringBoot是J2EE开发的一站式解决方案



#### 1、微服务

微服务：架构风格

一个应用应该是一组小型服务；可以通过http的方式进行沟通；

每一个功能元素最终都是一个服务

#### 2、导入的依赖

```xml
<dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

springBoot将所有的功能场景抽取出来，做成一个个的starters启动器，

只需要在项目里面引入这些starter相关的依赖

#### 3、SpringBoot主程序类，主入口类

```java
/*SpringBootApplication
* 主程序，说明这是个Springboot应用
* */

@SpringBootApplication
public class HelloMainApplication {
    public static void main(String[] args){

        //Spring应用启动起来
        SpringApplication.run(HelloMainApplication.class,args);
    }
}
```

@SpringBootApplication:

SpringBoot应用标注在某个类上说明这个类是SpringBoot的主配置类，

SpringBoot就应该运行这个类的main方法来启动SpringBoot应用

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)})
public @interface SpringBootApplication {
   // ……
}
```

@SpringBootConfiguration:  SpringBoot的配置类

标注在某个类上，表示这个是SpringBoot的配置类；

@Configuration配置类上来标注这个注解：

​	配置类-----配置文件；配置类也是容器中的组件@Component



@EnableAutoConfiguration：开启自动配置

以前需要配置的东西SpringBoot帮助我们配置，**@EnableAutoConfiguration**告诉SpringBoot开启自动配置功能；

```java
@AutoConfigurationPackage
@Import({EnableAutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
    String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";

    Class<?>[] exclude() default {};

    String[] excludeName() default {};
}
```

**@AutoConfigurationPackage**自动配置包

​			@Import({Registrar.class})

Spring的底层注解@import，给容器中导入一个组件，导入的组件由**@AutoConfigurationPackage**

将@SpringBootApplication标注的类所在的包扫描所有类

@Import(EnableAutoConfigurationImportSelector)

将所有需要导入的组件以全类名的方式返回，这些组件被SpringBoot读入







### 二、springboot配置

IDE会联网下载需要的模块

**resources文件夹中的目录结构**

static：保存所有的静态资源，js，css，images

template：保存所有的模板资源（Spring Boot默认jar包使用嵌入式的Tomcat，默认不支持JSP页面，可以使用模板引擎freemarker，thymeleaf）

SpringBoot使用一个全局的配置文件，配置文件名是固定的

application.properties

application.yml

配置文件的作用：修改SPringBoot自动配置的默认值

> YAML(YAML Aln't Markup Language)
>
> YAML A Markup Language 是一个标记语言
>
> YAML: isn't Markup  



#### 2.1.YAML的基本语法

k：V:表示一对键值对（空格必须有）

用空格的缩进控制层级关系，只要左对齐的一列数据，都是同一个层级的

```
server:
	port:8081
	path:/hello
```

属性和值也是大小写敏感



#### 2.2  yaml值的写法

字面量：普通的值（数字，字符串，布尔）

k：v字面直接来写

字符串默认不用加上单引号或者双引号

“”双引号：不会转义字符串里面的特殊字符：特殊字符会作为本身想表示的意思

name:“zhangsan \n list” 输出

‘’：单引号：会转义特殊字符，特殊字符最终只是一个普通的字符串数据

name:"zhangsan \n list" 输出: zhangsan \n list

行内写法

```yml
friends: {name: zhangsan,age: 10}
```

数组

```yaml
pets:
- cat
- dog 
- pig
```



#### 2.3@Value获取值和@ConfigurationProperties获取值比较

| Feature                       | @ConfigurationProperties | @Value   |
| ----------------------------- | ------------------------ | -------- |
| 功能                          | 批量注入文件中的属性     | 一一指定 |
| 松散语法绑定(Relaxed Binding) | 支持                     | 不支持   |
| spEL                          | 不支持                   | 支持     |
| JSR303数据校验                | 支持                     | 不支持   |
| 复杂类型封装                  | 支持                     | 不支持   |

配置文件yml还是properties他们都能获得到值



#### 2.4配置文件注入值校验

@ImportResource("")

SpringBoot推荐的给容器中添加组件的方式

1、配置类=========Spring配置文件

不来编写Spring的配置文件，推荐全注解的方式

@Bean将方法的返回值添加到容器中

Bean的默认id是方法名



#### 2.5 Profile

##### 2.5.1多profile文件

在主配置文件编写的时候，文件名可以是  application-(profile).properties/yml

默认使用application.properties的配置



2、yml支持多文档块方式

3、激活制定profile

​	1、在配置文件中激活

```
spring.profiles.active=dev
```

命令行激活

java  -jar 

--spring.profiles.active=dev

虚拟机参数：

-Dspring.profiles.active=dev



##### 2.5.2配置文件的优先级

-file:/config

-file:/

-classpath:/config

-classpath:/

互补配置

server.context-path=/

通过spring.config.location=

项目打包后，我们可以通过命令行参数的形式，启动项目的时候来

#### 2.7自动配置原理

配置文件可以写哪些

**自动配置原理**:

1)、springBoot启动的时候加载主配置类，开启了自动配置功能，@EnableAutoConfiguration作用：

利用@EnableAutoConfigurationImportSelector给容器中导入一些组件，可以插件selectImports()方法的内容：

> List<String> configures=

所有jar包下面类路径下META-INF/spring.factories 

把扫描到的这些文件的内容包装成properties对象

从properties中获取到的EnableAutoConfiguration.class类（类名）对应的值，然后把它们添加到容器中



3）、每一个自动配置类进行自动配置

4）、以HttpEncodingAutoConfiguration为例解释自动配置原理：

@ConditionalOnWebApplication  spring底层@Conditional注解，根据不同的条件，如果满足制定的条件，整个配置类里面的配置就会生效，判断当前应用是否是web应用，如果是，当前配置类生效

@ConditionalOnClass(CharacterEncodingFilter.class)判断当前项目有没有这个类CharacterEncodingFilter，springMVC中进行乱码解决的问题

@ConditionalOnProperty(prefix="spring.http.encoding",value="enabled",matchIfMissing=true)

//判断文件中是否存在某个配置，如果不存在，判断也是成立的

即使我们配置文件中不配置spring.http.encoding，也是默认生效的；



**精髓**：

1、springBoot启动会加载大量的自动配置类

2、我们看我们需要的功能有没有SPring配置写好的自动配置类

3、我们再来看这个自动配置类找那个到底配置了哪些组件，只要我们要用的组件有，我们就不需要再来配置了

4、给容器中自动配置类添加组件的时候，会从properties类中获取某些属性，我们就可以在配置文件中制定这些属性的值

xxxAutoCOnfiguration:自动配置类

给容器中添加组件

xxxProperties封装配置文件中的相关属性



debug=true开启

开启SPringBoot的debug模式，通过启动debug模式，让控制台打印自动配置报告，这样我们就可以方便知道哪些配置类生效





### 三、springboot与日志



| 日志门面                                    | 日志实现 |
| ------------------------------------------- | -------- |
| ~~JCL  jakarta commons logging~~   2014更新 |     Log4J     |
| SLF4J(Simple Logging Facade for java)       |      JUL(java.util.logging)    |
| ~~JBoss-logging~~                           |     Log4J2      |
|                       |     LogBack      |


**最终的解决方案**
日志门面：SLF4J
日志实现：LogBack

原因
SpringBoot：底层是Spring框架，SPring框架默认是用JCL

**SpringBoot选用SLF4J和LogBack**

SLF4J使用

如何在系统中使用SLF4J

以后开发的是偶，日志记录方法的使用，不应该来直接调用日志的实现类，而是使用日志抽象层

#### 2、SLF4J使用

1、如何在系统中使用SLF4J

以后开发的时候，日志记录方法的使用，不应该来直接调用日志的实现类，而是调用日志抽象层里面的方法；

给系统里面导入slf4J的jar和logback的实现jar

2、每一个日志的实现框架都有自己的配置文件，使用SLF4J以后，配置文件还是做成日志实现框架自己本身的配置文件



#### 3、遗留问题

a(slf4J+logback):Spring(commons-logging)、Hibernate(Jboss-logging)、mybatis、xxx

统一日志记录，即使是别的框架和我一起统一使用slf4J进行输出

如何让系统中所有的日志统一到slf4J

1、让系统中其他日志框架先排除出去；

2、用中间包来替换原有的日志框架；

3、我们导入slf4J其他的实现



总结：

1）、springBoot底层也是使用slf4J+logback的方式进行日志记录

2）、springBoot也把其他的日志都替换成了slf4J；

3）、如果要引入其他框架，一定要把这个框架的默认日志依赖移除？



springBoot默认使用的是info级别的日志

```properties
logging.level.com.springboot02=trace
#logging.path=指定目录
logging.path=/spring/log
#使用spring.log作为默认文件
logging.file=springboot.log
#在控制台输出的日志格式
logging.pattern.console=
#指定文件中日志输出的格式
logging.pattern.file=
```

SpringBoot修改日志的默认配置




