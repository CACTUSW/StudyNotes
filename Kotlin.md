# Kotlin

### 简史

**诞生之初**

2011年，由JetBrains开发，运行于JVM，2017年官宣成为Android开发语言。kotlin语法简洁，具备现代高级语言特性，能与java无缝互操作。

**Kotlin与JVM**

编译型语言，更多的依赖于编译器

<img src="Kotlin.assets/image-20220731001002036.png" alt="image-20220731001002036" style="zoom:80%;" />

**Why Kotlin？**

+ Java语言比较稳健，久经考验。多年来，他一直是最常用的一种编程语言，因此造就了庞大的生产代码库。自1995年java问世以来，对于优秀的编程语言应该满足什么样的条件，人们已通过实践积攒了很多的经验教训。然而，java却裹足不前，开发者喜欢的很多现代语言的高级特性，他都没有，或者迟迟加入
+ Kotlin从这些经验教训中受益良多，而Java中的某些早期设计却愈显陈旧。脱胎于旧语言，Kotlin解决了他们的很多痛点，成为了一门优秀的语言。相比Java，Kotlin进步巨大，带来了更可靠的开发体验。

 **Kotlin的跨平台特性**

+ Kotlin不仅支持编写代码在虚拟机上运行，而且还是一门跨平台的通用型语言，我们可以用Kotlin开发各种类型的原生应用，如Android、macOS、Windows、JavaScript应用。
+ Kotlin能脱离虚拟机层，直接编译成可以在Windows、Linux、macOS平台上运行的原生二进制代码

### 初识

##### 变量

$\huge \underbrace {var}_{变量定义关键字}\underbrace{maximumAge}_{变量名}\underbrace{:Int}_{类型定义} \overbrace=^{赋值运算符} \underbrace5_{赋值}$







**Kotlin内置数据类型**

| 类型    | 描述             | 示例                                    |
| ------- | ---------------- | --------------------------------------- |
| String  | 字符串           | "Hello World"                           |
| Char    | 单字符           | 'A'                                     |
| Boolean | true/false       | true false                              |
| Int     | 整数             | 5                                       |
| Double  | 小数             | 3.14                                    |
| List    | 元素集合         | 1，8,10    \|    "Jack","Rose","Jack"   |
| Set     | 无重复元素的集合 | "Jack","Jason","Jacky"                  |
| Map     | 键值对集合       | "small" to 5,"medium" to 8,"large" to 9 |

**只读变量**

+ 声明可修改变量，使用var关键字
+ 声明只读变量，使用val关键字

**类型推断**

+ 类型推断：对于已声明并赋值的变量，Kotlin允许省略类型定义

**编译时常量**

+ 只读变量并非绝对只读
+ 编译时常量只能在函数外定义，因为编译时常量必须在编译时赋值，而函数都是在运行时才掉用，函数内的变量也是在运行时赋值，编译时常量要在这些变量赋值钱就已存在。
+ 编译时常量只能是常见的基本数据类型：String、Int、Double、Float、Long、Short、Byte、Char、Boolean。

**Kotlin的引用类型与基本数据类型**

+ Java有两种数据类型：引用类型与基本数据类型
+ Kotlin只提供引用类型这一种数据类型，处于更高性能的需要，Kotlin编译器会在Java字节码中改用基本数据类型

##### 条件语句

**表达式**

+ if/else if表达式

+ range表达式

  + in A..B，in关键字用来检查某个值是否在指定范围之内

    ```kotlin
    val result = if(i in 1..3) {
        //code
    } else if(i in 4..12) {
        //code
    } else {
        //code
    }
    ```

    

+ when表达式

  + 允许编写条件式，在某个条件满足时，执行对应的代码

  + 只要代码包含else if分支，都建议改用when表达式

    ```kotlin
    val school = "小学"
    val result = when(school) {
        "小学" -> "小"
        "中学" -> "中"
        "大学" -> "大"
        else -> {
            "未知"
        }
    }
    ```

    

**String模板**

+ 模板支持在字符串的引号内放入变量值

  ```kotlin
  val origin = "Jack"
  val dest = "Rose"
  println("$origin love $dest")
  ```

  

+ 支持字符串里计算表达式的值并插入结果，添加在${}中的任何表达式，都会作为字符串的一部分求值

  ```kotlin
  val flag = false
  println("Anwser is: ${if(flag) "我可以" else "我不行"}")
  ```

##### 函数

  $\huge \overbrace{private}^{可见性修饰符}\underbrace{fun}_{函数声明关键字}\overbrace{doSomthing}^{函数名}(\underbrace{age:Int, flag:Boolean}_{函数参数}):\overbrace{String}^{返回类型}$

  

**函数参数**

+ 默认值参

  + 如果不打算传入参数，可以预先给参数指定默认值

    ```kotlin
    fun function(name:String, age:Int = 2) {
        //code
    }
    ```

    

+ 具名函数参数

  + 如果使用命名值参，就可以不用管值参的顺序

    ```kotlin
    fun main() {
        function(age = 4, name = "Volerde")
    }
    ```

**Unir函数**

+ 不是所有函数都有返回值，Kotlin中没有返回值的函数叫Unit函数，也就是说他们的返回类型都是Unit。在Kotlin之前，函数不返回任何东西用void描述，意思为"没有返回类型，忽略"，即如果函数不返回任何东西，忽略。但是，void这种解决方案无法解释现代语言的一个重要特征，泛型。

**Nothing类型**

+ TODO函数的任务就是抛出异常，就是永远别指望它能运行成功，返回Nothing类型

  ```kotlin
  public inline fun TODO(reason:String):Nothing = throw NotImplementedError
  ```


**反引号中的函数名**

+ Kotlin可以使用空格和特殊字符对函数命名，不过函数名要用一对反引号括起来。

  ```kotlin
  fun `**~special function with weird name**`() {
      //code
  }
  ```

  

+ 为了支持Kotlin和Java互操作，但Kotlin和Java各自却有着不同的保留关键字，不能作为函数名，使用反引号括住函数名就能避免任何冲突。

  ```kotlin
  fun main() {
  	MyJava.`is`()
  }
  ```

##### 匿名函数

+ 定义时不取名字的函数，称为匿名函数，匿名函数通常整体传递给其他函数，或者从其他函数返回。

+ 匿名函数对Kotlin来说很重要，有了它，我们能根据需要制定特殊规则，轻松定制标准库里的内置函数

  ```kotlin
  val count = "Mississippi".count({letter -> letter == 's'})
  val count = "Mississippi".count {e ->e == 's'}
  ```

**函数类型与隐式返回**

+ 匿名函数也有类型，匿名函数可以当做变量赋值给函数类型变量，就像其他变量一样，匿名函数就可以在代码里传递了。变量有类型，变量可以等于函数，函数也会有类型。函数的类型，由传入的参数和返回值类型决定

+ 和具名函数不一样，除了极少数情况外，匿名函数不需要return关键字返回数据，匿名函数会隐式或自动返回函数体最后一行语句的结果

  ```kotlin
  val function:()->String = {
      val holiday = "New Year."
      "Happy $holiday"
  }
  println(function)
  ```

**函数参数**

+ 与具名函数一样，匿名函数可以不带参数，也可以带一个或者多个任何类型的参数，需要带参数时，参数的类型放在匿名函数的类型定义中，参数名则放在函数定义中

  ```kotlin
  val function:(String) ->String = {name ->
      val holiday = "New Year."
      "$name,happy $holiday"
  }
  println(function("Volerde"))
  ```

**it关键字**

+ 定义只有一个参数的匿名函数时，可以使用it关键字来表示参数名。当你需要传入两个值参的时候，it关键字就不能用了

  ```kotlin
  val function:(String) ->String = {
      val holiday = "New Year."
      "$it,happy $holiday"
  }
  println(function("Volerde"))
  ```

**类型推断**

+ 定义一个变量时，如果已把匿名函数作为变量赋值给它，就不需要显示指名变量类型了

  ```kotlin
  val function = {
      val holiday = "New Year."
      "happy $holiday"
  }
  println(function)
  ```

+ 类型推断也支持带参数的匿名函数，但为了帮助编译器更准确的推断变量类型，匿名函数的参数名和参数类型必须都有

  ```kotlin
  val function:(String,Int) -> String = {name,year ->
      val holiday = "New Year"
  	"$name, Happy $holiday,$year"
  }
  ```

  ```kotlin
  val function = {name:String,year:Int ->
  	val holiday = "New Year"
  	"$name, Happy $holiday,$year"
  }
  ```

**lambda**

+ 我们将匿名函数称为lambda。将它的定义称为lambda表达式，它返回的数据称为lambda结果。lambda可以用希腊字符$\lambda$表示，是lambda演算的简称，lambda演算是一套数理演算逻辑，有数学家Alonzo Church（阿隆佐·丘齐）与20世纪30年代发明，在定义匿名函数时，使用了lambda演算记法

**定义参数是函数的函数**

+ 函数的参数是另一个函数

  ```kotlin
  fun main() {
      val getDiscount = {goodsName:String,hour:Int ->
          val currentYear = 2022
          "$currentYear ,$goodsName has $hour"
      }
      showOnBoard("Apple",getDiscount)
  }
  fun showOnBoard(goodsName:String,getDiscountWords: (String, Int) -> String) {
      val hour = (1..24).shuffled().last()
      println(getDiscountWords(goodsName,hour))
  }
  ```

**简略写法**

+ 如果一个函数的lambda参数排在最后，或者是唯一的参数，那么括住lambda值参的一堆圆括号就可以省略

  ```kotlin
  val count = "Mississippi".count {it == 's'}
  ```

  ```kotlin
  showOnBoard("Apple") { goodsName: String, hour: Int ->
  	val currentYear = 2022
  	"$currentYear ,$goodsName has $hour"
  }
  ```

**函数内联**

+ lambda可以更灵活的编写应用，但是，灵活需要代价
+ 在JVM上，定义的lambda会以对象实例的形式存在，JVM会为所有同lambda打交道的变量分配内存，这就产生了内存开销。更糟的是，lambda的内存开销会带来严重的性能问题。然而幸运的是，kotlin有一种优化机制叫内联，有了内联，JVM就不需要使用lambda对象实例了，因而得以避免变量内存分配。哪里需要使用lambda，编译器就会将函数体复制粘贴到哪里
+ 使用lambda的递归函数无法内联，因为会导致复制粘贴的无限循环，编译会发出警告

**函数引用**

+ 要把函数作为参数传给其他函数使用，除了传lambda表达式，kotlin还提供了其他方法，传递函数引用，函数引用可以把一个具名函数转换成一个值参，使用lambda表达式的地方，都可以使用函数引用

  ```kotlin
  fun main() {
      //要获得函数引用，使用::操作符，后跟要引用的函数名
      showOnBoard("Pear",::getDiscountWords)
  }
  fun getDiscountWords(goodsName:String,hour:Int):String {
      val currentYear = 2022
      return "${currentYear}年,$goodsName has $hour"
  }
  fun showOnBoard(goodsName: String, getDiscountWords: (String, Int) -> String) {
      val hour = (1..24).shuffled().last()
      println(getDiscountWords(goodsName, hour))
  }
  ```

**函数类型作为返回类型**

+ 函数类型也是有效的返回类型，即可以定义一个能返回函数的函数

  ```kotlin
  fun main() {
      val getDiscountWords = configDiscountWords()
      println(getDiscountWords("Watermelon"))
      //合写上两行
      println(configDiscountWords()("Watermelon"))
  }
  fun configDiscountWords():(String) ->String {
      val currentYear = 2022
      val hour = (1..24).shuffled().last()
      return {goodsName:String ->
          "${currentYear}年,$goodsName has ${hour}h"
      }
  }
  ```

**闭包**

+ kotlin中，匿名函数能修改并引用定义在自己的作用域之外的变量，匿名函数引用着定义自身的函数里的变量，kotlin中的lambda就是闭包

  ```kotlin
  fun configDiscountWords():(String) ->String {
      val currentYear = 2022
      val hour = (1..24).shuffled().last()
      return {goodsName:String ->
          //匿名函数引用着来自configDiscountWords中定义的变量current，hour
          //此匿名函数共享configDiscountWords的作用域
          "${currentYear}年,$goodsName has ${hour}h"
      }
  }
  ```

+ 能接收函数或者返回函数的函数叫做高级函数，高级函数广泛应用于函数式编程当中

![image-20220801021219953](Kotlin.assets/image-20220801021219953.png)

