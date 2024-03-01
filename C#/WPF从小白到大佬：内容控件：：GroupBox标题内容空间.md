GroupBox控件的功能是提供一个带标题的内容容器，它继承于HeaderedContentControl类，HeaderedContentControl继承于ContentControl类。通常它用来做一些局部的布局。由于GroupBox本身并没有什么成员，所以我们直接观察它的基类。

一、HeaderedContentControl基类

```cs
public class HeaderedContentControl : ContentControl
{
    public static readonly DependencyProperty HeaderProperty;
    public static readonly DependencyProperty HasHeaderProperty;
    public static readonly DependencyProperty HeaderTemplateProperty;
    public static readonly DependencyProperty HeaderTemplateSelectorProperty;
    public static readonly DependencyProperty HeaderStringFormatProperty;
 
    public HeaderedContentControl();
 
    public DataTemplateSelector HeaderTemplateSelector { get; set; }
    public DataTemplate HeaderTemplate { get; set; }
    public string HeaderStringFormat { get; set; }
    public bool HasHeader { get; }
    public object Header { get; set; }
    protected internal override IEnumerator LogicalChildren { get; }
 
    public override string ToString();
    protected virtual void OnHeaderChanged(object oldHeader, object newHeader);
    protected virtual void OnHeaderStringFormatChanged(string oldHeaderStringFormat, string newHeaderStringFormat);
    protected virtual void OnHeaderTemplateChanged(DataTemplate oldHeaderTemplate, DataTemplate newHeaderTemplate);
    protected virtual void OnHeaderTemplateSelectorChanged(DataTemplateSelector oldHeaderTemplateSelector, DataTemplateSelector newHeaderTemplateSelector);
 
}
```

在这个基类中，我们可以看到他继承于ContentControl基类，所以GroupBox要显示的内容都会放到Content属性中，而GroupBox的标题则放在Header属性中，注意，Header属性也是object。这足以说明GroupBox在私人定制方面的强大扩展性。

再加上HeaderTemplate属性，可以定制标题的外观。待我们在后面学了模板和样式，再回头来探讨这一类的属性的应用。

GroupBox的简单用法如下：

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d" FontSize="16"
        Title="WPF中文网之控件课程 - www.wpfsoft.com" Height="350" Width="500">
    <GroupBox Header="缩略图" Margin="5">
        <WrapPanel>
            <Border BorderBrush="LightGray" BorderThickness="1" CornerRadius="5" Padding="3" Margin="3">
                <Image Source="pack://application:,,,/Images/logo.png" Width="100" Height="100"/>
            </Border>
            <Border BorderBrush="LightGray" BorderThickness="1" CornerRadius="5" Padding="3" Margin="3">
                <Image Source="pack://application:,,,/Images/logo.png" Width="100" Height="100"/>
            </Border>
            <Border BorderBrush="LightGray" BorderThickness="1" CornerRadius="5" Padding="3" Margin="3">
                <Image Source="pack://application:,,,/Images/logo.png" Width="100" Height="100"/>
            </Border>
            <Border BorderBrush="LightGray" BorderThickness="1" CornerRadius="5" Padding="3" Margin="3">
                <Image Source="pack://application:,,,/Images/logo.png" Width="100" Height="100"/>
            </Border>
            <Border BorderBrush="LightGray" BorderThickness="1" CornerRadius="5" Padding="3" Margin="3">
                <Image Source="pack://application:,,,/Images/logo.png" Width="100" Height="100"/>
            </Border>
            <Border BorderBrush="LightGray" BorderThickness="1" CornerRadius="5" Padding="3" Margin="3">
                <Image Source="pack://application:,,,/Images/logo.png" Width="100" Height="100"/>
            </Border>
            <Border BorderBrush="LightGray" BorderThickness="1" CornerRadius="5" Padding="3" Margin="3">
                <Image Source="pack://application:,,,/Images/logo.png" Width="100" Height="100"/>
            </Border>
        </WrapPanel>
    </GroupBox>
</Window>
```

![[assets/WPF从小白到大佬：内容控件：：GroupBox标题内容空间/dd59929d20d5c7b243ad3d48e6936304_MD5.jpg]]

因为GroupBox的Content属性只能显示一个内容对象，如果要显示多个对象，那把给Content一个集合控件，比如上面的WrapPanel控件，这样就可以在WrapPanel控件中放多个子元素了。

在使用上，有一个集合控件与GroupBox类似，因为GroupBox只能显示一个区域，如果区域过大，在有限的窗体无法全部显示出来，该怎么办呢？ScrollViewer可以做到这一点。

下一节，我们来探讨一下ScrollViewer控件的用法。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：028-《GroupBox控件（标题容器）》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月29日