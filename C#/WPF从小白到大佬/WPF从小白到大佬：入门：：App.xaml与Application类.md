事实上，xaml类型的文件包含两部分，一部分以.xaml扩展名结尾的前端代码，另一部分以.xaml.cs结尾的后端代码，通常我们也把后端代码称为隐藏代码。

我们来看看App.xaml的前端代码和后端代码分别是什么。

前端代码的全称为Extensible Application Markup Language，简称XAML。

```html
//前端代码
<Application x:Class="HelloWorld.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:HelloWorld"
             StartupUri="MainWindow.xaml">
    <Application.Resources>
       
    </Application.Resources>
</Application>
```

```c#
//后端代码
using System;
using System.Collections.Generic;
using System.Configuration;
using System.Data;
using System.Linq;
using System.Threading.Tasks;
using System.Windows;
 
namespace HelloWorld
{
    /// <summary>
    /// App.xaml 的交互逻辑
    /// </summary>
    public partial class App : Application
    {
    }
}
```

我们先从比较熟悉的后端代码开始分析。WPF的后端代码可以是C#或VB两种语言，本教程一律采用C#语言进行演示。从上面的源代码可以看出，这里定义了一个名叫App的类型，且修饰符为partial关键字，标明App是一个局部类型。最后，App将继承Application父类。

> Partial有什么作用？
>
> 在C#中，使用 partial 关键字可以将一个类、结构体、接口或者方法分为多个部分进行声明，这些部分可以分布在同一个源文件中，也可以分布在不同的文件中。意思就是说，这里的App类型被分别定义在前端xaml和后端的cs文件中，就好比是一对夫妻，xaml主外，cs主内，两者合二为一，才能形成一个完整的App类型。

那么，前端是如何定义一个类型的呢？

在xaml代码中，我们可以看到有一个 `<Application>`的标签，同时在里面还有一句话x:Class="HelloWorld.App"，它定义一个名叫App的类型，这个类型位于命令空间HelloWorld之中，与后端代码的namespace HelloWorld保持一致。我们可以将x:Class和c#里面的class关键词看成是同一个东西，都表示定义某个类型。

既然前端代码和后端代码都继承了Application父类，那么，这个Application类到底是个什么东西？

```csharp
namespace System.Windows
{
    //
    // 摘要:
    //     封装 Windows Presentation Foundation (WPF) 应用程序。
    public class Application : DispatcherObject, IHaveResources, IQueryAmbient
    {
        [SecurityCritical]
        public Application();
        //获取或设置 System.Reflection.Assembly 提供包 统一资源标识符 (URI) 中的资源 WPF 应用程序。      
        public static Assembly ResourceAssembly { get; set; }
 
        //获取 System.Windows.Application 当前对象 System.AppDomain。
        public static Application Current { get; }
 
        //获取应用程序中实例化的窗口。
        public WindowCollection Windows { get; }
 
        //获取或设置该应用程序的主窗口。
        public Window MainWindow { get; set; }
 
        //获取或设置导致的情况， System.Windows.Application.Shutdown 来调用方法。
        public ShutdownMode ShutdownMode { get; set; }
 
        //获取或设置应用程序范围的资源，如样式和画笔的集合。
        [Ambient]
        public ResourceDictionary Resources { get; set; }
 
        //获取或设置 UI 一个应用程序启动时自动显示。
        public Uri StartupUri { get; set; }
 
        //获取应用程序作用域属性的集合。
        public IDictionary Properties { get; }
 
      
        public event EventHandler Deactivated;
        public event SessionEndingCancelEventHandler SessionEnding;
        public event DispatcherUnhandledExceptionEventHandler DispatcherUnhandledException;
        public event NavigatingCancelEventHandler Navigating;
        public event NavigatedEventHandler Navigated;
        public event NavigationProgressEventHandler NavigationProgress;
        public event NavigationFailedEventHandler NavigationFailed;
        public event LoadCompletedEventHandler LoadCompleted;
        public event EventHandler Activated;
        public event NavigationStoppedEventHandler NavigationStopped;
        public event FragmentNavigationEventHandler FragmentNavigation;
 
        public static StreamResourceInfo GetContentStream(Uri uriContent);
        public static string GetCookie(Uri uri);
        public static StreamResourceInfo GetRemoteStream(Uri uriRemote);
        public static StreamResourceInfo GetResourceStream(Uri uriResource);
        public static object LoadComponent(Uri resourceLocator);
        public static void LoadComponent(object component, Uri resourceLocator);
        public static void SetCookie(Uri uri, string value);
        public object FindResource(object resourceKey);
        public int Run(Window window);
        public int Run();
        public void Shutdown();
        public void Shutdown(int exitCode);
        public object TryFindResource(object resourceKey);
        protected virtual void OnActivated(EventArgs e);
        protected virtual void OnDeactivated(EventArgs e);
        protected virtual void OnExit(ExitEventArgs e);
        protected virtual void OnFragmentNavigation(FragmentNavigationEventArgs e);
        protected virtual void OnLoadCompleted(NavigationEventArgs e);
        protected virtual void OnNavigated(NavigationEventArgs e);
        protected virtual void OnNavigating(NavigatingCancelEventArgs e);
        protected virtual void OnNavigationFailed(NavigationFailedEventArgs e);
        protected virtual void OnNavigationProgress(NavigationProgressEventArgs e);
        protected virtual void OnNavigationStopped(NavigationEventArgs e);
        protected virtual void OnSessionEnding(SessionEndingCancelEventArgs e);
        protected virtual void OnStartup(StartupEventArgs e);
 
    }
}
```

