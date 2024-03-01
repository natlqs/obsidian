按钮，几乎每个具有UI界面的软件都会有它的身影，而按钮的形式也是有多种多样的，我们在这里简单的罗列一下。

|   |   |
|---|---|
|按钮名称|说明|
|Button|普通按钮|
|CheckBox|复选框按钮|
|RadioButton|单选框按钮|
|ToggleButton|是CheckBox、RadioButton的基类，表示可以切换状态|
|RepeatButton|重复，表示从按下到弹出过程中重复引发Click事件|
|GridViewColumnHeader|表示GridViewColumn 的列标题，其实它也是一个按钮|
|DataGridColumnHeader|表示DataGrid 列标题，也是一个按钮|
|DataGridRowHeader|表示DataGrid 行标题，也是一个按钮|

上面便是WPF中的按钮体系，这些按钮都有一个共同的基类ButtonBase，所以，我们了解清楚了ButtonBase，对于学习上面这些按钮，有莫大的帮助。

**一、ButtonBase概述**

ButtonBase是一个抽象类，所以，它不能被实例化。我们只能在它的子类中去使用它提供的一些属性、事件或方法成员。它只有一个事件，就是Click单击事件，毕竟鼠标双击事件在它的Control基类就有了嘛。另外，它还有一个非常厉害的Command属性，这个属性其实是一个接口，起什么作用呢？就是在单击按钮时，去执行这个Command属性所指定的一个具体命令。

这个Command命令是WPF命令系统里面的角色，也是WPF优于Winform的一个具体表现，Command命令也是MVVM模式中最重要的一环。我们会在后面专门探讨WPF的命令系统。

接下来，我们先看看ButtonBase的结构定义：

```cs
public abstract class ButtonBase : ContentControl, ICommandSource
{
    public static readonly RoutedEvent ClickEvent;
    public static readonly DependencyProperty CommandProperty;
    public static readonly DependencyProperty CommandParameterProperty;
    public static readonly DependencyProperty CommandTargetProperty;
    public static readonly DependencyProperty IsPressedProperty;
    public static readonly DependencyProperty ClickModeProperty;
 
    protected ButtonBase();
 
    public IInputElement CommandTarget { get; set; }
    public object CommandParameter { get; set; }
    public ICommand Command { get; set; }
    public bool IsPressed { get; protected set; }
    public ClickMode ClickMode { get; set; }
    protected override bool IsEnabledCore { get; }
 
    public event RoutedEventHandler Click;
 
    protected override void OnAccessKey(AccessKeyEventArgs e);
    protected virtual void OnClick();
    protected virtual void OnIsPressedChanged(DependencyPropertyChangedEventArgs e);
    protected override void OnKeyDown(KeyEventArgs e);
    protected override void OnKeyUp(KeyEventArgs e);
    protected override void OnLostKeyboardFocus(KeyboardFocusChangedEventArgs e);
    protected override void OnLostMouseCapture(MouseEventArgs e);
    protected override void OnMouseEnter(MouseEventArgs e);
    protected override void OnMouseLeave(MouseEventArgs e);
    protected override void OnMouseLeftButtonDown(MouseButtonEventArgs e);
    protected override void OnMouseLeftButtonUp(MouseButtonEventArgs e);
    protected override void OnMouseMove(MouseEventArgs e);
    protected internal override void OnRenderSizeChanged(SizeChangedInfo sizeInfo);
 
}
```

**二、ButtonBase的属性**

|   |   |
|---|---|
|属性名称|说明|
|CommandTarget|获取或设置要对其引发指定的命令的元素。|
|CommandParameter|获取或设置一个命令参数，这个参数是传递给Command 属性所指向的命令。|
|Command|获取或设置要在按此按钮时调用的命令。|
|IsPressed|获取当前按钮是否处于激活状态。|
|ClickMode|获取或设置按钮的单击模式|
|IsEnabledCore|获取的值 System.Windows.ContentElement.IsEnabled 属性。|
|||

**三、ButtonBase方法**

ButtonBase还提供了两个虚方法，分别是OnClick()和OnIsPressedChanged（）。说明这两个方法也是可以重写的，OnClick表示在按钮单击时执行的方法。

——重庆教主 2023年8月22日


