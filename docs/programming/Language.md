# 语言

> 体系记忆|巩固语言

- [语言](#语言)
  - [java](#java)
  - [Kotlin](#kotlin)
  - [Python](#python)


## java

> 封装、继承、多态

**类型和变量：** 它就是固定数据的范围和对数据的指向

- 内存中都是二进制数，为了方便操作数据，才有了数据类型，然后用变量来指向这个数据类型

- Java中除了基本数据类型外其它都是对象类型，对象由基本数据类型、数组、其它对象组成的

**赋值：**

- 首先变量就是指向了内存中一块地方，这块地方的实际值在赋值后确定

- 基本数据类型、数组、对象的赋值不同，给变量赋值是改变变量指向内存中的值，而给数组和对象赋值是改变引用的指向。   为什么数组和对象的赋值需要两块内存空间，然后改变值是改变引用指向的位置？基本类型用一块内存然后可以直接改变里面的值是因为一开始就规定了这块内存大小，但是数组和对象的赋值是不能确定内存大小的，如果把一个内存大的赋值给一个内存小的就会容纳不下，出现错误，所以只能修改它们指向的位置，就是修改地址值

  ```java
  数组赋值三种赋值方式：
  int[] arr = {1,2,3};
  int[] arr = new int[]{1,2,3};
  int[] arr = new int[3];arr[1] = 1;arr[2] = 2;arr[3] = 3;
  ```

**基本运算和执行流程：**

下面这些运算就是基本类型的运算，而其他类型的运算可以说是方法

- 算术运算：+、-、*、/、++、--、 ...
- 比较运算：大于（＞）、大于等于（>=）、小于（＜）、小于等于（<=）、等于（==）、不等于（! =）。
- 逻辑运算：与&、或|、非！、短路与&&、短路或||

流程就是CPU运行指示器指向的下一条指令的过程

- 顺序

- 条件：if else,if else if else,if, switch

- 循环：for、while、foreach     循环控制：break、continue

**函数：** 本质是减少重复代码和分解复杂操作

- 组成：修饰符、返回类型、函数名、参数、内部操作、是否return

- 参数传递引用类型就改变了实际值，比如参数是数组；可变长度的参数 int ... a 实际上就是一个数组参数

- 返回值：返回结果(有返回值的情况)给调用方

- 函数名可以重复，意思就是重载方法，表达操作相同，参数不同

- 尽量不用递归方法，因为容易造成栈溢出错误java.lang.StackOverflowError，因为每次调用方法都会在栈中存放参数，临时变量值，返回地址

  栈中没有变量指向堆内存中那块数据后，Java会执行垃圾回收堆内存


**<u>java面向对象</u>**

**类和面向对象：** 计算机中只有基本数据类型和其它类型(类)

- 类就是自定义数据类型，是数据和动作的容器，由类变量、类方法、实例变量、实例方法组成。实例方法可以访问类变量和类方法

- Point   p;   只是给了一个变量，除了声明它是Point类型的变量没有别的意义，p = new Point();后才给这个变量赋值，这样实际内存new出来了，p就有了指向，指向堆中一块空间

- 通过对象来操作对象的数据和方法。this表示当前实例

- 构造方法的作用就是new对象的时候初始化值，空参数的话对象的变量都是默认值，返回值就是实例本身。为什么有了参数构造方法就不能调用空参数构造方法，因为这样Java认为用户知道自己在干嘛，所以就用用户自己定义的带参构造方法，就不去再创建一份空构造方法

- 类和对象的生命周期就是类在内存中只加载一份，而对象有很多份，这样就可以使用不同的对象实例了

**类和类的组合：**

- 类的组合和Java一样，从二进制-基本类型-类-对象，相应的组合就是从点-线-面-体不断提升层次

- 道生一，一生二，二生三，三生万物

**类的组织方式：** 包package

- 声明类所在的包
- 声明需要用到的包中的类

**继承：** 继承这个机制想Object类就懂了   主要两个好处：复用代码，实现**多态**

- 子类继承父类，子类就有父类的公共属性和操作，子类可以实现自己特有的东西或者重写父类的东西。
- 好处是可以复用代码，少写公共代码。
- 多态就是子类对象赋值给父类引用变量，动态绑定就是看起来是父类的引用，但实际执行的是子类方法。
- 计算机程序思维：操作对象的程序不需要知道实际类型，就可以处理不同类型的特有行为。
  - 父类先初始化
  - super可以调用父类的非私有属性和方法，包含构造函数

继承细节：

- 构造方法：父类没有默认构造方法，子类就必须在构造方法中super父类有参构造方法

- 父类和子类的static变量、static方法重名，会触发静态绑定来选择执行父类还是子类的方法

  > 静态绑定就是关于继承中的static变量方法，动态绑定就是关于非static变量和方法
  >
  > 实例变量、静态变量、静态方法、private方法，都是静态绑定的。所以只有实例方法是动态绑定的

- 重载和重写都会尽量按照参数个数和类型的匹配程度来动态绑定地选择执行

- 父子类型转换，向下转型，意思就是用instanceof判断是不是子类对象赋值给父类引用的情况，是就能转

- 子类重写父类里面的东西时不能降低修饰符的可见性，不能public改为private

继承本质：

- 有了继承这条关系链后，加载子类前会把所有父类加载进来，动态绑定时会先从实际对象(子类对象)查找

继承注意事项：

- 子类扩展父类，得先明白父类的实现，避免影响子类预期的功能；然后父类的修改还得看着点子类
- 父类的某种操作实际不属于子类，比如鸟会飞，但是企鹅就不会，这样就有点尴尬
- 多态中：通过父类引用操作子类对象的程序而言，它是把对象当作父类对象来看待的，期望对象原则上属于父类，如果子类和父类三竿子关系打不着，就可能不是亲生的呗，更尴尬了
- 少用继承，如果要用就多关注公共部分，不是公共的加final
- 多用组合，但是用组合子类对象不能当做父类对象来处理了(动态绑定)。但是这时接口就出现了！

**接口：** 接口 - 抽象类 - **类** - 内部类 - 枚举

本质：

- 只关心能力，不关心类型；类实现接口说明了类有这些能力

- 与类一样，接口也可以使用instanceof关键字，用来判断一个对象是否实现了某接口

- 可以和组合一起使用，代替继承来复用代码了。**统一处理**不同子类对象        

  接口实现复用代码，实现多态的动态绑定

- 就是实现**动态绑定**的功能，动态绑定大多指的是实例方法

- Java8之后引入默认方法主要是函数式数据处理的需求，是为了方便给接口增加新方法

  函数式处理有时候需要给接口增加方法，所以后面jdk才有了默认方法，实现类也不需要实现它

抽象类：

- 就是规定继承这个抽象类的类必须实现这些抽象方法，就像说一句抽象的话，里面的道理可以适用在很多地方
- 抽象类和接口还有继承都一样可以声明抽象类的变量引用抽象类具体子类的对象。
- 抽象类有继承的部分优点(抽象类提供默认实现，实现类根据需要重写方法)，也有接口的特点(不能创建对象，和强制实现抽象方法）

内部类：对外部进行隐藏，但实际编译后是单独的class文件

- 静态内部类    用static修饰的在类内部的类，可以访问外部类的静态成员变量方法，和kotlin中object相似
- 成员内部类    没有static修饰的内部类，使用前得先创建外部类实例再创建内部类实例，可以访问外部类所以变量方法，内部类中不能定义静态变量和方法
- 方法内部类     在一个方法中定义使用的类，静态方法内部类可以访问方法外外部类的静态变量方法，实例方法只能访问外部静态变量方法，还可以访问方法的参数值
- 匿名内部类    就像直接new Thread() {自定义run方法}这么写，在创建对象的时候重新定义类，在只使用一次，并且想重定义内部方法时的场景使用

枚举：

- 取值有限，可以枚举出来
- 可以叫做枚举类，里面可以定义操作枚举值的方法，每个枚举值内部隐藏参数，并且每个枚举值可以自定义显式参数

**异常：**

异常其实就是个中断机制，和return相似的退出机制，当程序或者系统发生问题时的中断机制，主要看谁去处理，没有程序处理就中断程序打印异常栈信息

```java
父类：Throwable
    子类：Error系统资源错误          Exception应用程序错误
Exception的子类有个叫RuntimeException，这是运行时异常，只有运行时才能出现，也叫非检查异常，还有Error也是运行时异常
然后Exception除了子类RuntimeException和其本身都是检查异常，也叫非运行时异常。
未受检异常相当于程序错误，受检异常相当于系统、网络错误等
```

异常处理：

- catch可以嵌套多层
- 可以在catch中处理完后再抛出给下面的catch处理，可能当前处理不了，交给下面
- 如果发生异常后想打印自定义信息在控制台，就try捕获异常，然后catch里面处理时输出自定义信息，这样程序就不会中断，但是try里面发生异常点后面程序不会执行
- finally只需要记住里面有return会覆盖try和catch里面的return，如果没有就在try或catch中return后再执行finally里面的处理，反正不管其他，这里其实就是无论别的地方做啥，都会走这
- throws 对于受检异常，在方法上使用，不可以抛出而不声明，但可以声明抛出但实际不抛出；对应未受检异常不要求写这个。

**常用类使用：**

**泛型：** 这个东西吧，就是在接口、类、方法上使用，规定他们使用时的类型；可以复用代码，提高代码的安全性

注意它内部是通过类型擦除实现的，就是在使用类或方法时在<>中规定类型，在程序编译后是不存在的，被替换为Object，之后再通过强制类型转换为自己的规定类型

- 保证类型安全，在可以说是受检安全
- 在类、接口、方法的使用上可以添加不同的类型，从而复用类、接口、方法
- <>中的通配符不能是基本数据类型

**容器：** 

**文件：** 

**并发：** [入口](https:// github.com/PlusFlyCat/HomePage/blob/main/docs/High-Concurrency.md)

**反射：** 反射有句经典的说法是上帝给Java关了一扇门，但是又悄悄给它开了一扇窗，这个窗就是说反射

Java类本来是一个封装好一个房间，但是可以利用反射的方式知道房间里面的一切或创建这个房间；就是说可以用反射入口类Class来得到类中所有信息，比如类名、成员变量、方法、构造函数等，还可以修改成员变量、调方法、调构造函数来创建对象

得到类Class有很多种方式：而且基本类型、数组、枚举都有Class对象

```java
类名.class    比如  Integer.class
对象.class    比如  person.class 
对象.Object中的getClass方法   比如person.getClass
Class.forName("java.lang.Thread")
    
// 通过反射创建类：
// 通过Class对象的newInstance()方法来创建类的实例
Class<?> clazz = Class.forName("com.example.MyClass");
Object obj = clazz.newInstance();
// 通过Constructor类的newInstance()方法来创建类的实例
Class<?> clazz = Class.forName("com.example.MyClass");
Constructor<?> constructor = clazz.getConstructor();
Object obj = constructor.newInstance();
// 通过Method类的invoke()方法来调用类的方法
Class<?> clazz = Class.forName("com.example.MyClass");
Object obj = clazz.newInstance();
Method method = clazz.getMethod("myMethod", String.class);
Object result = method.invoke(obj, "Hello World!");
```

用上面方式得到Class对象后就直接点出，就有很多可以用的方法

反射的好处：

- 动态性：使得程序在运行时可以动态地获取类的信息，调用对象的方法，创建对象实例等
- 可扩展性：反射使得Java程序可以扩展并且具有更高的灵活性，程序可以在运行时根据不同的情况加载不同的类，调用不同的方法，实现不同的功能
- 框架支持：Java反射被广泛地应用于各种框架中，如Spring、JUnit等，通过反射机制，这些框架可以自动地加载类、实例化对象、调用方法等
- 调试支持：Java反射提供了调试程序的能力，可以在运行时获取对象的状态、属性、方法等信息，帮助开发人员更好地理解程序运行过程中的问题
- 代码灵活性：Java反射可以让代码更加灵活，可以动态地创建对象、调用方法、获取属性等，可以避免在编写代码时出现一些硬编码的限制

> 反射是在运行时才知道类型，所以还是容易出现问题的
>
> 动态代理里面就是用反射实现
>
> 好多人说用反射实现序列化和反序列化，这性能不好吧，可以用Protobuf工具，那个比xml体积还小
>
> 注意有的方法会随着jdk或者SDK的更新取消 比如最近SDK里面的newInstance方法

有个问题，为啥创建对象不直接new呢，还搞这么麻烦，是有什么好处吗？

答：其实是为了避免硬编码，就是程序在创建时类加载器会把所有class字节码放到内存，如果new了就会创建一个对象在内存中；这个过程中有个缺点就是程序开始就会大量加载class文件new对象操作，这太硬了，所以才有了反射这个动态加载方式，用到的时候再加载创建

**注解：**

**动态代理：** 并发里面的代理模式，就那个用法

**正则表达式：** 其实就是个字符表示机制，用来匹配一类字符的表达方式，每天都在用，比如linux中的 rm hh*.txt

用正则表达式可以查找、替换、验证、切分文本中的字符或字符串，它的组成是普通字符和元字符，元字符就是有自身的语法含义；

**函数式编程：** 就是一种紧凑的代码方式，就没必要写一些没用的代码

Lambda表达式是Java 8中引入的一种新的语法结构，它允许以更简洁的方式定义函数式接口（Functional Interface）的实现。在Java中，函数式接口是指只有一个抽象方法的接口，Lambda表达式可以直接表示该抽象方法的实现，从而避免了写大量的匿名类。

```java
(参数列表) -> 方法体
(parameters) -> expression  或者  (parameters) -> { statements; }
// 使用
(int a, int b) -> a + b
    
// 函数式接口实现
interface MyInterface {
    void doSomething();
}

MyInterface myInterface = () -> System.out.println("Do something!");
myInterface.doSomething();
```

Java中的一些Function、Predicate、Consumer接口就可以用Lambda来表示使用：

```java
// 将一个字符串转换为大写 
Function<String, String> toUpperCase = (String str) -> str.toUpperCase();
// 方法引用：Function<String, String> toUpperCase = String::toUpperCase;
System.out.println(toUpperCase.apply("lambda"));// 输出LAMBDA

// Lambda表达式作为方法参数传递
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
names.forEach(name -> System.out.println(name));

// 高级点的用法
// 使用andThen和compose方法来组合Lambda表达式
Function<String, String> toUpperCase = String::toUpperCase;
Function<String, String> addExclamation = str -> str + "!";
Function<String, String> composed = toUpperCase.andThen(addExclamation);
String result = composed.apply("hello"); //  输出 "HELLO!"

// 用java中的柯里化函数库Vavr和Guava来实现将多个参数的函数转换为一系列单参数函数
Function<Integer, Function<Integer, Integer>> add = a -> b -> a + b;
int result = add.apply(2).apply(3); //  输出 5

// 集合中的数据并行处理，提高了代码执行效率
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum = numbers.parallelStream().mapToInt(Integer::intValue).sum();// sum = 15
```

## Kotlin

> 可玩性比java丰富，代码更少、可读性强、适配Java与Java库、代码安全、智能检测

​     

**空值与空检测：** kotlin中没有基本数据类型，用的都是包装类型，当编译器检测返回值可能是null，就必须在类型后面加？

```kotlin
fun parseInt(str: String): Int? {
    return str.toIntOrNull() // 可能为null
}
```

**字符串操作：**

```kotlin
fun main() {
    val str = "Hello World!"
    for (c in str) {
        print(" $c")    //  H e l l o   W o r l d !
    }
    val upStr = str.uppercase()
    println(str)	// Hello World! 和Java一样，String是不可变的

    println("Keary $upStr")		// Keary HELLO WORLD! 字符串模板
}
```

**kotlin创建数组方式：**

```kotlin
    // 包装类型
    val array = Array(3) { i -> (i * i) } // [0,1,4]
    val arrayOf = arrayOf(1, 2, 3) // [1,2,3]
    val arrayOf1 = arrayOf("aa", "bb", "cc") // ["aa", "bb", "cc"]
    val arrayOfNulls = arrayOfNulls<String>(3) // [null,null,null]
    // 原生类型
    val intArray = IntArray(5) // [0,0,0,0,0]
    val intArray1 = IntArray(5) { 22 } // [22,22,22,22,22]
    val intArray2 = IntArray(5) { it + 2 } // [2,3,4,5,6]
```

**扩展属性/扩展函数/属性声明新方式：**

```kotlin
class MyClass

// 为MyClass类添加扩展属性  对属性的读取和写入进行自定义操作
var MyClass.stringRepresentation: String
    get() = this.toString()
    set(value) {
        println(value.length)
    }

// 扩展函数
fun MyClass.getName() = "Kotlin"

fun main() {
    val obj = MyClass()

    // 获取属性值
    val str = obj.stringRepresentation
    println(str) // 输出：MyClass@6ff3c5b5

    // 设置属性值
    obj.stringRepresentation = "Hello"
    // 输出：5
    
    println(obj.getName())
    // 输出Kotlin
}
```

**幕后字段/幕后属性：**在大多数情况下，直接使用公共属性（不带幕后字段）即可满足需求。只有在需要添加自定义逻辑或控制属性访问的特殊情况下，才会选择使用幕后字段和幕后属性

```kotlin
// Kitlin会自定为属性生成幕后字段    field表示name的幕后字段
class Person {
    var name: String = ""
        get() = field
        set(value) {
            field = value
        }
}
// 通过私有的_name字段来显式定义幕后字段，然后在name属性的访问器中使用该字段
class Person {
    private var _name: String = ""
    var name: String
        get() = _name
        set(value) {
            if (value >= 0) {
                _name = value
            }
        }
}
// 场景一：通过幕后字段，在属性访问器中添加自定义逻辑(对写入进行验证、转换)
// 场景二：封装属性的内部实现细节：幕后字段允许你将属性的实际存储与外部访问分离
// 场景三：属性计算：属性的值可能需要通过计算得出，而不是简单地存储在幕后字段中
```

**延迟初始化属性与变量：**通过lateinit关键字，可以延迟初始化属性，允许将非空类型的属性推迟到后面的代码中进行赋值

```kotlin
// 场景一：当变量的计算代价较高时，你可以使用延迟初始化来推迟计算，直到变量实际被使用。这可以在节省资源的同时提高性能
val expensiveData: List<String> by lazy {
    // 执行昂贵的操作来计算数据
    fetchDataFromNetwork()
}
// 场景二：在初始化对象的过程中，如果某些属性或变量依赖于其他对象
class MyClass {
    val dependency: MyDependency by lazy {
        MyDependency()
    }
}

val obj = MyClass()
// 在此之前，MyDependency不会被初始化
val result = obj.dependency.someFunction()
```

**数据类:** 自动生成equals()、hashCode()和toString()等函数

```kotlin
data class Person(val name: String, val age: Int)

fun main() {
    // 提供了一个copy()方法复制原始对象的同时修改属性值
    val person = Person("Bob", 30).copy(age = 18)
    println(person.toString()) // Person(name=Bob, age=18)
 
    // 解构声明
    val (name, age) = person
    println("name:$name, $age years of age") //name:Bob, 18 years of age
}
```

**密封类:**  受限的类继承结构，描述一组相关的类， 多用于when判断多个实例

```kotlin
sealed class Result {
    class Success(val date: Int) : Result()
    object Error : Result()
}

fun processResult(result: Result) {
    // 编译器可以确保已经处理了所有可能的情况，而不需要添加else分支
    when (result) {
        is Result.Success -> println("Result: ${result.date}")
        is Result.Error -> println("Error")
    }
}

fun main() {
    processResult(Result.Success(23)) // Result: 23
}

```

**内联类：** 有类的感觉，不会有对象的堆内存分配，运行时将内联类替换为包装类型，性能和内存效率方面具有优势

```kotlin
// UserId是一个内联类，包装了Long类型的值，对单个值进行封装和处理
inline class UserId(val value: Long)

fun getUserId(userId: UserId): Long {
    return userId.value
}

val id: UserId = UserId(123)
val extractedId: Long = getUserId(id)
```

**对象表达式：**使用对象表达式，在不创建具体类的情况下，直接创建并使用这个对象

```kotlin
open class Animal {
    open fun sound() {
        println("Animal makes a sound")
    }
}

fun main() {
    val cat = object : Animal() {
        override fun sound() {
            println("Meow")
        }
    }
    
    cat.sound() // 输出：Meow
}

// 在函数中创建临时的、局部作用域内的对象，以便在特定上下文中使用
fun someFunction() {
    val data = "Some data"

    val logger = object {
        fun log() {
            println("Logging: $data")
        }
    }
    
    logger.log() // 输出：Logging: Some data
}
```

**伴生对象： ** 在java中静态方法定义很简单，直接在方法添加static修饰符即可；但是在Kotlin中有两种方式：它这么做就是为了方便，比如定义工具类时就不需要写一堆static

- 定义单例类，这样里面所以方法都可以直接用调用静态方法的样子调用

  ```kotlin
  object MySingleton {
      fun getInstance() {
          println("Instance")
      }
  }
  
  fun main() {
      MySingleton.getInstance() // Instance
  } object MySingleton {    fun getInstance() {        println("Instance")    }}fun main() {    MySingleton.getInstance() // Instance}object Util {    fun doAction() {        println("xxx")    }}
  ```
  
- 定义伴生对象，这样就能在普通类中定义类似静态方法

  ```kotlin
  class Util {
  	fun doAction1() {
  		println("xxxx")
  	}
  	companion object {
          // @JvmStatic    添加这个后这个方法才是这个类的真正静态方法或字段，不添加也能用
  		fun doAction2() {
  			println("xxxx")
  		}
  	}
  }
  ```

**高阶函数与lambda表达式：**像操作变量一样操作函数，将函数作为参数或返回值；提供了更灵活的**函数组合**和**代码复用**方式

```kotlin
// 场景一：函数参数化，通用的map高阶函数，它接受一个转换函数作为参数，用于将集合中的每个元素进行转换
fun <T, R> List<T>.map(transform: (T) -> R): List<R> {
    val result = mutableListOf<R>()
    for (item in this) {
        result.add(transform(item))
    }
    return result
}

val numbers = listOf(1, 2, 3, 4, 5)
val squaredNumbers = numbers.map { it * it }  // 在调用函数操作的同时再执行第二种自定义操作

// 场景二：多个函数组合起来，形成新的函数。这样可以使代码更简洁、更可读，使用compose高阶函数将两个函数组合成一个新的函数
fun <T, R, V> compose(f: (R) -> V, g: (T) -> R): (T) -> V {
    return { x -> f(g(x)) }
}

val addOne: (Int) -> Int = { it + 1 }
val multiplyByTwo: (Int) -> Int = { it * 2 }
val addOneAndMultiplyByTwo = compose(multiplyByTwo, addOne)

val result = addOneAndMultiplyByTwo(3) // 输出：8

// 根据不同的条件返回不同的函数。这样可以根据需要动态生成函数,编写一个工厂函数，根据传入的参数返回一个特定的计算函数
fun createCalculator(operation: String): (Int, Int) -> Int {
    return when (operation) {
        "add" -> { a, b -> a + b }
        "subtract" -> { a, b -> a - b }
        "multiply" -> { a, b -> a * b }
        else -> throw IllegalArgumentException("Invalid operation")
    }
}

val addFunction = createCalculator("add")
val result = addFunction(2, 3) // 输出：5
```

**结构声明：** 将一个复杂的对象或数据结构解构成多个单独的变量

```kotlin
// 1.解构数据类
data class Person(val name: String, val age: Int)

val person = Person("John Doe", 30)
val (name, age) = person

println(name) // 输出："John Doe"
println(age)  // 输出：30

// 2.解构函数返回值：当函数返回多个值时，可以使用解构声明将这些值解构到多个变量中。这样可以方便地访问函数的返回结果
fun getUser(): Pair<String, Int> {
    return Pair("John Doe", 30)
}

val (name, age) = getUser()

println(name) // 输出："John Doe"
println(age)  // 输出：30

// 3.解构集合元素：遍历一个包含多个元素的集合时，可以使用解构声明将集合元素解构为单独的变量，以便更方便地访问元素的属性
val map = mapOf("key1" to "value1", "key2" to "value2")

for ((key, value) in map) {
    println("Key: $key, Value: $value")
}

// 4.使用Lambda表达式时，可以使用解构声明将Lambda的参数解构为单独的变量，以便更方便地访问参数
val list = listOf("Apple", "Banana", "Orange")

list.forEachIndexed { index, element ->
    println("Index: $index, Element: $element")
}

// 5.解构声明与when表达式结合使用：当使用when表达式时，可以使用解构声明将不同情况下的匹配结果解构到单独的变量中，
//   以便在每个分支中方便地访问和处理数据
fun processResponse(response: ApiResponse) {
    when (response) {
        is ApiResponse.Success -> {
            val (data, statusCode) = response
            println("Received data: $data, Status code: $statusCode")
        }
        is ApiResponse.Error -> {
            val (errorMessage, statusCode) = response
            println("Error message: $errorMessage, Status code: $statusCode")
        }
    }
}

// 6.自定义类中实现解构声明，你可以通过在类中定义component1()、component2()等方法来指定如何解构对象
class Point(val x: Int, val y: Int) {
    operator fun component1(): Int = x   // Kotlin要求每个类最多只能有五个componentN()方法，分别对应不同的属性
    operator fun component2(): Int = y   // 使用数据类好点，它会自定生成解构声明的componentN()方法
}

fun main() {
    val point = Point(3, 5)
    val (x, y) = point

    println("x: $x, y: $y")
}

```

**标准函数：**

- with   将**对象**作为**参数**并对其进行**lambda操作**，**最后一行代码**作为**返回值**返回

  ```kotlin
  val result = with(obj) {
  //  这里是obj的上下文
  "value" //  with函数的返回值
  }
  // 技巧1：在with大括号中顺序执行obj的方法
  // 
  ```

- run    和with差不多，但这个是**直接对对象**进行**lambda操作**，并将最后一行代码作为**返回值**返回

  ```kotlin
  val result = obj.run {
  //  这里是obj的上下文
  "value" //  run函数的返回值
  }
  ```

- apply  在**对象的基础**上对其操作，操作完后**返回此对象**

  ```kotlin
  val result = obj.apply {
  //  这里是obj的上下文
  }
  //  技巧1：大括号中对obj对象中某些属性进行赋值，最后一个括号后还可以.also{}接着处理这个对象
  ```

- let  在**对象的基础**上对其操作，并把**对象传给lambda**操作，最后返回此对象

  ```kotlin
  obj.let { obj -> // 常
      obj.xxxx
  }
  
  obj？.let { 
      // 如果obj非空就执行
  }
  ```

- alse  前面执行完后再顺带执行这个

  ```kotlin
  // 交换两个变量
  var a = 1
  var b = 2
  a = b.alse { b = a }
  ```

集合：

协程：

## Python



## Linux

















