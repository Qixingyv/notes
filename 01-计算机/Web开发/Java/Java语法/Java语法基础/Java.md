# 1 Java概述

## 1.1 Java发展历史

## 1.2 JVM、JRE、JDK

JVM：java虚拟机

加载.class并运行.class

JRE：java运行环境

除了包含JVM以外还包含了运行java程序所必须的环境

JRE = JVM+java系统类库(小零件)

JDK：java开发工具包

除了包含JRE以外还包含了开发java程序所必须的命令工具

**注意：**

- 运行java的最小环境为JRE
- 开发java的最小环境为JDK

## 1.3 编译运行环境

编译期：编译器编译.java文件，经过编译，生成.class字节码文件

运行期：解释器加载.class文件并解释运行

**特点：一次开发，到处使用**

>编译时不检查目录结构，解释器加载类时会检查目录结构

# 2 基础语法

## 2.1 注释

```java
//注释内容	单行注释
/*注释内容*/	多行注释
/**注释内容*/	文档注释
```

## 2.2 数据类型

Java是一种强类型语言，必须为每一个变量声明一个类型。共有8种基本类型：4种整型，2种浮点型，一种字符类型，一种布尔类型。

### 2.2.1 整型

| 类型  | 存储需求 | 取值范围                                             |
| ----- | -------- | ---------------------------------------------------- |
| byte  | 1字节    | -128~127                                             |
| short | 2字节    | -32768~32767                                         |
| int   | 4字节    | -2 147 483 648~2 147 483 647                         |
| long  | 8字节    | -9 223 372 036 854 775 808~9 223 372 036 854 775 807 |

> Java没有任何无符号形式的byte、short、int、long类型。

**关于字面量的注意事项：**

- 默认字面量为int类型
- 长整型数值有一个后缀L或l（推荐使用L）
- 十六进制有一个前缀0x或0X
- 8进制有一个前缀0（不建议使用8进制）
- 从Java7开始，可是使用0b或0B修饰二进制数字（如：0B10001000101111）
- 从Java7开始，可以通过使用_让字面量易读（如：100_000_000_000）

### 2.2.2 浮点型

| 类型   | 存储需求 | 取值范围                                    |
| ------ | -------- | ------------------------------------------- |
| float  | 4字节    | ±3.402 823 47E+38F（有效数为约6~7位）       |
| double | 8字节    | ±1.797 693 134 862 315 70E+（有效数约15位） |

**注意事项：**

- 字面量默认为double类型
- 如果需要float类型字面量，可以使用F或f修饰。也可以使用D或d修饰double类型。
- 所有非数值的值都认为是不同的
- 浮点型不适用于无法接受舍入误差的计算。如果不能接受误差，使用BigDecimal类。

### 2.2.3 字符串类型

- char类型再Java中占用两个字节空间
- Java中char类型使用Unicode字符集，采用UTF-16编码
- char类型字面量要用' '括起来
- char类型的值可以表示为16进制，范围从\u0000到\uFFFF
- Java中，也可以使用'\n'这种转义序列，但是转义序列会在解析代码之前得到处理。
- 小心注释中的\uXXXX，会导致语法错误
- 程序中最好不要使用char类型，用String类代替char

### 2.2.4 布尔类型

- 占用一个字节

- 整型和布尔类型不能相互转换（不能用0，1表示）
- 只有两个值：true，false

## 2.3 变量与常量

### 2.3.1 变量

**变量命名规则：**

- 要用英文，做到见名知意
- 只能包含字母、数字、_和$符，不能以数字开头
- 不能用关键字和保留名

**变量使用注意事项：**

- 千万不要使用未初始化的变量的值
- 在Java中，变量的声明要尽可能地靠近变量第一次使用的地方

### 2.3.2 常量

在Java中，常量就是值不变的变量

**常量命名规则**：

- 利用final修饰常量
- 常量名使用全大写
- 名字太长使用_分隔

```java
final int CM_PER_INCH = 222;
```

## 2.4 运算符

