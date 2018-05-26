> ```java
> implementation 'io.reactivex.rxjava2:rxandroid:2.0.2'
> ```

## 一、RxJava是什么

​	RxJava在GitHub的介绍是 "a library for composing asynchronous and event-based programs using observable sequences for the Java VM"（一个在 Java VM 上使用可观测的序列来组成异步的、基于事件的程序的库）

即  `异步`



## 二、RxJava的优点

### 1、拓展的观察者模式

* Rxjava相对于AsyncTask/Hander+Thread，如果随着程序逻辑越来越复杂，RxJava依然能够保持简洁。
* RxJava可以使用链式调用
* RxJava可以被自动Lambda化预览，使逻辑更清晰
* RxJava通过扩展的观察者模式实现

> 观察者模式面向的需求是：A 对象（观察者）对 B 对象（被观察者）的某种变化高度敏感，需要在 B 变化的一瞬间做出反应。
>
> 观察者不需要时刻盯着被观察者（例如 A 不需要每过 2ms 就检查一次 B 的状态），而是采用**注册**(Register)**或者称为**订阅**(Subscribe)**的方式，告诉被观察者：我需要你的某某状态，你要在它变化的时候通知我。 Android 开发中一个比较典型的例子是点击监听器 `OnClickListener` 。对设置 `OnClickListener` 来说， `View` 是被观察者， `OnClickListener` 是观察者，二者通过 `setOnClickListener()` 方法达成订阅关系。订阅之后用户点击按钮的瞬间，Android Framework 就会将点击事件发送给已经注册的 `OnClickListener` 。采取这样被动的观察方式，既省去了反复检索状态的资源消耗，也能够得到最高的反馈速度。

