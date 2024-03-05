ICommandSource其实是一个接口，我们观察ButtonBase基类会发现，它就继承了这个接口。

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
 
    //以下省略
 
}
```

所以，Button，RadioButton，CheckBox，ToggleButton等这些子控件都具有成为一个命令源的天赋。那么我们不禁要去探索一下这个ICommandSource到底是什么接口。

```cs
public interface ICommandSource
{
    ICommand Command { get; }
    object CommandParameter { get; }
    IInputElement CommandTarget { get; }
 
}
```

其实它的内容也不多，就只有3个属性，分别是Command，CommandParameter 和CommandTarget 。

Command就是在调用命令源时执行的命令。

CommandParameter 表示可在执行命令时传递给该命令的用户定义的数据值。

CommandTarget 表示在其上执行该命令的对象。

所以，假如我们定义了一个叫OpenCommand的命令，并且这个OpenCommand是某个ViewModel中的属性，那么，我们的按钮就可以实现下面这样的写法。

```xml
<Grid>
    <Button Content="打开"
            Click="Button_Click" 
            Command="{Binding OpenCommand}" 
            CommandParameter="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
</Grid>
```

在上面的例子中，我们即保留了Click事件的写法，同时也演示了Command命令的写法，以及CommandParameter命令参数的写法，这里采用了相对绑定的写法。

[关于绑定的知识请单击这里学习](http://www.wpfsoft.com/2023/09/14/2109.html)。

在这里，Button就是一个命令源（大多数情况下，命令源对象就是一个控件），OpenCommand是一个属性，但是这个属性的类型一定要继承ICommand接口，关于ICommand接口，我们会在下一节讲解。

——重庆教主 2023年10月11日