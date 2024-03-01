Control是许多控件的基类。比如最常见的按钮（Button）、单选(RadioButton)、复选（CheckBox）、文本框（TextBox）、ListBox、DataGrid、日期控件等等。这些控件通常用于展示程序的数据或获取用户输入的数据，我们可以将这一类型的控件称为内容控件或数据控件，它们与前面的布局控件有一定的区别，布局控件更专注于界面，而内容控件更专注于数据（业务）。

**Control类虽然可以实例化，但是在界面上是不会有任何显示的**。只有那些继承了Control的子类（控件）才会在界面上显示，而且所呈现的样子各不相同，为什么会是这样呢？

因为Control类提供了一个控件模板（ControlTemplate），而几乎所有的子类都对这个ControlTemplate进行了各自的实现，所以在呈现子类时，我们才会看到Button拥有Button的样子，TextBox拥有TextBox的样子。

> **Control**基类说
> 
> 我给你们一张白纸(ControlTemplate)，你们可以在上面想画什么就画什么

我们在这一章节并不对模板(Template)进行详细的介绍，只是先阐述模板的概念，接下来，我们先将目光聚焦到Control的结构定义：

```cs
public class Control : FrameworkElement
{
public static readonly DependencyProperty BorderBrushProperty;
public static readonly RoutedEvent PreviewMouseDoubleClickEvent;
public static readonly DependencyProperty TemplateProperty;
public static readonly DependencyProperty PaddingProperty;
public static readonly DependencyProperty IsTabStopProperty;
public static readonly DependencyProperty TabIndexProperty;
public static readonly DependencyProperty VerticalContentAlignmentProperty;
public static readonly DependencyProperty HorizontalContentAlignmentProperty;
public static readonly RoutedEvent MouseDoubleClickEvent;
public static readonly DependencyProperty FontStyleProperty;
public static readonly DependencyProperty FontStretchProperty;
public static readonly DependencyProperty FontSizeProperty;
public static readonly DependencyProperty FontFamilyProperty;
public static readonly DependencyProperty ForegroundProperty;
public static readonly DependencyProperty BackgroundProperty;
public static readonly DependencyProperty BorderThicknessProperty;
public static readonly DependencyProperty FontWeightProperty;
 
public Control();
 
public FontStyle FontStyle { get; set; }
public FontStretch FontStretch { get; set; }
public double FontSize { get; set; }
public FontFamily FontFamily { get; set; }
public Brush Foreground { get; set; }
public Brush Background { get; set; }
public Thickness BorderThickness { get; set; }
public bool IsTabStop { get; set; }
public VerticalAlignment VerticalContentAlignment { get; set; }
public int TabIndex { get; set; }
public Thickness Padding { get; set; }
public ControlTemplate Template { get; set; }
public FontWeight FontWeight { get; set; }
public Brush BorderBrush { get; set; }
public HorizontalAlignment HorizontalContentAlignment { get; set; }
protected internal virtual bool HandlesScrolling { get; }
 
public event MouseButtonEventHandler MouseDoubleClick;
public event MouseButtonEventHandler PreviewMouseDoubleClick;
 
public override string ToString();
protected override Size ArrangeOverride(Size arrangeBounds);
protected override Size MeasureOverride(Size constraint);
protected virtual void OnMouseDoubleClick(MouseButtonEventArgs e);
protected virtual void OnPreviewMouseDoubleClick(MouseButtonEventArgs e);
protected virtual void OnTemplateChanged(ControlTemplate oldTemplate, ControlTemplate newTemplate);
 
}
```

我们先来看看Control基类为它的子类们提供了哪些属性

|   |   |
|---|---|
|属性|说明|
|FontStyle|获取或设置控件的字体结构，类似于Word中字体的常规、斜体或倾斜|
|FontStretch|获取或设置紧缩或在屏幕上展开一种字体的程度。|
|FontSize|获取或设置字体大小。|
|FontFamily|获取或设置控件的字体系列。如：微软雅黑 = "Microsoft YaHei UI"|
|Foreground|获取或设置控件的字体颜色，也就是所谓的前景色画笔，它是一个刷子（Brush）|
|Background|获取或设置一个用于描述控件的背景画笔。|
|BorderThickness|获取或设置控件的边框宽度。|
|IsTabStop|获取或设置一个值，该值指示控件是否包括在选项卡上的导航窗格中。|
|VerticalContentAlignment|获取或设置控件的内容的垂直对齐方式。|
|TabIndex|获取或设置一个值，确定当用户导航控件通过使用 TAB 键元素接收焦点的顺序。|
|Padding|获取或设置在控件中的填充量。|
|**Template**|**获取或设置控件模板。**|
|FontWeight|获取或设置指定的字体粗细。|
|BorderBrush|获取或设置一个用于描述一个控件的边框背景画笔。|
|HorizontalContentAlignment|获取或设置控件的内容的水平对齐方式。|

