Think In Java 第二篇

##前言

开始处理第六到第九章节。

##任务

了解类对象，多形性，错误的处理。

##类再生
Java 引人注目的一项特性是代码的重复使用或者再生。但最具革命意义的是，除代码的复制和修改以外，我们还能做多得多的其他事情。
###合成的语法
为进行合成，我们只需在新类里简单地置入对象句柄即可。
(1) 在对象定义的时候。这意味着它们在构建器调用之前肯定能得到初始化。
(2) 在那个类的构建器中。
(3) 紧靠在要求实际使用那个对象之前。这样做可减少不必要的开销——假如对象并不需要创建的话。
###继承的语法
若采取这种做法，就可自动获得基础类的所有数据成员以及方法。
扩展：：在这里，大家可看到Deteregent.main()对 Cleanser.main()的调用是明确进行的。
就是在main方法里面调用Cleanser.main(args);
倘若省略所有访问指示符，则成员默认为“友好的”。这样一来，就只允许对包成员进行访问。
所以在计划继承的时候，一个比较好的规则是将所有字段都设为private，并将所有方法都设为public（protected 成员也允许衍生出来的类访问它；以后还会深入探讨这一问题）。
但在 scrub()里，不可只是简单地发出对scrub()的调用。那样便造成了递归调用，我们不愿看到这一情况。为解决这个问题，Java 提供了一个 super 关键字，它引用当前类已从中继承的一个“超类”（Superclass）。**所以表达式super.scrub()调用的是方法 scrub()的基础类版本。**
####初始化基础类
从外部看，似乎新类拥有与基础类相同的接口，而且可包含一些额外的方法和字段。
但继承并非仅仅简单地复制基础类的接口了事。创建衍生类的一个对象时，它在其中包含了基础类的一个“子对象”。这个子对象就象我们根据基础类本身创建了它的一个对象。从外部看，基础类的子对象已封装到衍生类的对象里了。
```groovy
class Game {
	Game(int i) {
		System.out.println("Game constructor");
	}
}
class BoardGame extends Game {
	BoardGame(int i) {
		super(i);
		System.out.println("BoardGame constructor");
	}
}
public class Chess extends BoardGame {
	Chess() {
		super(11);
		System.out.println("Chess constructor");
	}
	public static void main(String[] args) {
		Chess x = new Chess();
	}
}
```
下面是输出结果：
```groovy
Game constructor
BoardGame constructor
Chess constructor
```
基础类的构造参数是含一个参数的，所以第一个继承是带有擦拭参数的。问题就是在`super(i);`方法，有点意思。
扩展：：
```groovy
BoardGame() {
        super(1);
        System.out.println("BoardGame constructor");
    }
```
因为Game的构造函数是带参数的所以BoardGame()在写构造参数的时候必须用 super(1);这样才对。如果父类是默认的构造函数super()都是可以省略的。
###合成与继承的结合
####确保正确的清除
####名字的隐藏
####到底选择合成还是继承
###protected
经常想把某些东西深深地藏起来，但同时允许访问衍生类的成员。protected 关键字可帮助我们做到这一点。它的意思是“它本身是私有的，但可由从这个类继承的任何东西或者同一个包内的其他任何东西访问”。也就是说，Java 中的protected 会成为进入“友好”状态。
就是：受保护的方法，可以在继承类里面使用。
###累积开发
我们的程序不应纠缠于一些细树末节，而应着眼于创建和操作各种类型的对象，用它们表达出来自“问题空间”的一个模型。
###上溯造型
继承最值得注意的地方就是它没有为新类提供方法。
“新类属于现有类的一种类型”。
```groovy
class Instrument {
	public void play() {}
	static void tune(Instrument i) {
		// ...
		i.play();
	}
}

class Wind extends Instrument {
	public static void main(String[] args) {
		Wind flute = new Wind();
		Instrument.tune(flute); // Upcasting
        //在这里，我们将从一个Wind 句柄转换成一个Instrument 句柄的行为叫作“上溯造型”。
	}
}
```
####何谓“上溯造型”？
从衍生类到基础类，箭头朝上，所以通常把它叫作“上溯造型”，即Upcasting。
上溯造型肯定是安全的，因为我们是从一个更特殊的类型到一个更常规的类型。换言之，衍生类是基础类的一个超集。它可以包含比基础类更多的方法，但它至少包含了基础类的方法。进行上溯造型的时候，类接口可能出现的唯一一个问题是它可能丢失方法，而不是赢得这些方法。这便是在没有任何明确的造型或者其他特殊标注的情况下，编译器为什么允许上溯造型的原因所在。
###final 关键字
I like  this  feel; I like  final; start.
但它最一般的意思就是声明“这个东西不能改变”。之所以要禁止改变，可能是考虑到两方面的因素：**设计或效率**。由于这两个原因颇有些区别，所以也许会造成final 关键字的误用。
在接下去的小节里，我们将讨论final 关键字的三种应用场合：数据、方法以及类。
####final 数据
常数主要应用于下述两个方面：
(1) 编译期常数，它永远不会改变
(2) 在运行期初始化的一个值，我们不希望它发生变化
无论static还是 final字段，都只能存储一个数据，而且不得改变。
对于基本数据类型，final 会将值变成一个常数；但对于对象句柄，final 会将句柄变成一个常数。
进行声明时，必须将句柄初始化到一个具体的对象。
这一限制也适用于数组，它也属于对象。
```groovy
class Value {
    int i = 1;
}

public class FinalData {// Can be compile-time constants
    final int i1 = 9;
    static final int I2 = 99;
    // Typical public constant:
    public static final int I3 = 39;
    // Cannot be compile-time constants:
    final int i4 = (int) (Math.random() * 20);
    static final int i5 = (int) (Math.random() * 20);


    Value v1 = new Value();
    final Value v2 = new Value();
    static final Value v3 = new Value();
    //! final Value v4; // Pre-Java 1.1 Error:
    final int[] a = {1, 2, 3, 4, 5, 6};

    public void print(String id) {
        System.out.println(
                id + ": " + "i4 = " + i4 +
                        ", i5 = " + i5);
    }

    public static void main(String[] args) {
        FinalData fd1 = new FinalData();
//        fd1.i1++; // Error: can't change value
        fd1.v2.i++; // Object isn't constant!
        fd1.v1 = new Value(); // OK -- not final
        for (int i = 0; i < fd1.a.length; i++)
            fd1.a[i]++; // Object isn't constant!
//         fd1.v2 = new Value(); // Error: Can't
//         fd1.v3 = new Value(); // change handle
//         fd1.a = new int[3];
        fd1.print("fd1");
        System.out.println("Creating new FinalData");
        FinalData fd2 = new FinalData();
        fd1.print("fd1");
        fd2.print("fd2");
    }
}

```
由于i1和 I2都是具有 final属性的基本数据类型，并含有编译期的值，所以它们除了能作为编译期的常数使用外，在任何导入方式中也不会出现任何不同。
I3是我们体验此类常数定义时更典型的一种方式：public表示它们可在包外使用；Static 强调它们只有一个；而 final 表明它是一个常数。注意对于含有固定初始化值（即编译期常数）的 fianl static基本数据类型，它们的名字根据规则要全部采用大写。
也要注意i5 在编译期间是未知的，所以它没有大写。
这种差异可从输出结果中看出：
```groovy
fd1: i4 = 15, i5 = 9
Creating new FinalData
fd1: i4 = 15, i5 = 9
fd2: i4 = 10, i5 = 9
```
注意对于fd1和 fd2来说，i4的值是唯一的，但 i5的值不会由于创建了另一个FinalData 对象而发生改变。那是因为它的属性是static，而且在载入时初始化，而非每创建一个对象时初始化。
不能认为由于v2属于final，所以就不能再改变它的值。然而，我们确实不能再将v2绑定到一个新对象，因为它的属性是final。但是里面的值是可以改变的。
将句柄变成final 看起来似乎不如将基本数据类型变成 final那么有用。
#####空白final
尽管被声明成 final，但却未得到一个初始值。无论在哪种情况下，空白 final都必须在实际使用前得到正确的初始化。
现在强行要求我们对final 进行赋值处理——要么在定义字段时使用一个表达 式，要么在每个构建器中。这样就可以确保final 字段在使用前获得正确的初始化。
##### final 自变量
这意味着在一个方法的内部，我们不能改变自变量句柄指向的东西。
代码如下：：
```groovy
void with(final Gizmo g) {
//! g = new Gizmo(); // Illegal -- g is final
g.spin();
}
```
```groovy
// void f(final int i) { i++; } // Can't change
int g(final int i) { return i + 1; }
```
####final 方法
第一个是为方法“上锁”，防止任何继承类改变它的本来含义。设计程序时，若希望一个方法的行为在继承期间保持不变，而且不可被覆盖或改写，就可以采取这种做法。
采用final 方法的第二个理由是**程序执行的效率**：：只要编译器发现一个final 方法调用，就会（根据它自己的判断）忽略为执行方法调用机制而采取的常规代码插入方法（将自变量压入堆栈；跳至方法代码并执行它；跳回来；清除堆栈自变量；最后对返回值进行处理）。
**只有在方法的代码量非常少，或者想明确禁止方法被覆盖的时候，才应考虑将一个方法设为final。**
####final类
如果说整个类都是final（在它的定义前冠以 final关键字），就表明自己不希望从这个类继承，或者不允许其他任何人采取这种操作。
换言之，出于这样或那样的原因，我们的类肯定不需要进行任何改变；或者出于安全方面的理由，我们不希望进行子类化（子类处理）。
注意：数据成员既可以是 final，也可以不是，取决于我们具体选择。
**问题分析：**注意数据成员既可以是 final，也可以不是，取决于我们具体选择。将类定义成 final后，结果只是禁止进行继承——没有更多的限制。由于它禁止了继承，所以一个 final类中的所有方法都默认为 final。可为final 类内的一个方法添加final 指示符，但这样做没有任何意义。-------就是其他的类，不能对final类进行继承，
####final 的注意事项
final是否需要，到底需不需要？？
###初始化和类装载
由于 Java 中的一切东西都是对象，所以许多活动变得更加简单，这个问题便是其中的一例。
首次使用的地方也是static 初始化发生的地方。装载的时候，所有static对象和 static代码块都会按照本来的顺序初始化（亦即它们在类定义代码里写入的顺序）。当然，static 数据只会初始化一次。
####继承初始化
```groovy
class Insect {
    int i = 9;
    int j;

    Insect() {
        prt("i = " + i + ", j = " + j);
        j = 39;
    }

    static int x1 = prt("static Insect.x1 initialized");

    static int prt(String s) {
        System.out.println(s);
        return 47;
    }
}

public class Beetle extends Insect{

    int k = prt("Beetle.k initialized");

    Beetle() {
        prt("k = " + k);
        prt("j = " + j);
    }

    static int x2 = prt("static Beetle.x2 initialized");
    static int prt(String s) {
        System.out.println(s);
        return 63;
    }

    public static void main(String[] args) {
        prt("Beetle constructor");
        Beetle b = new Beetle();
    }
}
```
结果如下：：
```groovy
static Insect.x1 initialized
static Beetle.x2 initialized
Beetle constructor
i = 9, j = 0
Beetle.k initialized
k = 63
j = 39
```
问题分析：
> 迷惑点在构造函数哪里。
> 但是构造是在main里面进行的，所以打印了`Beetle constructor`，才能到构造函数。
> 结果显示static Insect.x1 initialized与static Beetle.x2 initialized提前调用了。
> ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
> 那么：：此处就是首先处理基础类的static int x1；然后处理衍生类的 static int x2；这样都处理之后才是构造开始。
> 构造开始，基础类，然后是衍生类（就是前面的问题了）。按照一定的顺序处理。


