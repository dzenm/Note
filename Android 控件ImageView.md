###一、ImageView的ScaleType属性



![img](https://upload-images.jianshu.io/upload_images/1458573-b7ce213f05816e85.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)



![img](https://upload-images.jianshu.io/upload_images/1458573-4e468ddb0dbcd11c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)



![img](https://upload-images.jianshu.io/upload_images/1458573-f79bf962aed9c5bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)



####说明

1. FIT_XY：对原图宽高进行放缩，该放缩不保持原比例来填充满ImageView。
2. MATRIX：不改变原图大小从ImageView的左上角开始绘制，超过ImageView部分不再显示。
3. CENTER：对原图居中显示，超过ImageView部分不再显示。
4. CENTER_CROP：对原图居中显示后进行等比放缩处理，使原图最小边等于ImageView的相应边。
5. CENTER_INSIDE：若原图宽高小于ImageView宽高，这原图不做处理居中显示，否则按比例放缩原图宽(高)是之等于ImageView的宽(高)。
6. FIT_START：对原图按比例放缩使之等于ImageView的宽高，若原图高大于宽则左对齐否则上对其。
7. FIT_CENTER：对原图按比例放缩使之等于ImageView的宽高使之居中显示。
8. FIT_END：对原图按比例放缩使之等于ImageView的宽高，若原图高大于宽则右对齐否则下对其。

> 当我们没有在布局文件中使用scaleType属性或者是没有手动调用setScaleType方法时，那么mScaleType的默认值就是FIT_CENTER。







----





###二、ImageView的src属性和background属性的区别

* imageButton是带有图标的Button控件,有src属性,也有一个所有控件都有的属性background。

	​	

- src是设置图片内容前景(原图大小),scaleType只对src缩放,centerInside按钮比例缩放图片
- background是设置背景
  - 当图片资源比控件小,src居中显示,background会拉伸充满控件
  - background是底层图片资源,src是覆盖在上面的资源,可以叠加使用

```
CENTER/center 在视图中心显示图片，并且不缩放图片

CENTER_CROP/centerCrop 按比例缩放图片，使得图片长 (宽)的大于等于视图的相应维度

CENTER_INSIDE/centerInside 按比例缩放图片，使得图片长 (宽)的小于等于视图的相应维度

FIT_CENTER/fitCenter 按比例缩放图片到视图的最小边，居中显示

FIT_END/fitEnd 按比例缩放图片到视图的最小边，显示在视图的下部分位置

FIT_START/fitStart 把图片按比例扩大/缩小到视图的最小边，显示在视图的上部分位置

FIT_XY/fitXY 把图片不按比例缩放到视图的大小显示

MATRIX/matrix 用矩阵来绘制
```

​	



### 三、Bitmap的使用

####1、BitmapFactory的四类方法加载Bitmap

> 1. decodeFile 从文件系统加载
>
>    a. 通过Intent打开本地图片或照片
>    b. 在onActivityResult中获取图片uri
>    c. 根据uri获取图片的路径
>    d. 根据路径解析bitmap:Bitmap bm = BitmapFactory.decodeFile(sd_path)
>
> 2. decodeResource 以R.drawable.xxx的形式从本地资源中加载       Bitmap bm = BitmapFactory.decodeResource(getResources(), R.drawable.aaa);
>
> 3. decodeStream 从输入流加载
>    a.开启异步线程去获取网络图片
>    b.网络返回InputStream
>    c.解析：`Bitmap bm = BitmapFactory.decodeStream(stream)`,这是一个耗时操作，要在子线程中执行
>
> 4. decodeByteArray 从字节数组中加载
>    接3.a,3.b,
>    c. 把InputStream转换成byte[]
>    d. 解析：Bitmap bm = BitmapFactory.decodeByteArray(myByte,0,myByte.length); 



####2、高效加载Bitmap

使用bitmap时，经常会遇到内存溢出等情况，这是因为图片太大或者android系统对单个应用施加的内存限制等原因造成的。

高效加载Bitmap的思想也很简单，就是使用系统提供给我们Options类来处理Bitmap。翻看Bitmap的源码，发现上述四个加载bitmap的方法都是支持Options参数的。

通过BitmapFactory.Options按一定的采样率来加载缩小后的图片，然后在ImageView中使用缩小的图片这样就会降低内存占用避免【OOM】，提高了Bitamp加载时的性能。

这其实就是我们常说的图片尺寸压缩。**尺寸压缩**是压缩图片的像素，一张图片所占内存的大小 图片类型＊宽＊高，通过改变三个值减小图片所占的内存，防止OOM，当然这种方式可能会使图片失真 。



​	**android 色彩模式说明：**

* ALPHA_8：每个像素占用1byte内存。

- ARGB_4444:每个像素占用2byte内存
- ARGB_8888:每个像素占用4byte内存
- RGB_565:每个像素占用2byte内存

 

​	Android默认的色彩模式为ARGB_8888，这个色彩模式色彩最细腻，		显示质量最高。但同样的，占用的内存也最大。

​	BitmapFactory.Options的inPreferredConfig参数可以 指定decode到内存中，手机中所采用的编码，可选值定义在Bitmap.Config中。缺省值是ARGB_8888

​	假设一张1024*1024，模式为ARGB_8888的图片，那么它占有的内存就是：1024*1024*4 = 4MB



1、采样率inSampleSize

- inSampleSize的值必须大于1时才会有效果，且采样率同时作用于宽和高；
- 当inSampleSize=1时，采样后的图片为图片的原始大小
- 当inSampleSize=2时，采样后的图片的宽高均为原始图片宽高的1/2，这时像素为原始图片的1/(2*2),占用内存也为原始图片的1/(2*2);
- inSampleSize的取值应该总为2的整数倍，否则会向下取整，取一个最接近2的整数倍，比如inSampleSize=3时，系统会取inSampleSize=2

假设一张1024*1024，模式为ARGB_8888的图片,inSampleSize=2，原始占用内存大小是4MB，采样后的图片占用内存大小就是(1024/2) * (1024/2 )* 4 = 1MB

 	

##### 2、获取采样率遵循以下步骤

1. 将BitmapFacpry.Options的inJustDecodeBounds参数设为true并加载图片`当inJustDecodeBounds为true时，执行decodeXXX方法时，BitmapFactory只会解析图片的原始宽高信息，并不会真正的加载图片` 
2. 从BitmapFacpry.Options取出图片的原始宽高(outWidth,outHeight)信息
3. 选取合适的采样率
4. 将BitmapFacpry.Options的inSampleSize参数设为false并重新加载图片

经过上面过程加载出来的图片就是采样后的图片，代码如下：

```java
public void decodeResource(View view) {
    Bitmap bm = decodeBitmapFromResource();
    imageview.setImageBitmap(bm);
}

private Bitmap decodeBitmapFromResource(){
    BitmapFactory.Options options = new BitmapFactory.Options();
    options.inJustDecodeBounds = true;
    BitmapFactory.decodeResource(getResources(), R.drawable.bbbb, options);
    options.inSampleSize = calculateSampleSize(options,300,300);
    options.inJustDecodeBounds =false;
    return  BitmapFactory.decodeResource(getResources(),R.drawable.bbbb,options);
}

// 计算合适的采样率(当然这里还可以自己定义计算规则)，reqWidth为期望的图片大小，单位是px
private int calculateSampleSize(BitmapFactory.Options options,int reqWidth,int reqHeight){
    Log.i("========","calculateSampleSize reqWidth:"+reqWidth+",reqHeight:"+reqHeight);
    int width = options.outWidth;
    int height =options.outHeight;
    Log.i("========","calculateSampleSize width:"+width+",height:"+height);
    int inSampleSize = 1;
    int halfWidth = width/2;
    int halfHeight = height/2;
    while((halfWidth/inSampleSize)>=reqWidth&& (halfHeight/inSampleSize)>=reqHeight){
        inSampleSize*=2;
        Log.i("========","calculateSampleSize inSampleSize:"+inSampleSize);
    }
    return inSampleSize;
}
```

 

#### 3、使用Bitmap时的一些注意事项

###### 1、不用的Bitmap及时释放

 ```java
if (!bmp.isRecycle()) {
    bmp.recycle();   //回收图片所占的内存
    bitmap = null;
    system.gc();  //提醒系统及时回收
}
 ```



虽然调用recycle()并不能保证立即释放占用的内存，但是可以加速Bitmap的内存的释放。
 释放内存以后，就不能再使用该Bitmap对象了，如果再次使用，就会抛出异常。所以一定要保证不再使用的时候释放。比如，如果是在某个Activity中使用Bitmap，就可以在Activity的onStop()或者onDestroy()方法中进行回收。

 

 ######2、捕获异常

因为Bitmap非常耗内存，了避免应用在分配Bitmap内存的时候出现OutOfMemory异常以后Crash掉，需要特别注意实例化Bitmap部分的代码。通常，在实例化Bitmap的代码中，一定要对OutOfMemory异常进行捕获。很多开发者会习惯性的在代码中直接捕获Exception。但是对于OutOfMemoryError来说，这样做是捕获不到的。因为OutOfMemoryError是一种Error，而不是Exception。

 

```java
Bitmap bitmap = null;
    try {
        // 实例化Bitmap
        bitmap = BitmapFactory.decodeFile(path);
    } catch (OutOfMemoryError e) {
    
    }
    if (bitmap == null) {
        return defaultBitmapMap; // 如果实例化失败 返回默认的Bitmap对象
    }
```



 ######3、【缓存通用的Bitmap对象】

有时候，可能需要在一个Activity里多次用到同一张图片。比如一个Activity会展示一些用户的头像列表，而如果用户没有设置头像的话，则会显示一个默认头像，而这个头像是位于应用程序本身的资源文件中的。如果有类似上面的场景，就可以对同一Bitmap进行缓存。`如果不进行缓存，尽管看到的是同一张图片文件，但是使用BitmapFactory类的方法来实例化出来的Bitmap，是不同的Bitmap对象。缓存可以避免新建多个Bitmap对象，避免内存的浪费。`在Android应用开发过程中所说的缓存有两个级别，一个是硬盘缓存，一个是内存缓存。



###### 4、图片的质量压缩

上述用inSampleSize压缩是尺寸压缩，Android中还有一种压缩方式叫质量压缩。**质量压缩**是在保持像素的前提下改变图片的位深及透明度等，来达到压缩图片的目的，**经过它压缩的图片文件大小(kb)会有改变，但是导入成bitmap后占得内存是不变的，宽高也不会改变**。因为要保持像素不变，所以它就无法无限压缩，到达一个值之后就不会继续变小了。显然这个方法并不适用与缩略图，其实也不适用于想通过压缩图片减少内存的适用，仅仅适用于想在保证图片质量的同时减少文件大小的情况而已

```java
private void compressImage(Bitmap image, int reqSize) {
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        image.compress(Bitmap.CompressFormat.JPEG, 100, baos);// 质量压缩方法，这里100表示不压缩，
        int options = 100;
        while (baos.toByteArray().length / 1024 > reqSize) { // 循环判断压缩后的图片是否大于reqSize，大于则继续压缩
            baos.reset();//清空baos
            image.compress(Bitmap.CompressFormat.JPEG, options, baos);// 这里压缩options%，把压缩后的数据放到baos中
            options -= 10;
        }
        // 把压缩后的baos放到ByteArrayInputStream中
        ByteArrayInputStream isBm = new ByteArrayInputStream(baos.toByteArray());
        //decode图片
        Bitmap bitmap = BitmapFactory.decodeStream(isBm, null, null);
    }
```

 

###### 5、Android加载大量图片内存溢出解决方案：

1. 尽量不要使用setImageBitmap或setImageResource或BitmapFactory.decodeResource来设置一张大图，因为这些函数在完成decode后，最终都是通过java层的createBitmap来完成的，需要消耗更多内存，可以通过BitmapFactory.decodeStream方法，创建出一个bitmap，再将其设为ImageView的 source
2. 使用BitmapFactory.Options对图片进行压缩(上述第二部分)
3. 运用Java软引用，进行图片缓存，将需要经常加载的图片放进缓存里，避免反复加载

 

 ####4、Bitmap一些其他用法

###### 1、图片旋转指定角度

 ```java
// 图片旋转指定角度
private Bitmap rotateImage(Bitmap image, final int degree) {
    int width = image.getWidth();
    int height = image.getHeight();
    if (width > height) {
        Matrix matrix = new Matrix();
        matrix.postRotate(degree);
        if (image != null && !image.isRecycled()) {
            Bitmap resizedBitmap = Bitmap.createBitmap(image, 0, 0, width, height, matrix, true);
            return resizedBitmap;
        } else {
            return null;
        }
    } else {
        return image;
    }
}
 ```



###### 2、图片合成

 ```java
private Bitmap createStarBitmap(float grade, int maxGrade) {
        Bitmap empty_star = BitmapFactory.decodeResource(getResources(), R.drawable.empty_star); // 空星
        Bitmap normal_star = BitmapFactory.decodeResource(getResources(), R.drawable.normal_star); // 实星
        Bitmap half_star = BitmapFactory.decodeResource(getResources(), R.drawable.half_star);
        ; // 半星
        int star_width = empty_star.getWidth();
        int star_height = empty_star.getHeight();
        Bitmap newb = Bitmap.createBitmap(star_width * 5, star_height, Bitmap.Config.ARGB_8888);// 创建一个底层画布
        Canvas cv = new Canvas(newb);
        for (int i = 0; i < maxGrade; i++) {
            if (i < grade && i + 1 > grade) // 画半星
            {
                cv.drawBitmap(half_star, star_width * i, 0, null);// 画图片的位置
            } else if (i < grade) // 画实心
            {
                cv.drawBitmap(normal_star, star_width * i, 0, null);// 画图片的位置
            } else
            // 画空心
            {
                cv.drawBitmap(empty_star, star_width * i, 0, null);// 画图片的位置
            }
        }
        // save all clip
        cv.save(Canvas.ALL_SAVE_FLAG);// 保存
        // store
        cv.restore();// 存储
        return newb;
    }

activity中调用
Bitmap bm = createStarBitmap(3.5f, 5);
imageview.setImageBitmap(bm);
 ```



上述代码展示的是通过右图的三张图片动态合成评分组件：

![img](https://upload-images.jianshu.io/upload_images/1479978-5bdfc27368334a23.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/499) 

 

 ######3、图片圆角

```java
public Bitmap toRoundCorner(Bitmap bitmap, int pixels) {
        Bitmap roundCornerBitmap = Bitmap.createBitmap(bitmap.getWidth(), bitmap.getHeight(), Bitmap.Config.ARGB_8888);
        Canvas canvas = new Canvas(roundCornerBitmap);
        int color = 0xff424242;// int color = 0xff424242;
        Paint paint = new Paint();
        paint.setColor(color);
        // 防止锯齿
        paint.setAntiAlias(true);
        Rect rect = new Rect(0, 0, bitmap.getWidth(), bitmap.getHeight());
        RectF rectF = new RectF(rect);
        float roundPx = pixels;
        // 相当于清屏
        canvas.drawARGB(0, 0, 0, 0);
        // 先画了一个带圆角的矩形
        canvas.drawRoundRect(rectF, roundPx, roundPx, paint);
        paint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.SRC_IN));
        // 再把原来的bitmap画到现在的bitmap！！！注意这个理解
        canvas.drawBitmap(bitmap, rect, rect, paint);
        return roundCornerBitmap;
    }
```

 ![img](https://upload-images.jianshu.io/upload_images/1479978-fe7aa305cf897bbb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/344)

 

###### 4、将Bitmap转换成drawable

`Drawable newBitmapDrawable = new BitmapDrawable(bitmap);`

还可以从BitmapDrawable中获取Bitmap对象
`Bitmap bitmap = new BitmapDrawable.getBitmap();`

  

###### 5、drawable转换成Bitmap

 ```java
public static Bitmap drawableToBitmap(Drawable drawable) {
        Bitmap bitmap = Bitmap.createBitmap(
                drawable.getIntrinsicWidth(),
                drawable.getIntrinsicHeight(),
                drawable.getOpacity() != PixelFormat.OPAQUE ? Bitmap.Config.ARGB_8888 : Bitmap.Config.RGB_565
        );
        Canvas canvas = new Canvas(bitmap);
        drawable.setBounds(0, 0, drawable.getIntrinsicWidth(), drawable.getIntrinsicHeight());
        drawable.draw(canvas);
        return bitmap;
    }
 ```



###### 6、图片的放大和缩小

 ```java
public Bitmap scaleMatrixImage(Bitmap oldbitmap, float scaleWidth, float scaleHeight) {
    Matrix matrix = new Matrix();
    matrix.postScale(scaleWidth,scaleHeight);// 放大缩小比例
    Bitmap ScaleBitmap = Bitmap.createBitmap(oldbitmap, 0, 0, oldbitmap.getWidth(), oldbitmap.getHeight(), matrix, true);
    return ScaleBitmap;
}
 ```

  

###### 7、图片裁剪

 ```java
public Bitmap cutImage(Bitmap bitmap, int reqWidth, int reqHeight) {
    Bitmap newBitmap = null;
    if (bitmap.getWidth() > reqWidth && bitmap.getHeight() > reqHeight) {
        bitmap = Bitmap.createBitmap(bitmap, 0, 0, reqWidth, reqHeight);
    } else {
        bitmap = Bitmap.createBitmap(bitmap, 0, 0, bitmap.getWidth(), bitmap.getHeight());
    }
    return bitmap;
}
 ```

 

###### 8、图片保存到sd

 ```java
public void savePic(Bitmap bitmap,String path) {
        File file = new File(path);
        FileOutputStream fileOutputStream = null;
        try {
            file.createNewFile();
            fileOutputStream = new FileOutputStream(file);
            bitmap.compress(Bitmap.CompressFormat.PNG, 100, fileOutputStream);
            fileOutputStream.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (fileOutputStream != null) {
                    fileOutputStream.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
 ```

 

 

 

 

 