Window窗体，其实也是一个控件，一个Application应用实例可能会有多个窗体，这些窗体随着用户的操作被创建于内存，最后被销毁于内存。大多数情况下，销毁的请求虽然由用户发起，但最终回收内存则是由GC垃圾回收器在干活儿。

我们以HelloWorld应用为例，打开MainWindowp窗体的源代码，切换至后端代码，可以发现MainWindow继承于Window类。

```csharp
namespace HelloWorld
{
    /// <summary>
    /// MainWindow.xaml 的交互逻辑
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }
    }
}
```

将鼠标的光标放至“Window”字符串上面，按下F12 ,就可以导航到Window类的定义页面，我们会发现Window类又继承于ContentControl类，下面是MainWindow主窗体的整个继承路线。

`MainWindow->Window->ContentControl->Control->FrameworkElement->UIElement->Visual->DependencyObject->DispatcherObject`

在这里，我们并不打算将MainWindow的所有知识详尽，因为这必须要将它一路继承下来的所有父亲都要交代清楚，我们只关注MainWinow的生命周期。所以我们在MainWinow的构造函数中写下如下代码：

```csharp
namespace HelloWorld
{
    /// <summary>
    /// MainWindow.xaml 的交互逻辑
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
 
            this.SourceInitialized += (s, e) => Console.WriteLine("1.MainWindow的SourceInitialized被执行");
 
            this.Activated += (s, e) => Console.WriteLine("2.MainWindow的Activated被执行");
 
            this.Loaded += (s, e) => Console.WriteLine("3.MainWindow的Loaded被执行");
 
            this.ContentRendered += (s, e) => Console.WriteLine("4.MainWindow的ContentRendered被执行");
 
            this.Deactivated += (s, e) => Console.WriteLine("5.MainWindow的Deactivated被执行");
 
            this.Closing += (s, e) => Console.WriteLine("6.MainWindow的Closing被执行");
 
            this.Closed += (s, e) => Console.WriteLine("7.MainWindow的Closed被执行");
 
            this.Unloaded += (s, e) => Console.WriteLine("8.MainWindow的Unloaded被执行");
 
        }
    }
}
```

然后我们直接F5调试，待主窗体显示后，直接关闭主窗体，观察输出（Ctrl+Alt+Q）结果。Application的生命周期和主窗体的生命周期是充满交织的，首先是Application的OnStartup，然后是主窗体的SourceInitialized，然后依次执行了Application的OnActivated和MainWindow的Activated，最后直到主窗体Closed，才轮到Application的OnExit。

```
1.OnStartup被触发
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\PresentationFramework.Aero2\v4.0_4.0.0.0__31bf3856ad364e35\PresentationFramework.Aero2.dll”。已跳过加载符号。模块进行了优化，并且调试器选项“仅我的代码”已启用。
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\PresentationCore.resources\v4.0_4.0.0.0_zh-Hans_31bf3856ad364e35\PresentationCore.resources.dll”。模块已生成，不包含符号。
1.MainWindow的SourceInitialized被执行
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“c:\program files\microsoft visual studio\2022\community\common7\ide\commonextensions\microsoft\xamldiagnostics\Framework\x86\Microsoft.VisualStudio.DesignTools.WpfTap.dll”。已跳过加载符号。模块进行了优化，并且调试器选项“仅我的代码”已启用。
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Runtime.Serialization\v4.0_4.0.0.0__b77a5c561934e089\System.Runtime.Serialization.dll”。已跳过加载符号。模块进行了优化，并且调试器选项“仅我的代码”已启用。
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\SMDiagnostics\v4.0_4.0.0.0__b77a5c561934e089\SMDiagnostics.dll”。已跳过加载符号。模块进行了优化，并且调试器选项“仅我的代码”已启用。
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.ServiceModel.Internals\v4.0_4.0.0.0__31bf3856ad364e35\System.ServiceModel.Internals.dll”。已跳过加载符号。模块进行了优化，并且调试器选项“仅我的代码”已启用。
2.OnActivated被触发
2.MainWindow的Activated被执行
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\System.Runtime.Serialization.resources\v4.0_4.0.0.0_zh-Hans_b77a5c561934e089\System.Runtime.Serialization.resources.dll”。模块已生成，不包含符号。
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\UIAutomationTypes\v4.0_4.0.0.0__31bf3856ad364e35\UIAutomationTypes.dll”。已跳过加载符号。模块进行了优化，并且调试器选项“仅我的代码”已启用。
“HelloWorld.exe”(CLR v4.0.30319: HelloWorld.exe): 已加载“C:\WINDOWS\Microsoft.Net\assembly\GAC_MSIL\UIAutomationProvider\v4.0_4.0.0.0__31bf3856ad364e35\UIAutomationProvider.dll”。已跳过加载符号。模块进行了优化，并且调试器选项“仅我的代码”已启用。
3.MainWindow的Loaded被执行
4.MainWindow的ContentRendered被执行
5.MainWindow的Closing被执行
6.MainWindow的Deactivated被执行
3.OnDeactivated被触发
7.MainWindow的Closed被执行
4.OnExit被触发
程序“[4808] HelloWorld.exe”已退出，返回值为 0 (0x0)。
```

我们来单独看看主窗体的生命周期，在上述输出结果中寻找带“MainWindow”的字符串，可以发现如下的输出结果。

- 1.MainWindow的SourceInitialized被执行
- 2.MainWindow的Activated被执行
- 3.MainWindow的Loaded被执行
- 4.MainWindow的ContentRendered被执行
- 5.MainWindow的Closing被执行
- 6.MainWindow的Deactivated被执行
- 7.MainWindow的Closed被执行

观察这些输出结果，与我们订阅事件的代码顺序一致，唯独少了Unloaded的结果输出。因为Unloaded事件没有被触发。下面我们将分析一下这些事件分别代表什么含义。

|                   |                                                |
| ----------------- | ---------------------------------------------- |
| SourceInitialized | 创建窗体源时引发此事件                         |
| Activated         | 当前窗体成为前台窗体时引发此事件               |
| Loaded            | 当前窗体内部所有元素完成布局和呈现时引发此事件 |
| ContentRendered   | 当前窗体的内容呈现之后引发此事件               |
| Closing           | 当前窗体关闭之前引发此事件                     |
| Deactivated       | 当前窗体成为后台窗体时引发此事件               |
| Closed            | 当前窗体关闭之后引发此事件                     |
| Unloaded          | 当前窗体从元素树中删除时引发此事件             |

由此我们可以得出结论，Window窗体的生命周期应如下图所示：

![[assets/WPF从小白到大佬：入门：：Window窗体的生命周期/559ce7639548fe2b18c4928f5052c0e9_MD5.jpg]]

在了解窗体的生命周期之后，我们就可以在它不同的生命周期处理一些不同的业务。例如在Application或Window的创建时加载一些本地设置，在窗体关闭或应用程序退出时保存一些本地设置。在了解了Window窗体的生命周期之后，我们再来学习Window窗体本身由哪些构成。

下一节，我们将讨论Window窗体的组成。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
>
> 文件名：003-《Window的生命周期》-源代码
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA
> 提取码：wpff

——重庆教主 2023年8月11日

