Label控件继承于ContentControl控件，它是一个文本标签，如果您想修改它的标签内容，请设置Content属性。我们曾提过ContentControl的Content属性是object类型，意味着Label的Content也是可以设置为任意的引用类型的。

我们来举个例子。

```cs
<StackPanel Orientation="Horizontal" HorizontalAlignment="Center" VerticalAlignment="Center">
    <Label Content="这是一个Label标签"/>
    <Label>
        <Label.Content>
            <Button Content="确定" Click="_Button1_Click"/>
        </Label.Content>
    </Label>
</StackPanel>
```

```cs
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
    }
 
private void _Button1_Click(object sender, RoutedEventArgs e)
{
    this.Close();
}
```

![[assets/WPF从小白到大佬：内容控件：：Label标签/0ec99a196b1035c18dffa27a430727fa_MD5.jpg]]

我们给第二个标签的Content属性设置了一个按钮，并对按钮的Click事件做了订阅回调，F5运行，事实证明，此时的Button是可以正常使用 。只不过，通常情况下，我们的Label只是用来显示一段文字，很少在Contnet里面编写其它控件代码。如果要编写其它控件代码以实现更复杂的自定义控件效果，我们建议使用UserControl用户控件。

对于文本的显示，除了可以在Label中显示，我们还有一个控件也可以实现，那就是TextBlock文字块。而且，TextBlock控件直接从FrameworkElement基类继承而来，效率比Label标签更高哦。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：021-《Label标签》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月23日