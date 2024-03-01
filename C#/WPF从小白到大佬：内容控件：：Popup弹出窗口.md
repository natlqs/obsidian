Popup类似于ToolTip，在指定的元素或窗体中弹出一个具有任意内容的窗口。Popup继承于FrameworkElement，算得上是独门独户的控件，因为大多数控件都是从Shape、Control或Panel三个类继承而来。

```cs
	public class Popup : FrameworkElement, IAddChild
{
    public static readonly DependencyProperty ChildProperty;
    public static readonly DependencyProperty IsOpenProperty;
    public static readonly DependencyProperty PlacementProperty;
    public static readonly DependencyProperty CustomPopupPlacementCallbackProperty;
    public static readonly DependencyProperty StaysOpenProperty;
    public static readonly DependencyProperty HorizontalOffsetProperty;
    public static readonly DependencyProperty VerticalOffsetProperty;
    public static readonly DependencyProperty PlacementTargetProperty;
    public static readonly DependencyProperty PlacementRectangleProperty;
    public static readonly DependencyProperty PopupAnimationProperty;
    public static readonly DependencyProperty AllowsTransparencyProperty;
    public static readonly DependencyProperty HasDropShadowProperty;
 
    public Popup();
 
    public bool HasDropShadow { get; }
    public bool AllowsTransparency { get; set; }
    public PopupAnimation PopupAnimation { get; set; }
    public Rect PlacementRectangle { get; set; }
    public UIElement PlacementTarget { get; set; }
    public double VerticalOffset { get; set; }
    public double HorizontalOffset { get; set; }
    public bool StaysOpen { get; set; }
    public UIElement Child { get; set; }
    public bool IsOpen { get; set; }
    public PlacementMode Placement { get; set; }
    public CustomPopupPlacementCallback CustomPopupPlacementCallback { get; set; }
    protected internal override IEnumerator LogicalChildren { get; }
 
    public event EventHandler Closed;
    public event EventHandler Opened;
 
    public static void CreateRootPopup(Popup popup, UIElement child);
    protected override Size MeasureOverride(Size availableSize);
    protected virtual void OnClosed(EventArgs e);
    protected virtual void OnOpened(EventArgs e);
    protected override void OnPreviewMouseLeftButtonDown(MouseButtonEventArgs e);
    protected override void OnPreviewMouseLeftButtonUp(MouseButtonEventArgs e);
    protected override void OnPreviewMouseRightButtonDown(MouseButtonEventArgs e);
    protected override void OnPreviewMouseRightButtonUp(MouseButtonEventArgs e);
    protected internal override DependencyObject GetUIParentCore();
 
}
```

属性成员

|   |   |
|---|---|
|属性名称|说明|
|HasDropShadow|只读属性，控件是否有投影效果。|
|AllowsTransparency|获取或设置控件是否包含透明内容。|
|PopupAnimation|获取或设置控件打开或关闭时的动画效果，None表示没有动画，Fade表示逐渐显示或淡出，Slide表示向上向下滑入，Scroll表示滚动效果。|
|PlacementRectangle|获取或设置控件打开时的矩形位置 。|
|PlacementTarget|获取或设置Popup控件在哪个控件身边打开（重点）。|
|VerticalOffset|获取或设置目标原点和 popup 对齐点之间的垂直距离。|
|HorizontalOffset|获取或设置目标原点和弹出项对齐之间的水平距离点。|
|StaysOpen|默认值为true，表示Popup打开后，如果失去焦点，Popup是否继续显示（重点）。|
|Child|获取或设置控件的内容，类似于ContentControl的Content属性，只能拥有一个元素（重点）。|
|IsOpen|获取或设置Popup控件是否可见。|
|Placement|枚举类，表示Popup 控件显示时的对齐方式。|

事件成员

Opened事件：Popup控件打开时引发的事件。

Closed事件：Popup控件关闭时引发的事件

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d" FontSize="16"
        Title="WPF中文网之控件课程 - www.wpfsoft.com" Height="350" Width="500">
    <StackPanel Orientation="Horizontal" HorizontalAlignment="Center" VerticalAlignment="Center">
        <CheckBox x:Name="checkbox" Content="WPF中文网" Height="30" Margin="5" ToolTip="WPF中文网之控件课程"/>
        <Popup Name="myPopup" 
               IsOpen="{Binding IsChecked, ElementName=checkbox}" 
               PlacementTarget="{Binding ElementName=checkbox}" 
               StaysOpen="True">
            <Border BorderThickness="1" Background="LightBlue">
                <StackPanel>
                    <TextBlock Text="官方网站" FontWeight="Bold" />
                    <TextBlock Text="点击这个按钮，进入WPF中文网站"/>
                    <Border BorderBrush="Silver" BorderThickness="0,1,0,0" Margin="0,4"/>
                    <TextBlock Text="http://www.wpfsoft.com" FontStyle="Italic"/>
                </StackPanel>
            </Border>
        </Popup>
    </StackPanel>
</Window>
```

![[assets/WPF从小白到大佬：内容控件：：Popup弹出窗口/6df500ad68a75e91e261b24c1058d7aa_MD5.jpg]]

代码分析

我们分别实例化了名叫checkbox和myPopup控件，myPopup的IsOpen属性绑定了checkbox的IsChecked，意思是， 当用户点击checkbox时，checkbox的IsChecked属性为true，myPopup的IsOpen属性也为true，于是就可以显示myPopup的内容了。

同时，myPopup的PlacementTarget属性也绑定到了checkbox控件，意味着myPopup将显示在checkbox控件身边，至于具体位置，可以设置Placement属性，有兴趣的小伙伴可以去尝试一下。

这里我们用到了Binding这个类，可以把它看成是一座桥梁，我们会在后面专门详细讲解Binding的用法。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：026-《Popup弹出窗口》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff