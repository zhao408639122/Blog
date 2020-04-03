---
title: QT学习笔记：API记录
date: 2020-03-17 16:32:35
tags:
categories: GUI工具
---

> 为了写课设，学习了一下QT，关于QT的所有基础内容会记录在这个帖子里，学习方法是看[
> ProgrammingKnowledge](https://www.youtube.com/channel/UCs6nmQViDpUw0nuIx9c_WvA)的视频，1.75倍速播放大概三个小时就能了解基本的特性与API。

## 环境配置

在[官网](http://download.qt.io/archive/qt/)就可以下载，同时清华和华科都有QT的镜像，考虑到并不会使用QT开发大型项目，基本不会用到什么新特性，我下载的5.12.3版本，下载安装后会附带四个开发工具。

+ QT creator 文本编辑器
+ QT designer UI可视化编辑器
+ QT linguist 语言翻译器
+ QT assistant 官方文档

推荐直接使用QT creator，当然也可以配置MSVC或者VS环境，官网给了功能集成好的编辑器就用呗🤗

## 语言特性

### QT Core与QT GUI

<!-- more -->

QT的两个基本库是QTcore和QTgui

在QTcore中包含QT的所有语言特性

在QTgui中包含QT的图形化类等

### MOC与qmake

QT是可以跨平台的GUI框架，在C\++环境下，编译前将会先使用MOC(Meta-Object-Compiler，好像是元对象编译器)进行编译，元对象是QT的语言特性之一，QT中会包含了各种宏，比如Q_OBJECT等，使用MOC先将宏编译后再使用C\++进行编译。

为了省点事，QT也提供了qmake生成makefile。

在生成项目时，QT creator会自动生成.pro文件，qmake会从.pro文件生成makefile文件。

### Signal、Slots

QT引进了槽函数机制，

+ 使用connect(signal, Signal::func(), slots,Slots::func())可以将信号与槽函数连接，函数将自动触发；
+ 使用disconnected()可以去取消连接

**添加槽时可以使用Qt creator可视化添加**。



## 类与API

### QLabel 

```cpp
	QLabel *label = new QLabel("Hello World");//构造函数
	label -> setWindowTitle("my")
    label -> show();
```



### QMessageBox

使用MessageBox作为弹出窗口

<img src="https://raw.githubusercontent.com/zhao408639122/Picbed/master/blog/20200317205549.png" />

| type           | function    | describe   |
| -------------- | ----------- | ---------- |
| void           | about       | 弹出文本   |
| void           | aboutQt     | 弹出Qt说明 |
| StandardButton | critical    | 弹出❌文本  |
| StandardButton | information | 弹出❕文本  |
| StandardButton | question    | 弹出❔文本  |
| StandardButton | warning     | 弹出⚠文本  |

**question函数有缺省参数，MessageBox有两个选项**，最后两个参数定义为选项的返回值，用或连接两个封装在QMessageBox中的选项，返回值类型也为StandardButton。

```cpp
	QmessageBox::StandardButton reply = QMessageBox::question(this, "My title",
                                       "This is my custom message", QMessageBox::Yes 																   | QMessageBox::No);
	if (reply == QMessageBox::Yes) {}
```

### QPlainTextEdit

使用QPlainTextEdit创造可编辑的文本区域

#### plainText：Qstring

+ 该成员为文本，使用**ToPlainText()**得到本文
+ 使用**setPlainText(const Qstring &text)**修改文本。

### Qpixmap

使用Qpixmap来添加图片，需要include\<Qpixmap>库

```cpp
	Qpixmap pix("filepositon");
	ui -> lable_pix -> setPixmap(pix.scaled(100, 100, Qt::keepAspectRatio));
```



## 可视化设计

### layout

在layout中使用prefered，可以使得元素随窗口大小自动改变。

layout有Horizontal，vertical，Grid和Form四种。

### Space、Splitter、Buddy、Tabs

Space可以控制在layout中的空间大小

Splitter可以创造不可见的、可以拉动的分割线

BuddyMode可以将多个元素作为一个类（大致是这个意思，等用到会更新的）

TabMode，将元素编号，没太看懂有什么用

