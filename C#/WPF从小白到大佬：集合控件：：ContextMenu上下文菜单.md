ContextMenu上下文菜单必须要依附于一个“宿主控件”。由于FrameworkElement基类有一个叫ContextMenu的属性，代表了鼠标右键时弹出一个菜单，所以大多数控件都可以设置“上下文菜单”。

ContextMenu继承于MenuBase，而MenuBase继承于ItemsControl。所以，ContextMenu本质上也是一个集合控件。而它的元素则是MenuItem。在用法上，与Menu控件差不多。

一、ContextMenu的定义

```cs
public class ContextMenu : MenuBase
{
    public static readonly DependencyProperty HorizontalOffsetProperty;
    public static readonly RoutedEvent OpenedEvent;
    public static readonly DependencyProperty StaysOpenProperty;
    public static readonly DependencyProperty CustomPopupPlacementCallbackProperty;
    public static readonly DependencyProperty HasDropShadowProperty;
    public static readonly RoutedEvent ClosedEvent;
    public static readonly DependencyProperty PlacementRectangleProperty;
    public static readonly DependencyProperty PlacementTargetProperty;
    public static readonly DependencyProperty IsOpenProperty;
    public static readonly DependencyProperty VerticalOffsetProperty;
    public static readonly DependencyProperty PlacementProperty;
 
    public ContextMenu();
 
    public double HorizontalOffset { get; set; }
    public bool StaysOpen { get; set; }
    public CustomPopupPlacementCallback CustomPopupPlacementCallback { get; set; }
    public bool HasDropShadow { get; set; }
    public PlacementMode Placement { get; set; }
    public Rect PlacementRectangle { get; set; }
    public UIElement PlacementTarget { get; set; }
    public bool IsOpen { get; set; }
    public double VerticalOffset { get; set; }
    protected internal override bool HandlesScrolling { get; }
 
    public event RoutedEventHandler Closed;
    public event RoutedEventHandler Opened;
 
    protected virtual void OnClosed(RoutedEventArgs e);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected override void OnIsKeyboardFocusWithinChanged(DependencyPropertyChangedEventArgs e);
    protected override void OnKeyDown(KeyEventArgs e);
    protected override void OnKeyUp(KeyEventArgs e);
    protected virtual void OnOpened(RoutedEventArgs e);
    protected override void PrepareContainerForItemOverride(DependencyObject element, object item);
    protected internal override void OnVisualParentChanged(DependencyObject oldParent);
 
}
```

二、属性成员

|   |   |
|---|---|
|属性名称|说明|
|HorizontalOffset|获取或设置目标原点和弹出项对齐之间的水平距离点。|
|StaysOpen|是否保持打开状态|
|CustomPopupPlacementCallback|获取或设置ContextMenu指示在屏幕位置的回调|
|HasDropShadow|是否有投影出现的上下文菜单。|
|Placement|获取或设置ContextMenu显示的相对位置|
|PlacementRectangle|获取或设置相对于其上下文菜单位于在打开时的区域。|
|PlacementTarget|获取或设置ContextMenu打开时的相对控件|
|IsOpen|是否打开|
|VerticalOffset|获取或设置目标原点和弹出项对齐之间的垂直距离点。|

三、ContextMenu示例

```cs
<Grid>
    <Border Background="LightBlue" Width="200" Height="100" CornerRadius="15">
        <Border.ContextMenu>
            <ContextMenu>
                <MenuItem Header="复制"/>
                <MenuItem Header="粘贴"/>
                <MenuItem Header="删除"/>
                <MenuItem Header="关于"/>
            </ContextMenu>
        </Border.ContextMenu>
    </Border>        
</Grid>
```

![[assets/WPF从小白到大佬：集合控件：：ContextMenu上下文菜单/7ea95f92d9eb76ef199438b531623638_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：046-《ContextMenu上下文菜单》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月8日