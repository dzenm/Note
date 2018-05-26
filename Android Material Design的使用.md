

### 一、AppBarLayout的五种ScrollFlags

#### 1、scroll

> Child View伴随着滚动事件而滚出或滚进屏幕。
>
> * Scroll可单独使用，其他值必须配合Scroll使用
> * Child View之中必须有有一个View设置该值，否则不起作用



* scroll的使用

child view和scrolling view连接成一个View的滑动模式

![img](https://upload-images.jianshu.io/upload_images/1094967-877bd6d61a4e22f9.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/338)



#### 2、enterAlways

> 快速返回模式：向下滚动时child view和scrolling view之间滚动的优先级
>
> * scroll优先滚动scrolling view
> * scroll|enterAlways优先滚动child view。直到先滚动的滚进屏幕之后再进行另一个滚动

* scroll和enterAlways的配合使用。

![img](https://upload-images.jianshu.io/upload_images/1094967-e0873ea34c4637a5.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/338)



#### 3、enterAlwaysCollapsed

> enterAlways的附加值
>
> * 存在一个最小高度，只针对向上滚动，先滚动child view到最小高度，然后滚动scrolling view完。再滚动剩下部分



* 需要三个一起使用scroll|enterAlways|enterAlwaysCollapsed

![img](https://upload-images.jianshu.io/upload_images/1094967-4ab6365b7fac590e.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/338)



####4、exitUntilCollapsed

> 存在一个最小高度
>
> * 向上滚动时，child view先滚动至最小高度，然后child view一直保持该高度
> * 向下滚动时，child view需等scrolling view滚动到顶部，才开始滚动child view

* scroll和exitUntilCollapsed的使用。必须设置minHeight

![img](https://upload-images.jianshu.io/upload_images/1094967-6f683857f6d567ca.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/338)



#### 5、snap

> 吸附效果
>
> * child view不会存在局部显示。滚动child view的一部分高度时松开手指，child view 要么全部滚出屏幕，要么全部滚下屏幕



*  scroll和snap的使用

![img](https://upload-images.jianshu.io/upload_images/1094967-7d9619ee3fb0d974.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/338)