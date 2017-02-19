---
title: Interview
categories:
  - default
tags:
  - default
date: 2017-02-09 09:38:53
updated: 2017-02-09 09:38:53
---

## C#面试题
118.a=10,b=15，在不用第三方变量的前提下，把a,b的值互换
答：a=a+b;b=a-b;a=a-b;
 
119.还有变态要求，需要代码最短呢。有两个结果：
1) a^=b^(b^=a^b); // 13个字节
2) a=b+(b=a)*0; // 11个字节
 
120.请简述面向对象的多态的特性及意义！
答：面向对象的编程使用了派生继承以及虚函数机制.一个本来指向基类的对象指针可以指向其派生类的.并访问从基类继承而来的成员变量和函数.而虚函数是专门为这个特性设计的,这个函数在每个基类的派生类中都是同一个名字,但函数体却并不一定相同,派生类往往为实现自己的功能而修改这个虚函数.这样用一个指针就能够实现对多种不同的派生类的访问, 并实现其派生类的特定功能(代码 )

116.重载与覆盖的区别？
答：1、方法的覆盖是子类和父类之间的关系，是垂直关系；方法的重载是同一个类中方法之间的关系，是水平关系。
　　2、覆盖只能由一个方法，或只能由一对方法产生关系；方法的重载是多个方法之间的关系。
　　3、覆盖要求参数列表相同；重载要求参数列表不同。
　　4、覆盖关系中，调用那个方法体，是根据对象的类型（对像对应存储空间类型）来决定；重载关系，是根据调用时的实参表与形参表来选择方法体的。

112.Static Nested Class 和 Inner Class的不同，说得越多越好
答：StaticNested Class是被声明为静态（static）的内部类，它可以不依赖于外部类实例被实例化。而通常的内部类需要在外部类实例化后才能实例化。

107.C#中 property 与 attribute的区别，他们各有什么用处，这种机制的好处在哪里？
答：attribute:自定义属性的基类;property :类中的属性
 
108.C#可否对内存进行直接的操作？
答：在.net下，.net引用了垃圾回收（GC）功能，它替代了程序员不过在C#中，不能直接实现Finalize方法，而是在析构函数中调用基类的Finalize()方法

101.在.Net中所有可序列化的类都被标记为_____? 
答：[serializable]
 
102.在.Net托管代码中我们不用担心内存漏洞，这是因为有了______?
答：GC。

99.类成员有_____种可访问形式？
答：this.; newClass().Method;

97.C#中 property 与 attribute的区别，他们各有什么用处，这种机制的好处在哪里？
答：一个是属性，用于存取类的字段，一个是特性，用来标识类，方法等的附加性质

91.什么是虚函数？什么是抽像函数？
答：虚函数：可由子类继承并重写的函数。
　　抽像函数：规定其非虚子类必须实现的函数，必须被重写。
 
92.什么是XML？
答：XML即可扩展标记语言。eXtensible Markup Language.标记是指计算机所能理解的信息符号，通过此种标记，计算机之间可以处理包含各种信息的文章等。如何定义这些标记，即可以选择国际通用的标记语言，比如HTML，也可以使用象XML这样由相关人士自由决定的标记语言，这就是语言的可扩展性。XML是从SGML中简化修改出来的。它主要用到的有XML、XSL和XPath等。

88.什么是反射？
答：动态获取程序集信息
 
89.用Singleton如何写设计模式
答：static属性里面new ,构造函数private

85.在c#中using和new这两个关键字有什么意义，请写出你所知道的意义？using 指令 和语句 new 创建实例 new 隐藏基类中方法。
答：using 引入名称空间或者使用非托管资源
　　new 新建实例或者隐藏父类方法
 
86.需要实现对一个字符串的处理,首先将该字符串首尾的空格去掉,如果字符串中间还有连续空格的话,仅保留一个空格,即允许字符串中间有多个空格,但连续的空格数不可超过一个.
答：string inputStr=" xx xx ";
　　inputStr = Regex.Replace(inputStr.Trim(),"*"," ");


77.谈谈final,finally, finalize的区别。
答：final－修饰符（关键字）如果一个类被声明为final，意味着它不能再派生出新的子类，不能作为父类被继承。因此一个类不能既被声明为 abstract的，又被声明为final的。将变量或方法声明为final，可以保证它们在使用中 不被改变。被声明为final的变量必须在声明时给定初值，而在以后的引用中只能读取，不可修改。被声明为 final的方法也同样只能使用，不能重载
　　finally－再异常处理时提供 finally 块来执行任何清除操作。如果抛出一个异常，那么相匹配的 catch 子句就会执行，然后控制就会进入 finally 块（如果有的话）。
　　finalize－方法名。Java 技术允许使用finalize() 方法在垃圾收集器将对像从内存中清除出去之前做必要的清理工作。这个方法是由垃圾收集器在确定这个对象没有被引用时对这个对象调用的。它是在 Object 类中定义的 ，因此所有的类都继承了它。子类覆盖 finalize() 方法以整理系统资源或者执行其他清理工作。finalize() 方法是在垃圾收集器删除对像之前对这个对象调用的。

76.short s1 = 1; s1 = s1 + 1;有什么错? short s1 = 1; s1 += 1;有什么错?
答：short s1 =1; s1 = s1 + 1;有错，
　　s1是short型，s1+1是int型,不能显式转化为short型。可修改为s1=(short)(s1 + 1) 。short s1 = 1;s1 += 1正确。


74.Set里的元素是不能重复的，那么用什么方法来区分重复与否呢? 是用==还是equals()? 它们有何区别?
答：Set里的元素是不能重复的，那么用iterator()方法来区分重复与否。equals()是判读两个Set是否相等。
　　equals()和==方法决定引用值是否指向同一对像equals()在类中被覆盖，为的是当两个分离的对象的内容和类型相配的话，返回真值。


73.List, Set, Map是否继承自Collection接口?
答：List，Set是Map不是


70.swtich是否能作用在byte上，是否能作用在long上，是否能作用在String上?
答：switch（expr1）中，expr1是一个整型，字符或字符串，因此可以作用在byte和long上，也可以作用在string上。


66.String s = new String("xyz");创建了几个String Object?
答：两个对象，一个是“xyx”,一个是指向“xyx”的引用对像s。
 
67.abstract class和interface有什么区别?
答：声明方法的存在而不去实现它的类被叫做抽像类（abstract class），它用于要创建一个体现某些基本行为的类，并为该类声明方法，但不能在该类中实现该类的情况。不能创建abstract 类的实例。然而可以创建一个变量，其类型是一个抽像类，并让它指向具体子类的一个实例。不能有抽像构造函数或抽像静态方法。Abstract 类的子类为它们父类中的所有抽像方法提供实现，否则它们也是抽像类为。取而代之，在子类中实现该方法。知道其行为的其它类可以在类中实现这些方法。
　　接口（interface）是抽像类的变体。在接口中，所有方法都是抽像的。多继承性可通过实现这样的接口而获得。接口中的所有方法都是抽像的，没有一个有程序体。接口只可以定义static final成员变量。接口的实现与子类相似，除了该实现类不能从接口定义中继承行为。当类实现特殊接口时，它定义（即将程序体给予）所有这种接口的方法。然后，它可以在实现了该接口的类的任何对像上调用接口的方法。由于有抽像类，它允许使用接口名作为引用变量的类型。通常的动态联编将生效。引用可以转换到接口类型或从接口类型转换，instanceof 运算符可以用来决定某对象的类是否实现了接口。




55.请编程实现一个冒泡排序算法？
答：int [] array= new int [*] ;
　　int temp = 0 ;
　　for (int i = 0 ; i < array.Length - 1 ;i++)
　　{
　　　　for (int j = i + 1 ; j < array.Length ;j++)
　　　　{
　　　　　　if (array[j] < array[i])
　　　　　　{
　　　　　　　　temp = array[i] ;
　　　　　　　　array[i] = array[j] ;
　　　　　　　　array[j] = temp ;
　　　　　　}
　　　　}
　　}
 
56.求以下表达式的值，写出您想到的一种或几种实现方法： 1-2+3-4+……+m
答：int Num =this.TextBox1.Text.ToString() ;
　　int Sum = 0 ;
　　for (int i = 0 ; i < Num + 1 ; i++)
　　{
　　　　if((i%2) == 1)
　　　　{
　　　　　　Sum += i ;
　　　　}
　　　　else
　　　　{
　　　　　　Sum = Sum - i ;
　　　　}
　　}
　　Console.WriteLine(Sum.ToString());
　　Console.ReadLine() ;


48.Const和ReadOnly？
答：Const用来申明编程时申明常量，ReadOnly用来申明运行时常量。
 
49.UDP和TCP连接有和异同？
答：TCP是传输控制协议，提供的是面向连接的，是可靠的，字节流服务，当用户和服务器彼此进行数据交互的时候，必须在他们数据交互前要进行TCP连接之后才能传输数据。TCP提供超时重拨，检验数据功能。UDP是用户数据报协议，是一个简单的面向数据报的传输协议，是不可靠的连接。
 
50.进程和线程分别该怎么理解？
答：进程是比线程大的程序运行单元，都是由操作系统所体会的系统运行单元
    一个程序中至少要有一个进程，有一个进程中，至少要有一个线程，线程的划分尺度要比进程要小，进程拥有独立的内存单元，线程是共享内存，从而极大的提高了程序的运行效率同一个进程中的多个线程可以并发执行。
 
51.在.NET中所有类的基类是？
答：object。
 
52.能用foreach遍历访问的对象需要实现？
答：需要实现IEnumerable接口和GetEnumerator()方法。
 
53.Heap与stack的差别？
答：Heap是堆，空间是由手动操作分配和释放的，它的存储区很大的自由存储区。
　　Stack是栈，是由是操作系统自动分配和释放的，栈上的空间是有限的。程序在编译期间变量和函数分配内存都是在栈上进行的，且在运行时函数调用时的参数的传递也是在栈上进行的。
 


42.Error和Exception有是区别？
答：error表示恢复不是不可能，但是很困难，exception表示一种实际或实现问题，它表示程序运行正常不可以发生的。
 
43.HashMap和Hashtable区别？
答：HashMap是Hashtable的轻量级实现，非线程安全的实现他们都实现了map接口，主要区别是HashMap键值可以为空null,效率可以高于Hashtable。
 
44.Collection和Collections的区别？
答：Collection是集合类的上级接口，Collections是针对集合类的一个帮助类，它提供一系列静态方法来实现对各种集合的搜索，排序，线程安全化操作。
 
45.Override, Overload,的区别？
答：Override是重写的意思，它表示重写基类的方法，而且方法的名称，返回类型，参数类型，参数个数要与基类相同。
　　Overload是重载是意思，它也表示重写基类的方法，但是只要方法名相同，别的可以不同。
 

41.数组有没有Length()这和方法？string有没有这个方法？
答：数组中没有这个方法，但有这个属性，string中有这个方法。


35.接口是否可以继承接口？抽象类是否可以实现接口？抽象类是否可以继承实体类？
答：接口是可以继承接口的，抽象类是可以实现接口的，抽象类可以继承实体类，但是有个条件，条件是，实体类必须要有明确的构造函数。
 
36.构造器Constructor是否可以被继承？是否可以被Override?
答：Constructor不可以被继承，因此不能被重写（Overriding），但可以被重载(Overloading).
 
37.是否可以继承String类？
答：因为String类是final类所以不可以继承string类。
 
38.当一个线程进入一个对象的方法后，其它线程是否可以进入该对象的方法？
答：不可以，一个对象的方法只能由一个线程访问。
 
39.用最有效的方法算出2乘以8等于几？
答：2<<3.

20.C#中，string str = null 与 string str =""，请尽量用文字说明区别
答：string str=""初始化对象分配空间，而stringstr=null初始化对象
 
21.详述.NET里class和struct的异同
答：结构与类共享几乎所有相同的语法，但结构比类受到的限制更多：尽管结构的静态字段可以初始化，结构实例字段声明还是不能使用初始值设定项。
　　结构不能声明默认构造函数（没有参数的构造函数）或析构函数。
　　结构的副本由编译器自动创建和销毁，因此不需要使用默认构造函数和析构函数。实际上，编译器通过为所有字段赋予默认值（参见默认值表）来实现默认构造函数。
　　结构不能从类或其他结构继承。
　　结构是值类型 -- 如果从结构创建一个对象并将该对象赋给某个变量，变量则包含结构的全部值。复制包含结构的变量时，将复制所有数据，对新副本所做的任何修改都不会改变旧副本的数据。
　　由于结构不使用引用，因此结构没有标识 -- 具有相同数据的两个值类型实例是无法区分的。C# 中的所有值类型本质上都继承自ValueType，后者继承自 Object。编译器可以在一个称为装箱的过程中将值类型转换为引用类型。
 
结构具有以下特点：
　　结构是值类型，而类是引用类型。
　　向方法传递结构时，结构是通过传值方式传递的，而不是作为引用传递的。
　　与类不同，结构的实例化可以不使用 new 运算符。
　　结构可以声明构造函数，但它们必须带参数。
　　一个结构不能从另一个结构或类继承，而且不能作为一个类的基。所有结构都直接继承自 System.ValueType，后者继承自 System.Object。
　　结构可以实现接口。
　　在结构中初始化实例字段是错误的。

 
19.列举ADO.NET中的共享类和数据库特定类
答：共享类：DataSet，DataTable，DataRow，DataColumn，DataRealtion，Constraint，DataColumnMapping，DataTableMapping
　　特定类：(x)Connection，(x)Command，(x)CommandBuilder，(x)DataAdapter，(x)DataReader，(x)Parameter，(x)Transaction

15.概述反射和序列化
答：反射：公共语言运行库加载器管理应用程序域。这种管理包括将每个程序集加载到相应的应用程序域以及控制每个程序集中类型层次结构的内存布局。程序集包含模块，而模块包含类型，类型又包含成员。反射则提供了封装程序集、模块和类型的对象。您可以使用反射动态地创建类型的实例，将类型绑定到现有对象，或从现有对象中获取类型。然后，可以调用类型的方法或访问其字段和属性。
　　序列化：序列化是将对象状态转换为可保持或传输的格式的过程。与序列化相对的是反序列化，它将流转换为对象。这两个过程结合起来，可以轻松地存储和传输数据。

15.new 关键字用法
答：(1)new 运算符 用于创建对象和调用构造函数。
    (2)new 修饰符 用于向基类成员隐藏继承成员。
    (3)new 约束   用于在泛型声明中约束可能用作类型参数的参数的类型。

14.C#中的接口和类有什么异同。
答：接口,是可以多继承,类只有单继承.接口强调了你必须实现,而没有具本实现的方法和虚类有点相似。

10.什么是装箱和拆箱？什么是重载？ 
答：装箱就是把值类型转成引用类型，拆箱相反把引用转换成值类型。
　　重载就是指一个方法名相同,参数个数不相同,返回值可以相同的方法。

8.如何把一个array复制到arrayist里 
答：foreach( object arr in array)arrayist.Add(arr);

7.2 C#中的委托是什么？事件是不是一种委托？
答：委托本质上是一种“方法接口”，它相当于C/C++中的函数指针，当然它比函数指针安全，在C#中通常用于事件处理。事件不是委托，不过由于事件的性质决定了处理它的程序逻辑能访问的参数，因此，在C#中处理事件的逻辑都包装为委托。

------

## 第一部分

1. 请简述值类型与引用类型的区别
  答：区别：
  1.值类型存储在内存栈中，引用类型数据存储在内存堆中，而内存单元中存放的是堆中存放的地址。
  2.值类型存取快，引用类型存取慢。
  3.值类型表示实际数据，引用类型表示指向存储在内存堆中的数据的指针和引用。
  4.栈的内存是自动释放的，堆内存是.NET中会由GC来自动释放。
  5.值类型继承自System.ValueType,引用类型继承自System.Object。
  6.值类型的变量直接存放实际的数据，而引用类型的变量存放的则是数据的地址，即对象的引用。
  7.值类型变量直接把变量的值保存在堆栈中，引用类型的变量把实际数据的地址保存在堆栈中。
  基于值类型的变量直接包含值。将一个值类型变量赋给另一个值类型变量时，将复制包含的值。这与引用类型变量的赋值不同，引用类型变量的赋值只复制对对象的引用，而不复制对象本身。
　所有的值类型均隐式派生自 System.ValueType。与引用类型不同，从值类型不可能派生出新的类型。但与引用类型相同的是，结构也可以实现接口。
　与引用类型不同，值类型不可能包含 null 值。然而，可空类型功能允许将null 赋给值类型。 每种值类型均有一个隐式的默认构造函数来初始化该类型的默认值。
　值类型主要由两类组成：结构、枚举； 结构分为以下几类：Numeric（数值）类型、整型、浮点型、decimal、bool、用户定义的结构。
　引用类型的变量又称为对象，可存储对实际数据的引用。声明引用类型的关键字：class、interface、delegate、内置引用类型： object、string

  可参考http://www.cnblogs.com/JimmyZhang/archive/2008/01/31/1059383.html

2. C#中所有引用类型的基类是什么
  答：引用类型的基类是System.Object 值类型的基类是System.ValueType同时，值类型也隐式继承自System.Object

3. 请简述ArrayList和List<Int>的主要区别
  答：ArrayList存在不安全类型‘（ArrayList会把所有插入其中的数据都当做Object来处理）装箱拆箱的操作（费时）List是接口，ArrayList是一个实现了该接口的类，可以被实例化。

4. 请简述GC（垃圾回收）产生的原因，并描述如何避免？
  答：GC回收堆上的内存，避免：
  1）减少new产生对象的次数
  2）使用公用的对象（静态成员）
  3）将String换为StringBuilder

5. 请描述Interface与抽象类之间的不同
  答：抽象类表示该类中可能已经有一些方法的具体定义，但接口就是公公只能定义各个方法的界面 ，不能具体的实现代码在成员方法中。类是子类用来继承的，当父类已经有实际功能的方法时该方法在子类中可以不必实现，直接引用父类的方法，子类也可以重写该父类的方法。实现接口的时候必须要实现接口中所有的方法，不能遗漏任何一个。

  参考http://www.cnblogs.com/seapub/archive/2012/08/08/2628433.html

6. 下列代码在运行中会产生几个临时对象？
  ```cs
  string a = new string("abc");
  a = (a.ToUpper() + "123").Substring(0, 2);    
  ```

  答：其实在C#中第一行是会出错的（Java中倒是可行）。应该这样初始化：
  `string b = new string(new char[]{'a','b','c'});`
 

7. 下列代码在运行中会发生什么问题？如何避免？
  ```cs
  List<int> ls = new List<int>(new int[] { 1, 2, 3, 4, 5 });
  foreach (int item in ls)
  {
      Console.WriteLine(item * item);
      ls.Remove(item);
  } 
  ```
  
  答：会产生运行时错误，因为foreach是只读的。不能一边遍历一边修改。

8. 请简述关键字Sealed用在类声明和函数声明时的作用，sealed修饰符有什么特点
  答：类声明时可防止其他类继承此类，在方法中声明则可防止派生类重写此方法。
  sealed 修饰符可以应用于类、实例方法和属性。密封类不能被继承；密封方法会重写基类中的方法，但其本身不能在任何派生类中进一步重写。当应用于方法或属性时，sealed 修饰符必须始终与 override一起使用。

9. 请简述private，public，protected，internal的区别
  答：
  public：对任何类和成员都公开，无限制访问
  private：仅对该类公开
  protected：对该类和其派生类公开
  internal：只能在包含该类的程序集中访问该类
  protected internal：protected + internal 访问仅限于从包含类派生的当前程序集或类型。


10. 反射的实现原理？
  答：审查元数据并收集关于它的类型信息的能力。
  参考http://blog.163.com/xuanmingzhiyou@yeah/blog/static/1424776762011612115124188/

  反射个人认为，就是得到程序集中的属性和方法。
  实现步骤：
  1,导入using System.Reflection;
  2，Assembly.Load("程序集")加载程序集,返回类型是一个Assembly
  3，   foreach (Type type in assembly.GetTypes())
              {
                  string t = type.Name;
              }
    得到程序集中所有类的名称
  4,Type type = assembly.GetType("程序集.类名");获取当前类的类型
  5,Activator.CreateInstance(type); 创建此类型实例
  6,MethodInfo mInfo = type.GetMethod("方法名");获取当前方法
  7,mInfo.Invoke(null,方法参数);

11. .Net与Mono的关系？
  答：Mono官网主页
  Mono is a software platform designed to allow developers to easily create cross platform applications. Sponsored by Xamarin, Mono is an open source implementation of Microsoft's .NET Framework based on the ECMA standards for C# and the Common Language Runtime.

  mono是.net的一个开源跨平台工具，就类似java虚拟机，java本身不是跨平台语言，但运行在虚拟机上就能够实现了跨平台。.net只能在windows下运行，mono可以实现跨平台跑，可以运行于linux，Unix，Mac OS等。

12. 简述Unity3D支持的作为脚本的语言的名称
  答：Unity的脚本语言基于Mono的.Net平台上运行，可以使用.NET库，这也为XML、数据库、正则表达式等问题提供了很好的解决方案。Unity里的脚本都会经过编译，他们的运行速度也很快。这三种语言实际上的功能和运行速度是一样的，区别主要体现在语言特性上。JavaScript：和网页中常用的JavaScript不一样，它编译后的运行速度很快，语法方面也会有不少区别。C#，Boo：可以看做是Python语言的变种，又糅合了Ruby和C#的特性，它是静态类型语言

13. Unity3D是否支持写成多线程程序？如果支持的话需要注意什么？
  答：参考http://www.unitymanual.com/3821.html
  仅能从主线程中访问Unity3D的组件，对象和Unity3D系统调用支持：如果同时你要处理很多事情或者与Unity的对象互动小可以用thread,否则使用coroutine。
  注意：C#中有lock这个关键字,以确保只有一个线程可以在特定时间内访问特定的对象

14. Unity3D的协程和C#线程之间的区别是什么？
  答：http://blog.csdn.net/kongbu0622/article/details/8775037
  多线程程序同时运行多个线程 ，而在任一指定时刻只有一个协程在运行，并且这个正在运行的协同程序只在必要时才被挂起。
  除主线程之外的线程无法访问Unity3D的对象、组件、方法。
  Unity3d没有多线程的概念，不过unity也给我们提供了StartCoroutine（协同程序）和LoadLevelAsync（异步加载关卡）后台加载场景的方法。 StartCoroutine为什么叫协同程序呢，所谓协同，就是当你在StartCoroutine的函数体里处理一段代码时，利用yield语句等待执行结果，这期间不影响主程序的继续执行，可以协同工作。而LoadLevelAsync则允许你在后台加载新资源和场景，所以再利用协同，你就可以前台用loading条或动画提示玩家游戏未卡死，同时后台协同处理加载的事宜asynchronous[e ɪˈ s ɪŋ kr ə n ə s] .synchronous同步。 

15. U3D中用于记录节点空间几何信息的组件名称，及其父类名称
  答：Transform 父类是 Component

16. 简述四元数的作用，四元数对欧拉角的优点？
  答：四元数用于表示旋转
  相对欧拉角的优点：
  1）能进行增量旋转
  2）避免万向锁
  3）给定方位的表达方式有两种，互为负（欧拉角有无数种表达方式）

17. 向量的点乘、叉乘以及归一化的意义？
  1）点乘描述了两个向量的相似程度，结果越大两向量越相似，还可表示投影
  2）叉乘得到的向量垂直于原来的两个向量
  3）标准化向量：用在只关系方向，不关心大小的时候

18. 矩阵相乘的意义及注意点
  用于表示线性变换：旋转、缩放、投影、平移、仿射
  注意矩阵的蠕变：误差的积累

19. 为何大家都在移动设备上寻求U3D原生GUI的替代方案
  不美观，OnGUI很耗费时间，使用不方便 

20. 请简述如何在不同分辨率下保持UI的一致性
  NGUI很好的解决了这一点，屏幕分辨率的自适应性，原理就是计算出屏幕的宽高比跟原来的预设的屏幕分辨率求出一个对比值，然后修改摄像机的size。
  原生GUI http://unity3d.9ria.com/?p=2587
  NGUI http://blog.csdn.net/mfc11/article/details/17681429

21. 为什么dynamic font在unicode环境下优于static font
  Unicode是国际组织制定的可以容纳世界上所有文字和符号的字符编码方案。使用动态字体时，Unity将不会预先生成一个与所有字体的字符纹理。当需要支持亚洲语言或者较大的字体的时候，若使用正常纹理，则字体的纹理将非常大。

22. Render的作用？描述MeshRender和SkinnedMeshRender的关系与不同
  A renderer is what makes an object appear on the screen。Mesh就是指模型的网格（同名组件是用于调整网格属性的），MeshFilter一般是用于获得模型网格的组件，而MeshRender是用于把网格渲染出来的组件，

23. 简述SkinnedMesh的实现原理
  http://blog.csdn.net/n5/article/details/3105872

24. 在场景中放置多个Camera并同时处于活动状态会发生什么
  答：游戏界面可以看到很多摄像机的混合，实际看到的画面由多个camera的画面组成，由depth、Clear Flag、Culling Mask都会影响最终合成效果。

25. Prefab的作用？如何在移动环境的设备下恰当地使用它？
  答：Prefab在实例化的时候用到，主要用于经常会用到的物体，属性方便修改
    在游戏运行时实例化，prefab相当于一个模板，对你已经有的素材、脚本、参数做一个默认的配置，以便于以后的修改，同事prefab打包的内容简化了导出的操作，便于团队的交流。
  http://www.cnblogs.com/88999660/archive/2013/03/15/2961663.html 

26. 如何销毁一个UnityEngine.Object及其子类
  答：使用Destroy()方法;

27. 为什么Unity3D中会发生在组件上出现数据丢失的情况？
  答：组件上绑定的对象被删除了

28. 如何安全的在不同工程间安全地迁移asset数据？三种
  答：
  将Assets目录和Library目录一起迁移
  导出包
  用unity自带的assets Server功能

29. MeshCollider和其他Collider的一个主要不同点？
  答：Convex？Meshcollider再快也是基于V3顶点~~ 建议还是用boxcollider，boxcollider本身是基于算法，没有面的概念。

30. 当一个细小的高速物体撞向另一个较大的物体时，会出现什么情况？如何避免？
  穿透（碰撞检测失败）
  http://forum.unity3d.com/threads/3353-collision-detection-at-high-speed

31. OnEnable、Awake、Start运行时的发生顺序？哪些可能在同一个对象周期中反复的发生？
  答Awake -》OnEnable-》Start，OnEnable在同一周期中可以反复地发生
  http://answers.unity3d.com/questions/217941/onenable-awake-start-order.html

32. 请简述OnBecameVisible及OnBecameInvisible的发生时机，以及这一对回调函数的意义？
  答: 当物体是否可见切换之时。可以用于只需要在物体可见时才进行的计算。

33. Unity3D如何获知场景中需要加载的数据？
  题目是获取的意思？Resource.Load，AssetBundle

34. MeshRender中material和sharedmaterial的区别？
  修改sharedMaterial将改变所有物体使用这个材质的外观，并且也改变储存在工程里的材质设置。不推荐修改由sharedMaterial返回的材质。如果你想修改渲染器的材质，使用material替代。

-----

## 第二部分

1. 请描述游戏动画有哪几种，以及其原理。
  答：主要有关节动画、骨骼动画、单一网格模型动画(关键帧动画)。 
  关节动画：把角色分成若干独立部分，一个部分对应一个网格模型，部分的动画连接成一个整体的动画，角色比较灵活，Quake2中使用这种动画；
  骨骼动画，广泛应用的动画方式，集成了以上两个方式的优点，骨骼按角色特点组成一定的层次结构，有关节相连，可做相对运动，皮肤作为单一网格蒙在骨骼之外，决定角色的外观；
  单一网格模型动画由一个完整的网格模型构成，在动画序列的关键帧里记录各个顶点的原位置及其改变量，然后插值运算实现动画效果，角色动画较真实。

2. alpha blend 工作原理
  答：Alpha Blend 实现透明效果，不过只能针对某块区域进行alpha操作，透明度可设。实际显示颜色 = 前景颜色*Alpha/255 + 背景颜色*(255-Alpha)/255

  alpha blend 用于做半透明效果。Color = (源颜色 *    源系数) OP (   目标颜色* 目标系数);其中OP（混合方式）有加，减，反减，取最小，取最大;

3. 写光照计算中的diffuse的计算公式
  答：diffuse = Kd x colorLight x max(N*L,0)；Kd 漫反射系数、colorLight 光的颜色、N 单位法线向量、L 由点指向光源的单位向量、其中N与L点乘，如果结果小于等于0，则漫反射为0。实际光照强度 I= 环境光(Iambient) + 漫反射光(Idiffuse) + 镜面高光(Ispecular);环境光：Iambient= Aintensity* Acolor; (Aintensity表示环境光强度，Acolor表示环境光颜色)
  漫反射光：Idiffuse = Dintensity*Dcolor*N.L;(Dintensity表示漫反射强度，Dcolor表示漫反射光颜色，N为该点的法向量，L为光源向量)
  镜面反射光：Ispecular = Sintensity*Scolor*(R.V)^n;(Sintensity表示镜面光照强度，Scolor表示镜面光颜色，R为光的反射向量，V为观察者向量，n称为镜面光指数),
  漫反射光(diffuse)计算公式为：Idiffuse = Dintensity*Dcolor*N.L ; (Dintensity表示漫反射强度，Dcolor表示漫反射光颜色，N为该点的法向量，L为光源向量)
  其他，3D渲染中，物体表面的光照计算公式为：
  I = 环境光(Iambient) + 漫反射光(Idiffuse) + 镜面高光(Ispecular);
    其中，环境光(ambient)计算公式为：
  Iambient= Aintensity* Acolor; (Aintensity表示环境光强度，Acolor表示环境光颜色)
    漫反射光(diffuse)计算公式为：
  Idiffuse = Dintensity*Dcolor*N.L ; (Dintensity表示漫反射强度，Dcolor表示漫反射光颜色，N为该点的法向量，L为光源向量)
  镜面光照(specular)计算公式为：
  Ispecular = Sintensity*Scolor*(R.V)n; (Sintensity表示镜面光照强度，Scolor表示镜面光颜色，R为光的反射向量，V为观察者向量)
  综上所得：整个光照公式为：
  I = Aintensity* Acolor+ Dintensity*Dcolor*N.L + Sintensity*Scolor*(R.V)n ;
  将一些值合并，并使用白色作为光照颜色，则上述公式可简化为：I = A + D*N.L + (R.V)n

  实际光照强度 I= 环境光(Iambient) + 漫反射光(Idiffuse) + 镜面高光(Ispecular);

  环境光：Iambient= Aintensity* Acolor; (Aintensity表示环境光强度，Acolor表示环境光颜色)

  漫反射光：Idiffuse = Dintensity*Dcolor*N.L;

  (Dintensity表示漫反射强度，Dcolor表示漫反射光颜色，N为该点的法向量，L为光源向量)

  镜面反射光：Ispecular = Sintensity*Scolor*(R.V)^n;

  (Sintensity表示镜面光照强度，Scolor表示镜面光颜色，R为光的反射向量，V为观察者向量，n称为镜面光指数)

4. LOD是什么，优缺点是什么？
  答：LOD(Level of detail)多层次细节，是最常用的游戏优化技术。它按照模型的位置和重要程度决定物体渲染的资源分配，降低非重要物体的面数和细节度，从而获得高效率的渲染运算。LOD技术即Levels of Detail的简称，意为多细节层次。LOD技术指根据物体模型的节点在显示环境中所处的位置和重要度，决定物体渲染的资源分配，降低非重要物体的面数和细节度，从而获得高效率的渲染运算。
  优点：可根据距离动态地选择渲染不同细节的模型
  缺点：加重美工的负担，要准备不同细节的同一模型，同样的会稍微增加游戏的容量。

5. 两种阴影判断的方法工作原理
  本影和半影：
  本影：景物表面上那些没有被光源直接照射的区域（全黑的轮廓分明的区域）。
  半影：景物表面上那些被某些特定光源直接照射但并非被所有特定光源直接照射的区域（半明半暗区域）
  工作原理：从光源处向物体的所有可见面投射光线，将这些面投影到场景中得到投影面，再将这些投影面与场景中的其他平面求交得出阴影多边形，保存这些阴影多边形信息，然后再按视点位置对场景进行相应处理得到所要求的视图（利用空间换时间，每次只需依据视点位置进行一次阴影计算即可，省去了一次消隐过程）
  阴影由两部分组成：本影与半影
  本影：景物表面上那些没有被光源直接照射的区域（全黑的轮廓分明的区域）
  半影：景物表面上那些被某些特定光源直接照射但并非被所有特定光源直接照射的区域（半明半暗区域）
  求阴影区域的方法：做两次消隐过程
  一次对每个光源进行消隐，求出对于光源而言不可见的区域L；
  一次对视点的位置进行消隐，求出对于视点而言可见的面S；
  shadow area= L ∩ S
  阴影分为两种：自身阴影和投射阴影
  自身阴影：因物体自身的遮挡而使光线照射不到它上面的某些可见面
  工作原理：利用背面剔除的方法求出，即假设视点在点光源的位置。
  投射阴影：因不透明物体遮挡光线使得场景中位于该物体后面的物体或区域受不到光照照射而形成的阴影
  工作原理：从光源处向物体的所有可见面投射光线，将这些面投影到场景中得到投影面，再将这些投影面与场景中的其他平面求交得出阴影多边形，保存这些阴影多边形信息，然后再按视点位置对场景进行相应处理得到所要求的视图（利用空间换时间，每次只需依据视点位置进行一次阴影计算即可，省去了一次消隐过程）
  若是动态光源此方法就无效了。

## Vertex Shader是什么？怎么计算？
  顶点着色器是一段执行在GPU上的程序，用来取代fixed pipeline中的transformation和lighting，Vertex Shader主要操作顶点。
  Vertex Shader对输入顶点完成了从local space到homogeneous space（齐次空间）的变换过程，homogeneous space即projection space的下一个space。在这其间共有world transformation, view transformation和projection transformation及lighting几个过程。


## MipMap是什么？作用？
  答：MipMapping：在三维计算机图形的贴图渲染中有常用的技术，为加快渲染进度和减少图像锯齿，贴图被处理成由一系列被预先计算和优化过的图片组成的文件，这样的贴图被称为MipMap。在三维计算机图形的贴图渲染中有一个常用的技术被称为Mipmapping。为了加快渲染速度和减少图像锯齿，贴图被处理成由一系列被预先计算和优化过的图片组成的文件,这样的贴图被称为 MIP map 或者 mipmap。

8. 用u3d实现2d游戏，有几种方式？
  答：一种用UI实现(GUI,NGUI...)，一种是采用3d实体对象（plane），绘制在3d对象上，调节摄像机，采用平行投影模式或则固定视角。   1.利用引擎自带的GUI
   2.把摄像机设为Orthographic，用面片作为2d元素
   3.利用第三方插件：NGUI、2dToolkit

8. Unity中碰撞器(Collider)和触发器(Trigger)的区别?
  碰撞器（Collider）有碰撞效果，IsTrigger=false，可以调用OnCollisionEnter/Stay/Exit函数
  触发器(Trigger)没有碰撞效果，isTrigger=true，可以调用OnTriggerEnter/Stay/Exit函数
  碰撞器是触发器的载体，而触发器只是碰撞器身上的一个属性。
  当Is Trigger=false时，碰撞器根据物理引擎引发碰撞，产生碰撞的效果，可以调用OnCollisionEnter/Stay/Exit函数；
  当Is Trigger=true时，碰撞器被物理引擎所忽略，没有碰撞效果，可以调用OnTriggerEnter/Stay/Exit函数。
  如果既要检测到物体的接触又不想让碰撞检测影响物体移动或要检测一个物件是否经过空间中的某个区域这时就可以用到触发器
  碰撞器会有碰撞的效果，IsTrigger = false,可以调用OnCollisionEnter/Stay/Exit函数。    触发器没有碰撞效果，isTrigger = true,可以调用OnTriggerEnter/stay/exit函数
  碰撞器是触发器的载体，而触发器只是碰撞器身上的一个属性。当Is Trigger=false时，碰撞器根据物理引擎引发碰撞，产生碰撞的效果，可以调用OnCollisionEnter/Stay/Exit函数;

  当Is Trigger=true时，碰撞器被物理引擎所忽略，没有碰撞效果，可以调用OnTriggerEnter/Stay/Exit函数。

  如果既要检测到物体的接触又不想让碰撞检测影响物体移动或要检测一个物件是否经过空间中的某个区域这时就可以用到触发器


9. 物体发生碰撞的必要条件
  答：需要检测碰撞的物体身上存在刚体组件（或被检测物体），也要碰撞器collider
  必须带有collider碰撞器和rigibody刚体属性或者人物控制器，其实人物控制器就包含了前两者，另外一个物体也要必须带有Collider，Collider分类：网格碰撞器，盒子碰撞器，胶囊碰撞器，球型碰撞器，地形碰撞器！
  两个物体都必须带有碰撞器Collider，其中一个物体还必须带有Rigidbody刚体。
  其中至少一个物体（运动的）必须带有碰撞器（collider）+刚体(Rigidbody)或者CharacterController，另一个物体也必须至少带有collider。　　两个物体都必须带有碰撞器(Collider)，其中一个物体还必须带有Rigidbody刚体，而且必须是运动的物体带有Rigidbody脚本才能检测到碰撞。

10. CharacterController和Rigidbody的区别
  Rigidbody具有完全真实物理的特性，而CharacterController可以说是受限的的Rigidbody，具有一定的物理效果但不是完全真实的。
  CharacterController自带胶囊碰撞器，里面好像封装了一个刚体,Rigidbody就是刚体，使物体带有物理的特性?
  Rigidbody具有完全真实物理的特性，Unity中物理系统最基本的一个组件，包含了常用的物理特性。而CharacterController可以说是受限的的Rigidbody，具有一定的物理效果但不是完全真实的，是Unity为了使开发者能方便的开发第一人称视角的游戏而封装的一个组件

11. 物体发生碰撞时，有几个阶段，分别对应的函数
  答：三个阶段， OnCollisionEnter， OnCollisionStay，OnCollisionExit

12. u3d中，几种施加力的方式，描述出来。
  rigidbody.AddForce/AddForceAtPosition,都是rigidbody的成员函数
  rigidbody.AddForce，rigidbody.AddForceAtPosition
  a)爆炸力（AddExplosionForce(force : float, forcePos : Vector3，radius : float, upwards : float, mode : ForceMode)），应用一个力到刚体来模拟爆炸效果,就是在爆炸力中心坐标position,搜索在radius范围内的刚体，对其释放力作用，超出radius范围的刚体不受力作用，爆炸力将随着刚体的距离线性减弱。
  b)力AddForce(force : Vector3, mode : ForceMode),主要施力给一个刚，使其移动。
  c)位置力AddForceAtPosition(force : Vector3, position : Vector3, mode : ForceMode), 在position施加一个力，施力的主体将会受到一个力和力矩。
  d)相对力AddRelativeForce(force : Vector3, mode : ForceMode),类似于AddForce；


13. 什么叫做链条关节
  Hinge Joint，可以模拟两个物体间用一根链条连接在一起的情况，能保持两个物体在一个固定距离内部相互移动而不产生作用力，但是达到固定距离后就会产生拉力。

14. 物体自旋转使用的函数叫什么
  答：transform.Rotate(eulerAngles : Vector3, relativeTo : Space = Space.self);

15. 物体绕某点旋转使用函数叫什么
  答：transform.RotateAround(point : Vector3, axis : Vector3, angles : float)

16. u3d提供了一个用于保存读取数据的类，（playerPrefs），请列出保存读取整形数据的函数
  答：PlayerPrefs.SetInt()，PlayerPrefs.GetInt(key : string, defaultValue : int = 0)

17. unity3d提供了几种光源，分别是什么
  答：平行光,点光源，聚光灯，环境光四种。
  平行光：Directional Light
  点光源：Point Light
  聚光灯：Spot Light
  区域光源：Area Light
  共4种，DirectionalLight、PointLight、SpotLight、AreaLight（用于烘焙）

18. unity3d从唤醒到销毁有一段生命周期，请列出系统自己调用的几个重要方法。
  答：void Awake(),void Start(), void Update(), void FixedUpdate(),void LateUpdate(), void OnGUI() ，void Reset(), OnDisable(), void OnDestroy()
  Awake——>Start——>Update——>FixedUpdate——>LateUpdate——>OnGUI——>Reset——>OnDisable——>OnDestroy
  ![](../_images/unity/09594731b420081605.png)

19. 物理更新一般在哪个系统函数里？
  答：void FixedUpdate()FixedUpdate,每固定帧绘制时执行一次，和update不同的是FixedUpdate是渲染帧执行，如果你的渲染帧效率低下的时候FixedUpdate调用次数就会跟着下降。FixedUpdate比较适合用于物理引擎的计算，因为是跟每帧的渲染有关。Update就比较适合做控制。

20. 移动相机动作在哪个函数里，为什么在这个函数里。
  答：void LateUpdate(),因为这个函数是在Update执行完毕才执行的，不然的话就有可能出现摄像机里面什么都看到的情况。LateUpdate，是在所有的update结束后才调用，比较适合用于命令脚本的执行。官网上例子是摄像机的跟随，都是所有的update操作完才进行摄像机的跟进，不然就有可能出现摄像机已经推进了，但是视角里还未有角色的空帧出现。

21. 当游戏中需要频繁创建一个物体对象时，我们需要怎么做来节省内存。
  做一个pool，游戏开始时预先实例化足够的数量，然后用的时候取不用的时候收回


## 什么是渲染管道？
  答：是指在显示器上为了显示出图像而经过的一系列必要操作。 渲染管道中的很多步骤，都要将几何物体从一个坐标系中变换到另一个坐标系中去。
  主要步骤有：本地坐标->视图坐标->背面裁剪->光照->裁剪->投影->视图变换->光栅化。

## 如何优化内存？
  答：有很多种方式，例如
  1.压缩自带类库；
  2.将暂时不用的以后还需要使用的物体隐藏起来而不是直接Destroy掉；
  3.释放AssetBundle占用的资源；
  4.降低模型的片面数，降低模型的骨骼数量，降低贴图的大小；
  5.使用光照贴图，使用多层次细节(LOD)，使用着色器(Shader)，使用预设(Prefab)。

3. 动态加载资源的方式？
  1.Resources.Load();
  2.AssetBundle
  Unity5.1版本后可以选择使用Git:   https://github.com/applexiaohao/LOAssetFramework.git

4. 什么是协同程序？
  答：在主线程运行时同时开启另一段逻辑处理，来协助当前程序的执行。换句话说，开启协程就是开启一个可以与程序并行的逻辑。可以用来控制运动、序列以及对象的行为。在主线程运行的同时开启另一段逻辑处理，来协助当前程序的执行，协程很像多线程，但是不是多线程，Unity的协程实在每帧结束之后去检测yield的条件是否满足。


1. 反向旋转动画的方法是什么？
2. 碰撞检测需要物体具备什么属性？
3. 用代码实现第三角色控制器
4. 实现吊机吊物体的功能
5. 获取、增加、删除组件的命令分别是什么？
  获取：GetComponent
  增加：AddComponent
  删除：Destroy

6. CrossFade命令作用是(C)
  A.动画放大 B.动画转换 C.动画的淡入为其他动画

7. Application.loadLevel命令为(A)
  A.加载关卡 B.异步加载关卡 C.加载动作

8. 调试记录到控制台的命令是什么？
  Debug.Log();

9. 编辑器类存放路径是什么？
  工程目录下的Assets/Editor文件夹下。

10. 使用原生GUI创建一个可以拖动的窗口命令是什么？
  GUI.DragWindow();

11. localPosition与Position的使用区别？
  localPosition：自身位置，相对于父级的变换的位置。 Position：在世界坐标transform的位置

12. 意义连线
  Mathf.Round  四舍五入
  Mathf.Clamp  限制
  Mathf.Lerp   插值

13. 写一个计时器工具，从整点开始计时，格式为：00:00:00

14. 写出Animation的五个方法

15. 怎么拿到一个对象上脚本的方法
  GameObject.GetComponent<>();

16. 上机题：用鼠标实现在场景中拖动物体，用鼠标滚轮实现缩放(用一个Cube即可)。

17. 请简述向量的点乘，向量的叉乘以及向量归一化的几何意义？
  点乘的几何意义是：计算两个向量之间的夹角，以及在某一方向上的投影；
  叉乘的几何意义是：创建垂直于平面，三角形，或者多边形的向量；参考：http://blog.sina.com.cn/s/blog_9283b6f601017hfw.html
 


## 在类的构造函数前加上static会报什么错?为什么?
  构造函数格式为 public+类名如果加上static会报错（静态构造函数不能有访问修饰符）
  原因：静态构造函数不允许访问修饰符，也不接受任何参数； 
  无论创建多少类型的对象，静态构造函数只执行一次； 
  运行库创建类实例或者首次访问静态成员之前，运行库调用静态构造函数； 
  静态构造函数执行先于任何实例级别的构造函数； 
  显然也就无法使用this和base来调用构造函数。

## C# String类型比stringBuilder类型的优势是什么?
  如果是处理字符串的话，用string中的方法每次都需要创建一个新的字符串对象并且分配新的内存地址，而stringBuilder是在原来的内存里对字符串进行修改，所以在字符串处理方面还是建议用stringBuilder这样比较节约内存。但是string 类的方法和功能仍然还是比stringBuilder类要强。
 
  string类由于具有不可变性（即对一个string对象进行任何更改时，其实都是创建另外一个string类的对象），所以当需要频繁的对一个string类对象进行更改的时候，建议使用StringBuilder类，StringBuilder类的原理是首先在内存中开辟一定大小的内存空间，当对此StringBuilder类对象进行更改时，如果内存空间大小不够，会对此内存空间进行扩充，而不是重新创建一个对象，这样如果对一个字符串对象进行频繁操作的时候，不会造成过多的内存浪费，其实本质上并没有很大区别，都是用来存储和操作字符串的，唯一的区别就在于性能上。
  String主要用于公共API，通用性好、用途广泛、读取性能高、占用内存小。
  StringBuilder主要用于拼接String，修改性能好。
  不过现在的编译器已经把 String 的 + 操作优化成 StringBuilder 了，所以一般用String就可以了，String是不可变的，所以天然线程同步。StringBuilder可变，非线程同步。
  http://zhidao.baidu.com/question/240364840.html?qbl=relate_question_0&word=String%C0%E0%D0%CD%B1%C8stringBuilder%C0%E0%D0%CD%B5%C4%D3%C5%CA%C6

3. C# 函数Func(string a, string b)用Lambda表达式怎么写?
  答：https://msdn.microsoft.com/zh-cn/library/bb397687.aspx

4. 数列1,1,2,3,5,8,13...第n位数是多少?用C#递归算法实现
  通项公式应该是an = a(n-1) + a(n-2)
  a3=2=1+1
  a4=3=1+1+1
  a5=1+1+1+2
  a6=1+1+1+2+3
  .........
  an=1+1+1+2+3+...+(n-2)+(n-3)=2+(n-2)(n-3)/2 (n>=3) 这是通项公式，至于递归？…

5. 一个简单的游戏,怪物会走动\攻击\死亡,游戏角色会走动,跳跃\攻击\格挡\死亡,还会接受玩家从输入端输入的指令,NPC会走动,他们彼此之间可以互相通信.请画出以上三种角色的UML图示.
答：

6. NGUI Button怎样接受用户点击并调用函数,具体方法名称是什么
  OnClick()主要是在UICamera脚本中用射线判断点击的物体并通过SendMessage调用OnClick() OnPress()等函数，可以说NGUI的按钮是通过发消息这个方式调用的。

7. 怎么判断两个平面是否相交?不能用碰撞体,说出计算方法
  答：https://www.baidu.com/s?wd=怎么判断两个平面是否相交&rsv_spt=1&issp=1&f=3&rsv_bp=1&rsv_idx=2&ie=utf-8&tn=baiduhome_pg&rsv_enter=0&rsv_pq=abcac720000083b6&rsv_t=ecdctoXQNURcfX%2FVm8MQdPtiBLttYO4HqJrZ1yNsNcxUqDe6DM4ovlQT4HdHZJMh6guT&inputT=2590&oq=C%23%20lamb&rsv_n=2&bs=c%23%20lambda

8. <愤怒的小鸟>给予初速度以后,怎么让小鸟受到重力和空气阻力的影响而绘制抛物线轨迹,说出具体的计算方法.
 Vector3 v代表初速度v'代表现在的速度，假设小鸟是沿的z轴也就是transform.forward方向运动的质量为1，那么v‘=v-new Vector3(0,g*t,f*t)，transform.Translate(v')做的就是抛物线运动（g为重力加速度不要用现实中的需要自己调试，f为阻力也要自己调试设置，t为时间）


先识别一下，你到了公司会让你干什么：

1.UI（面试题会偏向NGUI等ui操作，会考到一些简单的排序算法，数据结构，问题处理思路等）

2.数据逻辑层（会考到数据结构的搭配，配置表的构造等）

3.游戏控制流程（会考到状态机的设计，与服务器协议的设计，以及少量lua或python脚本编写内容）

4.项目架构（主程内容。。你懂得。反正我还不够格面这个。。）

UI的没什么好说的，去了之后天天layout，毫无乐趣，但是吧，新手都得虐过一次，才可以。数据逻辑层，也就是统筹整个游戏的后台运算数据，依照网络模块给予的数据，维护整个数据基的稳定和swift。这个工作与 unity

基本无关，但是是必不可少的，一般简历上写过什么acm之类的，会让你搞这个吧。游戏控制流程，至此开始进入高级程序员的行列，你会接触很多unity相关的内容，比如技能释放，角色换装，角色状态控制等等有趣的内容，通常如果你不是自废武功转3d或者再原来公司本来就干这个，只是干腻了跳槽而已，是不会让新手干这个的。项目架构这个略掉，我没资格谈这个。

说道这里本来应该结束废话的，但是可能引导大家对UI有了新的看法）——它不值得去做。其实不是的，UI这东西虽然无聊，但是是新手接触引擎，熟悉代码的最佳通道，同时也是所有公司愿意开放给各位新手的一个免费培训通道吧，因为UI代码很不值钱，你走了谁都能接上，所以他们愿意用新手。

下面来谈谈，如果我时面试出卷子的那个【sb】，我会问什么。这些问题都是项目中坑过我，害过我，让我哭让我痛得问题。

UI方面：

1.你觉得为什么UI摄像机和场景摄像机能协同工作，而且工作的这么尽如人意呢？

答案：UI摄像机和场景摄像机分别属于两个渲染层（Layer），所以它们之间的渲染互不干扰。它们工作得尽如人意（没有发生先后错乱，UI永远位于场景之上层）的原因就是因为摄像机深度（depth）控制的好。

2.你觉得怎么防止UI控件被点穿（如何过滤掉点击事件）。

答案：使用一个可渲染的物体或者pannel，绑定boxcollider组件即可。如果你单用一个不可渲染的物体（这里点cao一下UIwiget），即使设定了大小和boxcollider，也是无法屏蔽的。（坑在这里）。

3.关于UIGrid问题

答案：我基本不会使用UIGrid，我会直接在代码里设置localpos等等。（这个控件是很坑的）。

4.你对UI功能模块之间相互通信有什么好看法。（或者问成Broadcast和sendMes的看法）

答案：UI模块之间尽量解耦合，使用BroadCast机智或者delegate机智。由于Unity自身的BroadCast和SendMsg效率是很低的，（务必百度一下这俩的区别，都有，我懒得复制粘贴了。）所以推荐使用NGUI自带的那个消息机智。具体的使用方法，下载任何一款NGUI3.5左右的版本，都能清楚的看到。更或者自己实现一个msgerpool也可以，思路是使用泛型写法+字典+delegate，效率也很高。

5.众里寻他千百度，你怎么样能迅速找到某一个UI控件。

答案：分情况处理，这是一个优化策略的题目。首先如果这个控件我在awake的时候能知道，那么我会把它存成一个private变量，代码中使用的时候直接使用即可。如果不行，这个子物体是动态生成的（他可能有或者没有），那尽量使用FindChild，得到之后加以判断。如果不得不全局找一个东西（比如找到角色物体），才会用Find。

6.你对遮挡关系有什么好的策略。

答案：这种问题的诞生是由于Ngui2采用了一种zorder+depth方式处理遮挡关系造成的，已经在ngui3里完全屏蔽掉了。只要维护depth即可。

7.你对屏幕适配有什么好主意。

答案：屏幕适配根本没有完美的解决方案，如果是全屏模块，那么锁定目标机型，将目标机型做成满屏，其他机型充满宽或高后留白（也叫留黑边）处理即可。如果是屏幕模块（比如战斗界面，主城界面等等，你会看到下边的场景那种），需要采用Anchor来解决。NGUI2 Anchor有独立的控件，NGUI3Anchor已经被搞到Wighet和Pannel中，这个自己下去好好研究研究吧。

数据逻辑层方面

1.请简述一下你对数据结构的选取有什么看法

答案：批量取，经常遍历的数据，会采取List来存储，经常查找的会采取字典存储。同时如果多个字段比较重要（比如一份配置表经常会id索引以及name索引），我会开辟多个字典进行存储，牺牲空间换取逆向查找效率。

2.请把这份配置文件解析成你想要的数据结构，给我看看

答案：这份配置文件如果是xml或者json，那么它肯定会被先转换成HashTable，然后你根据配置表的可能拥有的字段（比如有一个物体，它有“使用后增加体力”这个字段，有的没有，你要用ContainsKey来检测是否有字段），挨个拆出并且存入你想要的数据结构中，如果你想要的数据结构是字典，需要注意的是在加入key的时候，应该询问是否包含了这个key，如果包含，则修改，如果不包含，则添加，，大家智力都没有问题。。试一试就会很清楚。

如果这个配置文件为excell，那么它将会被转化成一个string数组，数组的每一个元素，都是一个字段数据，按照配置表的顺序挨个取出来存入你得数据结构中即可。

3.请简述一下C#中，结构体和class的用法

（再次鸣谢面试我得那位同事，容忍了我回答的很烂）

答案：如果你是自废武功，从C++转到C#，这俩东西会让你大跌眼镜。C++中结构体和类几乎没有区别。C#中，结构体属于对象，而类属于引用，结构体不需要new出来，类必须new出来。这是个巨大的坑。比如你搞了一个结构体链表：

List<structA> structtestlist = new List<structA>();

然后给这个链表中注入数据，注入数据之后，你想更改其中的一个链节，你是这么做得

structA structlink = structtestlist[2];

structlink.data = 3;

这样做，根本没有修改到structtestlist中得值！因为声明structlink那一句话，是个对象，并不是指向那个（链节）的“指针”！

但是如果是class，你这么做毫无烦恼。

这个东西建议大家试试。很坑比。

4.接受到网络发来的数据，你会怎么办

答案：着手做项目之前，会搞一个txt，客户端内部消息协议罗列到上边，这个消息协议是网络协议的处理映射。比如网络协议告诉我，“因为你刚才吃了一个药，吃成功了，现在你得给我刷新，让用户大大能够看到”，我就必须通知所牵连的模块：“你得数据被刷新了，再老地方重新取一次，刷新界面给用户大大看”。牵连到得模块，比如就是战斗UI，它要把血条给加上去，还有character信息的数据基，更改当前血量数据等等。这些模块收到协议之后，改变自己的值：比如UI就会从数据基里取，而数据基会从网络包或者配置表里取等等。

游戏控制流程：

1.你对资源加载有什么看法。

答案：首先资源加载这个东西，就是老大难，用户一遍遍的吐槽加载的慢，但是谁也不愿意让加载的少导致后边必须加载而变得卡顿。对于比较耗费的加载，属于IO操作，在游戏刚登陆的时候，进行初始化加载。强制加载一些重要模块，比如登陆窗口（用户输入用户名密码的地方）等等。选择性加载一些必要的模块：如果这人需要新手引导，则加载新手引导模块，否则不加载，反之亦然。这些用户一定会用（或者一定不用）的模块，在登陆时候处理好。对于应用模块：比如我打开一个很庞大的UI，这个怎么加载，如果这个模块很吸金，用户用得概率很大，那么必须在登陆时加载，否则那只能用户打开的时候加载了，加载的时候要转圈提示用户。。关于场景加载更加的莫衷一是。既然大家被面到这一步了，相信也不需要我废话了，一个程序有一个程序的活法不是吗？

2.请给我设计一个状态机，完成一个简单的xxxx情景。

答：状态机并不是单纯的switch结构，如果你是新手，你说switch我可以容忍，如果你干了好些年 2D

x，然后想自废武功搞U3d了，再说switch就直接pass了。可以去搜一下相关资料，水不是很深，但篇幅有限。大概意思就是，每个对象都维护自己的状态机，状态机的状态改变，靠状态触发器。所有的状态（不是状态机。。状态机是一个主管它负责调度各种状态，状态是各种。。。状态。。哎不可言传啊。。），都会重写begin，excute，end这三个函数，本来敲了一大堆。。发现说也说不清楚，大家自己去看看吧还是。毕竟这只提供一种面试方式而已。。

3.角色换装，技能释放你会怎么做。

角色换装这个，各有各的活法，尸块换装，纸娃娃贴图等，这个莫衷一是，可以自己百度去，找一个自己喜欢的实现一下试试吧。技能释放水是很深的，这个能答就答，答不上来也不丢人。估计被面这些的，肯定都比我强，也就不需要我比比什么的了。。

4.动态更新有什么看法。

答：这个题牵扯到assetbundle问题。其中assetbundle面对资源的处理方法是截然不同的。比如对于prefab，可以很好的搞上去，对于二进制文件，也能搞得不错，但是对于代码这个东西，往往是大家争论的焦点——毕竟代码这个东西，iosAppStore是肯定不让你搞得，因为unity本身也不受苹果待见，所以它怕被秒杀，也不给用户提供更新脚本的功能。这就导致比如你更新了一个prefab，没法使用它上边挂的脚本！那更新有个jb用。。。提供一下解决代码更新的一点看法，现在我再尝试搞这一块，一孔之见，轻喷

安卓上可以使用unityreflaction机制进行编译后代码的动态更新，这个广大google开发者肯定是知道的。但是问题就是，这种机制会导致很庞大的开发成本——从代码层面完全跟ios搞了个大分支，如果是想跨平台的，这恐怕是不可取的吧。。

第二就是可以采用lua脚本无缝编写方法，因为unity脚本并不是真正的脚本，真正脚本的威力是它可以动态编译到宿主上，这点太牛逼了。lua就是这种牛逼脚本。所以你可以无缝换lua。。。这个没问题。所以立项就应该想好是lua还是什么的。。别回来做了一半，想改lua了，那就sb了。。

这两种机制在App Store上肯定是不让你搞的，第一种，越狱平台也不让搞，因为unity不让你搞。。

其实还有神秘的第三种做法，这第三种做法，就不分享了，毕竟还没有实验，说出来怕坑杀大家。。

不定期会更新这个帖子，时而一个月，时而半年的。。。大家见谅，希望对大家有帮助

1. 哪种实时光源是Unity中没有的？ A:点光源 B:方向光 C:聚光灯 D:日光灯 
2. 如何在Unity中创建地形系统？ A：Terrain->Create Terrain B：Component->Create Terrain C：Asset->Create Terrain D：Windows->Create Terrain 
3. 以下哪种操作步骤可以在场景中添加“Wind Zone”？ A：Terrain -> Wind Zone B：GameObject -> Create Other -> Wind Zone C：Component -> Physics -> Wind Zone D：Assets -> Create -> Wind Zone 
4. 在Unity编辑器中创建一个Directional Light，以下步骤正确的是？ A：Edit -> Rendering Setting -> Directional Light B：GameObject -> Create Other -> Directional Light C：Component -> Rendering -> Directional Light D：Assets -> Directional Light 
5. 下列哪一项不属于Camera中的“Clear Flags”？ A：Skybox B：Solid Color C：Depth Only D：Background 
6. 以下哪种脚本语言是Unity编辑器所不支持的？ A：JavaScript B：C# C：Boo D：Perl 
7. 对于Prefab，以下说法错误的是？ A：Prefab资源可以在项目中多次重复使用 B：由Prefab实例出的GameObject，其在Hierarchy视图中表现为蓝色 C：Prefab上的组件信息一经改变，其实例出的GameObject也会自动改变 D：实例出的GameObject上的组件信息一经改变，其对应的Prefab也会自动改变 
8. 下面哪种做法可以打开Unity的Asset Store？ A：Windows -> Asset Store B：Edit -> Asset Store C：File -> Asset Store D：Assets -> Asset Store 
9. 在哪个面板中可以修改物体的空间属性，如位置、朝向、大小等？ A：Project B：Inspector C：Hierarchy D：Toolbar 
10. 如何为一个Asset资源设定一个Label，从而能够方便准确的搜索到? A：在Project窗口中选中一个Asset,右键->Create->Label B：在Project窗口中选中一个Asset,右键->Add Label C：在Project窗口中选中一个Asset,在Inspector窗口中点击添加Label的图标 D：在Project窗口中选中一个Asset,在Inspector窗口中点击按钮“Add Label” 
1. Mecanim系统中，Body Mask的作用是？ A:指定身体的某一部分是否参与骨骼动画 B:指定身体的某一部分是否参与物理模拟 C:指定身体的某一部分是否可以输出骨骼信息 D:指定身体的某一部分是否参与渲染 
2. 以下哪种操作步骤可以打开Unity编辑器的Lightmapping视图？ A：File --> Lightmapping B：Assets --> Lightmapping C：Windows --> Lightmapping D：Component --> Lightmapping 
3. 下列关于光照贴图，说法错误的是？ A：使用光照贴图比使用实时光源渲染要快 B：可以降低游戏内存消耗 C：可以增加场景真实感 D：多个物体可以使用同一张光照贴图 
4. 如何为物体添加光照贴图所使用的UV？ A：不用添加，任何时候都会自动生成 B：更改物体导入设置，勾选”Generate Lightmap UVs” C：更改物体导入设置，勾选 “Swap UVs” D：更改物体导入设置，在UVs 选项中选择” Use Lightmaps” 
5. 在哪个模块下可以修改Render Path？ A：Camera B：Light C：Render Settings D：Project Settings->Quality 
6. 以下哪项技术不是目前Unity所支持的Occlusion Culling技术？ A：PVS only B：PVS and dynamic objects C：Automatic Portal Generation D：Dynamic Only 
7. 关于Vector3的API，以下说法正确的是？ A：Vector3.normalize可以获取一个三维向量的法线向量； B：Vector3.magnitude可以获取一个三维向量的长度； C：Vector3.forward与Vector3（0,0,1）是一样的意思； D：Vector3.Dot（向量A，向量B）是用来计算向量A与向量B的叉积 
8. 以下哪个函数在游戏进入新场景后会被马上调用？ A：MonoBehaviour.OnSceneWasLoaded B：MonoBehaviour.OnSceneEnter C：MonoBehaviour.OnLevelWasLoaded D：MonoBehaviour.OnLevelEnter 9. 什么是导航网格（ NavMesh）？ A：一种用于描述相机轨迹的网格 B：一种被优化过的物体网格 C：一种用于物理碰撞的网格 D：一种用于实现自动寻路的网格 
10. 下列那些选项不是网格层属性的固有选项? A：Default B：Walkable C：Not Walkable D：Jump




九：简述一下对象池，你觉得在FPS里哪些东西适合使用对象池

　　对象池就存放需要被反复调用资源的一个空间，当一个对象回大量生成的时候如果每次都销毁创建会很费时间，通过对象池把暂时不用的对象放到一个池中(也就是一个集合)。当下次要重新生成这个对象的时候先去池中查找一下是否有可用的对象，如果有的话就直接拿出来使用，不需要再创建。如果池中没有可用的对象，才需要重新创建，利用空间换时间来达到游戏的高速运行效果，在FPS游戏中要常被大量复制的对象包括子弹，敌人，粒子等



二十一：GPU的工作原理
简而言之，GPU的图形（处理）流水线完成如下的工作：（并不一定是按照如下顺序）
顶点处理：这阶段GPU读取描述3D图形外观的顶点数据并根据顶点数据确定3D图形的形状及位置关系，建立起3D图形的骨架。在支持DX8和DX9规格的GPU中，这些工作由硬件实现的Vertex Shader（定点着色器）完成。
光栅化计算：显示器实际显示的图像是由像素组成的，我们需要将上面生成的图形上的点和线通过一定的算法转换到相应的像素点。把一个矢量图形转换为一系列像素点的过程就称为光栅化。例如，一条数学表示的斜线段，最终被转化成阶梯状的连续像素点。
纹理帖图：顶点单元生成的多边形只构成了3D物体的轮廓，而纹理映射（texture mapping）工作完成对多变形表面的帖图，通俗的说，就是将多边形的表面贴上相应的图片，从而生成“真实”的图形。TMU（Texture mapping unit）即是用来完成此项工作。
像素处理：这阶段（在对每个像素进行光栅化处理期间）GPU完成对像素的计算和处理，从而确定每个像素的最终属性。在支持DX8和DX9规格的GPU中，这些工作由硬件实现的Pixel Shader（像素着色器）完成。
最终输出：由ROP（光栅化引擎）最终完成像素的输出，1帧渲染完毕后，被送到显存帧缓冲区。
总结：GPU的工作通俗的来说就是完成3D图形的生成，将图形映射到相应的像素点上，对每个像素进行计算确定最终颜色并完成输出。



十二：TCP/IP协议栈各个层次及分别的功能
答：网络接口层：这是协议栈的最低层，对应OSI的物理层和数据链路层，主要完成数据帧的实际发送和接收。
网络层：处理分组在网络中的活动，例如路由选择和转发等，这一层主要包括IP协议、ARP、ICMP协议等。
传输层：主要功能是提供应用程序之间的通信，这一层主要是TCP/UDP协议。
应用层：用来处理特定的应用，针对不同的应用提供了不同的协议，例如进行文件传输时用到的FTP协议，发送email用到的SMTP等。



四十九：Unity3D是否支持写成多线程程序？如果支持的话需要注意什么？
答：仅能从主线程中访问Unity3D的组件，对象和Unity3D系统调用
支持：如果同时你要处理很多事情或者与Unity的对象互动小可以用thread,否则使用coroutine。
注意：C#中有lock这个关键字,以确保只有一个线程可以在特定时间内访问特定的对象

五十：Unity3D的协程和C#线程之间的区别是什么？
答：多线程程序同时运行多个线程 ，而在任一指定时刻只有一个协程在运行，并且这个正在运行的协同程序只在必要时才被挂起。
除主线程之外的线程无法访问Unity3D的对象、组件、方法。
Unity3d没有多线程的概念，不过unity也给我们提供了StartCoroutine（协同程序）和LoadLevelAsync（异步加载关卡）后台加载场景的方法。 StartCoroutine为什么叫协同程序呢，所谓协同，就是当你在StartCoroutine的函数体里处理一段代码时，利用yield语句等待执行结果，这期间不影响主程序的继续执行，可以协同工作。


五十九：什么叫动态合批？跟静态合批有什么区别？
答：如果动态物体共用着相同的材质，那么Unity会自动对这些物体进行批处理。动态批处理操作是自动完成的，并不需要你进行额外的操作。
区别：动态批处理一切都是自动的，不需要做任何操作，而且物体是可以移动的，但是限制很多。静态批处理：自由度很高，限制很少，缺点可能会占用更多的内存，而且经过静态批处理后的所有物体都不可以再移动了。

六十：简述StringBuilder和String的区别？
答：
String是字符串常量。
StringBuffer是字符串变量 ，线程安全。
StringBuilder是字符串变量，线程不安全。
String类型是个不可变的对象，当每次对String进行改变时都需要生成一个新的String对象，然后将指针指向一个新的对象，如果在一个循环里面，不断的改变一个对象，就要不断的生成新的对象，所以效率很低，建议在不断更改String对象的地方不要使用String类型。
StringBuilder对象在做字符串连接操作时是在原来的字符串上进行修改，改善了性能。这一点我们平时使用中也许都知道，连接操作频繁的时候，使用StringBuilder对象。

六十一：什么是LightMap？
答：LightMap:就是指在三维软件里实现打好光，然后渲染把场景各表面的光照输出到贴图上，最后又通过引擎贴到场景上，这样就使物体有了光照的感觉。

六十二：Unity和cocos2d的区别
答：

Unity3D支持C#、javascript等，cocos2d-x 支持c++、Html5、Lua等。
cocos2d 开源 并且免费
Unity3D支持iOS、Android、Flash、Windows、Mac、Wii等平台的游戏开发，cocos2d-x支持iOS、Android、WP等。
六十三：C#和C++的区别？
答：
简单的说：C# 与C++ 比较的话，最重要的特性就是C# 是一种完全面向对象的语言，而C++ 不是，另外C# 是基于IL 中间语言和.NET Framework CLR 的，在可移植性，可维护性和强壮性都比C++ 有很大的改进。C# 的设计目标是用来开发快速稳定可扩展的应用程序，当然也可以通过Interop
和Pinvoke 完成一些底层操作

六十四:Unity3D Shader分哪几种，有什么区别？
答：表面着色器的抽象层次比较高，它可以轻松地以简洁方式实现复杂着色。表面着色器可同时在前向渲染及延迟渲染模式下正常工作。
顶点片段着色器可以非常灵活地实现需要的效果，但是需要编写更多的代码，并且很难与Unity的渲染管线完美集成。
固定功能管线着色器可以作为前两种着色器的备用选择，当硬件无法运行那些酷炫Shader的时，还可以通过固定功能管线着色器来绘制出一些基本的内容。

六十五：
已知strcpy函数的原型是：
char * strcpy(char * strDest,const char * strSrc);
1.不调用库函数，实现strcpy函数。
2.解释为什么要返回char *

    char * strcpy(char * strDest,const char * strSrc)
        {
                if ((strDest==NULL)||(strSrc==NULL)) //[1]
                        throw "Invalid argument(s)"; //[2]
                char * strDestCopy=strDest;  //[3]
                while ((*strDest++=*strSrc++)!='\0'); //[4]
                return strDestCopy;
        }
错误的做法：

    //不检查指针的有效性，说明答题者不注重代码的健壮性。
    //检查指针的有效性时使用((!strDest)||(!strSrc))或(!(strDest&&strSrc))，说明答题者对C语言中类型的隐式转换没有深刻认识。在本例中char *转换为bool即是类型隐式转换，这种功能虽然灵活，但更多的是导致出错概率增大和维护成本升高。所以C++专门增加了bool、true、false三个关键字以提供更安全的条件表达式。
    //检查指针的有效性时使用((strDest==0)||(strSrc==0))，说明答题者不知道使用常量的好处。直接使用字面常量（如本例中的0）会减少程序的可维护性。0虽然简单，但程序中可能出现很多处对指针的检查，万一出现笔误，编译器不能发现，生成的程序内含逻辑错误，很难排除。而使用NULL代替0，如果出现拼写错误，编译器就会检查出来。
//return new string("Invalid argument(s)");，说明答题者根本不知道返回值的用途，并且他对内存泄漏也没有警惕心。从函数中返回函数体内分配的内存是十分危险的做法，他把释放内存的义务抛给不知情的调用者，绝大多数情况下，调用者不会释放内存，这导致内存泄漏。
//return 0;，说明答题者没有掌握异常机制。调用者有可能忘记检查返回值，调用者还可能无法检查返回值（见后面的链式表达式）。妄想让返回值肩负返回正确值和异常值的双重功能，其结果往往是两种功能都失效。应该以抛出异常来代替返回值，这样可以减轻调用者的负担、使错误不会被忽略、增强程序的可维护性。
//忘记保存原始的strDest值，说明答题者逻辑思维不严密。
//循环写成while (*strDest++=*strSrc++);，同[1](B)。
//循环写成while (*strSrc!='\0') *strDest++=*strSrc++;，说明答题者对边界条件的检查不力。循环体结束后，strDest字符串的末尾没有正确地加上'\0'。
/**
 *返回strDest的原始值使函数能够支持链式表达式，增加了函数的“附加值”。同样功能的函数，如果能合理地提高的可用性，自然就更加理想。
    链式表达式的形式如：
        `nt iLength=strlen(strcpy(strA,strB));
    又如：
        char * strA=strcpy(new char[10],strB);
    返回strSrc的原始值是错误的。其一，源字符串肯定是已知的，返回它没有意义。其二，不能支持形如第二例的表达式。其三，为了保护源字符串，形参用const限定strSrc所指的内容，把const char *作为char *返回，类型不符，编译报错。
 */

六十六：C#中四种访问修饰符是哪些？各有什么区别？
答：1.属性修饰符 2.存取修饰符 3.类修饰符 4.成员修饰符。

属性修饰符：
* Serializable：按值将对象封送到远程服务器。
* STATread：是单线程套间的意思，是一种线程模型。
* MATAThread：是多线程套间的意思，也是一种线程模型。

存取修饰符：
* public：存取不受限制。
* private：只有包含该成员的类可以存取。
* internal：只有当前命名空间可以存取。
* protected：只有包含该成员的类以及派生类可以存取。

类修饰符：
* abstract：抽象类。指示一个类只能作为其它类的基类。
* sealed：密封类。指示一个类不能被继承。理所当然，密封类不能同时又是抽象类，因为抽象总是希望被继承的。

成员修饰符：
* abstract：指示该方法或属性没有实现。
* sealed：密封方法。可以防止在派生类中对该方法的override（重载）。不是类的每个成员方法都可以作为密封方法密封方法，必须对基类的虚方法进行重载，提供具体的实现方法。所以，在方法的声明中，sealed修饰符总是和override修饰符同时使用。
delegate：委托。用来定义一个函数指针。C#中的事件驱动是基于delegate + event的。
const：指定该成员的值只读不允许修改。
event：声明一个事件。
extern：指示方法在外部实现。
override：重写。对由基类继承成员的新实现。
readonly：指示一个域只能在声明时以及相同类的内部被赋值。
static：指示一个成员属于类型本身，而不是属于特定的对象。即在定义后可不经实例化，就可使用。
virtual：指示一个方法或存取器的实现可以在继承类中被覆盖。
new：在派生类中隐藏指定的基类成员，从而实现重写的功能。 若要隐藏继承类的成员，请使用相同名称在派生类中声明该成员，并用 new 修饰符修饰它。

六十七：Heap与Stack有何区别？
答：1.heap是堆，stack是栈。2.stack的空间由操作系统自动分配和释放，heap的空间是手动申请和释放的，heap常用new关键字来分配。3.stack空间有限，heap的空间是很大的自由区。
答：栈通常保存着我们代码执行的步骤，如在代码段1中 AddFive()方法，int pValue变量，int result变量等等。而堆上存放的则多是对象，数据等。（译者注:忽略编译器优化）我们可以把栈想象成一个接着一个叠放在一起的盒子。当我们使用的时候，每次从最顶部取走一个盒子。栈也是如此，当一个方法（或类型）被调用完成的时候，就从栈顶取走（called a Frame，译注：调用帧），接着下一个。堆则不然，像是一个仓库，储存着我们使用的各种对象等信息，跟栈不同的是他们被调用完毕不会立即被清理掉。


