# View

###1、view的位置参数

* 左上角为坐标原点，往右为x轴，往下为y轴

* top为view左上角到x轴距离 (通过getTop() 获取)

* right为view左上角到y轴距离 (通过getRight() 获取)

* bottom为view底部到x轴距离 (通过getBottom() 获取)

* left为view右边到y轴距离 (通过getLeft() 获取)

  view的width = right - left
  view的height = bottom - top


安卓3.0之后,新增x,y,translationX,translationY, 其中x为为view左上角x的坐标, y为view左上角y的坐标, translationX为view在x方向的偏移量, translationY为view在y方向的偏移量, 当view位置发生改变时,view的x,y,translationX,translationY 均发生变化

###2、MotionEvent和TouchSlop

#####1、MotionEvent是手指触摸屏幕后产生的事件,下列为常见的事件类型

* ACYION_DOWN---手指刚好接触屏幕调用
* ACTION_MOVE---手指在屏幕移动时调用
* ACTION_UP---手指移开屏幕时调用

正常情况下,手指触摸屏幕会触发以下事件

1. 点击屏幕后松开 (由ACTION_DOWN --> ACTION_UP)
2. 点击屏幕滑动后在松开 (由ACTION_DOWN --> ACTION_MOVE --> ACTION_UP)

  过MotionEvent可以得到点击事件发生的x和y坐标,    	getX()和getY()返回相对于当前view的x和y坐标,	getRawX()和getRawY()返回的是相对于手机屏幕左上角x和y的坐标


TouchSlop是识别滑动的最小距离,即第一次和第二次滑动之间的距离是否某个常量。不同设置该值不同,通过如下方法可以获取该值

```
ViewConfiguration.get(getContext()).getScaledTouchSlop()
```

#####2、VelocityTracker为滑动速度追踪
在view的onTouchEvent方法追踪当前单机事件速度

```
VelocityTracker velocityTracker = VelocityTracker.obtain();
velocityTracker.addMovement(event);
```

#####3、GestureDetector(手势检测)
用于检测用户单击、滑动、长按、双击灯行为，
首先创建一个GestureDetector对象并实现onGestureListener接口，onDoubleTapListener可以实现监听双击行为

```
GestureDetector mGestureDetector = new GestureDetector(this);	//解决长按屏幕后无法拖动的现象
mGestureDetector.setIsLongperssEnabled(false);
```
接管目标View的onTouchEvent方法，在待监听view的onTouchEvent方法zhong添加实现

```
boolean consume = mGestureDetector.onTouchEvent(event);
return consume;
```
然后可以实现OnGestureListener和OnDoubleTapListener的方法
OnGestureListener接口有以下方法

1. onDown()		手指轻触屏幕瞬间,由一个ACTION_DOWN触发
  . onShowPress()	手指轻触屏幕未松开或拖动,由一个ACTION_DOWN触发
  . onSongleTapUp()	手指(轻触屏幕)松开,单击触发一个MotionEvent ACTION_UP
  . onScroll()	手指按下屏幕并拖动,由一个ACTION_DOWN和多个ACTION_MOVE触发,属于拖动行为
2. onLongPress()  用户长久地按着屏幕不放,属于长按
  . onFling()	用户按下触摸屏、快速滑动后松开,由一个ACTION_DOWN,多个ACTION_MOVE和一个ACTION_UP触发,属于快速滑动行为

onDoubleTapListener()接口有以下方法

1. onDoubleTap()	 双击,两次连续单击组成,不可能和onSingleTapConfirmed()共存
  . onSingleTapConfirmed()	 严格的单击行为, 只能是单击,不可能是双击中的单击行为
  . onDoubleTapEvent()	表示发生了双击行为,在双击期间ACTION_DOWN,ACTION_MOVE,ACTION_UP都会触发此行为

* 常用的方法有
  * onSingleTapUp(单击)、onFling(快速滑动)、onScroll(拖动)、onLongPress(长按)和onDoubleTap(双击)
  * 实际开发中,可以不使用GestureDetector，完全可以自己在View的onTouchEvent方法中实现监听
  * 滑动监听相关的,建议自己在onTouchEvent中实现,要实现监听双击行为,必须用GestureDetector

#####4、Scroll弹性滑动对象
用于实现View的弹性滑动,具有过渡滑动效果.

