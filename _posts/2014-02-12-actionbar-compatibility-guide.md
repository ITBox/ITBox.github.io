---
layout: post
category: android-development
title: 使用Support Library实现ActionBar
description: ""
modified: 2014-02-11
tags: [actionbar]
comments: true
share: true
image:
  feature: abstract-3.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
comments: true
---



![](https://github.com/ITBox/ITBox.github.io/blob/master/images/D06A00CC-E866-4B48-B09A-41609D28B275.png?raw=true)



随着AndroidDesign的Holo风格越来越普及，Android应用程序也有了自己的设计风格，微信5.2也转向的Holo风格，ActionBar是Holo风格中重要的元素，接下来我将简单介绍ActionBar如何应用到项目中。
 
[ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)是android3.0之后（API Level 11）才加入的。在此之前一般是使用Github上的开源项目[ActionBarSherlock](https://github.com/JakeWharton/ActionBarSherlock)来兼容低版本。2013年google发布了v7 appcompact library,可以让开发者在android 2.1(API level 7)以及以上版本上使用ActionBar。我们这里主要讲解一下使用v7 appcompact library来实现ActionBar。

扩展阅读：[ActionBarSherlock和ActionBar Compatibility区别](http://stackoverflow.com/questions/7844517/difference-between-actionbarsherlock-and-actionbar-compatibility)

###ActionBar的显示与隐藏

 如果还没有v7支持库，打开SDK Manager安装即可

![SDK Manager](https://github.com/baoyongzhang/test_pages/blob/gh-pages/image-1.png?raw=true)

安装之后，在sdk目录下的extras目录中可应该可以找到v4,v7,v13支持库，v7中包含了三个项目，ActionBar只需要使用v7中的appcompat项目。

![v7 appcompat](https://github.com/baoyongzhang/test_pages/blob/gh-pages/image-2.png?raw=true)


接下来新建一个Android Project，我使用的是eclipse，然后导入v7中得appcompat项目，这里要注意一下，appcompat项目是包含资源文件的（比如ActionBar的背景图片），只导入jar包是不行的，我们的工程需要关联appcompat项目，如何关联请参考Android Library项目的使用。

在清单文件中配置activity的Theme，可以在application中配置全局Theme，appcompat提供了三种Theme：

> * 黑色主题：@Style/Theme.AppCompat
* 白色主题：@Style/Theme.AppCompat.Light
* 白色主题，黑色ActionBar：@Style/Theme.AppCompat.Light.DarkActionBar


也可以自定义Style继承上面几种Theme。


配置完主题之后，我们需要将Activity继承ActionBarActivity，如果需要使用v4包中的FragmentActivity也不需要担心，因为ActionBarActivity继承了FragmentActivity。

在Activity中调用getSupportActionBar()方法可以获取ActionBar对象，ActionBar默认是显示的，如果想隐藏可以调用ActionBar.hide()方法，显示则调用ActionBar.show(); 如果想更改ActionBar的背景可以调用ActionBar.setBackgroundDrawable();


{% highlight java %}

	    ActionBar actionBar = getSupportActionBar();
	    if (actionBar.isShowing()) {// 判断ActionBar是否显示
			actionBar.hide();// 隐藏ActionBar
	    } else {
			actionBar.show();// 显示ActionBar
	    }

{% endhighlight %}

####使用Logo替换icon####

默认的,ActionBar调用应用图标。如果在<application>或者<activity>元素中，指定logo属性，ActionBar将使用logo替代icon


**ActionBar一些常用的方法**

* setBackgroundDrawable(Drawable drawable):为ActionBar设置背景。
* setDisplayHomeAsUpEnabled(boolean b)：是否显示返回的按钮。
* setDisplayShowHomeEnabled(boolean b);是否显示icon
* setDisplayShowTitleEnabled(boolean b);是否显示标题
* setDisplayShowCustomEnabled(boolean b);是否显示自定义view
* setIcon();设置Icon
* setTitle();设置标题

###添加Action Item###
当你开启Activity时，系统通过调用Activity的onCreateOptionsMenu()方法来放置action items。使用这个方法inflate一个定义所有action items的菜单资源。

{% highlight xml%}

<menu xmlns:android="http://schemas.android.com/apk/res/android" >
    <item android:id="@+id/action_search"
          android:icon="@drawable/ic_action_search"
          android:title="@string/action_search"/>
    <item android:id="@+id/action_compose"
          android:icon="@drawable/ic_action_compose"
          android:title="@string/action_compose" />
</menu>

{% endhighlight %}

然后调用Activity的onCreateOptionsMenu()方法中添加将所有的action item添加到ActionBar上。

{% highlight java%}

@Override
public boolean onCreateOptionsMenu(Menu menu) {
    // Inflate the menu items for use in the action bar
    MenuInflater inflater = getMenuInflater();
    inflater.inflate(R.menu.main_activity_actions, menu);
    return super.onCreateOptionsMenu(menu);
}

{% endhighlight %}

想要item直接显示在actionbar上，需要在<item>标签中，添加一个showAsAction="ifRoom"属性。

{% highlight xml%}

<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:yourapp="http://schemas.android.com/apk/res-auto" >
    <item android:id="@+id/action_search"
          android:icon="@drawable/ic_action_search"
          android:title="@string/action_search"
          yourapp:showAsAction="ifRoom"  />
    ...
</menu>
{% endhighlight %}

如果没有足够的空间，它将以悬浮菜单的样式显示。

####使用support library的属性####
注意上面的showAsAction 属性使用了一个自定义命名空间。在使用support library定义的属性必须自定义命名空间。因为这些属性在一些老的Android设备上不存在。

如果同时指定了title和icon属性，action item默认只显示icon.如果需要显示标题，需要为showAsAction属性添加withText值。

{% highlight xml%}
<item yourapp:showAsAction="ifRoom|withText" ... />
{% endhighlight %}

如果icon可用并且actionbar 空间不足时，title将不显示。

尽管可能你不需要显示title，但是仍然要指定title属性的值。
* 如果空间不足，菜单将以悬浮状态显示，并且只显示title。
* 如果action item只显示icon,用户可以通过长按条目显示title。

你也可以设置showAsAction属性的值为always，让一个action item一直显示，但是你最好不要让一个action item一直显示。这样做在窄屏上出现一些布局适配的问题。

####处理条目点击#### 
当用户点击一个条目时，系统将点击的MenuItem传递给Activity的onOptionsItemSelected() 方法。

{% highlight java%}
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    // Handle presses on the action bar items
    switch (item.getItemId()) {
        case R.id.action_search:
            openSearch();
            return true;
        case R.id.action_compose:
            composeMessage();
            return true;
        default:
            return super.onOptionsItemSelected(item);
    }
}
{% endhighlight %}

向上返回的按钮的id是android.R.id.home 所以通过下面代码就能实现点击返回按钮返回的功能。
{% highlight java%}

 setDisplayHomeAsUpEnabled(true);//显示返回箭头
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    // Handle presses on the action bar items
    switch (item.getItemId()) {
        case android.R.id.home:
                finish();
        default:
            return super.onOptionsItemSelected(item);
    }
}

{% endhighlight %}

####使用分离的ActionBar####

![](http://developer.android.com/images/ui/actionbar-splitaction@2x.png)
ActionBar可以分割成屏幕上方和屏幕下方两部分类显示（如上图所示）。分割的ActionBar可以让空间能够更合理的利用。

为了实现分离的ActionBar，必须做以下两件事情：

1. 添加uiOptions="splitActionBarWhenNarrow"到<activity>元素或 <application> 元素中。这个属性只能在API 14+上起作用，低版本将忽略这个属性。
2.为了支持低版本，在<activity>元素中添加一个<mata-data>元素，

{% highlight xml%}

<manifest ...>
    <activity uiOptions="splitActionBarWhenNarrow" ... >
        <meta-data android:name="android.support.UI_OPTIONS"
                   android:value="splitActionBarWhenNarrow" />
    </activity>
</manifest>
{% endhighlight %}