对Beetle 运行Java 时，发生的第一件事情是装载程序到外面找到那个类。在装载过程中，装载程序注意它有一个基础类（即extends 关键字要表达的意思），所以随之将其载入。无论是否准备生成那个基础类的一个对象，这个过程都会发生（请试着将对象的创建代码当作注释标注出来，自己去证实）。若基础类含有另一个基础类，则另一个基础类随即也会载入，以此类推。
接下来，会在根基础类（此时是Insect）执行 static 初始化，再在下一个衍生类执行，以此类推。保证这个顺序是非常关键的，因为衍生类的初始化可能要依赖于对基础类成员的正确初始化。

此时，必要的类已全部装载完毕，所以能够创建对象。首先，这个对象中的所有基本数据类型都会设成它们的默认值，而将对象句柄设为null。随后会调用基础类构建器。在这种情况下，调用是自动进行的。但也完全可以用super 来自行指定构建器调用（就象在Beetle()构建器中的第一个操作一样）。基础类的构建采用与衍生类构建器完全相同的处理过程。基础顺构建器完成以后，实例变量会按本来的顺序得以初始化。最后，执行构建器剩余的主体部分。

###总结
我们通过合成来实现现有类型的“再生”或“重复使用”，将其作为新类型基础实施过程的一部分使用。
如果想实现接口的“再生”，就应使用继承。