Java中，运算符优先级如下

| 运算符                                                       |  结合性  |
| ------------------------------------------------------------ | :------: |
| ()(方法调用)， []， .(点操作符)                              | 从左向右 |
| !, ~, ++, --, +(一元运算), -(一元运算), ()(强制类型转换), new | 从右向左 |
| *, /, %                                                      | 从左向右 |
| +, -                                                         | 从左向右 |
| <<, >>, >>>                                                  | 从左向右 |
| <, <=, >, >=, instanceof                                     | 从左向右 |
| ==, !=                                                       | 从左向右 |
| &                                                            | 从左向右 |
| ^                                                            | 从左向右 |
| \|                                                           | 从左向右 |
| &&                                                           | 从左向右 |
| \|\|                                                         | 从左向右 |
| ?:                                                           | 从右向左 |
| =, +=, -=, /=, %=, &=, \|=, ^=, <<=, >>=, >>>=               | 从右向左 |

### 2.4.1 算数运算符

- 当参与/运算的两个数都是整数时，表示整数除法。否则，表示浮点数除法

  ```java
  15/2 = 7;
  15/0.2 = 7.5
  ```

- 整数被0除会产生一个异常，而浮点数被0除会得到无穷大或NaN结果

  ```
  23 / 0 	//java.lang.ArithmeticException: / by zero
  2.3 / 0 = Infinity
  ```

### 2.4.2 类型转换

![类型转换图](C:\WorkSpace\Notes\Java笔记\Java基础知识\assets\类型转换图.jpg)

上图为自动转换，黑线表示转换没有损失，虚线表示有损失

- 如果两个数中有一个是double类型，另一个操作数也会转换为double类型
- 否则，如果其中一个操作数是float类型，另一个操作数将会转换为float类型
- 否则，如果其中一个操作数是long类型，另一个操作数将会是long类型
- 否则，两个操作数将被转换为int类型

- 强制转换类型的语法格式为：目标类型 变量名 = (目标类型名)变量名;

  ```java
  int num1 = 10;
  byte num2 = (byte) num1;
  ```

### 2.4.3 自增自减运算符

自增自减有前后缀之分，前缀在运算之前进行计算，后缀在运算之后进行计算。

```java
int a = 1;
System.out.print(++n);	//2

int a = 1;
System.out.print(n++);	//1
System.out.print(n);	//2
```

### 2.4.4 逻辑与、逻辑或

Java中，逻辑与和逻辑或遵循短路原则，即第一个操作数能够确定表达式的值，第二个操作数就不必计算了

## 2.5 字符串

Java没有内置的字符串类型，而是在标准Java类库中提供了一个预定义类。Java很多关于字符串的操作都是通过类中的方法实现的。

### 2.5.1 String类中字符串的一些机制

- Java字符串中的字符从0开始计数

  ```java
  String str = "Hello";
  //            01234		H是第0位，o是第4位
  ```

- 当一个字符串和一个非字符串的值进行拼接后，非字符串也会转换成字符串。任何一个Java对象都可以转换成字符串

- String类没有提供修改字符串中某个字符的方法。String类的对象是不可变的

- 当创建一个新的字符串字面量时，会将该字符串放进公共存储池。进行赋值操作时，实际上是将字符串的地址赋给变量。

- 编译器可以让字符串共享。如果两个String类型的变量存的是相同的字符串字面量，那么两个变量将指向同一个字符串字面量的地址

- 只有字符串字面量是共享的，通过拼接或截取等操作得到的字符串并不共享，因此不能使用==来判断两个字符串的相等性

- 注意，空串和null串是不一样的，空串的String引用会指向一个长度为0的字符串。而null串则不会指向任何一个字符串

### 2.5.2 String类的一些操作

