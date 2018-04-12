# Android menu属性


```
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@[+][package:]id/resource_name"
          android:title="string"
          android:titleCondensed="string"
          android:icon="@[package:]drawable/drawable_resource_name"
          android:onClick="method name"
          android:showAsAction=["ifRoom" | "never" | "withText" | "always" | "collapseActionView"]
          android:actionLayout="@[package:]layout/layout_resource_name"
          android:actionViewClass="class name"
          android:actionProviderClass="class name"
          android:alphabeticShortcut="string"
          android:numericShortcut="string"
          android:checkable=["true" | "false"]
          android:visible=["true" | "false"]
          android:enabled=["true" | "false"]
          android:menuCategory=["container" | "system" | "secondary" | "alternative"]
          android:orderInCategory="integer"/>
    <group android:id="@[+][package:]id/resource name"
           android:checkableBehavior=["none" | "all" | "single"]
           android:visible=["true" | "false"]
           android:enabled=["true" | "false"]
           android:menuCategory=["container" | "system" | "secondary" | "alternative"]
           android:orderInCategory="integer">
        <item/>
    </group>
    <item>
        <menu>
          <item/>
        </menu>
    </item>
</menu>

```


* android:id	定义资源ID
   android:title	菜单的标题
    android:titleCondensed	简要标题(标题太长时使用)
    android:icon		菜单项图标
    android:onClick		点击事件(必须用public声明,只接受一个MenuItem对象,这个对象指明了被点击的菜单项。这个方法会优先标准的回调方法：onOptionsItemSelected()。
   * 警告：如果要使用ProGuard（或类似的工具）来混淆代码，就要确保不要重名这个属性所指定的方法，因为这样能够破坏功能。
      这个属性在API级别11中被引入。

      android:showAsAction	 显示时机和方式
   * 只用Activity包含了一个ActionBar对象时，菜单项才能够作为操作项来显示。这个属性在API级别11中被引入，有效值如下：

   * ifRoom 如果有针对这个项目的空间，则只会把它放到操作栏中
     withText 操作项也要包含文本（通过android:title属性来定义的）。可以把这个值与其他的Flag设置放到一起，通过管道符“|”来分离它们。
   * never 这个项目不会放到操作栏中
   * always 始终包这个项目放到操作栏中。要避免使用这个设置，除非在操作栏中始终显示这个项目是非常关键的。设置多个项目作为始终显示的操作项会导
     致操作栏中其他的UI溢出。
     icollapseActiionView 它定义了跟这个操作项关联的可折叠的操作View对象（用android:actionViewLayout来声明）。这个关键词在API级别14中被引入。

     android：actionViewLayout	引用布局资源(API级别11中被引入)
     android:actionProviderCalss	 类名，它是操作项目所使用的ActionProvider类的完整的类名。例如
   * “android.widget.ShareA ctionProvider”说明要使用
      ShareActionProvider			类。
    * 警告：如果要使用ProGuard（或类似的工具）来混淆代码，就要确保不要重名这个属性所指定的方法，因为这样能够破坏功能。
       这个属性在API级别14中被引入。

* android:alphabeticShortcut  定义一个字符快捷键

   android:numbericShortcut	定义一个数字快捷键

    android:checkable	 菜单项设置是否可以复选
    android:checked		菜单项是否可被选择
    android:visible		菜单项是否可见
    android:enabled		菜单项事都可用
    android:menuCategory	  菜单项优先级，有效值如下：
   值 说明

   * container 菜单项是容器的一部分
   * system 菜单项是由系统提供的。
   * secondary 提供给用户的辅助选择的菜单项（很少使用）
   * alternative 基于当前显示的数据来选择操作的菜单项。

     android:orderInCategory	整数值，它定义菜单项在菜单组中的重要性的顺序

     <group>	定义一个菜单组(menu子元素)
     android:id	group项资源ID
     android:checkableBeharior	菜单项组的可复选行为

   * none 没有可复选性
   * all 组内的所有的项目都被复选（使用复选框）
   * single 仅有一个项目能够被复选（使用单选按钮）
     android:visible		菜单组是否可见
     android:enabled		菜单组是否可用
     android:menuCategory	  菜单组优先级
   * container 菜单组是容器的一部分
   * system 菜单组是由系统提供的。
   * secondary 提供给用户的辅助选择的菜单组（很少使用）
   * alternative 基于当前显示的数据来选择操作的菜单组

