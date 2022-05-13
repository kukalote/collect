# Java 注释



| 注释 | 使用范围     |  含义    |
| ----------- | ---- | ---- |
| `@Override`            |   | 父类方法覆写 |
| `@Transient`            | DAO-persistence | 返回一个“虚拟”的属性(经过转换后的属性字段) |
| `@MappedSuperclass` |      | 用于继承 |
| `@PrePersist` | DAO-persistence | 持久化到数据库之前（即执行INSERT语句），Hibernate会先执行该方法 |
| `@Entity` | DAO-persistence | 当前类为实体 |
| `@PersistenceContext` | DAO-hibernate | 注入一个`EntityManager`对象 |
| `@WebServlet` | Servlet | Servlet说明自己能处理的路径 |
| `@GetMapping` | Servlet | 说明Get请求路径 |
| `@PostMapping` | Servlet | 说明Post请求路径 |
| `@WebFilter` | Filter | 该Filter需要过滤的URL |
|`@WebListener`|Listener|特定接口的类会被Web服务器自动初始化|
|`@EnableWebMvc`|Spring|编写正常的`AppConfig`后，只需加上`@EnableWebMvc`注解，就“激活”了Spring MVC|
|`@Autowired`|注入|自动注入|
|`@ExceptionHandler`|Spring MVC|注解的异常处理方法|
|`@CrossOrigin`|处理CORS|可以在`@RestController`的class级别或方法级别定义一个`@CrossOrigin`|
|`@SpringBootApplication`|Spring Boot|该注解实际上又包含了 `@SpringBootConfiguration  @Configuration`   `@EnableAutoConfiguration  @AutoConfigurationPackage`   `@ComponentScan`|
|`@ConditionalOnProperty`|Condition|如果有指定的配置，条件生效；|
|`@ConditionalOnBean`|Condition|如果有指定的Bean，条件生效；|
|`@ConditionalOnMissingBean`|Condition|如果没有指定的Bean，条件生效；|
|`@ConditionalOnMissingClass`|Condition|如果没有指定的Class，条件生效；|
|`@ConditionalOnWebApplication`|Condition|在Web环境中条件生效；|
|`@ConditionalOnExpression`|Condition|根据表达式判断条件是否生效|
|`@Configuration`|Config|任何一个标注了 @Configuration 的 Java 类定义都是一个 JavaConfig 配置类。读取配置文件 application.yml|
|`@ConfigurationProperties`|Config|可以非常方便地把一段配置加载到一个Bean中|
|`@EnableAutoConfiguration`|IoC 容器 - @import|借助 @Import 的帮助，将所有符合自动配置条件的 bean 定义加载到 IoC 容器。启动自动配置，`@EnableAutoConfiguration(exclude = DataSourceAutoConfiguration.class)`但排除指定的自动配置:|
|`@Import`|IoC 容器|在 XML 形式的配置中，我们通过 `<import resource="XXX.xml"/>` 的形式将多个分开的容器配置合到一个配置中，在 JavaConfig 形式的配置中，我们则使用 @Import 这个 Annotation 完成同样目的|
|`@Bean`|IoC 容器|任何一个标注了 @Bean 的方法，其返回值将作为一个 bean 定义注册到 Spring 的 IoC 容器，方法名将默认成为该 bean 定义的 id。|
|`@PropertySource`|IoC 容器|@PropertySource 用于从某些地方加载 *.properties 文件内容，并将其中的属性加载到 IoC 容器中|
|`@ComponentScan`|IoC 容器|对应 XML 配置形式中的 <context：component-scan> 元素，用于配合一些元信息 Java Annotation，比如 @Component 和 @Repository 等，将标注了这些元信息 Annotation 的 bean 定义类批量采集到 Spring 的 IoC 容器中。|
|`@EnableScheduling`|IoC 容器 - @import|通过 @Import 将 Spring 调度框架相关的 bean 定义都加载到 IoC 容器|
|`@EnableMBeanExport`|IoC 容器 - @import|通过 @Import 将 JMX 相关的 bean 定义加载到 IoC 容器。|
|`@ControllerAdvice`|Exception|定义统一的异常处理类，而不是在每个Controller中逐个定义|
|`@ExceptionHandler`|Exception|用来定义函数针对的异常类型，最后将Exception对象和请求URL映射到`error.html`中|
|`@Entity`|Hibernate|标志着这个类为一个实体 bean，所以它必须含有一个没有参数的构造函数并且在可保护范围是可见的|
|`@table`|Hibernate|注释允许您明确表的详细信息保证实体在数据库中持续存在。|
|`@Id`|Hibernate|每一个实体 bean 都有一个主键，你在类中可以用 **@Id** 来进行注释。主键可以是一个字段或者是多个字段的组合，这取决于你的表的结构。|
|`@GeneratedValue`|Hibernate|默认情况下，@Id 注释将自动确定最合适的主键生成策略，但是你可以通过使用 **@GeneratedValue** 注释来覆盖掉它。|
|`@Column`|Hibernate|注释用于指定某一列与某一个字段或是属性映射的细节信息。|
||||
||||
||||
||||

