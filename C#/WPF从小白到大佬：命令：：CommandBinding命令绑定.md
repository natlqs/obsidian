CommandBinding，顾名思义——命令绑定。这里并不是说将某个命令绑定到某个控件的事件上，而是将某个命令与一些逻辑代码进行绑定。至于命令如何绑定到控件上，或者在什么场景下绑定并调用命令，与CommandBinding无关。

我们首先来看看CommandBinding的定义

```cs
public class CommandBinding
{
    public CommandBinding();
    public CommandBinding(ICommand command);
    public CommandBinding(ICommand command, ExecutedRoutedEventHandler executed);
    public CommandBinding(ICommand command, ExecutedRoutedEventHandler executed,     CanExecuteRoutedEventHandler canExecute);
 
    public ICommand Command { get; set; }
 
    public event ExecutedRoutedEventHandler PreviewExecuted;
    public event ExecutedRoutedEventHandler Executed;
    public event CanExecuteRoutedEventHandler PreviewCanExecute;
    public event CanExecuteRoutedEventHandler CanExecute;
 
}
```

实例化一个CommandBinding对象，需要传入一个ICommand的命令对象， 以及ExecutedRoutedEventHandler委托和CanExecuteRoutedEventHandler委托。

ExecutedRoutedEventHandler委托里面的代码就是当前Command对象真正要执行的逻辑代码，这部分由程序员根据项目需求自己实现。

CanExecuteRoutedEventHandler委托是条件判断逻辑代码，表示当前的命令是否能够执行。

在了解了这些概念后，我们就可以用一个实际案例来说明WPF的预定义命令的用法。

第一步，实例化一个RoutedUICommand 命令

```xml
<Window.Resources>
<RoutedUICommand x:Key="PlayCommand" Text="Open"/>
</Window.Resources>
```

第二步，实例化一个CommandBinding对象

```xml
<Window.CommandBindings>
<CommandBinding Command="{StaticResource PlayCommand}"
                Executed="CommandBinding_Executed"
                CanExecute="CommandBinding_CanExecute"/>
</Window.CommandBindings>
```

这里需要定义两个回调函数。

```cs
private void CommandBinding_Executed(object sender,ExecutedRoutedEventArgs e){
	MessageBox.Show("我是ALT+S");
}
private void CommandBinding_Executed(object sender,CanExecuteRoutedEventArgs e){
	e.CanExecute = true;
}
```

第三步，调用PlayCommand命令

```xml
<StackPanel VerticalAlignment="Center">
    <Button Width="100" Height="30" 
            Content="播放" Margin="10"
            Command="{StaticResource PlayCommand}" />
</StackPanel>
```

由于我们在XAML中实例化的PlayCommand，所以引用时，必须以StaticResource 静态资源引用。

除了通过控件的Command属性去绑定PlayCommand命令，还有没有别的方式呢？有的！比如我们可以通过MouseBinding或者KeyBinding去绑定一个命令。

> 小小总结
> 
> Binding、MouseBinding、KeyBinding才是真正关注绑定命令的类型。

```xml
<Window.InputBindings>
    <!--鼠标+ctrl键触发command-->
    <MouseBinding Gesture="Control+WheelClick" Command="{StaticResource PlayCommand}"/>
    <!--快捷键触发command-->
    <KeyBinding Gesture="Alt+S" Command="{StaticResource PlayCommand}"/>
</Window.InputBindings>
```

完整的前端代码如下：

```xml
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld" 
        xmlns:forms="clr-namespace:System.Windows.Forms;assembly=System.Windows.Forms"
        mc:Ignorable="d" FontSize="14"
        Title="WPF中文网 - 命令 - www.wpfsoft.com" Height="350" Width="500">
    <Window.DataContext>
        <local:MainViewModel/>
    </Window.DataContext>
    <Window.Resources>
        <RoutedUICommand x:Key="PlayCommand" Text="Play"/>
    </Window.Resources>
    <Window.CommandBindings>
        <CommandBinding Command="{StaticResource PlayCommand}"
                Executed="CommandBinding_Executed"
                CanExecute="CommandBinding_CanExecute"/>
    </Window.CommandBindings>
    <Window.InputBindings>
        <!--鼠标+ctrl键触发command-->
        <MouseBinding Gesture="Control+WheelClick" Command="{StaticResource PlayCommand}"/>
        <!--快捷键触发command-->
        <KeyBinding Gesture="Alt+S" Command="{StaticResource PlayCommand}"/>
    </Window.InputBindings>
    <StackPanel VerticalAlignment="Center">
        <Button Width="100" Height="30" 
                Content="播放" Margin="10"
                Command="{StaticResource PlayCommand}" />
        <Button Width="100" Height="30" 
                Content="打开" 
                Command="{Binding OpenParamCommand}" 
                CommandParameter="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    </StackPanel>
</Window>
```

F5运行之后，我们可以单击按钮、或者按下键盘Alt+S、或者按下Ctrl键+鼠标滚轮，都可以触发PlayCommand命令。

![[assets/WPF从小白到大佬：命令：：CommandBinding命令绑定/d4c8fc50476f5298e082bb56bede39bd_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：072-《CommandBinding命令绑定》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年10月11日