<div style="text-align:center;font-size:20px">function3共享function2的作用域，function2贡献function1的作用域</div>

**lambda与匿名内部类**

+ 为什么要在代码中使用函数类型？函数类型能让开发者少写模式化代码，写出更灵活的代码。Java8支持面向对象编程和lambda表达式，但不支持将函数作为参数传给另一个函数或变量，不过Java的替代方案是匿名内部类

  ```java
  public static void main(String[] args) {
      showOnBoard("Strawberry", (goodsName, hour) -> {
          int currentYear = 2022;
          return currentYear+"年,"+goodsName+" has+"+hour+"h";
      });
  }
  public interface DiscountWords{
      String getDiscountWords(String goodsName,int hour);
  }
  public static void showOnBoard(String goodsName,DiscountWords discountWords) {
      int hour = new Random().nextInt(24);
      System.out.println(discountWords.getDiscountWords(goodsName,hour));
  }
  ```

##### null安全

+ 在Java中司空见惯的空指针异常NullPointerException，带来了很多麻烦。Kotlin作为更强大的语言，势必基于以往语言设计经验对其进行改良。Kotlin更多地把运行时可能会出现的null问题，以编程时错误的方式，提前在编译期强迫重视起来，而非等运行时报错，防患于未然

**可空性**

+ 对于null值问题，Kotlin反其道行之，除非另有规定，变量不可为null值，如此，运行时崩溃问题从根源解决

**Kotlin的null类型**

+ 为了避免NullPointerException，Kotlin的做法是不让给非空类型赋值null，但null在Kotlin中依然存在

  ```kotlin
  val str:String? = "Volerde"
  str = null
  ```

**null安全与异常**

+ Kotlin区分可空类型和非可空类型，所以，要让一个可空类型变量运行，而它又可能不存在，对于这种潜在危险，编译器时刻警惕。为了应对这种风险，Kotlin不允许在可空类型值上调用函数，除非主动接手安全管理

**安全调用操作符**

+ 有安全调用操作符时，编译器遇到null值，就跳过函数调用，而不是返回null

  ```kotlin
  var str:String? = "volerde"
  str = null
  //跳过函数执行，而不是返回null
  str?.replaceFirstChar { 'V' }
  ```

**带let的安全调用**

+ 安全调用允许在可空类型上调用函数，但是若要创建新值，或者判断不为null了就调用其他函数，怎么办？使用带let函数的安全调用操作符。可以在任何类型上调用let函数它的主要作用是在指定的作用域内定义一个或多个变量

  ```kotlin
  var str:String? = "volerde"
  str = null
  str = str?.let {
      //非空白字符串
      if (it.isNotBlank()) {
          it.replaceFirstChar { 'V' }
      }else {
          "Volerde"
      }
  }
  ```

**非空断言操作符**

+ !!.又称感叹号操作符，当变量值为null时，会抛出KotlinNullPointerException

  ```kotlin
  val str = readLine()!!.capitalize()
  ```

**if判断null值情况**

+ 我们也可以使用if判断，但是相比之下安全调用操作符用起来更灵活，代码更简洁。可以使用安全操作符进行多个函数的链式调用

**使用空合并操作符**

+ ?:操作符的意思是，如果左边的求值结果为null，就是用右边的结果值

  ```kotlin
  val newStr = str ?: "Volerde"
  ```

+ 空合并操作符也可以和let函数一起使用来代替if/else语句

  ```kotlin
  var str: String? = "volerde"
  str =
      str?.let {
          it.replaceFirstChar {
              if (it.isLowerCase()) it.titlecase(Locale.getDefault())
              else it.toString()
          }
      } ?: "Hanau"
  ```

**异常**

+ 抛出异常
+ 自定义异常
+ 异常处理

**先决条件函数**

+ Kotlin标准库提供了一些便利函数，使用这些内置函数，你可以抛出带自定义信息的异常，这些便利函数叫做先决条件函数，可以使用它定义先决条件，条件必须满足，目标代码才能执行

  | 函数           | 描述                                                         |
  | -------------- | ------------------------------------------------------------ |
  | checkNotNull   | 如果参数为null，则抛出IllegalStateException异常，否则返回非null值 |
  | require        | 如果参数为false，则抛出IllegalArgumentException异常          |
  | requireNotNull | 如果参数为null，则抛出IllegalStateException异常，否则返回非null值 |
  | error          | 如果参数为null，则抛出IllegalStateException异常并输出错误信息，否则返回非null值 |
  | assert         | 如果参数为false，则抛出AssertError异常，并打上断言编译器标记 |

