Panel其实是一个抽象类，不可以实例化，**WPF所有的布局控件都从Panel继承而来**，所以我们在学习布局控件之前，要先了解一下这个类。首先看一下它的定义：
```cs
public abstract class Panel : FrameworkElement, IAddChild
{
    public static readonly DependencyProperty BackgroundProperty;
    public static readonly DependencyProperty IsItemsHostProperty;
    public static readonly DependencyProperty ZIndexProperty;
 
    protected Panel();
 
    public bool HasLogicalOrientationPublic { get; }
    public Orientation LogicalOrientationPublic { get; }
    public bool IsItemsHost { get; set; }
    public UIElementCollection Children { get; }
    public Brush Background { get; set; }
    protected override int VisualChildrenCount { get; }
    protected internal UIElementCollection InternalChildren { get; }
    protected internal override IEnumerator LogicalChildren { get; }
    protected internal virtual Orientation LogicalOrientation { get; }
    protected internal virtual bool HasLogicalOrientation { get; }
 
    public static int GetZIndex(UIElement element);
    public static void SetZIndex(UIElement element, int value);
    public bool ShouldSerializeChildren();
    protected virtual UIElementCollection CreateUIElementCollection(FrameworkElement logicalParent);
    protected override Visual GetVisualChild(int index);
    protected virtual void OnIsItemsHostChanged(bool oldIsItemsHost, bool newIsItemsHost);
    protected override void OnRender(DrawingContext dc);
    protected internal override void OnVisualChildrenChanged(DependencyObject visualAdded, DependencyObject visualRemoved);
}
```
从它的代码定义来看，它继承于FrameworkElement基类和IAddChild接口。所以，所有 Panel 元素都支持 FrameworkElement 定义的基本大小调整和定位属性，包括 Height、Width、HorizontalAlignment、VerticalAlignment、Margin 和 LayoutTransform。

它有一个Background属性，意味着所有的布局控件都可以设置背景颜色。另外，它还有一个Children属性，这是一个集合属性，也就是说，所有的布局控件都可以添加多个子元素。这一点从它继承的IAddChild接口也能得到印证。
```cs
namespace System.Windows.Markup
{
    //提供了一种分析允许混合的子元素或文本的元素的方法。
    public interface IAddChild
    {
        //添加子对象。
        void AddChild(object value);
        //将节点的文本内容添加到对象。
        void AddText(string text);
    }
```

Panel提供了GetZIndex和SetZIndex方法成员，分别表示获取某个元素的ZIndex顺序和设置某个元素的ZIndex顺序。

什么是ZIndex？这是Panel提供的一个附加属性。假如一个单行单列的Grid布局控件中有两个Button,正常情况下，这两个Button都会以撑满Grid的方式呈现在Grid中，那么，到底哪一个Button在上面，哪一个Button在下面呢？就看这两个Button的Panel.ZIndex附加属性的值，值越大越在上面，而值较小的那个Button将被上面的Button遮盖，从而在视觉上，用户只能看到一个Button。

> 附加属性
> 
> 附加属性的一个用途是允许子元素存储实际上由父元素定义的属性的唯一值。 此功能的一项应用是让子元素通知父级它们希望如何在用户界面 (UI) 中呈现，这对应用程序布局非常有用。

```cs
<Grid >
    <Button  Content="WPF中文网1" Panel.ZIndex="1" Margin="20 40 60 80" Padding="50" />
    <Button  Content="WPF中文网2" Panel.ZIndex="0" Margin="20 40 60 80" Padding="50" />
</Grid>
```

![[08a70a3b33a87fa93c636688d307eb42_MD5.jpg]]
在上面的示例中，正常情况下，会显示“WPF中文网2"按钮，但是我们故意叫其Panel.ZIndex=0，从而小于上面那个按钮的属性值，所以显示了“WPF中文网1"按钮。

WPF 提供了一套全面的派生 Panel 实现，可实现许多复杂的布局。这里有一个非常非常需要注意的事项，那就是Panel的Background属性。有时候我们希望在布局控件上实现鼠标点击事件的获取，请记得一定要给Background属性设置一个颜色值，如果不希望有具体的颜色，那就设置成Transparent 。不然，您会踩坑的！**因为布局控件的Background属性没有值时，是不能引发鼠标相关事件的**。

**深入探究Panel类**

Panel作为布局控件的基类，拥有一个叫Children的属性，这个属性的类型是UIElementCollection。我们来看一下它的结构：

```cs
public class UIElementCollection : IList, ICollection, IEnumerable
{
    public UIElementCollection(UIElement visualParent, FrameworkElement logicalParent);
 
    public virtual UIElement this[int index] { get; set; }
 
    public virtual int Capacity { get; set; }
    public virtual object SyncRoot { get; }
    public virtual bool IsSynchronized { get; }
    public virtual int Count { get; }
 
    public virtual int Add(UIElement element);
    public virtual void Clear();
    public virtual bool Contains(UIElement element);
    public virtual void CopyTo(UIElement[] array, int index);
    public virtual void CopyTo(Array array, int index);
    public virtual IEnumerator GetEnumerator();
    public virtual int IndexOf(UIElement element);
    public virtual void Insert(int index, UIElement element);
    public virtual void Remove(UIElement element);
    public virtual void RemoveAt(int index);
    public virtual void RemoveRange(int index, int count);
    protected void ClearLogicalParent(UIElement element);
    protected void SetLogicalParent(UIElement element);
 
}
```

从它所定义的方法来看，我们会看到一些添加或移除某个元素的方法成员，例如Add，Insert，Remove，Contains等等，而这些方法的参数都有一个叫UIElement的形参，说明什么问题？**只要继承于UIElement的类（或控件），都可以添加到Panel或Panel子类的Children中，从而在前端呈现出来。**

WPF提供了六个用于UI布局的Panel子类，分别是：Grid、StackPanel、WrapPanel、DockPanel、 VirtualizingStackPanel和 Canvas。 这些面板元素易于使用、功能齐全并且可扩展，足以适用于大多数应用程序。

![[bd6874e1ed0ef5a18ae4e299f5cee0c0_MD5.jpg]]

一个Panel 的呈现就是测量和排列子控件，然后在屏幕上绘制它们。所以在布局的过程中会经过一系列的计算，那么子控件越多，执行的计算次数就越多，则性能就会变差。如果不需要进行复杂的布局，则尽量少用复杂布局控件（如 Grid和自定义复杂的Panel）；如果能简单布局实现就尽量使用构造相对简单的布局（如 Canvas、UniformGrid等），这种布局可带来更好的性能。 如果有可能，我们应尽量避免调用 UpdateLayout方法。

布局系统为Panel中的每个子控件完成两个处理过程：测量处理过程（Measure）和排列处理过程（Arrange）。每个子 Panel 均提供自己的 MeasureOverride 和 ArrangeOverride 方法，以实现自己特定的布局行为。

每个派生 Panel 元素都以不同方式处理大小调整约束。 了解 Panel 如何处理水平或垂直方向上的约束可以使布局更容易预测。

|   |   |   |
|---|---|---|
|控件名称|x维度|y维度|
|Grid|约束|约束，Auto 应用于行和列的情形除外|
|StackPanel（垂直）|约束|按内容约束|
|StackPanel（水平）|按内容约束|约束|
|DockPanel|约束|约束|
|WrapPanel|按内容约束|按内容约束|
|Canvas|按内容约束|按内容约束|

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：006-《Panel类》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月16日

