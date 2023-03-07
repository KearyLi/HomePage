# Android

> 一个有趣的操作系统

activity 就是掌握生命周期

fragment 和activity生命周期的区别和联系，怎么添加fragment，fragment和activity通信

service 也是在生命周期是做事情，activity绑定service进行通信，IntentService异步可自动退出的服务

broadcastRecevier







## AsyncTask 异步

这个早被弃用了，感觉这个东西太简单，方便，所以会有一些漏洞。现在多用java的并发包工具和Handler



## Android Handler 机制

首先得了解异步消息处理机制：由Message、MessageQueue、Looper、Handler四部分组成；执行流程就是创建一个message对象，Handler将这个message放到MessageQueue中，然后Looper发现消息队列中有数据就将数据取出来交给Handler的handleMessage方法处理

```java
override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding.fab.setOnClickListener {
            thread {
                val msg = Message()
                msg.what = 1//msg可以携带多种类型数据
                handler.sendMessage(msg)//开启线程将消息发送出去
            }
        }
}
private val handler = object : Handler(Looper.getMainLooper()) {//getMainLooper说明在主线程运行
        override fun handleMessage(msg: Message) {//主线程接受消息处理消息
            //super.handleMessage(msg)
            when(msg.what) {
                1 -> print("消息来了...")
            }
        }
}
```

由上面就知道Handler用于进程中线程间的通信，而Android中进程间通信用的是Binder

- Message 

先懂怎么用，有时间再深入，东西太多了





## View事件分发机制



















LiveData

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