##第七章 多形性
###上溯造型
####为什么要上溯造型
上代码
```groovy

class Note2 {
    private int value;

    private Note2(int val) {
        value = val;
    }

    public static final Note2
            middleC = new Note2(0),
            cSharp = new Note2(1),
            cFlat = new Note2(2);
}

class Instrument2 {
    public void play(Note2 n) {
        System.out.println("Instrument2.play()");
    }
}

class Wind2 extends Instrument2 {
    public void play(Note2 n) {
        System.out.println("Wind2.play()");
    }
}


class Stringed2 extends Instrument2 {
    public void play(Note2 n) {
        System.out.println("Stringed2.play()");
    }
}

class Brass2 extends Instrument2 {
    public void play(Note2 n) {
        System.out.println("Brass2.play()");
    }
}

public class Music2 {

//    public static void tune(Wind2 i) {
//        i.play(Note2.middleC);
//    }
//
//    public static void tune(Stringed2 i) {
//        i.play(Note2.middleC);
//    }
//
//    public static void tune(Brass2 i) {
//        i.play(Note2.middleC);
//    }

	/**
    这一个方法就能代替上面的相同名称的方法。
    */
    public static void tune(Instrument2 i) {
        i.play(Note2.middleC);
    }

    public static void main(String[] args) {
        Wind2 flute = new Wind2();
        Stringed2 violin = new Stringed2();
        Brass2 frenchHorn = new Brass2();
        tune(flute); // No upcasting
        tune(violin);
        tune(frenchHorn);
    }

}

```
必须为每种新增的Instrument2类编写与类紧密相关的方法。这意味着第一次就要求多得多的编程量。
这正是“多形性”大显身手的地方。
###深入理解
```groovy
public static void tune(Instrument i) {
	 // ...
	 i.play(Note.middleC);
}
```
上面的方法，怎样才能知道 Instrument句柄指向的是一个 Wind，而不是一个Brass 或Stringed 呢？？
####方法调用的绑定
将一个方法调用同一个方法主体连接到一起就称为“绑定”（Binding）。此处使用的是“动态绑定”
####产生正确的行为
####扩展性
###覆盖与过载
“过载”是指同一样东西在不同的地方具有多种含义；而“覆盖”是指它随时随地都只有一种含义，只是原先的含义完全被后来的含义取代了。
###抽象类和方法
“抽象方法”：它属于一种不完整的方法，只含有一个声明，没有方法主体。
下面是抽象方法声明时采用的语法：
abstract void X();
包含了抽象方法的一个类叫作“抽象类”。
```groovy
import java.util.*;
abstract class Instrument4 {
	int i; // storage allocated for each
	public abstract void play();
	public String what() {		return "Instrument4";	}
	public abstract void adjust();
}
class Wind4 extends Instrument4 {
	public void play() {
		System.out.println("Wind4.play()");
	}
	public String what() { return "Wind4"; }
	public void adjust() {}
}
class Percussion4 extends Instrument4 {
	public void play() {
		System.out.println("Percussion4.play()");
	}
	public String what() {		 return "Percussion4"; 	}
	public void adjust() {}
}
class Stringed4 extends Instrument4 {
	public void play() {
		System.out.println("Stringed4.play()");
	}
	public String what() { return "Stringed4"; }
	public void adjust() {}
	}
class Brass4 extends Wind4 {
	public void play() {
		System.out.println("Brass4.play()");
	}
	public void adjust() {
		System.out.println("Brass4.adjust()");
	}
}
class Woodwind4 extends Wind4 {
	public void play() {
		System.out.println("Woodwind4.play()");
	}
	public String what() { return "Woodwind4"; }
}
public class Music4 {
	// Doesn't care about type, so new types
	// added to the system still work right:
	static void tune(Instrument4 i) {
		// ...
		i.play();
	}
	static void tuneAll(Instrument4[] e) {
		for(int i = 0; i < e.length; i++)
			tune(e[i]);
	}
	public static void main(String[] args) {
		Instrument4[] orchestra = new Instrument4[5];
		int i = 0;
		// Upcasting during addition to the array:
		orchestra[i++] = new Wind4();
		orchestra[i++] = new Percussion4();
		orchestra[i++] = new Stringed4();
		orchestra[i++] = new Brass4();
		orchestra[i++] = new Woodwind4();
		tuneAll(orchestra);
	}
}
```
创建抽象类和方法有时对我们非常有用，因为它们使一个类的抽象变成明显的事实，可明确告诉用户和编译器自己打算如何用它。
###接口
它允许创建者规定一个类的基本形式：方法名、自变量列表以及返回类型，但不规定方法主体。
为创建一个接口，请使用interface 关键字，而不要用 class。与类相似，我们可在 interface关键字的前面增加一个 public关键字（但只有接口定义于同名的一个文件内）；或者将其省略，营造一种“友好的”状态。
####java的“多重继承”
x 从属于 a，也从属于 b，也从属于 c
interface后面多个接口，用逗号隔开。
```groovy
package polymorphism7;

interface CanFight {
    void fight();
}

interface CanSwim {
    void swim();
}

interface CanFly {
    void fly();
}

class ActionCharacter {
    public void fight() {
        System.out.println("flightTTTTTTTTTTTTTTTT");
    }
}

/**
 * 情况是：：
 * 类里面也有fight()方法。
 * 接口CanFight里面也有fight()方法。
 * hero下面的实现就没有了 fight()方法
 */
class Hero extends ActionCharacter
        implements CanFight, CanSwim, CanFly {

    @Override
    public void swim() {

    }

    @Override
    public void fly() {
        System.out.println("FLYYYYY");
    }
}

public class Adventure {
    /****************相应的接口和基础类***************************/
    static void t(CanFight x) { x.fight(); }
    static void u(CanSwim x) { x.swim(); }
    static void v(CanFly x) { x.fly(); }
    static void w(ActionCharacter x) { x.fight(); }
    /************************完成***********************************/

    public static void main(String []args){
        Hero h=new Hero();
        t(h);
        u(h);
        v(h);
        w(h);
    }
}
```
Hero 将具体类ActionCharacter 同接口 CanFight，CanSwim 以及CanFly合并起来。按这种形式合并一个具体类与接口的时候，具体类必须首先出现，然后才是接口（否则编译器会报错）。
####初始化接口中的字段
```groovy
public interface RandVals {
	int rint = (int)(Math.random() * 10);
	long rlong = (long)(Math.random() * 10);
	float rfloat = (float)(Math.random() * 10);
	double rdouble = Math.random() * 10;
}
```
由于字段是 static的，所以它们会在首次装载类之后、以及首次访问任何字段之前获得初始化。
当然，字段并不是接口的一部分，而是保存于那个接口的 static存储区域中。
###内部类
####内部类和上溯造型
```groovy
package polymorphism7;


/**
 * Contents 和Destination 代表可由客户程序员使用的接口
 * （记住接口会将自己的所有成员都变成public属性）。
 */

/**
 * 抽象类
 */
abstract class Contents {
    abstract public int value();
}

/**
 * 接口
 */
interface Destination {
    String readLabel();
}

public class Parcel3 {
    private class PContents extends Contents {
        private int i = 11;

        @Override
        public int value() {
            return i;
        }
    }

    protected class PDestination implements Destination {
        private String label;

        private PDestination(String whereTo) {
            label = whereTo;
        }

        @Override
        public String readLabel() {
            return label;
        }
    }

    /****************下面的两个方法，做到了类上溯的问题******************/
    public Destination dest(String s) {
        return new PDestination(s);
    }

    public Contents cont() {
        return new PContents();
    }
}

class Test {
    public static void main(String[] args) {
        Parcel3 p = new Parcel3();
        Contents c = p.cont();
        Destination d = p.dest("Tanzania");
    }

}

```
>  Parcel3 p = new Parcel3();
        Contents c = p.cont();
        Destination d = p.dest("Tanzania");
        这个地方已经完成了上溯了。
        内部类

