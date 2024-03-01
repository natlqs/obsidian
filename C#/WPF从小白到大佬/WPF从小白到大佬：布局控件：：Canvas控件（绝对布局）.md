Canvas控件允许我们像Winform一样拖拽子控件进行布局，而子控件的位置相对于Canvas来说是绝对的，所以我将它称为绝对布局。我们来看看它的结构定义：

```cs
public class Canvas : Panel
{
    public static readonly DependencyProperty LeftProperty;
    public static readonly DependencyProperty TopProperty;
    public static readonly DependencyProperty RightProperty;
    public static readonly DependencyProperty BottomProperty;
 
    public Canvas();
 
    public static double GetBottom(UIElement element);
    public static double GetLeft(UIElement element);
    public static double GetRight(UIElement element);
    public static double GetTop(UIElement element);
    public static void SetBottom(UIElement element, double length);
    public static void SetLeft(UIElement element, double length);
    public static void SetRight(UIElement element, double length);
    public static void SetTop(UIElement element, double length);
    protected override Size ArrangeOverride(Size arrangeSize);
    protected override Geometry GetLayoutClip(Size layoutSlotSize);
    protected override Size MeasureOverride(Size constraint);
 
}
```

观察它的结构，我们可以看到它提供了4个依赖属性，分别是LeftProperty，RightProperty，TopProperty和BottomProperty。其实是将这4个属性附加到子元素身上，以此来设置子元素距离Canvas上下左右的像素位置。

```cs
<Canvas>
    <Button  Content="WPF中文网1" Margin="5" />
    <Button  Content="WPF中文网2" Margin="5" />
    <Button  Content="WPF中文网3" Margin="5" />
    <Button  Content="WPF中文网4" Margin="5" />
    <Button  Content="WPF中文网5" Margin="5" />
</Canvas>
```

![[assets/WPF从小白到大佬：布局控件：：Canvas控件（绝对布局）/bb10af3a4e92ff5038b6c78e3a854fe0_MD5.jpg]]

上面的示例并没有指定button控件在Canvas控件中的上下左右停靠位置，所以这5个button默认会显示在Canvas的左上角，且只能显示最后一个，前面4个会被遮盖。我们来看看下面的例子。

```cs
<Canvas>
    <Button  Content="WPF中文网1" Margin="5" Canvas.Left="50"/>
    <Button  Content="WPF中文网2" Margin="5" Canvas.Top="50"/>
    <Button  Content="WPF中文网3" Margin="5" Canvas.Right="50"/>
    <Button  Content="WPF中文网4" Margin="5" Canvas.Bottom="50"/>
    <Button  Content="WPF中文网5" Canvas.Left="200" Canvas.Top="150" />
</Canvas>
```

![[assets/WPF从小白到大佬：布局控件：：Canvas控件（绝对布局）/8a8cb3e39cffe1e7e24ce503e2f141cf_MD5.jpg]]

上面的源代码中，我们从上到下，分别来分析一下5个button。

第一个button，设置了Canvas.Left="50"，它将保持距离Canvas左边50像素。

第二个button，设置了Canvas.Top="50"，它将保持距离Canvas顶部50像素。

第三个button，设置了Canvas.Right="50"，它将保持距离Canvas右侧50像素。

第四个button，设置了Canvas.Bottom="50"，它将保持距离Canvas底部50像素。

第五个button，设置了Canvas.Left="200" Canvas.Top="150"，也就是同时距离Canvas左边200像素，顶部150像素。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：012-《Canvas控件（绝对布局）》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月22日