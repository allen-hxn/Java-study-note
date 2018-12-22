

## 目录

[TOC]



## 基础篇

### 3*0.1==0.3返回值是什么

false，因为有些浮点数不能完全精确的表示出来。

### a=a+b与a+=b有什么区别吗?

+=操作符会进行隐式自动类型转换,此处a+=b隐式的将加操作的结果类型强制转换为持有结果的类型,而a=a+b则不会自动进行类型转换.如： byte a = 127; byte b = 127; b = a + b; // error : cannot convert from int to byte b += a; // ok （译者注：这个地方应该表述的有误，其实无论 a+b 的值为多少，编译器都会报错，因为 a+b 操作会将 a、b 提升为 int 类型，所以将 int 类型赋值给 byte 就会编译出错）

### short s1=1; s1 = s1 + 1;该段代码是否有错,有的话怎么改？

有错误,short类型在进行运算时会自动提升为int类型,也就是说s1+1的运算结果是int类型.

### short s1=1; s1 += 1; 该段代码是否有错,有的话怎么改？

+=操作符会自动对右边的表达式结果强转匹配左边的数据类型,所以没错.

### int和Integer的区别

- int 是JAVA八个基本类型中的一个,它的值范围为 `2147483648～2147483647`,占用4个字节,32位
- Integer 是引用类型,包装了基本类型中的`int`

int的包装类就是Integer，从Java 5开始引入了自动装箱/拆箱机制，使得二者可以相互转换。

```java
int i = 128;
Integer i2 = 128;
Integer i3 = new Integer(128);
System.out.println(i == i2); //Integer会自动拆箱为int，所以为true
System.out.println(i == i3); //true，理由同上
Integer i4 = 127;//编译时被翻译成：Integer i4 = Integer.valueOf(127);
Integer i5 = 127;
System.out.println(i4 == i5);//true
Integer i6 = 128;
Integer i7 = 128;
System.out.println(i6 == i7);//false
Integer i8 = new Integer(127);
System.out.println(i5 == i8); //false
Integer i9 = new Integer(128);
Integer i10 = new Integer(123);
System.out.println(i9 == i10);  //false
```

**以上的情况总结如下：**

1，无论如何，Integer与new Integer不会相等。不会经历拆箱过程，new出来的对象存放在堆，而非new的Integer常量则在常量池（在方法区），他们的内存地址不一样，所以为false。

2，两个都是非new出来的Integer，如果数在-128到127之间，则是true,否则为false。因为java在编译Integer i2 = 128的时候,被翻译成：Integer i2 = Integer.valueOf(128);而valueOf()函数会对-128到127之间的数进行缓存。

3，两个都是new出来的,都为false。还是内存地址不一样。

4，int和Integer(无论new否)比，都为true，因为会把Integer自动拆箱为int再去比。



### 字符串比较 == 和equals的区别

1.基本数据类型，也称原始数据类型。byte,short,char,int,long,float,double,boolean 
他们之间的比较，应用双等号（==）,比较的是他们的值。 
2.复合数据类型(类) 
当他们用（==）进行比较的时候，比较的是他们在内存中的存放地址，所以，除非是同一个new出来的对象，他们的比较后的结果为true，否则比较后结果为false。 JAVA当中所有的类都是继承于Object这个基类的，在Object中的基类中定义了一个equals的方法，这个方法的初始行为是比较对象的内存地 址，但在一些类库当中这个方法被覆盖掉了，如String,Integer,Date在这些类当中equals有其自身的实现，而不再是比较类在堆内存中的存放地址了。

```java
String str2 = "hello";
String str3 = "hello";
System.out.println("str3==str2： " + (str3==str2));  \\3
System.out.println("str3.equals(str2)： " + str3.equals(str2));  \\4
```

输出结果：

```
str3==str2： true
str3.equals(str2)： true
```

这里的又为什么都是输出true呢，因为它们都是从缓冲池取出来的，由于string类比较特殊，jdk专门做了缓存优化。

原来Java运行时会维护一个String Pool（String池）。String池用来存放运行时中产生的各种字符串，并且池中的字符串的内容不重复。而一般对象不存在这个缓冲池，并且创建的对象仅仅存在于方法的堆栈区。

## 面向对象

### 什么是类？

类是具有相同属性和方法的一组对象的集合，它为属于该类的所有对象提供了统一的抽象描述，其内部包括属性和方法两个主要部分。在面向对象的编程语言中，类是一个独立的程序单位，它应该有一个类名并包括属性和方法两个主要部分。

Java中的类实现包括两个部分：类声明和类体

### final有哪些用法

final也是很多面试喜欢问的地方,能回答下以下三点就不错了:

1. 被final修饰的类不可以被继承
2. 被final修饰的方法不可以被重写
3. 被final修饰的变量不可以被改变.如果修饰引用,那么表示引用不可变,引用指向的内容可变.
4. 被final修饰的方法,JVM会尝试将其内联,以提高运行效率
5. 被final修饰的常量,在编译阶段会存入常量池中.



### final,finalize和finally的不同之处

final 是一个修饰符，可以修饰变量、方法和类。如果 final 修饰变量，意味着该变量的值在初始化后不能被改变。

finalize 方法是在对象被回收之前调用的方法，给对象自己最后一个复活的机会，但是什么时候调用 finalize 没有保证。

finally 是一个关键字，与 try 和 catch 一起用于异常的处理。finally 块一定会被执行，无论在 try 块中是否有发生异常。

### Object中有哪些公共方法?

equals() clone() getClass() notify(),notifyAll(),wait() toString

### 多态

多态的定义：指允许不同类的对象对同一消息做出响应。即同一消息可以根据发送对象的不同而采用多种不同的行为方式。

主要有以下优点:

- 可替换性:多态对已存在代码具有可替换性.
- 可扩充性:增加新的子类不影响已经存在的类结构.
- 接口性:多态是超类通过方法签名,向子类提供一个公共接口,由子类来完善或者重写它来实现的.
- 灵活性:它在应用中体现了灵活多样的操作，提高了使用效率
- 简化性:多态简化对应用软件的代码编写和修改过程，尤其在处理大量对象的运算和操作时，这个特点尤为突出和重要

### 代码中如何实现多态

实现多态主要有以下三种方式：

1. 接口实现 
2. 继承父类重写方法 
3. 同一类中进行方法重载

### 虚拟机是如何实现多态的

动态绑定技术(dynamic binding)，执行期间判断所引用对象的实际类型，根据实际类型调用对应的方法。

### 接口和抽象类的区别

从设计层面来说，抽象是对类的抽象，是一种模板设计，接口是行为的抽象，是一种行为的规范。


Java提供和支持创建抽象类和接口。它们的实现有共同点，不同点在于： 

- 接口中所有的方法隐含的都是抽象的。而抽象类则可以同时包含抽象和非抽象的方法。 
- 类可以实现很多个接口，但是只能继承一个抽象类 
- 类可以不实现抽象类和接口声明的所有方法，当然，在这种情况下，类也必须得声明成是抽象的。 
- 抽象类可以在不提供接口方法实现的情况下实现接口。 
- Java接口中声明的变量默认都是final的。抽象类可以包含非final的变量。 
- Java接口中的成员函数默认是public的。抽象类的成员函数可以是private，protected或者是public。 
- 接口是绝对抽象的，不可以被实例化。抽象类也不可以被实例化，但是，如果它包含main方法的话是可以被调用的。 