```
Scroller scroller = new Scroller(mContext);

//缓慢滚动到指定位置
private void smoothScrollTo(int destX, int destY){
	int scrollX = getScrlooX();
	int delta = destX = scrlooX;
	//1000ms内滑动destX,效果就是满满滑动
	mScroller.startScroll(scrollX, 0, delta, 0, 1000);
	invalidate();
}

public void computeScroll(){
	if(mScroller.computeScrollOffset()){
		scrollTo(mScroller.getCurrX(),mScroller.getCurrY());
		postIncalidate();
	}
}
```

#####5、View的滑动

三种实现View的滑动：

1. 通过文本提供scrollTo/scrollBy方法来实现滑动
2. 通过动画给View施加平移效果来实现
3. 通过改变View的LayoutParams使得View重新布局实现滑动

######1、使用scrollTo/scrollBy

```
public void scrollTo(int x, int y){
	if(mScrollX != x || mScrollY != y){
		int oldX = mScrollX;
		int oldY = mScrollY;
		mScrollX = x;
		mScrollY = y;
		invalidateParentCaches();
		onScrollChanged(mScrollX, mScrollY, oldX, oldY)
		if(!awakenScrollBars()){
			postInvalidateOnAnimation();
		}
	}
}

public void scrollBy(int x, int y){
	scrollTo(mScrollX + x, mScrollY + y);
}
```

利用scrollTo和scrollBy来实现View的滑动。滑动过程中View的内部的两个属性mScrollX和mScrollY的改变规则,这两个属性可以通过getScrollX和getScrollY方法分别得到。滑动过程中 mScrollX的值总是等于View内容的左边缘和View内容的右边缘在水平方向的距离,而mScrollY的值总是等于View的上边缘和View的下边缘在竖直方向的距离。

scrollTo和scrollBy只能改变View内容的位置而不能改变View在布局中的位置,mScrollX和mScrollY的单位为像素,并且当View的左边缘在View的内容左边缘的右边时,mScrollX为正值,反之为负值;当View的上边缘在View的内容上边缘的下边时,mScrollY为正值,反之为负值。即从左向右滑动时,mScrollX为负值,反之为正值。如果从上往下滑动,mScrollY为负值,反之为正值。

######2、使用属性动画


```<?xml version = "1.0" encoding = "utf-8"?>
<set xmls:android = "http://schemas.android.com/apk/res/android"
android:fillAfter = "true"
android:zAdjustment = "normal" >

	<translate
		android:duration = "100"
		android:fromXDelta = "0"
		android:fromYDelta = "0"
		android:interpolator = "@android:anim/linear_interpolator"
		android:toXDelta = "100"
		android:toYDelta = "100" />
```


通过代码设置	

```
ObjectAnimator.ofFloat(targetView, "translateX", 0, 100).setDuration(100).start();
```
######3、改变布局参数

改变布局参数即改变LayoutParams,例如,Button的LayoutParams的marginLeft参数增加100px,在Button左边放置一个空的View,这个空View的默认宽度为0,当我们需要向右移动Button时,只需重新设置空View宽度即可,当空View的宽度增大时(假设Button的父容器是水平方向的LinearLayout),Button就自动被挤向右边,即实现了向右平移的效果。

```
MarginLayoutParams params = (MarginLayoutParams)mButton.getLayoutParams();
params.width += 100;
params.leftMargin += 100;
mButton.requestLayout();
//或者mButton.setLayoutParams(params);
```

######4、各种滑动方式的对比
scrollTo/scrollBy是View提供的原生方法,其作用是专门用于View的滑动,可以方便的实现滑动效果并且不影响内部元素的单击事件,缺点是只能滑动View的内容,不能滑动View的本身

android 3.0 以上通过属性动画来实现滑动没有明显的缺点,View动画和属性动画均不能改变View本身的属性,如果动画元素不需要响应用户的交互,适合用动画来做滑动,使用动画的优点,适合做一些复杂的效果。

改变布局除使用麻烦外,无其他缺点,其主要使用一些有交互性的View,因为这些View需要和用户交互,直接通过动画区实现会有问题,改变布局参数可以解决

* scrollTo/scrollBy：操作简单,适合对View内容的滑动
* 动画：操作简单,主要用于没有交互的View和实现复杂的动画效果
* 改变布局参数：操作稍微复杂,适用于有交互的View