六十九：结构体和类有何区别？
答：结构体是一种值类型，而类是引用类型。（值类型、引用类型是根据数据存储的角度来分的）
就是值类型用于存储数据的值，引用类型用于存储对实际数据的引用。那么结构体就是当成值来使用的，类则通过引用来对实际数据操作。

七十：请写出求斐波那契数列任意一位的值得算法

```cs
static int Fn(int n)
{
    if (n <= 0)
    {
        throw new ArgumentOutOfRangeException();
    }
    if (n == 1||n==2)
    {
        return 1;
    }
    return checked(Fn(n - 1) + Fn(n - 2)); // when n>46 memory will  overflow
}
```

七十一：ref参数和out参数是什么？有什么区别？
答：ref和out参数的效果一样，都是通过关键字找到定义在主函数里面的变量的内存地址，并通过方法体内的语法改变它的大小。
不同点就是输出参数必须对参数进行初始化。
ref参数是引用，out参数为输出参数。

七十二：C#的委托是什么？有何用处？
答：委托类似于一种安全的指针引用，在使用它时是当做类来看待而不是一个方法，相当于对一组方法的列表的引用。
用处：使用委托使程序员可以将方法引用封装在委托对象内。然后可以将该委托对象传递给可调用所引用方法的代码，而不必在编译时知道将调用哪个方法。与C或C++中的函数指针不同，委托是面向对象，而且是类型安全的。

七十三：协同程序的执行代码是什么？有何用处，有何缺点？

function Start() { 
    // - After 0 seconds, prints "Starting 0.0"
    // - After 0 seconds, prints "Before WaitAndPrint Finishes 0.0"
    // - After 2 seconds, prints "WaitAndPrint 2.0"
    // 先打印"Starting 0.0"和"Before WaitAndPrint Finishes 0.0"两句,2秒后打印"WaitAndPrint 2.0"
    print ("Starting " + Time.time );
    // Start function WaitAndPrint as a coroutine. And continue execution while it is running
    // this is the same as WaintAndPrint(2.0) as the compiler does it for you automatically
    // 协同程序WaitAndPrint在Start函数内执行,可以视同于它与Start函数同步执行.
    StartCoroutine(WaitAndPrint(2.0)); 
    print ("Before WaitAndPrint Finishes " + Time.time );
}

function WaitAndPrint (waitTime : float) {
    // suspend execution for waitTime seconds
    // 暂停执行waitTime秒
    yield WaitForSeconds (waitTime);
    print ("WaitAndPrint "+ Time.time );
}
作用：一个协同程序在执行过程中,可以在任意位置使用yield语句。yield的返回值控制何时恢复协同程序向下执行。协同程序在对象自有帧执行过程中堪称优秀。协同程序在性能上没有更多的开销。
缺点：协同程序并非真线程，可能会发生堵塞。

七十四：什么是里氏代换元则？
答：里氏替换原则(Liskov Substitution Principle LSP)面向对象设计的基本原则之一。 里氏替换原则中说，任何基类可以出现的地方，子类一定可以出现，作用方便扩展功能能

七十五：Mock和Stub有何区别？
Mock与Stub的区别：Mock:关注行为验证。细粒度的测试，即代码的逻辑，多数情况下用于单元测试。Stub：关注状态验证。粗粒度的测试，在某个依赖系统不存在或者还没实现或者难以测试的情况下使用，例如访问文件系统，数据库连接，远程协议等。

七十六：概述序列化：
答：序列化简单理解成把对象转换为容易传输的格式的过程。比如，可以序列化一个对象，然后使用HTTP通过Internet在客户端和服务器端之间传输该对象

七十八：概述c#中代理和事件？
答：代理就是用来定义指向方法的引用。
C＃事件本质就是对消息的封装，用作对象之间的通信；发送方叫事件发送器，接收方叫事件接收器；

七十九：C#中的排序方式有哪些？
答：选择排序，冒泡排序，快速排序，插入排序，希尔排序，归并排序

八十：射线检测碰撞物的原理是？
答：射线是3D世界中一个点向一个方向发射的一条无终点的线，在发射轨迹中与其他物体发生碰撞时，它将停止发射 。

八十一：客户端与服务器交互方式有几种？
答： socket通常也称作"套接字",实现服务器和客户端之间的物理连接，并进行数据传输，主要有UDP和TCP两个协议。Socket处于网络协议的传输层。
http协议传输的主要有http协议 和基于http协议的Soap协议（web service）,常见的方式是 http 的post 和get 请求，web 服务。

八十二：Unity和Android与iOS如何交互？

八十三：Unity中，照相机的Clipping Planes的作用是什么？调整Near、Fare两个值时，应该注意什么？
答：剪裁平面 。从相机到开始渲染和停止渲染之间的距离。

八十四：如何在Unity3D中查看场景的面试，顶点数和Draw Call数？如何降低Draw Call数？
答：在Game视图右上角点击Stats。降低Draw Call 的技术是Draw Call Batching

八十五：请问alpha test在何时使用？能达到什么效果？
Alpha Test,中文就是透明度测试。简而言之就是V&F shader中最后fragment函数输出的该点颜色值（即上一讲frag的输出half4）的alpha值与固定值进行比较。Alpha Test语句通常于Pass{}中的起始位置。Alpha Test产生的效果也很极端，要么完全透明，即看不到，要么完全不透明。

八十六：UNITY3d在移动设备上的一些优化资源的方法
答：
1.使用assetbundle，实现资源分离和共享，将内存控制到200m之内，同时也可以实现资源的在线更新
2.顶点数对渲染无论是cpu还是gpu都是压力最大的贡献者，降低顶点数到8万以下，fps稳定到了30帧左右
3.只使用一盏动态光，不是用阴影，不使用光照探头
粒子系统是cpu上的大头
4.剪裁粒子系统
5.合并同时出现的粒子系统
6.自己实现轻量级的粒子系统
animator也是一个效率奇差的地方
7.把不需要跟骨骼动画和动作过渡的地方全部使用animation，控制骨骼数量在30根以下
8.animator出视野不更新
9.删除无意义的animator
10.animator的初始化很耗时（粒子上能不能尽量不用animator）
11.除主角外都不要跟骨骼运动apply root motion
12.绝对禁止掉那些不带刚体带包围盒的物体（static collider ）运动
NUGI的代码效率很差，基本上runtime的时候对cpu的贡献和render不相上下
13每帧递归的计算finalalpha改为只有初始化和变动时计算
14去掉法线计算
15不要每帧计算viewsize 和windowsize
16filldrawcall时构建顶点缓存使用array.copy
17.代码剪裁：使用strip level ，使用.net2.0 subset
18.尽量减少smooth group
19.给美术定一个严格的经过科学验证的美术标准，并在U3D里面配以相应的检查工具

八十七：四元数有什么作用？
答：对旋转角度进行计算时用到四元数

八十八：将Camera组件的ClearFlags选项选成Depth only是什么意思？有何用处？
答：仅深度，该模式用于对象不被裁剪。

八十九：如何让已经存在的GameObject在LoadLevel后不被卸载掉？

void Awake()
{
    DontDestroyOnLoad(transform.gameObject);
}

九十：在编辑场景时将GameObject设置为Static有何作用？
答：设置游戏对象为Static将会剔除（或禁用）网格对象当这些部分被静态物体挡住而不可见时。因此，在你的场景中的所有不会动的物体都应该标记为Static。

九十一：有A和B两组物体，有什么办法能够保证A组物体永远比B组物体先渲染？
答：把A组物体的渲染对列大于B物体的渲染队列

九十二：将图片的TextureType选项分别选为Texture和Sprite有什么区别
答：Sprite作为UI精灵使用，Texture作用模型贴图使用。

九十三：问一个Terrain，分别贴3张，4张，5张地表贴图，渲染速度有什么区别？为什么？
答：没有区别，因为不管几张贴图只渲染一次。

九十四：什么是DrawCall？DrawCall高了又什么影响？如何降低DrawCall？
答：Unity中，每次引擎准备数据并通知GPU的过程称为一次Draw Call。DrawCall越高对显卡的消耗就越大。降低DrawCall的方法：
Dynamic Batching
Static Batching
高级特性Shader降级为统一的低级特性的Shader。

九十五：实时点光源的优缺点是什么？
答：可以有cookies – 带有 alpha通道的立方图(Cubemap )纹理。点光源是最耗费资源的。

九十六：Unity的Shader中，Blend SrcAlpha OneMinusSrcAlpha这句话是什么意思？
答：作用就是Alpha混合。公式：最终颜色 = 源颜色 源透明值 + 目标颜色（1 - 源透明值）

九十七：简述水面倒影的渲染原理
答: 原理就是对水面的贴图纹理进行扰动，以产生波光玲玲的效果。用shader可以通过GPU在像素级别作扰动，效果细腻，需要的顶点少，速度快

九十八：简述NGUI中Grid和Table的作用？
答：对Grid和Table下的子物体进行排序和定位

九十九：请简述NGUI中Panel和Anchor的作用
答：只要提供一个half-pixel偏移量，它可以让一个控件的位置在Windows系统上精确的显示出来（只有这个Anchor的子控件会受到影响）
如果挂载到一个对象上，那么他可以将这个对象依附到屏幕的角落或者边缘
3.UIPanel用来收集和管理它下面所有widget的组件。通过widget的geometry创建实际的draw call。没有panel所有东西都不能够被渲染出来,你可以把UIPanel当做Renderer
一百：能用foreach遍历访问的对象需要实现__接口或声明_方法的类型
答：IEnumerable；GetEnumerator


Unity Engine Specific Question :
Difference between Update,Fixed Update and Late Update.
What is Prefabs in Unity 3D?
What is the use of AssetBundle in Unity?
What is difference between Resources and StreamingAssets Folder.
What is Batching and what is the use of Batching?
Difference between Destroy and DestroyImmediate unity function
Difference between Start and Awake Unity Events
What is the use of deltatime?
Is this possible to collide two mesh collider,if yes then How?
Difference between Static and Dynamic Batching.
What is the use of Occlusion Culling?
How can you call C# from Javascript, and vice versa?
Arrange the event functions listed below in the order in which they will be invoked when an application is closed:
Update()
OnGUI()
Awake()
OnDisable()
Start()
LateUpdate()
OnEnable()
OnApplicationQuit()
OnDestroy()
C# related Questions :
 Difference between Class and Structure.
What is Coroutine,is it running on new thread?
Difference between Stack and Heap.
What do you mean by Inheritance ? Explain with example.
What do you mean by Polymorohism? Explain with example.
What is overriding ?
What is overloading ?
Difference between overriding and overloading.
What is the use of Virtual keyword ?
Difference between Static Class and Singleton.
What is Abstract Class ?
Difference between Abstract Class and interface.
What is Serialization and De-Serialization ?
Does C# support multiple inheritance ?
What do you mean by Generic Function or Generic Class ?