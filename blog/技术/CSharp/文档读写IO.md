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
`DirectoryInfo`和`FileInfo`两个类有许多来自FileSystemInfo类的属性，具体如下表。   

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
第一个要学习的IO路径处理类就是`DirectoryInfo`类。这个类包含了一系列`创建`、`移动`、`删除`以及`列举`路径或者子路径到的成员。除了FileSystemInfo基类提供的方法之外，DirectoryInfo类另外还提供了以下方法。   

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
```c#
class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("***** Fun with Directory(Info) *****\n");
        ShowWindowsDirectoryInfo();
        Console.ReadLine();
    }
    static void ShowWindowsDirectoryInfo()
    {
        // 输出目录信息
        DirectoryInfo dir = new DirectoryInfo(@"C:\Windows");
        Console.WriteLine("***** Directory Info *****");
        Console.WriteLine("FullName: {0}", dir.FullName);
        Console.WriteLine("Name: {0}", dir.Name);
        Console.WriteLine("Parent: {0}", dir.Parent);
        Console.WriteLine("Creation: {0}", dir.CreationTime);
        Console.WriteLine("Attributes: {0}", dir.Attributes);
        Console.WriteLine("Root: {0}", dir.Root);
        Console.WriteLine("*******************************\n")
    }
}
```
输出相关信息如下：
```c
***** Fun with Directory(Info) *****
***** Directory Info *****
FullName: C:\Windows
Name: Windows
Parent:
Creation: 7/9/2017 8:52:58 AM
Attributes: Directory
Root: C:\
**************************
```

### 通过DirectoryInfo类型来遍历文件 
处理获取目录的基本信息之外，我们还可以使用`DirectoryInfo`类型的相关方法。首先，我们可以利用GetFiles()方法来获取`C:\Windows\Web\Wallpaper`目录下所有关于`*.jpg`文件的相关信息。GetFile()方法返回一个`FileInfo`对象的数组，每个对象包含一个特定文件的相关信息。     
```c#
static void DisplayImageFiles()
{
    DirectoryInfo dir = new DirectoryInfo(@"C:\Windows\Web\Wallpaper");
    // 获取所有带有*.jpg扩展名的文件
    FileInfo[] imageFiles = dir.GetFiles("*.jpg", SearchOption.AllDirectories);
    Console.WriteLine("Found {0} *.jpg files\n", imageFiles.Length);
    // 然后输出每个文件的信息
    foreach (FileInfo f in imageFiles)
    {
        Console.WriteLine("***************************");
        Console.WriteLine("File name: {0}", f.Name);
        Console.WriteLine("File size: {0}", f.Length);
        Console.WriteLine("Creation: {0}", f.CreationTime);
        Console.WriteLine("Attributes: {0}", f.Attributes);
        Console.WriteLine("***************************\n");
    }
}
```
由于在调用`GetFiles()`方法时添加了`SearchOption`的参数，所以可以查看该根目录下所有的子目录。   

### 通过DirectoryInfo类型来创建子目录  
可以使用Directory.CreateSubdirectory方法来扩展目录结构。这个方法可以产生一个单独的子目录，也可以创建许多迭代包含的子目录。   
```c#
    DirectoryInfo dir = new DirectoryInfo(@"C:\");
    // C盘下创建\MyFolder文件夹
    dir.CreateSubdirectory("MyFolder");
    // 创建\MyFolder2\Data文件夹
    dir.CreateSubdirectory(@"MyFolder2\Data");
```
我们无法获取`CreateSubdirectory()`方法的返回值，但是我们应该意识到Directory对象代表了新创建对象的成功执行。注意在DirectoryInfo构造器中的[.]，代表了你可以访问该程序的执行目录(该项目的`*.exe`所在目录)。    
```c#
{
    DirectoryInfo dir = new DirectoryInfo(".");
    // 当前目录下创建\MyFolder文件夹
    dir.CreateSubdirectory("MyFolder");
    // 获取返回的DirectoryInfo对象
    DirectoryInfo myDataFolder = dir.CreateSubdirectory(@"MyFolder2\Data");
    // 输出..\MyFolder2\Data路径
    Console.WriteLine("New Folder is: {0}", myDataFolder);
}
```

