---
title: Android-布局
date: 2021-06-02 21:52:46
categories: Android
tags: [Android,Java,backend]
---

## 1. Activity

Activity是指一个可视化的界面,即一个主界面

```java
import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```



### 1.1 onCreate()方法

当界面被创建时首先执行的方法,包含一些初始化方法

如上文的onCreate()方法,首先执行继承自父类的onCreate()方法,然后执行setContentView()方法

在IDE中使用`Ctrl+Q`观察这个方法的方法签名

![image-20210602215857563](https://gitee.com/cao_ziqiang/img/raw/master/20210602215857.png)

发现我们需要传入这个布局资源的id。

### 1.2 R文件

在activity中会由系统来自动生成R.java文件,替我们把每一个资源都设置好了资源索引

```java
	@Override
	protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //设置内容视图
        //R:为每一个资源文件按照类别来分配一个索引
        //使我们可以通过R.类别名.资源索引来操作资源
        setContentView(R.layout.activity_main);
    }
```

### 1.3 layout文件

![image-20210603123939470](https://gitee.com/cao_ziqiang/img/raw/master/20210603123939.png)

在app模式下,打开res目录下的layout文件夹,这里为每一个activity指定了它的布局

### 1.4 清单文件

在APP模式下,打开manifests目录,在这个目录下有一个AndroidManifest.xml文件,里面配置了activity

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="cn.homyit.testapplication2">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.TestApplication2">
        <activity android:name=".MainActivity">
            <intent-filter>
                <!--将这个activity设置为启动界面-->
                <action android:name="android.intent.action.MAIN" />
				<!--将这个应用在应用列表里形成一个图标-->
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

每创建一个activity都需要在application中创建一个activity标签,并且指定它的android:name属性

## 2. 布局

布局是指对界面结构的全面规划与安排，通过api中提供的各种布局能够快速的完成对于界面的设计

在Android开发中可以使用xml和纯java代码两种方式来设置布局,但是我们一般使用xml文件来设置布局

布局的几个重要属性

```xml
android:layout_width	宽度

android:layout_height	高度

android:layout_padding	内边距

android:layout_margin	外边距
```

### 2.1 线性布局(LinearLayout)

 线性布局即依次从上往下从左到右来摆放和布局,常用与聊天窗口

几个重要属性

```xml
android:orientation		=>	方向(设置每个控件的摆放方向是水平摆放还是垂直摆放)
android:layout_weight	=>	权重(设置每个控件的权重,即所占一行的比重或者是一列的比重)
android:layout_gravity	=>	重力(设置每个控件的摆放位置相对于父容器的摆放位置)
```

xml文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">


    <!--vertical 垂直的 horizontal 水平的-->
    <!--layout_weight设置权重-->
    <TextView
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="homyit工作室"
        android:background="#fff000"
        android:layout_weight="1"
        android:layout_gravity="bottom"/>

    <TextView
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="homyit工作室"
        android:background="#ff0000"
        android:layout_weight="1"
        android:layout_gravity="center"/>

    <TextView
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="homyit工作室"
        android:background="#aaaaaa"
        android:layout_weight="1"
        android:layout_gravity="top"/>

</LinearLayout>
```

效果图:

![image-20210603200426718](https://gitee.com/cao_ziqiang/img/raw/master/20210603200426.png)

经典布局案例---聊天窗口

![image-20210622170857037](https://gitee.com/cao_ziqiang/img/raw/master/20210622170857.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="60dp"
        android:orientation="horizontal"
        android:background="#333333">

        <TextView
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_gravity="center_vertical"
            android:paddingLeft="15dp"
            android:text="<"
            android:textColor="#ffffff"
            android:textSize="30sp"
            android:layout_weight="1"/>

        <TextView
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_gravity="center_vertical"
            android:text="小P"
            android:textColor="#ffffff"
            android:textSize="30sp"
            android:layout_weight="1"/>
        <ImageView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:src="@mipmap/ic_launcher_round"/>
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_weight="1"
        android:orientation="horizontal"></LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="60dp"
        android:orientation="horizontal"
        android:background="#333333">
        <ImageView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:src="@mipmap/ic_launcher_round"
            android:layout_gravity="center_vertical"
            android:layout_weight="1"/>
        <ImageView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:src="@mipmap/ic_launcher_round"
            android:layout_gravity="center_vertical"
            android:layout_weight="1"/>
        <ImageView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:src="@mipmap/ic_launcher_round"
            android:layout_gravity="center_vertical"
            android:layout_weight="1"/>

    </LinearLayout>
</LinearLayout>
```

### 2.2 相对布局(RelativeLayout)

相对于父容器（取值: true/false)，如:android:layout_alignParentRight

相对于其他控件（取值:其他控件id)，如:android:layout_toRightOf



### 2.3 帧布局(FrameLayout)





### 2.4  表格布局(TableLayout)





### 2.5 网格布局(GridLayout)





### 2.6 约束布局(ConstraintLayout)



