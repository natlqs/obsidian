CheckBox继承于ToggleButton，而ToggleButton才继承于ButtonBase基类。

CheckBox控件的结构定义：

```cs
public class CheckBox : ToggleButton
{
    public CheckBox();
 
    protected override void OnAccessKey(AccessKeyEventArgs e);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected override void OnKeyDown(KeyEventArgs e);
 
}
```

CheckBox自身没有什么特别内容。一切都使用它的父类提供的属性、方法和事件。我们举例来说明它的用法。

前端代码

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d"
        Title="HelloWorld - www.wpfsoft.com" Height="350" Width="500">
    <StackPanel Orientation="Horizontal" HorizontalAlignment="Center" VerticalAlignment="Center">
        <TextBlock Text="今晚吃什么菜?" Margin="5"/>
        <CheckBox Name="_checkbox1" Content="红烧牛肉" Margin="5"/>
        <CheckBox Name="_checkbox2" Content="麻婆豆腐" Margin="5"/>
        <CheckBox Name="_checkbox3" Content="夫妻肺片" Margin="5"/>
        <Button x:Name="_button" Content="查看菜单"  Click="_button_Click"/>
    </StackPanel>
</Window>
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
            if (_checkbox1.IsChecked == true)
                order += _checkbox1.Content + ",";
            if (_checkbox2.IsChecked == true)
                order += _checkbox2.Content + ",";
            if (_checkbox3.IsChecked == true)
                order += _checkbox3.Content;
            if(!string.IsNullOrEmpty(order))
                MessageBox.Show(order);
        }
    }
}
```

}

![[assets/WPF从小白到大佬：内容控件：：CheckBox复选框/b6b28a39a0a7271ed5cdd8552193fa86_MD5.jpg]]

如上图所示，这是前端代码呈现的界面。F5启动后，我们勾选两个选项，然后点击查看菜单，观察结果。

![[assets/WPF从小白到大佬：内容控件：：CheckBox复选框/f947a409778ec91955c7a8988a4c9c75_MD5.jpg]]

我们通过判断CheckBox的IsChecked属性，来获取前端用户的选择，这通常是CheckBox控件最常用的用法，由于IsChecked是一个依赖属性，它还可以参与绑定，形成MVMM的应用模式，待我们讲到数据绑定章节，还会进一步讲解控件属性的绑定应用。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：018-《CheckBox复选框》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月23日