####方法和作用域中的内部类
在下面这个例子里，将修改前面的代码，以便使用：
(1) 在一个方法内定义的类
(2) 在方法的一个作用域内定义的类
(3) 一个匿名类，用于实现一个接口
(4) 一个匿名类，用于扩展拥有非默认构建器的一个类
(5) 一个匿名类，用于执行字段初始化
(6) 一个匿名类，通过实例初始化进行构建（匿名内部类不可拥有构建器）

```groovy
package polymorphism7;

public class Parcel4 {
    /**
     *(1) 在一个方法内定义的类
     * 上来就是返回一个接口
     * 利用内部类，得到上溯的接口
     * @param s
     * @return
     */
    public Destination dest(String s) {
        class PDestination implements Destination {

            private String label;

            private PDestination(String whereTo) {
                label = whereTo;
            }

            @Override
            public String readLabel() {
                return label;
            }
        }
        return new PDestination(s);
    }
    public static void main(String[] args) {
        Parcel4 p = new Parcel4();
        Destination d = p.dest("Tanzania");
    }
}
```
PDestination类属于 dest()的一部分，而不是 Parcel4的一部分（同时注意可为相同目录内每个类内部的一个内部类使用类标识符 PDestination，这样做不会发生命名的冲突）。
####链接到外部类
创建自己的内部类时，那个类的对象同时拥有指向封装对象（这些对象封装或生成了内部类）的一个链接。
```groovy
package polymorphism7;

interface Selector {
    boolean end();
    Object current();
    void next();
}

/**
 * 给人的感觉是内部类可以操控类里面的东西。
 *
 */
public class Sequence {
    private Object[] o;
    private int next = 0;

    public Sequence(int size) {
        o = new Object[size];
    }

    public void add(Object x) {
        if (next < o.length) {
            o[next] = x;
            next++;
        }
    }

    private class SSelector implements Selector {
        int i = 0;

        @Override
        public boolean end() {
            return i == o.length;
        }

        @Override
        public Object current() {
            return o[i];
        }

        @Override
        public void next() {
            if (i < o.length) i++;
        }
    }

    public Selector getSelector() {
        return new SSelector();
    }

    public static void main(String[] args) {

        Sequence s = new Sequence(10);
        for (int i = 0; i < 10; i++) {
            s.add(Integer.toString(i));
        }
        //得到内部类，通过内部类对外部的事件进行处理。
        Selector s1 = s.getSelector();
        while (!s1.end()) {
            System.out.println(s1.current());
            s1.next();
        }
    }
}
```
####static 内部类
(1) 为创建一个 static内部类的对象，我们不需要一个外部类对象。
(2) 不能从 static内部类的一个对象中访问一个外部类对象。
但在存在一些限制：由于static 成员只能位于一个类的外部级别，所以内部类不可拥有static 数据或static内部类。
```groovy
package polymorphism7;

public class Parcel10 {
    private static class PContents extends Contents {
        private int i = 11;

        @Override
        public int value() {
            return i;
        }
    }
    protected  static  class  PDestination implements Destination{

        private  String label;

        public PDestination(String labe) {
            this.label = labe;
        }

        @Override
        public String readLabel() {
            return label;
        }
    }

    public static Destination dest(String s) {
        return new PDestination(s);
    }
    public static Contents cont() {
        return new PContents();
    }
    public static void main(String[]args){
        Contents c = cont();
        Destination d = dest("Tanzania");
    }
}

```
这个与下面的问题类似  不同点是static的问题：
```groovy
package polymorphism7;

public class Parcel2 {
    class Contents {
        private int i = 11;

        public int value() {
            return i;
        }
    }

    class Destination {
        private String label;

        Destination(String whereTo) {
            label = whereTo;
        }

        String readLabel() {
            return label;
        }
    }

    /**
     * 一个外部类拥有一个特殊的方法，它会返回指向一个内部类的句柄。
     * 就是下面的两个方法。。。
     *
     * @param s
     * @return
     */
    public Destination to(String s) {
        return new Destination(s);
    }

    public Contents cont() {
        return new Contents();
    }

    public void ship(String dest) {
        Contents c = cont();
        Destination d = to(dest);
    }

    public static void main(String[] args) {
        Parcel2 p = new Parcel2();
        p.ship("Tanzania");
        Parcel2 q = new Parcel2();
        Parcel2.Contents c = q.cont();
        Parcel2.Destination d = q.to("Borneo");
    }
}
```
####引用外部类对象
```groovy
package polymorphism7;

public class Parcel11 {
    class Contents {
        private int i = 11;

        public int value() {
            return i;
        }
    }

    class Destination {
        private String label;

        Destination(String whereTo) {
            label = whereTo;
        }

        String readLabel() {
            return label;
        }
    }

    public static void main(String[] args) {
        Parcel11 p = new Parcel11();
        Parcel11.Contents c = p.new Contents();
        Parcel11.Destination d = p.new Destination("Tanzania");
        //上面的代码这是第一次接触问题就是在
        //p.new Contents();这样新建了一个对象。
    }
}

```
> 好牛的一个例子：：
> 最不好理解的两行代码是：：
>Parcel11.Contents c = p.new Contents();
>Parcel11.Destination d = p.new Destination("Tanzania");

