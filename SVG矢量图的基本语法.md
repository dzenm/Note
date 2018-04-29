##基本的语法：

- M：move to 移动绘制点，作用相当于把画笔落在哪一点。

- L：line to 直线，就是一条直线，注意，只是直线，直线是没有宽度的，所以你什么也看不到。

  ```
  android:strokeColor="#333330" android:strokeWidth="10" 设置颜色和线宽
  ```

- Z：close 闭合，嗯，就是把图封闭起来。

- C：cubic bezier 三次贝塞尔曲线

- Q：quatratic bezier 二次贝塞尔曲线

- A：ellipse 圆弧

- ​

####每个命令都有大小写形式，大写代表后面的参数是绝对坐标，小写表示相对坐标，相对于上一个点的位置。参数之间用空格或逗号隔开。

命令详解：

- M (x y) 把画笔移动到x,y，要准备在这个地方画图了。

- L (x y) 直线连到x,y，还有简化命令H(x) 水平连接、V(y)垂直连接。

- Z，没有参数，连接起点和终点

- C(x1 y1 x2 y2 x y)，控制点（x1,y1）（ x2,y2），终点x,y 。

- Q(x1 y1 x y)，控制点（x1,y1），终点x,y

- C和Q会在下文做简单对比。

- A(rx ry x-axis-rotation large-arc-flag sweep-flag x y) 

- ```
  android:pathData=" M50,50 a10,10 1,1 0 1,0" />
  rx ry 椭圆半径 
  x-axis-rotation x轴旋转角度 
  large-arc-flag 为0时表示取小弧度，1时取大弧度 （舍取的时候，是要长的还是短的）
  sweep-flag 0取逆时针方向，1取顺时针方向 
  ```

------

 

####L的用法：

```
android:pathData=" M10,0 L10,40 40,40" /> 
把画笔放在（10,0）位置，连线10，40点 在连线40，40点。。。于是，一个直角三角形出来了~这里没有写z，没什么关系。
    
```

 Q和C的对比： 详细了解贝塞尔曲线：

http://www.cnblogs.com/jay-dong/archive/2012/09/26/2704188.html

####Q

< android:pathData="M0,0 q30,90 80,20"/>　　　

控制点1，30,90 ： 
控制点2，80,20  : 

####C

```Java
<android:pathData=" M0,0 c0,0 30,90 80,20" />
```

C 第一控制点(0,0) 第二控制点(30,90) 结束点(80,20) 或 c 第一控制点 第二控制点 结束点



![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160506171337747-1886557208.png)



现在修改第一个控制点：

```
<android:pathData=" M0,0 c50,0 30,90 80,20" />
```



![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160506172407935-1982637910.png)



a: 这么多 数字，怎么看啊，可以直接拉到下面看作用。

```
<android:pathData=" M50,50 a10,5 0,1 0 1,0" />
```

以50，50为起点，逆时针画

椭圆图形，x轴半径10，y轴半径5 



 ![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160506173937435-1750876715.png)

转动x轴~~~

```
<android:pathData=" M50,50 a10,5 90,1 0 1,0" />
```



![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160506174049091-1789699417.png)



我想要椭圆上半段，此处修改为x轴半径的两倍

```
android:pathData=" M50,50 a10,5 90,1 0 20,0" />
```

椭圆左半段

```
<android:pathData=" M50,50 a10,5 90 1 0 0 10" />
```

椭圆右半段

```
<android:pathData=" M50,50 a10,5 90 1 1 0 10" />
```

```
<path
    android:fillColor="#fff70000"  下
       android:pathData=" M50,50 a10,5 0 1 0 1 0" />
    <path
        android:fillColor="#FFF22420" 上
        android:pathData=" M50,50 a10,5 0 1 1 1 0" />
    <path
        android:fillColor="#fff57000"右
        android:pathData=" M50,50 a10,5 0 1 1 1 1" />
    <path
        android:fillColor="#FF323243"左
        android:pathData=" M50,50 a10,5 0 1 0 0 1" />
```



![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160506184459091-505426871.png)



出现上面的情况可以想到是因为，起始点50,50在椭圆中的位置不同。那么，再修改一下。

```
<android:pathData=" M50,50 a10,5 0 1 1 0 7" />  
```

修改了右边椭圆的代码

![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160506185815732-1624446472.png)



现在取的是大弧度，所以看到这样的效果，如果 7改为10（也就是y轴半径的两倍）这刚好会在 一半的位置。现在取小弧度看看

![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160506190215169-343287233.png)

```
android:pathData=" M50,50 a10,5 0 0 1 0 7" /> ，可以看到小弧度 顺时针画图。
```

![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160506190215169-343287233.png)



再修改为逆时针，

```
<android:pathData=" M50,50 a10,5 0 0 0 0 7" /> 
```

![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160506190350669-1079033621.png)

椭圆的属性 差不多讲解完成了，如下

