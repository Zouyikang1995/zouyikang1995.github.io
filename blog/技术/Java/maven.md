# Maven软件依赖管理  

## Maven概述   
### 定义
Maven：一个用于`自动化构建项目`和`管理项目依赖`的工具。   

```注释
自动化构建项目：按照企业中主流的项目模板，创建完善的项目结构。     
管理项目依赖：配置式添加和管理，自动下载和导入。     
```

### Maven环境搭建
1. 前置环境：jdk环境  
   JDK：Java Development Kit
   JAVA_HOME / CLASSPATH / PATH 
2. Maven概述，构建Maven环境
   Maven Environment  
   MAVEN_HOME / M2_HOME / PATH 
   [Maven网站](https://maven.apache.org)     
3. 工具操作：IDEA概述 

### Maven快速入门    
1. IDEA中配置maven    
File → Settings → Build,Execution,Deployment → Build Tools → Maven   
Maven home directory：修改为安装maven的文件地址       
User settings file：修改为自己安装maven的conf文件夹下settings.xml地址     
Local repository：maven仓库可以使用默认仓库地址，也可自己修改   

```注释
当maven项目构建太慢时，可以将国外的仓库地址修改为国内的镜像仓库地址，比如阿里的仓库地址。
```

2. pom.xml文件中添加依赖地址dependency
[依赖检索网站](https://mvnrepository.com/)     
[插件搜索网址](https://maven.apache.org/plugins/index.html)       

3. Maven运行方式
   * 使用maven命令
     Add Configuration → ＋ → Maven
     Command line输入：tomcat7：run
   * 配置本地的Tomcat方式
     Add Configuration → ＋ → Tomcat Server/local
     Application Server: 选择tomcat配置的位置   
     Deployment → ＋ → Artifacts → 添加发布的项目

4. `问题`：创建Maven项目，挂死在构建项目环节 [INFO] Generating project in Batch mode 
   `原因`：防火墙，阻止/延缓了访问在国外的Maven仓库
   `解决办法`：Maven中央仓库设置为国内镜像
```java
<mirror>
    <id>alimaven</id>
    <name>aliyun maven</name>
    <url>
    http://maven.aliyun.com/nexus/content/groups/public
    </url>
    <mirrorOf>central</mirrorOf>
</mirror>
```

5. Maven文件结构   
   中央仓库：远程服务器，保存各种项目模板、依赖，插件等
         ↓
    Maven   →    本地仓库：存储下载的依赖、插件等
       
           -- bin     →  mvn clean/build/package...
           |_ boot
           |_ conf    →  settings.xml  →  jdk 1.8 maven repo
MAVEN_HOME |_ lib
           |_ usrlibs → org.springframework  org.apache org.mybatis
           |_ LICENSE 
           |_ NOTICE 
           |_ README.txt

## Maven基础操作
### 基础组件：仓库 Repository
`远程仓库/中央仓库`：Maven官方服务器中存储各大技术社区和企业开发工具   
`本地仓库`：存储仓库依赖的本地文件夹   
`私有服务器`

### 基础组件：配置
`Maven核心组件`：配置     
`全局配置`：settings.xml 
```xml
<localRepository/>    配置本地仓库
<interactiveMode/>    配置是否需要和用户交互
<usePluginRegistry/>  配置是否需要通过pluginRegistry.xml来配置插件
<offline/>            是否启用离线模式
<pluginGroups/>       用来配置插件的groupID没有配置的情况下，自动配置groupID
<servers/>            用于配置远程仓库所在的服务器在访问时所需要的身份验证信息  
<mirrors/>            给仓库列表配置对应的镜像列表  
<proxies/>            配置连接仓库的代理  
<profiles/>           全局构造参数配置列表   
<activeProfiles/>     手工解锁profile配置
<activation/>         profile的扩展选项
<properties/>         声明扩展配置项
<repositories/>       配置远程仓库列表，用于开发时多仓库的配置  
<pluginRepositories>   
```     
`项目配置`：pom.xml    
优先级：pom.xml > settings.xmlnote >settings.xml  项目配置 > 用户配置 > 全局配置   




