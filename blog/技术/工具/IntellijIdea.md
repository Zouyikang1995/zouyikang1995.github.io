# Intellij Idea

## 常用设置
1. 参数自动提示
   `File`→`Settings`→`Editor`→`General`→`Code Completion`
   Parameter Info下面选中三项：
   * Show parameter name hints on completion
   * Auto-display parameter info in 1000 ms
   * Show full signatures
2. Idea下安装lombok
   * `pom.xml`中加入lombok依赖包(使用springboot时可以不用指定版本号)
     ```java
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
     ```
   * Idea中加入lombok插件
     `File`→`Settings`→`Plugins`         
     搜索 `lombok`，点击安装install，然后提示自动重启，进行重启即可。  
3. `Springboot`在`idea`中使用`devtols`热部署自动编译
   * 在`Maven`的`pom.xml`文件中添加依赖
     ```java
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
     ```
   * 设置
     `File` -> `Settings` -> `Compiler`，勾选 `Build Project automatically`      
     按快捷键`Ctrl` + `Shift` + `Alt` + `/`，选择 `1.Registry...`，勾选 `compiler.automake.allow.when.app.running` 即可


## Intellij Idea ShortCuts


## lombok
### lombok常见问题
1. 查看`@Getter`和`Setter`等注解生成的代码
   
2. 编译时无法找到`@Getter`和`Setter`的问题    
   * 原因一： IDEA的编译方式错误
     `Settings`→`Build,Execution,Deployment`→`Compiler`→`Java Compiler`      
     Use compiler: `Javac`(选择)
   * 原因二：没有打开注解生成器Enable annotation processing
     `Settings`→`Build,Execution,Deployment`→`Compiler`→`Annotation Processors`     
     Enable annotation processing (选中对勾)    

### lombok常用注解
[来自lombok的注解](https://www.cnblogs.com/softzrp/p/10369972.html)

## 参考资料

[知乎：intellij idea参数可以自动提示么？](https://www.zhihu.com/question/24429111)     
[CSDN:IDEA下lombok安装，以及找不到get，set问题](https://blog.csdn.net/xzp_12345/article/details/80268834)        
[Springboot在idea中使用devtools热部署配置不生效的解决办法](https://www.huaweicloud.com/articles/46d2d4ff12cb14137c023391866eefa4.html)        