## Directory使用方法
在大多数情况下，Directory的静态成员与DirectoryInfo的实例成员具有相同的函数和属性。然而，`Directory`的函数通常只返回`string`类型的返回值，而`DirectoryInfo`的函数会返回`FileInfo`的对象。   
下面让我们看一下`Directory`类，使用`Directory.GetLogicalDrives()`方法来获得当前计算机所有磁盘的路径，然后利用静态方法`Directory.Delete()`去移除`\MyFolder2\Data`两个子路径(假设刚刚已经创建过这个路径)。
```c#
static void FunWithDirectoryType()
{
    // 列出当前计算机中所有的磁盘
    string[] drives = Directory.GetLogicalDrives();
    Console.WriteLine("Here are your drives:");
    foreach (string s in drives)
    Console.WriteLine("--> {0} ", s);
    // 删除已创建的文件夹
    Console.WriteLine("Press Enter to delete directories");
    Console.ReadLine();
    try
    {
        Directory.Delete(@"C:\MyFolder");
        // 第二个参数决定是否销毁子目录
        Directory.Delete(@"C:\MyFolder2", true);
    }
    catch (IOException e)
    {
        Console.WriteLine(e.Message);
    }
}
```

## DriveInfo类的使用
System.IO.namespace提供了一个`DriveInfo`类。如同`Directory.GetLogicalDrives()`方法一样，静态方法`DriveInfo.GetDrives()`也允许查询当前计算机的驱动器信息。不同的是，`DriveInfo`类提供更多相关的信息(比如，驱动器类型，可用空间等等)。    
```c#
class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("***** Fun with DriveInfo *****\n");
        // 获取所以的驱动器信息
        DriveInfo[] myDrives = DriveInfo.GetDrives();
        // 输出驱动器状态
        foreach(DriveInfo d in myDrives)
        {
            Console.WriteLine("Name: {0}", d.Name);
            Console.WriteLine("Type: {0}", d.DriveType);
            // 检查驱动器信息
            if(d.IsReady)
            {
                Console.WriteLine("Free space: {0}", d.TotalFreeSpace);
                Console.WriteLine("Format: {0}", d.DriveFormat);
                Console.WriteLine("Label: {0}", d.VolumeLabel);
            }
            Console.WriteLine();
        }
        Console.ReadLine();
    }
}
```
执行的结果如下：
```
***** Fun with DriveInfo *****
Name: C:\
Type: Fixed
Free space: 791699763200
Format: NTFS
Label: Windows10_OS
Name: D:\
Type: Fixed
Free space: 23804067840
Format: NTFS
Label: LENOVO
Press any key to continue . . .
```
接下来，我们将要学习如何创建、打开、关闭以及销毁一个文件了。  

## FileInfo使用方法
`FileInfo`类允许你获取磁盘下文件的相关信息(例如，创建时间、大小、文件属性)，并且提供文件`创建`、`复制`、`移动`和`销毁`的方法。除了继承自FileSystemInfo的方法之外，还有一些额外的方法如下： 
 
**FileInfo类的主要成员**    

| 成员 | 含义 |
| :-- | :-- |
| AppendText() | 创建一个StreamWriter对象来给文件添加文本信息 |
| CopyTo() | 将一个已有文件复制给一个新文件 |
| Create() | 创建一个新文件并且返回一个FileStream对象，来操作新创建的文件 |
| CreateText() | 创建一个StreamWriter对象，来操作新创建的文件 |
| Delete() | 删除FileInfo绑定的文件 |
| Directory | 获取上一级路径(父路径)的实例 |
| DirectoryName | 获取上一级路径(父路径)的全路径 |
| Length | 获取当前文件的大小 |
| MoveTo() | 将指定文件移动到新的位置，并且修改文件名 |
| Name | 获取文件名 |
| Open() | 打开文件夹并且可以读/写以及分享文件 |
| OpenRead() | 创建一个只读的FileStream对象 |
| OpenText() | 创建一个StreamReader对象，可以读取文件内 |
| OpenWrite() | 创建一个只读的FileSteam对象 |

FileInfo类的大多数方法返回特定的IO接口类(例如，`FileStream`和`StreamWriter`)，这些接口可以在相关文件中以不同的格式`读取`/`写入`信息。   

### FileInfo.Create()方法
可以通过`FileInfo.Create()`方法来创建文件如下：  
```c#
static void Main(string[] args)
{
    // 在C盘下创建一个新文件
    FileInfo f = new FileInfo(@"C:\Test.dat");
    FileStream fs = f.Create();
    // 使用FileStream对象...
    // 关闭文件流
    fs.Close();
}
```
注意`FileInfo.Create()`方法返回的是一个`FileStream`对象，它可以对相关文件进行同步或者异步的`读取`/`写入`操作。在你使用完`FileStream`对象之后，你必须关闭它来销毁流资源。由于`FileStream`继承自`IDisposable`，你可以使用C#的using作用域来让编译器产生销毁的流程。  
```c#
static void Main(string[] args)
{
    // Defining a using scope for file I/O
    // types is ideal.
    FileInfo f = new FileInfo(@"C:\Test.dat");
    using (FileStream fs = f.Create())
    {
        // Use the FileStream object...
    }
}
```

