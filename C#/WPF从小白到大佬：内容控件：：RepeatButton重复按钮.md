RepeatButton,顾名思义，重复执行的按钮。就是当按钮被按下时，所订阅的回调函数会不断被执行。那么，多长时间执行一次？

我们先看看它的结构定义：

```cs
public class RepeatButton : ButtonBase
{
    public static readonly DependencyProperty DelayProperty;
    public static readonly DependencyProperty IntervalProperty;
 
    public RepeatButton();
 
    public int Delay { get; set; }
    public int Interval { get; set; }
 
    protected override void OnClick();
    protected override AutomationPeer OnCreateAutomationPeer();
    protected override void OnKeyDown(KeyEventArgs e);
    protected override void OnKeyUp(KeyEventArgs e);
    protected override void OnLostMouseCapture(MouseEventArgs e);
    protected override void OnMouseEnter(MouseEventArgs e);
    protected override void OnMouseLeave(MouseEventArgs e);
    protected override void OnMouseLeftButtonDown(MouseButtonEventArgs e);
    protected override void OnMouseLeftButtonUp(MouseButtonEventArgs e);
 
}
```

一、属性分析

RepeatButton 自身提供了两个整型属性，分别是Delay 和Interval 。

**Delay 属性**：表示延时重复执行的毫秒数，就是说，RepeatButton被按下后会立即执行一次回调函数，如果您不松开鼠标，在等待Delay 毫秒后，就开始进行重复执行阶段。

**Interval 属性**：表示重复执行回调函数的时间间隔毫秒数。

接下来，我们观察下面的代码的运行结果。

二、RepeatButton 应用示例

```cs
    <StackPanel Orientation="Horizontal" HorizontalAlignment="Center" VerticalAlignment="Center">
        <TextBlock Text="牙膏已在手" Margin="5 7 5 5"/>
        <RepeatButton Name="_Button1" Content="开始挤牙膏" Delay="1000" Interval="500" Click="_Button1_Click"  Margin="5"/>
    </StackPanel>
```

```cs
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
        int count = 1;
        private void _Button1_Click(object sender, RoutedEventArgs e)
        {
            Console.WriteLine($"重复时间:{DateTime.Now.ToLongTimeString()} {DateTime.Now.Millisecond},重复次数:{count++}");
        }
    }
}
```

![[assets/WPF从小白到大佬：内容控件：：RepeatButton重复按钮/c44ec559fc4825b274ee071810dfe8fb_MD5.jpg]]

我们以生活中挤牙膏为例，当开始挤的动作发生后，我们会持续一小会儿，然后才松开，此时牙膏停止往外溢出。观察下面的输出结果：

```
重复时间:14:37:28 476,重复次数:1
重复时间:14:37:29 548,重复次数:2
重复时间:14:37:30 51,重复次数:3
重复时间:14:37:30 549,重复次数:4
重复时间:14:37:31 89,重复次数:5
重复时间:14:37:31 598,重复次数:6
重复时间:14:37:32 185,重复次数:7
重复时间:14:37:32 718,重复次数:8
```

结果显示，第一次和第二次输出时间刚好为1000毫秒，也就是Delay属性在起作用。然后，从第2次开始，每两次之间的时间间隔大约为500毫秒，这是因为Interval属性被设置为500。相信通过这个例子，您已明白这个按钮的用法。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：020-《RepeatButton重复按钮》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月23日