####从内部类继承
```groovy
package polymorphism7;

class WithInner {
    class Inner {
    }
}

/**
 * InheritInner只对内部类进行扩展，没有扩展外部类。
 * 必须在构建器中采用下述语法：enclosingClassHandle.super();
 * 它提供了必要的句柄，以便程序正确编译。
 */
public class InheritInner extends WithInner.Inner {
    InheritInner(WithInner wi) {
        wi.super();
    }

    public static void main(String[] args) {
        WithInner wi = new WithInner();
        InheritInner ii = new InheritInner(wi);
    }
}

```
> 一个新的类继承了一个上面类的内部类
> 在构造函数中的参数是上面的新类，
> InheritInner(WithInner wi) {
        wi.super();
    }

>enclosingClassHandle.super();
>它提供了必要的句柄，以便程序正确编译。

####内部类可以覆盖吗？
直接分析下面的代码就可以：
```groovy
package polymorphism7;

class Egg {
    /**
     * protected 添加和不添加意思是一样的。。
     */
    protected class Yolk {
        public Yolk() {
            System.out.println("Egg.Yolk");
        }
    }

    private Yolk y;

    public Egg() {
        System.out.println("New Egg()");
        y = new Yolk();
    }
}

public class BigEgg extends Egg {
    public class Yolk {
        public Yolk() {
            System.out.println("BBBBBBBBBBigEgg.Yolk()");
        }
    }

    public static void main(String[] args) {
        new BigEgg();
    }
}

```
下面是可以覆盖的方法，我的感觉是多了继承：：
```groovy
package polymorphism7;

class Egg2 {
    protected class Yolk {
        public Yolk() {
            System.out.println("Egg2.Yolk()");
        }

        public void f() {
            System.out.println("Egg2.Yolk.f()");
        }
    }

    private Yolk y = new Yolk();

    public Egg2() {
        System.out.println("New Egg2()");
    }

    public void insertYolk(Yolk yy) {
        y = yy;
    }

    public void g() {
        y.f();
    }
}

public class BigEgg2 extends Egg2 {

    public class Yolk extends Egg2.Yolk {
        public Yolk() {
            System.out.println("BBBBBBBBigEgg2.Yolk()");
        }

        public void f() {
            System.out.println("BBBBBBigEgg2.Yolk.f()");
        }
    }

    public BigEgg2() {
        insertYolk(new Yolk());
    }

    /**
     * 构造的顺序：：
     * Egg2 e2 = new BigEgg2();无参数的构造函数里面的代码隐藏。
     * 先是BigEgg2,
     * 1.找他的父类，父类里面先是 private Yolk y = new Yolk();
     * 然后是自己的构造函数。
     * 2.自己的函数，但是此函数是没有的没有涉及到其他的构造的。
     * Egg2 e2 = new BigEgg2();无参数的构造函数里面的代码放开。
     * 有了new Yolk()的构造，分析这个函数，
     * 先运行父类的构造函数然后是自己的构造函数。
     * <p>
     * Egg2 e2 = new BigEgg2();
     * e2.g();
     * 分析e2.g();
     * 此处就是分析使用哪里的f()方法。
     * 根据结果使用的是后来继承f()的方法。
     *
     */
    public static void main(String[] args) {
        Egg2 e2 = new BigEgg2();
        e2.g();
    }
}

```
####内部类标识符
由于每个类都会生成一个.class 文件
这些文件或类的名字遵守一种严格的形式：先是封装类的名字，再跟随一个$，再跟随内部类的名字。
例如，由InheritInner.java创建的.class 文件包括：
InheritInner.class
WithInner$Inner.class
WithInner.class
####为什么要用内部类：控制框架

###构建器和多形性
####构建器的调用顺序
这意味着对于一个复杂的对象，构建器的调用遵照下面的顺序：
(1) 调用基础类构建器。这个步骤会不断重复下去，首先得到构建的是分级结构的根部，然后是下一个衍生
类，等等。直到抵达最深一层的衍生类。
(2) 按声明顺序调用成员初始化模块。
(3) 调用衍生构建器的主体。
####继承和 finalize()

##第 8 章 对象的容纳
