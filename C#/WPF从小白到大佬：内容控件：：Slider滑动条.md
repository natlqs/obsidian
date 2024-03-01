Slider滑动条与ScrollBar滚动条有点相似，甚至某些情况下，两者还可以互换使用。Slider也继承于RangeBase基类，其功能是提供一个可以滑动取值的控件。

一、Slider类定义

```cs
public class Slider : RangeBase
{
    public static readonly DependencyProperty OrientationProperty;
    public static readonly DependencyProperty IsMoveToPointEnabledProperty;
    public static readonly DependencyProperty SelectionEndProperty;
    public static readonly DependencyProperty SelectionStartProperty;
    public static readonly DependencyProperty IsSelectionRangeEnabledProperty;
    public static readonly DependencyProperty TickFrequencyProperty;
    public static readonly DependencyProperty TickPlacementProperty;
    public static readonly DependencyProperty TicksProperty;
    public static readonly DependencyProperty AutoToolTipPrecisionProperty;
    public static readonly DependencyProperty AutoToolTipPlacementProperty;
    public static readonly DependencyProperty IntervalProperty;
    public static readonly DependencyProperty DelayProperty;
    public static readonly DependencyProperty IsDirectionReversedProperty;
    public static readonly DependencyProperty IsSnapToTickEnabledProperty;
 
    public Slider();
 
    public static RoutedCommand MinimizeValue { get; }
    public static RoutedCommand IncreaseSmall { get; }
    public static RoutedCommand DecreaseSmall { get; }
    public static RoutedCommand MaximizeValue { get; }
    public static RoutedCommand DecreaseLarge { get; }
    public static RoutedCommand IncreaseLarge { get; }
    public bool IsSnapToTickEnabled { get; set; }
    public int AutoToolTipPrecision { get; set; }
    public AutoToolTipPlacement AutoToolTipPlacement { get; set; }
    public int Interval { get; set; }
    public int Delay { get; set; }
    public bool IsDirectionReversed { get; set; }
    public Orientation Orientation { get; set; }
    public double TickFrequency { get; set; }
    public DoubleCollection Ticks { get; set; }
    public double SelectionStart { get; set; }
    public TickPlacement TickPlacement { get; set; }
    public bool IsSelectionRangeEnabled { get; set; }
    public bool IsMoveToPointEnabled { get; set; }
    public double SelectionEnd { get; set; }
 
    public override void OnApplyTemplate();
    protected override Size ArrangeOverride(Size finalSize);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected virtual void OnDecreaseLarge();
    protected virtual void OnDecreaseSmall();
    protected virtual void OnIncreaseLarge();
    protected virtual void OnIncreaseSmall();
    protected virtual void OnMaximizeValue();
    protected override void OnMaximumChanged(double oldMaximum, double newMaximum);
    protected virtual void OnMinimizeValue();
    protected override void OnMinimumChanged(double oldMinimum, double newMinimum);
    protected override void OnPreviewMouseLeftButtonDown(MouseButtonEventArgs e);
    protected virtual void OnThumbDragCompleted(DragCompletedEventArgs e);
    protected virtual void OnThumbDragDelta(DragDeltaEventArgs e);
    protected virtual void OnThumbDragStarted(DragStartedEventArgs e);
    protected override void OnValueChanged(double oldValue, double newValue);
 
}
```

二、属性成员

|   |   |
|---|---|
|属性名称|说明|
|IsSnapToTickEnabled|Slider会有一些刻度线，如果要求Thumb移动到最近的刻度线，则可将该值设置为true。|
|AutoToolTipPrecision|获取或设置Slider的值的小数点位数。|
|AutoToolTipPlacement|获取或设置按下Thumb时是否显示提示工具。|
|Interval|获取或设置用户按下RepeatButton时执行增加减少命令的时间间隔（毫秒）。|
|Delay|获取或设置用户按下RepeatButton时延时多少毫秒后执行命令|
|IsDirectionReversed|获取或设置增加值的方向。|
|Orientation|获取或设置Slider的方向。水平或垂直。|
|TickFrequency|获取或设置刻度线之间的间隔。默认为1.0|
|Ticks|获取或设置为 System.Windows.Controls.Slider 显示的刻度线的位置。|
|SelectionStart|获取或设置 System.Windows.Controls.Slider 的指定选择内容的最大值。|
|TickPlacement|获取或设置刻度线的位置|
|IsSelectionRangeEnabled|获取或设置显示选择范围|
|IsMoveToPointEnabled|如果Thumb 立即移动到鼠标单击的位置，则为true。|
|SelectionEnd|获取或设置 System.Windows.Controls.Slider 的指定选择内容的最大值。|

三、Slider示例

观察下面的例子，看看Slider如何通过拖动去改变元素的尺寸。

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d" FontSize="16"
        Title="WPF中文网之控件课程 - www.wpfsoft.com" Height="350" Width="500">
    <Grid x:Name="viewport" >
        <Grid.RowDefinitions>
            <RowDefinition Height="*"/>
            <RowDefinition Height="auto"/>
            <RowDefinition Height="auto"/>
        </Grid.RowDefinitions>
        <Canvas>
            <Border BorderBrush="LightGray" BorderThickness="1" CornerRadius="5" Padding="3" Margin="3">
                <Image Source="pack://application:,,,/Images/logo.png" 
                       Width="{Binding ElementName=slider,Path=Value}" 
                       Height="{Binding ElementName=slider,Path=Value}"/>
            </Border>
        </Canvas>
        <DockPanel Grid.Row="1">
            <TextBlock Text="滑动改变图片大小" Margin="3" FontSize="14"/>
            <Slider x:Name="slider" Minimum="50" Maximum="500" Value="50" Margin="3"/>
        </DockPanel>        
    </Grid>
</Window>
```

![[assets/WPF从小白到大佬：内容控件：：Slider滑动条/a5a4e059137fe3035977f8d7a4705533_MD5.jpg]]

上图是XAML在设计时的效果。

![[assets/WPF从小白到大佬：内容控件：：Slider滑动条/6ef6eca1af24e621cb1a0e6bbe7bf8cf_MD5.jpg]]

F5运行之后，我们可以拖动Slider的滑块，图片的尺寸因为绑定了Slider控件的Value属性，所以图片的大小会随着用户左右拖动而变化。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：031-《Slider滑动条》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff