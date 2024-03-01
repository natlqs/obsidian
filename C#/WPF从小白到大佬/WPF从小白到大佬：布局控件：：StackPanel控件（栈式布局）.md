StackPanel用于水平或垂直堆叠子元素。也就是说，StackPanel同样也有一个Children属性，而Children集合中的元素呈现在界面上时，只能是按水平或垂直方式布局。我们先来看看它的结构定义：
```cs
public class StackPanel : Panel, IScrollInfo, IStackMeasure
{
    public static readonly DependencyProperty OrientationProperty;
 
    public StackPanel();
 
    public double HorizontalOffset { get; }
    public double ViewportHeight { get; }
    public double ViewportWidth { get; }
    public double ExtentHeight { get; }
    public double ExtentWidth { get; }
    public bool CanVerticallyScroll { get; set; }
    public bool CanHorizontallyScroll { get; set; }
    public Orientation Orientation { get; set; }
    public double VerticalOffset { get; }
    public ScrollViewer ScrollOwner { get; set; }
    protected internal override Orientation LogicalOrientation { get; }
    protected internal override bool HasLogicalOrientation { get; }
 
    public void LineDown();
    public void LineLeft();
    public void LineRight();
    public void LineUp();
    public Rect MakeVisible(Visual visual, Rect rectangle);
    public void MouseWheelDown();
    public void MouseWheelLeft();
    public void MouseWheelRight();
    public void MouseWheelUp();
    public void PageDown();
    public void PageLeft();
    public void PageRight();
    public void PageUp();
    public void SetHorizontalOffset(double offset);
    public void SetVerticalOffset(double offset);
    protected override Size ArrangeOverride(Size arrangeSize);
    protected override Size MeasureOverride(Size constraint);
 
}
```

StackPanel提供了一些属性和方法，最常用的是Orientation枚举属性，用于设置子控件在StackPanel内部的排列方式，分别是水平排列（Horizontal）和垂直排列（Vertical）。默认值是垂直排列（Vertical）。


**一、水平排列**

```cs
<StackPanel Orientation="Vertical">
    <Button Content="WPF中文网1" Margin="20" />
    <Button Content="WPF中文网2" Margin="20" />
    <Button Content="WPF中文网3" Margin="20" />
    <Button Content="WPF中文网4" Margin="20" />
</StackPanel>
```
![[assets/WPF从小白到大佬：布局控件：：StackPanel控件（栈式布局）/45abb28173d4e65408bbeda3899631d6_MD5.jpg]]

请注意，当StackPanel子元素处于垂直排列时，此时子元素的宽度默认与StakcPanel的宽度保持一致，但是子元素的高度是与其自身的高度自适应显示。

**二、垂直排列**

```cs
<StackPanel Orientation="Horizontal">
    <Button Content="WPF中文网1" Margin="20" />
    <Button Content="WPF中文网2" Margin="20" />
    <Button Content="WPF中文网3" Margin="20" />
    <Button Content="WPF中文网4" Margin="20" />
</StackPanel>
```

![[assets/WPF从小白到大佬：布局控件：：StackPanel控件（栈式布局）/7e8ba422ac4893ef38675ca2ba8760d7_MD5.jpg]]

请注意，当StackPanel子元素处于水平排列时，此时子元素的高度默认与StakcPanel的高度保持一致，但是子元素的宽度是与其自身的宽度自适应显示。

**深入探究StackPanel控件**

可以利用子控件的HorizontalAlignment属性或VerticalAlignment来设置子控件在StackPanel内部的显示位置，比如在垂直排列布局模式下，可以设置HorizontalAlignment属性值，Left表示显示在左则，Right显示在右则，Center则居中显示，Stretch表示拉伸填充显示。

需要注意的是，由于WPF的控件布局都是采用自适应计算每个控件的位置，所以在设置了HorizontalAlignment或VerticalAlignment后，子控件的宽度和高度都会重新计算，主要是根据自身内容的尺寸计算。

```cs
<ScrollViewer>
    <StackPanel Orientation="Vertical">
        <Button Content="WPF中文网1" Margin="20" HorizontalAlignment="Left"/>
        <Button Content="WPF中文网2" Margin="20" HorizontalAlignment="Right"/>
        <Button Content="WPF中文网3" Margin="20" HorizontalAlignment="Center"/>
        <Button Content="WPF中文网4" Margin="20" HorizontalAlignment="Stretch"/>
        <Button Content="WPF中文网5" Margin="20" />
        <Button Content="WPF中文网6" Margin="20" />
        <Button Content="WPF中文网7" Margin="20" />
        <Button Content="WPF中文网8" Margin="20" />
        <Button Content="WPF中文网9" Margin="20" />
    </StackPanel>
</ScrollViewer>
```

![[assets/WPF从小白到大佬：布局控件：：StackPanel控件（栈式布局）/545dd933d74743fcfacde03f7725a393_MD5.jpg]]

于是，我们可以看到上图中前三行的按钮都是根据自身内容的宽高自适应绘制的。另外，如果StackPanel内部的子控件太多，则需要配合滚动条容器ScrollViewer控件。

和StackPanel类似的控件，还有两个，分别是WrapPanel和DockPanel。下一节，我们将探讨WrapPanel控件。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：008-《StackPanel控件（栈式布局）》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月17日












