RadioButton也继承于ToggleButton，作用是单项选择，所以被称为单选框。本质上，它依然是一个按钮，一旦被选中，不会清除，除非它”旁边“的单选框被选中。

```cs
public class RadioButton : ToggleButton
{
    public static readonly DependencyProperty GroupNameProperty;
 
    public RadioButton();
 
    public string GroupName { get; set; }
 
    protected override void OnAccessKey(AccessKeyEventArgs e);
    protected override void OnChecked(RoutedEventArgs e);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected internal override void OnToggle();
 
}
```

这个控件有一个重要属性叫GroupName——分组名称。默认值是一个空字符串。用来指定哪些RadioButton之间是互相排斥的。

我们将前面的例子简单修改一下代码。

前端代码

```cs
<StackPanel Orientation="Horizontal" HorizontalAlignment="Center" VerticalAlignment="Center">
    <TextBlock Text="今晚吃什么菜?" Margin="5"/>
    <RadioButton Name="_RadioButton1" Content="红烧牛肉" Margin="5"/>
    <RadioButton Name="_RadioButton2" Content="麻婆豆腐" Margin="5"/>
    <RadioButton Name="_RadioButton3" Content="夫妻肺片" Margin="5"/>
    <Button x:Name="_button" Content="查看菜单"  Click="_button_Click"/>
</StackPanel>
```

后端代码

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
 
        private void _button_Click(object sender, RoutedEventArgs e)
        {
            string order = string.Empty;
            if (_RadioButton1.IsChecked == true)
                order += _RadioButton1.Content + ",";
            if (_RadioButton2.IsChecked == true)
                order += _RadioButton2.Content + ",";
            if (_RadioButton3.IsChecked == true)
                order += _RadioButton3.Content;
            if(!string.IsNullOrEmpty(order))
                MessageBox.Show(order);
        }
    }
}
```

![[assets/WPF从小白到大佬：内容控件：：RadioButton单选框/139ea5e7251db8016b323a3befff2adc_MD5.jpg]]

F5运行之后，我们会发现，无论我们怎么选，始终只有一个RadioButton按钮被选中。

如果我们希望RadioButton按分组进行单项选择，该怎么办呢？

我们可以使用GroupName分组属性，两两一组，让用户始终都只能选择一荤一素两个菜，请看代码。

前端代码

```cs
<StackPanel Orientation="Horizontal" HorizontalAlignment="Center" VerticalAlignment="Center">
    <TextBlock Text="今晚吃什么菜?" Margin="5"/>
    <RadioButton Name="_RadioButton1" Content="红烧牛肉" GroupName="荤菜" Margin="5"/>
    <RadioButton Name="_RadioButton2" Content="糖醋排骨" GroupName="荤菜" Margin="5"/>
    <RadioButton Name="_RadioButton3" Content="麻婆豆腐" GroupName="素菜" Margin="5"/>
    <RadioButton Name="_RadioButton4" Content="清炒时蔬" GroupName="素菜" Margin="5"/>
    <Button x:Name="_button" Content="查看菜单"  Click="_button_Click"/>
</StackPanel>
```

后端代码

```cs
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
    }
 
private void _button_Click(object sender, RoutedEventArgs e)
{
    string order = string.Empty;
    if (_RadioButton1.IsChecked == true)
        order += _RadioButton1.Content + ",";
    if (_RadioButton2.IsChecked == true)
        order += _RadioButton2.Content + ",";
    if (_RadioButton3.IsChecked == true)
        order += _RadioButton3.Content + ",";
    if (_RadioButton4.IsChecked == true)
        order += _RadioButton4.Content;
    if (!string.IsNullOrEmpty(order))
        MessageBox.Show(order);
}
```

![[assets/WPF从小白到大佬：内容控件：：RadioButton单选框/92e9339891b52d16f8fe17e77744e56f_MD5.jpg]]

此时再操作时我们发现，红烧牛肉和糖醋排骨只能二选一，麻婆豆腐和清炒时蔬也只能二选一。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：019-《RadioButton单选框》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月23日