```java
//String类的构造方法
String name = " ";	//直接将字面量赋给对象变量

/*
	也可以用byte型数组来创建新的字符串
	第一个参数：byte型数组名
	第二个参数：数组开始的位置
	第三个参数：数组结束的位置
	第四个参数：根据什么字符集进行转换
*/
String line = new String(data,0,len, StandardCharsets.UTF_8);

//int length(); 获取字符个数
String str = "我爱Java!";
int len = str.length(); 
System.out.println(len); //7

//String trim();	去除当前字符串两边的空白字符
String str = "  hello world            ";
System.out.println(str); //  hello world
str = str.trim(); 
System.out.println(str); //hello world

//String toUpperCase()和String toLowerCase()	将当前字符中的英文部分转为大写/小写
String str = "我爱Java!";
String upper = str.toUpperCase(); //将str中英文部分转为全大写
System.out.println(upper); //我爱JAVA!
 
String lower = str.toLowerCase(); //将str中英文部分转为全小写
System.out.println(lower); //我爱java!

//Boolean startsWith(String str)和Boolean endsWith(String str)	
//判断当前字符串是否是以给定的字符串开始/结束
 String str = "thinking in java";
 boolean starts = str.startsWith("think"); //判断str是否是以think开头的
 System.out.println("starts:"+starts); //true

 boolean ends = str.endsWith(".png"); //判断str是否是以.png结尾的
 System.out.println("ends:"+ends); //false

//char charAt(int num)返回当前字符串指定位置上的字符
//            0123456789
String str = "thinking in java";
char c = str.charAt(9); 
System.out.println(c); //i

//int indexOf(String str)和int lastIndexOf(String str)
//检索给定字符串在当前字符串中的开始位置
//                      111111
//            0123456789012345
String str = "thinking in java";
int index = str.indexOf("in"); //检索in在字符串str中的开始位置
System.out.println(index); //2

index = str.indexOf("in",3); //从下标为3的位置开始找in第一次出现的位置
System.out.println(index); //5

index = str.indexOf("IN"); //当前字符串不包含IN，所以返回-1
System.out.println(index); //-1

index = str.lastIndexOf("in"); //找in最后一次出现的位置
System.out.println(index); //9

//String substring(int num1,int num2) 截取当前字符串中指定范围内的字符串
//从num1开始，到num2结束，但不包括num2
//                      1
//            01234567890
String str = "www.tedu.cn";
String name = str.substring(4,8); //截到下标4到7范围的字符串
System.out.println(name); //tedu

name = str.substring(4); //从下标4开始一直截到末尾
System.out.println(name); //tedu.cn

//静态方法valueOf():将其他数据类型转换为String
int a = 123;
String s1 = String.valueOf(a); //将int型变量a转换为String类型并赋值给s1
System.out.println(s1); //123---字符串类型

double d = 123.456;
String s2 = String.valueOf(d); //将double型变量d转换为String类型并赋值给s2
System.out.println(s2); //123.456----字符串类型

/*
	String提供了一个方法:byte[] getBytes()可以将该字符串按照指定的字符集转换为
    对应的一组字节。参数指定的就是字符集。用StandardCharsets.UTF_8。
*/
String line = "你好，我叫XXX";
byte[] data = line.getBytes(StandardCharsets.UTF_8);
```

### 2.5.3 StringBuilder类

String类对于插入、替换等操作的支持性非常不好。在Java中，使用StringBuilder类来完成相关的操作。

```java
String str = "好好学习java";
//复制str中的内容到builder中-----好好学习java
StringBuilder builder = new StringBuilder(str);

//append():追加内容
builder.append("，为了找个好工作!");
System.out.println(builder); //好好学习java，为了找个好工作!

//replace():替换部分内容
builder.replace(9,16,"就是为了改变世界"); //替换下标9到15的
System.out.println(builder); //好好学习java，就是为了改变世界!

//delete():删除部分内容
builder.delete(0,8); //删除下标0到7的
System.out.println(builder); //，就是为了改变世界!

//insert():插入操作
builder.insert(0,"活着"); //从下标0的位置插入
System.out.println(builder); //活着，就是为了改变世界!

//reverse():翻转
builder.reverse(); //翻转内容
System.out.println(builder); //!界世变改了为是就，着活
```

