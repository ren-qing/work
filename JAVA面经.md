# Java和PHP的区别

- 编译机制（**字节码-机器码**）：在20年之前，PHP还不支持像Java那样JIT运行时编译热点代码,但是PHP具有[**opcache机制**](#Opcache)。(a.php->经过zend编译->opcode->PHP解释器->机器码)2020-11,PHP8引入了 JIT编译器特性,JIT 是在 Opcache 优化的基础上结合 Runtime 信息将字节码（opcode->opcode_array->zender-array）编译为机器码缓存起来.现有的 Opcache 优化不受任何影响，并且 PHP 的 JIT 是在 Opcache 中提供的.JIT 不是对 Opcache 替代，而是增强，在启用 JIT 的情况下，如果 Zend 底层发现特定字节码已经编译为机器码，则可以绕过 Zend VM 直接让 CPU 执行机器码，从而提高代码性能。java是边一成字节码，class中间文件再进行解释器的解释。[参考博客](https://blog.csdn.net/ljh101/article/details/116704258)  [2](https://www.php.cn/php-weizijiaocheng-413238.html)

- 库的编写语言，编译过程：PHP的库函数用C实现,程序执行速度快，同等条件下对系统要求较低。而Java核心运行时类库用Java编写，java编译成的class文件，并不能被机器直接识别，所以Java应用运行的时候,用户编写的代码以及引用的类库和框架都要在**JVM**上解释执行. Java的HotSpot（热点）机制,直到有方法被执行10000次才会触发JIT编译, 在此之前运行在解释模式下,以避免出现JIT编译花费的时间比方法解释执行消耗的时间还要多的情况.

- **模板**：PHP内置模板引擎,自身就是模板语言.而Java Web需要使用JSP容器如Tomcat或第三方模板引擎.

- **线程、进程管理**：PHP也可以运行在多线程模式下,比如Apache的event MPM和Facebook的HHVM都是多线程架构.不管是多进程还是多线程的PHP Web运行模式,都不需要PHP开发者关心和控制,也就是说PHP开发者不需要写代码参与进程和线程的管理,这些都由PHP-FPM/HHVM/Apache实现.PHP-FPM进程管理和并发实现并不需要PHP开发者关心,而**Java多线程编程需要Java开发者编码参与**.PHP一个worker进程崩溃,master进程会自动新建一个新的worker进程,并不会导致PHP服务崩溃.而Java多线程编程稍有不慎(比如没有捕获异常)就会导致JVM崩溃退出.对于PHP-FPM和Apache MOD_PHP来说,服务进程常驻内存,但一次请求释放一次资源,这种内存释放非常彻底. PHP基于引用计数的GC甚至都还没发挥作用程序就已经结束了。

  

###### Opcache

Opcache是一种通过将解析的PHP脚本预编译的字节码（Operate Code）存放在共享内存中来避免每次加载和解析PHP脚本的开销，解析器可以直接从共享内存读取已经缓存的字节码（Operate Code），从而大大提高PHP的执行效率。

# Java中是如何支持正则表达式操作的

[正则表达式](#正则表达式)

Java中的String类提供了支持正则表达式操作的方法，包括：matches()、replaceAll()、replaceFirst()、split()。

此外，Java中可以用Pattern类表示正则表达式对象，它提供了丰富的API进行各种正则表达式操作：

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;
class RegExpTest {
    public static void main(String[] args) {
        String str = "成都市(成华区)(武侯区)(高新区)";
        Pattern p = Pattern.compile(".*?(?=\\()");
        Matcher m = p.matcher(str);
        if(m.find()) {
            System.out.println(m.group());
        }
    }
}
```

## 描述一下正则表达式及其用途

在编写处理字符串的程序时，经常会有查找符合某些复杂规则的字符串的需要。正则表达式就是用于描述这些规则的工具。换句话说，正则表达式就是记录文本规则的代码。计算机处理的信息更多的时候不是数值而是字符串，正则表达式就是在进行字符串匹配和处理的时候最为强大的工具，绝大多数语言都提供了对正则表达式的支持。

###### 正则表达式

正则表达式通常被用来检索、替换那些符合某个模式(规则)的文本。许多程序设计语言都支持利用正则表达式进行字符串操作。正则表达式是对字符串操作的一种逻辑公式，就是用事先定义好的一些特定字符、及这些特定字符的组合，组成一个“规则字符串”，这个“规则字符串”用来表达对字符串的一种过滤逻辑。

给定一个正则表达式和另一个字符串，我们可以达到如下的目的：
1. 给定的字符串是否符合正则表达式的过滤逻辑（称作“匹配”）：
2. 可以通过正则表达式，从字符串中获取我们想要的特定部分。

DFA正则引擎：不回溯

NFA 引擎：回溯，贪心

POSIX引擎：在它们可以确保已找到了可能的最长的匹配之前，它们将继续回溯。因此，POSIX NFA 引擎的速度慢于传统的 NFA 引擎；并且在使用 POSIX NFA 时，您恐怕不会愿意在更改回溯搜索的顺序的情况下来支持较短的匹配搜索，而非较长的匹配搜索。

[元字符参考视频](https://www.bilibili.com/video/BV1da4y1p7iZ?from=search&seid=6026304513394781676&spm_id_from=333.337.0.0)  [参考git说明](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)

|    元字符    | 描述                                                         |
| :----------: | :----------------------------------------------------------- |
|      \       | 将下一个字符标记符、或一个向后引用、或一个八进制转义符。例如，“\\n”匹配\n。“\n”匹配换行符。序列“\\”匹配“\”而“\(”则匹配“(”。即相当于多种编程语言中都有的“**转义字符**”的概念。 |
|      ^       | 匹配输入字行首。如果设置了RegExp对象的Multiline属性，^也匹配“\n”或“\r”之后的位置。 |
|      $       | 匹配输入行尾。如果设置了RegExp对象的Multiline属性，$也匹配“\n”或“\r”之前的位置。 |
|      *       | 匹配前面的子表达式任意次。例如，zo*能匹配“z”，也能匹配“zo”以及“zoo”。*等价于{0,}。 |
|      +       | 匹配前面的子表达式一次或多次(大于等于1次）。例如，“zo+”能匹配“zo”以及“zoo”，但不能匹配“z”。+等价于{1,}。 |
|      ?       | 匹配前面的子表达式零次或一次。例如，“do(es)?”可以匹配“do”或“does”。?等价于{0,1}。 |
|    {*n*}     | *n*是一个非负整数。匹配确定的*n*次。例如，“o{2}”不能匹配“Bob”中的“o”，但是能匹配“food”中的两个o。 |
|    {*n*,}    | *n*是一个非负整数。至少匹配*n*次。例如，“o{2,}”不能匹配“Bob”中的“o”，但能匹配“foooood”中的所有o。“o{1,}”等价于“o+”。“o{0,}”则等价于“o*”。 |
|  {*n*,*m*}   | *m*和*n*均为非负整数，其中*n*<=*m*。最少匹配*n*次且最多匹配*m*次。例如，“o{1,3}”将匹配“fooooood”中的前三个o为一组，后三个o为一组。“o{0,1}”等价于“o?”。请注意在逗号和两个数之间不能有空格。 |
|      ?       | 当该字符紧跟在任何一个其他限制符（*,+,?，{*n*}，{*n*,}，{*n*,*m*}）后面时，匹配模式是非贪婪的。非贪婪模式尽可能少地匹配所搜索的字符串，而默认的贪婪模式则尽可能多地匹配所搜索的字符串。例如，对于字符串“oooo”，“o+”将尽可能多地匹配“o”，得到结果[“oooo”]，而“o+?”将尽可能少地匹配“o”，得到结果 ['o', 'o', 'o', 'o'] |
|     .点      | 匹配除“\n”和"\r"之外的任何单个字符。要匹配包括“\n”和"\r"在内的任何字符，请使用像“[\s\S]”的模式。 |
|  (pattern)   | 匹配pattern并获取这一匹配。所获取的匹配可以从产生的Matches集合得到，在VBScript中使用SubMatches集合，在JScript中则使用$0…$9属性。要匹配圆括号字符，请使用“\(”或“\)”。 |
| (?:pattern)  | 非获取匹配，匹配pattern但不获取匹配结果，不进行存储供以后使用。这在使用或字符“(\|)”来组合一个模式的各个部分时很有用。例如“industr(?:y\|ies)”就是一个比“industry\|industries”更简略的表达式。 |
| (?=pattern)  | 非获取匹配，正向肯定预查，在任何匹配pattern的字符串开始处匹配查找字符串，该匹配不需要获取供以后使用。例如，“Windows(?=95\|98\|NT\|2000)”能匹配“Windows2000”中的“Windows”，但不能匹配“Windows3.1”中的“Windows”。预查不消耗字符，也就是说，在一个匹配发生后，在最后一次匹配之后立即开始下一次匹配的搜索，而不是从包含预查的字符之后开始。 |
| (?!pattern)  | 非获取匹配，正向否定预查，在任何不匹配pattern的字符串开始处匹配查找字符串，该匹配不需要获取供以后使用。例如“Windows(?!95\|98\|NT\|2000)”能匹配“Windows3.1”中的“Windows”，但不能匹配“Windows2000”中的“Windows”。 |
| (?<=pattern) | 非获取匹配，反向肯定预查，与正向肯定预查类似，只是方向相反。例如，“(?<=95\|98\|NT\|2000)Windows”能匹配“2000Windows”中的“Windows”，但不能匹配“3.1Windows”中的“Windows”。*python的正则表达式没有完全按照正则表达式规范实现，所以一些高级特性建议使用其他语言如java、scala等 |
| (?<!pattern) | 非获取匹配，反向否定预查，与正向否定预查类似，只是方向相反。例如“(?<!95\|98\|NT\|2000)Windows”能匹配“3.1Windows”中的“Windows”，但不能匹配“2000Windows”中的“Windows”。*python的正则表达式没有完全按照正则表达式规范实现，所以一些高级特性建议使用其他语言如java、scala等 |
|     x\|y     | 匹配x或y。例如，“z\|food”能匹配“z”或“food”(此处请谨慎)。“[z\|f]ood”则匹配“zood”或“food”。 |
|    [xyz]     | 字符集合。匹配所包含的任意一个字符。例如，“[abc]”可以匹配“plain”中的“a”。 |
|    [^xyz]    | 负值字符集合。匹配未包含的任意字符。例如，“[^abc]”可以匹配“plain”中的“plin”任一字符。 |
|    [a-z]     | 字符范围。匹配指定范围内的任意字符。例如，“[a-z]”可以匹配“a”到“z”范围内的任意小写字母字符。注意:只有连字符在字符组内部时,并且出现在两个字符之间时,才能表示字符的范围; 如果出字符组的开头,则只能表示连字符本身. |
|    [^a-z]    | 负值字符范围。匹配任何不在指定范围内的任意字符。例如，“[^a-z]”可以匹配任何不在“a”到“z”范围内的任意字符。 |
|      \b      | 匹配一个单词的边界，也就是指单词和空格间的位置（即正则表达式的“匹配”有两种概念，一种是匹配字符，一种是匹配位置，这里的\b就是匹配位置的）。例如，“er\b”可以匹配“never”中的“er”，但不能匹配“verb”中的“er”；“\b1_”可以匹配“1_23”中的“1_”，但不能匹配“21_3”中的“1_”。 |
|      \B      | 匹配非单词边界。“er\B”能匹配“verb”中的“er”，但不能匹配“never”中的“er”。 |
|     \cx      | 匹配由x指明的控制字符。例如，\cM匹配一个Control-M或回车符。x的值必须为A-Z或a-z之一。否则，将c视为一个原义的“c”字符。 |
|      \d      | 匹配一个数字字符。等价于[0-9]。grep 要加上-P，perl正则支持   |
|      \D      | 匹配一个非数字字符。等价于[^0-9]。grep要加上-P，perl正则支持 |
|      \f      | 匹配一个换页符。等价于\x0c和\cL。                            |
|      \n      | 匹配一个换行符。等价于\x0a和\cJ。                            |
|      \r      | 匹配一个回车符。等价于\x0d和\cM。                            |
|      \s      | 匹配任何不可见字符，包括空格、制表符、换页符等等。等价于[ \f\n\r\t\v]。 |
|      \S      | 匹配任何可见字符。等价于[^ \f\n\r\t\v]。                     |
|      \t      | 匹配一个制表符。等价于\x09和\cI。                            |
|      \v      | 匹配一个垂直制表符。等价于\x0b和\cK。                        |
|      \w      | 匹配包括下划线的任何单词字符。类似但不等价于“[A-Za-z0-9_]”，这里的"单词"字符使用Unicode字符集。 |
|      \W      | 匹配任何非单词字符。等价于“[^A-Za-z0-9_]”。                  |
|    \x*n*     | 匹配*n*，其中*n*为十六进制转义值。十六进制转义值必须为确定的两个数字长。例如，“\x41”匹配“A”。“\x041”则等价于“\x04&1”。正则表达式中可以使用ASCII编码。 |
|    \*num*    | 匹配*num*，其中*num*是一个正整数。对所获取的匹配的引用。例如，“(.)\1”匹配两个连续的相同字符。 |
|     \*n*     | 标识一个八进制转义值或一个向后引用。如果\*n*之前至少*n*个获取的子表达式，则*n*为向后引用。否则，如果*n*为八进制数字（0-7），则*n*为一个八进制转义值。 |
|    \*nm*     | 标识一个八进制转义值或一个向后引用。如果\*nm*之前至少有*nm*个获得子表达式，则*nm*为向后引用。如果\*nm*之前至少有*n*个获取，则*n*为一个后跟文字*m*的向后引用。如果前面的条件都不满足，若*n*和*m*均为八进制数字（0-7），则\*nm*将匹配八进制转义值*nm*。 |
|    \*nml*    | 如果*n*为八进制数字（0-7），且*m*和*l*均为八进制数字（0-7），则匹配八进制转义值*nml*。 |
|    \u*n*     | 匹配*n*，其中*n*是一个用四个十六进制数字表示的Unicode字符。例如，\u00A9匹配版权符号（&copy;）。 |
|    \p{P}     | 小写 p 是 property 的意思，表示 Unicode 属性，用于 Unicode 正表达式的前缀。中括号内的“P”表示Unicode 字符集七个字符属性之一：标点字符。其他六个属性：L：字母；M：标记符号（一般不会单独出现）；Z：分隔符（比如空格、换行等）；S：符号（比如数学符号、货币符号等）；N：数字（比如阿拉伯数字、罗马数字等）；C：其他字符。**注：此语法部分语言不支持，例：javascript。* |
|     \<\>     | 匹配词（word）的开始（\<）和结束（\>）。例如正则表达式\<the\>能够匹配字符串"for the wise"中的"the"，但是不能匹配字符串"otherwise"中的"the"。注意：这个元字符不是所有的软件都支持的。 |
|     ( )      | 将( 和 ) 之间的表达式定义为“组”（group），并且将匹配这个表达式的字符保存到一个临时区域（一个正则表达式中最多可以保存9个），它们可以用 \1 到\9 的符号来引用。 |
|      \|      | 将两个匹配条件进行逻辑“或”（or）运算。例如正则表达式(him\|her) 匹配"it belongs to him"和"it belongs to her"，但是不能匹配"it belongs to them."。注意：这个元字符不是所有的软件都支持的。 |

上面这东西能把人看困喽，建议结合视频和实操学习。

# 比较一下Java和JavaSciprt

- 首先：JavaScript 与Java是两个公司开发的不同的两个产品。Java 是原Sun Microsystems公司推出的面向对象的程序设计语言，特别适合于**互联网应用程序开发**；而JavaScript是Netscape公司的产品，为了扩展Netscape浏览器的功能而开发的一种可以嵌入Web页面中运行的**基于对象和事件驱动的解释性语言**。JavaScript的前身是LiveScript；而Java的前身是Oak语言。
- 基于对象和面向对象：Java是一种真正的**面向对象**的语言，即使是开发简单的程序，必须设计对象；**JavaScript是种脚本语言**，它可以用来制作与网络无关的，与用户交互作用的复杂软件。它是一种**基于对象（Object-Based）和事件驱动（Event-Driven）**的编程语言，因而它本身提供了非常丰富的内部对象供设计人员使用。
- 解释和编译：Java的源代码在执行之前，必须经过**编译**。JavaScript是一种解释性编程语言，其源代码不需经过编译，由**浏览器解释执行**。（目前的浏览器几乎都使用了JIT（即时编译）技术来提升JavaScript的运行效率）
- 强类型变量和类型弱变量：Java采用**强类型变量**检查，即所有变量在编译之前必须作**声明**；JavaScript中变量是**弱类型**的，甚至在使用变量前**可以不作声明**，JavaScript的解释器在运行时检查推断其数据类型。
- 代码格式不一样。



# 在Java中如何跳出当前的多重嵌套循环

在最外层循环前加一个标记如A，然后用break A;可以跳出多重循环。（Java中支持带标签的break和continue语句，作用有点类似于C和C++中的goto语句，但是就像要避免使用goto一样，应该避免使用带标签的break和continue，因为它不会让你的程序变得更优雅，很多时候甚至有相反的作用，所以这种语法其实不知道更好），根本不能进行字符串的equals比较，否则会产生NullPointerException异常。如果遇到一些比较复杂的多重循环，我更愿意建议使用多个方法来执行每一层的循环，这样会让程序结构显得更加清楚一些。



# &和&&的区别

&:

- 按位与

- 逻辑与

&&:

- 短路与

逻辑与跟短路与的差别是非常巨大的，虽然二者都要求运算符左右两端的布尔值都是true整个表达式的值才是true。&&之所以称为短路运算是因为，如果&&左边的表达式的值是false，右边的表达式会被直接短路掉，不会进行运算。很多时候我们可能都需要用&&而不是&，例如在验证用户登录时判定用户名不是null而且不是空字符串，应当写为：username != null &&!username.equals("")，二者的顺序不能交换，更不能用&运算符，因为第一个条件如果不成立，根本不能进行字符串的equals比较，否则会产生NullPointerException异常。



# int和Integer有什么区别

Java是一个近乎纯洁的面向对象编程语言，但是为了编程的方便还是引入了**基本数据类型**，但是为了能够将这些基本数据类型当成对象操作，Java为每一个基本数据类型都引入了对应的**包装类型**（wrapper class），int的包装类就是Integer，从Java 5开始引入了[**自动装箱/拆箱机制**](#自动装箱/拆箱机制)，使得二者可以相互转换。

基本数据类型不可以用方法，包装类有方法。

Java 为每个原始类型提供了包装类型：
\- 原始类型: boolean，char，byte，short，int，long，float，double
\- 包装类型：Boolean，Character，Byte，Short，Integer，Long，Float，Double

如：

```java
class AutoUnboxingTest {
  public static void main(String[] args) {
    Integer a = new Integer(3);
    Integer b = 3;         // 将3自动装箱成Integer类型
    int c = 3;
    System.out.println(a == b);   // false 两个引用没有引用同一对象
    System.out.println(a == c);   // true a自动拆箱成int类型再和c比较
  }
}
```

**1，尽量不要用new的形式来获得Integer对象。**

**2，Integer对象比较大小时，不要用==，尽量使用equals方法。**

由于我们要经常使用`Integer`类，所以他会提前为`int`型创建-128~127一共256个对象，如果在**自动装箱**的时候给**局部变量**的`int`型值是在上面的范围之中，就会直接将之前创建好的对象的地址值赋给等号左边的局部变量，

和`new`一个对象不同，如果`new`就会在堆内存开辟空间，`new`两次就开辟两段空间，两段空间地址值一定不一样，所以`System.out.println(i1 == i2);`的执行结果为`false`



###### 自动装箱/拆箱机制

装箱就是自动将**基本数据类型**转换为包装器类型；拆箱就是自动将包装器类型转换为基本数据类型。

Java中的数据类型分为两类：一类是基本数据类型，另一类是引用数据类型。

**8个**基本数据类型：int \ byte \ long \ short \ float \ double \ char \ boolean

8个对应的**包装类**：Integer \ Byte \ Long \ Short \ Float \ Double \ Character \ Boolean



| 基本类型 | 二进制位数   | 分装类    |
| -------- | ------------ | --------- |
| int      | 32（4字节）  | Integer   |
| byte     | 8（1字节）   | Byte      |
| long     | 64（8字节）  | Long      |
| short    | 16（2字节）  | Short     |
| float    | 32（4字节）  | Float     |
| double   | 64（8字节）  | Double    |
| char     | 16（2字节）  | Character |
| boolean  | 1（1/8字节） | Boolean   |



<img src="https://img-blog.csdn.net/20170418171248279?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzMwOTg3MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt="img" style="zoom: 70%;" />









# 在web应用开发过程中经常遇到输出某种编码的字符，如iso8859-1等，如何输出一个某种编码的字符串？

在Java中，String的getBytes()方法是得到一个操作系统默认的编码格式的字节数组。

方法里面我们是可以进行传参的，可以指定返回某种编码方式的字节数组。

这里有一个要注意的点就是，byte类型的初始化，以及必须要获取异常，否则就报错。

```java
Public String translate (String str) {
 String tempStr = “”;
 try {
 tempStr = new String(str.getBytes(“ISO-8859-1″), “GBK”);
 tempStr = tempStr.trim();
 }
 catch (Exception e) {
 System.err.println(e.getMessage());
 }
 return tempStr;
 }
```



# 说明**String** 和**StringBuffer**的区别

JAVA 平台提供了两个类：String和StringBuffer，它们可以储存和操作字符串，即包含多个字符的字符数据。

**Java.lang.StringBuffer**

这个String类提供了数值不可改变的字符串。

而这个StringBuffer类提供的字符串进行修改。

当你知道字符数据要改变的时候你就可以使用StringBuffer。

典型地，你可以使用StringBuffers来动态构造字符数据。

字符修改上的区别

初始化上的区别，String可以空赋值，后者不行，报错

| String                                                       | StringBuffer                                                 | StringBuilder        |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------- |
| String的**值是不可变**的，这就导致每次对String的操作都会生成新的String对象，不仅效率低下，而且浪费大量优先的内存空间 | StringBuffer是可变类，和线程安全的字符串操作类，任何对它指向的字符串的操作都不会产生新的对象。每个StringBuffer对象都有一定的**缓冲区容量**，当字符串大小没有超过容量时，不会分配新的容量，当字符串大小超过容量时，会自动增加容量 | 可变类，**速度更快** |
| 不可变                                                       | 可变                                                         | 可变                 |
|                                                              | 线程安全                                                     | **线程不安全**       |
|                                                              | 多线程操作字符串                                             | 单线程操作字符串     |



因为每次生成对象都会对系统性能产生影响，特别当内存中无引用对象多了以后， JVM 的 GC (Garbage Collection，垃圾回收)就会开始工作，那速度是一定会相当慢的。

![img](https://img-blog.csdn.net/20180411092328691?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTEwMTE3Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```java
//String 改变的是堆内存地址的指向
String s = null;   //String 可以赋空值
String s = “abc”; 

StringBuffer s = null; //结果警告：Null pointer access: The variable result can only be null at this location
StringBuffer s = new StringBuffer();//StringBuffer对象是一个空的对象
StringBuffer s = new StringBuffer(“abc”);//创建带有内容的StringBuffer对象,对象的内容就是字符串”
```

（1）如果要操作少量的数据用 String；

（2）多线程操作字符串缓冲区下操作大量数据 StringBuffer；

（3）单线程操作字符串缓冲区下操作大量数据 StringBuilder。

**特别**的是，String 对象的字符串拼接其实是被 JVM 解释成了 StringBuffer 对象的拼接，所以这些时候 String 对象的速度并不会比 StringBuffer 对象慢，而特别是以下的字符串对象生成中， String 效率是远要比 StringBuffer 快的：

```java
String S1 = "This is only a" + " simple" + " test";
StringBuffer Sb = new StringBuffer("This is only a").append(" simple").append(" test");
```

其实这是 JVM 的一个把戏，在 JVM 眼里，这个

```java
String S1 = "This is only a" + " simple" + " test"; //其实就是：
String S1 = "This is only a simple test"; 
```

这里要注意的是，如果你的字符串是来自另外的 String 对象的话，速度就没那么快了，譬如：

```java
String S2 = "This is only a";
String S3 = " simple";
String S4 = " test";
String S1 = S2 +S3 + S4;
```

这时候 JVM 会规规矩矩的按照原来的方式去做

Java中的String是一个类，而并非基本数据类型。 不过她却不是普通的类哦！！！

## String对象的创建



关于类对象的创建，很普通的一种方式就是利用构造器

```java
String s=new String("Hello world"); 
```

还有一种大家都很喜欢的创建方式

```java
String s="Hello world";
```

### Java class文件结构 和 常量池

[参考博客](https://www.iteye.com/blog/hxraid-687660)

Java程序要运行，首先需要编译器将源代码文件编译成字节码文件(也就是.class文件)。然后在由JVM解释执行.

![img](https://imgconvert.csdnimg.cn/aHR0cDovL2RsLml0ZXllLmNvbS91cGxvYWQvcGljdHVyZS9waWMvNTAwMTUvMWY3YmQwOTgtMmI0MS0zZjk1LWIxMjktNDYwMTkwZjM1MWJhLmdpZg)



其中，在class文件中有一个非常重要的项——**常量池** 。这个常量池专门放置源代码中的符号信息(并且不同的符号信息放置在不同标志的常量表中)。

```java
public class HelloWorld{
    void hello(){
        System.out.println("Hello world");	
	}
}
```

### JVM运行class文件

[参考博客](https://www.iteye.com/blog/hxraid-676235)

源代码编译成class文件之后，JVM就要运行这个class文件。它首先会用类装载器加载进class文件。然后需要创建许多内存[数据结构](http://lib.csdn.net/base/datastructure)来存放class文件中的字节数据。比如class文件对应的类信息数据、常量池结构、方法中的二进制指令序列、类方法与字段的描述信息等等。当然，在运行的时候，还需要为方法创建栈帧等。这么多的内存结构当然需要管理，JVM会把这些东西都组织到几个“**运行时数据区** ”中。这里面就有我们经常说的“**方法区** ”、“**堆** ”、“**Java栈** ”等。

上面我们提到了，在Java源代码中的每一个字面值字符串，都会在编译成class文件阶段，形成标志号 为8(CONSTANT_String_info)的常量表 。 当JVM加载 class文件的时候，会为对应的常量池建立一个内存数据结构，并存放在方法区中。同时JVM会自动为CONSTANT_String_info常量表中 的字符串常量字面值 在堆中 创建 新的String对象(intern字符串 对象，又叫拘留字符串对象)。然后把CONSTANT_String_info常量表的入口地址转变成这个堆中String对象的直接地址(常量池解 析)。

这里很关键的就是这个**拘留字符串对象** 。源代码中所有相同字面值的字符串常量只可能建立唯一一个拘留字符串对象。 实际上JVM是通过一个记录了拘留字符串引用的内部数据结构来维持这一特性的。在Java程序中，可以调用String的intern()方法来使得一个常规字符串对象成为拘留字符串对象。我们会在后面介绍这个方法的。

### 操作码助忆符指令

将根据二进制指令来区别两种字符串对象的创建方式：

#### new一个

```java
String s=new String("Hello world");
```

编译成class文件后：class字节码指令集代码

```java
new java.lang.String[15]//在堆中分配一个String类对象空间，并将该对象的地址堆入操作数栈。
dup//复制操作数栈顶数据，并压入操作数栈。该指令使得操作数栈中有两个String对象的引用值
ldc <String "Hello World">[17]//将常量池中的字符串常量“hello world”指向的堆中拘留String对象的地址压入操作数栈。
invokespecial java.lang.String(java.lang.String) [19]//调用String的初始化方法，弹出操作数栈顶的对象地址，用拘留String对象的值初始化new指令创建String对象，然后将这个对象的引用压入操作数栈。
astore_1 [s]//弹出操作数栈顶数据存放在局部变量区的第一个位置上。此时存放的是new指令创建出的，已经被初始化的String对象地址（此时的栈顶值弹出存入局部变量中去。）
```

【这里有个dup指令。其作用就是复制之前分配的Java.lang.String空间的引用并压入栈顶。那么这里为什么需要这样么做呢？因为invokespecial指令通过[15]这个常量池入口寻找到了java.lang.String()构造方法，构造方法虽然找到了。但是必须还得知道是谁的构造方法，所以要将之前分配的空间的应用压入栈顶让invokespecial命令应用才知道原来这个构造方法是刚才创建的那个引用的，调用完成之后将栈顶的值弹出。之后调用astore_1将此时的栈顶值弹出存入局部变量中去。】

事实上，在运行这段指令之前，JVM就已经为"Hello world"在堆中创建了一个拘留字符串( 值得注意的是：如果源程序中还有一个"Hello world"字符串常量，那么他们都对应了同一个堆中的拘留字符串)。然后用这个拘留字符串的值来初始化堆中用new指令创建出来的新的String对象，局部变量s实际上存储的是new出来的堆对象地址。 大家注意了，此时在JVM管理的堆中，有两个相同字符串值的String对象：一个是拘留字符串对象，一个是new新建的字符串对象。如果还有一条创建语句String s1=new String("Hello world")；堆中有几个值为"Hello world"的字符串呢? 答案是3个，大家好好想想为什么吧！

#### 普通

```java
String s="Hello world"
```

编译成class文件，class字节码指令集代码

```Java
ldc <String "Hello World"> [15] //将常量池中的字符串常量“hello world”指向的堆中拘留String对象的地址压入操作数栈
astore_1[str] //弹出操作数栈顶数据存放在局部变量区的第一个位置上。此时存放的是拘留字符串对象在堆中的地址。
```

和上面的创建指令有很大的不同，局部变量s存储的是早已创建好的拘留字符串的堆地址(没有new 的对象了)。 大家好好想想，如果还有一条穿件语句String s1="Hello word"；此时堆中有几个值为"Hello world"的字符串呢?答案是1个。那么局部变量s与s1存储的地址是否相同呢？ 呵呵, 这个你应该知道了吧。

String类型脱光了其实也很普通。真正让她神秘的原因就在于**CONSTANT_String_info常量表** 和**拘留字符串对象** 的存在。

### 字符串相等问题

```java
String sa = new String("Hello world");
String sb = new String("Hello world");
System.out.println(sa==sb);//false

String sa = "Hello world";
String sb = "Hello world";
System.out.println(sa==sb);//true
```

### 字符串＋问题

```java
String sa = "ab";
String sb = "cd";
String sab = sa+sb;
String s = "abcd";
System.out.println(sab==s);//false

String sc = "ab"+"cd";
String sd = "abcd";
System.out.println(sc==sd);//true
```

代码1中局部变量sa,sb存储的是堆中两个拘留字符串对象的地址。而当执行sa+sb时，JVM首先会在堆中创建一个StringBuilder类，同时用sa指向的拘留字符串对象完成初始化，然后调用append方法完成对sb所指向的拘留字符串的合并操作，接着调用StringBuilder的toString()方法在堆中创建一个String对象，最后将刚生成的String对象的堆地址存放在局部变量sab中。而局部变量s存储的是常量池中"abcd"所对应的拘留字符串对象的地址。 sab与s地址当然不一样了。这里要注意了，代码1的堆中实际上有五个字符串对象：三个拘留字符串对象、一个String对象和一个StringBuilder对象。
   代码2中"ab"+"cd"会直接在编译期就合并成常量"abcd"， 因此相同字面值常量"abcd"所对应的是同一个拘留字符串对象，自然地址也就相同。









[参考博客](https://blog.csdn.net/itchuxuezhe_yang/article/details/89966303)

# **synchronized**关键字？？

这个关键字是为**线程同步机制** 设定的。 每一个类对象都对应一把锁，当某个线程A调用类对象O中的synchronized方法M时，必须获得对象O的锁才能够执行M方法，否则线程A阻塞。一旦线程A开始执行M方法，将独占对象O的锁。使得其它需要调用O对象的M方法的线程阻塞。只有线程A执行完毕，释放锁后。那些阻塞线程才有机会重新调用M方法。这就是解决线程同步问题的锁机制。

另外，JVM运行程序主要的时间耗费是在创建对象和回收对象上。

# 谈谈大O符号(big-O notation)并给出不同**数据结构**的例子？？

大O符号表示程序运行时的渐变时间复杂度的上界。

大O描述当数据结构中的元素增加时，算法的规模和性能在最坏的情境下有多好。

大O符号也可用来描述其他的行为，比如：内存消耗。因为集合类实际上是数据结构，我们一般使用大O符号基于时间，内存和性能来选择最好的实现。大O符号可以对大量数据的性能给出一个很好的说明。

对于函数f(n),g(n),如果存在一个常数c，使得f(n)<=c*g(n),则f(n)=O(g(n));

## 例子：



# 数组(Array)和列表(ArrayList)的区别？什么时候应该使用Array而不是ArrayList？？？



Array和ArrayList的不同点：
Array可以包含基本类型和对象类型，ArrayList只能包含对象类型。
Array大小是固定的，ArrayList的大小是动态变化的。
ArrayList提供了更多的方法和特性，比如：addAll()，removeAll()，iterator()等等。
对于基本类型数据，集合使用自动装箱来减少编码工作量。但是，当处理固定大小的基本数据类型的时候，这种方式相对比较慢。



# 编译与解释

目的都是把高级语言翻译成机器语言。两种方式只是翻译的时间不同。

编译程序把人们熟悉的语言换成二进制的。把程序编译成为机器语言的文件，比如exe文件，以后要运行的话就不用重新翻译了，直接使用编译的结果就行了（exe文件）。

.java文件->编译->.class文件，编译成.class字节码,.class需要jvm解释，然后解释执行。Java程序需要编译但是没有直接编译成机器语言，即二进制语言，而是编译成字节码（.class）再用解释方式执行。java程序编译以后的class属于中间代码，并不是可执行程序exe，不是二进制文件，所以在执行的时候需要一个中介来解释中间代码，这既是java解释器，也就是所谓的java虚拟机（JVM），也叫JDK。