<android:pathData=" M50,50 a10,5 0 0 0 0 7" />

10，5 为椭圆x，y轴半径

第一个0 为 x轴旋转角度

第二个0 为取大小弧度，0为小，1为大

第三个0 为顺逆时针，0为逆1为顺

第四个0  为修改修改起始点在椭圆中的位置，y轴.

第四个 7 为修改修改起始点在椭圆中的位置，x轴。

这是前辈留下的图：

![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160506190808638-1289842152.png)

------

<path>里面还有哪些属性那？

![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160508105502924-1325270077.png)

- **android:name 定义该 path 的名字，这样在其他地方可以通过名字来引用这个路径**
- **android:pathData 和 SVG 中 d 元素一样的路径信息。**
- **android:fillColor 定义填充路径的颜色，如果没有定义则不填充路径**
- **android:strokeColor 定义如何绘制路径边框，如果没有定义则不显示边框**
- **android:strokeWidth 定义路径边框的粗细尺寸**
- **android:strokeAlpha 定义路径边框的透明度**
- **android:fillAlpha 定义填充路径颜色的透明度**
- **android:trimPathStart 从路径起始位置截断路径的比率，取值范围从 0 到1**
- **android:trimPathEnd 从路径结束位置截断路径的比率，取值范围从 0 到1**
- **android:trimPathOffset 设置路径截取的范围 Shift trim region (allows showed region to include the start and end), in the range from 0 to 1.**
- **android:strokeLineCap 设置路径线帽的形状，取值为 butt, round, square.**
- **android:strokeLineJoin 设置路径交界处的连接方式，取值为 miter,round,bevel.**
- **android:strokeMiterLimit 设置斜角的上限，Sets the Miter limit for a stroked path.**

下面详细讲一下 **android:strokeLineCap** **，android:strokeLineJoin 两个属性**

```
<android:pathData="M200,200 l100,300 M300,200 l-100,300
```

再没有添加这两条属性前：

![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160508110331345-1092644604.png)

 

添加语句：android:strokeLineCap="round" 后可以看到有三个点改变了格式（左下角是图形结束点，并没有改变）

![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160508110607330-989412318.png)

最后添加：android:strokeLineJoin="round" 左下角也做了改变，如下

![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160508110732486-627659826.png)

------

这xml开始部分的代码是做什么的那？ 

```
<vector xmlns:android="http://schemas.android.com/apk/res/android"
    android:width="24dp"
    android:height="24dp"
    android:viewportHeight="24.0"
    android:viewportWidth="24.0">
```

先看看有哪些属性，

![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160508110953845-182893206.png)

- **android:name 定义该drawable的名字**
- **android:width 定义该 drawable 的内部(intrinsic)宽度,支持所有 Android 系统支持的尺寸，通常使用 dp**
- **android:height 定义该 drawable 的内部(intrinsic)高度,支持所有 Android 系统支持的尺寸，通常使用 dp**
- **android:viewportWidth 定义矢量图视图的宽度，视图就是矢量图 path 路径数据所绘制的虚拟画布**
- **android:viewportHeight 定义矢量图视图的高度，视图就是矢量图 path 路径数据所绘制的虚拟画布**
- **android:tint 定义该 drawable 的 tint 颜色。默认是没有 tint 颜色的**
- **android:tintMode 定义 tint 颜色的 Porter-Duff blending 模式，默认值为 src_in**
- **android:autoMirrored 设置当系统为 RTL (right-to-left) 布局的时候，是否自动镜像该图片。比如 阿拉伯语。**
- **android:alpha 该图片的透明度属性**

------

<group>里面可以定义多了<path>，这样可以方便管理多个<path>

![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160508111553361-744918828.png)

- **android:name 定义 group 的名字**
- **android:rotation 定义该 group 的路径旋转多少度，这样图片就被旋转了，注意写数字的时候别晕了。**
- **android:pivotX 定义缩放和旋转该 group 时候的 X 参考点。该值相对于 vector 的 viewport 值来指定的。**
- **android:pivotY 定义缩放和旋转该 group 时候的 Y 参考点。该值相对于 vector 的 viewport 值来指定的。**
- **android:scaleX 定义 X 轴的缩放倍数**
- **android:scaleY 定义 Y 轴的缩放倍数**
- **android:translateX 定义移动 X 轴的位移。相对于 vector 的 viewport 值来指定的。**
- **android:translateY 定义移动 Y 轴的位移。相对于 vector 的 viewport 值来指定的。**

------

<clip-path>定义当前绘制的剪切路径，就是图像的一部分剪切下来。注意，clip-path 只对当前的 group 和子 group 有效。

![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160508112052064-534979620.png)

```
<clip-path android:pathData="M200,200 h200 v150 h-200 v-150"/>
```

原图为上面的 叉 ，剪切后为：



![img](https://images2015.cnblogs.com/blog/703717/201605/703717-20160508114428892-920665940.png)