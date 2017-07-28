C语言教程(简单的学习c语言,第五篇)
##前言
这一篇是C语言中输入输出，文件处理。
##任务
了解C语言中输入输出，文件处理。
这里的文件和java里面的文件，意义是不一样的。这里的文件是所有的设备。
##输入 & 输出
**输入**，这意味着要向程序填充一些数据。输入可以是以文件的形式或从命令行中进行。C 语言提供了一系列内置的函数来读取给定的输入，并根据需要填充到程序中。
**输出**，这意味着要在屏幕上、打印机上或任意文件中显示一些数据。C 语言提供了一系列内置的函数来输出数据到计算机屏幕上和保存数据到文本文件或二进制文件中。
#####标准文件
**C 语言把所有的设备都当作文件。**
所以设备（比如显示器）被处理的方式与文件相同。以下三个文件会在程序执行时自动打开，以便访问键盘和屏幕。

标准文件 |	文件指针	|	设备
-----|
标准输入	|	stdin	|	键盘
标准输出	|	stdout	|	屏幕
标准错误	|	stderr	|	您的屏幕
文件指针是访问文件的方式，本节将讲解如何从屏幕读取值以及如何把结果输出到屏幕上。
C 语言中的 I/O (输入/输出) 通常使用 **printf()** 和 **scanf()** 两个函数。
**scanf() **函数用于从标准输入（键盘）读取并格式化，** printf() **函数发送格式化输出到标准输出（屏幕）。
```groovy
#include <stdio.h>      // 执行 printf() 函数需要该库
int main()
{
    printf("菜鸟教程");  //显示引号中的内容
    return 0;
}
```
编译以上程序，输出结果为：
> 菜鸟教程

实例解析：
- 所有的 C 语言程序都需要包含 main() 函数。 代码从 main() 函数开始执行。
- printf() 用于格式化输出到屏幕。printf() 函数在 "stdio.h" 头文件中声明。
- stdio.h 是一个头文件 (标准输入输出头文件) and #include 是一个预处理命令，用来引入头文件。 当编译器遇到 printf() 函数时，如果没有找到 stdio.h 头文件，会发生编译错误。
- return 0; 语句用于表示退出程序。

```groovy
%d 格式化输出整数
#include <stdio.h>
int main()
{
    int testInteger = 5;
    printf("Number = %d", testInteger);
    return 0;
}
```
编译以上程序，输出结果为：
> Number = 5

在 printf() 函数的引号中使用 "%d" (整型) 来匹配整型变量 testInteger 并输出到屏幕。
```groovy
%f 格式化输出浮点型数据
#include <stdio.h>
int main()
{
    float f;
    printf("Enter a number: ");
    // %f 匹配浮点型数据
    scanf("%f",&f);
    printf("Value = %f", f);
    return 0;
}
```
#####getchar() & putchar() 函数
**int getchar(void) **函数从屏幕读取下一个可用的字符，并把它返回为一个整数。这个函数在同一个时间内只会读取一个单一的字符。您可以在循环内使用这个方法，以便从屏幕上读取多个字符。
**int putchar(int c)** 函数把字符输出到屏幕上，并返回相同的字符。这个函数在同一个时间内只会输出一个单一的字符。您可以在循环内使用这个方法，以便在屏幕上输出多个字符。
请看下面的实例：
```groovy
#include <stdio.h>

int main( )
{
   int c;

   printf( "Enter a value :");
   c = getchar( );

   printf( "\nYou entered: ");
   putchar( c );
   printf( "\n");
   return 0;
}
```
当上面的代码被编译和执行时，它会等待您输入一些文本，当您输入一个文本并按下回车键时，程序会继续并只会读取一个单一的字符，显示如下：
```groovy
$./a.out
Enter a value :runoob

You entered: r
```
#####gets() & puts() 函数
**char \*gets(char \*s) **函数从 **stdin** 读取一行到 **s** 所指向的缓冲区，直到一个终止符或 EOF。
**int puts(const char *s)** 函数把字符串 **s** 和一个尾随的换行符写入到 **stdout**。
实例：
```groovy
#include <stdio.h>

int main( )
{
   char str[100];

   printf( "Enter a value :");
   gets( str );

   printf( "\nYou entered: ");
   puts( str );
   return 0;
}
```
当上面的代码被编译和执行时，它会等待您输入一些文本，当您输入一个文本并按下回车键时，程序会继续并读取一整行直到该行结束，显示如下：
```groovy
$./a.out
Enter a value :runoob

You entered: runoob
```
#####scanf() 和 printf() 函数
**int  scanf(const char \*format, ...)** 函数从标准输入流 **stdin** 读取输入，并根据提供的 **format** 来浏览输入。
**int printf(const char *format, ...) **函数把输出写入到标准输出流 **stdout** ，并根据提供的格式产生输出。

**format** 可以是一个简单的常量字符串，但是您可以分别指定 %s、%d、%c、%f 等来输出或读取字符串、整数、字符或浮点数。还有许多其他可用的格式选项，可以根据需要使用。如需了解完整的细节，可以查看这些函数的参考手册。现在让我们通过下面这个简单的实例来加深理解：
```groovy
#include <stdio.h>
int main( ) {

   char str[100];
   int i;

   printf( "Enter a value :");
   scanf("%s %d", str, &i);

   printf( "\nYou entered: %s %d ", str, i);
   printf("\n");
   return 0;
}
```
当上面的代码被编译和执行时，它会等待您输入一些文本，当您输入一个文本并按下回车键时，程序会继续并读取输入，显示如下：
```groovy
$./a.out
Enter a value :runoob 123

You entered: runoob 123
```
> 注意事项：
> 在这里，应当指出的是，scanf() 期待输入的格式与您给出的 %s 和 %d 相同，这意味着您必须提供有效的输入，比如 "string integer"，如果您提供的是 "string string" 或 "integer integer"，它会被认为是错误的输入。
> 另外，在读取字符串时，只要遇到一个空格，scanf() 就会停止读取，所以 "this is test" 对 scanf() 来说是三个字符串。

##文件读写
一个文件，无论它是文本文件还是二进制文件，都是代表了一系列的字节。C 语言不仅提供了访问顶层的函数，也提供了底层（OS）调用来处理存储设备上的文件。本章将讲解文件管理的重要调用。
#####打开文件
您可以使用 **fopen( )** 函数来创建一个新的文件或者打开一个已有的文件，这个调用会初始化类型 FILE 的一个对象，类型 **FILE** 包含了所有用来控制流的必要的信息。下面是这个函数调用的原型：
> FILE *fopen( const char * filename, const char * mode );

在这里，**filename** 是字符串，用来命名文件，访问模式 **mode** 的值可以是下列值中的一个：
模式	|	描述
----|
r	|	打开一个已有的文本文件，允许读取文件。
w	|	打开一个文本文件，允许写入文件。如果文件不存在，则会创建一个新文件。在这里，您的程序会从文件的开头写入内容。
a	|	打开一个文本文件，以追加模式写入文件。如果文件不存在，则会创建一个新文件。在这里，您的程序会在已有的文件内容中追加内容。
r+	|	打开一个文本文件，允许读写文件。
w+	|	打开一个文本文件，允许读写文件。如果文件已存在，则文件会被截断为零长度，如果文件不存在，则会创建一个新文件。
a+	|	打开一个文本文件，允许读写文件。如果文件不存在，则会创建一个新文件。读取会从文件的开头开始，写入则只能是追加模式。
如果处理的是二进制文件，则需使用下面的访问模式来取代上面的访问模式：
> "rb", "wb", "ab", "rb+", "r+b", "wb+", "w+b", "ab+", "a+b"

#####关闭文件
为了关闭文件，请使用 fclose( ) 函数。函数的原型如下：
>  int fclose( FILE *fp );

如果成功关闭文件，**fclose( )** 函数返回零，如果关闭文件时发生错误，函数返回 **EOF**。这个函数实际上，会清空缓冲区中的数据，关闭文件，并释放用于该文件的所有内存。**EOF** 是一个定义在头文件 **stdio.h **中的常量。
C 标准库提供了各种函数来按字符或者以固定长度字符串的形式读写文件。
#####写入文件
下面是把字符写入到流中的最简单的函数：
> int fputc( int c, FILE *fp );

函数** fputc() **把参数 c 的字符值写入到 fp 所指向的输出流中。如果写入成功，它会返回写入的字符，如果发生错误，则会返回 **EOF**。您可以使用下面的函数来把一个以 null 结尾的字符串写入到流中：
> int fputs( const char *s, FILE *fp );
> int fprintf(FILE *fp,const char *format, ...)//也是可以的。

函数** fputs()** 把字符串 **s **写入到 fp 所指向的输出流中。如果写入成功，它会返回一个非负值，如果发生错误，则会返回** EOF**。您也可以使用 **int fprintf(FILE *fp,const char *format, ...)** 函数来写把一个字符串写入到文件中。尝试下面的实例：
> 注意：请确保您有可用的** /tmp** 目录，如果不存在该目录，则需要在您的计算机上先创建该目录。

