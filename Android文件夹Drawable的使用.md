# Drawable的使用
####1、自定义可绘制对象
* 使用方法：通过继承Drawable,类似于自定义View。使用打draw方法

#####1、圆角矩形的绘制


```
public class RoundImageDrawable extends Drawable  {  
  
    private Paint mPaint;  
    private Bitmap mBitmap;  
  
    private RectF rectF;  
  
    public RoundImageDrawable(Bitmap bitmap) {  
        mBitmap = bitmap;  
        BitmapShader bitmapShader = new 			BitmapShader(bitmap, TileMode.CLAMP,  
                TileMode.CLAMP);  
        mPaint = new Paint();  
        mPaint.setAntiAlias(true);  
        mPaint.setShader(bitmapShader);  
    }  
  
    @Override  
    public void setBounds(int left, int top, int right, int bottom) {  
        super.setBounds(left, top, right, bottom);  
        rectF = new RectF(left, top, right, bottom);  
    }  
  
    @Override  
    public void draw(Canvas canvas) {  
        canvas.drawRoundRect(rectF, 30, 30, mPaint);  
    }  
  
    @Override  
    public int getIntrinsicWidth() {  
        return mBitmap.getWidth();  
    }  
  
    @Override  
    public int getIntrinsicHeight() {  
        return mBitmap.getHeight();  
    }  
  
    @Override  
    public void setAlpha(int alpha) {  
        mPaint.setAlpha(alpha);  
    }  
  
    @Override  
    public void setColorFilter(ColorFilter cf) {  
        mPaint.setColorFilter(cf);  
    }  
  
    @Override  
    public int getOpacity() {  
        return PixelFormat.TRANSLUCENT;  
    }  
  
} 
```


* 核心代码就是draw，其中setAlpha、setColorFilter、getOpacity、draw这几个方法是必须实现的。getIntrinsicWidth、getIntrinsicHeight主要是为了在View使用wrap_content的时候，提供一下尺寸，默认为-1可不是我们希望的。setBounds就是去设置下绘制的范围。

  ```
  Bitmap bitmap = BitmapFactory.decodeResource(getResources(),  
                  R.drawable.mv);  
          ImageView imageView = (ImageView) findViewById(R.id.id_one);  
          imageView.setImageDrawable(new RoundImageDrawable(bitmap));
  ```

##### 2、圆形图片的绘制

* Paint绘制

  ```
   private Paint mPaint;  
      private int mWidth;  
      private Bitmap mBitmap ;   
    
      public CircleImageDrawable(Bitmap bitmap) {  
          mBitmap = bitmap ;   
          BitmapShader bitmapShader = new BitmapShader(bitmap, TileMode.CLAMP,  
                  TileMode.CLAMP);  
          mPaint = new Paint();  
          mPaint.setAntiAlias(true);  
          mPaint.setShader(bitmapShader);  
          mWidth = Math.min(mBitmap.getWidth(), mBitmap.getHeight());  
      }  
    
      @Override  
      public void draw(Canvas canvas) {  
          canvas.drawCircle(mWidth / 2, mWidth / 2, mWidth / 2, mPaint);  
      }  
    
      @Override  
      public int getIntrinsicWidth() {  
          return mWidth;  
      }  
    
      @Override  
      public int getIntrinsicHeight() {  
          return mWidth;  
      }  
    
      @Override  
      public void setAlpha(int alpha) {  
          mPaint.setAlpha(alpha);  
      }  
    
      @Override  
      public void setColorFilter(ColorFilter cf) {  
          mPaint.setColorFilter(cf);  
      }  
    
      @Override  
      public int getOpacity() {  
          return PixelFormat.TRANSLUCENT;  
      }
  }    
  ```

#####3、自定义Drawable状态

1、定义一个资源文件

```<?xml version="1.0" encoding="utf-8"?>  
<resources>  
    <declare-styleable name="MessageStatus">  
        <attr name="state_message_readed" format="boolean" />  
    </declare-styleable>  
</resources>
```
2、继承Item的容器


```
public class MessageListItem extends RelativeLayout {  
  
    private static final int[] STATE_MESSAGE_READED ={ R.attr.state_message_readed };  
    private boolean mMessgeReaded = false;  
  
    public MessageListItem(Context context, AttributeSet attrs) {  
        super(context, attrs);  
    }  
  
    public void setMessageReaded(boolean readed) {  
        if (this.mMessgeReaded != readed) {  
            mMessgeReaded = readed;  
            refreshDrawableState();  
        }  
    }  
  
    @Override  
    protected int[] onCreateDrawableState(int extraSpace) {  
        if (mMessgeReaded){  
            final int[] drawableState = super  
                    .onCreateDrawableState(extraSpace + 1);  
            mergeDrawableStates(drawableState, STATE_MESSAGE_READED);  
            return drawableState;  
        }  
        return super.onCreateDrawableState(extraSpace);  
    } 
}
```