##### 字符串操作

**substring**

+ 字符串截取，substring函数支持IntRange（表示一个整数范围的类型）的参数，until创建的范围不过阔上限值

  ```kotlin
  val name = "Volerde and Hanau"
  val index = name.indexOf(" ")
  //val str = name.substring(0,index)
  val str = name.substring(0 until index)
  ```

**split**

+ split函数返回的是List集合数据，List集合支持解构语法特性，允许在一个表达式里给多个变量赋值，解构常用来简化变量的赋值

  ```kotlin
  val name = "Volerde,LunarDust,Hanau"
  val split = name.split(",")
  val (origin,dest,proxy) = name.split(",")
  println("$origin $dest $proxy")
  ```

**replace**

+ 字符串替换

  ```kotlin
  val str = "The people's republic of china"
  //第一个参数，正则表达式，用来确定要替换哪些字符
  //第二个参数匿名函数，用来确定该如何替换正则表达式搜索到的字符
  val replace = str.replace(Regex("[aeiou]")) {
      when (it.value) {
          "a" -> "8"
          "e" -> "6"
          "i" -> "9"
          "o" -> "1"
          "u" -> "3"
          else -> it.value
      }
  }
  ```


**字符串比较**

+ 在Kotlin中，用=\=检查两个字符串中的字符是否匹配，用===检查两个变量是否指向内存堆上同一对象，而在Java中=\=做引用比较，做结构比较时用equals方法

  ```kotlin
  val str1 = "Volerde"
  val str2 = "Volerde"
  val str3 = "volerde".capitalize()
  println(str1 == str3)// true
  println(str1 === str2)// true ->指向常量池的同一内容
  println(str1 === str3)// false
  ```

**forEach**

+ 遍历字符

  ```kotlin
  "The people's Republic of China.".forEach {
  	println("$it")
  }
  ```

##### 数字类型

+ 和Java一样，Kotlin中所有数字类型都是又符号的，也就是说既可以表示正数，也可以表示负数

  | 类型   | 位   | 最大值                 | 最小值               |
  | ------ | ---- | ---------------------- | -------------------- |
  | Byte   | 8    | 127                    | -128                 |
  | Short  | 16   | 32767                  | -32768               |
  | Int    | 32   | 2147483647             | -2147483648          |
  | Long   | 64   | 9223372036854775807    | -9223372036854775808 |
  | Float  | 32   | 3.4028235E38           | 1.4E-45              |
  | Double | 64   | 1.7976931348623157E308 | 4.9E-324             |

 **安全转换函数**

+ Kotlin提供了toDoubleorNull和toIntOrNull这样的安全转换函数，如果数值不能正确转换，就返回null

  ```kotlin
  val number: Int? = "88.48".toIntOrNull()// 未转成功，number = null
  ```

**Double转Int**

+ 精度损失与四舍五入

  ```kotlin
  println(8.78.toInt())//精度损失
  println(8.78.roundToInt())//四舍五入
  ```

**Double类型格式化**

+ 格式化字符串是一串特殊字符，它决定该如何格式化数据

  ```kotlin
  val str = "%.2f".format(8.78787)
  ```

##### 标准库函数

**apply**

+ apply可以看做一个配偶函数，传入一个接收者，然后调用一系列函数来配置它，如果提供lambda给apply函数执行，会返回配置好的接收者

  ```kotlin
  val fileWithoutApply = File("E://example.txt")
  	fileWithoutApply.setReadable(true)
  	fileWithoutApply.setWritable(true)
  	fileWithoutApply.setExecutable(true)
  val fileWithApply = File("E://example.txt").apply { 
      setExecutable(true)
      setReadable(true)
      setExecutable(true)
  }
  ```

+ 当调用一个个函数类配置接收者时，变量名就省略掉了，这是因为在lambda表达式里，apply能让每个配置函数都作用与接收者，这种行为叫做相关作用域，以为lambda表达式里所有的函数调用都是针对接收者的，或者说，他们是针对接收者的隐式调用