## 2.6 流程控制

条件语句：else与最近的if为一组

多重选择：switch语句

```java
switch(choice)
{
	case 1:
		...
		break;
	case 2:
		...
		break;
	default:
		...
		break;
}
```

注：case标签可以是

- 类型为char、byte、short、int的常量表达式
- 枚举常量
- 从Java7开始，还可以是字符串字面量

## 2.7 方法：函数、过程

- 封装一段特定的业务逻辑功能
- 尽可能的独立，一个方法只干一件事
- 方法可以被反复多次调用
- 减少代码重复，有利于代码复用，有利于代码维护

```java
//方法定义
/*
	修饰词 返回值类型 方法名（参数列表）
	{
		方法体;
	}
*/

public static int add(int num1,int num2)
{
	int num3 = num1 + num2;
	return num3;
}

//方法调用
/*
	对于Java中的方法：
	- 修饰词根据实际情况设定
	- 返回值可以有变量接收，也可以没有变量接收
	- 传递参数时一定注意参数类型及数量
*/
public class{
    public static void main(String[] args)
    {
        int num1 = 1;
        int num2 = 2;
        int num3 = add(num1,num2);
        System.out.println(num3);
    }
    
    public static int add(int a,int b)
	{
		return a+b;
	}   
}
```

### return

```java
/*
	- 结束方法的执行
	- 返回结果给调用方
	- 用在有返回值方法中
*/
return value;

/*
	- 结束方法的执行
	- 用在无返回值的方法中
*/
return;
```

### 方法签名、方法重载

方法签名：方法名+参数类型。返回类型不是方法签名的一部分

重载：如果方法名相同，参数列表不相同。便出现了重载。编译器会根据给定的参数类型来匹配相应的方法。

### 方法参数

- Java程序设计语言总是采用按值调用
- 方法得到的是参数值的一个副本
- 对于基本类型，方法不能修改传递给它的任何参数变量的内容
- 对于引用类型，传参时传递的是对象的地址，方法可以改变对象的状态，但不能让该地址引用一个新对象

### 变长参数

JDK5之后推出了一个特性:变长参数

该特性用于适应那些传入参数的个数不固定的使用场景，使得使用一个方法就可以解决该问题，而无需穷尽所有参数个数组合的重载

```java
//一个方法里只能有一个变长参数，且必须是最后一个参数
public class ArgDemo {
    public static void main(String[] args)
    {
        dosome(1,"one");
        dosome(2,"one","two");
        dosome(1,"one","two","three");
    }
   
    public static void dosome(int a,String... arg)
    {
        System.out.println(arg.length);
        System.out.println(Arrays.toString(arg));
    }
}
```



# 3 面向对象

面向对象的程序是由对象组成的，每个对象包含对用户公开的特定功能部分和隐藏的实现部分

> 特定功能体现在通过对象调用方法
>
> 隐藏的实现体现在只需要关注方法能实现什么功能，不用关心方法是如何实现的

程序 = 算法+数据结构

## 3.1 对象与类

类是构造对象的模板或蓝图。由类构造对象的过程称为创建类的实例

### 3.1.1 类

#### 类的特点

1. 封装：封装就是将数据和行为组合在一个包中，并对对象的使用者隐藏具体的实现方式。

​		注：绝对不能让类中的方法直接访问其他类的实例字段

> 数据就是实例字段（每个对象的属性），行为就是每个对象的方法

2. 继承：可以通过扩展其他类来构建新类，在扩展一个已有的类时，这个扩展后的新类具有被扩展类的全部属性和方法

3. 多态：同一个方法，根据对象和对象变量的类型而采取的多种表现形式
   - 多态是方法的多态
   - 存在条件：
     - 有继承关系
     - 方法需要重写
     - 父类引用指向子类对象

#### 类与类之间的关系

