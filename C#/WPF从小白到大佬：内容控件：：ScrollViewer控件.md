如果某个控件的尺寸太大，当前界面无法全部显示，则可以将这个控件包含在ScrollViewer中，因为ScrollViewer控件封装了一个水平滚动条ScrollBar和一个垂直滚动条ScrollBar，所以，ScrollViewer就是一个包含其它可视元素的可滚动区域控件。

ScrollViewer继承于ContentControl，所以它也是一个内容控件，只能在Content属性中设置一个子元素，如果要在ScrollViewer中显示多个子元素，请设置一个集合控件。

ScrollViewer 控件既响应鼠标命令，也响应键盘命令，并定义许多可用于按预设的增量滚动内容的方法。 可以使用 ScrollChanged 事件来检测 ScrollViewer 状态的变化。

**一、ScrollViewer类的定义**

```cs
public class ScrollViewer : ContentControl
{
    public static readonly DependencyProperty CanContentScrollProperty;
    public static readonly DependencyProperty PanningRatioProperty;
    public static readonly DependencyProperty PanningDecelerationProperty;
    public static readonly DependencyProperty PanningModeProperty;
    public static readonly RoutedEvent ScrollChangedEvent;
    public static readonly DependencyProperty IsDeferredScrollingEnabledProperty;
    public static readonly DependencyProperty ViewportWidthProperty;
    public static readonly DependencyProperty ScrollableHeightProperty;
    public static readonly DependencyProperty ScrollableWidthProperty;
    public static readonly DependencyProperty ExtentHeightProperty;
    public static readonly DependencyProperty ViewportHeightProperty;
    public static readonly DependencyProperty ContentHorizontalOffsetProperty;
    public static readonly DependencyProperty ContentVerticalOffsetProperty;
    public static readonly DependencyProperty HorizontalOffsetProperty;
    public static readonly DependencyProperty ExtentWidthProperty;
    public static readonly DependencyProperty VerticalOffsetProperty;
    public static readonly DependencyProperty ComputedVerticalScrollBarVisibilityProperty;
    public static readonly DependencyProperty ComputedHorizontalScrollBarVisibilityProperty;
    public static readonly DependencyProperty VerticalScrollBarVisibilityProperty;
    public static readonly DependencyProperty HorizontalScrollBarVisibilityProperty;
 
    public ScrollViewer();
 
    public bool CanContentScroll { get; set; }
    public ScrollBarVisibility HorizontalScrollBarVisibility { get; set; }
    public ScrollBarVisibility VerticalScrollBarVisibility { get; set; }
    public Visibility ComputedHorizontalScrollBarVisibility { get; }
    public Visibility ComputedVerticalScrollBarVisibility { get; }
    public double HorizontalOffset { get; }
    public double VerticalOffset { get; }
    public double ExtentWidth { get; }
    public double ExtentHeight { get; }
    public double PanningDeceleration { get; set; }
    public double ScrollableHeight { get; }
    public double ViewportWidth { get; }
    public double ViewportHeight { get; }
    public double ContentVerticalOffset { get; }
    public double ContentHorizontalOffset { get; }
    public bool IsDeferredScrollingEnabled { get; set; }
    public PanningMode PanningMode { get; set; }
    public double ScrollableWidth { get; }
    public double PanningRatio { get; set; }
    protected internal override bool HandlesScrolling { get; }
    protected internal IScrollInfo ScrollInfo { get; set; }
 
    public event ScrollChangedEventHandler ScrollChanged;
 
    public static bool GetCanContentScroll(DependencyObject element);
    public static ScrollBarVisibility GetHorizontalScrollBarVisibility(DependencyObject element);
    public static bool GetIsDeferredScrollingEnabled(DependencyObject element);
    public static double GetPanningDeceleration(DependencyObject element);
    public static PanningMode GetPanningMode(DependencyObject element);
    public static double GetPanningRatio(DependencyObject element);
    public static ScrollBarVisibility GetVerticalScrollBarVisibility(DependencyObject element);
    public static void SetCanContentScroll(DependencyObject element, bool canContentScroll);
    public static void SetHorizontalScrollBarVisibility(DependencyObject element, ScrollBarVisibility horizontalScrollBarVisibility);
    public static void SetIsDeferredScrollingEnabled(DependencyObject element, bool value);
    public static void SetPanningDeceleration(DependencyObject element, double value);
    public static void SetPanningMode(DependencyObject element, PanningMode panningMode);
    public static void SetPanningRatio(DependencyObject element, double value);
    public static void SetVerticalScrollBarVisibility(DependencyObject element, ScrollBarVisibility verticalScrollBarVisibility);
    public void InvalidateScrollInfo();
    public void LineDown();
    public void LineLeft();
    public void LineRight();
    public void LineUp();
    public override void OnApplyTemplate();
    public void PageDown();
    public void PageLeft();
    public void PageRight();
    public void PageUp();
    public void ScrollToBottom();
    public void ScrollToEnd();
    public void ScrollToHome();
    public void ScrollToHorizontalOffset(double offset);
    public void ScrollToLeftEnd();
    public void ScrollToRightEnd();
    public void ScrollToTop();
    public void ScrollToVerticalOffset(double offset);
    protected override Size ArrangeOverride(Size arrangeSize);
    protected override HitTestResult HitTestCore(PointHitTestParameters hitTestParameters);
    protected override Size MeasureOverride(Size constraint);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected override void OnKeyDown(KeyEventArgs e);
    protected override void OnManipulationCompleted(ManipulationCompletedEventArgs e);
    protected override void OnManipulationDelta(ManipulationDeltaEventArgs e);
    protected override void OnManipulationInertiaStarting(ManipulationInertiaStartingEventArgs e);
    protected override void OnManipulationStarting(ManipulationStartingEventArgs e);
    protected override void OnMouseLeftButtonDown(MouseButtonEventArgs e);
    protected override void OnMouseWheel(MouseWheelEventArgs e);
    protected virtual void OnScrollChanged(ScrollChangedEventArgs e);
    protected override void OnStylusSystemGesture(StylusSystemGestureEventArgs e);
 
}
```

二、属性成员

HorizontalScrollBarVisibility：是否隐藏水平滚动条，为true表示隐藏，此时水平方向不可滚动。

VerticalScrollBarVisibility：是否隐藏垂直滚动条，为true表示隐藏，此时垂直方向不可滚动。

三、事件成员

ScrollChanged：当控件的滚动位置发生变化时将触发此事件。

四、示例

前端代码

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d" FontSize="16"
        Title="WPF中文网之控件课程 - www.wpfsoft.com" Height="350" Width="500">
    <ScrollViewer>
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
    </ScrollViewer>
</Window>
```

![[assets/WPF从小白到大佬：内容控件：：ScrollViewer控件/802b210c35b8a64138e6d69507d074ba_MD5.jpg]]

如上所示，我们在WrapPanel中增加了许多子元素，然后在外面套了一层ScrollViewer，由于WrapPanel是自动换行显示所有子元素（图片），所以，ScrollViewer会做出相应适配，只显示垂直滚动条。

既然ScrollViewer类封装了两个滚动条（ScrollBar），那我们就必须要了解一下ScrollBar的特性与用法，以加强我们对WPF控件的运用能力。下一节，我们将介绍ScrollBar。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：029-《ScrollViewer控件》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff


