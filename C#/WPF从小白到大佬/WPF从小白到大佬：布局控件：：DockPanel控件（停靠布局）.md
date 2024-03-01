官方解释，定义一个区域，从中可以按相对位置水平或垂直排列各个子元素。我们还是先来看一下它的结构定义：

```cs
public class DockPanel : Panel
{
    public static readonly DependencyProperty LastChildFillProperty;
    public static readonly DependencyProperty DockProperty;
 
    public DockPanel();
 
    public bool LastChildFill { get; set; }
 
    public static Dock GetDock(UIElement element);
    public static void SetDock(UIElement element, Dock dock);
    protected override Size ArrangeOverride(Size arrangeSize);
    protected override Size MeasureOverride(Size constraint);
 
}
```

DockPanel提供了一个LastChildFill 属性，用来指示最后一个子元素是否填满剩余的空间。其次，它还提供了一个枚举依赖属性，叫Dock。这个属性是附加到子元素身上的，用来指示子元素在DockPanel显示停靠方位，其值分为Left，Right，Top，Bottom。

DockPanel因为继承了FrameworkElement基类，所以您还可以使用FrameworkElement基类的HorizontalAlignment（水平对齐）和VerticalAlignment（垂直对齐）两个属性，用来设置子元素的排列方式。接下来我们将演示它最基本的用法。

一、经典布局

```cs
<DockPanel>
    <Button DockPanel.Dock="Left" Content="WPF中文网1" Margin="5" />
    <Button DockPanel.Dock="Top" Content="WPF中文网2" Margin="5" />
    <Button DockPanel.Dock="Right" Content="WPF中文网3" Margin="5" />
    <Button DockPanel.Dock="Bottom" Content="WPF中文网4" Margin="5" />
    <Button  Content="WPF中文网5" Margin="5" />
</DockPanel>
```

![[assets/WPF从小白到大佬：布局控件：：DockPanel控件（停靠布局）/8fefa32e6e132c1d7149c76c0ee58537_MD5.jpg]]

这是DockPanel最经典的布局方式，上下左右都停靠一个控件，中间剩余的空间，全部由最后一个控件填满。

二、水平布局

```cs
<DockPanel LastChildFill="False" HorizontalAlignment="Center">
    <Button Content="WPF中文网1" Margin="5" />
    <Button Content="WPF中文网2" Margin="5" />
    <Button Content="WPF中文网3" Margin="5" />
    <Button Content="WPF中文网4" Margin="5" />
    <Button Content="WPF中文网5" Margin="5" />
</DockPanel>
```

![[assets/WPF从小白到大佬：布局控件：：DockPanel控件（停靠布局）/b747b97ed0a1a67fe30cbe7e40520136_MD5.jpg]]

我们只需要设置LastChildFill为False，并设置HorizontalAlignment属性，并再指定子控件的停靠方向，就可以达到上图的效果。

```cs
<DockPanel LastChildFill="False" VerticalAlignment="Top">
    <Button Content="WPF中文网1" Margin="5"/>
    <Button Content="WPF中文网2" Margin="5"/>
    <Button Content="WPF中文网3" Margin="5"/>
    <Button Content="WPF中文网4" Margin="5"/>
    <Button Content="WPF中文网5" Margin="5"/>
</DockPanel>
```

![[assets/WPF从小白到大佬：布局控件：：DockPanel控件（停靠布局）/2ff99da1598da37edbe5cb46f39c2a7a_MD5.jpg]]

你可以通过摸索HorizontalAlignment（水平对齐）和VerticalAlignment（垂直对齐）两个属性，还可以达到更多意想不到的效果哦。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：010-《DockPanel控件（停靠布局）》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月18日




