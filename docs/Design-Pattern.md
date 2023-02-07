# 设计模式
> 抓住本质，理解思想，举一反三
>
> 结合经验与书籍总结《设计模式》《秒懂设计模式》《大话设计模式》

设计模式思考<br>问：感觉有的设计模式差不多啊？<br/>答：不对<br>问：



## 1. 概念

一套代码设计、代码体系结构的经验总结，和只会调用api工具箱的工程师不同，这个主要是面向框架，让代码编制更加工程化，让项目更加有可靠性、可扩展性、可重用性、可维护性、可移植性等特点，设计模式是用面向接口编程来降低耦合的。后面就会发现会用到很多接口来参与设计。

软件产品的不断迭代更新，设计模式根据各种场景提供了最优的设计方式，让每个模块最大限度得到复用及扩展。设计模式其实不是什么技术，而是软件工程一种思想，一种格局。

面向对象：实际存在的现实实物就是对象，计算机中由类(模型)来创建对象，发现造物的过程中模型之间有千丝万缕的关系，就像人与房屋有关系(对象就是小明住在202)，所以在代码设计中就得考虑用面向对象的三大特性(封装、继承、多态)来处理这千丝万缕的关系。 

- 封装就是将变量和方法放在一个类中，然后给这个类提供接口访问类中的东西，这样的类就干净整洁，但是不能把不相关的东西封装在一起，所以封装也得适度；

- 继承就是避免出现重复代码，子类有父类的东西，子类可以重写父类的东西，子类可以扩展父类的东西；

- 多态就是父类的引用可以指向本类对象或者子类的对象。面向接口编程的重要性在这可以体现了。

## 2. 背景

设计模式起源于建筑设计大师看到当时没有一个好的建筑结构规范，它想要改善人类的生活住所条件。后面才有了设计模式(可复用面向对象软件)，设计模式也是从C++模仿过来的。

设计模式格式：当设计出一个设计模式后，你想要全国的人知道，然后你讲解，你就带按照这个格式描述你的设计模式，这就是GoF格式。

- 模式名：有意义、简短而准确地描述设计模式，让人们交流起来方便，可以直接体现内涵的名字；
- 问题：该设计模式的出现，是为了解决什么问题下使用，和使用了这个设计模式会有怎样的结果；
- 环境：这个设计模式在什么环境下使用，这个设计模式的使用范围；
- 解决方案：这个设计的组成，之间是怎样协作的，每个成分的职责是什么，用一个抽象的描述来处理不同实际问题；
- 效果：用这个设计模式处理这个问题的结果怎样，有哪些弊端，有哪些时间空间上的优势；
- 例子：使用一个代表性的，能让别人理解设计模式的代码示例；
- 末态环节、推理、其他有关模式、已知应用。

## 3. 分类

> 23种设计模式，三大类型：创建型，结构型，行为型

- 创建型：单例模式、工厂方法模式、抽象工厂模式、建造者模式、原型模式；
- 结构型：代理模式、装饰模式、适配器模式、组合模式、桥梁模式、外观模式、享元模式；
- 行为型：模板方法模式、命令模式、责任链模式、策略模式、迭代器模式、中介者模式、观察者模式、备忘录模式、访问者模式、状态模式、解释器模式。

## 4. 设计原则

- 单一职责原则：就是封装得封装得好，一个类应该只有一个职责，为了模块解耦、可读性、可维护性、复用性、降低变更引起的风险

  应用：Javaweb中的分层思想，dao层、service层、controller层

- 里氏替换原则：这里就是继承和多态用得好，父类引用可以指向本类对象或子类对象

- 依赖倒置原则：就是面向接口编程用得好，底层模块依赖高层模块，高层模块不依赖底层模块，实现类(子类)依赖接口或者抽象类，依赖关系是由接口或抽象类来实现

  应用：在中大型项目中常用，用于扩展和维护，是实现开闭原则的方式之一

- 接口隔离原则：使用接口编程是一个很好的习惯，但是设计时得考虑接口粒度大小，粒度太小会导致接口数量剧增，太大会导致灵活性降低，所以设计时考虑这个接口中方法是否与模块子类匹配。

- 迪米特原则：一个对象应当对其他对象尽可能少地了解。就是A类和B类有联系，B类和C类有联系，这样如果A类想和C类交流，就直接和B类交流就行，没必要再去找C类。

- 开闭原则：对扩展开放，对修改关闭，尽可能少地修改源代码。上面的原则其实都是为了开闭原则服务的，都是工具。

![](../IMG/openOff.png)

```java
public interface IBook {
    public String getName();
    public double getPrice();
    public String getAuthor();
}
//----------------
public class NoveIBook implements IBook {
    //书名
    private String name;
    //价格
    private double price;
    //author
    private String author;
    public NoveIBook() {
    }
    public NoveIBook(String name, int price, String author) {
        this.name = name;
        this.price = price;
        this.author = author;
    }
    @Override
    public String getName() {
        return name;
    }
    @Override
    public double getPrice() {
        return price;
    }
    @Override
    public String getAuthor() {
        return author;
    }
}
//----------------
public class BookStore {
    private ArrayList<IBook> bookList = new ArrayList<>();
    public BookStore() {
        bookList.add(new OffNoveIBook("西游记",23,"吴承恩"));    //修改时就改这里OffNoveIBook
        bookList.add(new OffNoveIBook("红楼梦",32,"曹雪芹"));    //改成NoveIBook
    }
    public void showBookList() {
        System.out.println("----     书店列表     ----");
        System.out.println("书名\t价格\t作者");
        for (IBook iBook : bookList) {
            System.out.println(iBook.getName() + "\t" + iBook.getPrice() + "\t" + iBook.getAuthor());
        }
    }
    public static void main(String[] args) {
        BookStore bookStore = new BookStore();
        bookStore.showBookList();
    }
}
//----------------增加活动打折方案
public class OffNoveIBook extends NoveIBook {
    public OffNoveIBook(String name, int price, String author) {
        super(name, price, author);
    }
    @Override
    public double getPrice() {
        return super.getPrice()*0.8;
    }
}
```

## 5. 创建型模式（5种）

> 就是创建管理实例的不同方式和操作

### 5.1 单例模式

> 懒汉式、饿汉式，构造方法私有，饿汉式加synchronized

![Singleton](../IMG/Singleton.png)

```java
//饿汉式
public class Singleton {
    //太饿了，先实例化
    private static Singleton singleton_instance = new Singleton();

    //构造函数私有
    private Singleton() {
    }

    public static Singleton getInstance() {
        return singleton_instance;
    }
}
//----------------
//懒汉式
public class Singleton {
    private static Singleton singleton_instance = null;

    //构造函数私有，不让外面创建实例
    private Singleton() {
    }

    //方法同步
    synchronized public static Singleton getInstance() {
        if (singleton_instance == null) {
            singleton_instance = new Singleton();
        }
        return singleton_instance;
    }
}

//优点：减少内存和系统开销，创建只创建一次，在内存中也只有一份。
//缺点：构造函数都被私有了,无法拓展子类。
//注意：使用单例得注意有状态单例和无状态单例，有状态单例会在分布式系统中存在多份实例，还有序列化的问题。
//----------------
//懒加载更有运行效率的方式，加“双检锁”
public class Singleton {
    private static volatile Singleton singleton_instance = null;

    private Singleton() {
    }

    public Singleton getInstance() {
        if (singleton_instance == null) {
            synchronized (Singleton.class) {//在这加锁就是为了运行效率，如果一个线程创建了实例就不用走这了，不用让线程等待，直接用之前线程创建的实例
                if (singleton_instance == null) {
                    singleton_instance = new Singleton();
                }
            }
        }
        return singleton_instance;
    }
}
```



### 5.2 工厂方法模式

> 将产品对象的创建放到子类工厂中进行控制，目的就是隐藏产品创建对象的过程

![](../IMG/Factory.png)

```java
public interface Factory {
    public Product create();
}
//----------------
public interface Product {
    public void grow();

    public void harvest();
}
//----------------
public class ConcreteFactory implements Factory {
    //工厂生产产品
    public Product create() {
        return new ConcreteProduct();
    }
}
//----------------
public class ConcreteProduct implements Product {
    public void grow() {
        //增长
    }

    public void harvest() {
        //行为
    }
}
//----------------
public class ClientDemo {
    public static void main(String[] args) {
        ConcreteFactory concreteFactory = new ConcreteFactory();
        Product product1 = concreteFactory.create();
        product1.grow();
    }
}
//工厂方法模式就是在别的地方封装一下new对象的过程，现在将实例化封装在工厂中，也是为了系统的可扩展性和耦合性
//优点：封装，可扩展
```



### 5.3 抽象工厂模式

> 和工厂模式不同的是抽象工厂模式有多个产品，处理时是对产品组进行对象创建，处理

![](/home/hang/code/github_file/HomePage/IMG/AbstractFactory.png)

```java
public interface Factory {
    public ProductA createA();
    public ProductB createB();
}
public interface ProductA {
    public void grow();
    public void harvest();
}
public class ConcreteFactory implements Factory {
    //工厂生产产品
    public ProductA createA() {
        return new ProductA1();
    }
    public ProductB createB() {
        return new ProductB1();
    }
}
public class ProductA1 implements ProductA {
    public void grow() {
        //增长
    }
    public void harvest() {
        //行为
    }
}
public class ProductB1 implements ProductB {
    public void add() {
        //增长
    }
    public void deed() {
        //行为
    }
}
public class ClientDemo {
    public static void main(String[] args) {
        ConcreteFactory concreteFactory = new ConcreteFactory();
        Product productA1 = concreteFactory.createA();
        Product productB1 = concreteFactory.createB();
        productA1.grow();
        productB1.add();
    }
}
//优点：在工厂方法模式基础上可以对一系列有约束的产品种类进行封装创建对象
//缺点：产品种类增加，得手动修改工厂接口
```

### 5.4 建造者模式

> 产品本身只有属性，产品的操作放在建造者那处理，对了，建造者由导演控制那些操作