**let**

+ let函数能使某个连梁作用域其lambda表达式里，让it关键字能引用它。let和apply比较，let会把接收者穿个lambda，而apply什么都不传，匿名函数执行完，apply会返回当前接收者，而let会返回lambda的最后一行

  ```kotlin
  val result = listOf(3,2,1).first().let { 
      it * it
  }
  // 不用let
  val firstElement = listOf(3, 2, 1).first()
  val secondElement = firstElement * firstElement
  ```

  ```kotlin
  fun formatGreeting(guestName: String?): String {
      return guestName?.let {
          "Welcome, $it"
      } ?: "What's your name?"
  }
  // 不用let
  fun formatGreeting2(guestName: String?): String {
      return if (guestName != null) {
          "Welcome, $guestName"
      } else {
          "What's your name?"
      }
  }
  ```

**run**

+ 单看作用域行为，run和apply差不多，但与apply不同，run函数不返回接收者，run返回的是lambda结果

  ```kotlin
  val file = File("E:example.txt")
  val result = file.run {
  	readText().contains("great")
  }
  ```

+ run也能用来执行函数引用

  ```kotlin
  val result = "The people's Republic of China".run(::isLong)
  //当有多个函数调用时，run优势显而易见
  "The people's Republic of China"
  	.run(::isLong)
  	.run(::showMessage)
  	.run(::println)
  
  fun isLong(name: String) = name.length >= 10
  fun showMessage(isLong: Boolean): String {
      return if (isLong) {
          "Name is too long"
      } else {
          "Please rename"
      }
  }
  ```


**with**

+ with函数是run的变体，他们的功能是一样的，但是with的调用方式不同，调用with时需要值参作为其第一个参数传入

  ```kotlin
  val isToolong1 = "The people's Republic of China.".run {
      length >= 10
  }
  val isTooLong2 = with("The people's Republic of China.") {
  	length >= 10
  }
  ```

**also**

+ also函数和let函数功能相似，和let一样，also也是吧接收者作为值参传给lambda，但是also返回接收对象，而let返回lambda结果。因此，also适合针对同一原始对象，利用副作用做事。所以，可以基于原始接收者对象执行额外的链式调用。

  ```kotlin
  var fileContents: List<String>
  File("E://example.txt")
      .also {
          println(it.name)
      }.also {
          fileContents = it.readLines()
      }
  ```

**takeIf**

+ takeIf和其他标准函数有点不一样，takeIf函数需要判断lambda中提供的条件表达式，给出true或false结果，如果判断结果为true，从takeIf函数返回接收者对象，如图是false，则返回null。如过需要判断某个条件是否满足，再决定是否可以赋值变量或者执行某项任务，。takeIf就非常有用。takeIf类似if语句，但它的优势是可以直接在对象实例上调用，避免了临时变量赋值的麻烦

  ```kotlin
  val fileContents = File("E://example.txt")
      .takeIf { it.canRead() && it.canWrite() }
      ?.readText()
  //不用takeIf函数
  val file = File("E://example.txt")
  val fileContents2 = if (file.canRead() && file.canWrite()) {
      file.readText()
  } else {
      null
  }
  ```

**takeUnless**

+ takeIf辅助函数takeUnless，只有判断条件是false时才会返回原始接收者对象

  ```kotlin
  val fileContents3 = File("E://example.txt")
      .takeUnless { it.isHidden }
      ?.readText()
  ```

##### 集合

+ 集合可以方便处理一组数据，也可以作为值参传给函数，和其他变量类型一样，List、Set、Map类型的变量也分为两类，只读和可变

**List创建与元素获取**

+ getOrElse是一个安全索引值函数，需要两个参数，第一个为索引值，第二个是能提供默认值的lambda表达式，如果索引值不存在的话，可以用爱代替异常

+ getOrNull是Kotlin提供的另一个安全索引取值函数，它返回null结果，而不是抛出异常

  ```kotlin
  val list = listOf("Volerde", "LunarDust", "Hanau")
  println(list.getOrElse(4) { "Unknown" })
  println(list.getOrNull(4))
  println(list.getOrNull(4) ?: { "Unknown" })
  ```

  













