MVVMLight是一个实现MVVM模式的轻量级框架（相对于Prism），适用于Net framework版本下的WPF程序，目前已经停止维护，但是作为轻量及的开发框架，完全可以适用中小型的项目开发。它提供了IOC容器、MVMM模式、消息机制、对话框、命令等功能。本文将介绍如何安装mvvmlight以及使用它的Command命令。

一、下载安装mvvmlight框架

![[assets/WPF从小白到大佬：命令：：Mvvmlight之RelayCommand/79a7e9d1a2e2b698a461f30ff5f371ee_MD5.jpg]]

在nuget管理器中搜索mvvmlight，找到后下载安装即可。安装结束后，它会为我们的项目创建一个ViewModel的文件夹，并在其中创建两个类：ViewModelLocator和MainViewModel

![[assets/WPF从小白到大佬：命令：：Mvvmlight之RelayCommand/4b3a190b41bb19699c70bf8178a1fc75_MD5.jpg]]

由于本文只演示它的命令用法，所以就不展开mvvmlight的完整使用。需要注意的是，默认安装后，需要解决一个问题。那就是ViewModelLocator类报错问题。

![[assets/WPF从小白到大佬：命令：：Mvvmlight之RelayCommand/da519e3c151ae92de5065063fbadf702_MD5.jpg]]

这个问题就是ServiceLocator的命令空间已经不在Microsoft.Practices.ServiceLocation之中，而在CommonServiceLocator之中，所以需要将其替换一下。

![[assets/WPF从小白到大佬：命令：：Mvvmlight之RelayCommand/ad2c244da6fd7c03d49479f31f566024_MD5.jpg]]

然后，我们就可以在前面的示例的MainViewModel中，新建一条命令。

```cs
public class MainViewModel : ObservableObject
{
    public GalaSoft.MvvmLight.Command.RelayCommand<string> MvvmlightCommand { get; } 
    public MainViewModel()
    {
        MvvmlightCommand = new GalaSoft.MvvmLight.Command.RelayCommand<string>((message) =>
        {
            MessageBox.Show(message);
        });
    }
}
 
```

最后，我们在前端用一个Button来引用这个命令

```xml
<Button Content="mvvmlight" 
        Margin="5" 
        Command="{Binding MvvmlightCommand}" 
        CommandParameter="Hello,Mvvmlight"/>
```

mvvmlight框架的命令也是叫RelayCommand，同时它还有带泛型参数的RelayCommand命令。所以，大多数时候， 我们会直接安装mvvmlight、prism、ReactiveUI或者CommunityToolkit.Mvvm框架，这一类框架都是帮助我们更好的实现软件架构，所提供的功能也是大同小异，起码最基本的MVVM功能都得到了实现。

如果您理解了在前面的课程中如何实现ICommand接口，那么，这些框架中提供的命令，使用起来就变得简单许多。

![[assets/WPF从小白到大佬：命令：：Mvvmlight之RelayCommand/a21000dc445a4ab5222eebe05456329f_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：075-《mvvmlight框架之RelayCommand》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff