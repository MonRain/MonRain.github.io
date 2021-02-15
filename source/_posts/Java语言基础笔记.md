---
title: Java基础笔记
categories: Java
tags:
  - Java
  - 基础知识
date: 2021-02-15 10:16:31
---

# Java语言基础笔记
### interface ￼接口
```
￼[可见度] interface 接口名称 extend 其他接口{
	// 声明变量
	// 抽象方法
} 
```
interface接口可以理解为一种特殊的抽象类，对抽象类进一步特殊化，是抽象方法的集合。

接口不可以被***实例化***，但可以被***实现***。但是Java 8之后改进为允许定义默认方法和实现
更过详见gitee的wiki——《抽象类与接口》
### container 容器
container内容比较复杂，简单来说，是Java为开发者提前造好的轮子，就字面意思理解为==存放数据的容器==，解决基本数据类型（如数组）不能解决的问题。

此外还会涉及到数据结构、线程安全等的问题。

常用的部分container
> Collection **==元素==**的集合
>
> > 一、List 元素可重复

`List.toArray()把List转化为Array（Object数组）`

>>> ~~1. Vector：已经被弃用(From JDK1.5)，**线程同步**~~
>>>
>>> > ~~Stack：是满足后进先出的容器，但LinkedList可以实现所有栈功能~~

>>> 2. ArrayList：可以**动态增长**的数组，默认长度是10。
随机访问快，插入删除慢。
>>> 3. **LinkedList：同时属于Collection和Queue）** ：用**链表**实现的容器。**可以实现很多队列、栈的数据结构。**
查找慢（遍历整个链表），插入删除快。

>> 二、Queue 队列 先进后出FILO
>>> **LinkedList （同时属于Collection和Queue）** 
>>> PriorityQueue 优先级队列

>> 三、Set 集合 元素不可重复
>>> 1. HashSet：底层使用散列函数，查询方面有优化
>>>>  LinkHashSet

>>> 2. TreeSet：底层使用红黑树

> Map **==键值对==**的集合
>> 1. HashMap 适合查找、删除、插入
>> 2. TreeMap 适合遍历

上述表述极不全面，待补充。

### 多态Polymorphism
多态指的是一个**引用**变量在程序中可以充当多种角色。细化地讲，比如可以将一个类的引用变量赋值为其子类的实例，如代码：

```
/*
* Animal.class是Cat.class Pig.class的* 父类
*/
Animal an1;
Animal an2 = new Animal();
Animal an3 = new Cat();
Animal an4 = new Pig();
```

在多态问题中，还存在函数调用的问题：

+ 如上例语句3中，an3只能调用Animal的方法，因为不能保证Animal中有子类的方法。只有声明部分明确声明为子类类型时，其引用变量才能调用对应子类的方法
+ **动态调用**问题：若子类和父类同时含有相同名称的方法，默认调用最能体现具体特点的子类方法。

instanceof运算符能判断具体的实例是否满足某个特定的类型。

### 异常Exception
##### 异常的介绍
Java中所有能被抛出的对象都视为Throwable类的实例，Exception和Error都是Throwable类的子类。
其中：

+ Error表示程序遇到的严重错误，几乎无法挽回。
+ Exception程度轻一点，比如打开一个不存在的文件。

进一步地，Java将运行时异常(RuntimeException)定性为非检查性异常(unchecked exceptions)，而所有其它异常定性为检查性异常(checked exceptions)。
**前者**在运行时出现，由于运行程序的各种不可预见性，所以称之为“unchecked”。
**后者**在程序开发中可预见，因此称之为“checked”（有些不运行就难以发现的异常也是checked异常）。

可以通过继承来定义新的异常类型。

##### 异常的抛出与处理
在异常问题中，事件的主体有两个：

+ 代码：代码中可以编写有效获取并处理异常的程序。

代码主动进行异常抛出使用`throw new ExceptionType();`
在函数声明时也可以使用`throws`来声明此函数可能抛出的异常类别：

```
public void abc() throws IOException,NullPointerException{
...
}
```

+ JVM：当出现代码中未处理的异常时，JVM会在打印运行时的堆栈跟踪信息之后终止程序的运行。如下段代码所示（例）

