Think In Java 第三篇

##前言

主要是处理IO流的问题。

##任务

流程一定要过一遍。
IO流要弄明白啊！！
##问题
> 10.6 StreamTokenizer

##第 10 章 Java IO 系统
###10.1 输入和输出
可将Java 库的IO类分割为输入与输出两个部分.
通过继承，从InputStream（输入流）衍生的所有类都拥有名为read()的基本方法，用于读取单个字节或者字节数组。类似地，从 OutputStream衍生的所有类都拥有基本方法 write()，用于写入单个字节或者字节数组。
库的设计者首先决定与输入有关的所有类都是InputStream继承，而与输出有关的所有类都从OutputStream继承。
####InputStream 的类型
InputStream的作用是标志那些从不同起源地产生输入的类。这些起源地包括（每个都有一个相关的InputStream子类）：
(1) 字节数组
(2) String对象
(3) 文件
(4) “管道”，它的工作原理与现实生活中的管道类似：将一些东西置入一端，它们在另一端出来。
(5) 一系列其他流，以便我们将其统一收集到单独一个流内。
(6) 其他起源地，如Internet 连接等（将在本书后面的部分讲述）。
除此以外，FilterInputStream也属于 InputStream的一种类型，用它可为“破坏器”类提供一个基础类，以便将属性或者有用的接口同输入流连接到一起。
**表10.1 InputStream 的类型**
类 | 功能  |构建器参数／如何使用
------|
**ByteArrayInputStream** | 允许内存中的一个缓冲区作为InputStream使用  |  从中提取字节的缓冲区／作为一个数据源使用。通过将其同一个FilterInputStream 对象连接，可提供一个有用的接口
**StringBufferInputStream** | 将一个String 转换成 InputStream| 一个String（字串）。基础的实施方案实际采用一个StringBuffer（字串缓冲）／作为一个数据源使用。通过将其同一个 FilterInputStream对象连接，可提供一个有用的接口
**FileInputStream** | 用于从文件读取信息 |代表文件名的一个 String，或者一个 File 或FileDescriptor 对象／作为一个数据源使用。通过将其同一个FilterInputStream 对象连接，可提供一个有用的接口
**PipedInputString**| 产生为相关的 PipedOutputStream写的数据。|实现了“管道化”的概念PipedOutputStream／作为一个数据源使用。通过将其同一个 FilterInputStream对象连接，可提供一个有用的接口
**SequenceInputStream**| 将两个或更多的InputStream 对象转换成单个InputStream 使用 |两个InputStream对象或者一个Enumeration，用于 InputStream对象的一个容器／作为一个数据源使用。通过将其同一个FilterInputStream对象连接，可提供一个有用的接口
**FilterInputStream**| 对作为破坏器接口使用的类进行抽象；|那个破坏器为其他 InputStream类提供了有用的功能。

####OutputStream 的类型
这一类别包括的类决定了我们的输入往何处去：一个字节数组（但没有String；假定我们可用字节数组创建一个）；一个文件；或者一个“管道”。  除此以外，FilterOutputStream 为“破坏器”类提供了一个基础类，它将属性或者有用的接口同输出流连接起来。

**表10.2 OutputStream的类型**
类  | 功能 |	构建器参数／如何使用
-----|----
**ByteArrayOutputStream** |在内存中创建一个缓冲区。我们发送给流的所有数据都会置入这个缓冲区。| 可选缓冲区的初始大小／用于指出数据的目的地。若将其同FilterOutputStream 对象连接到一起，可提供一个有用的接口
**FileOutputStream**|	 将信息发给一个文件	| 用一个 String 代表文件名，或选用一个File 或FileDescriptor 对象／用于指出数据的目的地。若将其同FilterOutputStream 对象连接到一起，可提供一个有用的接口
**PipedOutputStream** | 我们写给它的任何信息都会自动成为相关的PipedInputStream的输出。|实现了“管道化”的概念 PipedInputStream／为多线程处理指出自己数据的目的地／将其同FilterOutputStream对象连接到一起，便可提供一个有用的接口
**FilterOutputStream**   |	对作为破坏器接口使用的类进行抽象处理；|那个破坏器为其他 OutputStream类提供了有用的功能。
###10.2 增添属性和有用的接口
利用层次化对象动态和透明地添加单个对象的能力的做法叫作“装饰器”（Decorator）方案。

问题：：装饰器为我们提供了大得多的灵活性（因为可以方便地混合与匹配属性），但它们也使自己的代码变得更加复杂。

####通过 FilterInputStream 从 InputStream 里读入数据
**表10.3 FilterInputStream 的类型**
类 	|	功能	| 构建器参数／如何使用
---|----
**DataInputStream**	 | 	与 DataOutputStream联合使用，使自己能以机动方式读取一个流中的基本数据类型（int，char，long 等等）	|	 InputStream/包含了一个完整的接口，以便读取基本数据类型
**BufferedInputStream**	| 	避免每次想要更多数据时都进行物理性的读取	|	告诉它“请先在缓冲区里找”InputStream，没有可选的缓冲区大小／本身并不能提供一个接口，只是发出使用缓冲区的要求。要求同一个接口对象连接到一起
**LineNumberInputStream** 	|	 跟踪输入流中的行号；可调用 getLineNumber()以及setLineNumber(int)   | 	只是添加对数据行编号的能力，所以可能需要同一个真正的接口对象连接
**PushbackInputStream** 	|	有一个字节的后推缓冲区，以便后推读入的上一个字符		|	 InputStream／通常由编译器在扫描器中使用，因为 Java 编译器需要它。一般不在自己的代码中使用

####通过 FilterOutputStream 向 OutputStream 里写入数据
所有方法都以“wirte”开头，例如 writeByte()，writeFloat()等等。
若想进行一些真正的格式化输出，比如输出到控制台，请使用PrintStream。
System.out 静态对象是一个PrintStream。PrintStream内两个重要的方法是 print()和println()。它们已进行了覆盖处理，可打印出所有数据类型。
**表10.4 FilterOutputStream 的类型**
类	| 功能	| 构建器参数／如何使用
-----|---
**DataOutputStream** 	|	与 DataInputStream配合使用，以便采用方便的形式将基本数据类型（int，char，long等）	|	写入一个数据流 OutputStream／包含了完整接口，以便我们写入基本数据类型
**PrintStream** 	|	用于产生格式化输出。	|	DataOutputStream 控制的是数据的“存储”，而PrintStream 控制的是“显示” OutputStream，可选一个布尔参数，指示缓冲区是否与每个新行一同刷新／对于自己的OutputStream对象，应该用“final”将其封闭在内。可能经常都要用到它
**BufferedOutputStream** | 用它避免每次发出数据的时候都要进行物理性的写入，要求它“请先在缓冲区里找”。	|	可调用flush()，对缓冲区进行刷新 OutputStream，可选缓冲区大小／本身并不能提供一个接口，只是发出使用缓冲区的要求。需要同一个接口对象连接到一起