- 依赖("uses-a")：如果一个类的方法使用或操纵另一个类的对象，我们就说一个类依赖于另一个类。应该尽可能地将互相依赖的类减至最少

  > 如果A类不知道B类的存在，A类就不会关心B类的改变，意味着B类的改变不会导致A类产生bug。

- 聚合("has-a")：类A的对象包含类B的对象

- 继承("is-a")：类A扩展类B，类A不仅包含类B的方法，还会有额外的一些功能

### 3.1.2 对象

#### 对象的特性

- 行为：可以对对象完成哪些操作，或者可以对对象应用哪些方法

- 状态：当调用方法时，对象该如何响应

  > 对象状态的改变必须通过调用该对象的方法实现，如果不经过方法调用就可以改变对象的状态，只能说明是破坏了封装性

- 标识：对象的状态不能完全描述一个对象，每个对象都有一个唯一的标识

  > 作为同一个类的实例，每个对象的标识总是不同的，状态也往往存在差异

### 3.1.3 Java中类的规则

在Java中，除了8大基本类型外，剩下的都是引用类型

Java中的类分为两种：一种是Java官方的准备的预定义类，一种是用户自己定义的自定义类

```java
class Student{
	private String name;
	private int age;
	private String address;
    
    public Students{};
    public Students(String name,int age,String address)
    {
        this.name = name;
        this.age = age;
        this.address = address;
    }
    
    public void eat(String foodName)
    {
        System.out.println(this.name+"在吃"+foodName);
    }
    
    public void study()
    {
        System.out.println(this.name+"在学习");
    }
    
    public String getName()
    {
        return this.name;
    }
    public void setName(String name)
    {
        this.name = name;
    }
}
```

#### 对象、对象变量

要想使用对象，首先必须构造对象，并指定其初始状态。然后对对象应用方法。

```java
Student s = new Student();	//创建一个Student对象
Student s1 = s;	//引用一个已有的对象
```

- 变量s称为对象变量，对象变量存的是对象的地址。对象变量的默认初始值为null

  给对象变量赋初始值有两种方法：

  - 第一种是通过new操作符来返回一个新对象
  - 第二种是引用一个已有的对象

  注：不能对null引用应用方法，会引发空指针异常

- new操作符：构造一个新的Student类型的对象，返回新创建对象的引用

#### 构造器

作用：给成员变量（属性）赋初始值

构造器的规则：

- 构造器与类同名

- 每个类可以有一个以上的构造器。如果编写一个类时，没有编写构造器，Java会默认提供一个无参构造器，将所有实例字段设置为默认初始值。如果编写了构造器，Java不会提供该无参构造器

- 构造器可以有0个，1个或多个参数

- 构造器没有返回值

- 构造器总是伴随着new操作符一起调用

- 可以在构造器中调用另一个构造器，在构造器第一行使用this()

  ```java
  public Student(String name)
  {
  	this(name,"lillian",22);
  }
  ```

#### 实例字段、更改器方法、访问器方法

实例字段注意事项：

- 实例字段默认初始值为：数值为0，布尔值为false，对象引用为null。

  > 实例字段可以不进行赋初值操作，但不建议这么做

- 强烈建议将实例字段标记为private，如果将实例字段设置为其他类型，程序中其他方法都可以对其进行修改，因此需要将实例字段标记为private。特殊情况下，可以标记为其他的权限。

- 可以在类定义中直接为实例字段赋值

- 实例字段可以使用final修饰，这样的字段必须在构造对象时初始化，并且以后不能修改这个字段
  - final修饰符对于基本类型或不可变类的字段尤其有用.
  - 对于可变的类，只是让该字段不指向其他对象，对象的属性可以变化


```java
private String name;
private int age;
private String address;
```

调用一个方法，对象的状态会改变，这种方法称为更改器方法

```java
public void setName(String name)
{
	this.name = name;
}
```

调用一个方法，只访问对象数据，不修改对象数据的方法称为访问器方法

**注意：**不要编写返回可变对象引用的访问器方法

```java
public String getName()
{
	return this.name;
}
```

#### 隐式参数，显式参数

```java
s.eat("noodle");
```