![éç¨è§å¯èæ¨¡å¼](http://ww3.sinaimg.cn/mw1024/52eb2279jw1f2rx4446ldj20ga03p74h.jpg)



#####RxJava的观察者模式：

>  RxJava 有四个基本概念：`Observable` (可观察者，即被观察者)、 `Observer` (观察者)、 `subscribe` (订阅)、事件。`Observable` 和 `Observer` 通过 `subscribe()` 方法实现订阅关系，从而 `Observable` 可以在需要的时候发出事件来通知 `Observer`。
>
> 与传统观察者模式不同， RxJava 的事件回调方法除了普通事件 `onNext()` （相当于 `onClick()` / `onEvent()`）之外，还定义了两个特殊的事件：`onCompleted()` 和 `onError()`。
>
> - `onCompleted()`: 事件队列完结。RxJava 不仅把每个事件单独处理，还会把它们看做一个队列。RxJava 规定，当不会再有新的 `onNext()` 发出时，需要触发 `onCompleted()` 方法作为标志。
> - `onError()`: 事件队列异常。在事件处理过程中出异常时，`onError()` 会被触发，同时队列自动终止，不允许再有事件发出。
> - 在一个正确运行的事件序列中, `onCompleted()` 和 `onError()` 有且只有一个，并且是事件序列中的最后一个。需要注意的是，`onCompleted()` 和 `onError()` 二者也是互斥的，即在队列中调用了其中一个，就不应该再调用另一个。

##### RxJava 的观察者模式大致如下图



![RxJava çè§å¯èæ¨¡å¼](http://ww3.sinaimg.cn/mw1024/52eb2279jw1f2rx46dspqj20gn04qaad.jpg)



### 2、 观察者模式的实现



####1. 创建Observe

* 创建   `Observer` ->  观察者，它决定事件触发的时候将会有怎样的行为，RxJava中的  `Observer`  接口的实现方式

```java
Observer<String> observer = new Observer<String>() {
    @override
    public void onNext(String s) {
        Log.d("tag", "Item: " + s);
    }
    
    @override
    public void onCompleted() {
        Log.d("tag", "Compaeted!");
    }
    
    @override
    public void onError(Throwable e) {
        Log.d("tag", "Error!");
    }
};
```



`Subscriber` 是RxJava内置的抽象类，具有一些扩展，使用方法和Observer接口一样，RxJava的subscriber过程，Observer会先被转换成一个Subscriber再使用，使用基本功能时，`Observer`  和  `Subscriber`是完全一样的

```java
Subscriber<String> subsveiber = new Subscriber<String>() {
    @override
    public void onNext(String s) {
        Log.d("tag", "Item: " + s);
    }
    
    @override
    public void onCompleted() {
        Log.d("tag", "Compaeted!");
    }
    
    @override
    public void onError(Throwable e) {
        Log.d("tag", "Error!");
    }
}
```



区别在于：

1. `onStart()`: 这是 `Subscriber` 增加的方法。它会在 subscribe 刚开始，而事件还未发送之前被调用，可以用于做一些准备工作，例如数据的清零或重置。这是一个可选方法，默认情况下它的实现为空。需要注意的是，如果对准备工作的线程有要求（例如弹出一个显示进度的对话框，这必须在主线程执行）， `onStart()` 就不适用了，因为它总是在 subscribe 所发生的线程被调用，而不能指定线程。要在指定的线程来做准备工作，可以使用 `doOnSubscribe()` 方法，具体可以在后面的文中看到。
2. `unsubscribe()`: 这是 `Subscriber` 所实现的另一个接口 `Subscription` 的方法，用于取消订阅。在这个方法被调用后，`Subscriber` 将不再接收事件。一般在这个方法调用前，可以使用 `isUnsubscribed()` 先判断一下状态。 `unsubscribe()` 这个方法很重要，因为在 `subscribe()` 之后， `Observable` 会持有 `Subscriber` 的引用，这个引用如果不能及时被释放，将有内存泄露的风险。所以最好保持一个原则：要在不再使用的时候尽快在合适的地方（例如 `onPause()` `onStop()` 等方法中）调用 `unsubscribe()` 来解除引用关系，以避免内存泄露的发生。



#### 2. 创建Observable

######方法一：使用  `create()`  创建  `Observable`  ->  被观察者，它决定什么时候触发事件以及触发怎样的事件。



```java
Observable observable = Observale.create(new Observable.OnSubscribe<String>() {
   @override
    public void call(Subscriber<? super String> subscriber) {
        subscriber.onNext("Hello");
        subscriber.onNext("Hi");
        subscriber.onNext("Aloha");
        subscriber.onCompleted();
    }
});
```

 

* OnSubscribe被存储在Observable对象中，当Observable被订阅的时候，OnSubscribe的call()方法会自动被调用，事件被调用的顺序依次触发(即观察者Subscriber将被调用三次和一次onCompleted())。被观察者调用了观察者的回掉方法，实现由被观察者向观察者的事件传递，即观察者模式

`create()`  方法是RxJava最基本的创造事件序列的方法，基于这个方法，RxJava还提供了一些方法用来快捷创建事件队列。

######方法二：`just(T...)`  将传入的参数依次发送出去

```java
Observable observable = Observable.just("Hello", "Hi", "Aloha");
// 依次调用：
// onNext("Hello");	
// onNext("Hi");	
// onNext("Aloha");	
// onCompleted();	
```

######方法三：`from(T[])`  /  `from(Iterable<? extends T>)` ：将传入的数组或Iterable拆分成具体对象后，依次发送出去

```java
String[] words = {"Hello", "Hi", "Aloha"};
Observable observable = Observable.from(words);
// 将会依次调用
// onNext("Hello");
// onNext("Hi");
// onNext("Aloha");
// onCompleted();
```



#### 3. Subscribe(订阅)

创建了  `observable `  和  `Observer`  之后再用 `subscribe()` 方法将它们联结起来。

```java
observable.subscribe(observer);
// 或者:
observable.subscribe(subscriber);
```

 `subscribe()` 这个方法有点怪：它看起来是『`observalbe` 订阅了 `observer` / `subscriber`』而不是『`observer` / `subscriber` 订阅了 `observalbe`』，这看起来就像『杂志订阅了读者』一样颠倒了对象关系。这让人读起来有点别扭，不过如果把 API 设计成 `observer.subscribe(observable)` / `subscriber.subscribe(observable)` ，虽然更加符合思维逻辑，但对流式 API 的设计就造成影响



`Observable.subscribe(Subscriber)` 的内部实现是这样的（仅核心代码）：

```java
// 注意：这不是 subscribe() 的源码，而是将源码中与性能、兼容性、扩展性有关的代码剔除后的核心代码。
// 如果需要看源码，可以去 RxJava 的 GitHub 仓库下载。
public Subscription subscribe(Subscriber subscriber) {
    subscriber.onStart();
    onSubscribe.call(subscriber);
    return subscriber;
}
```

可以看到，`subscriber()` 做了3件事：

1. 调用 `Subscriber.onStart()` 。这个方法在前面已经介绍过，是一个可选的准备方法。
2. 调用 `Observable` 中的 `OnSubscribe.call(Subscriber)` 。在这里，事件发送的逻辑开始运行。从这也可以看出，在 RxJava 中， `Observable` 并不是在创建的时候就立即开始发送事件，而是在它被订阅的时候，即当 `subscribe()` 方法执行的时候。
3. 将传入的 `Subscriber` 作为 `Subscription` 返回。这是为了方便 `unsubscribe()`.

 ![å³ç³»éå¾](http://ww3.sinaimg.cn/mw1024/52eb2279jw1f2rx4ay0hrg20ig08wk4q.gif)

除了 `subscribe(Observer)` 和 `subscribe(Subscriber)` ，`subscribe()` 还支持不完整定义的回调，RxJava 会自动根据定义创建出 `Subscriber`

```java
Action1<String> onNextAction = new Action1<String>() {
    // onNext()
    @Override
    public void call(String s) {
        Log.d(tag, s);
    }
};
Action1<Throwable> onErrorAction = new Action1<Throwable>() {
    // onError()
    @Override
    public void call(Throwable throwable) {
        // Error handling
    }
};
Action0 onCompletedAction = new Action0() {
    // onCompleted()
    @Override
    public void call() {
        Log.d(tag, "completed");
    }
};

// 自动创建 Subscriber ，并使用 onNextAction 来定义 onNext()
observable.subscribe(onNextAction);
// 自动创建 Subscriber ，并使用 onNextAction 和 onErrorAction 来定义 onNext() 和 onError()
observable.subscribe(onNextAction, onErrorAction);
// 自动创建 Subscriber ，并使用 onNextAction、 onErrorAction 和 onCompletedAction 来定义 onNext()、 onError() 和 onCompleted()
observable.subscribe(onNextAction, onErrorAction, onCompletedAction);
```



这段代码中出现的 `Action1` 和 `Action0`。 `Action0` 是 RxJava 的一个接口，它只有一个方法 `call()`，这个方法是无参无返回值的；由于 `onCompleted()` 方法也是无参无返回值的，因此 `Action0` 可以被当成一个包装对象，将 `onCompleted()` 的内容打包起来将自己作为一个参数传入 `subscribe()` 以实现不完整定义的回调。这样其实也可以看做将 `onCompleted()` 方法作为参数传进了 `subscribe()`，相当于其他某些语言中的『闭包』。 `Action1` 也是一个接口，它同样只有一个方法 `call(T param)`，这个方法也无返回值，但有一个参数；与 `Action0` 同理，由于 `onNext(T obj)` 和 `onError(Throwable error)` 也是单参数无返回值的，因此 `Action1`可以将 `onNext(obj)` 和 `onError(error)` 打包起来传入 `subscribe()` 以实现不完整定义的回调。事实上，虽然 `Action0` 和 `Action1`在 API 中使用最广泛，但 RxJava 是提供了多个 `ActionX` 形式的接口 (例如 `Action2`, `Action3`) 的，它们可以被用以包装不同的无返回值的方法。

> 注：正如前面所提到的，`Observer` 和 `Subscriber` 具有相同的角色，而且 `Observer` 在 `subscribe()` 过程中最终会被转换成 `Subscriber`对象，因此，从这里开始，后面的描述我将用 `Subscriber` 来代替 `Observer` ，这样更加严谨。





##### 4. 场景实例



> 为了把原理用更清晰的方式表述出来，本文中挑选的都是功能尽可能简单的例子，以至于有些示例代码看起来会有『画蛇添足』『明明不用 RxJava 可以更简便地解决问题』的感觉。当你看到这种情况，不要觉得是因为 RxJava 太啰嗦，而是因为在过早的时候举出真实场景的例子并不利于原理的解析，因此我刻意挑选了简单的情景。



* 例1：将字符串数组 `names` 中的所有字符串依次打印出来

```java
String[] names = ...;
Observable.from(names)
    .subscribe(new Actionl<String>() {
       @override
        public void call(String name) {
            Log.d("TAG", name);
        }
    });
```



* 例2：由指定的一个 drawable 文件 id `drawableRes` 取得图片，并显示在 `ImageView` 中，并在出现异常的时候打印 Toast 报错

```java
int drawableRes = ...;
ImageView imageView = ...;
Observable.create(new OnSubscribe<Drawable>() {
   @override
    public void call(Subscriber<? super Drawable> subscriber){
        Drawable drawable = getTheme().getDrawable(drawableRes);
        subscriber.onNext(drawable);
        subscriber.onCompleted();
    }
})subscribe(new Observer<Drawable>() {
    @override
    public void onNext(Drawable drawable) {
        imageView.setImageDrawable(drawable);
    }
    
    @override
    public void onCompleted() {
        
    }
    
    @override
    public void onError(Throwable e) {
        Toast.makeText(activity, "Error!", Toast.LENGTH_SHORT).show();
    }
};
```



* 通过创建出 `Observable`  和 `Subscriber` ，再用 `subscribe()` 将他们连接起来。一次RxJava基本使用就完成了。



#### 4. 线程控制器—Scheduler

​	线程生产事件和线程消费事件是一一对应的。需要切换线程，需要用到  `Scheduler` （调度器）



#####(1) Scheduler的API

RxJava内置的几种  `Scheduler`

> `Schedulers.immediate()` ：直接在当前线程运行，相当于不指定线程。这是默认的  `Schedule`
>
> `Schedulers.newThread()` ：总是启用新线程，并在新线程执行操作。
>
> `Schedulers.io()` ：I/O操作（读写文件，读写数据库，网络信息交互等）所使用的 `Scheduler` 。行为和 `newThread()` 差不多。`io()` 内部实现使用一个无数量上限的线程池，可以重用空闲的线程，因此多数情况下 `io()` 比 `newThread()` 更有效率。`io()` 里不比放计算工作，防止创建不必要的线程
>
> `Schedulers.computation()` ：计算CPU密集型计算所使用的Scheduler，如图形的计算。不会被I/O等操作限制性能，不应把I/O操作放在 `computation()` ，否则I/O操作的等待时间会浪费CPU
>
> `AndroidSchedulers.mainThread()` ：它指定的操作将在Android主线程运行



* `subscribeOn()` ：指定 `subscribe()` 所发生的线程。`Observable.OnSubscribe`  激活时运行该线程（事件产生的线程）
*  `observeOn()` ：指定 `Subscriber` 所运行在的线程（事件消费者线程）

```java
Observable.just(1, 2, 3, 4)
    .subscribeOn(Schedulers.io())//指定 subscribe() 发生在IO 线程
    .observeOn(AndroidSchedulers.mainThread())// 指定 Subscriber 的回掉发生在主线程
    .subscribe(new Action1<Integer>() {
       @override
        public void call(Integer number) {
            Log.d("TAG", "number: " + number);
        }
    });
```



* 被创建的事件的内容1、2、3、4会在IO线程发出。observeOn(AndroidSchedulers.mainThread())指定打印数字发生在主线程。适用于多数的 `后台线程取数据，主线程显示` 的程序流程



```java
int drawableRes = ...;
ImageView imageView = ...;
Observable.create(new OnSubscribe<Drawable>() {
   @override
    public void call(Subscriber<? super Drawable> subscriber) {
        Drawable drawable = getTheme().getDeawable(drawableRes);
        subscriber.onNext(drawable);
        subscriber.onCompleted();
    }
})
.subscribeOn(Schedulers.io())//指定subscribe()发生在IO线程
.observeOn(AndroidSchedulers.mainThread())//指定 Subscriber1 的回掉发生在主线程
.subscribe(new Observer<Drawable>() {
    @override
    public void onNext(Drawable drawable) {
        imageView.setImageViewDrawable(drawable);
    }
    
    @override
    public void onCompleted() {
        
    }
    
    @override
    public void onError(Throwable e) {
        Toast.makeText(acticity, "Error!", Toast.LENGTH_SHORT).show();
    }
});
```



* 其中加载图片发生在IO线程，图片设置被设定在了主线程。因此图片加载耗费时间过长不会造成界面的卡顿

#### 5. 变换

> 概念：将事件序列中的对象或整个序列进行加工处理，转换成不同的事件或事件序列



```java
Observable.just("images/logo.png") // 输入类型 String 
    .map(new Funcl<String, Bitmap>() {
        @override
        public Bitmap call(String filePath) { //参数类型 String
            return getBitmapFromPath(filePath); // 返回类型Bitmap    
        }
    })
    .subscriber(new Action1<Bitmap>() {
        @override
        public void call(Bitmap bitmap) { // 参数类型Bitmap
            showBitmap(bitmap);
        }
    });
```



> Funcl是RxJava的接口，和Actionl非常相似，用于包装含有一个参数的方法。Funcl和Action的区别在于，Funcl和FuncX都是包装的有返回值的方法，ActionX和FuncX一样，用于表示不同参数个数的方法，



`map()` 事件对象的直接转换（一对一的转换）

​	![map() ç¤ºæå¾](http://ww1.sinaimg.cn/mw1024/52eb2279jw1f2rx4fitvfj20hw0ea0tg.jpg)



`flatMap()` ：是一个很有用但非常难理解的变换（一对多的转换）

​	例1 ：假设有一个数据结构（学生），需要打印出一组学生的名字，实现方式如下

```java
Student[] student = ...;
Subscriber<String> subscriber = new Subscriber<String>() {
    @override
    public void onNext(String name) {
        Log.d("TAG", name);
    }
    ...
};
Observable.from(students)
    .map(new Funcl<String, String>() {
        @override
        public String call(Student student) {
            return student.getName();
        }
    })
    .subscribe(subsceiber);
```

​	

​	例2 ：如果要打印每个学生所需要修的所有课程的名称（即每个学生只有一个名字，但却有多个课程）

```java
Student[] students = ...;
Subscriber<Student> subscriber = new Subscriber<Student>() {
    @override
    public void onNext(Student student) {
        List<Course> courses = student.getCourse();
        for (int i = 0; i < courses.size(); i++) {
            Course course = courses.get(i);
            Log.d("TAG", course.getName());
        }
    }
    ...
};
Observable.from(students) 
    .subscribe(subscriber);
```



​	例3 ：假如不在 `Subscriber` 中使用for循环，而是通过 `Subscriber`直接传入单个 `Course`  对象。此时需要使用 `flatmap()` 

```java
Student students = ...;
Subscriber<Course> subscriber = new Subscriber<Course>() {
    @override
    public void onNext(Course course) {
        Log.d("TAG",course.getName());
    }
    ...
}
Observable.from(students)
    .flatMap(new Funcl<Student, Observale<Course>>() {
       @override
        public Observable<Course> call(Student student) {
            return Observable.from(student.getCourse());
        }
    })
    .subscribe(subscriber);
```



​	`flatMap()` 的原理如下

1. 使用传入的事件对象创建一个 `Observable` 
2. 不发送这个 `Observable` ，将它激活，它会开始发烧事件
3. 每一个创建出来的 `Observable` 发送的事件，都被汇入同一个 `Observable` ，这个 `Observable` 负责将这些事件统一交给 `Subscriber` 的回调方法。



​	![flatMap() ç¤ºæå¾](http://ww1.sinaimg.cn/mw1024/52eb2279jw1f2rx4i8da2j20hg0dydgx.jpg)

* 扩展：嵌套的Observable中可以添加异步代码，`flatMap()` 常用于嵌套的异步操作，例如嵌套的网络请求（Retrofit+RxJava）

```java
networkClient.token() // 返回Observable<String>，在订阅时请求token，并在相应后发送token
    .flatMap(new Funcl<String, Observable<Messages>>() {
       @override
        public Observable<Messages> call(String token) {
            //返回Observable<Message>,在订阅时请求消息列表，并在响应后发送请求到的消息列表
            return networkClient.messages();
        }
    })
    .subscribe(new Action1<Messages>() {
        @override
        public void call(Messages) {
            // 处理显示消息列表
            showMessages(messages);
        }
    });
```



* throttleFirst(): 在每次事件触发后的一定时间间隔内丢弃新的事件。常用作去抖动过滤，例如按钮的点击监听器： RxView.clickEvents(button) // RxBinding 代码，后面的文章有解释 .throttleFirst(500, TimeUnit.MILLISECONDS) // 设置防抖间隔为 500ms .subscribe(subscriber); 



####6. 线程控制：Scheduler线程的自由控制（二）

> RxJava可以切换多次线程，因为 `ObserveOn()` 指定的是Subscriber的线程，