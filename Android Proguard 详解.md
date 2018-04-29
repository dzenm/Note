**1、原理**
Java 是一种跨平台的、解释型语言，Java 源代码编译成中间”字节码”存储于 class 文件中。由于跨平台的需要，Java 字节码中包括了很多源代码信息，如变量名、方法名，并且通过这些名称来访问变量和方法，这些符号带有许多语义信息，很容易被反编译成 Java 源代码。为了防止这种现象，我们可以使用 Java 混淆器对 Java 字节码进行混淆。
混淆就是对发布出去的程序进行重新组织和处理，使得处理后的代码与处理前代码完成相同的功能，而混淆后的代码很难被反编译，即使反编译成功也很难得出程序 的真正语义。被混淆过的程序代码，仍然遵照原来的档案格式和指令集，执行结果也与混淆前一样，只是混淆器将代码中的所有变量、函数、类的名称变为简短的英 文字母代号，在缺乏相应的函数名和程序注释的况下，即使被反编译，也将难以阅读。同时混淆是不可逆的，在混淆的过程中一些不影响正常运行的信息将永久丢 失，这些信息的丢失使程序变得更加难以理解。
混淆器的作用不仅仅是保护代码，它也有精简编译后程序大小的作用。由于以上介绍的缩短变量和函数名以及丢失部分信息的原因， 编译后 jar 文件体积大约能减少25% ，这对当前费用较贵的无线网络传输是有一定意义的。



**2、语法**

- 输入输出选项
  - -include filename从给定的文件中读取配置参数
  - -injars class_path 
    输入（即使用的）jar文件路径
  - -outjars class_path 
    输出jar路径
  - -libraryjars class_path 
    指定的jar将不被混淆
  - -skipnonpubliclibraryclasses 
    跳过（不混淆）jars中的非公开课程
  - -dontskipnonpubliclibraryclasses 
    不跳过（混淆）jars中的非公开课默认选项
  - -dontskipnonpubliclibraryclassmembers 
    不跳过jars中的非公共类的成员
  - -keepdirectories [directory_filter] 
    指定目录keep in out jars中
- 保持不变的选项（混淆不进行处理的内容）
  - -keep {Modifier} {class_specification} 
    保护指定的类文件和类的成员
  - -keepclassmembers {modifier} {class_specification} 
    保护指定类的成员，如果此类受到保护他们会保护的更好
  - -keepclasseswithmembers {class_specification}保护指定的类和类的成员，但条件是所有指定的类和类成员是要存在。
  - -keepnames {class_specification}保护指定的类和类的成员的名称（如果他们不会压缩步骤中删除）
  - -keepclassmembernames {class_specification}保护指定的类的成员的名称（如果他们不会压缩步骤中删除）
  - -keepclasseswithmembernames {class_specification}保护指定的类和类的成员的名称，如果所有指定的类成员出席（在压缩步骤之后）
  - -printseeds {filename}列出类和类的成员 - 保存选项的清单，标准输出到给定的文件
- 压缩选项
  - -dontshrink 
    不启用shrink.shrink操作默认启用，主要的作用是将一些无效代码给移除，即没有被显示调用的代码。
  - -printusage [文件名] 
    打印被移除的代码，在标准输出
  - -whyareyoukeeping class_specification 
    打印在收缩过程中为什么有些代码被保留
- 优化选项
  - -dontoptimize 
    该选项表示不启用.optimization，默认启用
    当不使用该选项时，下面的才有效
  - -optimizations optimization_filter 
    根据optimization_filter指定要优化的文件
  - 优化通过n 
    优化数量n
  - -assumenosideeffects class_specification 
    优化时允许访问并修改类和类的成员的访问修饰符，可能作用域会变大。
  - -mergeinterfacesaggressively 
    合并接口，即使它们的实现类未实现合并后接口的所有方法。
- 混淆选项
  - -dontobfuscate 
    不混淆
  - -printmapping [文件名] 
    打印映射旧名到新名
  - -applymapping filename 
    打印相关
  - -obfuscationdictionary文件名
    指定外部模糊字典
  - -classobfuscationdictionary filename 
    指定class模糊字典
  - -packageobfuscationdictionary filename 指定package模糊字典
  - -overloadaggressively 
    过度加载，多个属性和方法使用相同的名字，只是参数和返回类型不同可能各种异常
  - -useuniqueclassmembernames 
    类和类成员都使用唯一的名字
  - -dontusemixedcaseclassnames 
    不使用大小写混合类名
  - -keeppackagenames [package_filter] 
    保持packagename不混淆
  - -flattenpackagehierarchy [package_name] 
    指定重新打包，所有包重命名，这个选项会进一步模糊包名好东西将包里的类混淆成n个再重新打包到一个个的包中，注：混淆是有用，但是我用的时候安装会崩溃，不知道为什么？
  - -repackageclasses [package_name] 
    将包里的类混淆成n个再重新打包到一个统一的包中会覆盖flattenpackagehierarchy选项
  - -keepattributes [attribute_filter] 
    混合时可能被移除下面这些东西，如果想保留，需要使用该项。“Annotation，Exceptions，Signature，Deprecated，SourceFile，SourceDir，LineNumberTable”
- 预校验选项
  - -dontpreverify 
    不预校验，默认选项
- 通用选项
  - -verbose 
    打印日志
  - -dontnote [class_filter] 
    不打印某些错误
  - -dontwarn [class_filter] 
    不打印警告信息
  - -ignorewarnings 
    忽略警告，继续执行
  - -printconfiguration [文件名] 
    打印配置文件
  - -dump [文件名] 
    指定打印类结构



