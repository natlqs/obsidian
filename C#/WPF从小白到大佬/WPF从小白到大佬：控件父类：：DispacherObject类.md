话说这个千年老二DispatcherObject类，但在WPF世界中，那也是一位顶级的带头大哥，位高权重。根据常识，像这样的顶级类型，基本上都不怎么干活的，主要是把握方针路线。

.NET为WPF准备了两个线程（WPF应用启动时），分别用于呈现界面（后台线程）和管理界面（UI线程）。后台线程一直隐藏于后台默默运行，我们感知不到，我们唯一能操作的就是UI线程。

绝大多数对象或控件都必须在UI线程上创建，而且，其它后台子线程不能直接访问UI线程上的控件，那么，后台线程非要访问UI线程的控件或对象，该怎么办呢？微软说，这样吧，我在UI线程上给你们提供一个中间商Dispatcher（调度员），将Dispatcher放到一个抽象类DispatcherObject，然后我向你保证，**我所有的控件都从这个DispatcherObject类继承**，这样当你们在后台线程中要访问控件时，就可以从控件中找到那位中间商Dispatcher，由中间商来完成你要对控件的操作访问。

从此，DispatcherObject在WPF的世界中，便登上了至高无上的宝座，成为了几乎所有类型的终极基类。

而作为DispatcherObject类的成员Dispatcher（调度员）又提供了哪些功能？简单点说，它便是后台线程和前台线程的架海紫金梁，虽然所有的控件都必须在前台UI线程中创建，但是在开发过程中，难免需要在后台线程中去操作控件，于是Dispatcher调度员提供了Invoke和BeginInvoke两个方法，供我们可以安全的访问UI线程中的控件。

> 官方解释
> 
> 在 WPF 中， DispatcherObject 只能由 Dispatcher 它与之关联的访问。 例如，后台线程无法更新与 Dispatcher UI 线程上关联的内容Button。 为了使后台线程访问该 Content 属性 Button，后台线程必须将工作委托给 Dispatcher 与 UI 线程关联的工作。 这是通过使用 Invoke 或BeginInvoke。 Invoke 是同步的， BeginInvoke 是异步的。 操作将添加到指定DispatcherPriority位置的队列Dispatcher中。

我们以前面课程中的HelloWorld项目为例，在Grid中添加一个button，在MainWindow的构造函数中增加如下代码。

前端代码

```html
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d"
        Title="HelloWorld" Height="350" Width="500">
    <Grid>
        <Button x:Name="button"/>
    </Grid>
</Window>
```

后端代码

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
 
            Task.Factory.StartNew(() =>
            {
                Task.Delay(3000).Wait();
 
                button.Dispatcher.Invoke(() =>
                {
                    button.Content = "www.wpfsoft.com";
                });
            }); 
        }
    }
}
```

最后F5运行调试，我们会看到3秒后，button控件的Content属性被我们改成了 "www.wpfsoft.com"。

![[6202d27d2107557424a81fbf180a8c97_MD5.jpg]]

我们利用Task工厂创建了一个子线程（后台线程），然后调用了button的Dispatcher调度员，其Invoke方法中传入了一个匿名函数，在这个匿名函数中去改变button按钮的Content属性。

为什么button按钮有Dispatcher？因为button按钮继承了WPF的带头大哥DispatcherObject类，而DispatcherObject类有Dispatcher成员。

那么DispatcherObject 类的主要方针路线到底是什么呢？主要有两个职责：

- 提供对对象所关联的当前 Dispatcher 的访问权限，意思是说谁继承了它，谁就拥有了Dispatcher。
- 提供方法以检查 (CheckAccess) 和验证 (VerifyAccess) 某个线程是否有权访问对象（派生于 DispatcherObject）。CheckAccess 与 VerifyAccess 的区别在于 CheckAccess 返回一个布尔值，表示当前线程是否有可以使用的对象，而 VerifyAccess 则在线程无权访问对象的情况下引发异常。

那么，谁又在第一时间继承了DispatcherObject，成为了一人之下，万人之上的二当家呢？答案是DependencyObject类，关于这个类，我们在下一节讨论。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：005-《DispatcherObject类》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月12日