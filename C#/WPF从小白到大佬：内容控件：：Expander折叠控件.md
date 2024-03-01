Expander也是一个内容控件，它有一个标题属性和内容属性。我们曾在前面讲解GroupBox时提到过父类[HeaderedContentControl（标题内容控件）](http://www.wpfsoft.com/2023/08/29/1497.html)。所以，感兴趣的小伙伴可以单击其链接前往复习。在这里，我们只关注Expander的基本功能，也就是可折叠。

一、Expander类的定义

```cs
public class Expander : HeaderedContentControl
{
    public static readonly DependencyProperty ExpandDirectionProperty;
    public static readonly DependencyProperty IsExpandedProperty;
    public static readonly RoutedEvent ExpandedEvent;
    public static readonly RoutedEvent CollapsedEvent;
 
    public Expander();
 
    public ExpandDirection ExpandDirection { get; set; }
    public bool IsExpanded { get; set; }
 
    public event RoutedEventHandler Expanded;
    public event RoutedEventHandler Collapsed;
 
    public override void OnApplyTemplate();
    protected virtual void OnCollapsed();
    protected override AutomationPeer OnCreateAutomationPeer();
    protected virtual void OnExpanded();
 
}
```

Expander自身只提供了两个属性，分别是ExpandDirection和IsExpanded。

ExpandDirection属性定义了Expander的内容在打开时的方向。它是一个枚举值，分别有Down、Up、Left和Right四个方向，默认方向为Down。

IsExpanded属性用来获取或设置内容窗口是否可见。比如在后端代码中，将这个属性赋值true，意味着展开Expander。

它还有两个事件成员，分别是Expanded和Collapsed，也就是其内容在展开和隐藏时触发。

二、Expander示例

前端代码

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d" FontSize="14"
        Title="WPF中文网之控件课程 - www.wpfsoft.com" Height="350" Width="500">
    <StackPanel Margin="15">
        <TextBlock Text="控件课程"/>
        <Expander Header="Button控件" 
                  ExpandDirection="Down" 
                  Expanded="Expander_Expanded" 
                  Collapsed="Expander_Collapsed">
            <Grid Background="#FFE5E5E5">
                <TextBlock TextWrapping="Wrap" Padding="10">
                    表示 Windows 按钮控件，该按钮对 Click 事件做出反应。
                    Button类 直接从 System.Windows.Controls.Primitives.ButtonBase 类继承。
                    Button是内容模型ContentControl的子类。ContentControl内容属性为 Content。
                    在用户单击 Button时做出响应的事件叫ButtonBase.Click 。                    
                </TextBlock>
            </Grid>
        </Expander>
        <Expander Header="TextBox控件" ExpandDirection="Left">
            <Grid Background="#FFE5E5E5">
                <TextBlock TextWrapping="Wrap" Padding="10">
                    TextBox控件是WPF的文本输入控件，使用户输入录入系统数据的入口之一。
                    有了此控件，用户可以将数据按照软件的流程录入进去。它允许用户输入一行或多行数据。
                </TextBlock>
            </Grid>
        </Expander>
        <Expander Header="ListBox控件" ExpandDirection="Right">
            <Grid Background="#FFE5E5E5">
                <TextBlock TextWrapping="Wrap" Padding="10">
                    ListBox控件继承自ContentControl类，是一个容器类的控件，
                    向ListBox控件中包含ListBoxItem元素向容器中添加成分，
                    也可以添加其他任意的控件。
                </TextBlock>
            </Grid>
        </Expander>
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
        }
 
        private void Expander_Expanded(object sender, RoutedEventArgs e)
        {
            MessageBox.Show("Expander展开");
        }
 
        private void Expander_Collapsed(object sender, RoutedEventArgs e)
        {
            MessageBox.Show("Expander隐藏");
 
        }
    }
```

![[assets/WPF从小白到大佬：内容控件：：Expander折叠控件/51aedafaa01aa5f1a04a0f71cffa353f_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：036-《Expander折叠控件》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月31日