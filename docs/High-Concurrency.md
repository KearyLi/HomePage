# 并发

## 1. 线程

> 线程的创建 - 线程出现的问题 - 用竞争机制解决 - 又产生新问题 - 注意避免

```java
 //两种创建线程的方式
 //1.继承Thread
 public class HelloThread extends Thread {
     @Override
     public void run() {
         System.out.println("hello");
     }
 }
     public static void main(String[] args) {
         Thread thread = new HelloThread();
         thread.start();//调用start后让操作系统创建线程分配相关资源以及调度
     }
 }
 
 //2.实现接口 ，为什么要有这个，就因为单继承的原因，可以说是单继承缺点的解决方式
 public class HelloRunnable implements Runnable {
     @Override
     public void run() {
         System.out.println("hello");
     }
     public static void main(String[] args) {
         Thread helloThread = new Thread(new HelloRunnable());
         helloThread.start();
     }
 }
 
 //线程有一些基本属性和方法，包括id、name、优先级、状态、是否daemo线程、sleep方法、yield方法、join方法、过时方法等，其实就是Thread类里面的方法，jdk文档很清楚
 //使用这些方法的技巧就是知道这个方法的作用，以及这个方法是在Object里面，还是Thread的静态方法，还是Thread的实例方法，知道这些就可以灵活调用了
 
```

线程的出现产生的问题

线程虽然提高了系统运行效率，但是在处理上面还是有缺点，避免了这两个缺点后线程就完美了

共享内存就是多个线程能访问同一个对象的属性和方法

缺点1      竞态条件：就是1000个线程用一个方法对一个常量加1，结果小于1000

​                  解决办法：synchronized、显示锁、原子变量

缺点2      内存可见性：一个线程对内存的修改另一个线程看不到，就是线程获取或操作完的值是对CPU中缓存中更新的，值没有实际在内存中更新，这时结果就不对

​                  解决办法：volatile关键字、synchronized关键字、显式锁同步

优点就是能充分利用CPU内存磁盘网络资源，在某些业务上使用线程也能优化用户体验，提高应用细节功能

缺点除了上面两个还有就是线程创建，调度，上下文切换都消耗系统资源；操作系统会为线程创建栈和程序计数器，调用和上下问切换会不断恢复删除CPU寄存器中的值，很可能导致缓存值失效。  为甚后面的缺点不放在上面列出，因为这是硬件层面的问题，解决不了，只能预防

这里考虑创建线程数量时得考虑时CPU密集型还是啥，因为上个项目里面就只有一个线程处理数据

## 2. 线程的竞争机制

synchronized修饰实例方法：一个类中实例方法用这个后，创建的一个对象里面只要有这个关键字的方法就不能同时执行，但是创建两个对象，两个对象就可以同时执行，两个对象是分开的。 关键字保护的是对象，让每个对象都有一个锁和等待队列。synchronized可以同步任何对象，任何对象都有锁和等待队列。

注：如果这个对象中有一个方法没用这个关键字，则另一个线程可以同步使用这个方法，所以在保护变量的时候，需要在所以访问该变量的方法上加synchronized

总结就是synchronized保护实例对象里面有这个关键字的方法或代码块

synchronized修饰静态方法保护的是类对象。就是保护那一份，因为它是静态方法，只有一份。

synchronized修饰代码块其实就是上面两个的意思，一个用synchronized(this)，一个用synchronized(Xxx.class)，也可以随便用对象，synchronized(new Object()),这样就给括号里面的对象加一个锁和等待队列。括号里面就是要保护的对象

```java
 public class Counter {
     private int count;
     public synchronized void incr() {
         count++;
     }
     public synchronized int getCount() {
         return count;
     }
 }
 //上面其实就是下面这个
 public class Counter {
     private int count;
     public void incr() {
         synchronized (this) {
             count++;
         }
     }
     public int getCount() {
         synchronized (this) {
             return count;
         }
     }
 }
 //使用锁对象
 public class Counter {
     private int count;
     private Object lock = new Object();
     public void incr() {
         synchronized (lock) {//去拿Object对象锁
             count++;
         }
     }
     public int getCount() {
         synchronized (lock) {
             return count;
         }
     }
 }
 public class CounterThread extends Thread {//使用
     Counter counter;
     public CounterThread(Counter counter) {
         this.counter = counter;
     }
     @Override
     public void run() {
         for (int i = 0; i < 1000; i++) {
             counter.incr();
         }
     }
     public static void main(String[] args) throws InterruptedException {
         int num = 1000;
         Counter counter = new Counter();
         Thread[] threads = new Thread[num];
         for (int i = 0; i < num; i++) {
             threads[i] = new CounterThread(counter);
             threads[i].start();
         }
         for (int i = 0; i < num; i++) {
             threads[i].join();
         }
         System.out.println(counter.getCount());
     }
 }
```

线程的竞争机制里面的一些问题点

- 可重入性：线程获取对象锁之后可以调用对象中其它被同步的方法
- 内存可见性：synchronized可以实现原子操作，在方法上用关键字，而且还能保证内存可见性，线程获得锁后去内存拿值，释放锁后值写回内存。    用这个实现原子操作成本太高，变量用volatile好，操作系统会给变量加特殊指令

- 死锁：线程相互都持有锁，但还是去获取对方的锁，就会造成死锁

显式锁就是对象的锁带有时间限制，得不到锁就释放，避免死锁		--这个点先放着

- 同步容器：

```java
 - 同步容器：就是实现了collection接口的同步容器类，里面和collection差不多，只是每个方法里面使用了synchronized关键字修饰一个Object对象给容器加锁；但是同步容器会出现问题，比如下面的问题
  
 // 复合操作
     public class EnhancedMap<K, V> {
     Map<K, V> map;
     public EnhancedMap(Map<K, V> map) {
         this.map = Collections.synchronizedMap(map);
     }
     public V putIfAbsent(K key, V value) {
         V old = map.get(key);
         if (old ! = null){
             return old;
         }
         return map.put(key, value);
     }
     public V put(K key, V value) {
         return map.put(key, value);
     }
 //…
 }
 
 //这个类中的map是线程安全的，但是putIfAbsent方法不是；这种一边将map改为同步容器，一边对map操作的复合操作就是有问题的
 
 //伪同步     就是同步错了，就像下面这样，正确的应该同步map对象
 public synchronized V putIfAbsent(K key, V value) {
      V old = map.get(key);
       if (old != null) {
           return old;
        }
       return map.put(key, value);
 }
 //解决伪同步
  public V putIfAbsent(K key, V value) {
         synchronized (map) {
             V old = map.get(key);
             if (old ! = null){
                 return old;
             }
             return map.put(key, value);
         }
     }
 //迭代   就是两个线程对同步容器一个修改，一个遍历；结果就是遍历的会产生异常，因为遍历容器时容器结构发生变化就会抛出异常，解决办法就是在遍历的时候对同步容器加锁
 
 //所以并发容器问题还是挺多的，性能也低，但是Java还有别的性能高的并发容器        有待深入研究
 //CopyOnWriteArrayList
 //ConcurrentHashMap
 //ConcurrentLinkedQueue
 //ConcurrentSkipListSet
```

## 3. 线程的协作机制

> 线程产生的竞态、内存可见性问题用上面竞争机制处理；但是线程也有更厉害的用法

 

 

 

## 4. 线程的中断