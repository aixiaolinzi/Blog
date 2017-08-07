Think In Java 第一篇

##前言

今天开始Java的读书笔记。这是一篇有年头的书籍。需要慢慢理解。我们要重视基础。

##任务

适应怎样阅读这本书。理解理解一下对象的入门。

##对象入门

面向对象编程（OOP）

就是说，已经有了现成的库，不需要我们自己编写对象库。而是在现在库的基础上解决问题。

###抽象的进步

所有编程语言的最终目的都是提供一种“抽象”方法。

汇编语言是对基础机器的少量抽象。

“命令式”语言（FORTRAN，BASIC和C）是对汇编语言的一种抽象。

面向对象程序设计：

1).所有数据都是对象。

2).程序是一大堆对象的组合：通过消息传递，各对象知道自己该做些什么。

3).每个对象都有自己的存贮空间，可容纳其他对象。

4).每个对象都有一种类型。

5).同一类所有对象都能接受相同的消息。

###等价与类似

等价就是我们完全能够将衍生类的一个对象换成基础类的一个对象（也可以理解为纯替换）。

类似就是新类型仍可以替换成基础类型，但这种替换并不完美，因为不可以在基础类里面访问新的函数。

###抽象的基础类和接口

愿景：不想其他任何人实际创建基础类的一个对象，只对上溯造型成它，以便使用它们的接口。

此处就是abstract关键字。

interface（接口）关键字将抽象类的概念更延伸了一步，它完全禁止了所有的函数定义。

### 对象的创建和存在时间

1.为获取最快的运行速度，存储以及存在时间可在编写程序时决定，只需将对象放置在堆栈或者静态存储区域即可。

这样便为存储空间的分配和释放提供了一个优先级。

但是我们牺牲了灵活性。

2.在内存池中动态创建对象（堆，内存堆）。

进入运行期，才能知道需要多少个对象，它们的存在时间有多长，以及准确的类型。

内存堆里面分配存储空间的时间比在堆栈里面的时间要长。

由于动态创建方法使对象本身本来就倾向于复杂，所以查找存储空间以及释放它所需的额外开销不会为对象的创建造成明显的影响。

**下面是释放**

若在堆栈或者静态空间里创建一个对象，编译器会判断对象的持续时间有多长，到时会自动“破坏”或者“清楚”它。

1.用程序化的方式决定何时破坏对象

2.或者利用由环境提供的一种“垃圾收集器”特性，自动寻找那些不在使用的对象。

####单根结构

java中的根只有一个，就是object。

####违例控制

就是在错误出现的地方抛出去。这样就不用强制的检查代码了。





###多线程

此处的多线程讲的是怎样的上锁，保持资源同步。

###分析和设计

分析问题的阶段：

阶段0：拟出一个计划

在整个过程中设置目标。有规划有计划而不是上来就写。

阶段1：要制作什么？

建立需求分析和系统规格。

阶段2：如何构建？

拿出设计方案。

阶段3：开始创建

有了计划开始干活。

阶段4：校订

就是维护。

##第二章 一切都是对象

####对象保存的地方

1.寄存器：最快的保存区域，因为它是处理器内存。

2.堆栈：RAM区域。堆栈指针若向下移，会创建新的内存，若向上移，则会释放那些内存。

3.堆：也在RAM区域，其中保存了java对象。堆最吸引人的地方就是编译器不必知道要从堆里分配多少存储空间，也不必知道存储的数据要在堆中停留多长时间。

4.静态存储：这里的静态（static）是指“位于固定位置”（也是在RAM里面）。运行期间，静态存储的数据将随时等候调用。

5.常数存储：常数值通常直接置于程序代码内部。可以考虑将他们置于只读存储器（ROM）。

6.非RAM存储：

####主要类型

高精度类型：BigInteger和BigDecimal

BigInteger支持任意精度事务整数；

BigDecimal支持任意精度的定点数字。

###绝不要清除对象

####作用域

用new关键字创建一个java对象的时候，它会超出作用域的范围之外。



##第三章 控制程序流程

###使用 Java 运算符

优先级，赋值（=），算术运算符（+，-，*，/，%），自动递增和递减，关系运算符（>,<,>=,<=,==,!=),逻辑运算符（AND(&&)，OR(||)，NOT(!)）。

按位运算符，

移位运算符：特殊（>>>）它使用了“零扩展”：无论正负，都在高位插入0。

三元if-else的使用

字串运算符+

造型运算符（Cast）：向上转型