实战：
```groovy
#include <stdio.h>

int main()
{
   FILE *fp = NULL;

   fp = fopen("/tmp/test.txt", "w+");//打开文件

   //下面两个都是写入。
   fprintf(fp, "This is testing for fprintf...\n");
   fputs("This is testing for fputs...\n", fp);

   fclose(fp);//关闭文件
}
```
当上面的代码被编译和执行时，它会在 /tmp 目录中创建一个新的文件 test.txt，并使用两个不同的函数写入两行。接下来让我们来读取这个文件。
#####读取文件
下面是从文件读取单个字符的最简单的函数：
> int fgetc( FILE * fp );

**fgetc()** 函数从 fp 所指向的输入文件中读取一个字符。
返回值是读取的字符，如果发生错误则返回 EOF。下面的函数允许您从流中读取一个字符串：
> char *fgets( char *buf, int n, FILE *fp );
>  int fscanf(FILE *fp, const char *format, ...)
>  上面方法的区别是：
>  第一个方法到一定长度或者到换位符停止
>  第二个方法到第一个空格就停止了。
 
函数 **fgets()** 从 fp 所指向的输入流中读取 n - 1 个字符。它会把读取的字符串复制到缓冲区 **buf**，并在最后追加一个 **null** 字符来终止字符串。
如果这个函数在读取最后一个字符之前就遇到一个换行符 '\n' 或文件的末尾 EOF，则只会返回读取到的字符，包括换行符。
您也可以使用 int fscanf(FILE *fp, const char *format, ...) 函数来从文件中读取字符串，但是在遇到第一个空格字符时，它会停止读取。
```groovy
#include <stdio.h>
 
int main()
{
   FILE *fp = NULL;
   char buff[255];

   fp = fopen("/tmp/test.txt", "r");
   fscanf(fp, "%s", buff);
   printf("1 : %s\n", buff );

   fgets(buff, 255, (FILE*)fp);
   printf("2: %s\n", buff );

   fgets(buff, 255, (FILE*)fp);
   printf("3: %s\n", buff );
   fclose(fp);

}
```
当上面的代码被编译和执行时，它会读取上一部分创建的文件，产生下列结果：
```groovy
1 : This
2: is testing for fprintf...

3: This is testing for fputs...
```
首先，**fscanf()** 方法只读取了** This**，因为它在后边遇到了一个空格。其次，调用 fgets() 读取剩余的部分，直到行尾。最后，调用 fgets() 完整地读取第二行。
######二进制 I/O 函数
下面两个函数用于二进制输入和输出：
```groovy
size_t fread(void *ptr, size_t size_of_elements, size_t number_of_elements, FILE *a_file);

size_t fwrite(const void *ptr, size_t size_of_elements,  size_t number_of_elements, FILE *a_file);
```
##预处理器
**C 预处理器**不是编译器的组成部分，但是它是编译过程中一个单独的步骤。简言之，C 预处理器只不过是一个文本替换工具而已，它们会指示编译器在实际编译之前完成所需的预处理。我们将把 C 预处理器（C Preprocessor）简写为 CPP。
所有的预处理器命令都是以井号（#）开头。它必须是第一个非空字符，为了增强可读性，预处理器指令应从第一列开始。下面列出了所有重要的预处理器指令：
指令	|	描述
-----|
#define	|	定义宏
#include	|	包含一个源代码文件
#undef	|	取消已定义的宏
#ifdef	|	如果宏已经定义，则返回真
#ifndef		|   如果宏没有定义，则返回真
#if	  |  如果给定条件为真，则编译下面代码
#else	|	#if 的替代方案
#elif	|	如果前面的 #if 给定条件不为真，当前条件为真，则编译下面代码
#endif	|	结束一个 #if……#else 条件编译块
#error	|	当遇到标准错误时，输出错误消息
#pragma	|	使用标准化方法，向编译器发布特殊的命令到编译器中
#####预处理器实例
分析下面的实例来理解不同的指令。
> \#define MAX_ARRAY_LENGTH 20

这个指令告诉 CPP 把所有的 MAX_ARRAY_LENGTH 替换为 20。使用 #define 定义常量来增强可读性。
> \#include <stdio.h>
\#include "myheader.h"

这些指令告诉 CPP 从系统库中获取 stdio.h，并添加文本到当前的源文件中。
下一行告诉 CPP 从本地目录中获取 myheader.h，并添加内容到当前的源文件中。
>\#undef  FILE_SIZE
\#define FILE_SIZE 42

这个指令告诉 CPP 取消已定义的 FILE_SIZE，并定义它为 42。
>\#ifndef MESSAGE
    \#define MESSAGE "You wish!"
\#endif

这个指令告诉 CPP 只有当 MESSAGE 未定义时，才定义 MESSAGE。
>\#ifdef DEBUG
   /\* Your debugging statements here */
