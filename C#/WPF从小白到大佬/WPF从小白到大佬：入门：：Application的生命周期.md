关于软件的生命周期，是指从启动软件到关闭软件的整个过程。

```html
<Application x:Class="HelloWorld.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:HelloWorld"
             StartupUri="MainWindow.xaml">
    <Application.Resources>
       
    </Application.Resources>
</Application>
```

我们观察一下HelloWorld应用程序的App.xaml前端代码， **StartupUri="MainWindow.xaml"** 表示本程序第一个启动的窗体是MainWindow，在前端代码中按下F7将进入到App的后端代码界面，在后端代码中，键入下面这些代码。

```csharp

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
        protected override void OnStartup(StartupEventArgs e)
        {
            base.OnStartup(e);
            Console.WriteLine("1.OnStartup被触发");
        }
 
        protected override void OnActivated(EventArgs e)
        {
            base.OnActivated(e);
            Console.WriteLine("2.OnActivated被触发");
        }
 
        protected override void OnDeactivated(EventArgs e)
        {
            base.OnDeactivated(e);
            Console.WriteLine("3.OnDeactivated被触发");
        }
 
        protected override void OnExit(ExitEventArgs e)
        {
            base.OnExit(e);
            Console.WriteLine("4.OnExit被触发");
        }
    }
}
```

然后我们按下F5启动调试流程，最小化窗体，还原窗体，最后关闭窗体。回到开发界面，在菜单栏-视图-输出中找到如下信息（或快捷键Ctro+Alt+Q）

```csharp
1.OnStartup被触发
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“C:\Windows\Microsoft.Net\assembly\GAC_MSIL\PresentationFramework.Aero2\v4.0_4.0.0.0__31bf3856ad364e35\PresentationFramework.Aero2.dll”。已跳过加载符号。模块进行了优化，并且调试器选项“仅我的代码”已启用。
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“C:\Windows\Microsoft.Net\assembly\GAC_MSIL\PresentationCore.resources\v4.0_4.0.0.0_zh-Hans_31bf3856ad364e35\PresentationCore.resources.dll”。模块已生成，不包含符号。
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“c:\program files\microsoft visual studio\2022\community\common7\ide\commonextensions\microsoft\xamldiagnostics\Framework\x86\Microsoft.VisualStudio.DesignTools.WpfTap.dll”。已跳过加载符号。模块进行了优化，并且调试器选项“仅我的代码”已启用。
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“C:\Windows\Microsoft.Net\assembly\GAC_MSIL\System.Runtime.Serialization\v4.0_4.0.0.0__b77a5c561934e089\System.Runtime.Serialization.dll”。已跳过加载符号。模块进行了优化，并且调试器选项“仅我的代码”已启用。
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“C:\Windows\Microsoft.Net\assembly\GAC_MSIL\SMDiagnostics\v4.0_4.0.0.0__b77a5c561934e089\SMDiagnostics.dll”。已跳过加载符号。模块进行了优化，并且调试器选项“仅我的代码”已启用。
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“C:\Windows\Microsoft.Net\assembly\GAC_MSIL\System.ServiceModel.Internals\v4.0_4.0.0.0__31bf3856ad364e35\System.ServiceModel.Internals.dll”。已跳过加载符号。模块进行了优化，并且调试器选项“仅我的代码”已启用。
2.OnActivated被触发
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“C:\Windows\Microsoft.Net\assembly\GAC_MSIL\System.Runtime.Serialization.resources\v4.0_4.0.0.0_zh-Hans_b77a5c561934e089\System.Runtime.Serialization.resources.dll”。模块已生成，不包含符号。
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“C:\Windows\Microsoft.Net\assembly\GAC_MSIL\UIAutomationTypes\v4.0_4.0.0.0__31bf3856ad364e35\UIAutomationTypes.dll”。已跳过加载符号。模块进行了优化，并且调试器选项“仅我的代码”已启用。
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“C:\Windows\Microsoft.Net\assembly\GAC_MSIL\UIAutomationProvider\v4.0_4.0.0.0__31bf3856ad364e35\UIAutomationProvider.dll”。已跳过加载符号。模块进行了优化，并且调试器选项“仅我的代码”已启用。
3.OnDeactivated被触发
2.OnActivated被触发
3.OnDeactivated被触发
4.OnExit被触发
程序“[10696] HelloWorld.exe”已退出，返回值为 0 (0x0)
```


我们可以明确的看到，在输入信息中所打印的信息按如下的顺序显示：

* 1.OnStartup被触发
* 2.OnActivated被触发
* 3.OnDeactivated被触发
* 2.OnActivated被触发
* 3.OnDeactivated被触发
* 4.OnExit被触发

由于我们可以得出结论，当我们启动WPF应用时，首先被执行的是OnStartup方法，其次是OnActivated方法，如果我们把当前应用最小化或切换到其它程序时，这时OnDeactivated会被执行，再切回来时再次执行OnActivated方法，最后，当我们关闭程序时，OnDeactivated会再次被执行，最后执行的是OnExit方法。

> Application的生命周期
>
> OnStartup->OnActivated->OnDeactivated->OnExit

* OnStartup：表示启动应用程序时
* OnActivated：表示激活应用程序时
* OnDeactivated：表示由激活状态变为非激活状态时
* OnExit：表示退出应用程序时

这只是Application的生命周期，事实上，由于Application总是会启动一个界面（窗体），而窗体也会有自己的生命周期。下一节，我们将讨论一下窗体（Window)的生命周期。

> 当前课程源码下载：（注明：本站所有源代码请按标题搜索）
>
> 文件名：002-《Application的生命周期》-源代码
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA
> 提取码：wpff

——重庆教主 2023年8月10日