Application类继承于DispatcherObject父类，我们曾在《WPF概述》一文中提到，DispatcherObject是WPF的最终抽象基类，下文我们将从这个最终抽象基类说起。在这里我们先讨论一下Application类。

Application类拥有一些属性、事件、和方法，与过去在C#学习中碰到的其它类型从结构上没什么不同。摘要显示，Application类是封装 Windows Presentation Foundation (WPF) 的应用程序。我们开发一款WPF应用程序，本质上是去继承了这个类，并创建了一些窗体和对话框，通过C#语言编写一系列的业务逻辑实现，最终编译成软件交付给用户。

捡几个比较重要的成员了解一下Application类吧！

一个应用程序就好比是一套房子，大多数情况下，这套房子拥有客厅、主卧、客卧、厨房、厕所、阳台等地方，而应用程序会包括多个窗体、对话框等界面，这些窗体和对话框就是这些不同的房间；在众多的窗体中，会有一个主窗体，也就是Application类的MainWindow属性，MainWindow就像是房子的客厅，因为我们推开门进去，通常映入眼帘的就是客厅。但是类似QQ软件呢，打开后并不是立即进入到主窗体，而是登录窗体，只有通过权限验证后才会进入主窗体。

那么，如何在启动程序时决定先显示哪个窗体呢？答案是Application类的StartupUri 属性。StartupUri属性是Uri类型，即统一资源标识符 (URI)，它可以指定应用程序第一次启动时显示的用户界面 (UI)。当然，它的功能还远远不止这个，关于URI，我们会专门拿一节内容探讨。

再对比一下QQ和微信两款软件，虽然都有登录窗体和主窗体，都能实现文字和语音信息交换，但是它们的界面截然不同，如何在WPF应用程序中设计出不同风格的界面？

答案是，我们只需要将这些设计要素（资源）放在Application的Resources属性中即可。换句话，我们希望窗体的背景颜色、字体大小、对各种控件所设计的模板样式等内容，全部都只需要放到Resources属性中，这样整个应用都会从Resources中去获取我们提供的资源，并最终呈现出我们想要的样子。

最后我想说的是，有诞生就会有消亡。就像秦始皇的阿房宫，从开始创建、到使用、被销毁，到最后只存在于人们的记忆中，阿房宫走完了它的一生；而一个应用程序，也会有它自己的生命周期，从它在计算机内存中诞生、诞生的过程、用户的使用过程、从内存中消失（退出），同样也会有一个生命过程，那么这个过程是怎么样的呢？

下一节，我们来讨论一下Application的生命周期。

——重庆教主 2023年8月10日
