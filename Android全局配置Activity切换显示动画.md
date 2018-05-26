# 全局配置Activity切换显示动画

####1、theme属性

AndroidManifest.xml 修改application节点 的Android：theme属性



####2、style属性

```
<resources>  
  
    <!-- Base application theme. -->  
    <style name="BaseTheme" parent="Theme.AppCompat.Light.NoActionBar">  
        <!-- Customize your theme here. -->  
        <item name="colorPrimary">@color/colorPrimary</item>  
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>  
        <item name="colorAccent">@color/colorAccent</item>  
        <item name="windowActionBar">false</item>  
        <item name="windowNoTitle">true</item>  
        <!-- 设置activity切换动画 -->  
        <item name="android:windowAnimationStyle">@style/activityAnimation</item>  
    </style>  
  
    <!-- animation 样式 -->  
    <style name="activityAnimation" parent="@android:style/Animation">  
        <item name="android:activityOpenEnterAnimation">@anim/slide_right_in</item>  
        <item name="android:activityOpenExitAnimation">@anim/slide_left_out</item>  
        <item name="android:activityCloseEnterAnimation">@anim/slide_left_in</item>  
        <item name="android:activityCloseExitAnimation">@anim/slide_right_out</item>  
    </style>  
  
    <style name="AppTheme" parent="BaseTheme">  
</resources>
```

####3、动画效果

在res目录下的anim（如果没有此文件夹，右键res新建）文件夹下，新建四个动画xml文件

* slide_left_in.xml

```
<?xml version="1.0" encoding="utf-8"?>  
<set xmlns:android="http://schemas.android.com/apk/res/android" >  
    <translate  
        android:duration="200"  
        android:fromXDelta="-100.0%p"  
		android:toXDelta="0.0" />  
</set>
```

* slide_left_out.xml

```
<?xml version="1.0" encoding="utf-8"?>  
<set xmlns:android="http://schemas.android.com/apk/res/android" >  
    <translate  
        android:duration="200"  
        android:fromXDelta="0.0"  
        android:toXDelta="-100.0%p" />  
</set>
```

* slide_right_in.xml

```
<?xml version="1.0" encoding="utf-8"?>  
<set xmlns:android="http://schemas.android.com/apk/res/android" >  
    <translate  
	    android:duration="200"  
        android:fromXDelta="100.0%p"       android:toXDelta="0.0" />  
</set>  
```

* slide_right_out.xml

```
<?xml version="1.0" encoding="utf-8"?>  
<set xmlns:android="http://schemas.android.com/apk/res/android" >  
    <translate  
        android:duration="200"  
        android:fromXDelta="0.0"  
        android:toXDelta="100.0%p" />  
</set>
```

