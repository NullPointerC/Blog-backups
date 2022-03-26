---
title: Core-Java学习笔记-9
categories:
  - Java
tags:
  - Java
  - backend
  - 计算机基础
abbrlink: a44891d8
date: 2021-07-20 08:37:48
---

> Core-Java第10章以及第11章，Java图形用户界面程序设计

## 一、 Java用户界面工具包

在Java1.0出现时，就包含了一个用于基本GUI设计的类库，名为抽象窗口工具包（abstract window toolkit，awt）。基本awt库处理用户界面元素的任务委托给系统，调用系统上的原生GUI工具包。由此，AWT编写的应用很好地屏蔽了平台的差异，可以很简单高效地实现跨平台。

又因为要完全屏蔽系统差异是很难做的，每个系统对于组件的细节不全相同，所以awt编写的程序没有原生的Windows或Macintosh那么好看。后由网景公司（Netscape）创建了IFC的GUI库引入了Java。底层只需要显示窗口即可，屏蔽了底层差异，故而引入了Java1.2标准库Swing。

后来因为Swing对于根据像素绘制和运行速度等问题，Sun公司于2007年引入了JavaFX。JavaFX在JVM上运行，有自己的一套机制。在2011年提供了JavaAPI。从JDK7开始和JDK，JRE一起打包。

## 二、显示窗体

底层窗口（没有包含在其他窗口中的窗口）称为窗体（frame）。AWT库中有一个Frame类，用于描述顶层窗口。这个类的Swing版本称为JFrame，扩展自Frame类。不绘制在画布上。修饰组件由用户的窗口系统绘制，不由Swing绘制。

### 2.1 创建窗体



所有的swing组件必须由事件分派线程配置，这是控制线程，它将鼠标点击和按键等事件传递给用户接口组件。

接下来定义用户关闭窗体时的响应动作，在多个窗体中的程序中，不能在用户关闭其中一个窗体时就让程序退出。

### 2.2 窗体属性

JFrame包含若干个改变窗体外观的方法。

- setLocation方法和setBounds方法用于设置窗体的位置。
- setIconImage方法用于告诉窗口系统在标题栏、任务栏切换窗口等位置显示哪个图标。
- setTitle方法用于改变标题栏的文字。
- setResizable利用一个boolean值确定是否允许用户改变窗体的大小。

下图给出了JFrame类的继承层次。

