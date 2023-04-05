# JepPack

> 开发组件工具集jetpack
>
> 为什么有它？之前Google没弄，社区自己做了一些有用架构，然后Google整理规范到jetpack
>
> 代码质量，可读性，易维护性高  
>
> 更新UI开发方式，规避View面向对象继承体系
>
> 函数式编程，声明式编程
>
> 不依赖android版本可以独立迭代，在androidx库中，向下兼容性强



Composable DSL：

> Composable DSL（领域特定语言）是一种可以通过组合和嵌套来创建更大的语言结构的DSL。它的设计目的是使用户能够通过简单的构建块来创建复杂的语言结构，从而实现更高级别的抽象。例如，一个图形用户界面（GUI）DSL可以使用一些基本构建块（如文本框、按钮和标签）来创建更高级别的GUI元素（如表单和菜单）。
>
> Composable DSL通常采用模块化的设计，其中不同的模块负责不同的任务。这些模块可以被组合在一起来创建更复杂的DSL结构。例如，一个Web应用程序DSL可以由不同的模块负责路由、模板、ORM等不同的任务，这些模块可以被组合在一起来创建完整的Web应用程序DSL。
>
> Composable DSL可以提高代码的可读性和可维护性，因为它们使程序员能够更自然地表达他们的意图。它们还可以提高开发速度，因为它们允许程序员使用预定义的构建块来构建复杂的语言结构，而不需要从头开始编写所有的代码。



## LiveData

> 原理就是设计模式中的观察者模式；将任意一种类型数据用LiveData包装起来，这样当数据变化时观察者就会发现，从而立马做出响应

getValue()、setValue()和postValue()方法。

- getValue()用于获取LiveData中包含的数据
- setValue()用于给LiveData设置数据，但是只能在主线程中调用
- postValue()用于在非主线程中给LiveData设置数据

MutableLiveData：可变的     LiveData是不可变的  他们的区别是什么？ 

用MutableLive封装数据然后私有化后，用LiveData来对开放获取_counter示例，但是不能给这个实例赋值，因为counter是LiveData类型的，里面赋值方法只对子类使用，示例和源码如下：

```kotlin
//示例
val counter: LiveData<Int>
     get() = _counter
private val _counter = MutableLiveData<Int>()

//androidx/lifecycle/LiveData.java源码：
@MainThread
    protected void setValue(T value) {
        assertMainThread("setValue");
        mVersion++;
```

