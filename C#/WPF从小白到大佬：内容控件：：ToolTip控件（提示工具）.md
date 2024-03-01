ToolTip控件继承于ContentControl，它不能有逻辑或视觉父级，意思是说，它不能单独存在于WPF的视觉树上（不能以控件的形式实例化），它必须依附于某个控件。因为它的功能被设计成提示信息，当鼠标移动到某个控件上方时，悬停一会儿，就会显示这个ToolTip的内容。

通常ToolTip会显示一句话，用来阐述某个控件的说明。这个控件存在于FrameworkElement基类中，也就是ToolTip属性，这个属性在FrameworkElement虽然被声明成object，而不是ToolTip类型，但是，我们仍然可以自定义ToolTip的内容。重点：**WPF几乎所有控件都可以拥有ToolTip小型提示弹窗**！

因为ToolTip继承于ContentControl控件，所以，ToolTip拥有的Content属性就可以显示任何类型，比如字符串、图像、其它控件组合布局。

```cs
public class ToolTip : ContentControl
{
    public static readonly DependencyProperty HorizontalOffsetProperty;
    public static readonly RoutedEvent OpenedEvent;
    public static readonly DependencyProperty StaysOpenProperty;
    public static readonly DependencyProperty CustomPopupPlacementCallbackProperty;
    public static readonly DependencyProperty PlacementProperty;
    public static readonly RoutedEvent ClosedEvent;
    public static readonly DependencyProperty PlacementTargetProperty;
    public static readonly DependencyProperty HasDropShadowProperty;
    public static readonly DependencyProperty IsOpenProperty;
    public static readonly DependencyProperty VerticalOffsetProperty;
    public static readonly DependencyProperty PlacementRectangleProperty;
 
    public ToolTip();
 
    public bool IsOpen { get; set; }
    public bool StaysOpen { get; set; }
    public CustomPopupPlacementCallback CustomPopupPlacementCallback { get; set; }
    public PlacementMode Placement { get; set; }
    public Rect PlacementRectangle { get; set; }
    public UIElement PlacementTarget { get; set; }
    public double HorizontalOffset { get; set; }
    public double VerticalOffset { get; set; }
    public bool HasDropShadow { get; set; }
 
    public event RoutedEventHandler Closed;
    public event RoutedEventHandler Opened;
 
    protected virtual void OnClosed(RoutedEventArgs e);
    protected override void OnContentChanged(object oldContent, object newContent);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected virtual void OnOpened(RoutedEventArgs e);
    protected internal override void OnVisualParentChanged(DependencyObject oldParent);
 
}
```

简单使用

```cs
<Button Content="确定" ToolTip="WPF中文网之控件课程"/>
```

自定义ToolTip内容

```cs
<Button x:Name="button2" Content="网站" Width="100" Height="30" Margin="5" Click="button2_Click">
    <Button.ToolTip>
        <StackPanel>
            <TextBlock Text="官方网站" FontWeight="Bold" />
            <TextBlock Text="点击这个按钮，进入WPF中文网站"/>
            <Border BorderBrush="Silver" BorderThickness="0,1,0,0" Margin="0,4"/>
            <TextBlock Text="http://www.wpfsoft.com" FontStyle="Italic"/>
        </StackPanel>
    </Button.ToolTip>
</Button>
```

```cs
private void button2_Click(object sender, RoutedEventArgs e)
{
    Process.Start("http://www.wpfsoft.com");
}
```

![](http://www.wpfsoft.com/wp-content/uploads/2023/08/2023082807362029.png)

虽然ToolTip可以自定义内容，但是，ToolTip的内容无法接收焦点。

与ToolTip有点类似的控件，还有一个叫Popup控件，也是一个弹出窗口，并可以在这个窗口内任意布局。我们在下一节来探讨它。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：025-《ToolTip控件（提示工具）》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff