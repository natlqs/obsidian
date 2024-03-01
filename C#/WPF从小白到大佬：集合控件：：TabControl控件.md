TabControl表示包含多个共享相同的空间在屏幕上的项的控件。它也是继承于Selector基类，所以TabControl也只支持单选操作。另外，TabControl的元素只能是TabItem，这个TabItem继承于HeaderedContentControl类，所以TabControl的元素实际上是一个带标题的ContentControl内容控件。

我们曾经在聊[GroupBox控件](http://www.wpfsoft.com/2023/08/29/1497.html)和[Expander折叠控件](http://www.wpfsoft.com/2023/08/31/1652.html)时都曾提到过这个HeaderedContentControl类，原来大家都用了这个带标题的内容控件。所以TabControl控件看起来就像是多个GroupBox组合而来。

一、TabControl的定义

```cs
public class TabControl : Selector
{
    public static readonly DependencyProperty TabStripPlacementProperty;
    public static readonly DependencyProperty SelectedContentProperty;
    public static readonly DependencyProperty SelectedContentTemplateProperty;
    public static readonly DependencyProperty SelectedContentTemplateSelectorProperty;
    public static readonly DependencyProperty SelectedContentStringFormatProperty;
    public static readonly DependencyProperty ContentTemplateProperty;
    public static readonly DependencyProperty ContentTemplateSelectorProperty;
    public static readonly DependencyProperty ContentStringFormatProperty;
 
    public TabControl();
 
    public DataTemplate ContentTemplate { get; set; }
    public string SelectedContentStringFormat { get; }
    public DataTemplateSelector SelectedContentTemplateSelector { get; }
    public DataTemplate SelectedContentTemplate { get; }
    public object SelectedContent { get; }
    public Dock TabStripPlacement { get; set; }
    public string ContentStringFormat { get; set; }
    public DataTemplateSelector ContentTemplateSelector { get; set; }
 
    public override void OnApplyTemplate();
    protected override DependencyObject GetContainerForItemOverride();
    protected override bool IsItemItsOwnContainerOverride(object item);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected override void OnInitialized(EventArgs e);
    protected override void OnItemsChanged(NotifyCollectionChangedEventArgs e);
    protected override void OnKeyDown(KeyEventArgs e);
    protected override void OnSelectionChanged(SelectionChangedEventArgs e);
 
}
```

二、属性成员

|   |   |
|---|---|
|属性名称|说明|
|ContentTemplate|表示TabItem元素的内容模板|
|SelectedContentStringFormat|当前所选内容的格式|
|SelectedContentTemplateSelector|获取当前选定的TabItem项的模板选择器|
|SelectedContentTemplate|当前选定的TabItem项的模板|
|SelectedContent|当前选定的TabItem项里面的内容（也是一些控件）|
|TabStripPlacement|获取或设置选项卡标题相对于选项卡上内容的对齐方式。|
|ContentStringFormat|指定如何设置内容的格式|
|ContentTemplateSelector|获取或设置内容模板选择器|

TabControl的SelectedContent可能是我们比较常用的一个属性，事实上，TabControl通常被当成布局控件来使用。

三、TabControl示例

前端代码

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld" 
        xmlns:forms="clr-namespace:System.Windows.Forms;assembly=System.Windows.Forms"
        mc:Ignorable="d" FontSize="14"
        Title="WPF中文网之控件课程 - www.wpfsoft.com" Height="350" Width="500">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition Height="50"/>
        </Grid.RowDefinitions>
        <TabControl x:Name="_tabControl" Grid.Row="0" SelectionChanged="_tabControl_SelectionChanged">
            <TabItem Header="首页">
                <Border Background="LightBlue">
                    <TextBlock Text="首页的界面" FontSize="24" HorizontalAlignment="Center" VerticalAlignment="Center"/>
                </Border>
            </TabItem>
            <TabItem Header="WPF目录">
                <Border Background="LightCoral">
                    <TextBlock Text="WPF目录的界面" FontSize="24" HorizontalAlignment="Center" VerticalAlignment="Center"/>
                </Border>
            </TabItem>
            <TabItem Header="官方文档">
                <Border Background="LightCyan">
                    <TextBlock Text="官方文档的界面" FontSize="24" HorizontalAlignment="Center" VerticalAlignment="Center"/>
                </Border>
            </TabItem>
            <TabItem Header="付费课程">
                <Border Background="LightGoldenrodYellow">
                    <TextBlock Text="付费课程的界面" FontSize="24" HorizontalAlignment="Center" VerticalAlignment="Center"/>
                </Border>
            </TabItem>
            <TabItem Header="统计">
                <Border Background="LightGreen">
                    <TextBlock Text="统计的界面" FontSize="24" HorizontalAlignment="Center" VerticalAlignment="Center"/>
                </Border>
            </TabItem>
        </TabControl>
        <TextBlock x:Name="_textBlock" TextWrapping="Wrap" Grid.Row="1"/>
    </Grid>
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
 
    private void _tabControl_SelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        var tab = sender as TabControl;
        var item = tab.SelectedItem as TabItem;
        var content = tab.SelectedContent;
        _textBlock.Text = "标题:" + item.Header.ToString() + " 内容:" + content;
    }
}
```

![[assets/WPF从小白到大佬：集合控件：：TabControl控件/ddc1dd49a4fe522dceaaa8d23fb14ce1_MD5.jpg]]

我们订阅了TabControl控件的SelectionChanged事件，并在回调函数中获取了当前选中的TabItem对象以及它里面的内容。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：043-《TabControl控件》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月6日