声明了一个STATE_MESSAGE_READED，然后在mMessgeReaded=true的情况下，通过onCreateDrawableState方法，加入我们自定义的状态。ListView选项的可用和不可用的使用



```
<com.zhy.view.MessageListItem xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:layout_width="match_parent"  
    android:layout_height="50dp"  
    android:background="@drawable/message_item_bg" >  
  
    <ImageView  
        android:id="@+id/id_msg_item_icon"  
        android:layout_width="30dp"  
        android:src="@drawable/message_item_icon_bg"  
        android:layout_height="wrap_content"  
        android:duplicateParentState="true"  
        android:layout_alignParentLeft="true"  
        android:layout_centerVertical="true"  
      />  
  
    <TextView  
        android:id="@+id/id_msg_item_text"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:layout_centerVertical="true"  
        android:layout_toRightOf="@id/id_msg_item_icon" />  
  
</com.zhy.view.MessageListItem> 
```

#####5、BitmapShader

BitmapShader是Shader的子类，可以通过Paint.setShader（Shader shader）进行设置、

这里我们只关注BitmapShader，构造方法：

mBitmapShader = new BitmapShader(bitmap, TileMode.CLAMP, TileMode.CLAMP);

* 参数1：bitmap

* 参数2，参数3：TileMode；

TileMode的取值有三种：

* CLAMP 拉伸

* REPEAT 重复

* MIRROR 镜像

如果大家给电脑屏幕设置屏保的时候，如果图片太小，可以选择重复、拉伸、镜像；

* 重复：就是横向、纵向不断重复这个bitmap

* 镜像：横向不断翻转重复，纵向不断翻转重复；

拉伸：这个和电脑屏保的模式应该有些不同，这个拉伸的是图片最后的那一个像素；横向的最后一个横行像素，不断的重复，纵项的那一列像素，不断的重复；

BitmapShader通过设置给mPaint，然后用这个mPaint绘图时，就会根据你设置的TileMode，对绘制区域进行着色。

这里需要注意一点：就是BitmapShader是从你的画布的左上角开始绘制的，不在view的右下角绘制个正方形，它不会在你正方形的左上角开始。

view的宽或者高大于我们的bitmap宽或者高岂不是会拉伸？

通过为BitmapShader设置一个matrix，去适当的放大或者缩小图片，不会让“ view的宽或者高大于我们的bitmap宽或者高 ”此条件成立的。

原理：拿到drawable转化为bitmap，然后直接初始化BitmapShader，画笔设置Shader，最后在onDraw里面进行画圆。

######1 自定义属性

在value/attr.xml定义一个枚举和一个圆角的大小borderRadius。

```
<?xml version="1.0" encoding="utf-8"?>  
<resources>  
  
    <attr name="borderRadius" format="dimension" />  
    <attr name="type">  
        <enum name="circle" value="0" />  
        <enum name="round" value="1" />  
    </attr>  
    
  
    <declare-styleable name="RoundImageView">  
        <attr name="borderRadius" />  
        <attr name="type" />  
    </declare-styleable>  
  
</resources>
```
######2 获取自定义属性

```
public class RoundImageView extends ImageView  {  
  
    /** 
     * 图片的类型，圆形or圆角 
     */  
    private int type;  
    private static final int TYPE_CIRCLE = 0;  
    private static final int TYPE_ROUND = 1;  
  
    /** 
     * 圆角大小的默认值 
     */  
    private static final int BODER_RADIUS_DEFAULT = 10;  
    /** 
     * 圆角的大小 
     */  
    private int mBorderRadius;  
  
    /** 
     * 绘图的Paint 
     */  
    private Paint mBitmapPaint;  
    /** 
     * 圆角的半径 
     */  
    private int mRadius;  
    /** 
     * 3x3 矩阵，主要用于缩小放大 
     */  
    private Matrix mMatrix;  
    /** 
     * 渲染图像，使用图像为绘制图形着色 
     */  
    private BitmapShader mBitmapShader;  
    /** 
     * view的宽度 
     */  
    private int mWidth;  
    private RectF mRoundRect;  
  
    public RoundImageView(Context context, AttributeSet attrs) {  
        super(context, attrs);  
        mMatrix = new Matrix();  
        mBitmapPaint = new Paint();  
        mBitmapPaint.setAntiAlias(true);  
  
        TypedArray a = context.obtainStyledAttributes(attrs,  
                R.styleable.RoundImageView);  
  
        mBorderRadius = a.getDimensionPixelSize(  
                R.styleable.RoundImageView_borderRadius, (int) TypedValue  
                        .applyDimension(TypedValue.COMPLEX_UNIT_DIP,  
                                BODER_RADIUS_DEFAULT, getResources()  
                                        .getDisplayMetrics()));// 默认为10dp  
        type = a.getInt(R.styleable.RoundImageView_type, TYPE_CIRCLE);// 默认为Circle  
  
        a.recycle();  
    } 
    
     protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {  
        Log.e("TAG", "onMeasure");  
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);  
  
        /** 
         * 如果类型是圆形，则强制改变view的宽高一致，以小值为准 
         */  
        if (type == TYPE_CIRCLE)  
        {  
            mWidth = Math.min(getMeasuredWidth(), getMeasuredHeight());  
            mRadius = mWidth / 2;  
            setMeasuredDimension(mWidth, mWidth);  
        } 
    }
}
```

