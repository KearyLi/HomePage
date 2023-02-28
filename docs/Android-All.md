# Android

> 一个有趣的操作系统



## 1. LiveData

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

