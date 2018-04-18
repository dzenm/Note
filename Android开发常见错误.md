####1、java.lang.UnsatisfiedLinkError

##### 产生原因：

引用了多方的sdk的so库之后，不同机型之间就会发生这样一个错误：[Java](http://lib.csdn.net/base/java).lang.UnsatisfiedLinkError，这是由于程序运行的时候未获取到争取的so库包产生的一个错误：你原先的项目中只使用了A公司提供的so包，他只提供了armeabi这个[架构](http://lib.csdn.net/base/architecture)的so包，后来项目需要又引用了B公司的提供的sdk，里面提供的so包还挺全的，arm64-v8a，armeabi-v7a，mips，mips64，x86等等，结果你就全放进去了， 后来发现突然某手机就出现了崩溃，然后一般都是因为这个问题Java.lang.UnsatisfiedLinkError。



##### 解决方法:

1.引入多种 so 库，如果只需要其中的几种，可以在gradle中添加下面的配置（比如）：

```
android {  
    defaultConfig {  
        ndk {  
            abiFilters "x86", "armeabi-v7a"  
        }  
    }  
    buildTypes {  
     
}  
```

如果照上面的配置，那打包时就只会把 "x86", "armeabi-v7a" 这两个目录及其下的 so 文件打包进 apk 。我们的问题也解决了，因为系统也只能找这两个目录下的 so 文件了。

同时需要在 gradle.properties 中添加：

```Java
android.useDeprecatedNdk = true  
```



​		