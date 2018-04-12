# ImageButton属性src与background的区别


imageButton是带有图标的Button控件,有src属性,也有一个所有控件都有的属性background。
	
* src是设置图片内容前景(原图大小),scaleType只对src缩放,centerInside按钮比例缩放图片
* background是设置背景

  * 当图片资源比控件小,src居中显示,background会拉伸充满控件
  * background是底层图片资源,src是覆盖在上面的资源,可以叠加使用


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