\#endif

这个指令告诉 CPP 如果定义了 DEBUG，则执行处理语句。在编译时，如果您向 gcc 编译器传递了 -DDEBUG 开关量，这个指令就非常有用。它定义了 DEBUG，您可以在编译期间随时开启或关闭调试。

#####预定义宏
ANSI C 定义了许多宏。在编程中您可以使用这些宏，但是不同直接修改这些预定义的宏。
宏	| 描述
-----|
\__DATE__	|当前日期，一个以 "MMM DD YYYY" 格式表示的字符常量。
\__TIME__	|当前时间，一个以 "HH:MM:SS" 格式表示的字符常量。
\__FILE__	|这会包含当前文件名，一个字符串常量。
\__LINE__	|这会包含当前行号，一个十进制常量。
\__STDC__	|当编译器以 ANSI 标准编译时，则定义为 1。
让我们来尝试下面的实例：
```GROOVY
#include <stdio.h>

main()
{
   printf("File :%s\n", __FILE__ );
   printf("Date :%s\n", __DATE__ );
   printf("Time :%s\n", __TIME__ );
   printf("Line :%d\n", __LINE__ );
   printf("ANSI :%d\n", __STDC__ );

}
```
当上面的代码（在文件 test.c 中）被编译和执行时，它会产生下列结果：
```groovy
File :test.c
Date :Jun 2 2012
Time :03:36:24
Line :8
ANSI :1
```
##预处理器运算符
C 预处理器提供了下列的运算符来帮助您创建宏：
**宏延续运算符（\）**
一个宏通常写在一个单行上。但是如果宏太长，一个单行容纳不下，则使用宏延续运算符（\）。例如：
> \#define  message_for(a, b)  \
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;printf(#a " and " #b ": We love you!\n")

**字符串常量化运算符（#）**
在宏定义中，当需要把一个宏的参数转换为字符串常量时，则使用字符串常量化运算符（#）。在宏中使用的该运算符有一个特定的参数或参数列表。例如：
```groovy
#include <stdio.h>

#define  message_for(a, b)  \
    printf(#a " and " #b ": We love you!\n")

int main(void)
{
   message_for(Carole, Debra);
   return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
> Carole and Debra: We love you!

**标记粘贴运算符（##）**
宏定义内的标记粘贴运算符（##）会合并两个参数。它允许在宏定义中两个独立的标记被合并为一个标记。例如：
```groovy
#include <stdio.h>

#define tokenpaster(n) printf ("token" #n " = %d", token##n)

int main(void)
{
   int token34 = 40;

   tokenpaster(34);
   return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
> token34 = 40

这是怎么发生的，因为这个实例会从编译器产生下列的实际输出：
>printf ("token34 = %d", token34);
>一个#是占位的感觉，两个##是拼接

这个实例演示了 token##n 会连接到 token34 中，在这里，我们使用了**字符串常量化运算符（#）**和**标记粘贴运算符（##）**。
**defined() 运算符**
预处理器 defined 运算符是用在常量表达式中的，用来确定一个标识符是否已经使用 #define 定义过。如果指定的标识符已定义，则值为真（非零）。如果指定的标识符未定义，则值为假（零）。下面的实例演示了 defined() 运算符的用法：
```groovy
#include <stdio.h>

#if !defined (MESSAGE)
   #define MESSAGE "You wish!"
#endif

int main(void)
{
   printf("Here is the message: %s\n", MESSAGE);  
   return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
> Here is the message: You wish!

#####参数化的宏
CPP 一个强大的功能是可以使用参数化的宏来模拟函数。例如，下面的代码是计算一个数的平方：
```groovy
int square(int x) {
   return x * x;
}
```
我们可以使用宏重写上面的代码，如下：
```groovy
#define square(x) ((x) * (x))
```
在使用带有参数的宏之前，必须使用** #define** 指令定义。参数列表是括在圆括号内，且必须紧跟在宏名称的后边。宏名称和左圆括号之间不允许有空格。例如：
```groovy
#include <stdio.h>

#define MAX(x,y) ((x) > (y) ? (x) : (y))

int main(void)
{
   printf("Max between 20 and 10 is %d\n", MAX(10, 20));  
   return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
> Max between 20 and 10 is 20

##头文件
头文件是扩展名为.h的文件，包含了C函数声明和宏定义，被多个源文件中引用共享。有两种类型的头文件：程序员编写的头文件和编译器自带的头文件。
在程序中要使用头文件，需要使用 C 预处理指令 #include 来引用它。前面我们已经看过 stdio.h 头文件，它是编译器自带的头文件。
引用头文件相当于复制头文件的内容，但是我们不会直接在源文件中复制头文件的内容，因为这么做很容易出错，特别在程序是由多个源文件组成的时候。
A simple practice in C 或 C++ 程序中，建议把所有的常量、宏、系统全局变量和函数原型写在头文件中，在需要的时候随时引用这些头文件。
#####引用头文件的语法
使用预处理指令 #include 可以引用用户和系统头文件。它的形式有以下两种：
> \#include <file>

这种形式用于引用系统头文件。它在系统目录的标准列表中搜索名为 file 的文件。在编译源代码时，您可以通过 -I 选项把目录前置在该列表前。
>\#include "file"

这种形式用于引用用户头文件。它在包含当前文件的目录中搜索名为 file 的文件。在编译源代码时，您可以通过 -I 选项把目录前置在该列表前。
#####引用头文件的操作
**\#include** 指令会指示 C 预处理器浏览指定的文件作为输入。预处理器的输出包含了已经生成的输出，被引用文件生成的输出以及 **#include** 指令之后的文本输出。例如，如果您有一个头文件 header.h，如下：
```groovy
char *test (void);
```
和一个使用了头文件的主程序 program.c，如下：
```groovy
int x;
#include "header.h"

int main (void)
{
   puts (test ());
}
```
编译器会看到如下的令牌流：
```groovy
int x;
char *test (void);

int main (void)
{
   puts (test ());
}
```
#####只引用一次头文件
如果一个头文件被引用两次，编译器会处理两次头文件的内容，这将产生错误。为了防止这种情况，标准的做法是把文件的整个内容放在条件编译语句中，如下：
```groovy
#ifndef HEADER_FILE
#define HEADER_FILE

the entire header file file

#endif
```
这种结构就是通常所说的包装器 **#ifndef**。当再次引用头文件时，条件为假，因为 HEADER_FILE 已定义。此时，预处理器会跳过文件的整个内容，编译器会忽略它。
#####有条件引用
有时需要从多个不同的头文件中选择一个引用到程序中。例如，需要指定在不同的操作系统上使用的配置参数。您可以通过一系列条件来实现这点，如下：
```groovy
#if SYSTEM_1
   # include "system_1.h"
#elif SYSTEM_2
   # include "system_2.h"
#elif SYSTEM_3
   ...
#endif
```
但是如果头文件比较多的时候，这么做是很不妥当的，预处理器使用宏来定义头文件的名称。这就是所谓的**有条件引用**。它不是用头文件的名称作为** #include** 的直接参数，您只需要使用宏名称代替即可：
```groovy
 #define SYSTEM_H "system_1.h"
 ...
 #include SYSTEM_H
```
SYSTEM_H 会扩展，预处理器会查找 system_1.h，就像 **#include** 最初编写的那样。SYSTEM_H 可通过 -D 选项被您的 Makefile 定义。
##强制类型转换
强制类型转换是把变量从一种类型转换为另一种数据类型。例如，如果您想存储一个 long 类型的值到一个简单的整型中，您需要把 long 类型强制转换为 int 类型。您可以使用强制类型转换运算符来把值显式地从一种类型转换为另一种类型，如下所示：
> (type_name) expression

请看下面的实例，使用强制类型转换运算符把一个整数变量除以另一个整数变量，得到一个浮点数：
```groovy
#include <stdio.h>

main()
{
   int sum = 17, count = 5;
   double mean;

   mean = (double) sum / count;
   printf("Value of mean : %f\n", mean );

}
```
当上面的代码被编译和执行时，它会产生下列结果：
> Value of mean : 3.400000
> 
> 这里要注意的是强制类型转换运算符的优先级大于除法，因此 sum 的值首先被转换为 double 型，然后除以 count，得到一个类型为 double 的值。

类型转换可以是隐式的，由编译器自动执行，也可以是显式的，通过使用强制类型转换运算符来指定。在编程时，有需要类型转换的时候都用上强制类型转换运算符，是一种良好的编程习惯。
######整数提升
整数提升是指把小于 int 或 unsigned int 的整数类型转换为 int 或 unsigned int 的过程。请看下面的实例，在 int 中添加一个字符：
```groovy
#include <stdio.h>

main()
{
   int  i = 17;
   char c = 'c'; /* ascii 值是 99 */
   int sum;

   sum = i + c;
   printf("Value of sum : %d\n", sum );

}
```
当上面的代码被编译和执行时，它会产生下列结果：
> Value of sum : 116

在这里，sum 的值为 116，因为编译器进行了整数提升，在执行实际加法运算时，把 'c' 的值转换为对应的 ascii 值。
#####常用的算术转换
常用的算术转换是隐式地把值强制转换为相同的类型。编译器首先执行整数提升，如果操作数类型不同，则它们会被转换为下列层次中出现的最高层次的类型：
![](c51.png)
常用的算术转换不适用于赋值运算符、逻辑运算符 && 和 ||。让我们看看下面的实例来理解这个概念：
```groovy
#include <stdio.h>

main()
{
   int  i = 17;
   char c = 'c'; /* ascii 值是 99 */
   float sum;

   sum = i + c;
   printf("Value of sum : %f\n", sum );

}
```
当上面的代码被编译和执行时，它会产生下列结果：
```groovy
Value of sum : 116.000000
```
在这里，c 首先被转换为整数，但是由于最后的值是 double 型的，所以会应用常用的算术转换，编译器会把 i 和 c 转换为浮点型，并把它们相加得到一个浮点数。
##错误处理
C 语言不提供对错误处理的直接支持，但是作为一种系统编程语言，它以返回值的形式允许您访问底层数据。在发生错误时，大多数的 C 或 UNIX 函数调用返回 1 或 NULL，同时会设置一个错误代码 errno，该错误代码是全局变量，表示在函数调用期间发生了错误。您可以在 <error.h> 头文件中找到各种各样的错误代码。
所以，C 程序员可以通过检查返回值，然后根据返回值决定采取哪种适当的动作。开发人员应该在程序初始化时，把 errno 设置为 0，这是一种良好的编程习惯。0 值表示程序中没有错误。
#####errno、perror() 和 strerror()
C 语言提供了 **perror()** 和 **strerror()** 函数来显示与 errno 相关的文本消息。
- **perror()** 函数显示您传给它的字符串，后跟一个冒号、一个空格和当前 errno 值的文本表示形式。
- **strerror()** 函数，返回一个指针，指针指向当前 errno 值的文本表示形式。
让我们来模拟一种错误情况，尝试打开一个不存在的文件。您可以使用多种方式来输出错误消息，在这里我们使用函数来演示用法。另外有一点需要注意，您应该使用 **stderr** 文件流来输出所有的错误。
```groovy
\#include <stdio.h>
\#include <errno.h>
\#include <string.h>
extern int errno ;
int main ()
{
   FILE * pf;
   int errnum;
   pf = fopen ("unexist.txt", "rb");
   if (pf == NULL)
   {
      errnum = errno;
      fprintf(stderr, "错误号: %d\n", errno);
      perror("通过 perror 输出错误");
      fprintf(stderr, "打开文件错误: %s\n", strerror( errnum ));
   }
   else
   {
      fclose (pf);
   }
   return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```groovy
错误号: 2
通过 perror 输出错误: No such file or directory
打开文件错误: No such file or directory
//----------------
注意：
被stderr弄蒙圈了。其实就是标准错误输出。
```
#####被零除的错误
在进行除法运算时，如果不检查除数是否为零，则会导致一个运行时错误。
为了避免这种情况发生，下面的代码在进行除法运算前会先检查除数是否为零：
```groovy
\#include <stdio.h>
\#include <stdlib.h>
main()
{
   int dividend = 20;
   int divisor = 0;
   int quotient;

   if( divisor == 0){
      fprintf(stderr, "除数为 0 退出运行...\n");
      exit(-1);
   }
   quotient = dividend / divisor;
   fprintf(stderr, "quotient 变量的值为 : %d\n", quotient );

   exit(0);
}
```
当上面的代码被编译和执行时，它会产生下列结果：
> 除数为 0 退出运行...

#####程序退出状态
通常情况下，程序成功执行完一个操作正常退出的时候会带有值 EXIT_SUCCESS。在这里，EXIT_SUCCESS 是宏，它被定义为 0。
如果程序中存在一种错误情况，当您退出程序时，会带有状态值 EXIT_FAILURE，被定义为 -1。所以，上面的程序可以写成：
```groovy
#include <stdio.h>
#include <stdlib.h>

main()
{
   int dividend = 20;
   int divisor = 5;
   int quotient;
 
   if( divisor == 0){
      fprintf(stderr, "除数为 0 退出运行...\n");
      exit(EXIT_FAILURE);
   }
   quotient = dividend / divisor;
   fprintf(stderr, "quotient 变量的值为: %d\n", quotient );

   exit(EXIT_SUCCESS);
}
```
当上面的代码被编译和执行时，它会产生下列结果：
> quotient 变量的值为 : 4

##递归
递归指的是在函数的定义中使用函数自身的方法。
语法格式如下：
```groovy
void recursion()
{
   recursion(); /* 函数调用自身 */
}

int main()
{
   recursion();
}
```
C 语言支持递归，即一个函数可以调用其自身。但在使用递归时，程序员需要注意定义一个从函数退出的条件，否则会进入死循环。
递归函数在解决许多数学问题上起了至关重要的作用，比如计算一个数的阶乘、生成斐波那契数列，等等。
#####数的阶乘
下面的实例使用递归函数计算一个给定的数的阶乘：
```groovy
#include <stdio.h>
 
double factorial(unsigned int i)
{
   if(i <= 1)
   {
      return 1;
   }
   return i * factorial(i - 1);
}
int  main()
{
    int i = 15;
    printf("%d 的阶乘为 %f\n", i, factorial(i));
    return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
> 15 的阶乘为 1307674368000.000000

#####斐波那契数列
下面的实例使用递归函数生成一个给定的数的斐波那契数列：
```groovy
#include <stdio.h>

int fibonaci(int i)
{
   if(i == 0)
   {
      return 0;
   }
   if(i == 1)
   {
      return 1;
   }
   return fibonaci(i-1) + fibonaci(i-2);
}

int  main()
{
    int i;
    for (i = 0; i < 10; i++)
    {
       printf("%d\t\n", fibonaci(i));
    }
    return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```groovy
0
1
1
2
3
5
8
13
21
34
```
#####可变参数
有时，您可能会碰到这样的情况，您希望函数带有可变数量的参数，而不是预定义数量的参数。C 语言为这种情况提供了一个解决方案，它允许您定义一个函数，能根据具体的需求接受可变数量的参数。下面的实例演示了这种函数的定义。
```groovy
int func(int, ... ) 
{
   .
   .
   .
}

int main()
{
   func(1, 2, 3);
   func(1, 2, 3, 4);
}
```
请注意，函数** func()** 最后一个参数写成省略号，即三个点号（...），省略号之前的那个参数总是 **int**，代表了要传递的可变参数的总数。为了使用这个功能，您需要使用 **stdarg.h** 头文件，该文件提供了实现可变参数功能的函数和宏。具体步骤如下：
- 定义一个函数，最后一个参数为省略号，省略号前面的那个参数总是 int，表示了参数的个数。
- 在函数定义中创建一个 va_list 类型变量，该类型是在 stdarg.h 头文件中定义的。
- 使用 int 参数和 va_start 宏来初始化 va_list 变量为一个参数列表。宏 va_start 是在 stdarg.h 头文件中定义的。
- 使用 va_arg 宏和 va_list 变量来访问参数列表中的每个项。
- 使用宏 va_end 来清理赋予 va_list 变量的内存。

现在让我们按照上面的步骤，来编写一个带有可变数量参数的函数，并返回它们的平均值：
```groovy
#include <stdio.h>
#include <stdarg.h>

double average(int num,...)
{

    va_list valist;
    double sum = 0.0;
    int i;

    /* 为 num 个参数初始化 valist */
    va_start(valist, num);

    /* 访问所有赋给 valist 的参数 */
    for (i = 0; i < num; i++)
    {
       sum += va_arg(valist, int);
    }
    /* 清理为 valist 保留的内存 */
    va_end(valist);

    return sum/num;
}

int main()
{
   printf("Average of 2, 3, 4, 5 = %f\n", average(4, 2,3,4,5));
   printf("Average of 5, 10, 15 = %f\n", average(3, 5,10,15));
}
```
当上面的代码被编译和执行时，它会产生下列结果。应该指出的是，函数 **average()** 被调用两次，每次第一个参数都是表示被传的可变参数的总数。省略号被用来传递可变数量的参数。
```groovy
Average of 2, 3, 4, 5 = 3.500000
Average of 5, 10, 15 = 10.000000
```
##内存管理
本章将讲解 C 中的动态内存管理。C 语言为内存的分配和管理提供了几个函数。这些函数可以在 `<stdlib.h> `头文件中找到。
序号	|	函数和描述
----|
1	|	void *calloc(int num, int size);<br>在内存中动态地分配 num 个长度为 size 的连续空间，并将每一个字节都初始化为 0。所以它的结果是分配了 num*size 个字节长度的内存空间，并且每个字节的值都是0。
2	|	void free(void *address); <br>该函数释放 address 所指向的内存块,释放的是动态分配的内存空间。
3	|	void *malloc(int num);<br>在堆区分配一块指定大小的内存空间，用来存放数据。这块内存空间在函数执行完成后不会被初始化，它们的值是未知的。
4	|	void *realloc(void *address, int newsize); <br>该函数重新分配内存，把内存扩展到 newsize。
#####动态分配内存
编程时，如果您预先知道数组的大小，那么定义数组时就比较容易。例如，一个存储人名的数组，它最多容纳 100 个字符，所以您可以定义数组，如下所示：
> char name[100];

但是，如果您预先不知道需要存储的文本长度，例如您向存储有关一个主题的详细描述。在这里，我们需要定义一个指针，该指针指向未定义所学内存大小的字符，后续再根据需求来分配内存，如下所示：
```groovy
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main()
{
   char name[100];
   char *description;

   strcpy(name, "Zara Ali");

   /* 动态分配内存 */
   description = malloc( 200 * sizeof(char) );
   if( description == NULL )
   {
      fprintf(stderr, "Error - unable to allocate required memory\n");
   }
   else
   {
      strcpy( description, "Zara ali a DPS student in class 10th");
   }
   printf("Name = %s\n", name );
   printf("Description: %s\n", description );
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```groovy
Name = Zara Ali
Description: Zara ali a DPS student in class 10th
```
上面的程序也可以使用** calloc()** 来编写，只需要把 malloc 替换为 calloc 即可，如下所示：
> calloc(200, sizeof(char));

当动态分配内存时，您有完全控制权，可以传递任何大小的值。而那些预先定义了大小的数组，一旦定义则无法改变大小。
#####重新调整内存的大小和释放内存
当程序退出时，操作系统会自动释放所有分配给程序的内存，但是，建议您在不需要内存时，都应该调用函数 **free()** 来释放内存。
或者，您可以通过调用函数** realloc()** 来增加或减少已分配的内存块的大小。让我们使用 realloc() 和 free() 函数，再次查看上面的实例：
```groovy
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main()
{
   char name[100];
   char *description;

   strcpy(name, "Zara Ali");

   /* 动态分配内存 */
   description = malloc( 30 * sizeof(char) );
   if( description == NULL )
   {
      fprintf(stderr, "Error - unable to allocate required memory\n");
   }
   else
   {
      strcpy( description, "Zara ali a DPS student.");
   }
   /* 假设您想要存储更大的描述信息 */
   description = realloc( description, 100 * sizeof(char) );
   if( description == NULL )
   {
      fprintf(stderr, "Error - unable to allocate required memory\n");
   }
   else
   {
      strcat( description, "She is in class 10th");
   }

   printf("Name = %s\n", name );
   printf("Description: %s\n", description );

   /* 使用 free() 函数释放内存 */
   free(description);
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```groovy
Name = Zara Ali
Description: Zara ali a DPS student.She is in class 10th
```
您可以尝试一下不重新分配额外的内存，strcat() 函数会生成一个错误，因为存储 description 时可用的内存不足。
##命令行参数
执行程序时，可以从命令行传值给 C 程序。这些值被称为命令行参数，它们对程序很重要，特别是当您想从外部控制程序，而不是在代码内对这些值进行硬编码时，就显得尤为重要了。
命令行参数是使用 main() 函数参数来处理的，其中，argc 是指传入参数的个数，argv[] 是一个指针数组，指向传递给程序的每个参数。下面是一个简单的实例，检查命令行是否有提供参数，并根据参数执行相应的动作：
```groovy
#include <stdio.h>

int main( int argc, char *argv[] )  
{
   if( argc == 2 )
   {
      printf("The argument supplied is %s\n", argv[1]);
   }
   else if( argc > 2 )
   {
      printf("Too many arguments supplied.\n");
   }
   else
   {
      printf("One argument expected.\n");
   }
}
```
使用一个参数，编译并执行上面的代码，它会产生下列结果：
```groovy
$./a.out  testing
The argument supplied is testing
```
使用两个参数，编译并执行上面的代码，它会产生下列结果：
```groovy
$./a.out testing1 testing2
Too many arguments supplied.
```
不传任何参数，编译并执行上面的代码，它会产生下列结果：
```groovy
$./a.out
One argument expected
```
应当指出的是，**argv[0]** 存储程序的名称，**argv[1]** 是一个指向第一个命令行参数的指针，*argv[n] 是最后一个参数。如果没有提供任何参数，argc 将为 1，否则，如果传递了一个参数，argc 将被设置为 2。

**多个命令行参数之间用空格分隔，但是如果参数本身带有空格，那么传递参数的时候应把参数放置在双引号 "" 或单引号 '' 内部。**让我们重新编写上面的实例，有一个空间，那么你可以通过这样的观点，把它们放在双引号或单引号""""。让我们重新编写上面的实例，向程序传递一个放置在双引号内部的命令行参数：还是上面的代码，直接上输入：
```groovy
$./a.out "testing1 testing2"

The argument supplied is testing1 testing2
//就是为了说明参数内带有空格，要用单引号或者双引号括起来。
```