例：实现一个跟手滑动的效果,拖动它可以在整个屏幕上随意滑动。重写onTouchEvent方法并处理ACTION_MOVE事件,根据两次滑动之间的距离可以实现滑动,为实现全屏滑动,采用动画的方式实现,无法采用scrollTo实现,可以采用改变布局来实现

```
public boolean onTouchEvent(MotionEvent event){
	int x = (int) event.getRawX();
	int y = (int) event.getRawY();
	switch (event.getAction()){
		case MotionEvent.ACTION_DOWN:{
			break;
		}
		case MotionEvent.ACTION_MOVE:{
			int deltaX = x - mLastX;
			int deltaY = y - mLastY;
			Log.d("TAG", "move, deltaX:" + deltaX + "deltaY:" + deltaY);
			int translationX = (int) ViewHelper.getTranslationX(this) + deltaX;
			int translationY = (int) ViewHelper.getTranslationX(this) + deltaY;
			ViewHelper.setTranslationX(this, translationX);
			ViewHelper.setTranslationY(this, translationY);
			break;
		}
		case MotionEvent.ACTION_UP:{
			break;	
		}
		default:
			break;
	}
	mLastX = x;
	mLastY = y;
	return true;
}

```

首先通过getRawX()和getRawY()方法来获取手指当前的左边,不能使用getX()和getY(),因为需要实现全屏滑动,所以需要获取当前点击事件在屏幕中的坐标而不是相对于View本身的左边,其次,要得到两次滑动之间的位移,有了这个位移就可以移动当前的View,移动方法采用的是动画兼容库nineoldandroids中的ViewHelper类所提供的setTranslationX()和setTranslationY()。ViewHelper还提供了一系列get/set方法,因为View的setTranslationX和setTranslationY只能在android 3.0及以上版本才能使用,用ViewHelper所提供的方法是没有版本要求的,与此类似的还有setX,setScalrX,setAlpha等方法。这一系列方法实际上是为属性动画服务的。由于动画的性质,3.0以下版本无法在新位置响应用户的点击事件

#####6、弹性滑动

######1.使用Scroller

```java
Scroller scroller = new Scroller(mContent);
//	缓慢滚动到指定位置
private void smoothScrollTo(int destX, int destY){
    int scrollX = getScrollX();
    int deltaX = destX - scrollX;
    //	1000ms内滑动destX,效果就是缓慢滑动
    mScroller.startScroll(scrollX, 0, deltaX, 0, 1000);
    invalidate();
}

public void computeScroll(){
    if(mScroller.computerScrollOffset()){
        scrollTo(mScroller.getCurrX(), mScroller.getCurrY());
        postInvaldate();
    }
}
```

* Scroller的典型实用方法，工作原理如下：

  先构造一个Scroller对象并且调用它的startScroll方法时，Scroller内部什么也没做，它只是保存了我们传递的几个参数。

  ```Java
  public void startScroll(int startX, int startY, int dx, int dy, int duration){
      mMode = SCROLL_MODE;
      mFinished = false;
      mDuration = duration;
      mStartTime = AnimationUtils.currentAnimalTimeMillis();
      mStartX = startX;
      mStartY = startY;
      mFinalX = startX + dx;
      mDeltaX = dx;
      mDeltaY = dy;
      mDurationReciprocal = 1.0f / (float) mDuration;
  }
  ```

  这个方法的参数含义很清楚，startX和startY表示的是滑动的起点，dx和dy表示的是要滑动的距离，而duration表示的是滑动事件，即整个滑动过程完成所需要的时间，注意这里的滑动是指View内容的滑动而非View本身位置的改变



### 3、View的工作原理

##### 1、初始ViewRoot和DecorView

​	viewRoot对应于ViewRootImpl类，它是连接WindowManager和DecorView的纽带，View的三大流程均是通过ViewRoot来完成的，在ActivityThread中，当Activity对象被创建完毕后，会讲DecorView添加到Window中，同时会创建ViewRootImpl对象，并将ViewRootImpl对象和DecorView建立关联。

```java
root = new ViewRootImpl(view.getContext(), display);
root.setView(view, wparams, panelParentView);  
```

View的绘制流程是从ViewRoot的performTraversals方法开始的，它经过measure、layout和draw三个过程才能最终将一个View绘制出来，其中measure是用来测量View的宽和高，layout用来确定View在父容器中的放置位置，而draw则负责将View绘制在屏幕上。11