###执行控制

逗号运算符：只有在for循环用到了逗号运算符了。

for(int i = 1, j = i + 10; i < 5;i++, j = i * 2)就是这意思。

中断和继续：break 和continue 控制循环的流程。

break 用于强行退出循环，不执行循环中剩余的语句。

continue 则停止执行当前的反复，然后退回循环起始和，开始新的反复。



标签：：

label1:

对Java 来说，唯一用到标签的地方是在循环语句之前。进一步说，它实际需要紧靠在循环语句的前方——在标签和循环之间置入任何语句都是不明智的。而在循环之前设置标签的唯一理由是：我们希望在其中嵌套另一个循环或者一个开关。这是由于 break和 continue 关键字通常只中断当前循环，但若随同标签使用，它们就会中断到存在标签的地方。

如下所示：

```groovy

label1:

外部循环{

	内部循环{

		//...

		break; //1

		//...

		continue; //2

		//...

		continue label1; //3

		//...

		break label1; //4

	}

}

```

> 

在条件1 中，break 中断内部循环，并在外部循环结束。

在条件2 中，continue 移回内部循环的起始处。

在条件3 中，continue label1 却同时中断内部循环以及外部循环，并移至label1 处。随后，它实际是继续循环，但却从外部循环开始。

条件4 中，break label1也会中断所有循环，并回到label1 处，但并不重新进入循环。



同样的规则亦适用于while：

(1) 简单的一个 continue 会退回最内层循环的开头（顶部），并继续执行。

(2) 带有标签的 continue 会到达标签的位置，并重新进入紧接在那个标签后面的循环。

(3) break 会中断当前循环，并移离当前标签的末尾。

(4) 带标签的break 会中断当前循环，并移离由那个标签指示的循环的末尾。

##第四章 初始化和清除

###构建器

构建方法时候使用的。就是构建方法。构建器的名字与类名相同。

###方法过载

我们用相同的词表达多种不同的含义——即词的“过载”。

带有参数的构建器；

Tree 既可创建成一颗种子，不含任何自变量；亦可创建成生长在苗圃中的植物。

每个过载的方法都必须采取独一无二的自变量类型列表。

**主类型的过载**

**返回值过载**

this 关键字：

1. 在构建器里调用构建器若为一个类写了多个构建器，那么经常都需要在一个构建器里调用另一个构建器，以避免写重复的代码。可用this 关键字做到这一点。

通常，当我们说this 的时候，都是指“这个对象”或者“当前对象”。而且它本身会产生当前对象的一个句柄。



>注意：

1.尽管可用this 调用一个构建器，但不可调用两个（硬性调用会报错，编译器直接报错）。

2.可用 this.s来引用成员数据。

3.在print()中，我们发现编译器不让我们从除了一个构建器之外的其他任何方法内部调用一个构建器。



static的含义：

我们不可从一个 static方法内部发出对非 static方法的调用，尽管反过来说是可以的。

除了全局函数不允许在Java中使用以外，若将一个 static方法置入一个类的内部，它就可以访问其他static 方法以及static 字段。



###收尾和垃圾收集

垃圾收集器只知道释放那些由new分配的内存，所以不知道如何释放对象的“特殊”内存。

Java 提供了一个名为finalize()的方法，可为我们的类定义它。在理想情况下，它的工作原理应该是这样的：一旦垃圾收集器准备好释放对象占用的存储空间，它首先调用finalize()，而且只有在下一次垃圾收集过程中，才会真正回收对象的内存。

####finalize()

垃圾收集只跟内存有关！

垃圾收集器会负责释放所有对象占据的内存，无论这些对象是如何创建的。

自己不必过多地使用finalize()。

finalize()最有用处的地方之一是观察垃圾收集的过程。

###成员变量
它与初始化的顺序有关，而不是与程序的编译方式有关。
初始化顺序：
在一个类里，初始化的顺序是由变量在类内的定义顺序决定的。
```groovy
class Tag {
	Tag(int marker) {
		System.out.println("Tag(" + marker + ")");
	}
}
class Card {
	Tag t1 = new Tag(1); // Before constructor
	Card() {
		// Indicate we're in the constructor:
		System.out.println("Card()");
		t3 = new Tag(33); // Re-initialize t3
	}
	Tag t2 = new Tag(2); // After constructor
	void f() {
		System.out.println("f()");
	}
	Tag t3 = new Tag(3); // At end
}

public class OrderOfInitialization {
	public static void main(String[] args) {
		Card t = new Card();
		t.f(); // Shows that construction is done
	}
} 
```
上面的运行是：
```groovy
Tag(1)
Tag(2)
Tag(3)
Card()
Tag(33)
f()
```
t3句柄会被初始化两次，一次在构建器调用前，一次在调用期间（第一个对象会被丢弃，所以它后来可被当作垃圾收掉）。
运行的顺序是：从上向下的，最后是构建函数里面的调用。
静态数据的初始化：：
但由于static 值只有一个存储区域，所以无论创建多少个对象，都必然会遇到何时对那个存储区域进行初始化的问题。
```groovy
class Bowl {
    Bowl(int marker) {
        System.out.println("Bowl(" + marker + ")");
    }

    void f(int marker) {
        System.out.println("f(" + marker + ")");
    }
}

class Table {
    static Bowl b1 = new Bowl(1);

    Table() {
        System.out.println("Table()");
        b2.f(1);
    }

    void f2(int marker) {
        System.out.println("f2(" + marker + ")");
    }

    static Bowl b2 = new Bowl(2);
}

class Cupboard {
    Bowl b3 = new Bowl(3);
    static Bowl b4 = new Bowl(4);

    Cupboard() {
        System.out.println("Cupboard()");
        b4.f(2);
    }

    void f3(int marker) {
        System.out.println("f3(" + marker + ")");
    }

    static Bowl b5 = new Bowl(5);
}

public class Garbage {

    public static void main(String args[]) {
        System.out.println("Creating new Table() in main");
        new Table();
        System.out.println("Creating new Cupboard() in main");
        new Cupboard();
        t2.f2(1);
        t3.f3(1);
    }

    static Table t2 = new Table();
    static Cupboard t3 = new Cupboard();
}

```
运行结果：
```groovy
Bowl(1)
Bowl(2)
Table()
f(1)
Bowl(4)
Bowl(5)
Bowl(3)
Cupboard()
f(2)
Creating new Table() in main
Table()
f(1)
Creating new Cupboard() in main
Bowl(3)
Cupboard()
f(2)
f2(1)
f3(1)
```
>总结：
>1.不考虑含有静态的情况。在new 一个对象的时候，如果里面有其他对象的构造，按照顺序从上向下依次构造。然后是运行自己的构造函数。
>2.考虑静态的问题。
>```groovy
>class Cupboard {
    Bowl b3 = new Bowl(3);
    static Bowl b4 = new Bowl(4);
    Cupboard() {
        System.out.println("Cupboard()");
        b4.f(2);
    }
    void f3(int marker) {
        System.out.println("f3(" + marker + ")");
    }
    static Bowl b5 = new Bowl(5);
}
>```
>new Cupboard的时候，如果是第一次new 就是先运行static 顺序是从上向下，然后是没有static的构造函数  顺序自上向下，然后是自己的构造函数。如果是第二次new 就不一样了，static的构造方法不在调用，没有static的函数调用 顺序自上而下，然后是自己的构造函数。

(1) 类型为 Dog的一个对象首次创建时，或者Dog 类的static方法／static 字段首次访问时，Java 解释器
必须找到Dog.class（在事先设好的类路径里搜索）。
(2) 找到Dog.class 后（它会创建一个 Class对象，这将在后面学到），它的所有 static初始化模块都会运
行。因此，static初始化仅发生一次——在 Class 对象首次载入的时候。
(3) 创建一个new Dog()时，Dog 对象的构建进程首先会在内存堆（Heap）里为一个 Dog对象分配足够多的存
储空间。
(4) 这种存储空间会清为零，将Dog中的所有基本类型设为它们的默认值（零用于数字，以及 boolean和
char 的等价设定）。
(5) 进行字段定义时发生的所有初始化都会执行。
(6) 执行构建器。正如第6 章将要讲到的那样，这实际可能要求进行相当多的操作，特别是在涉及继承的时
候。