---



**demo 实例**



```java
-ignorewarnings	# 忽略警告，避免打包时某些警告出现
-optimizationpasses 5	# 指定代码的压缩级别
-dontusemixedcaseclassnames	# 是否使用大小写混合
-dontskipnonpubliclibraryclasses	# 是否混淆第三方jar
-dontpreverify                      # 混淆时是否做预校验
-verbose                            # 混淆时是否记录日志
-optimizations !code/simplification/arithmetic,!field/*,!class/merging/*        # 混淆时所采用的算法

-libraryjars   libs/treecore.jar

-dontwarn android.support.v4.**     #缺省proguard 会检查每一个引用是否正确，但是第三方库里面往往有些不会用到的类，没有正确引用。如果不配置的话，系统就会报错。
-dontwarn android.os.**
-keep class android.support.v4.** { *; } # 保持哪些类不被混淆
-keep class com.baidu.** { *; }  
-keep class vi.com.gdi.bgl.android.**{*;}
-keep class android.os.**{*;}

-keep interface android.support.v4.app.** { *; }  
-keep public class * extends android.support.v4.**  
-keep public class * extends android.app.Fragment

-keep public class * extends android.app.Activity
-keep public class * extends android.app.Application
-keep public class * extends android.app.Service
-keep public class * extends android.content.BroadcastReceiver
-keep public class * extends android.content.ContentProvider
-keep public class * extends android.support.v4.widget
-keep public class * extends com.sqlcrypt.database
-keep public class * extends com.sqlcrypt.database.sqlite
-keep public class * extends com.treecore.**
-keep public class * extends de.greenrobot.dao.**

-keepclasseswithmembernames class * {	# 保持 native 方法不被混淆
    native <methods>;
}

-keepclasseswithmembers class * {	# 保持自定义控件类不被混淆
    public <init>(android.content.Context, android.util.AttributeSet);
}

-keepclasseswithmembers class * {	# 保持自定义控件类不被混淆
    public <init>(android.content.Context, android.util.AttributeSet, int);
}

-keepclassmembers class * extends android.app.Activity { //保持类成员
   public void *(android.view.View);
}

-keepclassmembers enum * {	# 保持枚举 enum 类不被混淆
    public static **[] values();
    public static ** valueOf(java.lang.String);
}

-keep class * implements android.os.Parcelable {	# 保持 Parcelable 不被混淆
  public static final android.os.Parcelable$Creator *;
}


-keep class MyClass;                              # 保持自己定义的类不被混淆
```





---



   3**、不能混淆的代码**

​	顾名思义，不能混淆代码，否则会出错。

​	1、放射的地方
	2、系统接口
	3、Jni接口
	4、  android.app.backup.BackupAgentHelper
		android.preference.Preference
		com.android.vending.licensing.ILicensingService
		……





---



   **5、bug（常见错误）**



* Proguard returned with error code 1. See console

1、更新proguard版本
2、android-support-v4 不进行混淆

3、添加缺少相应的库



* 使用gson包解析数据时，出现missing type parameter异常

1、在 proguard.cfg中添加

-dontobfuscate
-dontoptimize
2、在 proguard.cfg中添加

 removes such information by default, so configure it to keep all of it.

-keepattributes Signature

Gson specific classes

 -keep class sun.misc.Unsafe { *; }

-keep class com.google.gson.stream.** { *; }

Application classes that will be serialized/deserialized over Gson

-keep class com.google.gson.examples.android.model.** { *; }

3、类型转换错误
-keepattributes Signature

4、空指针异常
混淆过滤掉相关类与方法

5、安卓代码混淆与反射冲突，地图无法显示等问题解决及反编译方法，安卓反编译
此前的代码混淆，因为并没有用到反射，所以常规的代码混淆方式一遍就能通过，而此项目中某些类利用到了反射机制(本人的这个项目中有即时通讯功能，所以有 表情类资源，因此需要通过反射由文件名找到表情资源id)，当由文件名去寻找资源id时就报空指针异常了，期初我并不知道什么原因，通过反编译已经混淆的 apk，一步一步寻找到出错的地方，才恍然大悟，正是反射那一步出现了问题：Field field = R.drawable.class.getDeclaredField(name);走到这一步就挂了，程序直接崩溃。



* 解决办法：

1.在proguard.cfg文件中，将反射用到的类中的变量不被混淆：
如：-keep public class com.byl.bean.Expressions { *; }，表示Expressions 这个类及类中的所有变量及方法不被混淆，注意要写全路径；
2.过滤泛型：-keepattributes Signature
3.最重要的一点：保持R文件不被混淆，否则，你的反射是获取不到资源id的：-keep class **.R$* {*;}



* 补充一下：上个问题解决后，接下来又遇到了一个问题就是混淆后，地图无法正常使用了，博主使用的是百度地图，在proguard.cfg也已经写明了：

-keep class com.baidu.**   {*;}
-keep class vi.com.**   {*;}



* 6、android.provider.Settings$Global

Project target.

target=android-19



7、java.lang.reflect.UndeclaredThrowableException
-keep interface com.dev.impl.**

8、内存溢出和无法写入堆栈
javaOptions in proguard := Seq(…)
or
javaOptions in (Proguard, proguard) := Seq(…)

9、Error: Unable to access jarfile ..\lib\proguard.jar
路径问题

10、java.lang.NoSuchMethodError
没有相关方法，方法被混淆了，混淆过滤掉相关方法便可。

11、专业网站bug解决方法
http://proguard.sourceforge.net/index.html#manual/troubleshooting.html