![image-20210720091713577](http://static.codenote.xyz/img/20210720091713.png)

Component类是所有GUI对象的祖先。Window类是所有窗口对象的超类。

Component类中setLocation(x, y)方法是设置窗口左上方的位置。窗口左上角要位于水平向右x像素，垂直向下y像素。

Component类中setBounds方法实现一步调整组件的大小和位置。setBounds(x, y,width, height)

组件类很多方法都是成对出现，用于获取窗口属性。

Toolkit类相当于一个基地，其中包含有大量与原生窗口系统交互的方法。

## 三、在组件中显示信息

在Java中，窗体被设计为是组件的容器。

要实现一个组件上绘制，需要定义一个扩展了JComponent的类，覆盖paintComponent方法。

这个方法有一个Graphics类型的参数，Graphics对象保存着用于绘制图像和文本的一组设置，如字体和颜色。所有绘制都必须由Graphics完成，其中包括绘制文案、图像和文本的方法。

```java
class MyComponent extends JComponent {
    @Override
    public void paintComponent(Graphics g) {
        code for drawing
    }
}
```

无论何种原因，只要窗口需要重新绘制，事件处理器就会通知组件，从而引发所有组件的paintComponent方法。

用户扩大窗口或极小化窗口后重新恢复就会引发重新绘制的过程。

### 3.1 处理2D图形

要使用Java绘制2D图形，要使用Graphics2D类，只需将Graphics强制转换位Graphics2D。

要绘制一个图像，首先要创建实现了Shape接口的对象，然后调用Graphics2D类的draw方法。

Java2D库针对像素采用的是浮点坐标，而不是整数坐标。内部计算采用单精度float，

后Java2D库又提供了float类型的坐标和double类型的坐标。

![image-20210720100117172](http://static.codenote.xyz/img/20210720100117.png)

其中Rectangle2D是抽象类，有两个子类是静态内部类。

Paint2D类也同样有两个子类。

![image-20210720100720887](http://static.codenote.xyz/img/20210720100721.png)

这里省略了子类，其中Paint和Rectangle是遗留类。

Rectangle2D（矩形）和Ellipse2D（椭圆）对象很容易构造。需要指定

- 左上角的x和y坐标；
- 宽和高。

对于椭圆，这些表示外接矩形。

如：

```java
var e = new Ellipse2D.Double(150, 200, 100, 50);
```

这时会构造一个椭圆，外接矩形左上角位于（150，200）、宽100、高50。

构造椭圆时，通常知道椭圆中心、宽和高，不知道外接矩形。setFrameFromCenter方法使用中心点，但还是要给出四个顶点中的一个。

```java
var ellipse = new Ellipse2D.Double(centerX - width / 2, centerY - height / 2, width, height);
```

要想构造直线对象，需要提供起点和终点。这两个点可以使用Point2D，也可以使用一对数值。

```java
vaf line = new Line2D.Double(start, end);
```

或者：

```java
var line = new Line2D.Double(startX, startY, endX, endY);
```

### 3.2 使用颜色

setPaint方法可以为图形上下文的所有后续绘制选择颜色。

Color类用于定义颜色。也可以使用静态常量也可以使用三色分量创建对象。

### 3.3 使用字体

要想使用某种字体绘制字符，必须先创建Font类对象。只要指定字体名、字体风格和字体大小。

### 3.4 显示图像

可以使用ImageIcon类从文件读取图形：

```java
Image image = new Image(filename).getImage();
```

可以使用drawImage显示图像。

## 四、事件处理

GUI需要不断的监视按键或点击鼠标这样的事件。

### 4.1 基本事件处理概念

在AWT中，事件源（按钮或滚动条）有一些方法，允许注册事件监听器，这些对象会对事件的行为做出所需的响应。

通知一个事件监听器发生了事件，事件的信息会封装一个事件对象。如按钮可以发送ActionEvent对象，窗口可以方式WindowEvent对象。

```java
ActionListener listener = ...;
var button = new JButton("Ok");
button.addActionListener(listener);
```

### 4.2 处理点击事件

### 4.3 指定监听器

### 4.4 适配器类

WindowListener接口 -> WindowAdapter适配器

适配器类实现了接口中所有的方法，但是每个方法都不做任何事情。

### 4.5 动作

动作是封装命令执行、执行命令所需要参数的一个对象。

Action接口扩展了ActionListener接口，可以在使用任何ActionListener的地方使用Action对象。

### 4.6 鼠标事件

### 4.7 继承层次

## 五、首选API

<hr/>

## 一、 Swing和模型、视图、控制器设计模式

组件有三个特征：

- 内容，如，按键的状态（是否按下），或者文本域中文本。
- 外观（颜色，大小等）。
- 行为（对事件的反应）。

基于以上三点特种，Swing的设计者提出除了MVC设计模式。

- 模型（model）：存储内容；
- 视图（view）：显示内容
- 控制器（controller）：处理用户输入

对于大多数Swing类而言，模型类将实现一个名字以Model结尾的接口。

如按钮Button,有一个ButtonModel接口。每个JButton对象都存储着一个按钮模型对象。

## 二、布局管理

### 2.1 布局管理器

将几个按钮包含在一个JPanel对象中，用流布局管理器管理，这是面板的默认布局管理器。

组件放置在容器中，布局管理器会决定容器中组件的位置和大小。

通常，用户界面元素都会扩展Component类，组件可以放置在容器，容器也可以放置在另一个容器。

![image-20210720143134826](http://static.codenote.xyz/img/20210720143134.png)

每个容器都有一个默认的布局管理器，也可使用setLayout重新设置布局管理器。

### 2.2 边框布局

边框布局管理器（border layout manager）是每个JFrame内部窗格的默认布局浏览器。流布局管理器会完全控制每个组件的位置，边框布局让每个组件选择一个位置。将组件放在内容窗格的中、北、南、东或西部。

边框布局会扩展所有组件的尺寸来填满可用空间。

### 2.3 网格布局

网格布局会按行列排列所有组件。不过所有的组件大小都是一样的。

构造布局对象的构造器时，需要指定行数和列数。

## 三、文本输入

可使用JTextField和JTextArea组件来输入文本。JPasswordField只能接收单行文本，不会将内容显示。都继承自JTextComponent，这是一个抽象类，不能构造其对象。

### 3.1 文本域

用来给用户键入文本。

### 3.2 标签和标签组件

标签是容纳文本的组件，没有任何修饰，也不能响应用户的输入。

### 3.3 密码域

密码域会让输入的字符以回显字符（echo character）*表示。

### 3.4 文本区

输入比较多，超过一行时应该使用JTextArea组件来接收输入。

### 3.5 滚动窗格

在Swing中，文本区没有滚动条。将文本区放在滚动窗格中可以加入滚动条。

## 四、选择组件

### 4.1 复选框

JCheckBox

### 4.2 单选按钮

JRadioButton

### 4.3 边框

BorderFactory的静态方法创建、把边框传递给BorderFactory.createTitledBorder、调用setBorder方法将边框添加到组件。

### 4.4 组合框

下拉列表设置成可编辑就可以变为组合框。

### 4.5 滑动条

JSlider

## 五、菜单

### 5.1 构建菜单

JMenuBar

### 5.2 菜单项中的图标

JMenuItem

### 5.3 复选框和单选按钮菜单项

JCheckBoxMenuItem

JRadioButtonMenuItem

### 5.4 弹出菜单

JPopupMenu

### 5.5 键盘助记符和加速器

setMnemonic

setAccelerator

### 5.6 启用和禁用菜单项

启用setEnabled方法

### 5.7 工具条

工具条是按钮条，可以快速访问程序中最常见的命令。

JToolBar

### 5.8 工具提示

setToolTipText方法

## 六、复杂的布局管理

### 6.1 网格包布局

### 6.2 定制布局管理器

## 七、对话框

### 7.1 选项卡对话框

JOptionPane

### 7.2 创建对话框

### 7.3 数据交换

### 7.4 文件对话框









