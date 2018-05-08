

###线程的创建



#### 1、使用对象表达式创建线程

```Kotlin
val mythread = object : Thread() {
    override fun run() {
        Thread.sleep(1000)
        println("使用对象表达式创建线程")
    }
}.start()
```



####2、使用Lambda表达式创建线程

```kotlin
Thread({
    Thread.sleep(3000)
    println("使用Lambda表达式创建线程")
}).start()
```



#### 3、使用kotlin封装好的thread线程

* 一般方法创建线程

```kotlin
val t = Thread({
    Thread.sleep(1000)
    println("一般方法创建线程")
})
t.isDaemon = false
t.name = "Thread"
t.priority = 3
t.start()
```



* 经过kotlin封装

```kotlin
thread(start = true, isDeamon = false, name = "Thread", priority = 3){
    Thread.sleep(1000)
    println("经过kotlin封装")
}
```



#### 4、同步方法和代码块

kotlin中的同步方法不是使用关键字synchronized，它被替换为@Synchronized注解

```kotlin
@Synchronized fun appendFile(text : String, destFile : String){
    val f = File(destFile)
    if (!f.exists()) {
        f.creatNewFile()
    }
    f.appendText(text, Charset.defaultCharset())
}
```



#### 5、可变字段

kotlin没有volatile关键字，还是使用@volatile注解



```Kotlin
@Volatile private var running = false
fun start() {
    running = true 
    thread(start = true) {
        while (running) {
            println("start")
        }
    }
}
fun stop() {
    running = false 
    println("stop")
}
```

