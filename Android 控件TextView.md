### 一、 TextView的文字长度测量及各种padding解析



#### 1、TextView里各种padding的含义?

>  任何一个控件都是一个矩形区域，它有padding以及margin属性。

* TextView获取各种padding长度的接口

> getWidth()
>
> getHeight()
>
> getPaddingLeft/Right/Top/Bottom()
>
> getCompoundPaddingLeft/Right/Top/Bottom()
>
> getExtendedPaddingBottom/Top()
>
> getTotalPaddingLeft/Right/Top/Bottom)()



TextView最外层是一个矩形，

1. 它的宽高通过  `getWidth() ` 和  `getHeight()`  获取。对应你代码里的layout_width和layout_height

2. 里面有drawable和文字区域。文字在最中间，文字的四周  `drawableLeft`  ，`drawableRight`  ，`drawableTop ` ，`drawableBottom`  四个区域，它们属于同级。其中的  `paddingLeft`  ，`paddingRight ` ，`paddingTop`  ，`paddingBotoom`  是最外层和drawable文字的距离。对应  `getPaddiingLeft/Right/Top/Bottom()`

3. `getCompoundPaddingLeft/Top/Right/Bottom()`  翻译成中文就是获取混合的Padding, 它是获取文字区域到TextView边界之间的间隔

4. `getExtendedPaddingTop()`  这个是当有部分文字没有显示出来时，也就是设置了maxLine时，它的值就等于首行文字到TextView顶端的距离。同理，getExtendedPaddingBottom()就是最后一行文字到TextVeiw底部距离。其他情况下，他的值等于getCompoundPaddingTop/Bottom()的值

5. `getTotalPaddingLeft/Right/Top/Bottom()`  获取总的Padding。左右的值等于  `CompoundPadding`  的值，上下的值等于  `ExtendedPadding`  的值加上offset的值。也就是首行到TextView顶端和末行到TextView底部的值

   ​	![img](https://upload-images.jianshu.io/upload_images/1924341-5f1d2aaebafe69ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

 

####二、计算每行文字的长度



##### 方法一

> TextView.getPaint().measureText(String text)

该方法计算文字区域在TextView的总长度



##### 方法二

> TextView.getLayout().getLineWidth(int line)

该方法计算每一行的实际长度，当设置single属性时，测量的是一整行文字的长度，包括溢出部分



##### android:maxLines="1"和android:singleLine="true"有什么区别?

官方是推荐不再使用singleLine，用maxLines="1"代替

>maxLines：限制TextView的最高高度，大概就是指通过限制行数来限制最高高度。
>
> singleLine: 强制设置TextView的文字不换行。

* 区别就是：
  * maxLines还是会默认自动进行换行策略，假如一段文字自动换行后有5行，maxLines设置为1，那么就只显示第一行的内容，其他行不显示。 
  * 设置singleLine, 那么这段可以有5行的文字将会被强制放在1行里，然后看最多能显示多少字符，剩下的不显示



##### 为什么设置android:maxLines="1"时TextView的跑马灯效果就不能正常工作？

* 跑马灯要启动要同时满足四个条件，其中有一个条件就是这一行的文字长度要大于文字区域的宽度，文字区域的宽度就是TextView的getWidth()扣去ComPoundpaddingLeft再扣去CompoundPaddingRight剩下的长度。
* 如果是maxLines="1"的话，所有的文字会被自动换行，只显示第一行，而换行是什么，就是为了让每行文字的长度超过文字区域的宽度才进行的换行，也就是说，如果一段文字经过TextView的换行后，那么每行的文字长度都不会超过文字区域的长度。
* singleLine的话，不会对一段文字进行换行处理，就会超过文字区域的长度，所以如果要设置跑马灯效果的话，只能用singleLine不能用maxLines="1"。

 

 

 

 

 