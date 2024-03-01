严格来说，Border并不是一个布局控件，因为它并不是Panel的子类，而是Decorator装饰器的子类，而Decorator继承于FrameworkElement。要了解Border的用法，我们要先看看它的父类Decorator。

```cs
public class Decorator : FrameworkElement, IAddChild
{
    public Decorator();
 
    public virtual UIElement Child { get; set; }
    protected override int VisualChildrenCount { get; }
    protected internal override IEnumerator LogicalChildren { get; }
 
    protected override Size ArrangeOverride(Size arrangeSize);
    protected override Visual GetVisualChild(int index);
    protected override Size MeasureOverride(Size constraint);
 
}
```

Decorator 装饰器只有一个Child 属性，说明Decorator只能容纳一个子元素（UIElement），也就是**Border只能容纳一个子元素**。那我们再看看Border的结构定义：

```cs
public class Border : Decorator
{
    public static readonly DependencyProperty BorderThicknessProperty;
    public static readonly DependencyProperty PaddingProperty;
    public static readonly DependencyProperty CornerRadiusProperty;
    public static readonly DependencyProperty BorderBrushProperty;
    public static readonly DependencyProperty BackgroundProperty;
 
    public Border();
 
    public Thickness BorderThickness { get; set; }
    public Thickness Padding { get; set; }
    public CornerRadius CornerRadius { get; set; }
    public Brush BorderBrush { get; set; }
    public Brush Background { get; set; }
 
    protected override Size ArrangeOverride(Size finalSize);
    protected override Size MeasureOverride(Size constraint);
    protected override void OnRender(DrawingContext dc);
 
}
```

我们直接以表格的形式给出Border的相关属性。

|   |   |
|---|---|
|属性|说明|
|BorderThickness|设置Border边框的厚度（像素宽度）|
|Padding|设置子元素相对于Border边框的距离|
|CornerRadius|设置Border的圆角|
|BorderBrush|设置Border边框的颜色画刷|
|Background|设置Border的背景颜色画刷|

正是因为Border有这么多实用的属性， 所以， 我们通常在布局界面时，Border（装饰器）控件是首选。接下来，我们以一个例子来说明Border有多么好用。
```cs
<WrapPanel Margin="10">
    <Border Height="35" Margin="10" Padding="5" BorderThickness="1" BorderBrush="Gray">
        <TextBlock  Text="矩形 - Border控件" Margin="5" />
   </Border>
    <Border Height="35" Margin="10" Padding="5" BorderThickness="1" BorderBrush="Gray" CornerRadius="20">
        <TextBlock  Text="椭圆 - Border控件" Margin="5" />
    </Border>
    <Border Width="150" Height="150" Margin="10" Padding="5" BorderThickness="1" 
            Background="Red" BorderBrush="Gray" CornerRadius="75">
        <TextBlock  Text="圆形Border控件" Margin="5" HorizontalAlignment="Center" 
                    FontSize="16" FontWeight="Bold" VerticalAlignment="Center" Foreground="White"/>
    </Border>
</WrapPanel>
```

我们分别写了3个Border，第一个Border被设计成矩形，第二个Border增加了圆角属性，第三个Border通过CornerRadius属性，将值设置为宽度或高度的一半，就形成了一个正圆。

将来，我们再配合WPF的模板、样式、触发器会让Border的用法更上一层楼。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：013-《Border控件（边框布局）》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月22日




