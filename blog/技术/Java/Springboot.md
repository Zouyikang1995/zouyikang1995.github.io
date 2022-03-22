# Springboot
## Java发展史   
* 早期阶段：servlet、jsp(借助了微软的asp)
  额外知识：Java与C#相互借鉴，Java借鉴C#的语法，C#借鉴Java社区发展的一些编程思想。    
  C#语法优秀源于Anders Hejlsberg的设计，他创造了C#、Pascal(dephi)、TypeScript。           
* 第二阶段：structs2, spring：IOC、AOP, Hibernate：ORM, 传说中的`ssh`。   
  额外知识：微软设计了类似的ORM, EntityFramework
  structs的缺陷在于以action为入口，springmvc以方法为入口。     
* 第三阶段：springmvc, spring:IOC、AOP, mybatis, 所谓的`ssm`。       
* 第四阶段：springboot(基于springmvc)。    
  优势: 1. 约定大于配置。配置包含设置文件(路径，数据库接口等)等简单的配置和面向接口/抽象(Interface)的自动装配。     
        2. 集成第三方库，在maven中输入第三方库即可。       

## JDK的安装    
通常选择版本：JDK8，JDK11，JDK13。    
JDK8具有里程碑形式，新增了lambda表达式，stream流式编程等新特性。而JDK11，JDK13等缺乏有吸引力的新特性。    
步骤：
1. 下载[JDK安装包](https://www.oracle.com/java/technologies/downloads/)，一键安装。   
2. 设置环境变量。
   `变量名`：JAVA_HOME    `变量值`：C:\Program Files (x86)\Java\jdk1.8.0_91        // 要根据自己的实际路径配置    
   `变量名`：`CLASSPATH`  `变量值`：`.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;`         //记得前面有个"."     
   `变量名`：`Path`       `变量值`：%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;
3. 测试是否安装成功，终端中输入：
   java -version   
   javac   

## IOC与DI
IOC与DI的优势？(为什么IOC和DI在Java中这么重要，而在Python、JavaScript等动态语言中几乎不受待见。)   

### 参考资料
[Martin Fowler:Inversion of Control Containers and the Dependency Injection pattern](https://www.martinfowler.com/articles/injection.html)     
[Martin Fowler关于IOC和DI的文章（中文版）](https://www.cnblogs.com/ChrisMurphy/p/5054429.html)               
[稀土掘金：浅析Spring的IoC和DI](https://juejin.cn/post/6844903662259535880)       
[CSDN：对IOC和DI的通俗理解](https://blog.csdn.net/fuzhongmin05/article/details/55802816)    
[简书：简单认知IOC与DI](https://www.jianshu.com/p/21a270394315)     
[浅谈IOC与DI](https://www.jianshu.com/p/42748a82f10e)     

## SpringBoot版本号   
例子：`2.1.6.RELEASE` 
`2` - `主版本`，短期内不会变更，发生重大变化时，有时甚至不兼容之前的版本。   
`1` - `次版本`，发布新特性，通常保证兼容。  
`6` - `增量版本`，通常是bug修复，通常保证兼容。
* `RELEASE` - 发布版本/里程碑版本，可能有RC,Alpha,Beta,GA, 描述版本的发布计划或者是版本的发布状态。      
* `Alpha`：内部测试     
* `Beta`：准备公开测试(可能不稳定)      
* `SNAPSHOT`: 快照版本，比较稳定，还在持续跟进   
* `RELEASE/GA(General Availability)`：一般为稳定版本     

## SpringBoot项目创建
创建方法：
1. 官网[Spring initializr](https://start.spring.io/)创建。      
2. 使用Idea创建。
   `New Project` → `Spring Initializr`
`注意`：dependencies中要选中`spring web`。

## SpringBoot层级结构
常见层级结构如下：
1. `api`:写`Controller`，内部通常以Controller结尾。
2.

### Controller层
1. 加上注解交由SpringBoot控制，使其可以控制路由。 
   注解：`@Controller`  
2. 类方法上面添加注解注明`路由`、`路由种类`及`返回形式`。
   * 路由及种类：`@GetMapping("/test")`,`@PostMapping("/test")`,`@putMapping`,`@DeleteMapping`,`@DeleteMapping`。                 
   * 路由的全注解方式：`@RequestMapping(value = "/test", method = {RequestMethod.GET, RequestMethod.POST})`,通过method来限定路由方法，可以添加一个或者多个。    
   * 路由的全注解方式可以在类上标明，以简化重复路由的书写。     
   * 返回形式：`@ResponseBody`(将返回字符串写入到HttpServlet的response当中),可以将@ResponseBody注解统一写到类上面。  
   * 可以在类上添加`@RestController`注解代替`@Controller`注解和`@ResponseBody`注解。       
     `@RestController` = `@Controller` + `@ResponseBody`      
```java
示例：
@RestController
@RequestMapping("/banner")
public class BannerController {

    @GetMapping("/name/{name}")
    public Banner getByName(@PathVariable @NotBlank String name) {
        Banner banner = bannerService.getByName(name);
        if (null == banner) {
            throw new NotFoundException(30005);
        }
        return banner;
    }

    @PostMapping("/test/{id}")
    public PersonDTO test(@PathVariable(name = "id") @Range(min = 1, max = 10) Integer id, @RequestParam String name,
                          @RequestBody @Validated PersonDTO person) {
        PersonDTO dto = PersonDTO.builder().
                name(person.getName()).
                age(person.getAge()).
                password1(person.getPassword1()).
                password2(person.getPassword2()).
                build();
        return dto;
    }
}
```
3. 路由的构成形式。
   一般模式：`host:port/version/资源名/接口名` 
```json
例子：localhost:8080/v1/banner/test
```    
其中，版本号可以有以下几种处理方式。
* 以键值对形式写入到`header`当中。      
* 在url后以`url?version=1`的形式标明。    
* 在资源名前面指出,如以上示例。            

## Java编程思想深度理论知识
1. 软件工程方法论,目的是方便项目的`维护`和`迭代`。
2. 好的代码应该具有的特点：
   `不罗嗦`,`自描述性`,`可维护性`
3. 软件工程的原则：`开闭原则(OCP Open Closed principle)`
   对扩展是开放的，对修改是封闭的。
4. 实现开闭原则的关键:`面向抽象编程`. 
   利用interface,abstract.
采用实体类的时候需要新建该对象,但我们的目的并不是获取该对象,而是想要调用该对象的方法,因而使用interface就显得十分重要了。因此,在实体类中不要过分依赖于某个实体类,尽量面向接口编程。       
演化过程: `interface` -> `设计模式:工厂模式` -> `IOC/DI`    
目的： => `面向抽象` => `OCP` => `可维护的代码`
理解:
1. 关于interface: 单纯的interface可以统一方法的调用,但是它不能统一对象的实例化.   
2. 面向对象的两大主要内容: `实例化对象`,`调用方法(完成业务逻辑)`      
3. `表象`：只有一段代码中没有new的出现,才能保持代码的相对稳定,才能你逐步实现OCP。   
4. `实质`:一段代码如果要保持稳定,就不应该负责对象的实例化。     
5. `结论`：对象实例化是不可能消除的。  
6. `解决方法`: 因此需要把对象实例化的过程,转移到其他的代码片段里。      
7. `理由`：代码中总是会存在不稳定，隔离这些不稳定，保证其它的代码是稳定的。      
8. `原因`：变化造成了代码的不稳定。    
9. 配置文件属于系统外部，不属于代码本身。    

## Spring与SpringBoot
1. SSM = Spring + Spring MVC + MyBatis (Spring和Spring MVC被包含在Spring Framework当中。)   
2. Spring Framework是SpringBoot的基础，SpringBoot是Spring Framework的应用。  
3. SpringBoot的核心优势：`自动配置`。   

## SpringBoot的目的及意义
IOC实现：创建容器，加入容器，注入。其中，由于SpirngBoot作为一个容器，需要考虑到各种场景，具备灵活性。      
SpringBoot的总体目的： 
抽象意义：控制权交给用户   
具体意义：实现灵活的OCP      

## SpringBoot注入
将对象注入到容器的方式：
1. XML方式    
2. 注解模式  
stereotype annotations模式注解: 
* @Component 基础注解，组件/类/bean     
* @Service 
* @Controller
* @Repository
* @Configuration 
`IOC对象实例化注入时间`：在SpringBoot启动时实例化，即自动/提前实例化。可以通过@Lazy注解实现延迟实例化。      
`加入IOC容器的扫描对象`：通过`@ComponentScan`注解实现，默认扫描Application.java类的同级和下级目录。      
@Autowired注入方式：
1. 属性(成员变量)注入。（不规范，属性为private，对私有属性注入不推荐。）    
2. 构造函数注入。（推荐）   
3. setter注入。     

注入优先级：
1. by type(默认注入方式)，根据类型注入，如果同一个接口有多个实现的话，就会出错。     
2. by name，按照字段的名称来寻找实现类。  
3. 主动注入方式，加上注解@Qualifier    

## 面向对象中变化的解决方案
1. `策略模式`: 制定一个Interface，然后用多个类实现同一个interface。    
2. 一个类，利用属性来解决变化。  

### 策略模式的实现方式
1. by name：通过类的名称来切换注入类。      
2. 通过添加@Qualifier注解来指定注入类。      
3. 有选择的只注入一个bean，注释掉其它实现类上面的@Component注解。    
4. 使用@Primary注解来指定优先注入的bean。       

### 条件注解  
1. 创建自定义条件注解方法：通过@Conditional注解+实现Condition接口。   
例子：创建HeroConfiguration类加上@Configuration和@Bean注解，然后创建DianaCondition实现Condition方法。  
```java
@Configuration
public class HeroConfiguration {
    @Bean
    @Conditional(DianaCondition.class)
    public ISkill diana() {
        return new Diana("Diana", 18);
    }
}

public class DianaCondition implements Condition {
    @Override
    public boolean matches(ConditionContext conditionContext, AnnotatedTypeMetadata annotatedTypeMetadata) {
        String name = conditionContext.getEnvironment().getProperty("hero.condition");
        return "diana".equalsIgnoreCase(name);
    }
}

#application.properties
hero.condition = Diana
```
2. 内置的成品条件注解
* `@ConditionOnProperty`
   value -> 配置变量的key    
   havingValue -> 配置变量的值    
   matchIfMissing -> 如果配置文件中未找到相关配置则为true   
```java
@Bean
    @ConditionalOnProperty(value = "hero.condition", havingValue = "diana", matchIfMissing = true)
    public ISkill diana() {
        return new Diana("Diana", 18);
    }
```
* `@ConditionalOnBean` 当SpringIoc容器内存在指定Bean的条件    
 `@ConditionalOnMissingBean`与之含义相反
```java
@Bean
    @ConditionalOnBean(name = "mysql")
    public ISkill diana() {
        return new Diana("Diana", 18);
    }
```
@ConditionalOnClass    
@ConditionalOnExpression 基于SpEL表达式作为判断条件    
@ConditionalOnJava 基于JVM版本作为判断条件    
@ConditionalOnJndi 在JNDI存在时查找指定的位置    
@ConditionalOnMissingClass 当SpringIoc容器内不存在指定class的条件    
@ConditionalOnWebApplication 当前项目不是Web项目的条件   
@ConditionalOnProperty 指定的属性是否有指定的值   
@ConditionalOnResource 类路径是否有指定的值   
@ConditionalOnSingleCandidate 当指定Bean在SpringIoc容器内只有一个，或者虽然有多个但是指定首选的bean   
@ConditionalOnWebApplication 当前项目是Web项目的条件   

### 配置@Configuration
1. 为什么Spring偏爱配置    
OCP原则对应的是变化，变化需要隔离/反映到配置文件当中。其中，xml很好地隔离和反映了变化。 
2. 为什么要将变化隔离到配置文件。  
   * 配置文件具有集中性。   
   * 配置文件比较清晰，配置文件里面没有业务逻辑。     
3. 配置种类：
   * 常规配置 key：value    
   * xml配置，以类/对象形式进行出现。       
4. 利用@Configuration+@Bean的方式，可以灵活地将bean注入到IOC容器中。     
```java
例子：
@Configuration
public class DatabaseConfiguration{
   @Value("${mysql.ip}")
   private String ip;

   @Value("${mysql.port}")
   private Integer port;

   @Bean
   public IConnect mysql(){
      return new MySQL(this.ip, this.port);
   }
}
```
其中，有几点值得说明：
   * @Configuration和@Bean并不是一定要用在一起，可以根据情况灵活地选择应用。   
   * 利用@Component+@Value可以实现以上相同的目标，但是当IConnect接口的实现有多个时，Spring无法决定注入哪个实例。因而，利用@Configuration和@Bean解决的问题是，既包含常规配置(key,value)的配置模式，又包含以类/对象的配置模式而组成的混合配置模式。往往，当一个接口有多个实现时，无法通过@Component来实现接口灵活的自动注入，这是@Configuration的应用场景，从而实现了OCP的编程特性。              
5. @Configuration编程模式：既能灵活地在配置文件中修改bean的属性，又能将bean给注入到IOC容器中，从而实现了OCP原则的一种编程模式。    

### 自动装配/配置    
1. 原理是什么     
利用@Import和实现ImportSelector的类来导入第三方库。  
2. 为什么要有自动装配      
类似于@Component和@Configuration注解的用法，用来加载第三方SDK。同时，Spring框架与其它框架的不同之处在于IOC容器，自动装配解决的本质问题是将第三方的JDK加载到Spring的IOC容器当中。             

### SPI机制/思想
全名：Service Provider Interface   
本质：应对变化所做出的一种机制。    
做法：系统中有不同模块，相对应有不同的解决方案。    
调用方可以调用模块中的标准服务接口，方案可以拥有多种。方案1，方案2，方案3等等。   
基本原理：基于interface + 策略模式 + 配置文件   
不同点：与@Primary和@条件注解的不同之处在于，SPI注重于解决整体解决方案的变化，而@Primary和@条件注解着眼于具体的类/粒度小的变化。             



## 框架机制
与核心机制中包含的深度理论不同，框架机制是用来开发业务的。    

### 异常反馈
作用：用来反馈给前端/客户端    
存在种类：资源不存在/参数错误/内部错误等等。   

### 异常分类    
#### 语法层面分类   
1. 异常基类：Throwable
2. 引申的两种异常：
   Error：错误(操作系统上错误或者JVM的错误，发生错误后应用程序通常不能启动，代码通常不能处理)    
   Exception：异常(通常用代码处理，例如空指针异常)   
3. Exception异常的分类：
   CheckedException:(必须在程序当中进行处理，如果不处理则在编译过程中无法通过)   
   RuntimeException:运行时异常，不一定在编译阶段就能发现(不强制要求处理)      
4. 对于web开发来讲，如果我们实现了全局异常处理，用Exception和RunTimeException区别不大，建议使用RuntimeException。     
5. RuntimeException和CheckedException的使用场景。
   RuntimeException：对于一个异常来说我们无能为力，例如在数据库中查询一条数据没有记录。严格意义上来讲不是一个bug。             
   CheckException：如果对一个异常，我们可以处理。例如调用方法找不到，读取文件找不到等等。是一个真正的bug。       

#### 已知/未知层面
未知异常：对于前端开发者和用户，都是无意义的。服务端开发者代码逻辑有问题。模糊处理，服务器异常返回给前端。          

## 参数校验
重要性：   
1. 规范的参数校验可以提高前后端的开发效率。   
2. 对于保护Web项目的机密数据十分重要。   
注意事项：不能对参数不进行校验或者将校验参数写在控制器Controller中。    

### URL中的参数(GET请求)
* 方式一：URL路径当中的参数  
  例如：`@GettingMapping("/test/{id}")`         
```java
@GetMapping("/test/{idName}")
    public String test(@PathVariable(name="idName") Integer id) {
    }
```
通过pathVariable指定名字name，将idName与id进行关联。    

* 方式二：查询参数
  例如：`@GettingMapping("/test?parameter=xxx)`      
```java
@GetMapping("/test?name=xxx")
    public String test(@RequestParam String name) {
    }
```
### Request Body中的参数(POST请求)
使用@RequestBody注解，同时利用Map<String, Object>或者新建一个对象进行接收。   
* 方式一：(存在拆箱装箱的问题)  
```java
@Postmapping("/test")  
public String test(@RequestBody Map<String, Object> person){

}
```
* 方式二：(推荐)
新建PersonDTO对象，用来接收前端传来的person。   
```java
@Postmapping("/test")  
public String test(@RequestBody PersonDTO person){
   
}
```


### 全局异常处理   
UnifyResponse 统一错误响应  
```json
{
   code:10001,
   message:xxx,
   request:GET url
}
```

### SpringBoot中根据包名自动生成路由   
1. 创建AutoPrefixUrlMapping.java类继承`RequestMappinghandlerMapping`类。重写`getMappingForMethod`方法。    
```java
public class AutoPrefixUrlMapping extends RequestMappingHandlerMapping {

    @Value("${missyou.api-package}")
    private String apiPackagePath;

    @Override
    protected RequestMappingInfo getMappingForMethod(Method method, Class<?> handlerType) {
        RequestMappingInfo mappingInfo = super.getMappingForMethod(method, handlerType);
        if (null != mappingInfo) {
            String prefix = this.getPrefix(handlerType);
            return RequestMappingInfo.paths(prefix).build().combine(mappingInfo);
        }
        return mappingInfo;
    }

    private String getPrefix(Class<?> handlerType) {
        String packageName = handlerType.getPackageName();
        String dotPath = packageName.replaceAll(this.apiPackagePath, "");
        return dotPath.replace(".", "/");
    }
}
```

2. 创建AutoPrefixConfiguration.java类实现`webMvcRegistrations`接口，并通过`@Component`注解加入IOC容器。     
```java
@Component
public class AutoPrefixConfiguration implements WebMvcRegistrations {
    @Override
    public RequestMappingHandlerMapping getRequestMappingHandlerMapping() {
        return new AutoPrefixUrlMapping();
    }
}
```

### lombok的使用
* pom文件中加入dependency。
```java
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
```
#### 利用Lombok生成Getter和Setter
* 在entity类上面加上注解@Getter和@Setter
* @Data的区别：@Data除了Getter和Setter之外，还会重写equals，toString和hashCode方法。   
* 注意：final修饰的字段，@Data加上注解后会生成Getter方法，不会生成Setter方法。    
#### 利用Lombok生成构造器Constructor 
* 生成无参构造器：@NoArgsConstructor   
* 生成全部参数的构造器:@AllArgsConstructor   
* 对某个字段要求不为空：@NonNull
* 生成对要求字段的构造器：@RequiredArgsConstructor
```java
例子：
@Getter
@Setter
@AllArgsConstructor
@RequiredArgsConstructor
@NoArgsConstructor
public class PersonDTO{
    @NonNull
    private String name;
    private Integer age;
}
```
#### @Builder构造器模式
1. 类上面加上@Builder注解。
2. 类实例化时使用builder构造器。
```java
PersonDTO dto = PersonDTO.builder()
          .name("lucy")
          .age(18)
          .build();
```
* 注意：
1. 只在类上面加上@Builder注解后，不能使用Getter和Setter方法，不能用new来实例化类。
2. 针对情况1，在类上面加上@Setter方法也没有用，原因是@Builder会给类生成一个私有的无参构造函数。   
3. 要想既使用Builder方式，又想使用普通的构造方法，可以在类上再添加一个@NoArgsConstructor注解，生成一个无参构造函数，或者自己手动写一个无参的public构造函数。       
4. 使用@Builder的方法创建实体后，如果要将实体返回给前端，需要在类上面加上@Getter方法获取字段。     

#### JSR
定义：Java Specification Requests - Java提案规范         
* lombok是JSR-269规范的实现     
* 参数校验是JSR-303规范的实现：Bean Validation           
* JSR-303的一个实现：Hibernate-Validator
  常用注解：@Min，@Max，@Positive等
##### 常用注解   
1. 加上@Max，@Min等注解时要在类上面加上@Validated的注解。  
```java
@Validated
public class BannerController {
    @PostMapping("/test/{id}")
    public PersonDTO test(@PathVariable(name = "id") @Range(min = 1, max = 10) Integer id) {
    }
}
```
2. 在对自己编写的类进行参数校验时要在参数名前加上注解@Validated.   
BannerController.java
```java
@Validated
public class BannerController {
    @PostMapping("/test/{id}")
    public PersonDTO test(@PathVariable(name = "id") @Range(min = 1, max = 10) Integer id, @RequestBody @Validated PersonDTO person) {
    }
}
```
PersonDTO.java
```java
@Builder
@Getter
@PasswordEqual(min = 1)
public class PersonDTO {
    @NonNull
    @Length(min = 2, max = 10)
    private String name;
    private Integer age;
}
```
3. 如果在接收的对象中还有其它对象，需要在级联的对象上面打上@Valid的注解。  
```java
@Builder
@Getter
@PasswordEqual(min = 1)
public class PersonDTO {
    @NonNull
    @Length(min = 2, max = 10)
    private String name;
    private Integer age;

    @Valid
    private SchoolDTO schoolDTO;
}
```
4. @Valid和@Validated的区别。    
   * 两者都是用来作为参数校验的，某些情况下可以互换使用。      
   * 在开启验证时推荐使用@Validated，在级联时使用@Valid。     

##### 自定义注解
1. 新建注解@Annotation，需要配置@Retention，@Target，@Constraint等。      
各个注解的含义如下：
* @Documented - 生成JDK文档，非必须。
* @Retention - 注解的生命周期阶段。有RetentionPolicy.SOURCE, RetentionPolicy.RUNTIME, RetentionPolicy.CLASS三个阶段，一般可以选择RUNTIME。
* @Target - 标记该注解的作用对象，可以是类，属性，方法等。`@Target(value={TYPE,FIELD,METHOD,PARAMETER,CONSTRUCTOR,LOCAL_VARIABLE})`      
* @Constraint - 对应注解逻辑实现的类。
`自定义注解`：
```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE})
@Constraint(validatedBy = PasswordValidator.class)
public @interface PasswordEqual {
    int min() default 4;

    int max() default 6;

    String message() default "passwords are not equal";

    Class<?>[] groups() default {};

    Class<? extends Payload>[] payload() default {};
}
```
`注解逻辑实现类`：    
```java
public class PasswordValidator implements ConstraintValidator<PasswordEqual, PersonDTO> {
    private int min;
    private int max;

    @Override
    public void initialize(PasswordEqual constraintAnnotation) {
        this.min = constraintAnnotation.min();
        this.max = constraintAnnotation.max();
    }

    @Override
    public boolean isValid(PersonDTO personDTO, ConstraintValidatorContext constraintValidatorContext) {
        String password1 = personDTO.getPassword1();
        String password2 = personDTO.getPassword2();
        boolean match = password1.equals(password2);

        return match;
    }
}
```
自定义注解的使用：   
```java
@Builder
@Getter
@PasswordEqual(min = 1)
public class PersonDTO {
    @NonNull
    @Length(min = 2, max = 10)
    private String name;
    private Integer age;

    private String password1;
    private String password2;
}
```

## 项目分层与JPA技术
### 项目分层原则
1. 分层的目的：微服务出现之前，大型项目的每一层一般都是由不同的开发者或者团队完成的。目的在于分隔每层的开发，大家各司其职，互不打扰。归根结底，分层是对OCP原则更大层面的实践。      
* 注意：同样是分隔项目的开发，分层被称为水平分隔，而微服务被称为垂直分隔。    

### 如何创建数据表
1. 可视化管理工具(navicat, mysql workbench, php admin)。    
2. 手写sql语句。    
3. Model模型类(通过class类来映射数据表)。  
4. 数据库的配置，在`application.properties/yml`中。  
```yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/xxx?characterEncoding=utf-8&serverTimezone=GMT%2B8
    username: xxx
    password: xxx
  jpa:
    hibernate:
      ddl-auto: update
```   
其中，`ddl-auto`可以用来配置数据表的更新方式，可以是`none`, `update`, `create-drop`。  

### 数据库表与表之间关系  
`一对一`：理论上一般是一个表字段太多拆分为多个表，更多是现实存在的意义。创建一对一数据表时一般基于两方面考虑：
1. 提高查询效率。      
2. 从业务的角度，将不同字段归类。     
例如：人和身份证    

`一对多`：一张表对应多张表。
例如：班级和学生

`多对多`：多对多一般需要第三张表，两张表无法表示多对多的关系。    
例如：老师和学生     

### 数据库设计思想
* 数据库设计思考方式：
  1. 第一步：将表按照模型(实体)来思考，按照面向对象的原则，一个表就是一个对象。     
  2. 理解对象与对象之间的关系。(一对一，一对多，多对多)。      
  3. 细化工作，设计字段的限制，长度，唯一索引等。

* 从性能方面考虑数据库设计：
  1. 一张表中的记录不要超过5000万条。     
  2. 建立索引以优化表结构。
  3. 水平分表，将其拆分为多个表分摊数据。    
  4. 垂直分表，将字段拆分到多个表中。    

* 注意：数据库的优化更多不是体现在设计上，而是查询方式。另外，利用缓存去减少数据库的查询是解决性能最有效的方式。  

### 数据表创建
1. 利用JPA设计一对多关系的数据表。Banner对应多个BannerItem。
Banner.java   
```java
@Entity
@Getter
@Setter
@Where(clause = "delete_time is null")
public class Banner extends BaseEntity {
    @Id
    private Long id;
    private String name;
    private String description;
    private String title;
    private String img;

    @OneToMany(fetch = FetchType.LAZY)
    @JoinColumn(name = "bannerId")
    private List<BannerItem> items;
}
```
BannerItem.java
```java
@Entity
@Getter
@Setter
public class BannerItem extends BaseEntity {
    @Id
    private Long id;
    private String img;
    private String keyword;
    private short type;
    private String name;
    private Long bannerId;
}
```
* 其中，bannerId是BannerItem的外键，与Banner表中的id对应。     
* 可以用以下语句来设置主键自增长。 
```java
 @GeneratedValue(strategy = GenerationType.IDENTITY)   
```

### JPA查询数据库
1. BannerRepository仓储继承JpaRepository<>。传入泛型，<Banner, Long>前者是实体类型，后者是主键类型。查询方法根据Jpa查询语句的命名方法，可以通过Jpa省略书写实现类来实现查询方法。      
```java
@Repository
public interface BannerRepository extends JpaRepository<Banner, Long> {
    Banner findOneById(Long id);

    Banner findOneByName(String name);
}
```

2. 在Service层调用BannerRepository里面的方法。
```java
@Service
public class BannerServiceImpl implements BannerService {
    @Autowired
    private BannerRepository bannerRepository;

    public Banner getByName(String name) {
        return bannerRepository.findOneByName(name);
    }
}
```

3. 配置Jpa打印sql语句。
```java
spring:
  jpa:
    properties:
      hibernate:
        show_sql: true
        format_sql: true
```

4. 配置导航属性懒加载。   
```java
@OneToMany(fetch = FetchType.LAZY)
```

### Jpa导航关系配置
1. 双向一对多关系的配置。
   一方：关系被维护端
   多方：关系维护端
* 一方打上@OneToMany,多方打上@ManyToOne。
* 注明关联属性，@JoinColumn打在关系维护端（多方）。一方不必打上@JoinColumn。  
* 一方增加参数mappedBy，参数是多方导航属性。(这时候一方不能添加注解@JoinColumn)   
通过以上的设置，实际上一方就放弃了关系的维护，在实际操作中，数据库会优先保存一方，然后保存多方，这样避免了发送update语句。当然，也可以在一方和多方同时加上注解@JoinColumn，不采用mappedBy，这时候两方都会维护关系，保存对象的时候不可避免地会发送多余的update语句。     
`Banner.java`
```java
public class Banner extends BaseEntity {
    @Id
    private Long id;
    private String name;
    private String description;
    private String title;
    private String img;

    @OneToMany(mappedBy = "banner", fetch = FetchType.LAZY)
    // @JoinColumn(name = "bannerId")
    private List<BannerItem> items;
}
```
`BannerItem.java`
```java
public class BannerItem extends BaseEntity {
    @Id
    private Long id;
    private String img;
    private String keyword;
    private short type;
    // private Long bannerId;
    private String name;

    @ManyToOne
    @JoinColumn(name = "bannerId")
    private Banner banner;
}
```

2. 多对多关系。  
两个都需要加上注解@ManyToMany，关系维护方添加@JoinTable注解，里面指明joinColumns和inverseJoinColumns。关系被维护方加入mappedBy。    
`Theme.java`
```java
@Entity
public class Theme extends BaseEntity {
    @Id
    private Long id;
    private String title;
    private String description;

    @ManyToMany(fetch = FetchType.LAZY)
    @JoinTable(name = "theme_spc",joinColumns = @JoinColumn(name = "theme_id"),
                inverseJoinColumns = @JoinColumn(name = "spu_id"))
    private List<Spu> spuList;
}
```
`Spu.java`     
```java
@Entity
public class Spu extends BaseEntity {
    @Id
    private Long id;
    private String title;
    private String subtitle;

    @ManyToMany(mappedBy = "spuList")
    private List<Theme> themeList;
}
```

### 参考文档   
* [双向一对多关联关系](https://www.cnblogs.com/lj95801/p/5008519.html)    
* [JPA实体关系映射：@ManyToMany多对多关系、@OneToMany@ManyToOne一对多多对一关系和@OneToOne的深度实例解析。](https://www.jianshu.com/p/54108abb070f)                          

### 配置文件
1. 在resources目录下面新建`application-xxx.properties/yml`文件，命名以`application`开头，`-`后面命名可以自定义。      
2. 项目启动时，`application.properties/yml`会被读取，而`application-xxx.properties/yml`可以在`application.properties/yml`中指定。      
```yml
spring:
  profiles:
    active: xxx
```

## ORM的概念与思想
全称：Object Relation Mapping，对象关系映射。      
功能：数据库的作用在于存储数据和表示关系，数据库的存储方法可以用面向对象的思维来实现。    
数据库的组成：数据表，记录，字段。              
面向对象对应：类，对象，属性/成员变量。       

### 利用数据库表快速生成类
1. 利用IDEA的Database连接。
2. 打开IDEA → File → Project Structure → 项目右键Add → Persistence
   View  → Tool Windows → Persistence
3. 选择要生成的表进行导入即可。

### 实体类
1. 实体类要加上注解`@Entity`。      
2. 利用lombok的注解`@Getter`，`@Setter`省略Getter和Setter的书写。    
3. 类的主键要加上`@Id`注解。      
4. 提取基类时，基类要加上`@MappedSuperClass`注解，否则，在数据库查询时会出错。     
   * 通过注解@JsonIgnore不再将字段序列化传到前端。    
示例：
BaseEntity.java
```java
@Getter
@Setter
@MappedSuperclass
public abstract class BaseEntity {
    @JsonIgnore
    private Date createTime;
    @JsonIgnore
    private Date updateTime;
    @JsonIgnore
    private Date deleteTime;
}
```
Banner.java
```java
@Entity
@Getter
@Setter
@Where(clause = "delete_time is null")
public class Banner extends BaseEntity {
    @Id
    private Long id;
    private String name;
    private String description;
    private String title;
    private String img;

    @OneToMany(fetch = FetchType.LAZY)
    @JoinColumn(name = "bannerId")
    private List<BannerItem> items;
}
```
1. 配置jackson返回序列化。              
   * 返回字段下滑线连接(Snake)。            
   * 以1970年以来累计时间戳返回。                        
```java
spring:
  jackson:
    property-naming-strategy: SNAKE_CASE
    serialization:
      WRITE_DATES_AS_TIMESTAMPS: true
```

### 数据库的扩展
场景：表中的数据字段不够用。      
理论：表的字段不具备扩展性，但是记录具备扩展性。
     列不具备扩展性，行可以随意新增。
操作；可以将列的扩展转化为行的扩展，也就是多加一张表，将其它表的字段以记录的方式进行添加。    
示例：添加一张config表
```sql
id name         value                   table_name
1  color1       green                    theme
2  color2       red                      theme
3  title        chinese lunar new year   banner
4  description  all half price           theme
```
theme表
```
id name      img ...      extend
1  new year  http://xxx.  3&4
2
```
* 实际查询时，通过扩展字段extend找到config表中的记录，得到theme表中新增字段title和description的记录。       


## 快速开发技巧及常见问题
1. 布置springboot项目热重启。            
   * `dependency`中添加`devtools`             
   * `setting`中`compiler`设置为`build project automatically`     

2. 解决properties文件中乱码的问题    
`Settings` → `File Encodings` → `Default encoding for properties files:` → 选中`UTF-8` → `Transparent native-to-ascii conversion` → 打对勾    

## 参考资料
[Spring官方文档](https://spring.io/projects/spring-boot#learn)       
[接近8000字的Spring/SpringBoot常用注解总结！安排！](https://segmentfault.com/a/1190000022521844)     
[跟着官方文档学Spring Boot](https://zhuanlan.zhihu.com/p/55173112)      
[SpringBoot使用hibernate validator的基本注解](https://www.cnblogs.com/mr-yang-localhost/p/7812038.html#_label5)                   
[Java基础加强总结-注解](https://www.cnblogs.com/xdp-gacl/p/3622275.html)             