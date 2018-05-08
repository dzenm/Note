####1、单例之懒汉模式，最简单的单例模式，线程不安全，不支持多线程

```
public class SingletonLazy {

    private static SingletonLazy singletonLazy = null;

    private SingletonLazy() {
    }

    public static SingletonLazy getSingletonLazy() {
        return singletonLazy;
    }


    
     *
     * 什么是线程安全？
     *
     * 如果你的代码所在的进程中有多个线程在同时运行，而这些线程可能会同时运行这段代码。如果每次运行结果和单线程运行的结果是一样的，而且其他的变量的值也和预期的是一样的，就是线程安全的。
     * 或者说：一个类或者程序所提供的接口对于线程来说是原子操作，或者多个线程之间的切换不会导致该接口的执行结果存在二义性,也就是说我们不用考虑同步的问题，那就是线程安全的。
     */
}
```



*  而懒汉比较懒，只有当调用getInstance的时候，才回去初始化这个单例
* 懒汉式本身是非线程安全的，为了实现线程安全有几种写法，分别是上面的1、2、3,这三种实现在资源加载和性能方面有些区别。
* 而懒汉式顾名思义，会延迟加载，在第一次使用该单例的时候才会实例化对象出来，第一次调用时要做初始化，如果要做的工作比较多，性能上会有些延迟，之后就和饿汉式一样了。



#### 2、单例之懒汉模式在getInstance方法上加同步，线程是安全的

```
public class SingletonLazySynchronized {

    private static SingletonLazySynchronized singletonLazySynchronized = null;


    private SingletonLazySynchronized() {
    }

    /**
     * 1、在getInstance方法上加同步
     *
     * @return
     */
    public static synchronized SingletonLazySynchronized getSingletonLazySynchronized() {
        if (singletonLazySynchronized == null) {
            singletonLazySynchronized = new SingletonLazySynchronized();
        }
        return singletonLazySynchronized;
    }
}
```

* 第1种，在方法调用上加了同步，虽然线程安全了，但是每次都要同步，会影响性能，毕竟99%的情况下是不需要同步的



#### 3、单例之懒汉模式双重检查锁定，线程是安全的

```
public class SingletonLazyDoubleSynchronized {

    private static SingletonLazyDoubleSynchronized singletonLazyDoubleSynchronized = null;

    private SingletonLazyDoubleSynchronized() {
    }

    /**
     * 2、双重检查锁定
     *
     * @return
     */
    public static SingletonLazyDoubleSynchronized getSingletonLazyDoubleSynchronized() {
        if (singletonLazyDoubleSynchronized == null) {
            synchronized (SingletonLazySynchronized.class) {
                if (singletonLazyDoubleSynchronized == null) {
                    singletonLazyDoubleSynchronized = new SingletonLazyDoubleSynchronized();
                }
            }
        }
        return singletonLazyDoubleSynchronized;
    }
}
```

* 第2种，在getInstance中做了两次null检查，确保了只有第一次调用单例的时候才会做同步，这样也是线程安全的，同时避免了每次都同步的性能损耗



#### 4、单例之懒汉模式静态内部类，线程是安全的，这种比上面2、3都好一些，既实现了线程安全，又避免了同步带来的性能影响。

```
public class SingletonLazyStatic {

    private static class Single {
        private static final SingletonLazyStatic INSTANCE = new SingletonLazyStatic();
    }

    private SingletonLazyStatic() {
    }

    /**
     * 3、静态内部类
     *
     * @return
     */
    public static final SingletonLazyStatic getInstance() {
        return Single.INSTANCE;
    }
}
```

* 第3种，利用了classloader的机制来保证初始化instance时只有一个线程，所以也是线程安全的，同时没有性能损耗，所以一般我倾向于使用这一种。



#### 5、单例之饿汉模式静态内部类，饿汉式单例类.在类初始化时，已经自行实例化

```
public class SingletonHungry {

    private static final SingletonHungry singletonHungry = new SingletonHungry();

    private SingletonHungry() {
    }


    /**
     * 饿汉式在类创建的同时就已经创建好一个静态的对象供系统使用，以后不再改变，所以天生是线程安全的。
     * 静态内部类
     *
     * @return
     */
    public static SingletonHungry getSingletonHungry() {
        return singletonHungry;
    }
}
```



* 饿汉就是类一旦加载，就把单例初始化完成，保证getInstance的时候，单例是已经存在的了
* 饿汉式天生就是线程安全的，可以直接用于多线程而不会出现问题
* 饿汉式在类创建的同时就实例化一个静态对象出来，不管之后会不会使用这个单例，都会占据一定的内存，但是相应的，在第一次调用时速度也会更快，因为其资源已经初始化完成，