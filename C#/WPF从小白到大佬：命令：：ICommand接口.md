ICommand是WPF命令的代码协定，也就是说，WPF中所有的命令都要继承这个接口，不管是预定义命令还是自定义命令。

**一、ICommand的定义**

```cs
public interface ICommand
{
    //
    // 摘要:
    //     当出现影响是否应执行该命令的更改时发生。
    event EventHandler CanExecuteChanged;
 
    //
    // 摘要:
    //     定义确定此命令是否可在其当前状态下执行的方法。
    //
    // 参数:
    //   parameter:
    //     此命令使用的数据。 如果此命令不需要传递数据，则该对象可以设置为 null。
    //
    // 返回结果:
    //     如果可执行此命令，则为 true；否则为 false。
    bool CanExecute(object parameter);
    //
    // 摘要:
    //     定义在调用此命令时要调用的方法。
    //
    // 参数:
    //   parameter:
    //     此命令使用的数据。 如果此命令不需要传递数据，则该对象可以设置为 null。
    void Execute(object parameter);
 
}
```

这个接口比较关键的是CanExecute和Execute两个方法成员。前者表示当前命令是否可以执行，如果可以的话，WPF命令系统会自动帮我们去调用Execute方法成员。那么，我们要实现这个接口的话，通常只需要在CanExecute编写一些判断逻辑，在Execute调用一个委托就行了。至于这个委托的的签名和具体的代码内容，则是在实际应用时由开发者去编写不同的业务代码。接下来，我们来实现一个ICommand接口。

**二、ICommand的实现**

```cs
public class RelayCommand : ICommand
{
    public event EventHandler CanExecuteChanged;
    
    private Action action;
 
    public RelayCommand(Action action)
    {
        this.action = action;
    }
 
    public bool CanExecute(object parameter)
    {
        return true;
    }
 
    public void Execute(object parameter)
    {
        action?.Invoke();
    }
}
```

在上面的例子中，我们自定义了一个叫RelayCommand的Command类，非常重要的一点是，它的构造函数要求传入一个Action，这个委托传进来后，将来在Execute成员中被执行。

接下来，我们看看它的具体使用。

```cs
public class MainViewModel : ObservableObject
{
    public RelayCommand OpenCommand { get; set; } = new RelayCommand(() =>
    {
        MessageBox.Show("Hello,Command");
    });
}
```

我们在MainViewModel中编写了一个叫OpenCommand的属性，而属性的类型就是RelayCommand，在构造这个RelayCommand时，我们传入了一个匿名函数。如此，前端就可以绑定这个OpenCommand命令了。

前端代码

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
        
    </Window.Resources>
    <Grid>
        <Button Width="100" Height="30" Content="打开" Command="{Binding OpenCommand}" />
    </Grid>
</Window>
```

![[assets/WPF从小白到大佬：命令：：ICommand接口/ec09d41dfd482b141ec5c9e48d393881_MD5.jpg]]

单击按钮，弹出对话框，说明OpenCommand所指向的匿名函数代码块被执行。这便是ICommand接口最简单的自定义实现之一，如果我们需要在单击按钮时传入一些参数该怎么办呢？这就需要稍微改一下RelayCommand的代码了。

三、ICommand带参数的实现

```cs
public class RelayCommand : ICommand
{
    public event EventHandler CanExecuteChanged;
    
    private Action action;
    private Action<object> objectAction;
    public RelayCommand(Action action)
    {
        this.action = action;
    }
 
    public RelayCommand(Action<object> objectAction)
    {
        this.objectAction = objectAction;
    }
    public bool CanExecute(object parameter)
    {
        return true;
    }
 
    public void Execute(object parameter)
    {
        action?.Invoke();
        objectAction?.Invoke(parameter);
    }
}
```

在这里，我们增加了一个带`Action<object>`参数的构造函数，将来定义命令时，就可以将一个带有object参数的方法传到这个RelayCommand中来。

MainViewModel代码如下

```cs
public class MainViewModel : ObservableObject
{
    public RelayCommand OpenCommand { get; set; } = new RelayCommand(() =>
    {
        MessageBox.Show("Hello,Command");
    });
 
    public RelayCommand OpenParamCommand { get; set; } = new RelayCommand((param) =>
    {
        MessageBox.Show(param.ToString());
    });
    
}
```

在MainViewModel中，我们增加了一个OpenParamCommand命令，并传入了一个带参数的匿名函数。最后看看前端代码如何调用。

```xml
<StackPanel VerticalAlignment="Center">
    <Button Width="100" Height="30" Content="打开" Command="{Binding OpenCommand}" />
    <Button Width="100" Height="30" 
            Content="打开" 
            Command="{Binding OpenParamCommand}" 
            CommandParameter="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
</StackPanel>
```

![[assets/WPF从小白到大佬：命令：：ICommand接口/49349bb877bcbff2b154e48626f9c822_MD5.jpg]]

由此可见，我们利用CommandParameter属性将前端的button对象传递到了命令的回调函数中。虽然RelayCommand已经具备了参数的传递，但是，我们还有更优雅的方式去实现它。

四、ICommand的泛型参数实现

```cs
public class RelayCommand<T> : ICommand
{
    public event EventHandler CanExecuteChanged;
    public Action<T> Action { get; }
    public RelayCommand(Action<T> action)
    {
        Action = action;
    }
    public bool CanExecute(object parameter)
    {
        return true;
    }
 
    public void Execute(object parameter)
    {
        Action?.Invoke((T)parameter);
    }
}
```

我们利用泛型来简化了RelayCommand的定义，而在定义泛型RelayCommand和使用时，与前面带object参数的RelayCommand的用法是差不多的。

```cs
public RelayCommand<object> OpenTParamCommand { get; set; } = new RelayCommand<object>((t) =>
{
    MessageBox.Show(t.ToString());
});

```

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：071-《ICommand接口》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff