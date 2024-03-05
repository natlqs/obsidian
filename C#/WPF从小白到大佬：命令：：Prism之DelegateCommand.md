前面提到过prism框架，它也有定义好的命令，本节课就来介绍一下这个框架的安装与命令的使用。

一、下载安装Prism框架

打开nuget包管理器，搜索prism.unity关键词，下载Prism.Unity组件。

![[assets/WPF从小白到大佬：命令：：Prism之DelegateCommand/c5eee9095741b8060931d34154af02db_MD5.jpg]]

![[assets/WPF从小白到大佬：命令：：Prism之DelegateCommand/5b87f0d534b5c8dda0b3f1553649679a_MD5.jpg]]

目前最新版本为8.1.97,Unity其实是一个IOC容器，而prism则包含了这个容器，安装完成后，会在项目的引用中找到prism的身影。

![[assets/WPF从小白到大佬：命令：：Prism之DelegateCommand/0661297cf1fc7735c4f52feaf4b84659_MD5.jpg]]

一共有3个项目，分别是prism，prism.unity.wpf和prism.wpf。正确安装好Prism框架后，我们就可以使用它的命令了，Prism框架提供了`DelegateCommand、DelegateCommand<T>和CompositeCommand`三种命令，分别是无参命令、有参命令和合并命令。

二、prism框架的命令

使用prism提供的命令分为两步，第一步定义命令，第二步调用命令。首先在C#后端的ViewModel中定义上述3种命令。

```cs
public class MainViewModel : ObservableObject
{
    public DelegateCommand DelegateCommand { get; }
    public DelegateCommand<string> ParamCommand { get; }
    public CompositeCommand CompositeCommand { get; }
    public GalaSoft.MvvmLight.Command.RelayCommand<string> MvvmlightCommand { get; } 
    public MainViewModel()
    {
        DelegateCommand = new DelegateCommand(() =>
        {
            MessageBox.Show("无参的DelegateCommand");
        });
 
        ParamCommand = new DelegateCommand<string>((message) =>
        {
            MessageBox.Show(message);
        });
        CompositeCommand = new CompositeCommand();
        CompositeCommand.RegisterCommand(DelegateCommand);
        CompositeCommand.RegisterCommand(ParamCommand);
 
        MvvmlightCommand = new GalaSoft.MvvmLight.Command.RelayCommand<string>((message) =>
        {
            MessageBox.Show(message);
        });        
    }    
}

```

在这里，我们保留了上一节课定义的MvvmLight中的命令，方便对比，其实除了类型名不同，使用方式是相同的。CompositeCommand 合并命令需要先实例化，再通过它的RegisterCommand将需要合并执行的命令注册到其中，然后在前端用三个按钮分别绑定这3个命令。

```xml
<Button Content="prism无参数" 
        Margin="5" 
        Command="{Binding DelegateCommand}" 
        CommandParameter="Hello,Prism"/>
<Button Content="prism有参数" 
        Margin="5" 
        Command="{Binding ParamCommand}" 
        CommandParameter="Hello,Prism"/>
<Button Content="prism合并命令" 
        Margin="5" 
        Command="{Binding CompositeCommand}" 
        CommandParameter="Hello,Prism"/>
```

当单击“合并命令”按钮时，会依次执行前面两个命令。

![[assets/WPF从小白到大佬：命令：：Prism之DelegateCommand/f023011263454f7897546b7da2061451_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：076-《Prism之DelegateCommand》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年10月12日