复写了onMeasure方法，主要用于当设置类型为圆形时，我们强制让view的宽和高一致。

######4 设置BitmapShader


```/** 
     * 初始化BitmapShader 
     */  
    private void setUpShader() {  
        Drawable drawable = getDrawable();  
        if (drawable == null) {  
            return;  
        }  
  
        Bitmap bmp = drawableToBitamp(drawable);  
        // 将bmp作为着色器，就是在指定区域内绘制bmp  
        mBitmapShader = new BitmapShader(bmp, TileMode.CLAMP, TileMode.CLAMP);  
        float scale = 1.0f;  
        if (type == TYPE_CIRCLE) {  
            // 拿到bitmap宽或高的小值  
            int bSize = Math.min(bmp.getWidth(), bmp.getHeight());  
            scale = mWidth * 1.0f / bSize;  
  
        } else if (type == TYPE_ROUND) {  
            // 如果图片的宽或者高与view的宽高不匹配，计算出需要缩放的比例；缩放后的图片的宽高，一定要大于我们view的宽高；所以我们这里取大值；  
            scale = Math.max(getWidth() * 1.0f / bmp.getWidth(), getHeight()  
                    * 1.0f / bmp.getHeight());  
        }  
        // shader的变换矩阵，我们这里主要用于放大或者缩小  
        mMatrix.setScale(scale, scale);  
        // 设置变换矩阵  
        mBitmapShader.setLocalMatrix(mMatrix);  
        // 设置shader  
        mBitmapPaint.setShader(mBitmapShader);  
    }
```

在setUpShader中，首先对drawable转化为我们的bitmap;

然后初始化mBitmapShader = new BitmapShader(bmp, TileMode.CLAMP, TileMode.CLAMP);

接下来，根据类型以及bitmap和view的宽高，计算scale；

关于scale的计算：

圆形时：取bitmap的宽或者高的小值作为基准，如果采用大值，缩放后肯定不能填满我们的圆形区域。然后，view的mWidth/bSize ; 得到的就是scale。

圆角时：因为设计到宽/高比例，我们分别getWidth() * 1.0f / bmp.getWidth() 和 getHeight() * 1.0f / bmp.getHeight() ；最终取大值，因为我们要让最终缩放完成的图片一定要大于我们的view的区域，有点类似centerCrop；

比如：view的宽高为10*20；图片的宽高为5*100 ； 最终我们应该按照宽的比例放大，而不是按照高的比例缩小；因为我们需要让缩放后的图片，自定大于我们的view宽高，并保证原图比例。

有了scale，就可以设置给我们的matrix；

然后使用mBitmapShader.setLocalMatrix(mMatrix);

最后将bitmapShader设置给paint。

关于drawable转bitmap的代码：



```/** 
     * drawable转bitmap 
     *  
     * @param drawable 
     * @return 
     */  
    private Bitmap drawableToBitamp(Drawable drawable) {  
        if (drawable instanceof BitmapDrawable)  {  
            BitmapDrawable bd = (BitmapDrawable) drawable;  
            return bd.getBitmap();  
        }  
        int w = drawable.getIntrinsicWidth();  
        int h = drawable.getIntrinsicHeight();  
        Bitmap bitmap = Bitmap.createBitmap(w, h, Bitmap.Config.ARGB_8888);  
        Canvas canvas = new Canvas(bitmap);  
        drawable.setBounds(0, 0, w, h);  
        drawable.draw(canvas);  
        return bitmap;  
    }
```


最后会在onDraw里面调用setUpShader（），然后进行绘制。

