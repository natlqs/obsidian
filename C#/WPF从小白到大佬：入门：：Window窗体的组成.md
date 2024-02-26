Window窗体本质上也是一个控件，只不过它和一般控件有所区别。比如它具有Closing和Closed事件，而一般控件是不可以关闭的；另外，Window窗体可以容纳其它控件，最后，窗体由两部分构成，即工作区和非工作区。

![[assets/WPF从小白到大佬：入门：：Window窗体的组成/12033298843d4ab8d2d83a74e499564b_MD5.jpg]]

**非工作区**

非工作区主要包含以下几个要素，它们分别是：图标、标题、窗体菜单、最小化按钮、最大化按钮、关闭按钮、窗体边框、右下角鼠标拖动调整窗体尺寸。

**工作区**

如图所示，我们在窗体中最中心放置了一个TextBlock文字块控件，说明在这个区域内可以容纳和呈现一般控件。具体情况我们先看一下本例中的前端代码。

```html
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d"
        Title="HelloWorld" Height="350" Width="500">
    <Grid>
        <TextBlock Text="WPF中文网" 
                   Margin="0 50 0 0" 
                   FontSize="48" 
                   HorizontalAlignment="Center" 
                   VerticalAlignment="Center"/>
    </Grid>
</Window>
```

关于Window窗体的的工作区，本质上是指Window类的Content属性。Content属性表示窗体的内容，类型为object，即可以是任意的引用类型 。需要注意的是，Content属性并不在Window类中，而在它的父类ContentControl类中。

> 技术细节
> 
> 默认的<Window></Window>之中只能存在一个控件，就是因为Content是object类型，意思是只接受一个对象。那如何向窗体中增加多个控件呢？微软给出了示例，就是先放一个Grid 布局控件，因为Grid控件是一个集合控件，我们可以将多个控件放在Grid控件中，关于这些知识的扩展，我们将在WPF的《可视化树》章节中详细的探讨。

接下来，我们就要学习WPF的控件，并掌握各种控件的用法。但是这些控件都有各自继承的父类，所以在学习控件之前，我们要先学习这些控件父类，它们有的是抽象父类，有的是非抽象父类，不同的父类承担着不同的功能。

下一节，WPF控件的父类们

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：004-《Window窗体的组成》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月11日