### FileInfo.Open()方法
你可以使用`FileInfo.Open()`方法来打开已存在的文件，也可以比`FileInfo.Create()`方法更加精确地创建新文件。这是由于`Open()`方法需要几个与新建文件相关的参数。一旦调用`Open()`方法，就会返回一个`FileStream`对象。
```c#
static void Main(string[] args)
{
    // 通过FileInfo.Open()创建新文件
    FileInfo f2 = new FileInfo(@"C:\Test2.dat");
    using(FileStream fs2 = f2.Open(FileMode.OpenOrCreate,
    FileAccess.ReadWrite, FileShare.None))
    {
        // 使用FileStream对象...
    }
}
```
这个版本的`Open()`方法需要三个参数。第一个参数代表基本I/O请求(例如，新建文件，打开已存在的文件，以及在文件里添加)，这个参数你可以通过FileMode枚举得到：
```c#
public enum FileMode
{
    CreateNew,
    Create,
    Open,
    OpenOrCreate,
    Truncate,
    Append
}
```

**FileMode枚举成员**   
 
| 成员 | 含义 |
| :-- | :-- |
| CreateNew | 创建一个新文件，如果已经存在该文件就抛出一个`IOException`的异常 |
| Create | 创建一个新文件，如果已经存在就将该文件重写 |
| Open | 打开一个已存在的文件，如果该文件不存在就抛出一个`FileNotFoundException`的异常 |
| OpenOrCreate | 打开一个已存在的文件，如果不存在就创建该文件 |
| Truncate | 打开一个已存在的文件然后清空该文件大小成0字节 |
| Append | 打开一个文件并移动光标到文件尾部进行写入操作(该操作只能在写入流下进行)，如果文件不存在就新创建一个新文件 |

`Open()`方法的第二个参数是`FileAccess`枚举，它决定着流的`读取`/`写入`行为。
```c#
public enum FileAccess
{
    Read,
    Write,
    ReadWrite
}
```
最后，`Open()`方法的第三个参数是`FileShare`，它决定着如何同其它的文件处理器进行文件共享。
```c#
public enum FileShare
{
    Delete,
    Inheritable,
    None,
    Read,
    ReadWrite,
    Write
}
```

### FileInfo.OpenRead()和FileInfo.OpenWrite()方法
`FileInfo.Open()`让你以一个复杂的方式去获取文件处理器，但是`FileInfo`类也提供这些函数，例如OpenRead()和OpenWrite()。正如你所想，这些方法返回一个`只读`/`只写`的属性对象，而不需要提供枚举的参数值。比如FileInfo.Create和FileInfo.Open(),OpenRead(),OpenWrite()返回一个`FileStream`对象。   
```c#
// 事先保证“C:/”磁盘下有Test.dat和Test.dat文件
static void Main(string[] args)
{
    // 获取一个只读的FileStream对象
    FileInfo f3 = new FileInfo(@"C:\Test3.dat");
    using(FileStream readOnlyStream = f3.OpenRead())
    {
        // 使用FileStream对象...
    }
    // 获取一个只写FileStream对象
    FileInfo f4 = new FileInfo(@"C:\Test4.dat");
    using(FileStream writeOnlyStream = f4.OpenWrite())
    {
        // 使用FileStream对象...
    }
}
```

