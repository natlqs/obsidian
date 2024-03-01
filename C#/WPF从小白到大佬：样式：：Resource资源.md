WPF中的资源是指可以在应用中的不同位置重复使用的对象。而拥有这些对象的文件，我们可称为资源文件。这些资源文件可以编译到程序集中，可使用包 URI 来引用一组特殊的 WPF 应用程序代码文件，包括窗口、页面、流文档和资源字典。 例如，可以将 Application.StartupUri 属性设置为包 URI，用于引用要在应用程序启动时加载的窗口或页面。

```cs
<Application x:Class="HelloWorld.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:HelloWorld"
             StartupUri="MainWindow.xaml">
    <Application.Resources>
        
    </Application.Resources>
</Application>
```

在App.xaml文件中，通常默认的StartupUri属性值为MainWindow.xaml，这个MainWindow.xaml文件实际上就是一个资源文件，它包含了主窗体界面的所有XAML代码。

**一、使用资源文件**

我们可以在XAML中删除StartupUri属性的设置，在Application的后端C#代码中去引用主窗体的资源文件。

```cs
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        base.OnStartup(e);
 
        Uri uri = new Uri("MainWindow.xaml", UriKind.Relative);
        base.StartupUri = uri;
        
    }        
}
```

如上所示，我们重写了OnStartup方法成员，利用Uri包引入MainWindow.xaml，并设置到StartupUri属性，F5运行，同样会启动主窗体。

WPF的资源的形式是多样化的，它可以是一个主窗体，也可以是一个画笔，或一个样式，或一个别的对象。关键的一点是，它是一个对象。什么是对象？一个类型被实例化之后，就成了一个对象。所以，我们从本质上讲，**资源就是类型实例化之后的对象**。

那么，这些资源在程序运行过程中被放在哪儿了？

**二、资源的老家**

首先，资源是以字典的形式存在于程序中——也就是ResourceDictionary（资源字典）。其次，它们通常保存在Resources属性中。哪些类型有Resources属性？

答案是：Application类、FrameworkElement基类和FrameworkContentElement基类。

> 来自官方的引述
> 
> 每个框架级元素（FrameworkElement 或 FrameworkContentElement）都具有 Resources 属性，该属性是包含已定义资源的 ResourceDictionary 类型。 你可以在任何元素上定义资源，例如 Button。 但是，最常在根元素上定义资源。资源字典中的每个资源都必须具有唯一键。 在标记中定义资源时，可通过 x:Key 指令来分配唯一键。 通常情况下，这个键是一个字符串；但是，也可使用相应的标记扩展将其设置为其他对象类型。 资源的非字符串键用于 WPF 中的某些功能区，尤其是样式、组件资源和数据样式。

从Resources属性可以分析出——每个控件都可以单独定义资源，因为它们都继承了Resources属性。但是，通常情况下，我们会在Application中去定义Resources属性，这样全局或所有的控件都可以引用。

关于如何定义资源，以及了解资源字典的概念与用法，我们将在下一节介绍。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：048-《Resource资源》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月11日