大部分的属性都比较好理解，这里着重介绍一下Template属性。如果把人比作是一个Control(控件)，那么”着装“就是Template（模板）。在大街上，我们会看到不同着装的人来来往往。

所以**Control的Template定义了控件的外观**（着装）。

> 数据模板
> 
> 与控件模板类似的，还有一个概念叫数据模板。形象地说，还是把人比作控件，那么人体的五脏六腑就是这个控件的数据，而五脏六腑（数据）的外观就是指数据模板。

我们来看一个实际的例子

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d"
        Title="HelloWorld - www.wpfsoft.com" Height="350" Width="500">
    <Control Margin="10">
        
    </Control>
 
</Window>
```

![[assets/WPF从小白到大佬：内容控件：：Control基类/e6bf1eeb850bdf339fcd0b5c040d0498_MD5.jpg]]

我们在Window窗体中实例化了一个Control类，但是，界面上空无一物。这是因为当前这个Control对象还没有”穿衣服“，也就是说，它的Template并没有内容，实际上，此刻Template属性等于null。

那我们尝试给他穿一件衣服吧。

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d"
        Title="HelloWorld - www.wpfsoft.com" Height="350" Width="500">
    <Control Margin="10">
        <Control.Template>
            <ControlTemplate TargetType="Control">
                <Border Background="LightBlue">
                    <TextBlock Text="WPF中文网" FontSize="20" HorizontalAlignment="Center" VerticalAlignment="Center"/>
                </Border>
            </ControlTemplate>
        </Control.Template>
    </Control>
</Window>
```

![[assets/WPF从小白到大佬：内容控件：：Control基类/aa5c69c61ae7eae1b36265fae2fdd63c_MD5.jpg]]

我们为Control的Template实例化了一个ControlTemplate对象，并在这个对象中增加了一个Border，在Border中又增加了一个TextBlock子元素，于是Control就有了这样一件新衣服。

在这里，我们要明白一个要点是，Control类的Template属性是ControlTemplate类型的。所以上面的代码才必须这样写才可以。而ControlTemplate又是什么东东？为什么在xaml中实例化时，后面要跟一句TargetType="Control"？关于这一点，我们将在后面专门拿一章节来阐述ControlTemplate以及它的父类。

**事件**

在这一小节里，您只要能明白Template的概念就行了。除了这个属性，Control类还提供了两个事件，它们分别是PreviewMouseDoubleClick和MouseDoubleClick。

|   |   |
|---|---|
|事件名称|说明|
|PreviewMouseDoubleClick|表示鼠标双击或多次单击时触发的事件|
|MouseDoubleClick|表示鼠标双击或多次单击时触发的事件|

以Preview开头的事件叫隧道事件或预览事件，而MouseDoubleClick没有以Preview开头，所以它叫冒泡事件。

WPF的前端代码其实是一棵树，当我们在某个目标控件上进行鼠标操作时，所引发的事件有两个方向，一是从Winow根节点一直路由到目标控件上，看起来就好像是从外面一直沿着这棵树路由引发至里面，这就像我们开车进入隧道一样，所以Preview开头的事件叫隧道事件。

冒泡事件事件的路由方向相反，是从目标控件位置开始，一直路由引发至最外层的Window窗体。

关于路由事件，我们会在后面拿一章节专门讲解，这里只提一下这个概念。

通常，我们并不会直接实例化Control基类，确实这样做对我们实际帮助不大，我们要使用的——是它膝下各个子控件，而在这众多的子控件中，Button是最常见最简单的控件了。不过，Button的基类是ButtonBase，ButtonBase的基类是ContentControl，ContentControl的基类是Control。如果我们要探讨Button控件，看样子必须要先介绍一下ContentControl基类和ButtonBase基类才行了。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：015-《Control基类》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月22日

