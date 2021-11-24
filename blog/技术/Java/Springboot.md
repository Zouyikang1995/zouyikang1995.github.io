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
* `@ConditionOnBean` 当SpringIoc容器内存在指定Bean的条件    
 `@ConditionalOnMissingBean`与之含义相反
```java
@Bean
    @ConditionalOnBean(name = "mysql")
    public ISkill diana() {
        return new Diana("Diana", 18);
    }
```

## 配置@Configuration
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


## 快速开发技巧
1. 布置springboot项目热重启。
   * `dependency`中添加`devtools`
   * `setting`中`compiler`设置为`build project automatically`    

## 参考资料
[Spring官方文档](https://spring.io/projects/spring-boot#learn)       
[接近8000字的Spring/SpringBoot常用注解总结！安排！](https://segmentfault.com/a/1190000022521844)     
[跟着官方文档学Spring Boot](https://zhuanlan.zhihu.com/p/55173112)      