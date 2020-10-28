# 文档读写IO

## IO口
在.NET中，System.IO命名空间拥有文件输入/输出(Input/Output)的基础类。System.IO定义了许多相关类、接口、枚举、代理等等，大多数都可以在`mscorlib.dll`中找到。另外，`System.dll`也有一些相关类。在Visual Studio中，这两个程序集(Assembly)会被自动引入参照。其中，主要的类及功能如下表。   
| 非抽象的I/O类 | 含义 |
| :-- | :-- |
| BinaryReader/BinaryWriter | 这些类可以让你通过二进制获取基础类数据(integers, Booleans,strings等等) |
| BufferedStream | 这个类为字节流提供临时存储 |
| Directory | 这个类用来处理计算机路径，使用静态成员 |
| DirectoryInfo | 与Directory功能相同，只不过使用实例化对象(`object reference`) |
| DriveInfo | 提供详细的磁盘信息 |
| File/FileInfo | 这两个类用来处理文件，File类需要使用静态成员，而FileInfo通过实例化对象使用 |
| FileSystemWatcher | 监视特定目录下文件的变动 |
| MemoryStream | 读取内存中的数据 |
| Path | 对string类型的文件或目录地址进行处理 |
| StreamWriter/StreamReader | 获取文件内文字信息 |
| StringWriter/StringReader | 通过string缓存处理文字信息 |

## Directory(Info)和File(Info)
System.IO提供了四个操作文件的类，同时也可以处理目录。首先是Directory和File，这两个类可以通过类的静态方法对文件进行`创建`、`删除`、`复制`以及`移动`操作。与他们紧密相关的是FileInfo和DirectoryInfo类，这两个类可以通过实例化产生，作用与前面两个一样。其中，Directory和File类继承自System.Object，而DirectoryInfo和FileInfo来源于抽象类FileSystemInfo。     
FileInfo和DirectoryInfo`更适合`用来获取文件或者目录的详细信息，因为他们的对象返回`强的数据类型`。相比较而下，Directory和File类只返回简单的字符串类型。

## 抽象类FileSystemInfo 
DirectoryInfo和FileInfo两个类有许多来自FileSystemInfo类的属性，具体如下表。   

*FileSystemInfo属性*
| 属性 | 含义 | 
| :-- | :-- |
| Attributes | 获取/设置当前文档的属性，通过设置FileAttributes枚举类型(read-only, encrypted, hidden或者compressed) |
| CreationTime | 获取/设置当前文件/目录的创建时间 |
| Exists | 判断一个文件/目录是否存在 |
| Extension | 获取一个文件的扩展名 |
| FullName | 获取文件/目录的文件名/全路径名 |
| LastAccessTime | 获取/设置当前文件/目录的最后访问时间 |
| LastWriteTime | 获取/设置当前文件/目录的最后修改时间 |
| Name | 获取当前文件名/目录名 |

## DirectoryInfo使用方法
第一个要学习的IO路径处理类就是DirectoryInfo类。这个类包含了一系列创建、移动、删除以及列举路径或者子路径到的成员。除了FileSystemInfo基类提供的方法之外，DirectoryInfo类另外还提供了以下方法。   
*DirectoryInfo类的主要成员*
| 成员 | 含义 | 
| :-- | :-- |
| Create()/CreateSubdirectory() | 当指定路径名的时候，创建一个路径/子路径 |
| Delete() | 删除路径以及该路径下的内容 |
| GetDirectories() | 返回一个DirectoryInfo类的数组，每个类里面装载着在该路径下的子路径 |
| GetFiles() | 返回FileInfo类型数组，每个类里面装载着该路径下的文件 |
| MoveTo() | 将一个路径下以及该路径下的内容移动至另一个路径下 |
| Parent | 获取一个路径的上一级路径 |
| Root | 获取一个路径的根路径 |
通过指定一个特定的路径，就可以开始使用DirectoryInfo类来构造一个对象了，如果你想获取当前路径，也就是该工程产生的可执行文件(*.exe)所在路径，可以直接使用`.`。以下是一个例子：    
```c#
// 绑定当前工作路径。
DirectoryInfo dir1 = new DirectoryInfo(".");
// 绑定 C:\Windows
DirectoryInfo dir2 = new DirectoryInfo(@"C:\Windows");
```
在第二个例子里面，我们假设`C:\Windows`路径在当前计算机上存在。如果这个路径不存在，就会抛出一个System.IO.DirectoryNotFoundException的错误。因此，如果你指定一个不存在的路径，你需要在代码执行之前使用Create()方法创建该路径。     
```c#
// 绑定一个不存在的路径，然后创建这个路径。
DirectoryInfo dir3 = new DirectoryInfo(@"C:\MyCode\Testing");
dir3.Create();
```
创建DirectoryInfo对象之后，就可以通过继承自FileSystemInfo类的对象来访问目录有关的信息了。