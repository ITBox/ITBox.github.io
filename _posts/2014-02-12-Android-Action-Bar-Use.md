---
layout: post
category: android
title: Android中ActionBar的使用
description: "设计漂亮的Android应用汇总."
modified: 2014-02-11
tags: [actionbar]
comments: true
share: true
---
* 随着AndroidDesign的Holo风格越来越普及，Android应用程序也有了自己的设计风格，微信5.2也转向的Holo风格，ActionBar是Holo风格中重要的元素，接下来笔者简单介绍ActionBar如何应用到项目中。
* ActionBar是android3.0之后（API Level 11）才加入的，所以要想兼容低版本，需要导入支持库v7，v7是13年推出的，在此之前一般是使用Github上得开源项目ActionBarSherlock，既然Google已经推出了支持库，ActionBarSherlock就没必要再介绍了。

* 如果还有v7支持库，打开SDK Manager安装即可

![SDK Manager](https://github.com/baoyongzhang/test_pages/blob/gh-pages/image-1.png?raw=true)

* 安装之后，在sdk目录下的extras目录中可应该可以找到v4,v7,v13支持库，v7中包含了三个项目，ActionBar只需要使用v7中的appcompat项目。

![v7 appcompat](https://github.com/baoyongzhang/test_pages/blob/gh-pages/image-2.png?raw=true)


* 接下来新建一个Android Project，笔者使用的是eclipse，然后导入v7中得appcompat项目，这里要注意一下，appcompat项目是包含资源文件的（比如ActionBar的背景图片），只导入jar包是不行的，我们的工程需要关联appcompat项目，如何关联请参考Android Library项目的使用。

* 在清单文件中配置activity的Theme，可以在application中配置全局Theme，appcompat提供了三种Theme：
```
		黑色主题：@Style/Theme.AppCompat
		白色主题：@Style/Theme.AppCompat.Light
		白色主题，黑色ActionBar：@Style/Theme.AppCompat.Light.DarkActionBar
```

* 也可以自定义Style继承上面几种Theme。

{% highlight xml %}
	<application
        	android:allowBackup="true"
        	android:icon="@drawable/ic_launcher"
        	android:label="@string/app_name"
        	android:theme="@style/Theme.AppCompat" >
{% endhighlight %}

* 配置完主题之后，我们需要将Activity继承ActionBarActivity，如果需要使用v4包中的FragmentActivity也不需要担心，因为ActionBarActivity继承了FragmentActivity。

* 在Activity中调用getSupportActionBar()方法可以获取ActionBar对象，ActionBar默认是显示的，如果想隐藏可以调用ActionBar.hide()方法，显示则调用ActionBar.show(); 如果想更改ActionBar的背景可以调用ActionBar.setBackgroundDrawable();


{% highlight java %}
	public class BaseActivity extends ActionBarActivity {
		ActionBar actionBar = getSupportActionBar();
		// 隐藏ActionBar
		actionBar.hide();
		// 设置ActionBar背景
		actionBar.setBackgroundDrawable(new ColorDrawable(Color.RED));
	}
{% endhighlight %}

* 显示“返回按钮”，就是左上角logo左侧有个箭头，可以点击之后关闭当前Activity，这里说“返回按钮”并不是很准确。调用ActionBar.setDisplayHomeAsUpEnabled(true);即可显示。

{% highlight java %}
	actionBar.setDisplayHomeAsUpEnabled(true);
{% endhighlight %}

* 不过点击并不会有“返回”的效果，我们需要设置它的点击事件。

{% highlight java %}
	@Override
	public boolean onOptionsItemSelected(MenuItem item) {
		switch (item.getItemId()) {
		case android.R.id.home:
			finish();	// 点击“返回”关闭当前Activity
			return true;
		default:
			return super.onOptionsItemSelected(item);
		}
	}
{% endhighlight %}

`未完待续...`
