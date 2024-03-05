ReactiveUI 是集成了 .Net 的 ReatIve 扩展的 MVVM 框架，用来创建运行与任何移动设备或者桌面平台的优雅的可测试的用户接口。它支持 Xamarin.iOS，Xamarin.Android，Xamarin.Mac， WPF，Windows Forms，Windows Phone 8 和 Windows Store 应用程序。

**ReactiveUI**是一个可组合的跨平台模型 - 视图 - 视图模型框架，适用于所有.NET平台，受功能性反应式编程的启发。它允许您在一个可读位置围绕功能表达想法，从用户界面抽象出可变状态，并提高应用程序的可测试性

## 反应式编程简介[](https://www.wpfsoft.com/2023/10/17/2491.html#%E5%8F%8D%E5%BA%94%E5%BC%8F%E7%BC%96%E7%A8%8B%E7%AE%80%E4%BB%8B)

很久以前，当计算机编程首次出现时，机器必须手动编程。如果技术人员以正确的顺序输入正确的机器代码序列，则生成的程序行为将满足业务要求。而不是告诉计算机如何完成它的工作，哪个容易出错并且过分依赖于程序员的无懈可击，为什么我们不告诉它它的工作是什么，让它把剩下的工作弄清楚了？

ReactiveUI的灵感来自功能反应式编程的范例，它允许您将用户输入建模为随时间变化的函数。这非常酷，因为它允许您从用户界面中抽象出可变状态，并在一个可读位置表达功能，同时提高应用程序可测试性。

如何安装ReactiveUI组件？在VisualStdio的nuget包管理器中搜索关键字，请安装ReactiveUI.WPF版本，安装结束后，会在项目的引用中看到ReactiveUI和ReactiveUI.WPF。

![[assets/WPF从小白到大佬：命令：：ReactiveUI之ReactiveCommand/fcc1dd3c656c37ac430a2abac453eea6_MD5.jpg]]

如何在ReactiveUI中使用命令？ReactiveCommand是使用静态工厂方法创建的，该方法允许您创建同步或异步执行的命令逻辑。在ReactiveCommand类型中提供了一系列的以Create开头的静态方法，这些方法可以创建不同的命令。ReactiveCommand命令继承了一个带泛型的命令基类ReactiveCommandBase<TParam,TResult>,所以它的回调函数都是带有参数和返回值的，如果无需传参及返回值，可将泛型实参写成Unit。另外，ReactiveUI采用了大量的观察者模式，所以在创建命令时，还可以使用IObservable执行逻辑，并去订阅当前命令，当命令执行后，再去执行订阅回调函数。下面我简单罗列一下ReactiveUI命令的创建方式。

Create()创建一个命令，执行同步Func或Action。

CreateCombined()创建一个合并命令，可一次性执行多个命令。

CreateFromObservable()创建一个命令，使用IObservable执行逻辑。

CreateFromTask()创建一个命令，执行C#任务，允许使用C#async/await运算符。

CreateRunInBackground()创建一个背景命令。

## ReactiveCommand示例[](https://www.wpfsoft.com/2023/10/17/2491.html#ReactiveCommand%E7%A4%BA%E4%BE%8B)

首先，我们创建一个MainViewModel，并在其中声明一些命令。

```cs
internal class MainViewModel:ReactiveObject
{
    public ICommand GeneralCommand { get; }
    public ICommand ParameterCommand { get; }
    public ICommand TaskCommand { get; }
    public ICommand CombinedCommand { get; }
    public ReactiveCommand<Unit,DateTime> ObservableCommand { get; }
    public MainViewModel()
    {
        GeneralCommand = ReactiveCommand.Create(General);
        ParameterCommand = ReactiveCommand.Create<object, bool>(Parameter);
        TaskCommand = ReactiveCommand.CreateFromTask(RunAsync);
 
        var childCommand = new List<ReactiveCommandBase<Unit,Unit>>();
        childCommand.Add(ReactiveCommand.Create<Unit, Unit>((o) => 
        {
           
            MessageBox.Show("childCommand1");
            return Unit.Default; 
        }));
        childCommand.Add(ReactiveCommand.Create<Unit, Unit>((o) =>
        {
            MessageBox.Show("childCommand2");
            return Unit.Default;
        }));
        childCommand.Add(ReactiveCommand.Create<Unit, Unit>((o) =>
        {
            MessageBox.Show("childCommand3");
            return Unit.Default;
        }));
 
        CombinedCommand = ReactiveCommand.CreateCombined(childCommand);
 
        ObservableCommand = ReactiveCommand.CreateFromObservable<Unit, DateTime>(DoObservab
        ObservableCommand.Subscribe(v => ShowObservableResult(v));
 
    }
 
    private void RunInBackground()
    {
        throw new NotImplementedException();
    }
 
    private IObservable<DateTime> DoObservableCommand(Unit arg)
    {
        //todo 业务代码
 
        var result = DateTime.Now;
 
        return Observable.Return(result).Delay(TimeSpan.FromSeconds(1));
    }
 
    private void ShowObservableResult(DateTime v)
    {
        MessageBox.Show($"时间：{v}");
    }
 
    private async Task RunAsync()
    {
        await Task.Delay(3000);
    }
 
    private bool Parameter(object arg)
    {
        MessageBox.Show(arg.ToString());
        return true;
    }
 
    private void General()
    {
        MessageBox.Show("ReactiveCommand！");
    }       
}
 
```

在这个示例中，并分演示了ReactiveCommand的普通命令、带参命令、Task命令、合并命令和观察者命令的用法。接下来创建XAML前端控件对象，将这些命令绑定到Button上面。

```xml
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d"
        Title="WPF从小白到大佬 - 命令" Height="350" Width="500">
    <Window.DataContext>
        <local:MainViewModel/>
    </Window.DataContext>
    <StackPanel>
        <TextBlock Text="ReactiveUI之ReactiveCommand课程" FontSize="28" Margin="5"/>
        <StackPanel Orientation="Horizontal">
            <Button Margin="5" Content="普通命令" Command="{Binding GeneralCommand}"/>
            <Button Margin="5" Content="参数命令" Command="{Binding ParameterCommand}" 
                    CommandParameter="Hello,Parameter"/>
            <Button Margin="5" Content="子线程命令" Command="{Binding TaskCommand}"/>
            <Button Margin="5" Content="合并命令" Command="{Binding CombinedCommand}"/>
            <Button Margin="5" Content="Observable命令" Command="{Binding ObservableCommand}"/>
        </StackPanel>
    </StackPanel>
</Window>
```

![[assets/WPF从小白到大佬：命令：：ReactiveUI之ReactiveCommand/00ecfd36822b0198393742be61a87658_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：077-《ReactiveUI之ReactiveCommand》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年10月17日