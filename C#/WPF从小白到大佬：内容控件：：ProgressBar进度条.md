ProgressBar进度条通常在我们执行某个任务需要花费大量时间时使用，这时可以采用进度条显示任务或线程的执行进度，以便给用户良好的使用体验。

**一、ProgressBar类定义**

```cs
public class ProgressBar : RangeBase
{
    public static readonly DependencyProperty IsIndeterminateProperty;
    public static readonly DependencyProperty OrientationProperty;
 
    public ProgressBar();
 
    public bool IsIndeterminate { get; set; }
    public Orientation Orientation { get; set; }
 
    public override void OnApplyTemplate();
    protected override AutomationPeer OnCreateAutomationPeer();
    protected override void OnMaximumChanged(double oldMaximum, double newMaximum);
    protected override void OnMinimumChanged(double oldMinimum, double newMinimum);
    protected override void OnValueChanged(double oldValue, double newValue);
 
}
```

ProgressBar自身只有两个属性，分别是IsIndeterminate和Orientation 。

IsIndeterminate属性：如果为true，表示以动画从左到右滑动的方式展示进度效果。

Orientation属性：表示进度条的方式，水平时从左至右增长，垂直时从下到上增长。

**二、ProgressBar例子**

前端代码

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d" FontSize="16"
        Title="WPF中文网之控件课程 - www.wpfsoft.com" Height="350" Width="500">
    <StackPanel VerticalAlignment="Center">
        <ProgressBar x:Name="_ProgressBar" 
                     IsIndeterminate="False"  
                     Value="50" 
                     Minimum="0" 
                     Maximum="100" 
                     Orientation="Horizontal" 
                     Height="10" 
                     Margin="15"/>
        <TextBlock x:Name="_TextBlock" 
                   Text="50%" 
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Window>
```

后端代码

```cs
public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            Loaded += (s, e) =>
            {
                Task.Factory.StartNew(() =>
                {
                    for (int i = 0; i <= 100; i++)
                    {
                        Dispatcher.Invoke(() => {
 
                            _TextBlock.Text = $"{i}%";
                            _ProgressBar.Value = i;
                        });
                        
                        Task.Delay(25).Wait();
                    }
                });
            };
 
        }
    }
```

**三、代码分析**

我们在主窗体的Loaded事件中增加了一个子线程，需要注意的是，在子线程中不可以直接更新UI线程的控件，所以我们利用Dispatcher类，将访问UI线程的代码成成一个匿名函数，交给Dispatcher去执行。F5运行后，您将看到一个进度条从0增长到100。

![[assets/WPF从小白到大佬：内容控件：：ProgressBar进度条/432908de6a95edad833d9653b15e1c0c_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：032-《ProgressBar进度条》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月30日