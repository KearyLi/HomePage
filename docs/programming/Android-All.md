# Android
- [Android](#android)
  - [Camera2](#camera2)
  - [Activity](#activity)
  - [View原理与自定义](#view原理与自定义)
  - [AsyncTask 异步](#asynctask-异步)
  - [Handler 机制](#handler-机制)
  - [Binder 机制](#binder-机制)
  - [配置测试发布](#配置测试发布)
  - [1. 设置开发环境](#1-设置开发环境)
  - [2. 编写代码](#2-编写代码)
  - [3. 构建运行](#3-构建运行)
- [其它技术](#其它技术)
  - [Protocol Buffers](#protocol-buffers)
  - [Retrofit](#retrofit)
  - [OkHttp](#okhttp)
  - [SQLite](#sqlite)

> activity 就是掌握生命周期
>
> fragment 和activity生命周期的区别和联系，怎么添加fragment，fragment和activity通信
>
> service 也是在生命周期是做事情，activity绑定service进行通信，IntentService异步可自动退出的服务
>
> broadcastRecevier

## Camera2

> Camera2在Android5.0推出的，它出现是为了替换废弃的Android4.4 Camera应用级相机框架

android.info.supportedHardwareLevel：不是每个Android5.0以上的设备支持Camera2API所以功能，所以得用这个来查询设备支持程度：

- INFO_SUPPORTED_HARDWARE_LEVEL_LEGACY 旧版，功能和废弃的CameraAPI差不多
- INFO_SUPPORTED_HARDWARE_LEVEL_LIMITED 有限，设备能做的功能占Camera2大部分，并且得使用HAL3.2以上版本
- INFO_SUPPORTED_HARDWARE_LEVEL_FULL 全部，设备能提供所有Camera2中所有功能，但得使用HAL3.2及Android5.0以上版本
- INFO_SUPPORTED_HARDWARE_LEVEL_3 级别3，设备支持YUV和RAW流处理
- INFO_SUPPORTED_HARDWARE_LEVEL_EXTERNAL 外部相机使用

Camera2主要类：

- CameraManager 相机管理，用来检测摄像头，打开摄像头，获取摄像头支持

  ```java
  //使用之前在<uses-permission android:name="android.permission.CAMERA"/>
  //AvailabilityCallback 相机设备开启后会产生回调
  public static abstract class AvailabilityCallback {
      public void onCameraAvailable(@NonNull String cameraId) {}
      public void onCameraUnavailable(@NonNull String cameraId) {}
  }
  ```

- CameraDevice 物理或逻辑摄像头

  ```java
  //上面的API打开摄像头后提供回调接口，这样执行CameraDevice中的API后会回调到这
  public static abstract class StateCallback {
      // 当 CameraDevice.close() 调用时会触发；如果设备已经关闭，
      // 后续再次调用 CameraDevice 相关方法会抛出异常 IllegalStateException
      public void onClosed (CameraDevice camera){...}
      // 表示设备不能再被使用；再次访问会抛出 CameraAccessException
      public abstract void onDisconnected (CameraDevice camera){...}
      // 开启设备失败；再次访问抛出 CameraAccessException，并给出 error
      public abstract void onError (CameraDevice camera, int error);
      // 设备被正常打开
      public abstract void onOpened (CameraDevice camera);
  }
  ```

- CameraCaptureSession 应用程序和CameraDevice之间的会话通道类，传输从摄像头捕获的数据和从新处理之前同一会话中的数据，数据在surface中渲染输出

  ```java
  //CameraCaptureSession需要异步执行，它会创建会话管道和数据缓冲区，这个过程耗时
  CameraCaptureSession.StateCallback//会话通道建立完成后，通道的状态变化会回调到这里面
  CameraCaptureSession.CaptureCallback//捕获结果回调
  CameraConstrainedHighSpeedCaptureSession//该类的子类，表示高速会话通道，用来传输高帧率数据
  ```

- CameraMetadata 摄像头的控制命令，固定的，给CameraDevice的命令，请求摄像头的参数、捕获结果

  ```java
  //抽象类，键值对的数据结构，固定的，只有一个公共方法来获取键值信息
  public List<TKey> getKeys() {
      Class<CameraMetadata<TKey>> thisClass = 
          (Class<CameraMetadata<TKey>>) getClass();
      return Collections.unmodifiableList(
              getKeys(thisClass, getKeyClass(), this, /*filterTags*/null));
  }
  ```

- CameraRequest 从相机设备获取数据的请求，一些命令

  ```java
  //继承CameraMetadata，意思就是带着命令控制CameraDevice，从中获取数据
  ```

- CameraResult 从相机设备得到的返回结果

  ```java
  //同样继承CameraMetadata
  ```

- CameraCharacteristics 描述CameraDevice相机设备的属性，固定的，每个手机有不同的摄像头属性，用CameraManager.getCameraCharacteristics(String cameraID)来查询

  ```java
  //这个也是固定的数值，
  ```


> 流程就是CameraManager检测并打开CameraDevice，建立应用和相机设备之间的CameraCaptureSession通道传输数据，CameraRequest用CameraMetaData命令发送数据下去，然后返回CameraResult



## Activity

> Activity就是APP与用户交互的入口点，它的作用是实现系统和应用程序之间的交互
>
> 用好生命周期可以优化APP的用户使用，比如切换应用后停止视频播放，减少系统资源消耗，保存进度等等

Activity中使用Intent启动时，有显式Intent和隐式Intent两种

显式Intent：

```kotlin
val intent = Intent(this,SecondActivity::class.java)
startActivity(intent)
```

隐式Intent：这个主要就是在清单文件中添加IntentFilter中的action标签来匹配Intent；注意这个的标签还有data,具体使用时再看

```kotlin
val intent = Intent()
intent.action = "com.hang.kearypro.myaction"
startActivity(intent)
```

生命周期：

- onCreate()  第一次创建activity时调用；初始化工作、加载资源、初始化一些数据
- onStart()  可见，但是在后台，不能和用户交互
- onResume()  位于任务栈顶、可见、在前台、可与用户交互
- onPause()  不在栈顶、但可见；不能做耗时操作，执行完它后新activity才会启动
- onStop()  不可见；不能太耗时
- onRestart()  从onStop()不可见重新onStart()，onResume()到可见；打开app后回到桌面再回到app就走这个
- onDestory()  实例直接销毁；一般在旋转屏幕、内存调度、返回上一个activity使当前activity从任务栈中移除的情况会调用这个

注意：

1. onCreate和onDestory属于完整生存期；onStart和onStop属于可见生存期；onResume和onPause属于前台生存期
2. 启动新activity前会先执行旧activity的onPause才调用新activity的生命周期(sdk31)
3. 如果想要保存状态信息，可以实现onSaveInstanceState回调方法，它会在onStop之前回调

启动模式：

- standard 启动一个就老实创建一个activity实例
- singleTop  启动一个时判断是否和任务栈顶实例相同
- singleTask  启动一个时判断是否和任务栈中实例相同，相同就弹出此activity上其他activity，使之处于栈顶
- singleInstance  另外创建一个任务栈存放activity实例，比如b是这个启动模式，那么a->b->c后BACK就直接回到a，因为b在另外一个任务栈，a和c在一个任务栈，并且a在c下面；但是再BACK一次后会到b

## View原理与自定义

> view在Android中很重要，在实现自定义控件时，会发生滑动冲突(事件处理冲突)问题，解决这个问题就得明白view的事件分发机制、事件处理机制即view的内部原理；总的来说就是自定义view会发生问题，发生问题前就得明白它，然后完美解决它

**View是什么？** 其实可以说是一个控件，它包含了微件(Button,TextView..)和ViewGroup，为甚这么说呢，是因为它们的都继承View类

**View位置坐标：** weight、height、left、top、right、bottom、x、y、translationX、translationY

其中translationX、translationY值用于平移View时更改的值，它是View相对于父容器的偏移量；平移时不会更改left、right这样的值，变的是x、y、translationX、translationY偏移量的值

**View事件：**

- MotionEvent(ACTION_DOWN、ACTION_MOVE、ACTION_UP)
- TouchSlop(系统识别滑动的最小距离，手指滑动屏幕距离小于这个值就认为是点击)
- VeIocityTracker(手指滑动速度，分为水平和垂直方向速度)
- GestureDetector(检测手指单击、滑动、长按、双击等行为)
- ScroIIer(由于scrollTo/scrollBy的滑动是瞬间完成的，用这个可以给滑动添加过渡效果)

**View滑动：** view除了点击就是滑动，滑动有三种实现方式

- scrollTo/scrollBy   适用于简单的View内容滑动

  它们的使用**只能改变View的内容在View内的位置，不能改变View在布局中的位置**；scrollBy属于相对滑动，scrollTo属于绝对滑动；scrollTo源码方法中的mScrollX和mScrollY为View左边缘和内容左边缘距离、View右边缘到内容右边缘距离；scrollBy本质是调用scrollTo来实现

- 添加动画   适用于没有交互的View动画效果

  就是操作View的**translationX/Y属性**，有两种动画方式，第一种原始动画就是在xml中定义动画标签，第二种属性动画就是在代码中使用一些动画库来实现；注意在实现动画的时候注意View的真正位置

- 改变View的LayoutParams参数改变原始布局   适用于有交互的布局或动画效果

  具体就是获取View的参数，然后改变它，注意这个得在UI线程中操作

  ```kotlin
  val params = toSecontBtn.layoutParams
  params.height += 10
  ```

| 滑动方式                   | 特点                                                     |
| -------------------------- | -------------------------------------------------------- |
| scrollTo/scrollBy          | 实现View的内容滑动；滑动效果不影响View内部单击事件；     |
| 动画                       | 可以实现View复杂滑动效果；在实现时得注意View的真正位置； |
| 改变View的LayoutParams参数 | 适用于有交互情况的View                                   |

**View弹性滑动：** 上面的滑动是让View咻地一下过去，没有过渡效果

- ScroIIer

  ```kotlin
  toSecontBtn.scrollBy(20,30)//让button中内容向左上移动一点距离
  ```

  Scroller本身其实不能实现弹性滑动，它得结合computeScroll方法，这个方法会让View不断重新绘制，这个绘制才是实现弹性滑动的根本原理

- 动画

  ```kotlin
  //通过动画将Button向右移动300距离，并持续3s
  ObjectAnimator.ofFloat(toSecontBtn,"translationX",0f,300f).setDuration(3000).start()
  ```

- 延时

  就是在普通View滑动的基础上使用Handler或View的postDelayed不断发送消息，然后在消息处理中对View处理滑动，从而达到弹性滑动效果

  ```kot
  ```

**View事件分发机制：** 事件分发机制主要就是为了更好实现自定义View，避免滑动冲突等问题出现

点击事件的传递规则就是MotionEvent的点击事件分发，点击事件分发其实就是得明白一个MotionEvent事件产生后是怎么由Activity传递到具体View的过程；这里涉及三个方法dispatchTouchEvent、onInterceptTouchEvent和onTouchEvent

- dispatchTouchEvent 事件传递时最先调用，返回结果受当前onTouchEvent和下面onInterceptTouchEvent方法的结果影响，表示是否调度此事件
- onInterceptTouchEvent 用返回结果来表示当前View是否拦截事件，true表示拦截
- onTouchEvent 事件被上面方法拦截后会调这个方法处理事件，这个方法的返回值表示是否处理当前事件

这三个方法的在每个View中都有，即当事件来到此View，那么dispatchTouchEvent方法一定会被调用，然后调用onInterceptTouchEvent这个方法来判断此View是否拦截事件，如果拦截就把事件交给onTouchEvent处理，如果不拦截即返回false就继续将事件传递给此View的子View

```java
public boolean dispatchTouchEvent(MotionEvent ev) {//原理的伪代码过程
    boolean consume = false;
   if (onInterceptTouchEvent(ev)) {
        consume = onTouchEvent(ev);
   } else {
        consume = child.dispatchTouchEvent(ev);//当前View不拦截事件，事件继续传递给子View
   }
    return consume;
}
```

注意：如果此View拦截并设置有OnTouchListener，那这个就像个大哥一样会处理事件，如果他处理不了返回false才会把事件叫给onTouchEvent小弟处理

事件分发过程，首先事件是由Activity给Window，再给顶级View，然后才进行分发事件给具体的子View处理；所以会发现一个事，当具体子View处理不了事件，即onTouchEvent返回false后，很可能事件会回退到Activity中的onTouchEvent来处理

注意：

- 这里说的事件指down、move...move、up的系列事件
- 如果一个View拦截并处理事件中的其中一个，那么说明此系列事件都由此View接受处理
- onInterceptTouchEvent只在这个系列事件中第一次判断时调用
- 如果down事件到此View的onTouchEvent返回false则说明它处理不了，这时系列事件中后面事件都交给它的父View处理，**注意！**
- ViewGroup默认不拦截事件，源码中ViewGroup中onInterceptTouchEvent方法默认返回false
- 若View没有onInterceptTouchEvent方法，所以事件给它就会直接调用onTouchEvent方法

**View事件分发机制细节：**

1. 事件一般是说MotionEvent，它最初是由Activity的dispatchTouchEvent接收调度，这个方法内部有个window的调用方法getWindow().superDispatchTouchEvent(ev))，实际上就是PhoneWindow实现类将事件传递给一个mDecor，这个mDecor就是activity中setContentView方法中设置的View，到这就**完成了从activity到顶层View的事件传递**

2. 到这事件已经来到顶层View(ViewGroup)，这时ViewGroup中的onInterceptTouchEvent如果返回true则拦截事件给它的onTouchEvent处理，如果返回false说明ViewGroup不拦截事件，事件继续传递给里面具体子View进行调度，拦截，处理

   ```java
   //ViewGroup 中dispatchTouchEvent方法中关键源码
   
   // Handle an initial down.
               if (actionMasked == MotionEvent.ACTION_DOWN) {
                   // Throw away all previous state when starting a new touch gesture.
                   // The framework may have dropped the up or cancel event for the previous gesture
                   // due to an app switch, ANR, or some other state change.
                   cancelAndClearTouchTargets(ev);
                   resetTouchState();
               }
   
   // Check for interception.
               final boolean intercepted;
               if (actionMasked == MotionEvent.ACTION_DOWN
                       || mFirstTouchTarget != null) {
                   final boolean disallowIntercept = (mGroupFlags & FLAG_DISALLOW_INTERCEPT) != 0;
                   if (!disallowIntercept) {
                       intercepted = onInterceptTouchEvent(ev);
                       ev.setAction(action); // restore action in case it was changed
                   } else {
                       intercepted = false;
                   }
               } else {
                   // There are no touch targets and this action is not an initial down
                   // so this view group continues to intercept touches.
                   intercepted = true;
               }
   ```

3. 上面是ViewGroup 中dispatchTouchEvent方法中关键源码，其中有一点很重要，首先如果ViewGroup不拦截事件处理而是交给子View处理，那mFirstTouchTarget就指向子View，就不为空；反之就为空，为空的话如果当前ViewGroup如果有move和up事件来的话就会默认交给它处理

   然后注意从resetTouchState();这个调用可以清楚，每次down事件来都会重置FLAG_DISALLOW_INTERCEPT状态，这个状态值是子View设置的，所以每次down事件会重置它，从而每次当前的ViewGroup会判断是否拦截新的down事件   **很重要**

   ```java
   // 当前View不处理事件，事件分发给子View
   final int childrenCount = mChildrenCount;
                       if (newTouchTarget == null && childrenCount != 0) {
                           final float x =
                                   isMouseEvent ? ev.getXCursorPosition() : ev.getX(actionIndex);
                           final float y =
                                   isMouseEvent ? ev.getYCursorPosition() : ev.getY(actionIndex);
                           // Find a child that can receive the event.
                           // Scan children from front to back.
                           final ArrayList<View> preorderedList = buildTouchDispatchChildList();
                           final boolean customOrder = preorderedList == null
                                   && isChildrenDrawingOrderEnabled();
                           final View[] children = mChildren;
                           for (int i = childrenCount - 1; i >= 0; i--) {
                               final int childIndex = getAndVerifyPreorderedIndex(
                                       childrenCount, i, customOrder);
                               final View child = getAndVerifyPreorderedView(
                                       preorderedList, children, childIndex);
   
                               // If there is a view that has accessibility focus we want it
                               // to get the event first and if not handled we will perform a
                               // normal dispatch. We may do a double iteration but this is
                               // safer given the timeframe.
                               if (childWithAccessibilityFocus != null) {
                                   if (childWithAccessibilityFocus != child) {
                                       continue;
                                   }
                                   childWithAccessibilityFocus = null;
                                   i = childrenCount;
                               }
   
                               if (!child.canReceivePointerEvents()
                                       || !isTransformedTouchPointInView(x, y, child, null)) {
                                   ev.setTargetAccessibilityFocus(false);
                                   continue;
                               }
   
                               newTouchTarget = getTouchTarget(child);
                               if (newTouchTarget != null) {
                                   // Child is already receiving touch within its bounds.
                                   // Give it the new pointer in addition to the ones it is handling.
                                   newTouchTarget.pointerIdBits |= idBitsToAssign;
                                   break;
                               }
   
                               resetCancelNextUpFlag(child);
                               if (dispatchTransformedTouchEvent(ev, false, child, idBitsToAssign)) {
                                   // Child wants to receive touch within its bounds.
                                   mLastTouchDownTime = ev.getDownTime();
                                   if (preorderedList != null) {
                                       // childIndex points into presorted list, find original index
                                       for (int j = 0; j < childrenCount; j++) {
                                           if (children[childIndex] == mChildren[j]) {
                                               mLastTouchDownIndex = j;
                                               break;
                                           }
                                       }
                                   } else {
                                       mLastTouchDownIndex = childIndex;
                                   }
                                   mLastTouchDownX = ev.getX();
                                   mLastTouchDownY = ev.getY();
                                   newTouchTarget = addTouchTarget(child, idBitsToAssign);
                                   alreadyDispatchedToNewTouchTarget = true;
                                   break;
                               }
   
                               // The accessibility focus didn't handle the event, so clear
                               // the flag and do a normal dispatch to all children.
                               ev.setTargetAccessibilityFocus(false);
                           }
                           if (preorderedList != null) preorderedList.clear();
                       }
   ```

4. 当前ViewGroup不拦截处理事件后会事件分发给子view，

**View滑动冲突：** 滑动冲突的场景一般是在有两层可交互的的View叠加，在用户操作的时候发生预料之外的现象

- 场景一：外部滑动和内部滑动方向不一致；例如外面框可以左右滑动，但是里面有个框可以上下滑动

- 场景二：外部滑动和内部滑动方向一致：例如外面框和内部框都可以左右滑动

- 场景三：结合场景一和场景二的场景：例如SlideMenu内部有个ViewPager，完事里面再嵌套一个ListView

  解决方式：

- 场景一：根据手指滑动水平/竖直方向的距离差、夹角、速度的不同分辨出是想要外面View活动还是内部View活动

  ```kot
  //外部拦截法，父 View 的 onInterceptTouchEvent 方法中拦截子 View 的滑动事件，
  //然后将它们传递给自己的 onTouchEvent 方法进行处理
  override fun onInterceptTouchEvent(ev: MotionEvent?): Boolean {
          var intercepted: Boolean = false
          val x = ev?.x
          val y = ev?.y
          when (ev?.action) {
              MotionEvent.ACTION_DOWN -> intercepted = false//默认不拦截down事件
              MotionEvent.ACTION_MOVE -> {
                  if (条件) {//根据某种条件来拦截事件并让此父View处理
                      intercepted = true//当前view拦截事件并处理
                  } else {
                      intercepted = false
                  }
              }
              MotionEvent.ACTION_UP -> intercepted = false //避免子View处理不了up事件
          }
          return intercepted
      }
  //内部拦截法，即在子View中重写dispatchTouchEvent,
  //利用parent.requestDisallowInterceptTouchEvent(true)请求父 View 不要拦截滑动事件
      override fun dispatchTouchEvent(ev: MotionEvent?): Boolean {
          val x = ev?.x
          val y = ev?.y
          when (ev?.action) {
              MotionEvent.ACTION_DOWN -> parent.requestDisallowInterceptTouchEvent(true)
              MotionEvent.ACTION_MOVE -> {
                  if (条件) {//根据某种条件来判断当前View是否处理事件
                      parent.requestDisallowInterceptTouchEvent(false)
                  }
              }
          }
          return super.dispatchTouchEvent(ev)
      }
  ```

  

- 场景二：由于两个嵌套View的活动方向是一样的，所以上面的办法不适用，这时就得按照业务的某种要求，利用这个要求来选择让哪个View活动

- 场景三：解决办法也是和场景二差不多，按照某种业务规律、规定来处理事件

**View自定义：** 前面都是基础知识，自定义View才是主角

> 主要掌握view的测量、布局、绘制工作原理，还有一些回调方法(构造方法、onAttach、onDetach..)的实现

在自定义的时候可以选择继承View、ViewGroup或现有控件Button、TextView等

基本概念：View的显示过程，首先当activity创建好后会将DecorView添加到Window中，即添加到activity中；然后这时会创建ViewRoot的实现类ViewRootImpl对象，来和DecorView关联上，DecorView中的View绘制就是利用ViewRoot中performTraversals方法中的measure、layout、draw三个过程来执行遍历View实现绘制，（DecorView实际就是内容栏，属于顶级View，就是setContentView中设置的那个View），事件分发都是先到DecorView后再分发给具体子View

measure决定了View的宽高、layout决定了View在布局中的位置、draw决定了View的样式内容

MeasureSpec是什么？它其实就是View的规格，为什么有它是因为View的实际大小参数会被父容器规则影响；MeasureSpec实际就是个32位的int值，它又由高2位的SpecMode和30位的SpecSize组成，其中一个SpecMode和一个SpecSize会组成一个MeasureSpec

- SpecMode.UNSPECIFIED   父容器不对子View的规格有任何限制
- SpecMode.EXACTLY   父容器计算出并规定了View的规格，这个规格就是SpecSize值，对应于LayoutParams中match_parent和具体数值两种模式
- SpecMode.AT_MOST   父容器给了一个View规格，它不能超过这SpecSize值，对应就是LayoutParams中wrap_content

所以可以明白MeasureSpec其实就是DecorView和子View的实际规格；对于DecorView，它的MeasureSpec由窗口尺寸和自身LayoutParams共同决定；对于子View，它的MeasureSpec由父容器和LayoutParams共同决定实际规格；一旦某个view的MeasureSpec决定了，onMeasure就可以得到View的实际大小规格了

```xml
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"//LayoutParams的精确模式，代表窗口大小
    android:layout_height="match_parent"//wrap_content是内容最大模式，最大就是窗口大小
    tools:context=".MainActivity">
    <Button
        android:id="@+id/btn_to_second"
        android:layout_width="wrap_content"//100dp就是固定大小，直接指定LayoutParams值
        android:layout_height="wrap_content"
        android:layout_marginBottom="144dp"
        android:text="@string/btn_to_second"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.526"
        app:layout_constraintStart_toStartOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```







## AsyncTask 异步

> 这个早被弃用了，这个东西过于封装，方便，所以会有一些漏洞。现在多用java的并发包工具和Handler

## Handler 机制

> 它的存在就是因为Android中主线程才能更新UI，主线程不能执行耗时操作，所以得要用子线程

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
private val handler = object : Handler(Looper.getMainLooper()) {//绑定到主线程和主线程的消息队列
        override fun handleMessage(msg: Message) {//主线程接受消息处理消息
            //super.handleMessage(msg)
            when(msg.what) {
                1 -> print("消息来了...")//就可以在UI线程修改页面处理了
            }
        }
}//可以说主线程的Looper、MessageQueue、Handler吗？可以  多个子线程可以对应多个Handler，向里面发送消息
```

由上面就知道Handler用于进程中线程间的通信，而Android中进程间通信用的是Binder

- Message 专门给Handler处理的消息或Runnable对象，既可以存储简单数据也可以存储复杂类型数据，单链表数据结构，MessageQueue用链表结构实现队列

  ```java
  int what;int arg1;Object obj;Bundle data;//里面有这些字段
  private static final int MAX_POOL_SIZE = 50;//消息最大为50
  ```

  消息池 sPool Message里面用sPool指向单链表顶部来实现栈，它的作用是消息使用后会被回收在这，这部分消息是为了重复利用，并且采用了享元模式避免重复消息对象；创建Message的首选是调用Message.obtain()，源码里面推荐的，本来也是这个道理。

  

  ```java
  public long when;//这个可以在传递消息时设置，MessageQueue中Message的顺序就看这个值的大小 测试时使用
  Handler target;//保存处理当前消息的Handler
  int flags;//消息的状态标记 0、FLAG_IN_USE、FLAGS_TO_CLEAR_ON_COPY_FROM、FLAG_ASYNCHRONOUS
  
  public Messenger replyTo;
  //可选的Messenger，可以发送此消息的回复。具体如何使用的语义取决于发送方和接收方
  ```

- Looper  用于为线程运行消息循环，提供线程上下文环境，创建与线程绑定；创建管理MessageQueue；消费者模式(没消息就阻塞等待)的Looper在MessageQueue中获取消息(只要Looper还在就一直运行，要不跟随应用退出，要不就阻塞)，并叫Handler处理消息；

  **一个Looper只对应一个线程，反之同样**，比如Looper.getMainLooper()就是对应当前主线程，为什么能直接获取，因为Android在系统启动的时候就用ActivityThread.java创建了

  ```java
  /**
  *Returns the application's main looper,which lives in the main thread of the application.
  */
  public static Looper getMainLooper() {
  private Looper(boolean quitAllowed) {
  //Looper的构造函数是私有的，通常都是由静态方法prepare()来创建;上面这个方法也是一样用来创建Looper
  public static void loop() {//这个方法是用来不断从MessageQueue中取消息的
      
  //这是一个源码中实现Looper线程的典型示例，使用prepare和loop的分离来创建一个初始Handler来与Looper通信。
     class LooperThread extends Thread {
         public Handler mHandler;
   
         public void run() {
             Looper.prepare();//初始化当前线程的Looper
   
             mHandler = new Handler(Looper.myLooper()) {
                 public void handleMessage(Message msg) {
                     // process incoming messages here
                 }
             };
   
             Looper.loop();//进入循环消息模式，有Message就将它发送给它内部target引用的Handler
         }
     }
  ```

- Handler 它的子类必须为static，避免内存泄露；用于发送消息、处理消息、移除消息；Looper在哪个线程是由Handler初始化位置决定。

  从它的构造方法可以看出创建Handler就意味着Looper，MessageQueue，Callback，mAsynchronous的创建

  而且源码里面有创建消息的方法，但是直接用Message比较好

  ```java
  public Handler(@NonNull Looper looper, @Nullable Callback callback, boolean async) {
          mLooper = looper;//如果构造函数中没Looper就使用默认Looper
          mQueue = looper.mQueue;//使用looper中创建的MessageQueue
          mCallback = callback;//设置回调接口，默认为null
          mAsynchronous = async;//默认同步
  }
  public final Message obtainMessage() {//就这个，少用
          return Message.obtain(this);
  }
  public final boolean sendMessage(@NonNull Message msg) {
          return sendMessageDelayed(msg, 0);//在当前时间之前的所有挂起消息后，将消息压入消息队列的末尾
  }
  ```

- MessageQueue  前面说过了消息队列是Looper中创建的，这个队列中Message位置是由Message中when值排序的先进先出；

  ```java
  Message next() {//Looper会调用这个来取消息，这里面有个nativePollOnce(ptr, nextPollTimeoutMillis)调用来判断没消息后的阻塞
  
  ```

- HandlerThread创建与销毁  由两个Handler和一个HandlerThread实现后台线程串行执行；有一个循环器的线程。然后可以使用循环程序创建处理程序。注意，就像普通线程一样，start()仍然必须被调用。内部实现机制主要是实现了Looper；它的退出在activity中将引用设置为null就行；

  ```java
  //示例： UI线程的Handler
  Handler mHandler = new Handler(){//这个空构造现在已经被弃用
      @Override
      public void handleMessage(Message msg) {
          super.handleMessage(msg);
          // 处理UI更新
  };
  
  HandlerThread mBackThread = new HandlerThread("mybackthread");
  mBackThread.start();
  // 后台线程的Handler
  Handler mBackHandler = new Handler(mBackThread.getLooper());//初始化在start()之后，为了得到mBackThread的Looper
  
  mBackHandler.post(new Runnable() {//把这个Runnable放到消息队列中，开启一个线程起到多个线程的作用
      @Override
      public void run() {
          // 后台线程执行耗时操作，异步后台任务执行；这个其实和最开始的异步消息处理机制相似，但是这个
          //比Thread多了个消息循环机制(有自己的Looper)，还更容易管理
          ...
          // mHandler发消息，回到主线程更新UI
          mHandler.sendMessage(msg);
      }
  });//用完及得退出，避免内存泄露，使用quit或quitSafely(清空消息之前把非延迟消息派发出去处理)
  //使用场景：对于非UI线程又想用消息机制时、替换平时的Thread匿名线程使用、I/O操作
  ```

  Android中多个线程创建的多个不同的Handler都关联到主线程的Looper，一个Looper对应一个线程和消息队列

  ![](../IMG/HandlerProcess.png)

> 项目中就用这个机制来不断发送收集event

## Binder 机制

> 之前的Handler主要是创建一个子线程并在里面循环处理消息，属于子线程之间的消息传递；这个Binder属于进程间的通信机制，比如activity和service就是两个进程，他们通信方式之一就是Binder机制。
>
> 一开始接触Binder的用法是activity绑定后台service，并且可以调用service里面的方法，这样就得用到Binder机制。
>
> 首先为什么要有Binder，因为每个应用运行在自己的进程中，进程这个东西是在内存中的，然后Linux中有个东西叫内核Kernel，它也叫内核空间，也在内存中，是一块用于进程与硬件间交流的“中间人”，这个内核中间人它很严格，保证整个系统的生态正常运行；但是现在需要进程和进程之间通信怎么办呢，这得需要内核它老人家同意啊，这时就出现个老人家的徒弟，让进程可以通过徒弟去劝说内核老人家处理通信这件事，这个徒弟就在内核态空间中一个角落，叫动态可加载模块(Loadable Kernel Module，LKM)，这个模块也叫Binder驱动，驱动进程-硬件-进程的数据传输。
>
> 然后有个问题，Linux中进程通信不是有管道、socket这些吗，为什么还要弄个Binder这个出来？主要有安全和性能两方面原因，Binder它在安全上使用的机制可以对通信的两方做身份验证，比socket更安全，在数据传输上也更高效。
>
> 下面是简单的activity绑定后台service，用Binder进行通信，还有AIDL也是用Binder通信。

```kotlin
class MyService : Service() {//后台service
    private val mBinder = DownloadBinder()
        class DownloadBinder : Binder() {
            fun startDownload() {
                Log.d("MyService", "startDownload executed")
            }
            fun getProgress(): Int {
                Log.d("MyService", "getProgress executed")
                    return 0
            }
        }
    override fun onBind(intent: Intent): IBinder {
        return mBinder//连接完后会回调这个方法返回Binder对象
    }
    ...
}
//activity绑定service并调用里面的方法
lateinit var downloadBinder: MyService.DownloadBinder
    private val connection = object : ServiceConnection {
        override fun onServiceConnected(name: ComponentName, service: IBinder) {
            downloadBinder = service as MyService.DownloadBinder
            downloadBinder.startDownload()//用Binder对象控制
            downloadBinder.getProgress()
        }
        override fun onServiceDisconnected(name: ComponentName) {
        }
    }
override fun onCreate(savedInstanceState: Bundle?) {
...
    bindServiceBtn.setOnClickListener {
    val intent = Intent(this, MyService::class.java)
        bindService(intent, connection, Context.BIND_AUTO_CREATE) // 绑定Service
}
    unbindServiceBtn.setOnClickListener {
        unbindService(connection) // 解绑Service
    }
}

//AIDL的IPC通信
//服务端
class MyService : Service() {

    private val mBinder = object : IMyAidlInterface.Stub() {
        override fun saveData(data: String?) {
            if (data != null) {
                ,,,//执行数据保存动作
            }
        }
    }
    override fun onCreate() {
        super.onCreate()
    }
}
//客户端
class MainActivity : AppCompatActivity() {
    private val TAG = "MainActivity"
    var remoteInterface: IMyAidlInterface? = null

    private val connection = object : ServiceConnection {
        override fun onServiceConnected(name: ComponentName?, service: IBinder?) {
            remoteInterface = IMyAidlInterface.Stub.asInterface(service)
        }

        override fun onServiceDisconnected(name: ComponentName?) {
        }
    }

    private fun bindRemote() {
        val intent = Intent()
        intent.component =
            ComponentName("com.example.serviceaidl", "com.example.serviceaidl.MyService")

        var bindService = bindService(intent, connection, Context.BIND_AUTO_CREATE)
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val sendButton: Button = findViewById(R.id.sendRemote)
        val editData: EditText = findViewById(R.id.editData)

        sendButton.setOnClickListener {
            val dataNum: String = editData.text.toString()

            bindRemote()
            remoteInterface?.saveData(dataNum)
        }

    }

    override fun onDestroy() {
        super.onDestroy()
        unbindService(connection)
    }

}
//在AIDL生成的接口文件中有Binder代理对象和Binder本地对象
//如果这个service是和activity在一个项目中，那就是本地对象
//如果service在别的app中，那使用的就是Binder代理对象来进行信息传递
//
```

> 实现原理：在内核空间的Binder模块中有个叫ServiceManager，它会保存每个service进程的信息。
>
> 注意这里可能不是activity向Binder通知，但是肯定是一个进程
>
> 具体就是activity会向Binder通知，叫Binder建立SM，然后每个service进程启动后会向SM记录自己的信息，所以进程间通信就得经过Binder和SM这部分。然后其实客户端调服务端这样的IPC，客户端调用服务端中的方法，看起来是在客户端中调用了，但实际上客户端中调用方法这个动作只是给Binder一个信号(代理对象)，让Binder从SM中找到这个服务端，然后叫服务端执行这个方法，然后返回结果(也是一个代理对象)，Binder再把这个结果给客户端。
>
> 到现在就可以发现Binder内部操作挺多的，但是在使用上来看很简单，客户端自己做命令，服务端自己执行。这就是面向对象思想中的封装复杂操作，让使用者轻松使用这个机制。























## 配置测试发布

一个APP的从0到1：

1. 配置开发环境：就是配置电脑环境、Androidstudio软件、然后再根据开发的APP创建项目
2. 编写代码：利用AndroidStudio写代码、设计界面、资源等
3. 构建运行：配置好build.gradle，生成对应调试apk在测试机上运行
4. 调试分析和测试：与测试组结合解决bug，用工具查看APP内存、CPU、网络、渲染等性能，并优化
5. 发布到应用商店：APP代码设计、代码、bug、性能等问题解决完后用安全密匙签名，发布到Google商店

## 1. 设置开发环境

> 设置jdk，sdk，下载个AndroidStudio，配置好就是

## 2. 编写代码

工具：

- PNG\JPEG和webp图片转换：搞张图片放到IDEA中右键直接转换，选无损或有损就OK了
- lint 工具用于检查代码是否存在可能影响 Android 应用的质量和性能的结构问题



## 3. 构建运行

**构建工具Gradle：** 自动化构建和打包应用程序

在 Android Studio 中，用于构建 APK 的 Gradle 实际上是一个构建工具链，由 **Gradle 构建系统**和 **Android Gradle 插件**组成

Gradle 是一个基于 Groovy 语言的构建工具，它提供了一种灵活和可扩展的方式来构建和打包 Android 应用程序

Android Gradle 插件是一个构建插件，它扩展了 Gradle 构建系统，使其能够理解和处理 Android 应用程序的特定需求，如资源管理、应用签名、APK 打包等。它还提供了许多构建任务和插件，帮助自动化构建过程，从而提高构建效率

在 Android Studio 中，Gradle 将应用程序的代码和依赖项编译成 Dex 文件，将资源打包成 APK 文件，并使用 Android SDK 提供的工具将 APK 文件进行签名和对齐，最终生成一个可安装的 APK 文件

Gradle 的构建过程包括以下几个步骤：

1. 读取构建文件：Gradle 读取项目根目录下的 build.gradle 文件和 app 模块下的 build.gradle 文件，这些文件包含了应用程序的构建配置信息和依赖项声明。
2. 下载依赖项：Gradle 根据 build.gradle 文件中的依赖项声明自动下载所需的库文件和插件，并将其缓存到本地 Gradle 缓存中。
3. 编译源代码：Gradle 使用 Java 编译器编译应用程序的源代码，并生成 Dex 文件
4. 打包资源：Gradle 使用 AAPT 工具将应用程序的资源打包成 APK
5. 签名 APK：Gradle 使用 APK 签名工具将 APK 文件进行签名，以确保其未被篡改
6. 对齐 APK：Gradle 使用 zipalign 工具将 APK 文件进行对齐，以提高应用程序的启动性能
7. 输出 APK：Gradle 将签名和对齐后的 APK 文件输出到 app/build/outputs/apk 目录下，用来部署到设备或上传到应用商店

**Gradle和Android Gradle插件的区别：**

当你创建一个Android应用程序项目时，Android Studio会自动为你创建一个基本的Gradle构建文件(build.gradle)。这个文件包含了一些Gradle任务，用于编译代码、打包应用程序、生成APK文件等等。

但是，如果你要构建Android应用程序，那么只使用标准的Gradle构建工具可能并不够。这是因为构建Android应用程序需要一些特定的任务和功能，例如生成R.java文件、处理资源、合并清单文件、签名和生成apk文件等等。为了简化构建过程并提供这些特定的任务和功能，Google开发了一个名为Android Gradle插件的Gradle插件。这个插件与Gradle一起使用，以便更轻松地构建和打包Android应用程序

Android Gradle插件中含有Gradle插件API，可以自己编写适合当前应用的插件，为应用程序提供自定义构建逻辑

因此，Android Gradle插件只是是Gradle构建工具的一个扩展，使得可以更轻松地构建和打包Android应用程序。

- 处理资源文件
- 生成R.java文件
- 合并清单文件
- 签名APK文件
- 生成APK文件



**build.gradle内容详解：**



build类型：自定义设置应用构建打包时使用的属性，从而生成调试版/发布版的APK/AAB，用来调试或发布，其中调试版会用调试密匙为应用签名，而发布版会用发布密匙为应用签名；因为发布版会应用代码混淆、资源缩减等操作，所以体积会比调试版小不少

产品变种：同一份工程代码，build.gradle中自定义设置，使之生成不同类型、不同体积的APK/AAB,比如免费版和收费版apk

build变体：产品变种和build类型结合的结果，在build.gradle中使用特定规则将产品变种和build类型结合起来的结果；比如产品变种有person和plus两个版本，然后build类型有debug和release两个类型，那么总的build变体就有四种结果：persondebug、personrelease、plusdebug、plusrelease

清单条目：好像是在清单条目中设置值可以覆盖AndroidManifest.xml文件中的设置

签名：调试版可以自动生成，也可以手动在build配置中自行配置；发布版不会自动生成签名，需要在google申请密匙，然后将密匙和证书放到build配置中，才能上传到google应用商店

代码和资源缩减：在ProGuard规则文件中给每个build变体指定相应规则，规则会缩减生成apk/AAB体积，还不影响原有功能

多apk支持：就是设置规则，例如每种屏幕密度或语言构建一种apk，这样不太好，而且目前都是只生成一个AAB上传到google商店

build 配置文件：里面用领域特定语言 (DSL) 以 Groovy 动态描述和操作构建apk/aab的逻辑；到这步其实就是代码完成之后，对应用的一种配置，一种优化

顶层settings.gradle：里面就是声明应用使用了哪些模块，比如：

include ‘:app’

include ':project'

顶层gradle.properties：定义gradle守护堆大小，全局gradle设置，比如android.enableJetifier=true

顶层local.properties：定义本地SDK路径

顶层build.gradle：声明所有模块共用的依赖、变量、清理build文件的代码，比如：

顶层versions.gradle：声明应用需要的外部依赖及其版本号，别的地方需要依赖直接在这拿就行，统一在这个文件管理依赖

def xxxx = [:]

build_versions.min_sdk = 33 

androidx.core_ktx = "androidx.core:core-ktx:1.7.0"

使用：build_versions.min_sdk

模块build.gradle：每个模块单独需要的配置，自定义build类型和产品变种、覆盖顶层build.gradle的设置

配置完后Sync Project同步项目文件，使项目导入已定义的配置和执行一些gralde检查



源代码集：每当给项目创建一个模块后模块内都有一个main/目录，这个就是主源代码集，里面有源代码和资源，这个属于主源代码集，如果项目需要创建build变体，最好再定义src/productFlavor/这样类似的源代码集，在里面放专属相应build变体的代码资源





# 其它技术

> 技术，检验基础与实践

## Protocol Buffers

> Protocol Buffers是一种用于序列化结构化数据的与语言无关、与平台无关的可扩展机制。
>
> 为什么使用它，就是因为它在网络上传输流量使用少，存储时所占用的空间小；它比java自带的序列化好，没有XML的空间密集型(会对解编码造成性能损失)

**特性：**

- 它就是一个谷歌的序列化机制，和xml差不多，而且体积更小，更快，更简单.
- 用于结构化数据与数据流之间的转换；它可以用在java、Python、C++、go等语言项目中.
- 它向后兼容性好，后面对`.proto`添加删除字段是不会影响现有的proto服务.
- 支持不同语言转换，与别的语言共享数据，用java生成的序列化数据可以用Python来解析出来.

**不适合场景：**

- 消息体积大的场景，因为它是一次加载到内存的方式，所以对应几兆以上的数据就不推荐用这个
- 读取序列化数据场景，因为它可能会将相同数据转为不相同的二进制序列化，得再解析出来一次才能知道是相同的
- 科学项目场景，因为一些浮点数太长的它的转换都不太好

**使用：**定义数据结构化方式`.proto` - 编译器工具转为源代码 - 对应语言将结构化数据写入数据流或将数据流转为结构化数据

1. 

web项目使用：添加Maven配置<dependency>...

Android使用：build.gradle中添加依赖`implementation 'com.google.protobuf:protobuf-java:3.22.2'` 注意版本号，Android中推荐使用Java Lite

`For Android users, it's recommended to use protobuf Java Lite runtime because of its smaller code size.`

2. 

编译器工具中添加.proto文件，并且在里面定义结构化语言对应的消息；为要序列化的每个数据结构添加一条消息；

```protobuf
message Person {
  optional string name = 1;
  optional int32 id = 2;
  optional string email = 3;
}
```

用proto编译器工具将.proto文件转换为对应的语言(源代码)，这个源代码里面描述的就是包含每个字段的简单访问器和方法；每个消息对应.java文件中一个类和一个用来产出流的Builder类

使用protobuf API写入和读取消息：该类提供的get/set方法将字段转换为高效二进制格式，

```java
Person john = Person.newBuilder()
    .setId(1234)
    .setName("John Doe")
    .setEmail("jdoe@example.com")
    .build();
output = new FileOutputStream(args[0]);
john.writeTo(output);
```

在java项目中就可以用生成类源代码文件中的方法将 结构化语言 与 原始字节流互相转换；



**工作原理：**

![drawio](/home/lihang/Applications/drawio.png)

**.probo语法概括：**

```protobuf
//示例
enum EnumNote {
  ENAA = 0;
  ENBB = 1;
}
message Person {
  optional string name = 1;
  optional EnumNote id = 2;
  optional string email = 3;
}
```

定义字段时，可以定义可选、可重复、单数的；定义消息`message`来嵌套数据集、定义枚举`enum`能规定一组值供选择、定义`oneof`可选字段能在字段中选择其中一个、定义`map`映射能添加键值对数据。

数据类型可以有基本数据类型，其他数据类型(不常用，比如Timestamp、Duration,相比于用这个，更应该在程序中自定义使用)

字段编号给不能重复，如果删除字段就保留编号，防止混乱使用

**.proto语法：**

- 如果需要使用proto3，需要在.proto文件的首行定义`syntax = "proto3"`

- 一条消息表示一个结构化数据，其实可以看做一个对象，只是这个对象体积很小

  ```protobuf
  message Person {
    optional string name = 0;//字段
    optional string email = 1;//这里也可以用非基本类型，比如枚举，其他消息类型
  }
  ```

- 字段编号不能重复，1~15占一个字节，16 ~2047占两个字节，1 ~15最好给频繁出现的字段

- 如果想删除字段，为了避免以后误重复使用造成数据问题，可以使用保留字段

  ```java
  message Foo {
    reserved 2, 15, 9 to 11;
    reserved "foo", "bar";
  }
  ```

- 如果在项目中不给消息字段set值，那这个消息字段默认为对应类型的默认值

- 如果想限制消息字段的取值就用enum枚举

  ```java
  enum Corpus {
    CORPUS_UNSPECIFIED = 0;//从0开始,对应枚举value值
    CORPUS_UNIVERSAL = 1;
    CORPUS_WEB = 2;
  }
  ```

okk，结束protobuf，在项目中够用了，注意版本不同，里面有些定义是会过时的，看官网说明就行

官方文档：https://protobuf.dev/		不同语言项目所需编译器下载：https://github.com/protocolbuffers/protobuf

## Retrofit



## OkHttp



## SQLite

