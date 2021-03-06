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
####用 Zip 进行多文件保存
下面的代码就可以处理所有问题。
```groovy

     String[] arg = new String[]{"E:\\testjava\\testjava\\d.txt", "E:\\testjava\\c.txt", "E:\\testjava\\d.txt"};
        String[] arg1 = new String[]{"testjava\\testjava\\d.txt", "testjava\\c.txt", "testjava\\d.txt"};


        try {
            FileOutputStream f = new FileOutputStream("test.zip");

            CheckedOutputStream csum = new CheckedOutputStream(f, new Adler32());
            ZipOutputStream out =
                    new ZipOutputStream(
                            new BufferedOutputStream(csum));
            out.setComment("A test of Java Zipping");

            for (int i = 0; i < arg.length; i++) {
                System.out.println("Writing file " + arg1[i]);

                //读取要压缩的值。
                BufferedReader in =
                        new BufferedReader(
                                new FileReader(arg[i]));
                //往特定的目录里面写。
                out.putNextEntry(new ZipEntry(arg1[i]));

                int c;
                while ((c = in.read()) != -1) {
                    out.write(c);
                }
                in.close();
            }
            out.close();
            System.out.println("Checksum: " + csum.getChecksum().getValue());
            /****************************上面已经完成写的操作********************************************/


            /************************************下面开始读的操作****************************************************/
            System.out.println("Reading file");
            FileInputStream fi = new FileInputStream("test.zip");//要读的文件
            CheckedInputStream csumi = new CheckedInputStream(fi, new Adler32());
            //而且尽管 CheckedInputStream 和CheckedOutputStream 同时提供了对Adler32和CRC32校验和的支持，
            //但是ZipEntry 只支持 CRC的接口。这虽然属于基层 Zip格式的限制，但却限制了我们使用速度更的Adler32。
            ZipInputStream in2 =
                    new ZipInputStream(
                            new BufferedInputStream(csumi));
            ZipEntry ze;
            System.out.println("Checksum: " + csumi.getChecksum().getValue());
            while ((ze = in2.getNextEntry()) != null) {
                System.out.println("**********Reading file " + ze);
                int x;
                while ((x = in2.read()) != -1)
                    System.out.write(x);//也是在控制台上面打印出来。
            }
            in2.close();

            /***************************下面是读取所有的文件*********************************/
            // Alternative way to open and read
            // zip files:下面是相应的文件读取出来。
            ZipFile zf = new ZipFile("test.zip");
            Enumeration e = zf.entries();
            while (e.hasMoreElements()) {
                ZipEntry ze2 = (ZipEntry) e.nextElement();
                System.out.println("File: " + ze2);
            }

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
```
对于要加入压缩档的每一个文件，都必须调用 putNextEntry()，并将其传递给一个 ZipEntry 对象。
ZipEntry 对象包含了一个功能全面的接口，利用它可以获取和设置 Zip文件内那个特定的 Entry（入口）上能够接受的所有数据：名字、压缩后和压缩前的长度、日期、CRC校验和、额外字段的数据、注释、压缩方法以及它是否一个目录入口等等。然而，虽然 Zip格式提供了设置密码的方法，但Java 的 Zip库没有提供这方面的支持。而且尽管 CheckedInputStream 和CheckedOutputStream 同时提供了对Adler32和CRC32校验和的支持，但是ZipEntry 只支持 CRC的接口。这虽然属于基层 Zip格式的限制，但却限制了我们使用速度更快的Adler32。
为解压文件，ZipInputStream 提供了一个 getNextEntry()方法，能在有的前提下返回下一个ZipEntry。作为一个更简洁的方法，可以用ZipFile 对象读取文件。该对象有一个entries()方法，可以为 ZipEntry 返回一个Enumeration（枚举）。
为读取校验和，必须多少拥有对关联的Checksum 对象的访问权限。在这里保留了指向CheckedOutputStream和CheckedInputStream 对象的一个句柄。但是，也可以只占有指向Checksum 对象的一个句柄。
Zip流中一个令人困惑的方法是setComment()。正如前面展示的那样，我们可在写一个文件时设置注释内容，但却没有办法取出 ZipInputStream内的注释。看起来，似乎只能通过 ZipEntry 逐个入口地提供对注释的完全支持。
当然，使用 GZIP 或Zip库时并不仅仅限于文件——可以压缩任何东西，包括要通过网络连接发送的数据。
####Java 归档（jar ）实用程序
jar实用程序已与Sun 的JDK配套提供，可以按我们的选择自动压缩文件。请在命令行调用它：
`jar [选项] 说明 [详情单] 输入文件`
其中，“选项”用一系列字母表示（不必输入连字号或其他任何指示符）。如下所示：
>c 创建新的或空的压缩档
t 列出目录表
x 解压所有文件
x file 解压指定文件
f 指出“我准备向你提供文件名”。若省略此参数，jar 会假定它的输入来自标准输入；或者在它创建文件
时，输出会进入标准输出内
m 指出第一个参数将是用户自建的详情表文件的名字
v 产生详细输出，对 jar做的工作进行巨细无遗的描述
O 只保存文件；不压缩文件（用于创建一个 JAR文件，以便我们将其置入自己的类路径中）
M 不自动生成详情表文件

