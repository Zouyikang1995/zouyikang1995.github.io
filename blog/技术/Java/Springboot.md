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

## 快速开发技巧
1. 布置springboot项目热重启。
   * `dependency`中添加`devtools`
   * `setting`中`compiler`设置为`build project automatically`    

## 参考资料
[Spring官方文档](https://spring.io/projects/spring-boot#learn)       
[接近8000字的Spring/SpringBoot常用注解总结！安排！](https://segmentfault.com/a/1190000022521844)     
[跟着官方文档学Spring Boot](https://zhuanlan.zhihu.com/p/55173112)      