######5 绘制


```
@Override  
    protected void onDraw(Canvas canvas) {  
        if (getDrawable() == null) {  
            return;  
        }  
        setUpShader();  
  
        if (type == TYPE_ROUND) {  
            canvas.drawRoundRect(mRoundRect, mBorderRadius, mBorderRadius,  
                    mBitmapPaint);  
        } else {  
            canvas.drawCircle(mRadius, mRadius, mRadius, mBitmapPaint);  
            // drawSomeThing(canvas);  
        }  
    }  
      
    @Override  
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {  
        super.onSizeChanged(w, h, oldw, oldh);  
        // 圆角图片的范围  
        if (type == TYPE_ROUND)  
            mRoundRect = new RectF(0, 0, getWidth(), getHeight());  
    }
```

圆角矩形的限定范围mRoundRect在onSizeChanged里面进行了初始化


######6 状态的存储与恢复

如果内存不足，而恰好我们的Activity置于后台，不幸被重启，或者用户旋转屏幕造成Activity重启，我们的View应该也能尽可能的去保存自己的属性。
状态保存什么用处呢？比如，现在一个的圆角大小是10dp，用户点击后变成50dp；当用户旋转以后，或者长时间置于后台以后，返回我们的Activity应该还是50dp；
简单的存储一下，当前的type以及mBorderRadius


```
private static final String STATE_INSTANCE = "state_instance";  
    private static final String STATE_TYPE = "state_type";  
    private static final String STATE_BORDER_RADIUS = "state_border_radius";  
  
    @Override  
    protected Parcelable onSaveInstanceState() {  
        Bundle bundle = new Bundle();  
        bundle.putParcelable(STATE_INSTANCE, super.onSaveInstanceState());  
        bundle.putInt(STATE_TYPE, type);  
        bundle.putInt(STATE_BORDER_RADIUS, mBorderRadius);  
        return bundle;  
    }  
  
    @Override  
    protected void onRestoreInstanceState(Parcelable state) {  
        if (state instanceof Bundle)  
        {  
            Bundle bundle = (Bundle) state;  
            super.onRestoreInstanceState(((Bundle) state)  
                    .getParcelable(STATE_INSTANCE));  
            this.type = bundle.getInt(STATE_TYPE);  
            this.mBorderRadius = bundle.getInt(STATE_BORDER_RADIUS);  
     
        } else {  
            super.onRestoreInstanceState(state);  
        }  
  
    }
```

同时我们也对外公布了两个方法，用于动态修改圆角大小和type

```
public void setBorderRadius(int borderRadius) {  
        int pxVal = dp2px(borderRadius);  
        if (this.mBorderRadius != pxVal) {  
            this.mBorderRadius = pxVal;  
            invalidate();  
        }  
    }  
  
    public void setType(int type) {  
        if (this.type != type) {  
            this.type = type;  
            if (this.type != TYPE_ROUND && this.type != TYPE_CIRCLE) {  
                this.type = TYPE_CIRCLE;  
            }  
            requestLayout();  
        }  
  
    }  
  
    public int dp2px(int dpVal) {  
        return (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP,  
                dpVal, getResources().getDisplayMetrics());  
    } 
```


######7 调用


```<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:tools="http://schemas.android.com/tools"  
    xmlns:zhy="http://schemas.android.com/apk/res/com.zhy.variousshapeimageview"  
    android:layout_width="match_parent"  
    android:layout_height="wrap_content" >  
  
    <LinearLayout  
        android:layout_width="match_parent"  
        android:layout_height="match_parent"  
        android:orientation="vertical" >  
     <com.zhy.view.RoundImageView  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_margin="10dp"  
            android:src="@drawable/qiqiu"  
            zhy:borderRadius="60dp"  
            zhy:type="round" >  
        </com.zhy.view.RoundImageView>  
    </LinearLayout>  
  
</ScrollView>
```


```
public class MainActivity extends Activity  {  
    private RoundImageView mQiQiu;  
    private RoundImageView mMeiNv ;   
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
          
        mQiQiu = (RoundImageView) findViewById(R.id.id_qiqiu);  
        mMeiNv = (RoundImageView) findViewById(R.id.id_meinv);  
          
        mQiQiu.setOnClickListener(new OnClickListener() {  
            @Override  
            public void onClick(View v) {  
                mQiQiu.setType(RoundImageView.TYPE_ROUND);  
            }  
        });  
          
        mMeiNv.setOnClickListener(new OnClickListener() {  
              
            @Override  
            public void onClick(View v) {  
                mMeiNv.setBorderRadius(90);  
            }  
        });  
  	}
}
```
