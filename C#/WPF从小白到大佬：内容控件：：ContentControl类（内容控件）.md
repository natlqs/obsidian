ContentControl是一个神奇的类！

为什么这么说呢，因为它有一个Content属性，关键是这个属性的类型是object。也就是说，本质上，它可以接收任意引用类型的实例。

但是，通常情况下，Content属性接收UI控件。因为，ContentControl控件最终会把Content属性里面的内容显示到界面上。

我们先看看它的结构定义：

```cs
public class ContentControl : Control, IAddChild
{
    public static readonly DependencyProperty ContentProperty;
    public static readonly DependencyProperty HasContentProperty;
    public static readonly DependencyProperty ContentTemplateProperty;
    public static readonly DependencyProperty ContentTemplateSelectorProperty;
    public static readonly DependencyProperty ContentStringFormatProperty;
 
    public ContentControl();
 
    public DataTemplate ContentTemplate { get; set; }
    public bool HasContent { get; }
    public object Content { get; set; }
    public string ContentStringFormat { get; set; }
    public DataTemplateSelector ContentTemplateSelector { get; set; }
    protected internal override IEnumerator LogicalChildren { get; }
 
    public virtual bool ShouldSerializeContent();
    protected virtual void AddChild(object value);
    protected virtual void AddText(string text);
    protected virtual void OnContentChanged(object oldContent, object newContent);
    protected virtual void OnContentStringFormatChanged(string oldContentStringFormat, string newContentStringFormat);
    protected virtual void OnContentTemplateChanged(DataTemplate oldContentTemplate, DataTemplate newContentTemplate);
    protected virtual void OnContentTemplateSelectorChanged(DataTemplateSelector oldContentTemplateSelector, DataTemplateSelector newContentTemplateSelector);
 
}
```

**那么，我如果非要把其它类型的对象（比如字符串）强行塞给Content属性呢？**

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d"
        Title="HelloWorld - www.wpfsoft.com" Height="350" Width="500">
    <ContentControl Foreground="Red" FontSize="36" HorizontalAlignment="Center" VerticalAlignment="Center">
        WPF中文网
    </ContentControl>
</Window>
```

[[assets/WPF从小白到大佬：内容控件：：ContentControl类（内容控件）/12ab06d69e1528faecae14afd7f8bb1e_MD5.jpeg|Open: Pasted image 20240227144002.png]]
![[assets/WPF从小白到大佬：内容控件：：ContentControl类（内容控件）/12ab06d69e1528faecae14afd7f8bb1e_MD5.jpeg]]

如上所示，我们在ContentControl 内部只写了一句“WPF中文网”，并设置了Foreground和FontSize等属性，居然不但没报错，还将字符串显示出来了，这真是太神奇了！

> 重庆教主友情提示
> 
> 别忘记了ContentControl 继承于Control 基类，所以我们的ContentControl 也可以设置Foreground和FontSize哦！

ContentTemplate属性（内容模板）

这个属性表示获取或设置用来显示的内容的数据模板，说白了，就是决定“WPF中文网”这几个字的穿着，如果没有设置数据模板，它将以默认的数据模板来显示这几个字。接下来，我们演示一下这个属性的用法，并简要说明其中的关系。

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d"
        Title="HelloWorld - www.wpfsoft.com" Height="350" Width="500">
    <ContentControl Foreground="Red" FontSize="36" HorizontalAlignment="Center" VerticalAlignment="Center">
        <ContentControl.ContentTemplate>
            <DataTemplate>
                <TextBlock Text="{Binding}" Foreground="Green" FontSize="16"/>
            </DataTemplate>
        </ContentControl.ContentTemplate>
        WPF中文网
    </ContentControl>
</Window>
```

![[assets/WPF从小白到大佬：内容控件：：ContentControl类（内容控件）/485ce9bb53be6d799d16a284e2d852fb_MD5.jpg]]

ContentControl类的ContentTemplate属性是DataTemplate类型，所以我们在xaml中实例化了一个DataTemplate（数据模板）对象，并在其中增加了一个TextBlock控件，将TextBlock控件的Text属性写成了Binding形式，并设置了字体颜色和大小。

最终呈现的效果上图所示，字体颜色为绿色，大小为16。虽然ContentControl也设置了字体颜色为红色，大小为36，但是已经失效了。好比ContentControl给Content提供了一件红色的外衣，但是，我们又特地提供了一件绿色的外衣，于是，ContentControl就被绿了。

关于数据模板中的TextBlock控件的Text属性写成了Binding(绑定)形式，这是指将ContentContent控件的Contnet属性绑定到TextBlock控件的Text属性中，写成伪代码就是：

```
TextBlock.Text = ContentContent.Content
```

这是您第一次在这个系列中看到Binding（绑定）的出现。我们将在后面专门拿一章节进行探讨。这里能理解这个ContentTemplate就行了。

**ContentControl控件能不能容纳多个子控件？**

不能！因为ContentControl控件只能显示Content属性里面的内容，而Content属性是object，只能接收一个对象。

HasContent属性：表示ContentControl是否有内容。

ContentStringFormat属性：获取或设置ContentControl要显示字符串的格式。

ContentTemplateSelector属性：模板选择器， 我们会在模板一章节介绍。

**常规用法**

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d"
        Title="HelloWorld - www.wpfsoft.com" Height="350" Width="500">
    <ContentControl Foreground="Red" FontSize="60" HorizontalAlignment="Center" VerticalAlignment="Center">
        <Button Content="WPF中文网"/>
    </ContentControl>
</Window>
```

![[assets/WPF从小白到大佬：内容控件：：ContentControl类（内容控件）/da27356342cdfef22ac314a83d3b4da4_MD5.jpg]]

这次我们在ContentControl放了一个Button，需要注意一点的是：Button的字号会随着ContentControl的设置而变化，但是字体颜色不会随着ContentControl的设置而变化。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：016-《ContentControl基类》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月22日
