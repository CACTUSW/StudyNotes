# 基础

### 注释

> ##### 单行注释

```java
//单行注释以双斜杠开头
```

> ##### 多行注释

多行注释

以/*开头，

以*/结尾

```Java
/*  

*/
```

> ##### JavaDoc注释

文档注释

javadoc命令是用来生成自己API文档的

以/**开头

以*/结尾

中间每行加*

**参数信息**

+ @author 作者名
+ @version 版本号
+ @since 指名需要最早使用的jdk版本
+ @param 参数名
+ @return 返回异常值
+ @throws 异常抛出情况

```java
/**
*@Description HelloWorld
*@Author XXX
*/
```

### 标识符和关键字

> ##### 关键字

| abstract   | assert       | boolean   | break      | byte   |
|:----------:| ------------ | --------- | ---------- | ------ |
| case       | catch        | char      | class      | const  |
| continue   | default      | do        | double     | else   |
| enum       | extends      | final     | finally    | float  |
| for        | goto         | if        | implements | import |
| instanceof | int          | interface | long       | native |
| new        | package      | private   | protected  | public |
| return     | strictfp     | short     | static     | super  |
| switch     | synchronized | this      | throw      | throws |
| transient  | try          | void      | volatile   | while  |

> ##### 标识符

**注意：**

- 所有标识符都应以字母(A-Z或a-z)、美元符($)、或者下划线(_)开始
- 首字母之后可以是字母(A-Z或a-z)、美元符($)、或者下划线(_)或数字的任何字符组合
- **不能使用关键字作为变量名或方法名**
- 标识符是大小写敏感的
- 可以使用中文命名，但一般不建议用，也不建议用拼音

### 数据类型

> ##### 基本数据类型

+ 数值类型
  + 整数类型
    + byte占1个字节
    + short占2个字节
    + int占4个字节
    + long占8个字节
  + 浮点类型
    + float占4个字节
    + double占8个字节
  + 字符类型
    + char占2个字节
+ boolean类型
  + 占1位，其值只有true和false

> ##### 引用数据类型

+ 类
+ 接口
+ 数组

### 类型转换

![image-20210319201459877.png](http://lsky.volerde.space/i/2022/05/16/62823afa0515d.png)

+ 强制类型转换        **(类型)变量名**        **高—–低**
+ 自动类型转换       **低—–高**    

### 变量、常量、作用域

> ##### 变量

Java变量是程序中最基本的存储单元，其要素包括变量名，变量类型和**作用域**

```java
type vrName [=value] [{,varName[=value]}];
//数据类型    变量名 = 值;
```

> ##### 变量作用域

+ 类变量
+ 实例变量
+ 局部变量

```java
public class Variable{//    类
    static int allclicks = 0;    //    类变量，前面加了关键词static
    String str = "Hello,World!";    //    实例变量，前面无关键词，如果不初始化，则会变成该类型的默认值 0 0.0
                                    //    布尔值默认为:false    除了基本类型，其余默认值都是null

    public void method{//    method方法
        int i = 1;    //    局部变量，定义在方法里面
    }
}
```

**注意事项:**

+ **每个变量都有类型，类型可以是基本类型，也可以是引用类型**
+ **变量名必须是合法的标识符**
+ **变量声明是一条完整的语句，因此每一个声明必须以分号结束**

**变量命名规范:**

+ 所有变量，方法，类名:    **见名知意**
+ 类成员变量:    首字母小写和驼峰原则:    monthSalary
+ 局部变量:    首字母小写和驼峰原则
+ 常量:    大写字母和下划线:    MAX_VALUE
+ 类名:    首字母大写和驼峰原则:    Man,GoodMan
+ 方法名:    首字母小写和驼峰原则:    run(),runRun()

> ##### 常量

+ 初始化之后不能再改变值！不会变动的值！

+ 常量名一般用大写字符

```java
//final 常量名 = 值;
final double PI = 3.14;
public static final double PI = 3.14;    //    public,static,final为修饰符，可调换顺序，不区分前后
```

### 基本运算符

+ 算术运算符:    +，-，*，/，%，++，--
+ 赋值运算符    =
+ 关系运算符:    >，<，>=，<=，==，!=
+ 逻辑运算符:    &&，||，!
+ 位运算符:    &，|，\^，~，<<，>>，<<<
+ 条件运算符    ?  :
+ 扩展赋值运算符:    +=，-=，*=，/=

```java
//++与--    自增与自减运算符
int a = 3;
int b = a++;//a++,先给b赋值，再进行自增
int c = ++a;//++a，先自增，再给c赋值
```

```java
&&    //逻辑与运算：两个结果都为真，结果才为true
||    //逻辑或运算：两个结果有一个为真，结果就为true
!    //逻辑非运算：如果是真，则变为假，如果是假，则变为真、
    //逻辑运算中存在短路运算
```

```java
/*
A = 0011 1100
B = 0000 1101
--------------------
A&B = 0000 1100
A|B = 0011 1101
A^B = 0011 0001
~B  = 1111 0010
--------------------
<<    *2
>>    /2
*/
```

```java
//字符串连接符    +
int a = 10;
int b = 20;
System.out.println(""+a+b);//结果为1020，a,b被转换为了string类型
System.out.println(a+b+"");//结果为30
```

```java
?  :    //条件运算符
x?y:z    //如果x为true，结果为y，否则为z
```

注：

1. 很多运算，会使用一些工具类来进行操作

### 包机制

+ 语法格式：

```java
package    pkg1[. pkg2[. pkg3...]]
```

+ **一般利用公司域名倒置作为包名**

+ 为了能够使用某一个包的成员，我们需要在java程序中明确导入该包。使用“import”导入

```java
import package1[.package2...].(classname|*);
```

### 流程控制

+ **凡是属于IO流（输入输出流）的类如果不关闭会一直占用资源，要用后就关。**

> ##### 用户交互scanner

+ next():
  + 一定要读取到有效字符后才可以停止输入
  + 对输入有效字符前遇到的空白，next()方法会自动将其去掉
  + 只有输入有效字符后才将后面输入的空白作为分隔服或者结束符
  + next()不能得到带有空格的字符串

+ nextline():
  + 以Enter为结束符，也就是说 nextline()方法返回的是输入回车之前的所有字符
  + 可以获得空白

> ##### 顺序结构

+ JAVA的基本结构就是顺序结构，除非特别指明否则就按照顺序一句一句执行。
+ 顺序结构是最简单的算法结构。
+ 语句与语句之间，框与框之间都是按照从上打下的顺序进行的，它是由若干个依次执行的处理步骤组成的，==它是任何一个算法都离不开的一种基本算法结构。==

> ##### 选择结构

+ if单选泽结构
  
  ```java
  if(布尔表达式){
      //如果布尔表达式的值为true
  }else{
      //如果布尔表达式的值为false
  }
  ```

+ if双选择结构

+ if多选择结构

+ 嵌套的if结构

+ switch多选择结构
  
  + switch语句中的变量类型可以是：
    + byte、short、int或者char
    + ==从Java SE 7开始，switch支持字符串String类型了==
    + case标签必须为字符串常量或字面量
  
  ```java
  switch(expression){
      case value :
          //语句
          break;//可选
      case value :
          //语法
          break;//可选
      default :
          //语句
  }
  ```

> ##### 循环结构

+ while循环
  
  ```java
  while(布尔表达式){
      //循环内容
  }
  ```

+ do...while循环
  
  ```java
  do{
      //代码语句
  }while(布尔表达式);
  ```
  
  + while和do-while区别：
    + while先判断后执行。du-while是后判断先执行
    + do-while总能保证循环体会被至少执行一次！这是他们的主要差别。

+ for循环
  
  ```java
  for(初始化;布尔表达式;更新){
      //代码语句
  }
  ```

+ 在Java5中引入了一种主要用于数组的增强型for循环。
  
  ```java
  for(声明语句 : 表达式){
      //代码语句
  }
  ```
  
  + 声明语句：声明新的局部变量，该变量的类型必须和数组元素的类型匹配。其作用域限定在循环语句块，其值与此时数组元素的值相等。
  + 表达式：表达式是要访问的数组名，或者是返回值为数组的方法。 

> ##### print&println的区别

```java
print    //输出完不会换行
println    //输出完会换行    
    \t    //输出tab键
    \n    //输出换行符
```

> ##### break&continue

+ 在任何循环语句的主体部分，均可用break控制循环的流程。break用于强行退出循环，不执行循环中剩余的语句。（break语句也在switch语句中使用）

+ continue语句用在~~徐怒汉语句体~~中，用于终止某次循环过程，即跳过循环体中尚未执行的语句，接着进行下一次是否执行循环的判定。

+ 关于goto关键字 
  
  + goto关键字很早就在程序设计语言中出现。尽管goto仍是Java的一个保留字，但并未在语言中正式使用；==Java没有goto==。然而，在break和continue这两个关键字上，我们仍能看出一些goto的影子——带标签的break和continue。
  
  + “标签”是指后面跟一个冒号的标识符，例如：label:
  
  + 对Java来说唯一用到标签的地方是在循环语句之前。而在循环之前设置标签的唯一理由是：我们希望在其中嵌套另一个循环，由于break和continue关键字通常只中断当前循环，但若随同标签
    
    使用，他们就会中断到存在标签的地方。

# 方法

### 何谓方法

+ Java方法是语句的集合，他们在一起执行一个功能。
  + 方法是解决一类问题的步骤的有序组合
  + 方法包含于类或对象中
  + 方法在程序中被创建，在其他地方被引用
+ 设计方法的原则：方法的本意是功能块，就是实现某个功能的语句块的集合。我们设计方法的时候，最好保持方法的原子性，==就是一个方法只完成一个功能，这样利于我们后期的拓展==
+ 方法的命名规则：首字母小写和驼峰原则:    run(),runRun()

### 方法的定义及调用

**方法定义：**

+ Java的方法类似于其他语言的函数，是一段==用来完成特定功能的代码片段==，一般情况下，定义一个方法包括一下语法：

+ ==方法包含一个方法头和一个方法体。==下面是一个方法的所有部分：
  
  + ==修饰符==：修饰符，这是可选的，告诉编译器如何调用该方法。定义了该方法的访问类型。
  + ==返回值类型==：方法可能会返回值。returnValueType是方法返回值得数据类型。有些方法执行所需的操作，没有返回值。在这种情况下，returnValueType是关键字void
  + ==方法名==:是方法的实际名称。方法名和参数表共同构成方法签名。
  + ==参数类型==：参数像是一个占位符。当方法被调用时，传递值给参数。这个字被称为实参或变量。参数列表是指方法的参数类型、顺序和参数的个数。参数是可选的，方法可以不包含任何参数。
    + 形式参数：在方法被调用是用于接收外界输入的参数。
    + 实参：调用方法时实际传给方法的数据。
  + ==方法体==：方法体包括具体的语句，定义该方法的功能。
  
  ```java
  修饰符 返回值类型 方法名（参数类型 参数名）}
      ···
      方法体
      ···
      return 返回值；
  }
  ```

**方法调用：**

+ **调用方法：对象名.方法名（实参列表）**

+ **Java支持两种调用方法的方式，根据方法是否返回值来选择。**

+ **当方法返回一个值的时候，方法调用通常被当作一个值。例如：**
  
  ```java
  int larger = max(30,40);
  ```

+ **如果方法返回值是void，方法调用一定是一条语句。**
  
  ```java
  System.out.println("Hello");
  ```

+ **值传递 和 引用传递**

### 方法重载

+ 重载就是在一个类中，有相同的函数名称，但形参不同的函数
+ 方法的重载的规则：
  + 方法名称必须相同
  + 参数列表必须不同（个数不同、类型不同或参数排列顺序不同）
  + 方法的返回类型可以相同也可以不相同
  + 仅仅返回类型不同不足以成为方法的重构
+ 实现理论：
  + 方法名称相同时，编译器会根据调用方法的参数个数、参数类型等去逐个匹配，以选择对应的方法，如果匹配失败，则编译器报错

### 命令行传参

+ 有时候你希望运行一个程序的时候再传递给它消息。只要靠命令行传参数给main()函数实现。
  
  ```java
  public class CommandLine{
      public static void main(string args[]){
          for(int i =0;i<args.length;i++){
              System.out.println("args["+i+"]:"+args[i]);
          }
      }
  }
  ```

### 可变参数

+ JDK1.5开始，Java支持传递同类型的可变参数的一个方法

+ 在方发声明中，在指定参数类型后加一个省略号（...）

+ 一个方法中只能指定一个可变参数，它必须是方法的最后一个参数。任何普通的参数必须在它之前声明
  
  ```java
  public static void printMax(double... numbers){
      if (numbers.length==0){
          System.out.println("No argument passed");
          return;
      }
  
      double result = numbers[0];
  
      //排序
      for (int i =1;i<numbers.length;i++){
          if (numbers[i]>result){
              result = numbers[i];
          }
      }
      System.out.println("The max value is "+result);
  }
  ```

### 递归

+ 递归就是自己调用自己
+ 利用递归可以用简单的程序来解决一些复杂的问题。它通常吧一个大型复杂的问题层层转化为一个与原问题相似的规模较小的问题来求解，递归策略只需要少量的程序就可描述出解题过程所需要的多次重复计算，大大的减少了程序的代码量。递归的能力在于用有限的语句来定义对象的无限集合。
+ 递归结构包括两个部分：
  + 递归头：什么时候不调用自身方法。如果没有头，将陷入死循环。
  + 递归体： 什么时候需要调用自身方法。

# 数组

### 数组概述

+ 数组是相同类型数据的有序集合
+ 数组描述的是相同类型的若干个数据，按照一定的先后次序排列组合而成
+ 其中，每一个数据称为一个数组元素，每个数组元素可以通过一个数组下标来访问他们。

### 数组声明创建

+ 首先必须声明数组变量，才能在程序中使用数组。
  
  ```java
  dataType[] arrayRefVar;    //首选方法
  dataType arrayRefVar[];    //效果相同，但不是首选方法
  ```

+ Java语言使用new操作符来创建数组
  
  ```java
  dataType[] arrayRefVar = new dataType[arraySize];
  ```

+ 数组的元素是通过索引访问的，数组索引从0开始

+ 获取数组长度
  
  ```java
  arrays.length
  ```

> ##### 内存分析

**Java内存分析：**

+ Java内存
  + 堆
    + 存放new的对象和数组
    + 可以被所有的线程共享，不会存放别的对象引用
  + 栈
    + 存放基本变量类型（会包含这个基本类型的具体数值）
    + 引用对象的变量（会存放这个引用在堆里面的具体地址）
  + 方法区
    + 可以被所有的线程共享
    + 包含了所有的class和static变量

> ##### 三种初始化

+ 静态初始化
  
  ```java
  int[] a ={1,2,3};
  Man[] men ={new Man(1,1),new Man(2,2)};
  ```

+ 动态初始化
  
  ```java
  int[] a =new int[2];
  a[0] = 1;
  a[1] = 2;
  ```

+ 数组的默认初始化
  
  + 数组是引用类型，它的元素相当于类的实例变量，因此数组一经分配空间，其中的每个元素也被按照实例变量同样的方式被隐式初始化。

> ##### 数组的四个基本特点

+ 其长度是确定的。数组一旦被创建，它的大小是不可以改变的。
+ 其元素必须是相同类型，不允许出现混合类型。
+ 数组中的元素可以是任意数据类型，包括基本类型和引用类型。
+ 数组变量属引用类型，数组也可以看成是对象，数组中的每个元素相当于该对象的成员变量，数组本身就是对象，Java中对象是在堆中的，因此数组无论保存原始类型还是其他对象类型，==数组本身是在堆中的。==

> ##### 数组边界

+ 下标的合法区间：[0，length-1]，如果越界就会报错
  
  ```java
  public static void main(String[] args) {
      int[] a=new int[2];
      System.out.println(a[2]);
  }
  ```

+ ==ArrayIndexOutOfBoundsException:数组下标越界异常！==

+ 小结：
  
  + 数组是相同数据类型（数据类型可以为任意类型）的有序集合
  + 数组也是对象。数组元素相当于对象的成员变量
  + 数组长度是确定的，不可变的。如果越界，就会报错。

### 数组使用

+ 普通For循环
+ For-Each循环
+ 数组作方法入参
+ 数组作返回值

### 多维数组

+ 多维数组可以看成是数组的数组，比如二维数组就是一个特殊的一维数组，其每一个元素都是一个一维数组

+ 二维数组
  
  ```java
  int a[][] = new int[2][5];//一个2行5列的数组
  ```

### arrays类

+ 数组的工具类java.util.Arrays
+ 由于数组对象本身并没有什么方法可以供我们调用，但API中提供了一个工具类Arrays供我们使用，从而可以对数据对象进行一些基本操作。
+ ==查看JDK帮助文档==
+ Arrays类中的方法都是static修饰的静态方法，在使用的时候可以直接使用类名进行调用，而“不用”使用对象来调用（注意：是“不用”而不是“不能”）
+ 具有以下常用功能：
  + 给数组赋值：通过fill方法
  + 对数组排序：通过sort方法，按升序
  + 比较数组：通过equals方法比较数组中元素值是否相等
  + 查找数组元素：通过binarySearch方法能对排序好的数组进行二分查找法操作

> ##### 冒泡排序

+ 两层循环，外层冒泡轮数，内层依次比较

+ 算法的时间复杂度为O(n2)
  
  ```java
  public static int[] sort(int[] array){//冒泡排序
      //临时变量
      int temp;
      //外层循环，判断要进行多少次
      for (int i = 0; i < array.length-1; i++) {
          //内层循环，比较两个数大小，如果第一个数大于第二个数，则交换两数位置
          for (int j = 0; j < array.length-1-i; j++) {
              if (array[j+1]<array[j]){
                  temp = array[j];
                  array[j]=array[j+1];
                  array[j+1]=temp;
              }
          }
      }
      return array;
  }
  ```

### 稀疏数组

> ##### 稀疏数组介绍

+ 当一个数组中大部分元素为0，或者为同一值得数组时，可以用稀疏数组来保存该数组
+ 稀疏数组的处理方式是：
  + 记录数组一共有几行几列，有多少个不同值
  + 把具有不同值的元素和行列及值记录在一个小规模的数组中，从而缩小程序的规模
+ 如图：左边为原始数据，右边是稀疏数组

![image-20210329140656818.png](http://lsky.volerde.space/i/2022/05/16/62823afc7e2ac.png)      ![image-20210329140710666.png](http://lsky.volerde.space/i/2022/05/16/62823afcc62d8.png)

# 面向对象

### 初识面向对象

> ##### 面向过程&面向对象

+ 面向过程思想
  + 步骤清晰简单，第一步做什么，第二步做什么......
  + 面对过程适合处理一些较为简单的问题
+ 面对对象思想
  + 物以类聚，分类的思维模式，思考问题首先会解决问题需要哪些分类，然后对这些分类进行单独思考。最后，才对某个分类下的细节进行面向过程的思索
  + 面向对象适合处理复杂的问题，适合处理需要多人协作的问题
+ ==对于描述复杂的事物，为了从宏观上把握、从整体上分析，我们需要使用面向对象的思路来分析整个系统。但是，具体到细微操作，任然需要面向过程的思路去处理。==

> 什么是面向对象

+ 面向对象编程（Object-Oriented Programming，OOP）
+ 面向对象编程的本质就是：==以类的方式组织代码，以对象的组织（封装）数据。==
+ 抽象：编程思想！
+ 三大特性
  + ==封装==
  + ==继承==
  + ==多态==
+ 从认识论的角度考虑是先有对象后有类。对象，是具体的事物。类，是抽象的，是对对象的抽象
+ 从代码运行的角度考虑是先有类后有对象。类是对象的模板。

### 方法回顾和加深

+ 方法的定义
  + 修饰符
  + 返回类型
  + break和return的区别
  + 方法名
  + 参数列表
  + 异常抛出
+ 方法的调用
  + 静态方法
  + 非静态方法
  + 形参和实参
  + 值传递和引用传递
  + this关键字

### 对象的创建分析

> ##### 类和对象的关系

+ 类是一种抽象的数据类型，它是对某一类事物整体的描述/定义，但是并不能代表某一个具体的事物
+ 对象是抽象概念的具体实例

> ##### 创建与初始化对象

+ ==使用new关键字创建对象==
+ 使用new关键字创建的时候，除了分配内存空间之外，还会给创建好的对象进行默认的初始化以及对类中构造器的调用
+ 类中的构造器也称为构造方法，是在进行创建对象的时候必须要调用的。并且构造器有以下两个特点
  + 必须和类的名字形同
  + 必须没有返回值，也不能写void
+ ==构造器必须要掌握==

### 面向对象三大特征

> ##### 封装

+ 该露的露，该藏的藏
  + 程序设计追求==“高内聚，低耦合”==。高内聚就是类的内部数据操作细节自己完成，不允许外部的干涉；低耦合就是仅暴露少量的方法给外部使用。
+ 封装（数据的隐藏）
  + 通常，应禁止直接访问一个对象中数据的实际表示，二应通过操作接口来访问，这称为信息隐藏。
+ ==属性私有，get/set==

> ##### 继承

+ 继承的本质是对某一批类的抽象，从而实现对现实世界更好的建模。
+ extends的意思是“扩展”。子类是父类的扩展。
+ JAVA类中只有单继承，没有多继承
+ 继承是类和类之间的一中关系。除此之外，类和类之间的关系还有依赖、组合、聚合等
+ 继承关系的两个类，一个为子类（派生类），一个为父类（基类）。子类继承父类，使用关键词extends来表示
+ 子类和父类之间，从意义上讲应该具有“is a”的关系

+ object类

+ super
  
  ```markdown
  super注意点：
      1、super调用父类的构造方法，必须在构造方法的第一个
      2、super必须只能出现在子类的方法或者构造方法中
      3、super和this不能同时调用构造方法
  VS this：
      代表的对象不同：
          this：本身调用着这个对象
          super： 代表父类对象的应用
      前提：
          this：没有继承也可以使用
          super： 只有在继承条件下才可以使用
      构造方法：
          this（）：本类的构造
          super（）：父类的构造
  ```

+ 方法重写
  
  ```markdown
  重写：必须有继承关系，子类重写父类的方法
      1、方法名必须相同
      2、参数列表必须相同
      3、修饰符：范围可以扩大但不能缩小：    public>Protected>Default>private
      4、抛出异常：范围，可以被缩小，但不能扩大：    ClassNotFoundException-->Exception（大）
  重写，子类的方法和父亲必要一致；方法体不同
  为什么需要重写：
      1、父类的功能子类不一定需要，或者不一定不满足
  Alt + Insert：Override
  ```

> ##### ==**多态**==

+ 即统一方法可以根据发送对象的不同而采取多种不同的行为方式

+ 一个对象的实际类型是确定的，但可以指向对象的引用的类型有很多

+ 多态存在的条件
  
  + 有继承关系
  + 子类重写父类方法
  + 父类引用指向子类对象

+ 注意：多态是方法的多态，属性没有多态性

+ instanceof
  
  ```markdown
  类型转换异常！    ClassCastException!
  ```

### 抽象类和接口

> ##### 抽象类

+ ==abstract==修饰符可以用来修饰方法也可以修饰类，如果修饰方法，那么该方法就是抽象方法；如果修饰类，那么该类就是抽象类。
+ 抽象类可以没有抽象方法，但是有抽象方法的类一定要声明为抽象类。
+ 抽象类，不能使用new关键字来创建对象，它是用来让子类继承的。
+ 抽象方法，只有方法的声明，没有方法的实现，它是用来让子类实现的。
+ 子类继承抽象类，那么就必须要实现抽象类没有实现的抽象方法，否则该子类也要声明为抽象类。

> 接口

+ 普通类：只有具体实现
+ 抽象类：具体实现和规范（抽象方法）都有！
+ 接口：==只有规范==
+ 接口就是规范，定义的是一组规则，体现了现实世界中“如果你是...则必须能...”的思想，e.g.如果你是天使，则必须能飞.
+ 接口的本质是契约
+ OO的精髓，是对对象的抽象，最能体现这一点的就是接口。为什么我们讨论设计模式都只是针对具备了抽象能力的语言（e.g. c++,java,c#等），就是因为设计模式所研究的，实际上就是如何合理的去抽象。

```markdown
声明类的关键字是class，声明接口的关键字是interface
```

### 内部类

> 内部类

+ 内部类就是在一个类的内部定义一个类，比如，A类中定义了一个B类，那么B类相对A类来说就称为内部类，而A类相对B类来说就是外部类了。
1. 成员内部类
2. 静态内部类
3. 局部内部类
4. 匿名内部类

# 异常

### 什么是异常（Exception）

异常指程序运行中不期而至的各种状况，如：文件找不到、网络连接失败、非法参数等

### 异常体系结构

+ 检查性异常
+ 运行时异常
  + ArrayIndexOutBoundsException(数组下标越界)
+ 错误ERROR 

### Java异常处理机制

### 处理异常

### 自定义异常

### 总结

# 多线程

线程简介

线程实现

线程状态

线程同步

线程通信问题

高级主题

# 注解

## 什么是注解（Annotation）

+ Annotation是从JDK5.0开始引入的新技术
+ Annotation的作用：
  + 不是程序本身，可以对程序做出解释。（这一点和注释（Comment）没什么区别）
  + 可以被其他程序（比如：编译器等）读取
+ Annotation的格式：
  + 直接是以“@注释名”在代码中存在的，还可以添加一些参数值，例如:@SuppressWarnings(value=“unchecked”).
+ Annotation在哪里使用
  + 可以附加在package,class,method,field等上面,相当于给他们添加了额外的辅助信息我们可以通过反射机制编程实现对这些元数据的访问

## 内置注解

+ @Override:定义在java.lang.OVerride中,此注释只适用于修饰方法,表示一个方法声明打算重写超类中的另一个方法声明.
+ @Deprecated:定义在java.lang.Deprecated中,磁珠是可以用于修饰方法,属性,类,表示不鼓励程序员使用这样的元素,通常是因为他很危险或者存在更好的选择.
+ @SuppressWarnings:定义在java.lang.SuppressWarnings中,用来抑制编译时的警告信息
  + 与前两个注释不同,磁珠是需要添加一个参数才能正确使用
    + @SuppressWarnings(“all”)
    + @SuppressWarnings(“unchecked”)
    + @SuppressWarnings(value={“unchecked”,“deprecation”})
    + 等等......

> ### 元注解

+ 元注解的作用就是负责注解其他注解,java定义了4个标准的meta-annotation类型,他们被用来通过对其他annotation类型作说明.

+ 这些类型和他们所支持的类在java.lang.annotation包中找到(@Target,@Retention,@Documented,@Inherited)
  
  + @Target:用于描述注解的使用范围(即:被描述的注解可以用在什么地方)
  + @Retention:表示需要在什么级别保存该注释信息,用于描述注解的生命周期
    + (SOURCE<CLASS<RUNTIME)
  + @Documented:说明该注解将被包含在javadoc中
  + @Inherited:说明子类可以继承父类中的该注解

> ### 自定义注解

+ 使用@interface自定义注解时,自动继承了java.lang.annotation接口
  + @interface用来声明一个注解,格式:public@interface 注解名{定义内容}
  + 其中的每一个方法实际上都声明了一个配置参数
  + 方法的名称就是参数的名称
  + 返回值类型就是参数的类型(返回值只能是基本类型,Class,String,enum).
  + 可以通过default来声明参数的默认值
  + 如果只有一个参数成员,一般参数名为value
  + 注解元素要有值,我们定义注解元素时,经常使用空字符串,0作为默认值

# 反射

## java反射机制概述

> ### java Reflection

+ Reflection(反射)是java被视为动态语言的关键,反射机制允许程序在执行期借助于Reflection API取得任何类的内部信息,并能直接操作任意对象的内部属性及方法.
  
  `Class c = Class.forName("java.lang.String")`

+ 加载完类后,在堆内存的方法区中就产生了一个Class类型的对象(一个类只有一个Class对象),这个对象就包含了完整的类的结构信息.我们可以通过这个对象看到类的结构.
  
  ![image-20210412152820115.png](http://lsky.volerde.space/i/2022/05/16/62823afdab38c.png)

> ### Java反射机制研究及应用

Java反射机制提供的功能

+ 在运行时判断任意一个对象所属的类
+ 在运行时构造任意一个类的对象
+ 在运行时判断任意一个类所具有的成员变量和方法
+ 在运行时获取泛型信息
+ 在运行时调用任意一个对象的成员变量和方法
+ 在运行时处理注解
+ 生成动态代理
+ ......

## 理解Class类并获取Class实例

> ### Class类

反射可以得到的信息:某个类的属性,方法和构造器,某个类到底实现了哪些接口对于每个类而言,JRE都为其保留了一个不变的Class类型的对象.一个Class对象包含了特定某个结构(class/interface/enum/annotation/primitive type/void[])的有关信息

+ Class本身也是一个类
+ Class对象只能有系统建立对象
+ 一个加载的类在JVM中只会有一个Class实例
+ 每个类的实例都会记得自己是有哪个Class实例所生成
+ 通过Class可以完整地得到一个类中的所有被加载的结构
+ Class类是Reflection的根源,针对任何你想动态加载,运行的类,唯有先获得相应的Class对象

> ### Class类的常用方法

| 方法名                                          | 功能说明                               |
| -------------------------------------------- | ---------------------------------- |
| static ClassforName(String name)             | 返回指定类名name的Class对象                 |
| Object newInstance()                         | 调用缺省构造函数,返回Class对象的一个实例            |
| getName                                      | 返回此Class对象所表示的实体(类,接口,数组类或void)的名称 |
| Class getSuperClass()                        | 返回当前Class对象的父类的Class对象             |
| Class[] getinterfaces()                      | 获取当前Class对象的接口                     |
| Constructor[] getConstructors()              | 返回一个包含某些Constructor对象的数组           |
| ClassLoader getClassLoader()                 | 返回该类的类加载器                          |
| Method getMethod(String name,Class....    T) | 返回一个Method对象,此对象的形参类型为param Type   |
| Field[] getDeclaredFields()                  | 返回Field对象的一个数组                     |

> ### 获取Class类的实例

1. 若已知具体的类,通过类的class属性获取,该方法最为安全可靠,程序性能最高
   
   `Class clazz = Person.class;`

2. 已知某个类的实例,调用该实例的getClass()方法获取Class对象
   
   `Class clazz = person.getClass();`

3. 一直一个类的全类名,且该类在类路径下,可通过Class类的静态方法forName()获取,可能抛出ClassNotFoundException
   
   `Class clazz = Class.forName("demo01.Student");`

4. 内置基本数据类型可以直接用类名.Type

5. 还可以利用ClassLoader

> ### 哪些类型可以有Class对象

+ class:外部类,成员(成员内部类,静态内部类),局部内部类,匿名内部类.
+ interface:接口
+ []:数组
+ enum:枚举
+ annotation:注解@interface
+ primitive type:基本数据类型
+ void

## 类加载与ClassLoader

> ### Java内存分析

+ Java内存
  + 堆
    + 存放new的对象和数组
    + 可以被所有的线程共享,不会存放别的对象引用
  + 栈
    + 存放基本变量类型(会包含这个基本类型的具体数值)
    + 引用对象的变量(会存放这个引用在堆里面的具体地址)
  + 方法区
    + 可以被所有的线程共享
    + 包含了所有的class和static变量

> ### 了解:类的加载过程

当程序主动使用某个类时,如果该类还未被加载到内存中,则系统会通过如下三个步骤对该类进行初始化

![image-20210412170941253.png](http://lsky.volerde.space/i/2022/05/16/62823afe0f917.png)

> ### 类的加载与ClassLoader的理解

+ 加载:将class文件字节码内容加载到内存中,并将这些静态数据转换成方法区的运行时数据结构,然后生成一个代表这个类的java.lang.Class对象
+ 链接:将Java类的二进制代码合并到JVM的运行状态之中的过程
  + 验证:确保加载的类信息符合JVM规范,没有安全方面的问题
  + 准备:正式为类变量(static)分配内存斌设置类变量初识默认值的阶段,这些内存都将在方法去中进行分配
  + 解析:虚拟机常量池内的符号引用(常量名)替换为直接引用(地址)的过程
+ 初始化:
  + 执行类构造器<clinit>()方法的过程.类构造器<clinit>()方法是由编译器自动收集类中的所有类变量的赋值动作和静态代码块中的语句合并产生的.(类构造器是构造类信息的,不是构造该类对象的构造器).
  + 当初始化一个类的时候,如果发现其父类还没有进行初始化,则需要先触发其父类的初始化.
  + 虚拟机会保证一个类的<linit>()方法在多线程环境中被正确加锁和同步.

> ### 什么时候会发生类初始化?

+ 类的主动引用(一定会发生类的初始化)
  + 当虚拟机启动,先初始化main方法所在的类
  + new一个类的对象
  + 调用类的静态成员(除了final常量)和静态方法
  + 使用java.lang.reflect包的方法对类进行反射调用
  + 当初始化一个类，如果其父类没有被初始化，则先会初始化它的父类
+ 类的被动引用（不会发生类的初始化）
  + 当访问一个静态域是，只有真正声明这个域的类才会被初始化。如：当通过子类引用弗雷的静态常量，不会导致子类初始化
  + 通过数组定义类引用，不会触发此类的初始化
  + 引用常量不会触发此类的初始化(常量在链接阶段就存入调用类的常量池中了)

> ### 类加载器的作用

+ 类加载的作用：将class文件字节码内容加载到内存中，并将这些静态数据转换成方法区的运行时数据结构，然后在堆中生成一个代表这个类的java.lang.Class对象，作为方法区中类数据的访问入口。

+ 类缓存：标准的JavaSE类加载器可以按要求查找类，但一旦某个类被加载到类加载器中，它将维持加载（缓存）一段时间.不过JVM垃圾回收机制可以回收这些Class对象.
  
  ![image-20210412191422878.png](http://lsky.volerde.space/i/2022/05/16/62823afe54d6c.png)

## 获取运行时类的完整结构

> ### 获取运行时类的完整结构

通过反射获取运行时类的完整结构

Field,Method,Constructor,Superclass,Interface,annotation

+ 实现的全部接口
+ 所继承的父类
+ 全部的构造器
+ 全部的方法
+ 全部的Field
+ 注解
+ ...

> ### 调用指定的方法

通过反射，调用类中的方法，通过Method类完成

1. 通过Class类的getMethod(String name,Class...parameterTypes)方法取得一个Method对象,并设置测方法操作时所需要的参数类型

2. 使用object invoke(Object obj,Object[] args)进行调用,并向方法中传递要设置的obj对象的参数信息
   
   ![image-20210413111305364.png](http://lsky.volerde.space/i/2022/05/16/62823afeccd29.png)

==Object invoke(Object obj,Object...args)==

+ Object对应原方法的返回值,若原方法无返回值,此时返回null
+ 若原方法为静态方法,此时形参Object obj可为null
+ 若原方法形参列表为空,则Object[] args为null
+ 若原方法声明为private,则需要在调用次invoke()方法前,显示调用方法对象的setAccessible(true)方法,即可访问private的方法.

==SetAccessible==

+ Method和Field,Constructor对象都有setAccessible()方法
+ setAccessible作用是启动和禁用访问安全检查的开关
+ 参数值为true则指示反射的对象在使用是应该取消java语言访问检查
  + 提高反射的效率.如果代码中必须使用反射,而该句代码需要频繁的被调用,那么设置为true
  + 使得原本无房访问的私有成员也可以访问
+ 参数值为false则指示反射的对象应该实施Java语言访问检查

```java
public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException, NoSuchMethodException, InvocationTargetException, NoSuchFieldException {
    Class<?> aClass = Class.forName("reflection.Test01$User");

    //构造对象
    Test01.User o = (Test01.User) aClass.newInstance();//本质上调用了无参构造器
    System.out.println(o);

    //通过构造器创建对象
    //调用了有参构造器
    Constructor<?> constructor = aClass.getDeclaredConstructor(String.class, int.class, int.class);
    Object volunteer = constructor.newInstance("Volunteer", 001, 19);
    System.out.println(volunteer);

    //通过反射调用普通方法
    Test01.User instance = (Test01.User) aClass.newInstance();
    //通过反射获取一个方法
    Method sige = aClass.getDeclaredMethod("sige", String.class);
    //invoke:激活
    //invoke(对象,"激活的值");
    sige.invoke(instance,"sige");
    System.out.println(instance.getName());

    //通过反射操作属性
    Test01.User user = (Test01.User) aClass.newInstance();
    Field name = aClass.getDeclaredField("name");

    name.setAccessible(true);
    name.set(user,"nameless");
    System.out.println(user.getName());
}
```