eat方法有两个参数

- 一个是隐式参数，指的是调用eat方法的s对象

  > 隐式参数又被称为方法调用的目标或接收者

- 一个是显式参数，指的是参数列表的值

在编写类的时候，使用关键字this指示隐式参数

```java
public Students(String name,int age,String address)
{
        this.name = name;	//this.name可以理解为该对象的名字
        this.age = age;
        this.address = address;
    }
```

> 这样做的好处是避免了局部变量和实例字段同名出现的遮蔽问题（局部变量遮蔽实例变量）

#### 静态字段和静态方法

静态字段规则：

- 当所有对象共享同一资源时，可以用使用静态字段。实例字段每个对象都有一个自己的副本，而对于静态字段，每个对象没有副本，只有一个

- 可以将常量加上static关键字，使其变成静态常量

  ```java
  private static final PER = 2;
  ```

静态方法规则：

- 静态方法由static关键字修饰

- 静态方法没有隐式参数，静态方法不在对象上执行

- 可以使用对象调用静态方法，但静态方法不能访问对象属性

- 当方法不需要访问对象状态，或只需要访问类的静态字段时，可以将方法定义为静态方法

  ```java
  public static void add(int num1,int num2){};
  ```

#### 初始化块和静态块

初始化块就是将一些代码放进块中，建议将初始化块放在字段之后定义

```java
class Employee
{
	private static int nextId;
    
	private int id;
	private String name;
	private double salary;
	
	{
		id = nextId;
		nextId++;
	}
	
	public Employee{}
}
```

> 一般情况下，不会使用初始化块，会将初始化代码放进构造器中

如果类的静态字段需要很复杂的初始化代码，可以使用静态块

```java
static
{
	private int nextId = 0;
}
```

#### 构造对象的流程

1. 如果构造器第一行调用了另一个构造器，则使用所提供的参数执行第二个构造器
2. 所有数据字段初始化为默认初始值（0，false,null）
3. 按照类中的顺序，执行所有字段初始化方法和初始化块
4. 执行构造器主体

### 3.1.4 包和类的使用

Java允许使用包将类组织在一个集合里。借助包，可以方便的组织管理自己的代码，还可以将自己的代码与他人的代码分开管理

#### 包

package:声明包

- 确保类名的唯一性，如果不同包下类名相同，表示的是不同的类
- 类的全称：包名.类名
- 为了包名的绝对唯一，会用因特网域名倒序的形式作为包名。对于不同的工程使用不同的子包
- 如果没有在源文件中放置包，则这个源文件中的类属于无名包

#### 类

import：导入类

- 同包的类可以直接访问

- 不同包的类不能直接访问，若想访问：

  - 使用完全限定名

    ```java
    oopdemo.Student s = new oopdemo.Student();
    ```

  - 使用import声明包

    ```java
    package oopdemo2;
    
    import oopdemo.Student;
    
    public class Test{
    	public static void main(String[] args)
        {
            Student s = new Student();
        }
    }
    ```

- 如果需要用两个包中同一个类名的类，则需要使用完全限定名

#### 关于类的编写和编译

- 在包中定位类是编译器的工作，类文件中的字节码总是使用完整的包名引用其他类
- 在一个类中，只能有一个公共类（由public修饰），可以有任意数目的非公共类
- 一个源文件中，有多少个类，就会编译出多少个.class文件
- 如果A类使用B类，在编译A类时，B类也会一起编译
- 在编译时，应该将源文件放在指定目录下。编译器会将类文件放在相同的目录结构中

  > 编译器在编译源文件时不检查目录结构，解释器加载类时会检查目录结构

## 3.2 继承

### 3.2.1 继承的作用

- 作用：代码复用
- 通过extends来实现继承
- 超类/父类：共有的属性和行为
  派生类/子类：特有的属性和行为
