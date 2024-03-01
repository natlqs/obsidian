VirtualizingStackPanel 类（虚拟化元素）和StackPanel 类在用法上几乎差不多。其作用是在水平或垂直的一行中排列并显示内容。它继承于一个叫VirtualizingPanel的抽象类，而这个VirtualizingPanel抽象类继承于Panel布局基类。

```cs
	public class VirtualizingStackPanel : VirtualizingPanel, IScrollInfo, IStackMeasure
{
    public static readonly DependencyProperty IsVirtualizingProperty;
    public static readonly DependencyProperty VirtualizationModeProperty;
    public static readonly DependencyProperty OrientationProperty;
    public static readonly RoutedEvent CleanUpVirtualizedItemEvent;
 
    public VirtualizingStackPanel();
 
    public double VerticalOffset { get; }
    public double HorizontalOffset { get; }
    public double ViewportHeight { get; }
    public double ViewportWidth { get; }
    public double ExtentHeight { get; }
    public double ExtentWidth { get; }
    public bool CanVerticallyScroll { get; set; }
    public bool CanHorizontallyScroll { get; set; }
    public Orientation Orientation { get; set; }
    public ScrollViewer ScrollOwner { get; set; }
    protected override bool CanHierarchicallyScrollAndVirtualizeCore { get; }
    protected internal override Orientation LogicalOrientation { get; }
    protected internal override bool HasLogicalOrientation { get; }
 
    public static void AddCleanUpVirtualizedItemHandler(DependencyObject element, CleanUpVirtualizedItemEventHandler handler);
    public static void RemoveCleanUpVirtualizedItemHandler(DependencyObject element, CleanUpVirtualizedItemEventHandler handler);
    public virtual void LineDown();
    public virtual void LineLeft();
    public virtual void LineRight();
    public virtual void LineUp();
    public Rect MakeVisible(Visual visual, Rect rectangle);
    public virtual void MouseWheelDown();
    public virtual void MouseWheelLeft();
    public virtual void MouseWheelRight();
    public virtual void MouseWheelUp();
    public virtual void PageDown();
    public virtual void PageLeft();
    public virtual void PageRight();
    public virtual void PageUp();
    public void SetHorizontalOffset(double offset);
    public void SetVerticalOffset(double offset);
    protected override Size ArrangeOverride(Size arrangeSize);
    protected override double GetItemOffsetCore(UIElement child);
    protected override Size MeasureOverride(Size constraint);
    protected virtual void OnCleanUpVirtualizedItem(CleanUpVirtualizedItemEventArgs e);
    protected override void OnClearChildren();
    protected override void OnItemsChanged(object sender, ItemsChangedEventArgs args);
    protected virtual void OnViewportOffsetChanged(Vector oldViewportOffset, Vector newViewportOffset);
    protected virtual void OnViewportSizeChanged(Size oldViewportSize, Size newViewportSize);
    protected override bool ShouldItemsChangeAffectLayoutCore(bool areItemChangesLocal, ItemsChangedEventArgs args);
    protected internal override void BringIndexIntoView(int index);
 
}
```

VirtualizingStackPanel 类有什么作用？

比如在ListBox集合控件中需要显示500条数据，那整个屏幕只能显示20条，剩余的480条数据在ListBox控件要不要一次性绘制出来？其实就算绘制出来，用户的屏幕也看不见，只能是拖动滚动条才能看见后面的数据。既然屏幕只能显示20条数据，何不只绘制20条数据的UI子元素，剩下的480条数据的子元素在拖动滚动条时才绘制，这将大大减少计算机的性能消耗，提高UI界面的呈现速度，提高软件的流畅性。

所以，**VirtualizingStackPanel 类的作用是开启虚拟化技术，延迟那些看不见的子元素的绘制与渲染。**

要开启这项技术，只需要设置Listbox集合控件的附加属性VirtualizingStackPanel.IsVirtualizing="True"即可。因为ListBox的ItemsPanel（元素布局模板）默认采用了VirtualizingStackPanel控件布局。

——重庆教主 2023年8月20