###10.3 本身的缺陷：RandomAccessFile
RandomAccessFile用于包含了已知长度记录的文件，以便我们能用 seek()从一条记录移至另一条；然后读取或修改那些记录。各记录的长度并不一定相同；只要知道它们有多大以及置于文件何处即可。
首先，我们有点难以相信RandomAccessFile 不属于InputStream 或者OutputStream 分层结构的一部分。
从根本上说，RandomAccessFile 类似DataInputStream和 DataOutputStream的联合使用。
**一些方法：**，getFilePointer()用于了解当前在文件的什么地方，seek()用于移至文件内的一个新地点，而 length()用于判断文件的最大长度。此外，构建器要求使用另一个自变量（与C 的fopen()完全一样），指出自己只是随机读（"r"），还是读写兼施（"rw"）。这里没有提供对“只写文件”的支持。也就是说，假如是从DataInputStream继承的，那么 RandomAccessFile也有可能能很好地工作。
###10.4 File 类
File 类有一个欺骗性的名字——通常会认为它对付的是一个文件，但实情并非如此。它既代表一个特定文件的名字，也代表目录内一系列文件的名字。
####目录列表器
####检查与创建目录
###10.5 IO 流的典型应用
####输入流
```groovy
package io;

import java.io.*;

/**
 * 这是经典的例子
 */
public class IOStreamDemo {
    public static void main(String[] args) {
        String fileName = "E:\\testjava\\c.txt";
        try {
            DataInputStream in = new DataInputStream(new BufferedInputStream(new FileInputStream(fileName)));
            String s, s2 = new String();
            while ((s = in.readLine()) != null)
                s2 += s + "\n";
            in.close();
            System.out.println(s2);
            System.out.println("111111111111111111111111111111111111111111111111111111111");

            StringBufferInputStream in2 = new StringBufferInputStream(s2);
            int c;
            while ((c = in2.read()) != -1) {
                System.out.print((char) c);
            }
            System.out.println("222222222222222222222222222222222222222222222222222222");

            try {
                DataInputStream in3 = new DataInputStream(new StringBufferInputStream(s2));
                while (true) {
                    System.out.print((char) in3.readByte());
                }
            } catch (EOFException e) {
                System.out.println("End of stream encountered");
            }
            System.out.println("333333333333333333333333333333333333333333333333");

            //开始🔛写的操作。
            try {
                LineNumberInputStream li = new LineNumberInputStream(new StringBufferInputStream(s2));
                DataInputStream in4 = new DataInputStream(li);
                PrintStream out1 = new PrintStream(new BufferedOutputStream(new FileOutputStream("IODemo.out")));
                while ((s = in4.readLine()) != null)
                    out1.println("Line " + li.getLineNumber() + s);
                out1.close(); // finalize() not reliable!
            } catch (EOFException e) {
                System.out.println("End of stream encountered");
            }
            System.out.println("444444444444444444444444444444444444444444444444444");
            // 5. Storing & recovering data
            try {
                DataOutputStream out2 = new DataOutputStream(new BufferedOutputStream(new FileOutputStream("Data.txt")));
                out2.writeBytes("Here's the value of pi: \n");
                out2.writeDouble(3.14159);
                out2.close();
                DataInputStream in5 = new DataInputStream(new BufferedInputStream(new FileInputStream("Data.txt")));
                System.out.println(in5.readLine());
                System.out.println(in5.readDouble());
            } catch (EOFException e) {
                System.out.println("End of stream encountered");
            }
            System.out.print("5555555555555555555555555555555555555555555");

            // 6. Reading/writing random access files
            RandomAccessFile rf = new RandomAccessFile("rtest.dat", "rw");
            for (int i = 0; i < 10; i++)
                rf.writeDouble(i * 1.414);
            rf.close();
            rf = new RandomAccessFile("rtest.dat", "rw");
            rf.seek(5 * 8);
            rf.writeDouble(47.0001);
            rf.close();

            rf = new RandomAccessFile("rtest.dat", "r");
            for (int i = 0; i < 10; i++)
                System.out.println("Value " + i + ": " + rf.readDouble());
            rf.close();
            System.out.println("6666666666666666666666");

            //直接写入到Data2.txt
            PrintStream out3 = new PrintStream("Data2.txt");
            out3.print("Test of PrintFile");
            out3.close();
            System.out.println("777777777777777777777777");
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```
下面的解释都是对应一定个例子的。
1. 缓冲的输入文件:需要使用一个FileInputStream->BufferedInputStream
2. 从内存输入:并用它创建一个 StringBufferInputStream（字串缓冲输入流）
3. 格式化内存输入:StringBufferInputStream 的接口是有限的，所以通常需要将其封装到一个DataInputStream 内，从而增强它的能力。若选择用readByte()每次读出一个字符，那么所有值都是有效的，所以不可再用返回值来侦测何时结束输入。相反，可用available()方法判断有多少字符可用。
4. 行的编号与文件输出
5. 保存和恢复数据
```groovy
            try {
                DataOutputStream out2 = new DataOutputStream(new BufferedOutputStream(new FileOutputStream("Data.txt")));
                out2.writeBytes("Here's the value of pi: \n");
                out2.writeDouble(3.14159);
                out2.close();
                DataInputStream in5 = new DataInputStream(new BufferedInputStream(new FileInputStream("Data.txt")));
                System.out.println(in5.readLine());
                System.out.println(in5.readDouble());
            } catch (EOFException e) {
                System.out.println("End of stream encountered");
            }
```
但为了输出数据，以便由另一个数据流恢复，则需用一个DataOutputStream 写入数据，并用一个DataInputStream 恢复（获取）数据。当然，这些数据流可以是任何东西，但这里采用的是一个文件，并进行了缓冲处理，以加快读写速度。
注意字串是用writeBytes()写入的，而非writeChars()。然后再用readLine()将字符当作普通的ASCII 行读回。
writeDouble()将double 数字保存到数据流中，并用补充的readDouble()恢复它。
6. 读写随机访问文件
```groovy
            RandomAccessFile rf = new RandomAccessFile("rtest.dat", "rw");
            for (int i = 0; i < 10; i++)
                rf.writeDouble(i * 1.414);
            rf.close();
            rf = new RandomAccessFile("rtest.dat", "rw");
            rf.seek(5 * 8);
            rf.writeDouble(47.0001);
            rf.close();

            rf = new RandomAccessFile("rtest.dat", "r");
            for (int i = 0; i < 10; i++)
                System.out.println("Value " + i + ": " + rf.readDouble());
            rf.close();
```
RandomAccessFile 与 IO层次结构的剩余部分几乎是完全隔离的，尽管它也实现了DataInput 和DataOutput 接口。所以不可将其与 InputStream及 OutputStream子类的任何部分关联起来。尽管也许能将一个ByteArrayInputStream 当作一个随机访问元素对待，但只能用RandomAccessFile打开一个文件。必须假定RandomAccessFile 已得到了正确的缓冲，因为我们不能自行选择。
可以自行选择的是第二个构建器参数：可决定以“只读”（r）方式或“读写”（rw）方式打开一个RandomAccessFile 文件。
7. 快速文件输入
若想创建一个对象，用它从一个缓冲的DataInputStream中读取一个文件，可将这个过程封装到一个名为InFile的类内。
```groovy
class InFile extends DataInputStream {
    /**
     * Creates a DataInputStream that uses the specified
     * underlying InputStream.
     *
     * @param in the specified input stream
     */
    public InFile(InputStream in) {
        super(in);
    }

    public InFile(String filename) throws FileNotFoundException {
        super(new BufferedInputStream(new FileInputStream(filename)));
    }

    public InFile(File file) throws FileNotFoundException {
        this(file.getPath());
    }
}
```
8.快速输出格式化文件
亦可用同类型的方法创建一个PrintStream，令其写入一个缓冲文件。
9.快速输出数据文件，这个地方和7是对应的。

####从标准输入中读取数据
以Unix 首先倡导的“标准输入”、“标准输出”以及“标准错误输出”概念为基础，Java 提供了相应的System.in，System.out 以及System.err。
#### 管道数据流，会在14章讲解。
PipedInputStream（管道输入流）和 PipedOutputStream（管道输出流）
###10.6 StreamTokenizer（翻译：流分解器）
> 例子比较长在实际的实战中没有明显的效果。
> 继续加把劲。

###10.7 Java 1.1 的 IO 流
InputStreamReader将一个 InputStream转换成 Reader，OutputStreamWriter 将一个OutputStream转换成 Writer。
####数据的发起与接收
发起＆接收：Java 1.0 类 对应的 Java 1.1 类
Sources & Sinks:Java 1.0 class | Corresponding Java 1.1 class
----|
InputStream  |Reader<br>converter:  InputStreamReader
OutputStream 	| W r i t e r<br>converter:  OutputStreamWriter
FileInputStream	|  FileReader
FileOutputStream		| FileWriter
StringBufferInputStream		| StringReader
(no corresponding class) 	| 	StringWriter
ByteArrayInputStream 	|	CharArrayReader
ByteArrayOutputStream	| CharArrayWriter
PipedInputStream		| PipedReader
PipedOutputStream	| PipedWriter

####修改数据流的行为
过滤器：Java 1.0 类 |	对应的Java 1.1 类
-----|----
FilterInputStream	| FilterReader
FilterOutputStream	| FilterWriter（没有子类的抽象类）
BufferedInputStream	| BufferedReader（也有 readLine()）
BufferedOutputStream 	|BufferedWriter
DataInputStream 	|	使用DataInputStream（除非要使用 readLine()，那时需要使用一个 BufferedReader）
PrintStream 		|		PrintWriter
LineNumberInputStream	| LineNumberReader
StreamTokenizer	| StreamTokenizer（用构建器取代Reader）
PushBackInputStream	| PushBackReader
为了将向PrintWriter 的过渡变得更加自然，它提供了能采用任何OutputStream对象的构建器。
PrintWriter提供的格式化支持没有 PrintStream 那么多；但接口几乎是相同的。
####未改变的类
DataOutputStream
File
RandomAccessFile
SequenceInputStream
特别未加改动的是DataOutputStream，所以为了用一种可转移的格式保存和获取数据，必须沿用InputStream和 OutputStream层次结构。
####重导向标准 IO
在System 类中添加了特殊的方法，允许我们重新定向标准输入、输出以及错误 IO流。
下述简单的静态方法调用：
setIn(InputStream)
setOut(PrintStream)
setErr(PrintStream)
>		package io;
		import java.io.*;
		public class Redirecting {
		    public static void main(String[] args) {
		        try {
		            BufferedInputStream in =
		                    new BufferedInputStream(
                                    new FileInputStream("HelloWorld.iml"));
		            PrintStream out =
		                    new PrintStream(
		                            new BufferedOutputStream(new FileOutputStream("test.out")));
                 System.setOut(out);
	            System.setIn(in);
		      System.setErr(out);
            BufferedReader br =
                    new BufferedReader(
                            new InputStreamReader(System.in));
            String s;
            while ((s = br.readLine()) != null)
                System.out.println(s);
	            out.close(); // Remember this!
	        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
	}

问题来了：
> System.setOut(out);出现的位置很重要啊！
> 如果在上面就进行了处理，那么问题来了。下面的输出是没有的。不是没有了而是在文件里面处理了。
>  System.out.println(s);如果在控制台出现了，那么文件里面就没有了。

Note:The constructor java.io.PrintStream(java.io.OutputStream) has been deprecated.
注意：不推荐使用构建器java.io.PrintStream（java.io.OutputStream）。
然而，无论 System.setOut()还是System.setErr()都要求用一个 PrintStream作为参数使用，所以必须调用PrintStream构建器。
###10.8 压缩
来了一个没有处理过的。
Java 1.1 压缩类    |	 功能
------|
CheckedInputStream		| GetCheckSum()为任何InputStream 产生校验和（不仅是解压）
CheckedOutputStream		| GetCheckSum()为任何OutputStream 产生校验和（不仅是解压）
DeflaterOutputStream	| 用于压缩类的基础类
ZipOutputStream		|	 一个DeflaterOutputStream，将数据压缩成Zip文件格式
GZIPOutputStream	|	 一个DeflaterOutputStream，将数据压缩成GZIP 文件格式
InflaterInputStream 	|	用于解压类的基础类
ZipInputStream	|	 一个 DeflaterInputStream，解压用 Zip文件格式保存的数据
GZIPInputStream		|	 一个DeflaterInputStream，解压用GZIP 文件格式保存的数据

尽管存在许多种压缩算法，但是Zip和 GZIP 可能最常用的。
####用 GZIP 进行简单压缩
GZIP 接口非常简单，所以如果只有单个数据流需要压缩（而不是一系列不同的数据），那么它就可能是最适当选择。





