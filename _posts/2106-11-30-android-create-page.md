---
layout: post
title:  "Android 之 创建简单的用户页面&启动另一个Activity"
date:   2016-11-30 23:12:45
categories: Android
excerpt: Android create page
---

* content
  {:toc}



###  创建简单的用户页面

Android程序的图像用户界面使用一个层级结构View和ViewGroup对象构成。View对象一般是button或者text field这些UI部件，ViewGroup对象是不可见的视图容器，定义了子视图的布局，比如一个网格布局或者一个垂直列表。

Android程序的图像用户界面使用一个层级结构View和ViewGroup对象构成。View对象一般是button或者text field这些UI部件，ViewGroup对象是不可见的视图容器，定义了子视图的布局，比如一个网格布局或者一个垂直列表。
Android 提供一个XML词汇表对应View和ViewGroup的子类，所以你可以在XML中使用层次结构的UI元素来定义你的UI。![](http://my.csdn.net/uploads/201208/01/1343827041_4739.png)

#### 创建一个线性布局

​	打开res/layout文件夹中的activity_main.xml文件。

​	BlankActivity模板默认创建的activity_main.xml文件使用RelativeLayout作为根视图，一个TextView作为子视图。

​	首先，删除`<TextView>`元素，把`<RelativeLayout>`元素改成`<LinearLayout>`。然后添加android:orientation属性，并设置值为"horizontal"。结果如下：

```html
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:orientation="horizontal"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent" >  
</LinearLayout>  
```

LinearLayout是一个视图组（ViewGroup的子类），它使用水平或者垂直的方式排列它的子视图，通过android:orientation属性设置。LinearLayout的子视图安装XML中出现的顺序显示在屏幕中。

另外两个属性，android:layout_width和android:layout_height是所有的视图必须有的，用来指定视图的尺寸。

因为LinearLayout是根视图，所以我们设置宽和高为"match_parent"让它填满整个屏幕，这个值的意思就是扩展这个视图的宽和高，直到匹配它的父视图的宽和高。

#### 增加一个文本域

​	在`<LinearLayout>`中添加一个`<EditText>`元素去创建一个用户可编辑的文本域。

​	和每个View对象一样，你需要定义某些XML属性去指定EditText对象的特征。下面是你需要在`<LinearLayout>`中要声明的：

```html
<EditText android:id="@+id/edit_message"  
    android:layout_width="wrap_content"  
    android:layout_height="wrap_content"  
    android:hint="@string/edit_message" />  
```

关于这些属性：

android:id

> 为视图提供一个唯一的标识，你可以在代码中通过这个标识引用这个对象，比如读取和操纵对象。（你会在下一个课程中看到）
>
> 当你要从XML中引用资源对象时，@符号是必须的，它后面跟着资源类型，一个斜线，然后是资源的名称。
>
> 当你第一次定义一个资源ID的时候，在资源类型前加一个+号是必须的。当你编译程序时，SDK工具会使用这个ID名称在项目中的gen/R.java文件中创建一个新的资源ID，这个新的资源ID引用对应的EditText元素。一旦使用这个方法声明了资源ID，使用这个ID的使用就不需要加号了。使用加号仅仅是在指定一个新的资源ID时是必须的，对于实体资源来说不是必须的，比如字符串或者布局。

android:layout_width和android:layout_height

> 这里使用"wrap_content"代替具体的宽高值来指定视图的大小，意思是和填充这个视图的内容一样大。如果你使用"match_parent"的话，EditText会填满整个屏幕，因为它会和它的父视图LinearLayout一样大。更多信息可以查看[Layouts](http://developer.android.com/guide/topics/ui/declaring-layout.html)。

android:hint

> 这个属性的值会在文本域为空的时候显示。这里使用"@string/edit_message"代替一个固定的字符串，它的值在一个单独的字符串资源文件中定义。因为使用的是实体资源（不仅仅是标示符），所以不需要加号。当然，因为你还没有定义一个字符串资源，所以你会看到一个编译错误。下面章节我们会定义一个字符串资源。
>
> 提示：这个字符串资源和元素ID使用了相同名称：edit_message，不过，引用资源的时候会通过资源类型定义作用域（比如id或者string），所以使用相同的名称不会造成冲突。



#### 增加字符串资源

​	当你需要在界面添加文本时，你通常需要指定每个字符串为一个资源。字符串资源可以让你在一个地方管理所有的UI文本，查找和更新都很方便。外部化的字符串也可以让你针对不同的语言本地化你的程序，根据不同的语言提供可替代的字符串资源。

​	你的工程默认包含了res/values/strings.xml文件。打开这个文件，删除名称为"hello_world"的`<string>`元素。添加名称为"edit_message"的元素，值为"Enter a message"。

​	另外再添加一个名为"button_send"，值为"Send"的元素，等会我们会用到。

添加好的strings.xml像这样：

```html
<?xml version="1.0" encoding="utf-8"?>  
<resources>  
    <string name="app_name">My Frist App</string>  
    <string name="edit_message">Enter a message</string>  
    <string name="button_send">Send</string>  
    <string name="menu_settings">Settings</string>  
    <string name="title_activity_main">MainActivity</string>  
</resources>  
```



#### 添加一个按钮并启动另一个Activity

在layout.xml里面，直接拖就行

![](http://ww1.sinaimg.cn/large/006pzljrgw1faajlnyny1j30740exmy5.jpg)

在这个界面可以设置按钮的一些属性~

然后在Text页面可以改它的名字

​	![](http://ww1.sinaimg.cn/large/006pzljrgw1faajpew7zzj30fr0e60u6.jpg)

然后在MainActivity.java里面就可以设置它啦

![](http://ww3.sinaimg.cn/large/006pzljrgw1faajrb26tyj30io04et9a.jpg)

上面就是个例子，有没有看到熟悉的bilibili~

来，测试一下

​	![](http://ww4.sinaimg.cn/large/006pzljrgw1faajx39mtoj309o0ha0tg.jpg)

点一下试试呗

​	![](http://ww1.sinaimg.cn/large/006pzljrgw1faajyiqzxij30900gr769.jpg)

​	有没有很激动呢！！！

​	想和我一起去看 你的名字 嘛~