下面是调用 jar的一些典型方法：
>jar cf myJarFile.jar *.class

用于创建一个名为myJarFile.jar 的 JAR文件，其中包含了当前目录中的所有类文件，同时还有自动产生的
详情表文件。
>jar cmf myJarFile.jar myManifestFile.mf *.class

与前例类似，但添加了一个名为myManifestFile.mf的用户自建详情表文件。
>jar tf myJarFile.jar

生成myJarFile.jar 内所有文件的一个目录表。
>jar tvf myJarFile.jar

添加“verbose”（详尽）标志，提供与myJarFile.jar 中的文件有关的、更详细的资料。
>jar cvf myApp.jar audio classes image

假定audio，classes 和image 是子目录，这样便将所有子目录合并到文件myApp.jar 中。其中也包括了“verbose”标志，可在jar 程序工作时反馈更详尽的信息。

如果用O 选项创建了一个JAR文件，那个文件就可置入自己的类路径（CLASSPATH）中：
CLASSPATH="lib1.jar;lib2.jar;"
Java 能在lib1.jar 和 lib2.jar 中搜索目标类文件。

jar工具的功能没有zip工具那么丰富。例如，不能够添加或更新一个现成 JAR 文件中的文件，只能从头开始新建一个 JAR文件。此外，不能将文件移入一个 JAR文件，并在移动后将它们删除。然而，在一种平台上创建的JAR 文件可在其他任何平台上由jar工具毫无阻碍地读出（这个问题有时会困扰zip工具）。
正如大家在第13 章会看到的那样，我们也用JAR为Java Beans 打包。

####对象序列化
它面向那些实现了Serializable接口的对象，可将它们转换成一系列字节，并可在以后完全恢复回原来的样子。

语言里增加了对象序列化的概念后，可提供对两种主要特性的支持。Java 1.1 的“远程方法调用”（RMI）使本来存在于其他机器的对象可以表现出好象就在本地机器上的行为。将消息发给远程对象时，需要通过对象序列化来传输参数和返回值。
####寻找类
####transient（临时）关键字
下面的代码就全都明白了：：
```groovy
/**
 * transient（临时）关键字,具体的实施
 */
public class Logon implements Serializable {
    private Date date = new Date();
    private String username;
    private transient String password;

    Logon(String name, String pwd) {
        username = name;
        password = pwd;
    }

    @Override
    public String toString() {
        String pwd = (password == null) ? "(n/a)" : password;
        return "logon info: \n " +
                "username: " + username +
                "\n date: " + date.toString() +
                "\n password: " + pwd;
    }

    /**
     * 由于Externalizable 对象默认时不保存它的任何字段，
     * 所以transient 关键字只能伴随Serializable使用。
     *
     * @param args
     */
    public static void main(String[] args) {
        Logon a = new Logon("Hulk", "myLittlePony");
        System.out.println("logon a=" + a);

        try {
            ObjectOutputStream out =
                    new ObjectOutputStream(
                            new FileOutputStream("Logon.out"));

            out.writeObject(a);
            out.close();
            int seconds = 5;
            long t = System.currentTimeMillis() + seconds * 1000;
            while (System.currentTimeMillis() < t) ;
            ObjectInputStream in =
                    new ObjectInputStream(
                            new FileInputStream("Logon.out"));
            System.out.println("Recovering object at " + new Date());
            a = (Logon) in.readObject();
            System.out.println("logon a=" + a);

        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}

```
>可以看到，其中的date 和 username 字段保持原始状态（未设成transient），所以会自动序列化。然而，password 被设为 transient，所以不会自动保存到磁盘；

####Externalizable的替代方法
```groovy
package io;

import java.io.*;

public class SerialCtl implements Serializable {
    String a;
    transient String b;

    public SerialCtl(String aa, String bb) {
        a = "Not Transient: " + aa;
        b = "Transient: " + bb;
    }

    public String toString() {
        return a + "\n" + b;
    }


    private void writeObject(ObjectOutputStream stream) throws IOException {
        System.out.println("先运行1111111111111");
        stream.defaultWriteObject();
        stream.writeObject(b);
    }

    private void readObject(ObjectInputStream stream) throws IOException, ClassNotFoundException {
        System.out.println("后运行22222222222222222");
        stream.defaultReadObject();
        b = (String) stream.readObject();
    }

    public static void main(String[] args) {
        SerialCtl sc = new SerialCtl("Test1", "Test2");
        System.out.println("Before:\n" + sc);
        ByteArrayOutputStream buf = new ByteArrayOutputStream();
        try {
            ObjectOutputStream o =
                    new ObjectOutputStream(buf);
            o.writeObject(sc);
            ObjectInputStream in =
                    new ObjectInputStream(
                            new ByteArrayInputStream(buf.toByteArray()));
            SerialCtl ctl = (SerialCtl) in.readObject();
            System.out.println("After:::::\n" + ctl);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```
>上面的操作，就是为了处理transient的问题。
> 
>若准备通过默认机制写入对象的非 transient 部分，那么必须调用defaultWriteObject()，令其作为writeObject()中的第一个操作；并调用defaultReadObject()，令其作为 readObject()的第一个操作。这些都是不常见的调用方法。举个例子来说，当我们为一个ObjectOutputStream 调用defaultWriteObject()的时候，而且没有为其传递参数，就需要采取这种操作，使其知道对象的句柄以及如何写入所有非transient的部分。这种做法非常不便。

##第 11 章 运行期类型鉴定
运行期类型鉴定（RTTI）的概念初看非常简单——手上只有基础类型的一个句柄时，利用它判断一个对象的正确类型。
一种是“传统”RTTI，它假定我们已在编译和运行期拥有所有类型；
另一种是 Java1.1特有的“反射”机制，利用它可在运行期独立查找类信息。
###对 RTTI 的需要
####Class 对象
事实上，我们要用Class 对象创建属于某个类的全部“常规”或“普通”对象。
#####1. 类标记
在Java 1.1 中，可以采用第二种方式来产生 Class对象的句柄：使用“类标记”。对上述程序来说，看起来就象下面这样：
Gum.class;
这样做不仅更加简单，而且更安全，因为它会在编译期间得到检查。由于它取消了对方法调用的需要，所以执行的效率也会更高。
实战：：
```groovy
package rtti11;

class Candy {
    static {
        System.out.println("Loading Candy");
    }
}

class Gum {
    static {
        System.out.println("Loading Gum");
    }
}

class Cookie {
    static {
        System.out.println("Loading Cookie");
    }
}

public class SweetShop {
    public static void main(String[] args) {
        System.out.println("inside main");
        new Candy();
        System.out.println("After create Candy");
        try {
            Class.forName("rtti11.Gum");//问题是此处需要带有包名，才知道找的是哪个类
            //为获得 Class的一个句柄，一个办法是使用forName()。
            // 它的作用是取得包含了目标类文本名字的一个 String（注意拼写和大小写）。
            // 最后返回的是一个Class 句柄。
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        System.out.println("After Class.forName(\"Gum\")");
        new Cookie();
        System.out.println("After creating Cookie");
    }

}

```
类型|
----|
boolean.class	| Boolean.TYPE
char.class	|  Character.TYPE
byte.class	|  Byte.TYPE
short.class |	Short.TYPE
int.class	|  Integer.TYPE
long.class	|  Long.TYPE
float.class	| Float.TYPE
double.class	| Double.TYPE
void.class	|  Void.TYPE

####造型前的检查
迄今为止，我们已知的 RTTI 形式包括：
(1) 经典造型，如"(Shape)"，它用 RTTI 确保造型的正确性，并在遇到一个失败的造型后产生一个ClassCastException 违例。
(2) 代表对象类型的Class 对象。可查询Class 对象，获取有用的运行期资料。
###RTTI 语法
获得指向适当 Class对象的的一个句柄：：
1.一个办法是用一个字串以及Class.forName()方法。
2.如果已有了它的一个对象，那么为了取得Class 句柄，可调用属于 Object根类一部分的一个方法：getClass()。它的作用是返回一个特定的 Class句柄，用来表示对象的实际类型。

实战：：
```groovy
package rtti11;

interface HasBatteries {
}

interface Waterproof {
}

interface ShootsThinks {
}

class Toy {
    Toy() {
    }

    Toy(int i) {
    }
}

class FancyToy extends Toy implements HasBatteries, Waterproof, ShootsThinks {
    FancyToy() {
        super(1);
    }
}

public class ToyTest {
    public static void main(String[] args) {
        Class c = null;
        try {
            c = Class.forName("rtti11.FancyToy");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        printInfo(c);//打印类本身

        Class[] faces = c.getInterfaces();//找到实现的所有接口
        for (int i = 0; i < faces.length; i++) {
            printInfo(faces[i]);
        }

        Class cy = c.getSuperclass();//父类啊
        Object o = null;
        try {
            o = cy.newInstance();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
        printInfo(o.getClass());

    }

    static void printInfo(Class cc) {
        System.out.println("Class name: " + cc.getName() +
                " is interface? [" + cc.isInterface() + "]");
        //cc.isInterface()就是说cc是否是接口
    }
}

```
分析：：
Class.getInterfaces 方法会返回Class 对象的一个数组，用于表示包含在 Class 对象内的接口。
若从表面看，Class 的 newInstance()方法似乎是克隆（clone()）一个对象的另一种手段。
cy只是一个Class 句柄，编译期间并不知道进一步的类型信息。一旦新建了一个实例后，可以得到Object句柄。
###11.3 反射：运行期类信息
编译器必须明确知道 RTTI 要处理的所有类。
在运行期查询类信息的另一个原动力是通过网络创建与执行位于远程系统上的对象。
这就叫作“远程方法调用”（RMI），它允许 Java 程序（版本 1.1以上）使用由多台机器发布或分布的对象。
所以 RTTI 和“反射”之间唯一的区别就是对RTTI 来说，编译器会在编译期打开和检查.class文件
####一个类方法提取器