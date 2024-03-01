因为ToggleButton作为CheckBox（复选框）和RadioButton（单选框）的基类，我们在学习CheckBox和RadioButton之前要先了解一下这个基类。

```cs
public class ToggleButton : ButtonBase
{
    public static readonly RoutedEvent CheckedEvent;
    public static readonly RoutedEvent UncheckedEvent;
    public static readonly RoutedEvent IndeterminateEvent;
    public static readonly DependencyProperty IsCheckedProperty;
    public static readonly DependencyProperty IsThreeStateProperty;
 
    public ToggleButton();
 
    public bool IsThreeState { get; set; }
    public bool? IsChecked { get; set; }
 
    public event RoutedEventHandler Checked;
    public event RoutedEventHandler Indeterminate;
    public event RoutedEventHandler Unchecked;
 
    public override string ToString();
    protected virtual void OnChecked(RoutedEventArgs e);
    protected override void OnClick();
    protected override AutomationPeer OnCreateAutomationPeer();
    protected virtual void OnIndeterminate(RoutedEventArgs e);
    protected virtual void OnUnchecked(RoutedEventArgs e);
    protected internal virtual void OnToggle();
}
```

ToggleButton基类提供了两个属性和三个事件

IsThreeState属性为true表示控件支持3个状态，IsChecked属性为true表示当前控件已被选中。Checked事件表示选中时引发的事件，Unchecked事件表示从选中状态改为未选状态时引发的事件，Indeterminate事件表示不确定状态时引发的事件