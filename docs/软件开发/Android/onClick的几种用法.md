---
title: onClick 的几种用法
date: 2016-9-26 14:30
categories:
  - 笔记
tags:
  - Android
---

![1](https://img.alicdn.com/imgextra/i3/1064479076/TB2g.bWtXXXXXXcXXXXXXXXXXXX_!!1064479076.jpg)

> Android 控件的点击事件的几种用法

# 1. 匿名内部类 



---



# 2. 接口实现



<!-- more -->

---

# 3. onClick 控件属性

* 首先在需要设置点击事件的控件的属性中添加

  android:onClick="method"

  > *method*  是点击事件响应的方法名

* 然后在加载当前布局的类中创建 method 方法

  ## Demo

  布局

  ```xml
  <Button
  	android:id="@+id/button_1"
  	android:layout_width="wrap_content"
      android:layout_height="wrap_content" 
  	android:onClick="onClick"
  	android:text="@string/button1_name" />
  <Button
  	android:id="@+id/button@"
  	android:layout_width="wrap_content"    				
  	android:layout_height="wrap_content" 
  	android:onClick="onClick"  
  	android:text="@string/button1_name" />
  ```

  类文件

  ````java
  public class MainActivity extends Activity {
  	@Override
  	protected void onCreate(Bundle savedInstanceState) {  
        	super.onCreate(savedInstanceState);
  		setContentView(R.layout.activity_main);            
  	}

  	public void onClick (View v) {
      	switch (v.getId) {
              case R.id.button_1:
                  Toast.makeText(MainActivity.this,
                 	"Button_1",Toast.LENGTH_SHORT).show();
                  break;
  			case R.id.button_2:
              	Toast.makeText(MainActivity.this, 						"Button_2",Toast.LENGTH_SHORT).show();
              break;
      	}
  	}
  }
  ````