```
Exception in thread "main" java.lang.NullPointerException
at java.util.ArrayList.toArray(ArrayList.java:358)
at net.datastructures.HashChainMap.bucketGet(HashChainMap.java:35)
at net.datastructures.AbstractHashMap.get(AbstractHashMap.java:62)
at dsaj.design.Demonstration.main(Demonstration.java:12)
```

需要注意的是，在堆栈的跟踪信息中，每一步都有机会获取(catch)和处理异常，下级未处理的异常会被传递给上级调用函数，当异常层层上报都没有被有效处理，JVM就要插手打印信息并终止程序。
JVM在不经过代码时也可能抛出异常（如堆栈溢出）。

正确的异常处理方法为：

```
try{
	expressions;
} catch(EXCEPTIONtype1 e){
	expressions;
} catch(EXCEPTIONtype2 e){
	expressions;
} catch(EXCEPTIONtype3 e){
	expressions;
}...
...
final{
	expressions;
}
```
 值得注意的是，从JDK SE 7开始，允许catch中异常类型的合并：

```

try{
	expressions;
} catch(EXCEPTIONtype1| EXCEPTIONtype2| EXCEPTIONtype3 e){
	expressions;
}
...
final{
	expressions;
}
```
同样是从JDK SE7开始，尝试了一种新的“try with resource”(尚未研究，略）
### 强制类型转换与泛型
#### 强制类型转换Casting
主要关注引用类型的强制类型转换问题，一般包含两个子问题：

+ widening casting：**具体->抽象**，向上转换

从 **低层次** 类型T转换为 **高层次** 类型U。一般有以下三种情况：
> 子类对象T 转换为 父类对象U
> 子接口T 转换为 父接口U
> T使用了接口U

*类似多态问题*

+ narrow casting ：**抽象->具体**，向下转换

从 **高层次** 类型T转换为 **低层次** 类型U。一般有以下三种情况：
> 父类对象T 转换为 子类对象U
> 父接口T 转换为 子接口U
> U使用了接口T

一般来说，向上转换比较安全，编译器可以自动认定转换的正确性，但向下转换不行。

因此产生了运算符**`instanceof`**，语法为

**`Obj instanceof Type`**

当强制转换出现问题时，会出现`ClassCastException`异常

#### 泛型Generics
泛型产生的对应需求是，需要将同一类功能实现到不同类型的数据上，而不针对每个特定的类型都造出重复的车轮子。

+ 泛型的前世

泛型时自Java SE 5才开始有的，在此之前，使用Object类进行相似功能，如：

```Java
public class Test{
	Object A;
	public void Test(Object a){
		A = a;
	}
	public Object Method(){
		return A;
	}
}
```

由于Object的所有类型的父类（超类），因此可以接受所有类型的向上强制类型转换，所以看上去是个很好的选择，比如：

```Java
...
String str = “hello”
public Test test = new Test(str);
String ret = test.Method();
...
```

上述代码的初衷在于使用一个String类型创建一个Test实例test，然后获取其中存储的内部变量赋值给String对象ret。

但事实上，这是不安全的甚至编译不会通过，因为在最后一行代码中，涉及到了将test对象内部存储的Object对象test.A向下转换到类型String，因此需要显示强制类型转换：

```Java
String ret = (String) test.Method();
```

+ Java SE 5 之后到泛型

泛型支持在类型定义中使用参数化的类型定义变量：

```Java
public class Test<T>{
	T A;
	public void Test(Object a){
		A = a;
	}
	public T Method(){
		return A;
	}
}
```

上述代码中，声明了一个参数化类型T，并且在内部代码中视为一个已知类型使用，在实例化和后续使用时，如示例：

```Java
...
String str = “hello”
public Test<String> test;
test = new Test<String>(str);		// 可选1
test = new Test<>(str);			// 可选2
String ret = test.Method();
...
```

上述代码中，声明Test类型的实例test时，指定了Test内部的参数化类型为String，因此在实例test内部将T类型视为String类型，所以对于方法test.Method()返回的也自然就是String类型的对象，不需要进行类型转换了。

按照代码书写惯例，参数化类型一般采用单个大写字母。

需要注意的是，代码中“可选1”和“可选2”是两种实例化方法，最初是使用“可选1”的风格。在Java SE 7之后为了简化，采用了“可选2”的风格，这种风格也常被称为“菱形语法”，赋值时自动匹配待赋值引用变量声明时所规定的泛型类型。

如果没有指定类型`test = new Test(str)`，则视为使用经典类型（Java SE 5 之前），泛型被视为指定成Object类型

泛型不接受基本类型的制定，如double需要制定为其包装类型Double。

+ 泛型与数组联合使用的风险

泛型对象和数组一起使用时存在不安全的风险，具体来讲，Java允许使用带泛型的数组声明，但是不允许在实例化(new)的时候生成泛型实例直接赋值给数组。可以接受的折衷办法是使用旧版方法（不使用菱形语法）然后经过强制类型转换赋值给对应数组，尽管此办法可行，但编译时仍会报出warning，因为这是不安全的。一般以下两种情况会遇到此风险。

-> 外部创建泛型类的数组

```
Test<A>[] tests
tests = new Test<A>[10];		[错]
tests = new Test[10];			[对]
tests[0] = new Test<A>;			[对]
```

-> 泛型类内部创建泛型类数组

```
public class Test<A>{
	A[] a;
	public Test(){
		a = new A[10];					[错]
		a = (A[]) new Object[10]		[对]
	}
}
```

+ 方法中使用泛型

```
public class Test(){
	public <A> void method(A[] a){
		A temp=a[0];
	}
}
```

+ 限定泛型范围

```
public class Test <A extends ExistClass>
```

将泛型A可以指定的类型范围强制限定为某个已知的类型范围内，保证内部某些方法的可用性。

### 反射Reflect
代码引入：

```Java
//对于一个类Test
class Test{
	//定义方法
	public void method1(){
	}
	void method2(){
	}
	protected void method3(){
	}
	private void method4(){
	}
	//定义构造器
	public Test(){
	}
	Test(){
	}
	protected Test(){
	}
	private Test(){
	}
	//定义属性
	public int i = 1;
	private double d = 2.1;
}

//正常使用
class test01{
	public static void main(String[] args){
		Test test = new Test();
		test.method1(); 
	}
}
//使用反射
class test02{
	public static void main(String[] args){
		Class cls = Class.forName(“com.xxx.xxx.Test”);
		Object o = cls.newInstance();
		Method m1 = c.getMethod(“Method01”);
		m1.invoke(o);
	}
}
```

乍一看很蒙，这就是反射的第一印象，这东西不理解的时候看起来很蒙，理解之后看起来就很容易懂了。
#### 反射的理解层面

#### 反射的具体使用
##### 获取Class类对象（字节码信息）的几种方法
```
//方式1:通过具体实例对象的getClass()方法
Test test1 = new Test();
Class cls1 = test1.getClass();
//方式2:通过具体类的内置静态class属性
Class cls2 = Test.class;
//方式3:通过Class类的静态方法forName()方法[- 最常用 -]
Class cls3 = Class.forName(“com.xxx.xxx.Test”);
//方式4:通过类的加载器
ClassLoader cloader = Test.class.getClassLoader(); 
Class cls4 = loader.loadClass(“com.xxx.xxx.Test”);
```

注意：

+ 若判断`cls1==...==cls4`会返回`true`，因为他们都代表同一个类别Test的字节码。
+ 方法3是最常用的，用于动态指定操作的类、对象、方法等。由于其他三种方法中都已经包含指定特定的类、对象、方法进行操作，一般不会应用到反射的最大应用场景——动态指定类和对象。

##### 获取类的构造器以及对象的创建

```
Class c = Class.forName(“com.xxx.xxx.Test”);

//获取包括其父类在内的所有public构造器
Constructor[] cons = c.getConstructors();
//获取包括其父类在内的所有构造器（不局限于public）
//public protected default private都可以获取到
Constructor[] cons = c.getDeclaredConstructors();

//获取特定构造器
//若需要获取的构造器非pubilc，需要使用getDeclaredConstructor(<Class>);
//根据参数列表区分不同构造器
Constructor cons1 = c.getConstructor(int.class, double.class, SpecificClass);

//创建对象：使用构造器创建对象
Object o = cons1.newInstance();

```

##### 获取类的属性和赋值

```
Class c = Class.forName(“com.xxx.xxx.Test”);

Field[] feilds = c.getFeilds();
Field age = c.getFeild(“age”);
//同理若需要非public属性时
Field[] feilds = c.getDeclaredFeilds();
Field size = c.getDeclaredFeild(“bombsize”);

//还可以获取属性的修饰符、数据类型……

size.getModifiers();
size.getName();
size.getType();

//给属性赋值
size.set(??,34);//Error!

Object oset = cons1.newInstance();
size.set(oset, 34);//valid
```

##### 获取类的方法和调用

```
Class c = Class.forName(“com.xxx.xxx.Test”);

Method[] m1 = c.getMethods();
Method m2 = c.getMethod(“methodName”)

Object o = cons1.newInstance();
m2.Invoke(o);
```

#### 反射值得思考的问题
+ 简而言之，反射的用法很多，功能很强大，除上述用法之外，还可以获得其父类、接口、注解等一系列信息，具体实现方法可以参考API文档。
+ 反射的规律性，反射的功能很多，但是从规律上而言，大致可以这么看：批量获取所有目标可以调用getXxxs()或者getDeclaredXxxs()方法，获取单个目标可以调用不带s版本的对应方法。获取public目标可以调用getXxxs() 或者getXxx(...)，获取非public目标可以调用对应方法的Declared版本。
+ 反射是否破坏了面向对象的封装？回答：事实上确实破坏了，但是反射有它自己的实现环境，迎合了一部分需求。应该尽量保持面向对象的封装型，但在一定的需求情况下考虑变通。打个比方，1、男生不能进女厕所，2、女生不能进男生宿舍。

### 注解Annotation
#### 注解的理解层面
严格来说，注解对程序的逻辑实现没什么用。注解起到的作用是，在程序中充当**提醒**的角色，提醒程序猿、编译器等一系列让程序debug向正常运行方向的主体。
比如：

```
@Override
public void method1(){

}
```

这段代码的目的是重写某个主体内部的method1()方法。如果程序猿不写@Override，程序也能正常编译，如果逻辑没问题的话正常运行。但是一旦程序猿犯困，嘛method1方法名写错了，如果不写@Override的话，程序会继续创建一个新方法，默默无闻，傻不拉几；如果@Override存在，编译器会发现卧槽，你这方法我以前没见过，怎么重写？是不是你拼错了还是发生了啥？醒醒！
#### 常用的三个注解
```Java
@Override//重写方法
@Deprecated//标记已经被淘汰的方法
@SuppressWarning//标记以抑制编译器警告
```
####  注解的自定义
```
public @interface Haha(){
	//看上去是无参数方法，实际上理解成一个成员变量
	//约定俗成如果只有一个无参数方法（）就命名为value
	//String[]成员变量返回值（类型） value成员变量的名称 
	String[] value();
	//内部定义参数的注解——标记
	//内部没有定义参数的注解——元数据
}
```
#### 元注解
```Java
标记注解的注解——元注解
@Retention注解存活时间：源代码、编译、运行
@Target注解的使用范围：类型、属性、方法……
@Documented注解是否被收入文档
@Inherited继承于被标记的注解也会继承注解
相应的参数查看源码即可得
```
### I/O

Java的IO的四大基类
![Alt](https://i.loli.net/2020/11/14/SKMlFCAeRzQb3qH.jpg)

#### File类(java.io.File)
File类的理解：将系统中的文件、文件夹抽象为File类（联想到Linux系统中“万物皆文件”），如无特殊说明，本段后续统称文件和文件夹为**文件**。
##### File类的基本使用：

```
/* 创建一个关联于某个路径的file对象
* 理解：这里只是在程序中创建了一个对象，对象指向了某个路径。
* 至于这个路径到底正确与否、存在与否，至少到这里程序是不关心的。
* 相当于在程序中写入了一个地址记录，至于地址到底有没有人，有没有房子，那是以后的事，这里只是在纸上写了一行字
* /
File file = new File(“PATH”);
// 可以通过这个对象对指定文件进行一系列操作
String name = file.getName();
...

```

##### 那么这个路径到底对不对呢？
在程序执行过程中，如果路径出现问题，会报错。
一般来说面临两个问题：

1. 路径拼错了
2. 对应目录不存在

对于路径拼错这个问题，双十一重新买双手买个脑子能解决。

对于对应目录不存在这个问题，如果进行后续操作会遇到问题，比如打开一个不存在的文件，这是比较反智的。因此一般会进行如下操作：

```
File file = new File(“PATH”);

if (!file.exists()){
	try{
		file.createNewFile();
	}
	catch (IOException){
		e.printStackTrace();
	}
}else{
	System.out.println(“File exists!”);
	file.xxxxx();
}

//同理：上述代码创建文件，我们也可以创建文件夹

if (!file.exists()){
	try{
		file.mkdir();
	}
	catch (IOException){
		e.printStackTrace();
	}
}else{
	System.out.println(“Directory exists!”);
	file.xxxxx();
}
	/* 创建文件夹时，注意到有两个方法：
	* mkdir()
	* mkdirs()：可以一次性创建多层文件夹
	*/

```

#### 流
File类关注文件本身，可以操作文件的各种属性。但对于文件内部的具体内容，需要引入流的观念。
##### 流的分类
###### 输入流和输出流
“流”可以分为输入输出流，类比物理学的“参考系”说明方法而言，如果以Java程序为参考系，Java获取到的数据就是输入流，Java送出的数据就是输出流。
###### 字节流和字符流
字节流：(Byte Stream)针对二进制类型数据
字符流：(Char Stream)针对字符类型数据
具体使用字节流还是字符流还是要看要操作什么类型的数据。都是统称，下面还包含很多东西。
###### 节点流和处理流
节点流：直接和操作对象接触
处理流：处理已经存在的流（包括节点流和处理流）
#### 字节流：InputStream接口、OutputStream接口
##### FileInputStream类、FileOutStream类
读取文本和写入文本

```
File file = new File(PATH);
// 1. 读取数据
...
FileInputStream FIS = new FileInputStream(file);
// 逐字节读取文件中的数据
int i = FIS.read();//转换成ASCII码
while(i>0){
	i=FIS.read();
}

// 按字节数独取文件中的数据
Byte[] bytes = new Byte[1024];
int i = FIS.read(bytes);// 每次独取1KB，独取到的内容存放在bytes数据里，返回共读取的字节数

// 2. 写入数据
FileOutputStream FOS = new FOS(file);
FOS.write(bytes);
FOS.write(bytes,true);//true代表不覆盖写，而是在后边追加
...
```

#### 字符流：Reader接口、Writer接口
##### Reader接口 实现类

![Alt](https://i.loli.net/2020/11/12/pzCJ5WHZbwax1Bm.jpg)


> BufferedReader
> FileReader
> InputStreamReader
> ...

###### InputStreamReader类
InputStreamReader和FileInputStream很像，一个是按字节byte读取，一个是按照字符char读取

```
File file = new File(PATH);

FileInputStream FIS = new FileInputStream(file);
InputStreamReader ISR = new InputStreamReader(FIS,’gbk’);

char[] c = new char[64];
int read = ISR.read(c);
```

###### FileReader类

```
FileReader FR = new FileReader(file);

char[] c = new char[64];

int i = FR.read(c);

```

###### BufferReader类

```
FileInputStream FIS = new FileInputStream(file);

InputStreamReader ISR = new InputStreamReader(FIS);

BufferedReader BR = new BufferedReader(ISR);

String line = null;
//可以一行一行读取，不用破坏文本原来结构
while((line = BR.readLine())!=null){
	System.out.println(line);
}
```

##### Writer接口 实现类

与Reader接口差不多，对应理解即可

#### IO流的关闭 
JDK1.6之前需要手动关闭，之后Java可以自动关闭

```
try(//放各种需要关闭的可能出现异常的流
...
FileInputStream FIS = new FileInputStream(file);
...
)
{
...
}
```

推荐博客：
1、juejin.im/post/6844903910348603405
2、赵彦军的博客（CSDN）