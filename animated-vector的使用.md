# animated-vector

直接在布局文件里调用背景即可

```
<animated-vector xmlns:android="http://schemas.android.com/apk/res/android"  
                 android:drawable="@drawable/vectordrawable" >  
  <target  
      android:name="rotationGroup"  
      android:animation="@anim/rotation" />  
  <target  
      android:name="v"  
      android:animation="@anim/path_morph" />  
</animated-vector>
```


旋转动画

```
<objectAnimator  
    xmlns:android="http://schemas.android.com/apk/res/android"  
    android:duration="6000"  
    android:propertyName="rotation"  
    android:valueFrom="0"  
    android:valueTo="360"/>
```

 变形动画

```
 <objectAnimator  
    xmlns:android="http://schemas.android.com/apk/res/android"  
    android:duration="3000"  
    android:propertyName="pathData"  
    android:valueFrom="M300,70 l 0,-70 70,70 0,0   -70,70z"  
    android:valueTo="M300,70 l 0,-70 70,0  0,140 -70,0 z"  
    android:valueType="pathType"/> 
```