- 派生类既能访问自己的，也能访问超类，但是超类不能访问派生类的
- 一个超类可以有多个派生类，一个派生类只能有一个超类---------单一继承
- 具有传递性
- java规定：构造派生类之前必须先构造超类
- 在派生类的构造方法中若没有调用超类的构造方法，则默认super()调用超类的无参构造方法
- 在派生类的构造方法中若自己调用了超类的构造方法，则不再默认提供

### 3.2.2 super

super：指代当前对象的超类对象
super的用法:
super.成员变量名---------------------访问超类的成员变量
super.方法名()-------------------------调用超类的方法
super()-----------------------------------调用超类的构造方法

### 3.2.3 向上造型

- 超类型的引用指向派生类的对象
- 能点出来什么，看对象变量的类型

### 3.2.4 重写

- 发生在父子类中，方法名相同，参数列表相同
- 重写方法被调用时，看对象的类型

- 重写遵循"两同两小一大"原则：-----------了解，一般都是一模一样的
  - 两同：
    - 方法名相同
    - 参数列表相同
  - 两小：
    - 派生类方法的返回值类型小于或等于超类方法的。void和基本类型时，必须相等
    - 引用类型时，小于或等于派生类方法抛出的异常小于或等于超类方法的
  - 一大：
    - 派生类方法的访问权限大于或等于超类方法的

### 3.2.5 抽象方法

- 由abstract修饰
- 只有方法的定义，没有具体的实现(连{}都没有

### 3.2.6 抽象类

- 由abstract修饰
- 包含抽象方法的类必须是抽象类
- 抽象类不能被实例化(new对象)
- 抽象类是需要被继承的，派生类：
  - 重写所有抽象方法-----------------变不完整为完整
  - 也声明为抽象类---------------------一般不这么用
- 抽象类的意义：
  - 封装共有的属性和行为---------------代码复用
  - 为所有派生类提供统一的类型------向上造型
  - 可以包含抽象方法，为所有派生类提供统一的入口(能点出来)，派生类的行为不同，但入口是一致的，同时相当于定义了一个标准(强制重写)

### 3.2.7 内部类

#### 成员内部类

- 成员内部类：应用率低-------------------了解即可
- 类中套类，外面的称为外部类，里面的称为内部类
- 内部类通常只服务于外部类，对外不具备可见性
- 内部类对象通常在外部类中创建
- 内部类中可以直接访问外部类的成员(包括私有的)
- 内部类中有个隐式的引用指向了创建它的外部类对象：外部类名.this----API时会用

#### 匿名内部类

- 匿名内部类：-----------------------大大简化代码
- 若想创建一个类(派生类)的对象，并且对象只被创建一次，可以做成匿名内部类
- 在匿名内部类中默认外面的变量为final的----这是规定，记住就行了-----API时会用
- 面试题：问：内部类有独立的.class吗？ 答：有

### 3.2.8 类型转换

#### 自动类型转换

- 超类型的引用指向派生类的对象
- 能点出来什么，看引用的类型
- 能造型成为的数据类型有：超类+所实现的接口

#### 强制类型转换

- 引用所指向的对象，就是该类型
- 引用所指向的对象，实现了该接口或继承了该类
- 强转时若不符合如上条件，则发生ClassCastException类型转换异常
- 建议在强转之前先通过instanceof判断引用的对象是否是该类型

## 3.3 访问控制修饰符

访问控制修饰符：--------------------------保证数据的安全

- public：公开的，任何类
- private：私有的，本类
- protected：受保护的，本类、派生类、同包类
- 默认的：什么也不写，本类、同包类

> 说明：
> \- 类的访问权限只能是public或默认的
> \- 类中成员的访问权限如上4种都可以
> \- 访问权限由高到低依次为：public>protected>默认的>private

## 3.4 接口

- 是一种引用数据类型
- 由interface定义
- 只能包含常量和抽象方法
- 接口不能被实例化(new对象)
- 接口是需要被实现/继承的，实现类/派生类：
  ----必须重写所有抽象方法
- 一个类可以实现多个接口，用逗号分隔，若又继承又实现时，应先继承后实现
- 接口可以继承接口
