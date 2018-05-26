##一、View的基础知识

###1、分类

​	单一视图：一个View，如TextView

​	视图组：多个View组成的ViewGroup，如LinearLayout



### 2、View类简介

* View类是Android中各组件的基类，Android中的UI组件由View、ViewGroup组成
* View有4个构造函数，自定义View必须重写至少一个构造函数

```
// 如果View是在Java代码里面new的，则调用第一个构造函数
 public CarsonView(Context context) {
        super(context);
 }

// 如果View是在.xml里声明的，则调用第二个构造函数
// 自定义属性是从AttributeSet参数传进来的
    public  CarsonView(Context context, AttributeSet attrs) {
        super(context, attrs);
 }

// 不会自动调用
// 一般是在第二个构造函数里主动调用
// 如View有style属性时
    public  CarsonView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
 }

    //API21之后才使用
    // 不会自动调用
    // 一般是在第二个构造函数里主动调用
    // 如View有style属性时
    public  CarsonView(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
 }
```



### 3、Android的坐标体系

- 屏幕的左上角为坐标原点
- 向右为x轴增大方向
- 向下为y轴增大方向



![img](https://upload-images.jianshu.io/upload_images/944365-ee0cd39fd788e293.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/336)



* View的坐标



* ![img](https://upload-images.jianshu.io/upload_images/944365-398c610a464cbdc8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/437)



4个顶点的位置描述分别由4个值决定：
 （请记住：**View的位置是相对于父控件而言的**）

- Top：子View上边界到父view上边界的距离
- Left：子View左边界到父view左边界的距离
- Bottom：子View下边距到父View上边界的距离
- Right：子View右边界到父view左边界的距离

 如下图：



![img](https://upload-images.jianshu.io/upload_images/944365-2fb2682c45d05ff9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/437)

 

 

### 4、位置获取方式

#####view的位置获取

 ```
// 获取Top位置
public final int getTop() {  
    return mTop;  
}  

// 其余如下：
  getLeft();      //获取子View左上角距父View左侧的距离
  getBottom();    //获取子View右下角距父View顶部的距离
  getRight();     //获取子View右下角距父View左侧的距离
 ```

#### MotionEvent中get()和getRaw()的区别

```
//get() ：触摸点相对于其所在组件坐标系的坐标
 event.getX();       
 event.getY();

//getRaw() ：触摸点相对于屏幕默认坐标系的坐标
 event.getRawX();    
 event.getRawY();
```



### 五、使用颜色

```
//java中使用Color类定义颜色
int color = Color.GRAY;     //灰色

//Color类是使用ARGB值进行表示
int color = Color.argb(127, 255, 0, 0);   //半透明红色
int color = 0xaaff0000;                   //带有透明度的红色
```



* 注意：

> ```
> #f00            //低精度 - 不带透明通道红色
> #af00           //低精度 - 带透明通道红色
> #ff0000         //高精度 - 不带透明通道红色
> #aaff0000       //高精度 - 带透明通道红色
> ```







## 二、Canvas类的使用



![img](https://upload-images.jianshu.io/upload_images/944365-f91b9bb80a2239a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)





### 1、简介

* 定义：画布是一种绘制时的规则，Android平台2D图形绘制的基础
* 作用：规定绘制内容时的规则和内容，





### 2、Canvas基础用法

* 使用步骤：
  * **步骤1**：创建一个画笔对象 
  * **步骤2**：画笔设置，即设置绘制内容的具体效果（如颜色、大小等等） 
  * **步骤3**：初始化画笔（尽量选择在View的构造函数）

1.  创建画笔

> Paint paint = new Paint();		

2. 设置画笔颜色

> paint.setColor(int color);		

3. 设置画笔模式

> paint.setStyle(Style style);
>
> // Style有三种类型
>
> // 类型1: Paint.Style.FILLANDSTROKE（描边+填充）
>
> // 类型2: Paint.Style.FILL（只填充不描边）
>
> // 类型3: Paint.Style.STROKE  (只描边不填充)

4. 设置画笔的粗细

> Paint.setStrokeWidth(float width)

5. 获取画笔颜色

> paint.getColor()

6. 设置Shader(即着色器，定义图形的着色、外观)

> paint.setShader(Shader shader)

7. 设置画笔的a，r，g，b值

> paint .setARGB(int a, int r, int g, int b)

8. 设置透明度

> paint.setAlpha(int a)

9. 获取画笔的Alpha值

> paint.getAlpha()

10. 设置字体大小颜色

> paint.setTextSize(float textSize)
>
> // 文字Style的三种模式
>
> paint.setStyle(Style style)
>
> // 类型1: Paint.Style.FILLANDSTROKE（描边+填充）
>
> // 类型2: Paint.Style.FILL（只填充不描边）
>
> // 类型3: Paint.Style.STROKE  (只描边不填充)

11. 设置对齐方式

> paint.setTextAlign()
>
> // LEFT：左对齐
>
> // CENTER：居中对齐
>
> // RIGHT：右对齐

12. 设置文本的下划线

> paint.setUnderlineText(boolean underlineText)

13. 设置文本的下划线

> paint.setStrikeThruText(boolean strikeThruText)

14. 设置文本粗体

> paint.setFakeBoldText(boolean fakeBoldText)

15. 设置斜体

> paint.setTextSkewX(-0.5f)

16. 设置文字阴影

> paint.setShadowLayer(5,5,5,Color.YELLOW)





### 三、关闭硬件加速

> ​	在Android4.0的设备上，在打开硬件加速的情况下，使用自定义View可能会出现问题

* 具体关闭方式如下：在AndroidMenifest.xml的application节点添加

```xml
android:hardwareAccelerated="false"
```





### 四、Canvas对象创建和获取

* Canvas对象和获取的方法有4个

```java
// 方法1
// 利用空构造方法直接创建对象
Canvas canvas = new Canvas()；

// 方法2
// 通过传入装载画布Bitmap对象创建Canvas对象
// CBitmap上存储所有绘制在Canvas的信息
Canvas canvas = new Canvas(bitmap)

// 方法3
// 通过重写View.onDraw（）创建Canvas对象
// 在该方法里可以获得这个View对应的Canvas对象

@Override
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    //在这里获取Canvas对象
}

// 方法4
// 在SurfaceView里画图时创建Canvas对象

SurfaceView surfaceView = new SurfaceView(this);
// 从SurfaceView的surfaceHolder里锁定获取Canvas
SurfaceHolder surfaceHolder = surfaceView.getHolder();
//获取Canvas
Canvas c = surfaceHolder.lockCanvas();
        
// ...（进行Canvas操作）
// Canvas操作结束之后解锁并执行Canvas
surfaceHolder.unlockCanvasAndPost(c);
```



**官方推荐方法4来创建并获取Canvas**，原因：

- SurfaceView里有一条线程是专门用于画图，所以方法4的**画图性能最好，并适用于高质量的、刷新频率高的图形**
- 而方法3刷新频率低于方法3，**但系统花销小，节省资源**





![img](https://upload-images.jianshu.io/upload_images/944365-ff8cc50eb32128a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)



#### 一、绘制颜色

```
// 传入一个Color类的常量参数来设置画布颜色
// 绘制蓝色
canvas.drawColor(Color.BLUE);
```



​	![img](https://upload-images.jianshu.io/upload_images/944365-6664b385824de539.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/280)



### 二、绘制基本图形

1. 绘制点(drawPoint)

```java
// 特别注意：需要用到画笔Paint
// 所以之前记得创建画笔
// 为了区分，这里使用了两个不同颜色的画笔

// 描绘一个点
// 在坐标(200,200)处
canvas.drawPoint(300, 300, mPaint1);    

// 绘制一组点，坐标位置由float数组指定
// 此处画了3个点，位置分别是：（600,500）、（600,600）、（600,700）
canvas.drawPoints(new float[]{         
                600,500,
                600,600,
                600,700
        },mPaint2);
```



​	![img](https://upload-images.jianshu.io/upload_images/944365-efd689c40b73cf9b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/366)



2. 绘制直线(drawLine)

> 两点确定一条直线



```java
// 画一条直线
// 在坐标(100,200)，(700,200)之间绘制一条直线
   canvas.drawLine(100,200,700,200,mPaint1);

// 绘制一组线
// 在坐标(400,500)，(500,500)之间绘制直线1
// 在坐标(400,600)，(500,600)之间绘制直线2
        canvas.drawLines(new float[]{
                400,500,500,500,
                400,600,500,600
        },mPaint2);
    }
```



​	![img](https://upload-images.jianshu.io/upload_images/944365-31d3be148e1d0069.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/370)



3. 绘制矩形(drawRect)

> 矩形的对角线顶点确定一个矩形，一般是采用左上角和右下角的两个点的坐标。



```java
// 关于绘制矩形，Canvas提供了三种重载方法

// 方法1：直接传入两个顶点的坐标
// 两个顶点坐标分别是：（100,100），（800,400）
canvas.drawRect(100,100,800,400,mPaint);

// 方法2：将两个顶点坐标封装为RectRectF
Rect rect = new Rect(100,100,800,400);
canvas.drawRect(rect,mPaint);

// 方法3：将两个顶点坐标封装为RectF
RectF rectF = new RectF(100,100,800,400);
canvas.drawRect(rectF,mPaint);

// 特别注意：Rect类和RectF类的区别
// 精度不同：Rect = int & RectF = float
// 三种方法画出来的效果是一样的。
```



​	![img](https://upload-images.jianshu.io/upload_images/944365-6423d2e5278530df.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/370)



4. 绘制圆角矩形

> 矩形的对角线顶点确定一个矩形



```java
// 方法1：直接传入两个顶点的坐标
// API21时才可使用
// 第5、6个参数：rx、ry是圆角的参数，下面会详细描述
       canvas.drawRoundRect(100,100,800,400,30,30,mPaint);
      
// 方法2：使用RectF类
RectF rectF = new RectF(100,100,800,400);
canvas.drawRoundRect(rectF,30,30,mPaint);
```



​	![img](https://upload-images.jianshu.io/upload_images/944365-08bc785093daef5a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/360)



- 与矩形相比，圆角矩形多了两个参数rx 和 ry

- 圆角矩形的角是椭圆的圆弧，rx 和 ry实际上是椭圆的两个半径

- 特别注意：当 rx大于宽度的一半， ry大于高度一半 时，画出来的为椭圆

- 实际上，在rx为宽度的一半，ry为高度的一半时，刚好是一个椭圆；但由于当rx大于宽度一半，ry大于高度一半时，无法计算出圆弧，所以drawRoundRect对大于该数值的参数进行了修正，**凡是大于一半的参数均按照一半来处理**

  
  ​	

  ![img](https://upload-images.jianshu.io/upload_images/944365-a2d54cc88fac1fcb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/570)

5. 绘制椭圆

> 矩形的对角线顶点确定矩形，根据传入矩形的长宽作为长轴和短轴画椭圆，椭圆传入的参数和矩形是一样的，绘制椭圆实际上是绘制一个矩形的内切图形



```java
// 方法1：使用RectF类
RectF rectF = new RectF(100,100,800,400);
canvas.drawOval(rectF,mPaint);

// 方法2：直接传入与矩形相关的参数
canvas.drawOval(100,100,800,400,mPaint);

// 为了方便表示，画一个和椭圆一样参数的矩形
canvas.drawRect(100,100,800,400,mPaint);
```



​	![img](https://upload-images.jianshu.io/upload_images/944365-4c4404e53ab8e052.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/374)



6. 绘制圆

> - 圆心坐标+半径决定圆



```java
// 参数说明：
// 1、2：圆心坐标
// 3：半径
// 4：画笔

// 绘制一个圆心坐标在(500,500)，半径为400 的圆。
canvas.drawCircle(500,500,400,mPaint); 
```



​	![img](https://upload-images.jianshu.io/upload_images/944365-0f27a18b3343b160.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/371)



7. 绘制圆弧

> 通过圆弧角度的起始位置和扫过的角度确定圆弧



```java
// 绘制圆弧共有两个方法
// 相比于绘制椭圆，绘制圆弧多了三个参数：
startAngle  // 确定角度的起始位置
sweepAngle // 确定扫过的角度
useCenter   // 是否使用中心（下面会详细说明）

// 方法1
public void drawArc(@NonNull RectF oval, float startAngle, float sweepAngle, boolean useCenter, @NonNull Paint paint){}

// 方法2
public void drawArc(float left, float top, float right, float bottom, float startAngle,
            float sweepAngle, boolean useCenter, @NonNull Paint paint) {}
```



为了理解第三个参数：`useCenter`，看以下示例：

```
// 以下示例：绘制两个起始角度为0度、扫过90度的圆弧
// 两者的唯一区别就是是否使用了中心点

// 绘制圆弧1(无使用中心)
RectF rectF = new RectF(100, 100, 800,400);
// 绘制背景矩形
canvas.drawRect(rectF, mPaint1);
// 绘制圆弧
canvas.drawArc(rectF, 0, 90, false, mPaint2);

// 绘制圆弧2(使用中心)
RectF rectF2 = new RectF(100,600,800,900);
// 绘制背景矩形
canvas.drawRect(rectF2, mPaint1);
// 绘制圆弧
canvas.drawArc(rectF2,0,90,true,mPaint2);
```



​	![img](https://upload-images.jianshu.io/upload_images/944365-bb94a463cd2f5c27.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/371)



- 不使用中心点：圆弧的形状 = （起、止点连线+圆弧）构成的面积
- 使用中心店：圆弧面积 = （起点、圆心连线 + 止点、圆心连线+圆弧）构成的面



### 三、绘制文字



- 情况1：指定文本开始的位置



> 1. 即指定文本基线位置
> 2. 基线x默认在字符串左侧，基线y默认在字符串下方



```java
// 参数text：要绘制的文本
// 参数x，y：指定文本开始的位置（坐标）

// 参数paint：设置的画笔属性
    public void drawText (String text, float x, float y, Paint paint)

// 实例
canvas.drawText("abcdefg",300,400,mPaint1);



// 仅绘制文本的一部分
// 参数start，end：指定绘制文本的位置
// 位置以下标标识，由0开始
    public void drawText (String text, int start, int end, float x, float y, Paint paint)
    public void drawText (CharSequence text, int start, int end, float x, float y, Paint paint)

// 对于字符数组char[]
// 截取文本使用起始位置(index)和长度(count)
    public void drawText (char[] text, int index, int count, float x, float y, Paint paint)
// 实例：绘制从位置1-3的文本
		canvas.drawText("abcdefg",1,4,300,400,mPaint1);

        // 字符数组情况
        // 字符数组(要绘制的内容)
        char[] chars = "abcdefg".toCharArray();

        // 参数为 (字符数组 起始坐标 截取长度 基线x 基线y 画笔)
        canvas.drawText(chars,1,3,200,500,textPaint);
        // 效果同上
```

​	![img](https://upload-images.jianshu.io/upload_images/944365-a4994282812d548e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)



- 情况2：指定每个文字的位置



```java
// 参数text：绘制的文本
// 参数pos：数组类型，存放每个字符的位置（坐标）
// 注意：必须指定所有字符位置
 public void drawPosText (String text, float[] pos, Paint paint)

// 对于字符数组char[],可以截取部分文本进行绘制
// 截取文本使用起始位置(index)和长度(count)
 public void drawPosText (char[] text, int index, int count, float[] pos, Paint paint)

// 特别注意：
// 1. 在字符数量较多时，使用会导致卡顿
// 2. 不支持emoji等特殊字符，不支持字形组合与分解

  // 实例
 canvas.drawPosText("abcde", new float[]{
                100, 100,    // 第一个字符位置
                200, 200,    // 第二个字符位置
                300, 300,    // ...
                400, 400,
                500, 500
        }, mPaint1)；




// 数组情况（绘制部分文本）
 char[] chars = "abcdefg".toCharArray();

 canvas.drawPosText(chars, 1, 3, new float[]{
                300, 300,    // 指定的第一个字符位置
                400, 400,    // 指定的第二个字符位置
                500, 500,    // 指定的第三个字符位置

        }, mPaint1);
```



![img](https://upload-images.jianshu.io/upload_images/944365-919c352d2548397a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)



- 情况3：指定路径，并根据路径绘制文字

```java
// 在路径(540,750,640,450,840,600)写上"在Path上写的字:Carson_Ho"字样
        // 1.创建路径对象
        Path path = new Path();
        // 2. 设置路径轨迹
        path.cubicTo(540, 750, 640, 450, 840, 600);
         // 3. 画路径
        canvas.drawPath(path,mPaint2);
        // 4. 画出在路径上的字
        canvas.drawTextOnPath("在Path上写的字:Carson_Ho", path, 50, 0, mPaint2);
```



![img](https://upload-images.jianshu.io/upload_images/944365-298dfa377797653d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/378)





### 四、绘制图片

>绘制图片分为：绘制矢量图（drawPicture）和 绘制位图（drawBitmap)



#### 1、绘制矢量图（drawPicture）

> * 矢量图（Picture）的作用：存储（录制）某个时刻Canvas绘制内容的操作
>
> 1. 相比于再次调用各种绘图API，使用Picture能节省操作 & 时间
> 2. 如果不手动调用，录制的内容不会显示在屏幕上，只是存储起来
> 3. **特别注意：使用绘制矢量图时前请关闭硬件加速，以免引起不必要的问题！**

 

 ```java
// 获取宽度
Picture.getWidth ()；

// 获取高度
Picture.getHeight ()

// 开始录制 
// 即将Canvas中所有的绘制内容存储到Picture中
// 返回一个Canvas
Picture.beginRecording（int width, int height）

// 结束录制
Picture.endRecording ()

// 将Picture里的内容绘制到Canvas中
Picture.draw (Canvas canvas)

// 还有两种方法可以将Picture里的内容绘制到Canvas中
// 方法2：Canvas.drawPicture（）
// 方法3：将Picture包装成为PictureDrawable，使用PictureDrawable的draw方法绘制。
 ```



> // 步骤1：创建Picture对象 Picture mPicture = new Picture();  
>
> // 步骤2：开始录制  mPicture.beginRecording（int width, int height）; 
>
>  // 步骤3：绘制内容 or 操作Canvas canvas.drawCircle(500,500,400,mPaint); ...（一系列操作） 
>
>  // 步骤4：结束录制 mPicture.endRecording ();  
>
>  // 步骤5：某个时刻将存储在Picture的绘制内容绘制出来 mPicture.draw (Canvas canvas);





- 实例介绍
  将坐标系移动到(450,650)；绘制一个圆，将上述Canvas操作录制下来，并在某个时刻重新绘制出来。

 ```java
// 步骤1：创建Picture对象
Picture mPicture = new Picture();

// 步骤2：开始录制
Canvas recordingCanvas = mPicture.beginRecording(500, 500);
// 注：要创建Canvas对象来接收beginRecording()返回的Canvas对象

// 步骤3：绘制内容 or 操作Canvas
// 位移
// 将坐标系的原点移动到(450,650)
recordingCanvas.translate(450,650);

// 记得先创建一个画笔
Paint paint = new Paint();
paint.setColor(Color.BLUE);        paint.setStyle(Paint.Style.FILL);
// 绘制一个圆
// 圆心为（0，0），半径为100 
recordingCanvas.drawCircle(0,0,100,paint);

// 步骤4：结束录制
mPicture.endRecording();

// 步骤5：将存储在Picture的绘制内容绘制出来
有三种方法：
Picture.draw (Canvas canvas)
Canvas.drawPicture（）
PictureDrawable.draw（）
// 将Picture包装成为PictureDrawable

将Picture包装成为PictureDrawable
 ```





###2、绘制位图（drawBitmap）



> 作用：将已有的图片转换为位图（Bitmap），最后再绘制到Canvas上



#### (1) 获取Bitmap对象的方式

要绘制Bitmap，就要先获取一个Bitmap对象，具体获取方式如下：



![img](https://upload-images.jianshu.io/upload_images/944365-471a8ae2ba6ab7a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/540)



*  特别注意：**绘制位图（Bitmap）是**读取已有的图片**转换为Bitmap，最后再绘制到Canvas。

 

- 对于第1种方式：排除
- 对于第2种方式：虽然满足需求，但一般不推荐使用

- 对于第3种方式：满足需求，下面会着重讲解

 ```
// 共3个位置：资源文件、内存卡、网络

// 位置1：资源文件(drawable/mipmap/raw)
Bitmap bitmap =BitmapFactory.decodeResource(mContext.getResources(),R.raw.bitmap);

// 位置2：资源文件(assets)
		Bitmap bitmap=null;
        try {
            InputStream is = mContext.getAssets().open("bitmap.png");
            bitmap = BitmapFactory.decodeStream(is);
            is.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

// 位置3：内存卡文件
    Bitmap bitmap = BitmapFactory.decodeFile("/sdcard/bitmap.png");

// 位置4：网络文件:
// 省略了获取网络输入流的代码
        Bitmap bitmap = BitmapFactory.decodeStream(is);
        is.close();
 ```



#### (2) 绘制Bitmap

绘制Bitmap共有四种方法：

```
// 方法1
    public void drawBitmap (Bitmap bitmap, Matrix matrix, Paint paint)

 // 方法2
    public void drawBitmap (Bitmap bitmap, float left, float top, Paint paint)

// 方法3
    public void drawBitmap (Bitmap bitmap, Rect src, Rect dst, Paint paint)

// 方法4
    public void drawBitmap (Bitmap bitmap, Rect src, RectF dst, Paint paint)
```



**方法1**

 ```java
public void drawBitmap (Bitmap bitmap, Matrix matrix, Paint paint)

// 后两个参数matrix, paint是在绘制时对图片进行一些改变
// 后面会专门说matrix
  
// 如果只是将图片内容绘制出来只需将传入新建的matrix, paint对象即可：
  canvas.drawBitmap(bitmap,new Matrix(),new Paint());
// 记得选取一种获取Bitmap的方式
// 注：图片左上角位置默认为坐标原点。
 ```



![img](https://upload-images.jianshu.io/upload_images/944365-e0eb1fdd7f79e987.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/426)



**方法2**

```
// 参数 left、top指定了图片左上角的坐标(距离坐标原点的距离)：
public void drawBitmap (Bitmap bitmap, float left, float top, Paint paint)

canvas.drawBitmap(bitmap,300,400,new Paint());
```



![img](https://upload-images.jianshu.io/upload_images/944365-69bfe74957add3b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/426)



**方法3**

```java
public void drawBitmap (Bitmap bitmap, Rect src, Rect dst, Paint paint)
// 参数（src，dst） = 两个矩形区域
// Rect src：指定需要绘制图片的区域（即要绘制图片的哪一部分）
// Rect dst 或RectF dst：指定图片在屏幕上显示(绘制)的区域
// 下面我将用实例来说明

// 实例
// 指定图片绘制区域
// 仅绘制图片的二分之一
Rect src = new Rect(0,0,bitmap.getWidth()/2,bitmap.getHeight());

// 指定图片在屏幕上显示的区域
Rect dst = new Rect(100,100,250,250);
// 绘制图片 
canvas.drawBitmap(bitmap,src,dst,null);
```



![img](https://upload-images.jianshu.io/upload_images/944365-f36ea85f6a140a65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)



* 如果src规定绘制图片的区域大于dst指定显示的区域的话，那么图片的大小会被缩放。



#### 方法3的应用场景：

- 便于素材管理
  当我需要画很多个图时，如果1张图=1个素材的话，那么管理起来很不方便；如果素材都放在一个图，那么按需绘制会便于管理



![img](https://upload-images.jianshu.io/upload_images/944365-723a1255e9625937.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)



* 实现动态效果 动态效果 = 逐渐绘制图形部分，如下：



![img](https://upload-images.jianshu.io/upload_images/944365-a05fc6cf05056c87.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/392)



在绘制时，只需要一个资源文件，然后逐渐描绘就可以	

​					![img](https://upload-images.jianshu.io/upload_images/944365-1fc8aa5ee631e58b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/144)		



绘制过程如下



![img](https://upload-images.jianshu.io/upload_images/944365-b0d59f897573c931.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)