### FileInfo.OpenText()方法
另外一个`FileInfo`类型的打开方法是`OpenText()`。与Create(),Open(),OpenRead()或者OpenWrite()方法不同的是，`OpenText()`方法返回一个`StreamReader`类型的实例，而不是一个FileStream类型。假设在你的`C:\`磁盘下有一个`boot.ini`的文件，你可以通过以下方法获取文件内容：   
```c#
static void Main(string[] args)
{
    // 获取一个StreamReader对象
    FileInfo f5 = new FileInfo(@"C:\boot.ini");
    using(StreamReader sreader = f5.OpenText())
    {
        // 使用StreamReader对象...
    }
}
```
这里StreamReader类型提供一种读取文件内字符数据的方法。

### FileInfo.CreateText()和FileInfo.AppendText()方法
最后两个FileInfo类型的方法是`CreateText()`和`AppendText()`。它们两个都返回`StreamWriter`对象。   
```c#
static void Main(string[] args)
{
    FileInfo f6 = new FileInfo(@"C:\Test6.txt");
    using(StreamWriter swriter = f6.CreateText())
    {
        // 使用StreamWriter对象...
    }   
    FileInfo f7 = new FileInfo(@"C:\FinalTest.txt");
    using(StreamWriter swriterAppend = f7.AppendText())
    {
        // 使用StreamWriter对象...
    }
}
```
这里`StreamWriter`类型提供一种写入文件字符数据的方法。

## File使用方法
`File`类型使用`静态成员`，然而功能上与FileInfo类型一样。File提供AppendText(),Create(),Open(),OpenRead(),OpenWrite()和OpenText()方法。在大多数情况下，File和FileInfo可以`相互替换`着使用。要看同样的效果，可以将前面的示例用File改写。   
```c#
static void Main(string[] args)
{
    // 通过File.Create()来获取FileStream
    using(FileStream fs = File.Create(@"C:\Test.dat"))
    {}
    // 通过File.Open()来获取FileStream
    using(FileStream fs2 = File.Open(@"C:\Test2.dat",
    FileMode.OpenOrCreate,
    FileAccess.ReadWrite, FileShare.None))
    {}
    // 获取一个只读的FileStream对象
    using(FileStream readOnlyStream = File.OpenRead(@"Test3.dat"))
    {}
    // 获取一个只写的FileStream对象
    using(FileStream writeOnlyStream = File.OpenWrite(@"Test4.dat"))
    {}
    // 获取一个StreamReader对象
    using(StreamReader sreader = File.OpenText(@"C:\boot.ini"))
    {}
    // 获取StreamWriters
    using(StreamWriter swriter = File.CreateText(@"C:\Test6.txt"))
    {}
    using(StreamWriter swriterAppend = File.AppendText(@"C:\FinalTest.txt"))
    {}
}
```

### 其它File类成员
File类型还有一些其它成员，这些成员能够很大程度上`简化`读取/写入文本信息的操作。   

*File类型的方法*

| 方法 | 含义 |
| :-- | :-- |
| ReadAllBytes() | 打开指定的文件，返回二进制数组，然后关闭文件 |
| ReadAllLines() | 打开指定的文件，返回字符串数组，然后关闭文件 |
| ReadAllText() | 打开指定的文件，返回字符串(System.String)，然后关闭文件 |
| WriteAllBytes() | 打开指定的文件，写入字节数组，然后关闭文件 |
| WriteAllLines() | 打开指定的文件，写入字符串数组，然后关闭文件 |
| WriteAllText() | 打开指定的文件，写入字符串，然后关闭文件 |

你可以利用File的这些方法来读取/写入文件。甚至，这里的每个方法都会在文件操作完成后自动关闭。例如，下面这个console项目可以写入数据到C磁盘。 
```c#
class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("***** Simple I/O with the File Type *****\n");
        string[] myTasks = {
        "Fix bathroom sink", "Call Dave",
        "Call Mom and Dad", "Play Xbox One"};
        // 输出C磁盘下所有文件的信息
        File.WriteAllLines(@"tasks.txt", myTasks);
        // 读取所有文件后输出
        foreach (string task in File.ReadAllLines(@"tasks.txt"))
        {
            Console.WriteLine("TODO: {0}", task);
        }
        Console.ReadLine();
    }
}
```
关键在于，如果你想迅速获取文件处理工具，File类型会帮你省略几个关键词。然而，使用FileInfo对象的优势在于，你可以使用返回的`FileSystemInfo`抽象类来操作文件。  

## 抽象流类(Abstract Stream)
到目前为止，我们已经掌握了很多方法去获取FileStream,StreamReader以及StreamWriter对象。但是我们还没有用过这些对象去`读取`/`写入`数据。为了做到这一点，我们必须先了解`流`这一个概念。在IO处理中，`流(Stream)`代表着数据源到目的地之间的流动。流提供了字节数据的交换方法，而不用在意通过哪种设备(比如，文件、网络连接或者打印机)存储或者展现数据。   
抽象类System.IO.Stream类定义了几个处理同步/异步数据交互的方法，通过存储媒介(例如，文件或者内存)。     
再重申一遍，`Stream`代表`字节数据的流动`；因此，直接学习字节流可能很让人费解。一些基于流的类型可以提供查询(*seeking*)，通过查询我们可以获取或者调整在流之中的`数据位置`。下标将帮你更好地了解流的功能。  

*抽象流成员* 

| 成员 | 含义 |
| :-- | :-- |
| CanRead/CanWrite/CanSeek | 决定着当前流是否支持读，查询和写 |
| Close() | 关闭当前流然后`释放`流相关的资源(例如socket或者file处理类)，实际上，这个方法与`Dispose()`相关联；因此，关闭流的方法等同于销毁流(Dispose()方法) |
| Flush() | 更新当前数据的缓存然后`清除数据`。如果流不继承自`buffer`，则这个类不进行处理 |
| Length | 返回字节流的长度 |
| Position | 在当前流中决定着位置 |
| Read()/ReadByte()/ReadAsync() | 从当前流中读取一段字符，然后更新在当前流之中的位置 |
| Seek() | 设置当前流中决定的位置 |
| SetLength() | 设置当前流的长度 |
| Write()/WriteByte()/WriteAsync() | 写入一段字节长度的数据到流当中，然后更新流的位置 |

### FileStream使用方法
`FileStream`类继承自抽象类Stream，用来基于文件流的处理。这是很早创建的一个流，它只能读取或者写入`单个字节`或者`字节数组`。然而，你会发现实际上很少直接利用FileStream进行数据交换。相反，你或许会遇到各种各样流的包装类，这些类让文字数据更好处理。不管怎样，你会发现通过FileStream进行同步的数据读取/写入功能是很有帮助的。     
假设你有一个FileStreamApp的项目，你的目标是写入一些简单的文字信息到myMessage.dat文件中。然而，FileStream`只能`处理`字节数据`，你必须将`System.String`类型的数组`转化成`字节数组。幸运的是，System.Text命名空间下定义了一个名叫`Encoding`的类，它可以将字符串string转化成字节数组。    
一旦转码后，可以通过`FileStream.Write()`方法将字节数组写入文件。为了将这些字节读到内存中，你必须将`流stream`的位置给还原(Position属性)，然后调用`ReadByte()`方法。最后，你可以输出字节数组以及转化后的字符文本。下面是相关代码：   
```c#
// Don't forget to import the System.Text and System.IO namespaces.
static void Main(string[] args)
{
    Console.WriteLine("***** Fun with FileStreams *****\n");
    // Obtain a FileStream object.
    using(FileStream fStream = File.Open(@"myMessage.dat",FileMode.Create))
    {
        // Encode a string as an array of bytes.
        string msg = "Hello!";
        byte[] msgAsByteArray = Encoding.Default.GetBytes(msg);
        // Write byte[] to file.
        fStream.Write(msgAsByteArray, 0, msgAsByteArray.Length);
        // Reset internal position of stream.
        fStream.Position = 0;
        // Read the types from file and display to console.
        Console.Write("Your message as an array of bytes: ");
        byte[] bytesFromFile = new byte[msgAsByteArray.Length];
        for (int i = 0; i < msgAsByteArray.Length; i++)
        {
            bytesFromFile[i] = (byte)fStream.ReadByte();
            Console.Write(bytesFromFile[i]);
        }
        // Display decoded messages.
        Console.Write("\nDecoded Message: ");
        Console.WriteLine(Encoding.Default.GetString(bytesFromFile));
    }
    Console.ReadLine();
}
```
这个例子通过`FileStream`处理了文件数据，但是它也显示了直接使用FileStream的`缺点`：需要直接操作字节数据。其它继承自Stream类型的类也是类似的处理方式。比如，我们可以利用MemoryStream类往内存中写入字节数据，还可以用NetworkStream类来写入网络连接的字节数据。    
之前也提到过，System.IO命名空间下提供了几个reader和writer类型的类，它们可以隐藏stream类处理字节数据的实现细节。    

## StreamWriter和StreamReader使用方法
当你想要读取/写入字符数据时，`StreamWriter`和`StreamReader`类是很有用的。这两种类型都默认处理`Unicode`码，但是你可以通过修改System.Text.Encoding属性来修改编码。    
StreamReader类继承自`TextReader`抽象类，`StringReader`也是一样。TextReader基类提供了一些基础的方法，它能够让继承的类读取字符流以及在字符流中定位，下标描述了TextWriter类的核心成员。   

*TextWriter类的核心成员*  

| 成员 | 含义 | 
| :-- | :-- |
| Close() | 这个方法关闭`writer`并释放相关资源。在处理过程中，缓存自动被清空(这个方法等同于Dispose()方法) |
| Flush() | 这个方法为当前的writer清空所有缓存，并且让所有缓存的数据写入当前的设备；然而，它并不会关闭writer |
| NewLine | 这个属性为writer创建新的一行。默认结束符是`\r\n` |
| Write()/WriteAsync() | 这个重写的方法会写入数据到文本流(`text stream`)，而且不会换行 |
| WriterLine()/WriteLineAsync() | 这个重写的方法写入数据到文本流，同时会换行 |

SteamWriter类继承了Write(),Close()和Flush()方法，还定义了额外的`AutoFlush`属性。当设置为`true`的时候，这个属性会在你调用write操作时让StreamWriter`清空`所有缓存。但是当AutoFlush为`false`时会获得更好的性能，只不过StreamWriter写完时要记得调用`Close()`方法。    

### 写入文本文件
为了实际操作SteamWriter类型，我们创建一个StreamWriterReaderApp工程并且引入System.IO。在Main()方法里面用`File.CreateText()`方法创建一个当前执行目录下名为`reminders.txt`的文件。利用返回的`StreamWriter`对象，我们可以给文件添加一些文字内容。    
```c#
static void Main(string[] args)
{
    Console.WriteLine("***** Fun with StreamWriter / StreamReader *****\n");
    // 获取StreamWriter然后写入字符串
    using(StreamWriter writer = File.CreateText("reminders.txt"))
    {
        writer.WriteLine("Don't forget Mother's Day this year...");
        writer.WriteLine("Don't forget Father's Day this year...");
        writer.WriteLine("Don't forget these numbers:");
        for(int i = 0; i < 10; i++)
        writer.Write(i + " ");
        // 插入一行
        writer.Write(writer.NewLine);
    }
    Console.WriteLine("Created file and wrote some thoughts...");
    Console.ReadLine();
}
```
在执行这个工程之后可以通过文件查看内容，我们会发现文件在当前执行目录的`bin\Debug`路径下，因为我们在调用`CreateText()`时并未指定绝对路径。    

### 读取文本文件
接下来，我们会学习如何通过`StreamReader`来读取文件。这个类继承自`TextReader`抽象类，因此他拥有以下一些属性。   

*TextReader核心属性*    

| 成员 | 含义 | 
| :-- | :-- |
| Peek() | 返回下一个有效字符的位置(`integer`类型)，并且不改变位置。如果已经到达字符串的末尾则返回-1 | 
| Read()/ReadAsync() | 从`输入流`中读取数据 |
| ReadBlock()/ReadBlockAsync() | 从指定位置开始，在当前流当中读取一个`特定长度的字符`并写入缓存 |
| ReadLine()/ReadLineAsync() | 在`当前流`中读取一行字符并且返回该字符串 |
| ReadToEnd()/ReadToEndAsync() | 从当前位置开始`到字符结尾`，读取当前流中所有的字符并返回该字符串 |

如何利用SteamReader扩展刚刚的工程，可以读取`reminder.txt`文件里面的内容。  
```c#
static void Main(string[] args)
{
    Console.WriteLine("***** Fun with StreamWriter / StreamReader *****\n");
    ...
    // 从文件中读取数据
    Console.WriteLine("Here are your thoughts:\n");
    using(StreamReader sr = File.OpenText("reminders.txt"))
    {
        string input = null;
        while ((input = sr.ReadLine()) != null)
        {
            Console.WriteLine (input);
        }
    }
    Console.ReadLine();
}
```

### 直接创建StreamWriter/StreamReader类
一个令人疑惑的地方在于，当你使用System.IO时，你会发现你可以通过不同的方法获取相同的结果。例如，你已经知道可以利用`File`或者`FileInfo`类型的`CreateText()`方法来获取`StreamWriter`。但是你发现也可以直接创建`StreamWriters`和`StreamReaders`。例如，你可以这样操作当前工程：    
```c#
static void Main(string[] args)
{
    Console.WriteLine("***** Fun with StreamWriter / StreamReader *****\n");
    // 获取StreamWriter并且写入字符串
    using(StreamWriter writer = new StreamWriter("reminders.txt"))
    {
        ...
    }
    // 从文件中读取数据
    using(StreamReader sr = new StreamReader("reminders.txt"))
    {
        ...
    }
}
```
尽管我们可以看到利用这么多类似的方法去读取文件IO，但是要记住结果是非常灵活的。不管使用哪种方法都是操作字符流的，接下来我们该去探讨StringWriter和StringReader类了。     

## StringWriter和StringReader使用方法
我们可以利用`StingWriter`和`StringReader`类型来把文本内容当做流来进行处理。当我们想要处理基于字符的信息时，StringWriter和StringReader将是很有用的。下面的工程通过`StringWriter`对象写入字符串信息，而不是写入硬盘上的文件。    
```c#
static void Main(string[] args)
{
    Console.WriteLine("***** Fun with StringWriter / StringReader *****\n");
    // 创建一个StringWriter并且将字符添加到内存中
    using(StringWriter strWriter = new StringWriter())
    {
        strWriter.WriteLine("Don't forget Mother's Day this year...");
        // Get a copy of the contents (stored in a string) and dump
        // to console.
        Console.WriteLine("Contents of StringWriter:\n{0}", strWriter);
    }
    Console.ReadLine();
}
```
StringWriter和StringReader都是继承自相同的基类`TextWriter`，所以写的逻辑是类似的。然而，基于StringWriter的本质，我们可以使用`GetStringBuilder()`方法去实现`System.Text.StringBuilder`对象。   
```c#
using (StringWriter strWriter = new StringWriter())
{
    strWriter.WriteLine("Don't forget Mother's Day this year...");
    Console.WriteLine("Contents of StringWriter:\n{0}", strWriter);
    // 获取内部StirngBuilder
    StringBuilder sb = strWriter.GetStringBuilder();
    sb.Insert(0, "Hey!! ");
    Console.WriteLine("-> {0}", sb.ToString());
    sb.Remove(0, "Hey!! ".Length);
    Console.WriteLine("-> {0}", sb.ToString());
}
```
当我们想要从流中读取字符数据时，我们可以使用`StringReader`类型。它和`StreamReader`类的功能相似。实际上，`StringReader`类只是继承了一些内部方法，去读取一些`字符信息`，而不是读取文件。    
```c#
using (StringWriter strWriter = new StringWriter())
{
    strWriter.WriteLine("Don't forget Mother's Day this year...");
    Console.WriteLine("Contents of StringWriter:\n{0}", strWriter);
    // 从StringWriter中读取数据
    using (StringReader strReader = new StringReader(strWriter.ToString()))
    {
        string input = null;
        while ((input = strReader.ReadLine()) != null)
        {
            Console.WriteLine(input);
        }
    }
}
```

## BinaryWriter和BinaryReader使用方法
`BinaryWriter`和`BinaryReader`都继承自System.Object。这两种类型都允许以`二进制格式`往流中`读取`/`写入`单独的数据。`BinaryWriter`类定义了一个重写的`Write()`方法将数据放入流中。处理Write()方法之外，BinaryWriter还提供了额外的方法`获取`/`设置`基于流的类型。

*BinaryWriter核心成员*

| 成员 | 含义 |
| :-- | :-- |
| BaseStream | 这个只读的属性提供了获取`BinaryWriter`对象产生流的静态方法 |
| Close() | 这个方法关闭二进制流 |
| Flush() | 这个方法清空二进制流的缓存 | 
| Seek() | 这个方法设置当前流的位置 |
| Write() | 这个方法往当前流中写入数据 |

BinaryReader类实现了BinaryWriter类提供的功能。  

*BinaryReader核心成员*

| 成员 | 含义 |
| :-- | :-- |
| BaseStream | 这个只读的属性提供了获取BinaryReader对象产生流的方法 |
| Close() | 这个方法关闭二进制reader |
| PeekChar() | 这个方法返回下一个有效字符，然而并不会改变流之中的位置 |
| Read() | 这个方法读取存存储在数组中指定字节数/字符数的数据 |
| ReadXXXX() | BinaryReader定义了许多读取方法，这些方法可以获取流的下一个类型。（例如，`ReadBoolean()`,`ReadByte()`和`ReadInt32()`） |

下面这个例子往`*.dat`文件中写入数据。

```c#
static void Main(string[] args)
{
    Console.WriteLine("***** Fun with Binary Writers / Readers *****\n");
    // 为文件创建一个writer
    FileInfo f = new FileInfo("BinFile.dat");
    using(BinaryWriter bw = new BinaryWriter(f.OpenWrite()))
    {
        // 输出BaseStream类型
        // (System.IO.FileStream in this case).
        Console.WriteLine("Base stream is: {0}", bw.BaseStream);
        // 产生一些数据写入文件
        double aDouble = 1234.67;
        int anInt = 34567;
        string aString = "A, B, C";
        // 写入数据
        bw.Write(aDouble);
        bw.Write(anInt);
        bw.Write(aString);
    }
    Console.WriteLine("Done!");
    Console.ReadLine();
}
```
注意到返回自`FileInfo.OpenWrite()`方法的`FileStream`对象，传入BinaryWriter类型的构造函数。利用这个技术可以在写入数据之前更容易利用流。由于BinaryWriter的构造函数需要任意一个继承自`Stream`流的类型(例如，FileStream，MemoryStream，或者BufferStream)，因此，比起写入MemoryStream对象，往内存中写入二进制对象更加容易。     
BinaryReader类型也提供了一系列方法从BinFile.dat文件中读取数据。这里，我们使用读取方法从文件流中读取数据。    
```c#
static void Main(string[] args)
{
    ...
    FileInfo f = new FileInfo("BinFile.dat");
    ...
    // 从流中读取二进制数据
    using(BinaryReader br = new BinaryReader(f.OpenRead()))
    {
        Console.WriteLine(br.ReadDouble());
        Console.WriteLine(br.ReadInt32());
        Console.WriteLine(br.ReadString());
    }
    Console.ReadLine();
}
```

## 文档监测
我们已经掌握了readers和writers类，接下来学习`FileSystemMatcher`类。当我们想要通过程序监控文件变化时，这个类将十分有用。特别是，我们可以利用`FileSystemMatcher`类型去监控`System.IO.NotifyFilters`中定义的任意一种枚举类型。  
```c#
public enum NotifyFilters
{
    Attributes, CreationTime,
    DirectoryName, FileName,
    LastAccess, LastWrite,
    Security, Size
}
```
在使用FileSystemMatcher类型之前，我们需要设置我们想要监控的文件夹路径，同时使`Filter`属性中写入我们想要监控文件的`扩展名`。    
现在，我们也许想要处理`Changed`,`Create`和`Delete`事件，这些事件都是通过`FileSystemEventHandler`代理来处理。这个代理可以调用任何满足以下条件的函数：     
```c#
// FileSystemEventHandler代理必须指向下面的方法
void MyNotificationHandler(object source, FileSystemEventArgs e)
```
我们也可以使用RenameEventHandler代理类型来处理重命名事件，代理需要满足以下条件：   
```c#
// RenamedEventHandler必须指向下面的方法
void MyRenamedHandler(object source, RenamedEventArgs e)
```
既然我们可以使用传统的`代理`/`事件`语法来处理这些事件，我们肯定就可以使用`lambda表达式`语法了。    
接下来，我们将看到以下监控文件的处理结果。假设我们在`C:\`盘下面创建了一个`MyFolder`的文件夹，文件夹下面包含`*.txt`文件。下面的工程将监控MyFolder文件夹下所有txt文档，无论是这些文档进行了`创建`、`删除`、`修改`或者`重命名。
```c#
static void Main(string[] args)
{
    Console.WriteLine("***** The Amazing File Watcher App *****\n");
    // 指定要监控的文件
    FileSystemWatcher watcher = new FileSystemWatcher();
    try
    {
        watcher.Path = @"C:\MyFolder";
    }
    catch(ArgumentException ex)
    {
        Console.WriteLine(ex.Message);
    return;
    }
    // 设置要监控的条件
    watcher.NotifyFilter = NotifyFilters.LastAccess
                            | NotifyFilters.LastWrite
                            | NotifyFilters.FileName
                            | NotifyFilters.DirectoryName;
    // 仅仅监控txt文件
    watcher.Filter = "*.txt";
    // Add event handlers.
    watcher.Changed += new FileSystemEventHandler(OnChanged);
    watcher.Created += new FileSystemEventHandler(OnChanged);
    watcher.Deleted += new FileSystemEventHandler(OnChanged);
    watcher.Renamed += new RenamedEventHandler(OnRenamed);
    // 添加时间处理器
    watcher.EnableRaisingEvents = true;
    // 等待用户退出该项目
    Console.WriteLine(@"Press 'q' to quit app.");
    while(Console.Read()!='q')
    ;
}
```
下面的两个事件处理器简单地打印了文件变动信息：    
```c#
static void OnChanged(object source, FileSystemEventArgs e)
{
    // 当一个文件发生了改变、创建或者删除时触发
    Console.WriteLine("File: {0} {1}!", e.FullPath, e.ChangeType);
}
static void OnRenamed(object source, RenamedEventArgs e)
{
    // 当一个文件被重命名时触发
    Console.WriteLine("File: {0} renamed to {1}", e.OldFullPath, e.FullPath);
}
```
执行项目进行测试并且打开文件夹，尝试给文件`重命名`、`创建`txt文件、`删除`txt文件，等等。你会发现项目中有如下的输出信息：
```
***** The Amazing File Watcher App *****
Press 'q' to quit app.
File: C:\MyFolder\New Text Document.txt Created!
File: C:\MyFolder\New Text Document.txt renamed to C:\MyFolder\Hello.txt
File: C:\MyFolder\Hello.txt Changed!
File: C:\MyFolder\Hello.txt Changed!
File: C:\MyFolder\Hello.txt Deleted!
```

## 参考
[文章来源：Pro C# 7 With .NET and .NET Core, Eighth Edition. Andre Troelsen, Philip Japikse](https://www.amazon.co.jp/-/en/Andrew-Troelsen/dp/1484230175)           
[C#文件操作](http://c.biancheng.net/csharp/100/)