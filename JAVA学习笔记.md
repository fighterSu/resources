# JAVA学习笔记



## package的使用

类似**C++**之中的**namespace**.
package表示该程序所属的包，它**只能有一个或者没有**。如果**有**，**必须放在最前面**；如果**没有**，那该**程序属于默认包**。默认包视运行目录而定
一般在建立项目后，再建立包，再建立类，应用会自动声明当前类所在的包。

声明这个类所属的包，方便区分同名变量，同时方便其它包的程序调用这个类定义的方法

### package的命名

**一直使用默认包会导致冲突，JVM之中只允许同一个**

Java建议采用**Internet域名**的倒序作为包的前缀。

**通常包名都用小写字母命名**.

以百度公司为例，域名为 www.baidu.com

则在获得公司许可前提下，可使用以下包名

com.baidu.www.mypackage

在该包新建一个Mian类，它的所在位置如下

![图片1.png](http://ww1.sinaimg.cn/large/006W2odlgy1gtckk6ntycj608q03qq3502.jpg)

可以看到，包名不仅仅是简单的包名，并且**包名与文件系统的目录结构一一对应.**

可以看到**每个点都是一个一级目录的分隔点**

**Java中的每个类都属于某一个包, 类在编译时被添加到包中. 默认情况下, 类在编译时放在当前目录(默认包).**

把类添加到指定包中, 只需在源程序的最前端加上:

````java
package packageName;
````



## 库的引入

```java
import java.io.*
```
与C语言中的 **#include " "** 作用类似

**除了package和import语句以外，其它执行操作的语句，都只能在类的大括号里面**

**如果要使用其他包之中的类，要使用import引入其它包**

现在一般不用自己手动引入，ide会帮助我们完成这一工作



## 源程序代码文件基本结构

```java
package package01;  // 包语句
import java.util.Scanner;	// 导入语句（可选）
public class FirstApplication {   // 定义类、接口
    public static void main(String[] args) {    // 主方法，程序的起点
        Scanner input = new Scanner(System.in);
        System.out.print("Input 2 intgers: ");
        int a = input.nextInt();
        int b = input.nextInt();
        int c = a + b;
        System.out.println(a + "+" + b + "=" + c);
    }
}
```



### 注意事项

1. Java**允许**在**一个源程序代码文件**里面编写**多个类**，但**强烈建议 ：一个源程序代码文件里面只编写一个类，并用 public 修饰**
2. 同**一个源程序代码文件最多只有一个类用public 修饰**。并且要求**源程序代码文件名**与**public类**的**类名相同（包括字母大小写）**
3. 编译源程序（Windows下，使用javac），**每个类**生成**一个**拓展名为class的字节文件
4. 可以分别编译每个类，也可以直接编译主类，**编译器会自动先编译需要的类**
5. **JRE  Java Runtime Environment**    java运行环境



## 数据类型

**增加**的类型：**byte**

 1. **范围** ：-128——127；
 2. **类型**：整形



### 字符型
1. 字节大小：2 字节
2. 取值：0-65535
3. 编码集：Unicode编码



**Unicode编码和ASCII码**
1. Unicode前128位刚刚好是ASCII表
2. 都可以让字符数据转换成整数
3. 区别
   整数代表Unicode表中的编码，必须进行强制类型转换
   ```java
   int c = 97;
   char s = c; // X 错误的用法 
   char s = (char)c;
   ```
   ASCII码可以直接转换
   ```C
   int n = 97;
   char a = n; // a 表示字母 'a' 
   ```



### 自动装箱的享元模式

```java
Integer i1 = 789; 		//自动装箱, 系统根据基本类型数据789自动创建一个包装类对象 , 把该对象的引用保存到i1中
Integer i2 = 789; 		//根据789创建一个新的Integer对象, 把该对象的引用保存到i2中

int num = i1; 			//自动拆箱, 系统把i1对象的value字段赋值给num

//包装类对象判等
System.out.println( i1.equals(i2));   	//true
System.out.println( i1 == i2 ); 		//false

Integer i3 = 88;
Integer i4 = 88;
System.out.println( i3 == i4 ); 		//true
/*
 * Java认为-128~127之间的整数是使用最频繁的, 这个范围内的整数装箱后采用享元模式
 * 	-128~127之间的整数生成的包装类对象存储在方法区的常量池中
 * 	在执行i3 = 88时, 会在常量池中创建一个包装类对象, 
 * 	在执行 i4 = 88时, 直接把常量池中的包装类对象的引用赋值给i4, 即现在i3和i4引用了常量池中同一个包装类对象
 */

Long  gg1 = 123L;
Long  gg2 = 123L;
System.out.println( gg1 == gg2 );  		//true

```





### 常量的声明

**C++**

```c++
const double PI 3.14
```
#### define 和 const 的区别

1. define是宏定义，程序在预处理阶段将用define定义的内容进行了替换。因此程序运时，常量表中并没有用define定义的常量，系统不为它分配内存。
   const定义的常量，在程序运行时在常量表中，系统为它分配内存。

2. define定义的常量，预处理时只是直接进行了替换。所以编译时不能进行数据类型检验。
   const定义的常量，在编译时进行严格的类型检验，可以避免出错。

3. define定义表达式时要注意“边缘效应”，例如如下定义：

   ```c
   #define N 2+3 //我们预想的N值是5，我们这样使用N
   int a = N/2; //我们预想的a的值是2.5，可实际上a的值是3.5
   ```

原因是 N 用2+3替代，则 a = 2+3/2 = 3.5  故**应避免用宏定义含有运算符的式子**

**所以通常不把define宏定义的量称为常量**



**JAVA**

```java
final float PI = 3.14;
```
**区别：JAVA需要声明常量类型以及需要等号和分号**
**注意：JAVA的声明需要在类里面，否则会报错**（除了包语句和导入语句以外，其他语句都需要在类里面）
**栗子**
![编译代码](https://img-blog.csdnimg.cn/20201004093835351.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTY0MDA0NA==,size_16,color_FFFFFF,t_70#pic_center)
![错误信息](https://img-blog.csdnimg.cn/20201004093835347.png#pic_center)
**正确声明**
![正确声明](https://img-blog.csdnimg.cn/20201004094535859.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTY0MDA0NA==,size_16,color_FFFFFF,t_70#pic_center)
**这里再讲一下我遇到的另外一个问题，
错误: 无法从静态上下文中引用非静态 变量 this
由于有网友做了解答我就直接引用了hh**
[文章链接](https://blog.csdn.net/qq_33740600/article/details/53165844)
**主要是 ~~没有将声明实例化~~ 就直接使用，所以会报错**
![错误示范](https://img-blog.csdnimg.cn/20201004100218174.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTY0MDA0NA==,size_16,color_FFFFFF,t_70#pic_center)
![错误信息](https://img-blog.csdnimg.cn/20201004100251511.png#pic_center)
**正确用法**
先将声明实例化

```java
Hello A = new Hello( );
size = A.PI*r*r;
```

![正确用法](https://img-blog.csdnimg.cn/202010041004017.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTY0MDA0NA==,size_16,color_FFFFFF,t_70#pic_center)

或者将**常量PI 变成静态常量**也可以解决

```java
public class test {
    static final double PI = 3.1415926;
    public static void main(String[] args){
        System.out.println("PI = " + PI);
    } 
} // 输出结果：PI = 3.1415926
```



### 运算逻辑



#### 运算遵循的原则

1. 严格遵循从**左到右**；
2. 按照**运算符的优先级**进行
3. 运算符优先级相同，则根据运算符结合方向结合

**栗子**

```java
int a = 0;
int x1 = a++ + a++ + a++; // 根据从左到右原则，则对应每项为 0、1、2
a = 0;
int x2 = ++a + ++a + ++a; // 根据从左到右原则，则对应每项为 1、2、3
System.out.println(x1 + " " + x2);
// 运行结果为 3 6 
```

**C语言没有这种严格从左到右的原则，在不同编译器编译上述代码，可能会出现不同结果**

### 数据类型转换

不同数据类型混合进行运算，根据数据类型大小进行自动转换，再进行运算

byte < short < char < int < long < float < double



**需要注意：整形数据类型运算最低级别为 int 类型**



**不同类型数据算术运算时的转换规则：**

1.如果运算对象之一是double，就将另一个转换为double型。

2.否则，如果运算对象之一是float，就将另一个转换为float型。

3.否则，如果运算对象之一是long，就将另一个转换为long型。

4.**否则，两个运算对象都转换为int型**。(int  是Java 运算的最小类型)



#### 拓展赋值运算符

和 C 语言之中差别不大，主要讲一个区别，**赋值运算符会自动进行强制类型转换**

```java
short s1 = 1;
s1 = s1 + 1; //会报错
short s2 = 1;
s2 += 1; // ==> s2 = (short)(s2 + 1);
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201023182929149.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTY0MDA0NA==,size_16,color_FFFFFF,t_70#pic_center)这个读者可以自己编译看看
## 流程控制

**对于多个判断条件，最好先写好各个判断的条件，再开始写具体的语句**

### switch 语句

和 C 语言的区别是**判断的表达式可以是 String**

栗子

```java
String s;
Scanner input = new Scanner(System.in);
s = inuput.next();
switch(s)
{
    case "张三" :
        System.out.println("张三获胜");
        break;
    case "李四" :
        System.out.println("李四获胜");
        break;
    default:
        System.out.println("Error");
}
```

### break 和 continue 的新用法

**标号的使用**

```java
outer: for(int i = 0; i < 10 ;i++)
{
    for(int j = 0; j < 10 ;j++)
    {
        if(i == 3) continue outer; // 跳到标号 outer 指代的循环的 i++ ,再进入判断 i<10
        a[i][j] = i;
        if(i>5) break outer;  // 跳出标号 outer 指代的循环
    }
}
```
**标号的使用可以快速跳出多重循环，提高算法效率**



##  String类的使用方法

C++用法

https://blog.csdn.net/manonghouyiming/article/details/79827040

主要讲一个区别，**java之中没有~~s[i]~~这种用法**

```java
Scanner input = new Scanner(System.in);
String s = input.nextLine();
System.out.println(s[0]);  //会报错
```

java中得到第几个元素**需要使用 charAt()方法**

```java
Scanner input = new Scanner(System.in);
String s = input.nextLine();
System.out.println(s.charAt(0));
// 输入 test
// 输出 t
```

**String类型常用方法**

| 常用方法                                              | 功能说明                                                     |
| :---------------------------------------------------- | ------------------------------------------------------------ |
| public int length()                                   | 返回字符串的长度                                             |
| public boolean equals(Object anObject)                | 将给定字符串与当前字符串相比较，若两字符串相等，则返回true,否则   返回false |
| public String substring(int beginIndex)               | 返回字符串中从beginIndex开始到字符串末尾的子串               |
| public String substring(int  beginIndex,int endlndex) | 返回从beginIndex开始到endIndex-1的子串                       |
| public char charAt(int index)                         | 返回index指定位置的字符                                      |
| public int indexOf(String str)                        | 返回str在字符串中第一次出现的位置                            |
| public int compareTo(String  anotherString)           | 若调用该方法的字符串大于参数字符串，则返回大于0的值，若相等则返回0，小于参数字符串则返回小于0的值 |
| public String replace(char oldChar, char  newChar)    | 以newChar字符替换字符串中所有oldChar字符                     |
| public String trim()                                  | 去掉字符串的首尾空格                                         |
| public String toLowerCase()                           | 将字符串中的所有字符都转换为小写字符                         |
| public String toUpperCase()                           | 将字符串中的所有字符都转换为大写字符                         |

**格式化方法：String.(String format, Object... args)**

**参数格式同printf();**



**需要重复更改的字符串应当使用StringBiffer（线程安全）或者StringBuilder（线程不安全)来定义**

**一般使用StringBuilder,除非特别要求线程安全**



## 键盘输入数据

书上有两种方法，这里仅介绍第二种



### 步骤
1. 引用对应的类库

```java
import java.util.Scanner //包含专门用于输入的类 Scanner
```
2. 使用Scanner类创建对象

```java
Scanner reader = new Scanner(System.in); 
//创建 Scanner对象用于读取System.in的输入
```
3. 声明变量类型

```java
int a;
```
4. 调用reader对象的hasNextXXX()方法，判断用户输入的是否是相应类型数据

5. 调用 reader 对象的nextXXX()方法，读取键盘对应数据

```java
if(reader.hasNextInt()){
	a = reader.nextInt();
}
```
**对应的方法**包括：**nextByte()、nextDouble()、nextFloat()、nextInt()、nextLong()、nextShort()、next()、nextLine()**
**形式**:next数据类型()
**用法**:对应数据类型代表需要从键盘获取的数据的类型

**next()和nextLine()的异同**

**相同**:
都是用于**从键盘**读取**字符串**的

**不同**:
**next()方法一定要读取到有效字符后才可以结束输入**,对输入**有效字符之前遇到的空格键、Tab键或Enter 键等,next()方法会自动将其去掉**，只有在输入有效字符之后，next()方法才将其后输入的空格键、Tab键或Enter键视为分隔符或结束符;而**nextLine()方法的结束符只是Enter键，即nextLine()方法返回的是Enter键之前的所有字符**

**Scanner类的对象使用完毕后不能关闭**。
不然 **整个 System.in 会被关闭**，可能**严重影响其它资源运行**



## 数组

**java.util.Arrays**

Java 提供了一个数组相关的工具包，集合了数组的常用方法，读者可以自行查看API 文档



### 声明方式



#### 一维数组



C语言
```C
int a[10];
```
JAVA
需要先声明，再申请空间才能使用
```java
int[] a = new int[10];
```
第一个[]声明是数组类型，相当于**只有一个数组名字**，还不能使用
第二个[10]里面数字代表申请空间个数，int[10]代表申请10个存放int类型数据的空间
**new 用于申请空间**

**特殊初始化**
```java
int[] a = {1,2,3,4,5};
```
像这样声明了一个整形数组a，虽然没有指定数组长度，但由于括号里面初值有5个，所以编译器会按照次序存放在a[0]-a[4].



#### 多维数组



**Java语言没有真正的多维数组。**

**所谓多维数组，是数组元素也是数组的数组。**

**声明数组**

int[][] a = new int[10][10]  二维数组
以此类推，三维就是三个框框

```java
// 方式1：同时创建整个多维数组对象

int[][] x = new int[3][4]; //同时指定行、列数

// 方式2：分别创建 “行对象”和 “列对象”

int[][] matrix = new int[5][];           //创建 “行对象”
matrix[0] = new int[] { 1, 2, 3, 4, 5 }; //创建 “列对象”
matrix[1] = new int[] { 2, 3, 4, 5 };
matrix[2] = new int[] { 3, 4, 5 };
matrix[3] = new int[] { 4, 5 };
matrix[4] = new int[] { 5 };

// 多维数组每一行都是一个数组，都有length属性，可以输出每行的长度

x.length = 3     // 行数
x[0].length = 4  // 列数
x[1].length = 4
x[2].length = 4
// 第二个同理
```





### 新的遍历方式
```java
for( type element : array )
{
  System.out.println(element);
  ···
}
```
**栗子**
```java
int[] a = {1，2，3，4，5}；
for( int element : a)
     System.out.println(element);
```
![完整代码](https://img-blog.csdnimg.cn/20201004193602794.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTY0MDA0NA==,size_16,color_FFFFFF,t_70#pic_center)

![运行结果](https://img-blog.csdnimg.cn/20201004193529941.png#pic_center)



### 类类型数组

顾名思义，就是数组元素是类的数组
用数组来存放对象，一般要经过如下两个步骤:

(1)**声明类类型的数组变量**，并用**new运算符分配内存空间给数组**;

(2)用**new创建新的对象**，**分配内存空间给它**,并**让数组元素指向它**。
先看例子
```java
class Spot
{
	double x;
	double y;
}
Spot[] a = new Spot[n + 5]; // 类类型数组的声明和普通数组声明类似
```
**需要注意的是：类类型的数组使用new 进行初始化，仅仅是获得了对应的可以存放对象的地址的空间，所以在上述讲的是，“让数组元素指向它”，其中的”它“就是使用 new 创建的新的对象**

这里可以类比C 语言中的结构体数组
```C
struct student
{
	char name[10];
	char sex[5];
};

struct student a[10+5];
```
定义过程比较类似，但**初始化方法并不相同**
先讲C 语言中的，直接  **cin>>a[0].name** 即可
下面是建立一个Spot类数组，然后给每个数组元素赋值的操作
```java
import java.util.Scanner;

public class test{
	public static void main(String[] args){
		System.out.print("输入二维坐标系中点的个数: ");
		Scanner input = new Scanner(System.in);
		int n = input.nextInt();
		Spot[] a = new Spot[n + 5]; // a 获得了 n+5 个 “可以存放Spot 类对象地址” 的空间
		System.out.printf("输入%d个点的横坐标和纵坐标:\n", n);
		for (int i = 0; i < n; i++)
		{
			Spot temp = new Spot(); // 每次需要重新申请空间，因为之前的空间已经使用了
			temp.x = input.nextDouble();
			temp.y = input.nextDouble();
			a[i] = temp; // 让数组元素指向它
		}
		for (int i = 0; i < n; i++)
		{
			System.out.println(a[i].x + " " + a[i].y); // 打印Spot类数组元素，确保初始化成功
		}
	}
}

class Spot
{
	double x;
	double y;
}
```



## Java的输出语句

```java
System.out.print( );
System.out.println( );
```
以上两个语句类似C++ 之中的cout<<
println( )较print( )多了一个默认换行
具体使用方法见下栗子
```java
int a = 1, b = 2;
System.out.println(a + " + " + b + " = " + (a+b));
System.out.print(a + " + " + b + " = " + a+b);
```
读者需要注意的是上述语句之中，**（a+b) 和 a + b 在 含有""的输出语句之中的差别**，具体看下图

源代码如下

```java
public class test{
	public static void main(String[] args){
		int a = 1, b = 2;
		System.out.println(a + " + " + b + " = " + (a + b));
		System.out.print(a + " + " + b + " = " + a + b);
	}
}
```

运行结果如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020080519766.png#pic_center)

可以看到 **(a+b)** 代表的是 **a 和 b 的和**，而 **a+b** 仅仅代表**输出 a 后接着输出 b**.

**需要注意，以上区别仅仅在存在""内容条件下成立，若输出语句之中不含"",则本结论不成立**



第三种和C语言中的printf比较类似,相关用法几乎完全相同
System.out.printf ( );
**栗子**

```java
int a = 1, b = 2;
System.out.printf("a + b = %d\n",a+b);
```

**需要注意的是，double 型变量在 C 中的是 %lf ，而java 中是 %f**



## 面向对象



### 数据域封装

**什么时候都需要封装**

1. 使用 private 关键字去封装数据

2. 根据变量使用范围确定是否提供访问器get...( )方法（bollean类型变量为is...( )）以及修改器set...( )方法 

| 变量使用要求   |          | 提供方法                  |
| -------------- | -------- | ------------------------- |
| non-read-write | 不可读写 | 不提供get…( ) 和 set..( ) |
| readOnly       | 只读     | 只提供get..( )            |
| writeOnly      | 只写     | 只提供set..( )            |
| read-write     | 读写     | 提供get..( )和set...( )   |

```java
public class Book {
    private String isbn;
    private boolean notPublished;

    public String getIsbn() {
        return isbn;
    }
    public void setIsbn(String isbn) {
        this.isbn = isbn;
    }

    public boolean isNotPublished() {
        return notPublished;
    }
    public void setNotPublished(boolean notPublished) {
        this.notPublished = notPublished;
    }
}
```



### this 关键字的使用

**在类的构造方法中使用**,调用本类的其他构造方法。
**语法：this(实参表)**
如果使用该语句,**必须在构造方法的第一句(显性的第一句)**

**在类的构造方法和实例方法中使用，代表对当前对象的引用。**
**访问实例**变量语法：**this.实例变量名**
**方法实例**方法语法：**this.实例方法名(实参表)**
**每个构造方法和实例方法中都隐含有一个this引用。**

```java
public class Rectangle {
    double width;
    double height;
    public Rectangle(double width, double height) {
        this.width = width; //this必须使用,引用正在创建的对象
        this.height = height;
    }
    public Rectangle() {
        this(1.0, 1.0);     //调用本类的其他构造方法
    }
    public void setWidth(double width) {
        this.width = width; //this必须使用,引用当前对象
    }
    public double getArea() {
        return this.width * this.height; //this可以省略
    }
}
```



### 初始器的使用

**静态初始化器与实例初始化器**

**初始化器**是**直接在类中定义**的**用一对{}括起来的语句组**。

**静态初始化器**使**用static关键字修饰**，用来**初始化静态变量**。

**实例初始化器没有修饰关键字**，用来**初始化实例变量**。

**一个类**可以有**0个或多个**静态和实例**初始化器**。

![image-20210103182942406](https://gitee.com/fighterSu/resources/raw/image/typora/202108121047704.png)

**静态初始化器的执行**：**类首次加载到内存时**，**首先**是**静态数据域变量的变量初始化**；然后**按排列顺序执行类中static初始化器**。

**实例初始化器的执行**：每次使用 “new 构造方法();” 创建对象时，首先是实例数据域变量的变量初始化，然后开始执行本类的构造方法，**即在构造方法第一条语句执行之前（隐式第一，比构造方法还前）**，**按排列顺序执行类中的实例初始化器，然后执行构造方法中的剩余语句**。



## 继承





### 子类的构造方法



## 对象类型和 instanceof 运算符



### instanceof 运算符

**！！！是一个运算符**

运算符**语法**：**对象 instanceof 类**

运算**结果**：**boolean**

运算**规则**：**如果对象的类是后面的类或其子类，返回true；否则返回false。**

```java
class A{}
class B extends A {}
class C extends B {}
class D extends C {}
class E extends C {}

public static void main(String[] args){
    D d = new D();
    System.out.println(d instanceof A);  // 结果为：true
    System.out.println(d instanceof B);  // 结果为：true
    System.out.println(d instanceof C);  // 结果为：true
    System.out.println(d instanceof D);  // 结果为：true
	System.out.println(d instanceof Object);  // 结果为：true
    System.out.println(d instanceof E);  // 结果为：false
}
```





使用instanceof **判断一个对象是否属于某个类**只是**初略判断**。

**精确判断**一个对象的类：

**对象.getClass() == 类名.class**



### 子类转父类

**父类类型**的**对象引用**可以**引用**一个**父类对象或者任何的子类对象**. 称为**隐式类型转换(向上类型转换)**。

```java
Object obj = new Student(); 
// System.out.println(obj.getClass());  //结果为： class 包名.Student
// 上面等价于下面
Student stud = new Student();
Object obj = stud;
```

**注意：第一条语句，Object 类型的引用变量obj引用的是他的一个子类 Student 的对象，所以obj.getClass()，得到的结果为： class 包名.Student**





栗子

```java
class A{}
class B extends A {}
class C extends B {}
class D extends C {}
class E extends C {}

public static void main(String[] args){
    Object d = new D();  // 实际上 Object 类型的引用变量 d 引用的是 D 类的一个对象
    System.out.println(d.getClass());    // 结果为：class 包名.D
    System.out.println(d instanceof A);  // 结果为：true
    System.out.println(d instanceof B);  // 结果为：true
    System.out.println(d instanceof C);  // 结果为：true
    System.out.println(d instanceof D);  // 结果为：true
	System.out.println(d instanceof Object);  // 结果为：true
    System.out.println(d instanceof E);  // 结果为：false
}
```

### 父类转子类

如果**没有显式地强制类型转换**, 一个**子类类型的对象引用不可以引用父类对象**. 把父类类型强制转换为子类类型称为向下强制类型转换。

```java
Object obj = new Date();
Date date = obj;             // 编译出错
Date date = (Date)obj;   // 正确 

// 下面语句无语法错误, 但是导致运行错误
Date d = (Date) new Object();  
```



### 不能进行平等转换

如果**两个类类型**是**平等的**，即**任何一个都不是另一个的祖先或后代**，则**这两个类类型不能进行类型转换**。

```java
// Date 类型和 String 类型无对应继承继承关系，不能进行类型转换
Date d = new String();                  // 语法错误 
Date d = (Date) new String();       // 语法错误
String str = (String )new Date();  // 语法错误

```





## 多态性

![image-20210103183214443](https://gitee.com/fighterSu/resources/raw/image/typora/202108121050511.png)



### 方法覆盖

**对象**：实例方法



**使用场景：**父类和子类中有**相同的方法**，但是其**实现(方法体)不同**。



#### 覆盖规则

子类定义一个与继承**实例方法同名**的方法。



子类方法的**返回类型**要求：

**基本类型或void**：与父类方法的返回类型**相同**

**类：与父类方法返回类型相同或是返回类型的子类**



**子类方法的形参的个数、类型必须与父类方法完全相同。参数名称可以不同。**



**访问权限**：子类方法的访问权限**必须大于等于**父类方法的访问权限。



#### 辅助修饰符  @Override

元注解

用法：加在重写的方法的前一行

**作用：检查该方法重写是否符合语法规范**



### 方法重写

对象：静态方法

作用：隐藏父类的方法，并没有覆盖



### super关键字

**关键字super在子类中使用，主要有2种用途:**

调用父类的构造方法：super( ),super(args0,args1...)

访问已经继承的成员变量和实例方法

**子类访问继承自父类的实例成员的语法，无论否被隐藏或重写**。

**super.方法名(参数)**

**super.成员变量**



### equals 方法重写

```java
@Override
public boolean equals(Object other) {
    if (this == other) {
        return true;
    }
    if (other == null) {
        return false;
    }
    if (this.getClass() != other.getClass()) {
        return false;
    }
    // 前三个都一样
	// 后面根据要求定义是否相等
    Product t = (Product) other;
    if (!this.id.equals(t.id)) {
        return false;
    }
    if (!this.name.equals(t.name)) {
        return false;
    }
    if (Double.doubleToLongBits(this.price) != Double.doubleToLongBits(t.price)) {
        return false;
    }
    if (this.inventory != t.inventory) {
        return false;
    }
    return this.shelfDate.equals(t.shelfDate);
}
```





### hashCode 方法重写

**注意：equals()方法比较了哪些数据域的变量，重写hashCode()方法就要使用上那些变量**

下面是对应上面的equals()方法的hashCode()的重写

```java
@Override
public int hashCode(){
    return Objects.hash(id,name,price,inventory,shelfDate);
}
```



相关知识连接

https://www.cnblogs.com/skywang12345/p/3324958.html

https://www.cnblogs.com/myseries/p/10977868.html

**`hashCode`散列码计算**（来自：Effective Java）

1. 把某个非零的常数值，比如`17`，保存在一个名为`result`的`int`类型的变量中。
2. 对于对象中每个关键域`f`(**指`equals`方法中涉及的每个域**)，完成以下步骤：
   1. 为该域计算`int`类型的散列码c：
      1. 如果该域是`boolean`类型，则计算(`f?1:0`)。
      2. 如果该域是`byte`，`char`，`short`或者int类型，则计算`(int)f`。
      3. 如果该域是`long`类型，则计算`(int)(f^(f>>>32))`。
      4. 如果该域是`float`类型，则计算`Float.floatToIntBits(f)`。
      5. 如果该域是`double`类型，则计算`Double.doubleToLongBits(f)`，然后按照步骤**2.1.3**，为得到的`long`类型值计算散列值。
      6. 如果该域是一个对象引用，并且该类的`equals`方法通过递归地调用`equals`的方式来比较这个域，则同样为这个域递归地调用`hashCode`。如果需要更复杂的比较，则为这个域计算一个范式`(canonical representation)`，然后针对这个范式调用`hashCode`。如果这个域的值为`null`，则返回`0`(其他常数也行)。
      7. 如果该域是一个数组，则要把每一个元素当做单独的域来处理。也就是说，递归地应用上述规则，对每个重要的元素计算一个散列码，然后根据步骤**2.2**中的做法把这些散列值组合起来。如果数组域中的每个元素都很重要，可以利用发行版本**1.5**中增加的其中一个`Arrays.hashCode`方法。
   2. 按照下面的公式，把步骤2.1中计算得到的散列码`c`合并到`result`中：`result = 31 * result + c`; //此处`31`是个奇素数，并且有个很好的特性，即用移位和减法来代替乘法，可以得到更好的性能：31*i == (i<<5) - i， 现代JVM能自动完成此优化。
3. 返回`result`
4. 检验并测试该`hashCode`实现是否符合通用约定。

**简单的重写**

**利用Objects类提供的hash（）方法**

```java
@Override
	public int hashCode() {
		return Objects.hash(lengthOfSide, numberOfSides, x, y);
	}

	public int myHashCode() {
		final int prime = 31;
		int result = 1;
		long temp;
		temp = Double.doubleToLongBits(lengthOfSide);
		result = prime * result + (int) (temp ^ (temp >>> 32));
		result = prime * result + numberOfSides;
		temp = Double.doubleToLongBits(x);
		result = prime * result + (int) (temp ^ (temp >>> 32));
		temp = Double.doubleToLongBits(y);
		result = prime * result + (int) (temp ^ (temp >>> 32));
		return result;
	}
```

两个代码是**等价**的，可能**计算出的哈希代码不同**，则是**Object.hash()的参数顺序**和**求解哈希代码的变量处理顺序**不同导致的，只需要将**参数顺序和求解哈希代码是变量处理顺序修改成一致即可**，如以上代码，计算出的哈希代码相同

## 接口

类似抽象类，但两者各有优缺点

实现关键字：implements

特性：可以同时实现多个接口，在一定程度上实现多重继承的效果

## UML类图

引用来源

在线链接：http://www.uml.org.cn/oobject/201211231.asp



 本地文件：[深入浅出ML类图.html](深入浅出UML类图.html) 



### 类的图示

![img](http://www.uml.org.cn/oobject/images/2012112311.jpg)

图1对应的Java代码片段如下：

```java
public class Employee {
	private String name;
	private int age;
	private String email;
	
	public void modifyInfo() {
		......
	}
}
```



在UML类图中，类一般由三部分组成：

(1) 第一部分是类名：每个类都必须有一个名字，类名是一个字符串。

(2) 第二部分是类的属性(Attributes)：属性是指类的性质，即类的成员变量。一个类可以有任意多个属性，也可以没有属性

UML 规定属性的表示方式为：          **可见性 名称:类型 [ = 缺省值 ]**

其中：

- “可见性”表示该属性对于类外的元素而言是否可见，包括公有(public)、私有(private)和受保护(protected)三种，在类图中分别用符号+、-和#表示。
- “名称”表示属性名，用一个字符串表示。
- “类型”表示属性的数据类型，可以是基本数据类型，也可以是用户自定义类型。
- “缺省值”是一个可选项，即属性的初始值。

(3) 第三部分是类的操作(Operations)：操作是类的任意一个实例对象都可以使用的行为，是类的成员方法。

UML规定操作的表示方式为：			**可见性 名称(参数列表) [ : 返回类型]**

其中：

- “可见性”的定义与属性的可见性定义相同。
- “名称”即方法名，用一个字符串表示。
- “参数列表”表示方法的参数，其语法与属性的定义相似，参数个数是任意的，多个参数之间用逗号“，”隔开。
- “返回类型”是一个可选项，表示方法的返回值类型，依赖于具体的编程语言，可以是基本数据类型，也可以是用户自定义类型，还可以是空类型(void)，如果是构造方法，则无返回类型。



在类图2中，操作method1的可见性为public(+)，带入了一个Object类型的参数par，返回值为空(void)；操作method2的可见性为protected(#)，无参数，返回值为String类型；操作method3的可见性为private(-)，包含两个参数，其中一个参数为int类型，另一个为int[]类型，返回值为int类型。

![img](http://www.uml.org.cn/oobject/images/2012112312.jpg)

​                                                            图2 类图操作说明示意图



### 关联关系

关联(Association)关系是类与类之间最常用的一种关系，它是一种结构化关系，用于表示一类对象与另一类对象之间有联系，如汽车和轮胎、师傅和徒弟、班级和学生等等。在UML类图中，用实线连接有关联关系的对象所对应的类，在使用Java、C#和C++等编程语言实现关联关系时，通常将一个类的对象作为另一个类的成员变量。在使用类图表示关联关系时可以在关联线上标注角色名，一般使用一个表示两者之间关系的动词或者名词表示角色名（有时该名词为实例对象名），关系的两端代表两种不同的角色，因此在一个关联关系中可以包含两个角色名，角色名不是必须的，可以根据需要增加，其目的是使类之间的关系更加明确。

如在一个登录界面类LoginForm中包含一个JButton类型的注册按钮loginButton，它们之间可以表示为关联关系，代码实现时可以在LoginForm中定义一个名为loginButton的属性对象，其类型为JButton。如图1所示：

![img](http://www.uml.org.cn/oobject/images/2012112314.jpg)

```java
public class LoginForm {
private JButton loginButton; //定义为成员变量
……
}

public class JButton {
    ……
}
```

比较常见的两种关联关系

#### 聚合关系

​       聚合(Aggregation)关系表示整体与部分的关系。在聚合关系中，成员对象是整体对象的一部分，但是成员对象可以脱离整体对象独立存在。在UML中，聚合关系用带空心菱形的直线表示。例如：汽车发动机(Engine)是汽车(Car)的组成部分，但是汽车发动机可以独立存在，因此，汽车和发动机是聚合关系，如图6所示：

![img](http://www.uml.org.cn/oobject/images/2012112319.jpg)

​																				图6 聚合关系实例

​       在代码实现聚合关系时，成员对象通常作为**构造方法、Setter方法或业务方法**的  **参数**  **注入到整体对象中**，图6对应的Java代码片段如下：

```java
public class Car {
	private Engine engine;

    //构造注入
	public Car(Engine engine) {
		this.engine = engine;
	}
    
    //设值注入
public void setEngine(Engine engine) {
    this.engine = engine;
}
……
}

public class Engine {
	……
} 
```

#### 组合关系

​		组合(Composition)关系也表示类之间整体和部分的关系，但是在组合关系中整体对象可以控制成员对象的生命周期，一旦整体对象不存在，成员对象也将不存在，成员对象与整体对象之间具有同生共死的关系。在UML中，组合关系用带实心菱形的直线表示。例如：人的头(Head)与嘴巴(Mouth)，嘴巴是头的组成部分之一，而且如果头没了，嘴巴也就没了，因此头和嘴巴是组合关系，如图7所示：

![img](http://www.uml.org.cn/oobject/images/20121123110.jpg)

图7 组合关系实例

在代码实现组合关系时，通常在整体类的构造方法中直接实例化成员类，图7对应的Java代码片段如下：

```java
public class Head {
	private Mouth mouth;

	public Head() {
		mouth = new Mouth(); //实例化成员类
	}
……
}

public class Mouth {
	……
} 
```

### 依赖关系

​		依赖(Dependency)关系是一种使用关系，特定事物的改变有可能会影响到使用该事物的其他事物，在需要表示一个事物使用另一个事物时使用依赖关系。大多数情况下，依赖关系体现在某个类的方法使用另一个类的对象作为参数。在UML中，依赖关系用带箭头的虚线表示，由依赖的一方指向被依赖的一方。例如：驾驶员开车，在Driver类的drive()方法中将Car类型的对象car作为一个参数传递，以便在drive()方法中能够调用car的move()方法，且驾驶员的drive()方法依赖车的move()方法，因此类Driver依赖类Car，如图1所示：

![img](http://www.uml.org.cn/oobject/images/20121123111.jpg)

​																			图1 依赖关系实例

在系统实施阶段，依赖关系通常通过三种方式来实现，第一种也是最常用的一种方式是如图1所示的**将一个类的对象作为另一个类中方法的参数**，第二种方式是**在一个类的方法中将另一个类的对象作为其局部变量**，第三种方式是**在一个类的方法中调用另一个类的静态方法**。图1对应的Java代码片段如下：

```java
public class Driver {
	public void drive(Car car) {
		car.move();
	}
    ……
}

public class Car {
	public void move() {
		......
	}
    ……
}  
```

### 泛化关系

​		泛化(Generalization)关系也就是**继承关系**，用于描述父类与子类之间的关系，父类又称作基类或超类，子类又称作派生类。在UML中，泛化关系用带空心三角形的直线来表示。在代码实现时，我们使用面向对象的继承机制来实现泛化关系，如在Java语言中使用extends关键字、在C++/C#中使用冒号“：”来实现。例如：Student类和Teacher类都是Person类的子类，Student类和Teacher类继承了Person类的属性和方法，Person类的属性包含姓名(name)和年龄(age)，每一个Student和Teacher也都具有这两个属性，另外Student类增加了属性学号(studentNo)，Teacher类增加了属性教师编号(teacherNo)，Person类的方法包括行走move()和说话say()，Student类和Teacher类继承了这两个方法，而且Student类还新增方法study()，Teacher类还新增方法teach()。如图2所示：

![img](http://www.uml.org.cn/oobject/images/20121123112.jpg)

​																			图2 泛化关系实例

```java
//父类
public class Person {
protected String name;
protected int age;

public void move() {
        ……
}

    public void say() {
    ……
    }
}

//子类
public class Student extends Person {
private String studentNo;

public void study() {
    ……
    }
}

//子类
public class Teacher extends Person {
private String teacherNo;

public void teach() {
    ……
    }
}
```

### 接口与实现关系

在很多面向对象语言中都引入了接口的概念，如Java、C#等，在接口中，通常没有属性，而且所有的操作都是抽象的，只有操作的声明，没有操作的实现。UML中用与类的表示法类似的方式表示接口，如图3所示：

![img](http://www.uml.org.cn/oobject/images/20121123113.jpg)

​																			图3 接口的UML图示

接口之间也可以有与类之间关系类似的继承关系和依赖关系，但是接口和类之间还存在一种实现(Realization)关系，在这种关系中，类实现了接口，类中的操作实现了接口中所声明的操作。在UML中，类与接口之间的实现关系用带空心三角形的虚线来表示。例如：定义了一个交通工具接口Vehicle，包含一个抽象操作move()，在类Ship和类Car中都实现了该move()操作，不过具体的实现细节将会不一样，如图4所示：

![img](http://www.uml.org.cn/oobject/images/20121123114.jpg)

​																			图4 实现关系实例

实现关系在编程实现时，不同的面向对象语言也提供了不同的语法，如在Java语言中使用implements关键字，而在C++/C#中使用冒号“：”来实现。图4对应的Java代码片段如下：

```java
public interface Vehicle {
public void move();
}

public class Ship implements Vehicle {
public void move() {
    ……
    }
}

public class Car implements Vehicle {
public void move() {
    ……
    }
}
```

## 正则表达式

**需要注意，在JAVA之中需要使用两个"\\\\"才能表示一个"\\"**

**上面这句话看源代码可能更加清楚一点**

[\u4E00-\u9FA5]汉字

[\uFE30-\uFFA0]全角字符



## 异常

### 关键字

try

catch

finally

throws

throw

### 自定义异常

#### 步骤

1. 新建一个类以Exception为后缀，继承Exception类或者RuntimeException.
   1. Exception: 编译时异常，异常可能比较频繁出现时选用
   2. RuntimeException：运行时异常，异常出现比较少时选用
2. 提供两个构造方法，一个无参，一个带有String参数。



## 集合

### 集合重要结论

1. **集合中存的是引用**，不是基本数据类型也不是Java对象
2. 可以使用**Collections**里面的方法将**线程不安全的数据结构改成线程安全**



### 选用原则

![容器选用原则](https://gitee.com/fighterSu/resources/raw/image/typora/202108141959204.png)



### 容器元素的迭代

本文中以ArrayList为例简介Java之中容器迭代元素的几种方式

### 初始数据结构定义

```java
List<String> testList = new ArrayList<>();
int numberOfElements = 5;
for(int i = 0; i < numberOfElements; i++){
    testList.add(String.valueOf(i));
}
```

### 1. 使用foreach方式进行迭代

适用对象：**实现了 Iterable 接口的数据结构**

```java
for(String element : testList){
    System.out.println(element);
}
```



### 2. 转为数组后遍历

适用对象：**实现了toArray()方法的数据结构**

```java
// 2. 转为数组后迭代
Object[] testArray = testList.toArray();
for (int length = testArray.length, i = 0; i < length; i++) {
     System.out.println(testArray[i]);
}
```



### 3. 利用size()和get()方法进行遍历

适用对象：**实现了size()以及get()方法的数据结构**

```java
// 3. 利用 .size() 获取容量， 利用 .get() 方法实现迭代
for(int length = testList.size(), i = 0; i < length; i++){
    System.out.println(testList.get(i));
}
```



### 4. 利用 .iterator()方法获取迭代器对象进行迭代

适用对象：**继承了Iterable接口的数据结构**

```java
// 4. 利用 .iterator()方法获取迭代器对象进行迭代
Iterator<String> iterator = testList.iterator();

while (iterator.hasNext()) {
    System.out.println(iterator.next());
}
```



### 可以利用迭代器进行元素删除

**Iterator类提供了remove()方法，可以删除之前使用next()后所指向的元素**

不过需要注意的是，**一次next()之后只能使用一次remove()，也就是说使用remove()后，迭代器不会自动指向下一个对象**

remove()源码注释说明了这一点

```java
* Removes from the underlying collection the last element returned
* by this iterator (optional operation).  This method can be called
* only once per call to {@link #next}.
```



**删除第一个和第三个元素**

```java
iterator = testList.iterator();
System.out.println(Arrays.toString(testList.toArray()));

// 5. 利用迭代器删除集合对象
/* 删除第一个和第三个元素 */
iterator.next();
// 删除元素后不会自动向后移动，需要手动移动，一个next()之后只能调用一次remove(),如果想要再次remove()必须先next()
iterator.remove();

/* 移除下下个元素 */
iterator.next();
iterator.next();
iterator.remove();
System.out.println(Arrays.toString(testList.toArray()));
```



### contains()和remove()方法底层

**底层都是equals()方法,如果数据结构之中存储的数据类型没有重写equls方法，那么会调用Object之中的equals方法，只有说同个变量才返回true**





### 容器深拷贝

**由于各类容器实现的clone()函数都是浅克隆，这里实现列表的深克隆，其他的类似即可**



```java
/**
 * 列表深拷贝
 * 
 * @param <E>     列表元素类型
 * @param srcList 源列表
 * @return 新列表
 */
@SuppressWarnings("unchecked")
public static <E> List<E> deepCopyList(List<E> srcList) {
	ByteArrayOutputStream byteOut = new ByteArrayOutputStream();
	try (byteOut; ObjectOutputStream out = new ObjectOutputStream(byteOut)) {
		out.writeObject(srcList);
		try (ByteArrayInputStream byteIn = new ByteArrayInputStream(byteOut.toByteArray());
				ObjectInputStream in = new ObjectInputStream(byteIn)) {
			return (List<E>) in.readObject();
		}
	} catch (Exception e) {
		e.printStackTrace();
	}
	return null;
}

/*
 * 在对象序列化之后 ,即把对象已经保存到文件中了,  又在Person类中添加了一个字段,修改了Person类结构,
 * 	再进行反序列化时, 出现了异常:
 *  java.io.InvalidClassException:
 *  	com.wkcto.chapter06.objectstream.Person; local class incompatible:
 * 		stream classdesc serialVersionUID = 3479771803741762411,
 * 		local class serialVersionUID = 1549311491347595402
 * 分析原因:
 * 		流中类的描述信息中 serialVersionUID的值与本地字节码文件中 serialVersionUID字段的值不		   相等引发的异常
 *
 * 		当类Person实现了Serializable接口后, 系统会自动的在Person类中增加一个						serialVersionUID序列化版本号字段
 * 		在lisi对象序列化时, serialVersionUID字段的值是: 3479771803741762411
 * 		当序列化后, 又在Person类添加了gender字段, 编译后,在字节码文件中重新生成了一个					serialVersionUID的值:1549311491347595402
 * 		在进行反序列化时, 系统会检查流中serialVernUsioID序列化版本号字段与本地字节码文件中			serialVersionUID字段的值是否一样
 * 			如果相等就认为是同一个类的对象, 如果这个serialVersionUID序列化版本号字段的值不相		等,就认为是不同类的对象
 * 解决方法:
 * 		保证反序列化时流中serialVersionUID字段 的值,与本地字节码文件中serialVersionUID字段的		   值相等即可
 * 		可以在Person类实现了Serializable接口后, 手动的添加一个serialVersionUID字段
 */

```



## Properties类的使用

**实质：Map<String, String>**



### 加载数据到对象

1. **新建输入流**
2. **新建Properties类**
3. **调用Properties对象的load方法，(管道传输数据)**



### 配置文件

**以 .properties 为后缀名**

**文件里面的格式：**

**\# 号后面是注释**

**key1=value1**

**key2=value2**



**注意：key和value和=不要之间不要加多余的空格，会出问题**

```java
// 获取资源文件，必须是在类路径 src 下并以 .properties 结尾的配置文件
// 路径不要加 .properties
ResourceBundle bundle = ResourceBundle.getBundle("com/fightersu/reflect/data");
String className = bundle.getString("className");
System.out.println(className);
```



### 业务使用场景

```java
/**
 * 定义Collection引用
 */
static Collection collection;

static {
    /*
     * 静态代码块, 在类加载内存后,在类使用前执行,
     *      有时, 这个类需要依赖一些外部资源, 就可以在静态代码块中加载这些依赖资源
     *      在本例中, 可以在静态代码块中从配置文件中读取Collection集合的实现类名
     */
    try {
        ResourceBundle bundle = ResourceBundle.getBundle("data");
        String className = bundle.getString("className");

        //通过反射技术 创建实例
        Class<?> class1 = Class.forName(className);
        collection = (Collection) class1.newInstance();
    } catch (ClassNotFoundException | InstantiationException | IllegalAccessException e) {
        e.printStackTrace();
    }
}

/**
 * 把数据添加到集合中
 */
public void addData() {
    collection.add("data11");
}

/**
 * 显示数据
 */
public void show() {
    System.out.println(collection);
}
```





## 序列化及反序列化

### 含义

1. **序列化：将Java对象拆分写入硬盘文件**
2. **反序列化：将硬盘之中拆分的Java对象重新组合成Java对象**



### 实现

**实现标记性接口 Serializable 接口**

**具体左右：让 JVM 自动生成序列化版本号**



**建议：**<u>**每个实现序列化的类自己写序列化版本号**</u>

**作用：以后能够随便更改类源码，并且还能反序列化出来之前序列化进去的数据**

![image-20210815230001728](https://gitee.com/fighterSu/resources/raw/image/typora/202108152300010.png)

![image-20210815230433987](https://gitee.com/fighterSu/resources/raw/image/typora/202108152304104.png)



### 关键字 transient

作用：**让被标记的属性不被序列化**





### JVM怎么区分类

1. **查看类名（包括前面的包名）是否一致**
2. **比对序列化版本号是否一致**



### 多个对象的序列化

1. **逐个对象进行序列化**，读取时连续读取即可

```java
DirectoryEntry one = new DirectoryEntry("one");
DirectoryEntry two = new DirectoryEntry("two");

try (ObjectOutputStream objectOutputStream = new ObjectOutputStream(
		new FileOutputStream("data"))) {
	objectOutputStream.writeObject(one);
	objectOutputStream.writeObject(two);
} catch (IOException e) {
	e.printStackTrace();
}
```

2. **利用容器进行多个对象序列化**，即直接将整个容器对象序列化存储，再读取出容器

```java
DirectoryEntry one = new DirectoryEntry("one");
DirectoryEntry two = new DirectoryEntry("two");
DirectoryEntry three = new DirectoryEntry("three");

// 2. 利用List等容器进行序列化
List<DirectoryEntry> list = new ArrayList<>(3);
list.add(one);
list.add(two);
list.add(three);

try (ObjectOutputStream objectOutputStream = new ObjectOutputStream(
		new FileOutputStream("data"))) {
	objectOutputStream.writeObject(list);
} catch (IOException e) {
	e.printStackTrace();
}

try (ObjectInputStream objectInputStream = new ObjectInputStream(
		new FileInputStream("data"))) {
	List<DirectoryEntry> list2 = (List<DirectoryEntry>) objectInputStream.readObject();
	for (DirectoryEntry directoryEntry : list2) {
		System.out.println(directoryEntry);
	}
} catch (Exception e) {
	e.printStackTrace();
}
```





## 进程与线程

### 概述

**进程：一个应用程序**

**线程：一个进程之中的执行场景或者说执行单元**

一个进程可以启动多个线程



**内存分配的问题**

1. 对于由同一个进程派生出来的**多个线程，共享堆和方法区**
2. **每个线程拥有属于自己的栈空间，用来执行方法，进栈弹栈**

![004-一个线程一个栈](https://gitee.com/fighterSu/resources/raw/image/typora/202108162149171.png)



### 线程start()和run()的不同

**start()方法的作用：启动分支线程，在JVM开辟新的栈空间，任务即完成，结束**

**启动的新的线程会自动调用run()方法执行任务，并且run()方法在支线程的支栈的底部**



**直接调用现象实例对象的run()方法，仅仅是调用一个方法，不会开辟新的线程，run()方法会在main方法的主栈里面**





### **守护线程**

守护线程是为其他线程提供服务的线程,也叫后台线程. JVM中垃圾回收器就是一个守护线程。

守护线程不能单独运行, 当JVM中只有守护线程时, JVM会退出。



### 线程创建的三种方式

1. **直接继承 Thread 类**
2. **实现 Runnable 接口**
3. **实现 Callable 接口（FutureTask）**
   1. **优点：支线程有返回值**
   2. **缺点：主线程需要等待知道支线程执行完毕返回执行结果才能继续向下执行**







### 常用方法

| 修饰符         | 函数名及描述                                                 |
| :------------- | ------------------------------------------------------------ |
| static  int    | activeCount() 返回活动线程数量                               |
| static Thread  | currentThread() 返回当前线程                                 |
| ClassLoader    | getContextClassLoader() 返回类加载器                         |
| long           | getId() 返回线程的id,每个线程都有唯一 的id                   |
| String         | getName() 返回线程名称.                                      |
| int            | getPriority() 返回线程优先级                                 |
| Thread.State   | getState() 返回线程状态                                      |
| void           | interrupt() 中断线程，可以强行唤醒睡眠中的线程，线程在抛出错误后，继续执行 |
| static boolean | interrupted() 判断线程的中断状态                             |
| boolean        | isAlive() 判断线程是否终止                                   |
| boolean        | isDaemon() 是否为守护线程                                    |
| boolean        | isInterrupted() 判断线程的中断状态                           |
| void           | join() 线程合并                                              |
| void           | run()                                                        |
| void           | setDaemon(boolean on) 设置线程为守护线程                     |
| void           | setName(String name) 设置线程名称                            |
| void           | setPriority(int newPriority) 设置优先级                      |
| static void    | sleep(long millis)线程睡眠(休眠)                             |
| void           | start() 开启新的线程                                         |
| void           | stop() 线程终止（过时）                                      |
| String         | toString()                                                   |
| static void    | yield() 线程让步                                             |



### 线程生命周期

![img](https://gitee.com/fighterSu/resources/raw/image/typora/202108151143298.png)



### 唤醒睡眠线程

```java
Thread t = new Thread(() ->{
    System.out.println("t start....");
    try{
        Thread.sleep(1000 * 60 * 60 * 24 * 365);
    } catch (Exception e) {
        e.printStackTrace();
    }
    System.out.println("t end....");
}, "t")
   
// 中止线程睡眠，会抛出 InterruptedException 异常，然后线程进入退出睡眠状态
t.interrupt();
```





### 合理终止线程

```java
public class ThreadTest01 {
    public static void main(String[] args) {
        int repeatCount = 10;
        // Thread thread = new Thread(() -> {
        //     for (int i = 0; i < repeatCount; i++) {
        //         System.out.println("支线程打印：" + i);
        //     }
        // }, "myThread");

        MyThread t = new MyThread();
        Thread thread = new Thread(t);

        thread.start();

        // 过时方法，直接杀死线程，不推荐使用，会丢失数据
        // thread.stop();

        // 正确做法，之后会自动退出
        // t.isRunning = false;

        for (int i = 0; i < repeatCount; i++) {
            System.out.println("main主线程打印：" + i);
            if(i == 6){
                t.isRunning = false;
            }
        }
    }
}

/**
 * 正确终止线程做法
 * 1. 添加一个布尔标记成员
 * 2. 在run()方法里面每次执行任务前判断线程是否存活
 * 3. 如果不再存活，则进行保存数据，然后退出（return;)即结束进程
 */
class MyThread implements Runnable {
    boolean isRunning = true;

    @Override
    public void run() {
        for (int repeatCount = 10, i = 0; i < repeatCount; i++) {
            if(isRunning) {
                System.out.println("支线程打印：" + i);
            }else{
                System.out.println("保存支线程需要保存的数据。。。支线程已经结束");
                return;// 退出，结束线程
            }
        }
    }
}
```



~~直接使用t.stop()~~,**这样会直接结束线程，来不及保存数据**，早已经过时



### 线程安全问题

**发生前提：**

1. **多线程并发**
2. **多个线程共享数据**
3. **共享数据会发生修改**



**解决思路：利用排队机制来解决，线程不能并发了。**

1. **尽量使用局部变量替代实例变量和静态变量**
2. **让对象不共享，多少个线程就相应创建多少个变量**



### synchronized 关键字的使用

**使用原则：**

1. **包含的代码越少越好**
2. **最好不要嵌套使用，可能造成死锁**

```java
// 同步语句
synchronized(对象){
    临界代码段
}

// 同步实例方法
public synchronized 返回类型 方法名（）{
    方法体
}

// 同步实例方法等效如下
public 返回类型 方法名（）{
    synchronized(this){
        方法体
    }
}

// 静态方法使用 synchronized 修饰，是类锁，一个类只有一个
// 当调用了Blank.save()时，调用Blank.take()的线程必须等待，反之亦然
public class Blank{
    int totalNumber = 20000;// 总余额
public static synchronized void save(int saveNumber) {
	System.out.println("save前余额：" + totalNumber);
	totalNumber += saveNumber;

}

public static synchronized void take(int takeNumber) {
	System.out.println("take前余额：" + totalNumber);
	totalNumber -= takeNumber;
}
}

// 有个比较特殊的对象锁
synchronized ("abc"){}
// "abc" 在字符串常量池之中，这样定义锁区的，会让整个程序所有使用这样定义的代码段同步
// 一段执行完了，另一端才能执行

// 局部变量不共享，不存在线程安全问题
// 使用时注意，同步对象不能改
// 经常出现，使用Integer等类型定义数据作为锁对象，
// 然后又对该对象进行加减，那就不是原来那个对象了

// 错误示例
Thread t1 =  new Thread(() -> {
    for (int i = 0; i < 10; i++) {
        synchronized (Blank.number) {
            Blank.number += 100;
        }
    }
}, "t1");

Thread t2 =  new Thread(() -> {
    for (int i = 0; i < 10; i++) {
        synchronized (Blank.number) {
            Blank.number += 100;
        }
    }
}, "t2");

class Blank {
    public static Integer number = 20000;
}


// 正确示例
Blank blank = new Blank();
Thread t1 =  new Thread(() -> {
    for (int i = 0; i < 10; i++) {
        synchronized (blank) {
            blank.number += 100;
        }
    }
}, "t1");

Thread t2 =  new Thread(() -> {
    for (int i = 0; i < 10; i++) {
        synchronized (blank) {
            blank.number += 100;
        }
    }
}, "t2");

class Blank {
    public int number = 20000;
}
```



## 反射

**反射依赖Class对象**

**Class对象代表的是类的整个字节码**

### 如何获取Class对象

```java
// 获取类字节码文件
// 1. 利用Class.forName(String classFullName)静态方法获取，参数为：完整类名
// 使用这个方法会进行类加载（一个类只会进行一次，静态代码块会在这时加载）
// 只想让一个类的静态代码块运行可以采取这个方法
Class stringClass = Class.forName("java.lang.String");

// 2. 每个类都有class属性，可以通过 类名.class 能够返回字节码文件
Class class1 = String.class;

// 3. Object类之中有个getClass()实例方法，所以所有类型对象都有getClass()实例方法
// 可以使用 对象名.getClass()方法获取能够返回字节码文件
String sting = "abc";
Class class2 = sting.getClass();
System.out.println(stringClass == class1);// true
System.out.println(stringClass == class2);// true
System.out.println(class1 == class2); // true

//4)基本类型，可以使用 包装类.TYPE 来获取
Class<?> class4 = int.class;
Class<?> class5 = Integer.class;
System.out.println( class4 == class5 );         //false
Class<?> class6 = Integer.TYPE; 
System.out.println( class4 == class6 );         //true
```



### 获取字段、方法、构造器

```java
// 以获取字段为例，其它的类锁
Class<?> userClass = User.class;

// System.out.println(userClass.getName());
// System.out.println(userClass.getSimpleName());

// 获取类定义的所以字段，与权限无关
// Field[] userFields = userClass.getDeclaredFields();

// 获取类以及其从父类继承的所有的权限为 public 的字段 
Field[] userFields = userClass.getFields();
for (Field field : userFields) {
    // 获取字段修饰符
    int fieldModifier = field.getModifiers();
    System.out.print(fieldModifier == 0 ? "" :
            Modifier.toString(fieldModifier) + " ");
    
    // 获取字段数据类型
    System.out.print(field.getType().getSimpleName() + " ");
    
    // 获取字段名字
    System.out.println(field.getName());
}
```



### 利用反射获取及设置属性

```java
// 访问和修改 private 修饰的属性会抛出以下错误，
// 可以使用 field.setAccessible(true)函数来打破封装，引发安全问题

//Exception in thread "main" java.lang.IllegalAccessException: 
//	class com.fightersu.reflect.FieldTest cannot access a member of class 		
//	com.fightersu.reflect.User with modifiers "private"at 						
//	java.base/jdk.internal.reflect.Reflection.newIllegalAccessException
//  (Reflection.java:361)

// 获取Class，使用Class.forName(完整类名)，这里的类名以后可以放在配置文件里面
Class<?> userClass = Class.forName("com.fightersu.reflect.User");

User user = new User(10, "张三");

// 设置属性三要素
//      要素1：对象
//      要素二：属性名
//      要素三：值

// user.setName("李四");

// 1. 利用反射新建对象
Object obj = userClass.newInstance();

// 2. 获取属性名
Field nameField = userClass.getDeclaredField("name");

// 利用反射可以打破封装，这是它的一个缺点，存在安全问题
nameField.setAccessible(true);
// 3. 设置属性值，利用获取到的 Field 对象，可以设置该类型任意对象的特定属性值
nameField.set(obj, "王五");
nameField.set(user, "李四");

// 获取属性两要素
//      要素1：对象
//      要素二：属性名
// System.out.println(user.getName());

// 3. 获取属性值，利用获取到的 Field 对象，可以获取该类型任意对象的特定属性值
System.out.println(nameField.get(obj));
System.out.println(nameField.get(user));
```



### 利用反射调用函数

```java
// 调用函数三要素
//       要素一：对象
//       要素二：方法名
//       要素三：形参表
// User user = new User();
// String info = user.login("admin", "admin") ? "登录成功" : "登录失败";
// System.out.println(info);
// System.out.println(user.login("superAdmin") ? "超级管理员登录成功" : "超级管理员登录失败");

// 获取Class
Class<?> userClass = Class.forName("com.fightersu.reflect.User");

// 1. 新建对象
Object obj = userClass.newInstance();

// 2. 获取方法
Method loginMethod1 = userClass.getDeclaredMethod("login", String.class);
Method loginMethod2 = 
    userClass.getDeclaredMethod("login", String.class, String.class);

// 3. 调用方法
System.out.println(
    (boolean) loginMethod2.invoke(obj, "admin", "admin") ? "登录成功" : "登录失败");
System.out.println(
    (boolean) loginMethod1.invoke(obj, "superAdmin") ? "超级管理员登录成功" 
    : "超级管理员登录失败");
```





### 利用反射实例化对象

```java
// 获取Class
Class<?> userClass = Class.forName("com.fightersu.reflect.User");

// 1. 直接使用 类.newInstance() 方法创建对象 (已过时)
Object obj1 = userClass.newInstance();
System.out.println(obj1);

//获取构造方法的要素
//        要素一：形参表

// 获取无参构造方法
Constructor<?> constructor1 = userClass.getConstructor();
Object obj2 = constructor1.newInstance();
System.out.println(obj2);

// 获取有参构造方法
Constructor<?> constructor2 = userClass.getConstructor(int.class, String.class);
Object obj3 = constructor2.newInstance(10, "李四");
System.out.println(obj3);
// 输出结果如下
// User{number=0, sex=false, age=0.0, name='anonymousUser'}
// User{number=0, sex=false, age=0.0, name='anonymousUser'}
// User{number=10, sex=false, age=0.0, name='李四'}
```





### 类加载器

分类：

1. **启动类加载器：rt.jar**
2. **扩展类加载器：ext\\\*.jar**
3. **应用类加载器：classpath（自定义环境变量classpath变量）**



String s = "abc";

**代码在开始执行之前，会将所需要类全部加载到JVM当中。**
		通过类加载器加载，看到以上代码类加载器会找String.class
		文件，找到就加载，那么是怎么进行加载的呢？



**首先通过“启动类加载器”加载。**
	**注意：启动类加载器专门加载：C:\Program Files\Java\jdk1.8.0_101\jre\lib\rt.jar**
	**rt.jar中都是JDK最核心的类库。**

**如果通过“启动类加载器”加载不到的时候，**
**会通过"扩展类加载器"加载。**
	**注意：扩展类加载器专门加载：C:\Program Files\Java\jdk1.8.0_101\jre\lib\ext\*.jar**

**如果“扩展类加载器”没有加载到，那么**
**会通过“应用类加载器”加载。**
	**注意：应用类加载器专门加载：classpath中的类。**



**java中为了保证类加载的安全，使用了双亲委派机制。**
		**优先从启动类加载器中加载，这个称为“父”**
		**“父”无法加载到，再从扩展类加载器中加载，**
		**这个称为“母”。双亲委派。如果都加载不到，**
		**才会考虑从应用类加载器中加载。直到加载**
		**到为止。**



### 利用类加载器获取文件绝对路径

**前提：文件存放在类路径下，即（src/\*\*下）**

```java
// 获取文件绝对路径再新建文件流
String filePath = Objects.requireNonNull(Thread.currentThread().
        getContextClassLoader().
        getResource("com/fightersu/reflect/data.properties")).getPath();
// System.out.println(filePath);
// FileReader reader = new FileReader(filePath);

// 直接获取输入流
InputStream reader =Thread.currentThread().
    getContextClassLoader().
    getResourceAsStream("com/fightersu/reflect/data.properties");

Properties classData = new Properties();
classData.load(reader);

// System.out.println(classData.getProperty("className"));
Class<?> targetClass = Class.forName(classData.getProperty("className"));
Object obj = targetClass.newInstance();
System.out.println(obj);
```





## 注解

### 定义语法

```java
[修饰符列表] @interface 注解名{
    ....
}
```



### 注解的属性

**可以在注解里面添加属性，语法如下：**

```java
[修饰符列表] @interface 注解名{
    属性类型 属性名(); // 不添加默认值 default 字段，使用时，必须使用"属性名"=属性值 声明字段
    属性类型 属性名() default 默认值; // 添加默认值 default 字段，使用时，可以不声明该字段
}

/**
 * 注解定义
 */
public @interface MyAnnotation {
    String name();
    String value() default "110";
}

/**
 * 注解使用
 */
@MyAnnotation(name = "李四")
public boolean login(String userName) {
    return "superAdmin".equals(userName);
}

/**
 * 特殊情况，当注解之中只有一个属性，且这个属性名为 value 时，在使用时可以不写"属性名"=字段
 * 直接使用(属性值)即可
 */
public @interface MyAnnotation {
    String value();
}

/**
 * 注解使用
 */
@MyAnnotation("李四")
public boolean login(String userName) {
    return "superAdmin".equals(userName);
}
```



### 元注解

**定义：用来注解注解类型的注解**

常用元注解：

1. **@Target(ElementType.类型)：标识这个注解能添加的位置，没有添加该注解即所有位置都可以添加注解**

   **参数说明：**

   ```java
   public enum ElementType {
       /** Class, interface (including annotation type), or enum declaration */
       TYPE,
   
       /** Field declaration (includes enum constants) */
       FIELD,
   
       /** Method declaration */
       METHOD,
   
       /** Formal parameter declaration */
       PARAMETER,
   
       /** Constructor declaration */
       CONSTRUCTOR,
   
       /** Local variable declaration */
       LOCAL_VARIABLE,
   
       /** Annotation type declaration */
       ANNOTATION_TYPE,
   
       /** Package declaration */
       PACKAGE,
   
       /**
        * Type parameter declaration
        *
        * @since 1.8
        */
       TYPE_PARAMETER,
   
       /**
        * Use of a type
        *
        * @since 1.8
        */
       TYPE_USE,
   
       /**
        * Module declaration.
        *
        * @since 9
        */
       MODULE
   }
   ```

   

2. **@Retention(RetentionPolic.保留时段)：**

   **参数说明：**

   ```java
   public enum RetentionPolicy {
       /**
        * Annotations are to be discarded by the compiler.
        * 只保留在源代码文件之中，class文件之中没有，编译时起作用
        */
       SOURCE,
   
       /**
        * Annotations are to be recorded in the class file by the compiler
        * but need not be retained by the VM at run time.  This is the default
        * behavior.
        * 保留在class文件之中
        */
       CLASS,
   
       /**
        * Annotations are to be recorded in the class file by the compiler and
        * retained by the VM at run time, so they may be read reflectively.
        *
        * @see java.lang.reflect.AnnotatedElement
        * 保留在class文件之中，并且可以通过反射机制获取
        */
       RUNTIME
   }
   ```

   

### 元注解使用实例

1. @Target()

```java
/**
 * @Target标签，标识注解使用范围
 * ElementType.TYPE 标识该注解只能添加在类上面，添加在Field或者Method上面时会报错
 */
@Target(ElementType.TYPE)
public @interface MyAnnotation {
}

// 正常
@MyAnnotation
public class MyClass{
}

// '@MyAnnotation' not applicable to method
@MyAnnotation
public m(){}

// '@MyAnnotation' not applicable to field
@MyAnnotation
public int number;
```

2. @Retention()

   ```java
   /**
    * RetentionPolicy.SOURCE
    * 只保留在源文件之中，编译后的class文件反编译看不到该注解
    * 在编译时起作用
    * 典型代表 @Override 覆盖父类方法标识，会检查方法覆盖是否合乎规范
    */
   // Override注解定义
   @Target(ElementType.METHOD)
   @Retention(RetentionPolicy.SOURCE)
   public @interface Override {
   }
   
   // java源代码文件之中定义
   @Override
   public String toString() {return "User{}";}
   
   // class文件反编译结果，没有保留 @Override 这个注解标识
   public String toString() {return "User{}";}
   
   /**
    * RetentionPolicy.CLASS
    * 保留在class文件之中，但通过反射无法获取
    */
   // 注解定义
   @Retention(RetentionPolicy.CLASS)
   public @interface MyAnnotation {
       String value() default "默认值";
   }
   
   // 注解使用
   @MyAnnotation
   public boolean login(String userName) {
       return "superAdmin".equals(userName);
   }
   
   // 测试
   Method loginMethod1 = userClass.getDeclaredMethod("login", String.class);
   
   if (loginMethod1.isAnnotationPresent(MyAnnotation.class)) {
       MyAnnotation myAnnotation = loginMethod1.getAnnotation(MyAnnotation.class);
       System.out.println(myAnnotation.value());
   } else {
       System.out.println("该注解不存在，或者无法通过反射获取");
   }
   // 输出：该注解不存在，或者无法通过反射获取
   // 原因：注解使用@Retention(RetentionPolicy.CLASS)标识，
   // 		虽然在class文件之中保留了下来，但是不能通过反射机制获取
   
   /**
    * RetentionPolicy.RUNTIME
    * 保留在class文件之中，并且可以通过反射获取
    * 典型代表 @Deprecated 方法已过时标识
    */
   
   // java源代码文件之中定义与class文件反编译结果一致，保留了 @Deprecated 注解标识
   // 标识该方法或类已过时，不建议再使用，有更好的替代方案
   @Deprecated
   public boolean login(String userName) {
       return "superAdmin".equals(userName);
   }
   
   // 使用已过时方法时会出现下面的情况
   String info = new User().login("admin", "admin") ? "登录成功" : "登录失败";
   // 提示'login(java.lang.String, java.lang.String)' 已经过时了
   
   // 注解定义
   @Retention(RetentionPolicy.RUNTIME)
   public @interface MyAnnotation {
       String value() default "默认值";
   }
   
   // 注解使用
   @MyAnnotation
   public boolean login(String userName) {
       return "superAdmin".equals(userName);
   }
   
   // 测试
   // 通过反射获取函数
   Method loginMethod1 = userClass.getDeclaredMethod("login", String.class);
   
   // 通过反射判断注解是否在该方法上面
   if (loginMethod1.isAnnotationPresent(MyAnnotation.class)) {
       // 通过反射获取注解
       MyAnnotation myAnnotation = loginMethod1.getAnnotation(MyAnnotation.class);
       // 通过注解获取注解的属性
       System.out.println(myAnnotation.value());
   } else {
       System.out.println("该注解不存在，或者无法通过反射获取");
   }
   // 输出：默认值
   // 原因：注解使用@Retention(RetentionPolicy.RUNTIME)标识，
   // 		不仅在class文件之中保留了下来，并且可以通过反射机制获取
   ```
   
   

### 通过反射获取注解

**前提条件：目标注解被  @Retention(RetentionPolicy.RUNTIME) 所注解**

**步骤：**

1. 获取class对象
2. 获取对应的Field，Method等
3. 利用 **isAnnotationPresent(注解class)** 函数判断注解是否存在
4. 利用 **getAnnotation(注解class)**  函数获取注解
5. 可以使用  **注解名.属性()** 来获取注解的属性





### 注解在开发之中的使用

**业务场景：**

**假设有这样一个注解，叫做：@Id
			这个注解只能出现在类上面，当这个类上有这个注解的时候，
			要求这个类中必须有一个 int 类型的 id 属性。如果没有这个属性
			就报异常。如果有这个属性则正常执行！**



**需求实现：**

```java
// 自定义异常类
public class HasNotSetIdFieldException extends Exception {
    public HasNotSetIdFieldException() {
    }

    public HasNotSetIdFieldException(String message) {
        super(message);
    }
}

// 定义 @Id 注解
// 必须使用 @Retention(RetentionPolicy.RUNTIME) 注解该自定义注解
// 否则无法通过反射获取类是否被 @Id 注解所修饰
// 通过Target标识只能在类上面使用
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface Id {
}

// 测试类
@Id
public class User {
    public double id;
}

// 测试内容
// 获取 class 对象
Class<?> userClass = Class.forName("com.fightersu.annotation.User");
// 被 @Id 注解
String targetFieldName = "id";
Field[] fields = userClass.getDeclaredFields();
boolean isOk = false;
for (Field field : fields) {
    if (targetFieldName.equals(field.getName()) && 
        field.getType().equals(int.class)) {
        isOk = true;
        break;
    }
}

if (!isOk) {
   throw new HasNotSetIdFieldException("被 @Id 所注解的类必须定义 int 类型的 id 变量");
}
// 抛出异常
// Exception in thread "main" com.fightersu.annotation.HasNotSetIdFieldException: 
// 被 @Id 所注解的类必须定义 int 类型的 id 成员
// 将 User类之中的 id 的类型改为 int ，则不会再抛出该异常
```