明确进行的静态初始化：：
```groovy
class Spoon {
	static int i;
	static {
	i = 47;
    }
}
```
尽管看起来象个方法，但它实际只是一个static 关键字，后面跟随一个方法主体。与其他 static初始化一样，这段代码仅执行一次——首次生成那个类的一个对象时，或者首次访问属于那个类的一个 static 成员时（即便从未生成过那个类的对象）。
```groovy
class Cup {
	Cup(int marker) {
		System.out.println("Cup(" + marker + ")");
	}
	void f(int marker) {
		System.out.println("f(" + marker + ")");
	}
}
class Cups {
	static Cup c1;
	static Cup c2;
	static {
		c1 = new Cup(1);
		c2 = new Cup(2);
	}
	Cups() {
		System.out.println("Cups()");
	}
}
public class ExplicitStatic {
	public static void main(String[] args) {
		System.out.println("Inside main()");
		Cups.c1.f(99); // (1)
	}
	static Cups x = new Cups(); // (2)
	static Cups y = new Cups(); // (2)
} 
```
在标记为(1)的行内访问 static 对象c1 的时候，或在行(1)标记为注释，同时(2)行不标记成注释的时候，用于Cups 的 static初始化模块就会运行。若(1)和(2)都被标记成注释，则用于 Cups 的static 初始化进程永远不会发生。

非静态实例的初始化::
```groovy
class Mug {
	Mug(int marker) {
		System.out.println("Mug(" + marker + ")");
	}
	void f(int marker) {
		System.out.println("f(" + marker + ")");
	}
}
public class Mugs {
	Mug c1;
	Mug c2;
	{
		c1 = new Mug(1);
		c2 = new Mug(2);
		System.out.println("c1 & c2 initialized");
	}
	Mugs() {
		System.out.println("Mugs()");
	}
	public static void main(String[] args) {
		System.out.println("Inside main()");
		Mugs x = new Mugs();
	}
}
```
大家可看到实例初始化从句:
```groovy
{
	c1 = new Mug(1);
	c2 = new Mug(2);
	System.out.println("c1 & c2 initialized");
}
```
###数组初始化
观察封装器类型 Integer，它是一个类，而非基本数据类型。如果在新建的Integer类型的数组中没有赋值得到的是null；

##隐藏实施过程
进行面向对象的设计时，一项基本的考虑是：如何将发生变化的东西与保持不变的东西分隔开。
Java 推出了“访问指示符”的概念，允许库创建者声明哪些东西是客户程序员可以使用的，哪些是不可使用的。
这种访问控制的级别在“最大访问”和“最小访问”的范围之间，分别包括：public，“友好的”（无关键字），protected 以及private。
###包：库单元
import java.util.*;
它的作用是导入完整的实用工具（Utility）库，该库属于标准Java 开发工具包的一部分。
上面是导入整个库。
import java.util.Vector;这样的使用是最多的。
一个有效的程序就是一系列.class 文件，它们可以封装和压缩到一个 JAR文件里（使用Java 1.1 提供的jar 工具）。
Java 解释器负责对这些文件的寻找、装载和解释（注释①）。
①：Java 并没有强制一定要使用解释器。一些固有代码的Java 编译器可生成单独的可执行文件。
注意根据Java 包（封装）的约定，名字内的所有字母都应小写，甚至那些中间单词亦要如此。
####创建独一无二的包名
1. 自动编译：若只发现X.class，它就是必须使用的那一个类。然而，如果它在相同的目录中还发现了一个 X.java，编译器就会比较两个文件的日期标记。如果X.java 比X.class 新，就会自动编译X.java，生成一个最新的 X.class。
2. 冲突
> import com.bruceeckel.util.*;
 import java.util.*;
 
 由于java.util.*也包含了一个 Vector 类，所以这会造成潜在的冲突。
 假设我想使用标准的 Java Vector，那么必须象下面这样编程：
>  java.util.Vector v = new java.util.Vector();

由于 Java 的设计思想是成为一种自动跨平台的语言。

Java 访问指示符 poublic，protected 以及private都置于它们的最前面——无论它们是一个数据成员，还是一个方法。
1.public：接口访问
```groovy
public class Cookie {
    public Cookie() {
        System.out.println("Cookie constructor");
    }

    void foo() {
        System.out.println("foo");
    }
}
```
问题来了：
如果调用Cookie的类在同一个目录下。则new 一个新的对象可以调用foo()方法。
如果不在同个目录下，则new 一个新的对象不可以调用foo()方法。
2.private ：不能接触！
3.protected：“友好的一种”
###接口与实现
###类访问
亦可用访问指示符判断出一个库内的哪些类可由那个库的用户使用。
(1) 每个编译单元（文件）都只能有一个public 类。
(2) public类的名字必须与包含了编译单元的那个文件的名字完全相符，甚至包括它的大小写形式。
(3) 可能（但并常见）有一个编译单元根本没有任何公共类。
###总结
对于任何关系，最重要的一点都是规定好所有方面都必须遵守的界限或规则。
