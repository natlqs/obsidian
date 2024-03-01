ListView继承于ListBox，在ListBox控件的基础上增加了数据视图。从而让我们可以很轻松的设置每一列的标题，以显示某个数据表结构及内容。

一、ListView定义

```cs
public class ListView : ListBox
{
    public static readonly DependencyProperty ViewProperty;
 
    public ListView();
 
    public ViewBase View { get; set; }
 
    protected override void ClearContainerForItemOverride(DependencyObject element, object item);
    protected override DependencyObject GetContainerForItemOverride();
    protected override bool IsItemItsOwnContainerOverride(object item);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected override void OnItemsChanged(NotifyCollectionChangedEventArgs e);
    protected override void PrepareContainerForItemOverride(DependencyObject element, object item);
 
}
```

ListView类增加了一个叫View的属性，这个属性用来定义控件的数据样式，决定数据怎样显示。View属性的类型是ViewBase，但是，我们真正在使用View属性时，实际上实例化的是GridView类，因为GridView类是ViewBase的子类。所以，我们要看了解一下GridView的定义。

```cs
public class GridView : ViewBase, IAddChild
{
    public static readonly DependencyProperty ColumnCollectionProperty;
    public static readonly DependencyProperty ColumnHeaderContainerStyleProperty;
    public static readonly DependencyProperty ColumnHeaderTemplateProperty;
    public static readonly DependencyProperty ColumnHeaderTemplateSelectorProperty;
    public static readonly DependencyProperty ColumnHeaderStringFormatProperty;
    public static readonly DependencyProperty AllowsColumnReorderProperty;
    public static readonly DependencyProperty ColumnHeaderContextMenuProperty;
    public static readonly DependencyProperty ColumnHeaderToolTipProperty;
 
    public GridView();
 
    public static ResourceKey GridViewItemContainerStyleKey { get; }
    public static ResourceKey GridViewStyleKey { get; }
    public static ResourceKey GridViewScrollViewerStyleKey { get; }
    public string ColumnHeaderStringFormat { get; set; }
    public DataTemplateSelector ColumnHeaderTemplateSelector { get; set; }
    public DataTemplate ColumnHeaderTemplate { get; set; }
    public Style ColumnHeaderContainerStyle { get; set; }
    public GridViewColumnCollection Columns { get; }
    public object ColumnHeaderToolTip { get; set; }
    public bool AllowsColumnReorder { get; set; }
    public ContextMenu ColumnHeaderContextMenu { get; set; }
    protected internal override object ItemContainerDefaultStyleKey { get; }
    protected internal override object DefaultStyleKey { get; }
 
    public static GridViewColumnCollection GetColumnCollection(DependencyObject element);
    public static void SetColumnCollection(DependencyObject element, GridViewColumnCollection collection);
    public static bool ShouldSerializeColumnCollection(DependencyObject obj);
    public override string ToString();
    protected virtual void AddChild(object column);
    protected virtual void AddText(string text);
    protected internal override void ClearItem(ListViewItem item);
    protected internal override IViewAutomationPeer GetAutomationPeer(ListView parent);
    protected internal override void PrepareItem(ListViewItem item);
 
}
```

GridView提供了一些可供设置的模板和样式属性，这些我们先放一边，在WPF基础章节的内容学习中，我们先学习它的Columns 属性，它是一个集合属性，而集合中元素的类型是GridViewColumn。

GridViewColumn最关键的只有两个属性，分别是标题和要显示的成员（指向了Person实体的某个属性名）。

好了，我们以上一节中的Person实体为例。

二、ListView示例

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
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition Width="200"/>
        </Grid.ColumnDefinitions>
        <ListView Grid.Column="0" x:Name="listview" SelectionChanged="listview_SelectionChanged">
            <ListView.View>
                <GridView>
                    <GridViewColumn Header="姓名" DisplayMemberBinding="{Binding Name}"/>
                    <GridViewColumn Header="年龄" DisplayMemberBinding="{Binding Age}"/>
                    <GridViewColumn Header="地址" DisplayMemberBinding="{Binding Address}"/>
                </GridView>  
            </ListView.View>
        </ListView>
        <StackPanel Grid.Column="1">
            <StackPanel Orientation="Horizontal" Margin="5">
                <TextBlock Text="姓名:"/>
                <TextBlock x:Name="_TextBlockName"/>
            </StackPanel>
            <StackPanel Orientation="Horizontal" Margin="5">
                <TextBlock Text="年龄:"/>
                <TextBlock x:Name="_TextBlockAge"/>
            </StackPanel>
            <StackPanel Orientation="Horizontal" Margin="5">
                <TextBlock Text="地址:"/>
                <TextBlock x:Name="_TextBlockAddress"/>
            </StackPanel>
        </StackPanel>
    </Grid>
</Window>
```

后端代码

```cs
namespace HelloWorld
{
    public class Person
    {
        public string Name { get; set; }
        public int Age { get; set; }
        public string Address { get; set; }
 
    }
 
    /// <summary>
    /// MainWindow.xaml 的交互逻辑
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            
            listview.Items.Add(new Person { Name = "张三", Age = 22, Address = "广东省廉江市车板镇大坝村" });
            listview.Items.Add(new Person { Name = "李四", Age = 23, Address = "江西省景德镇市市辖区" });
            listview.Items.Add(new Person { Name = "王五", Age = 24, Address = "上海市虹口区" });
        }
        private void listview_SelectionChanged(object sender, SelectionChangedEventArgs e)
        {
            ListView listView = sender as ListView;
            if (listView == null) return;
 
            var person = listView.SelectedItem as Person;
            if (person == null) return;
 
            _TextBlockName.Text = person.Name;
            _TextBlockAge.Text = person.Age + "岁";
            _TextBlockAddress.Text = person.Address;
        }
    }
}
```

![[assets/WPF从小白到大佬：集合控件：：ListView数据列表控件/24a711e369502db8ea5d2d6b703158dc_MD5.jpg]]

三、代码分析

首先，我们在前端实例化了一个ListView控件，并为View属性实例化了一个GridView对象（注意xaml语法的写法）,最后为GridView对象实例化了3列GridViewColumn ，分别设置为姓名年龄和地址，特别需要注意的是DisplayMemberBinding属性的写法，这里采用了数据绑定的写法，意思是将ListView控件的数据源的Name属性显示在姓名那一列，Age属性显示在年龄那一列，Address属性显示在地址那一列（我们明确知道ListView数据源的类型就是Person的实例集合）。

事件处理

在ListView控件的SelectionChanged事件中，我们先将sender转成ListView ，再从中获取当前选中项（即person），最后显示详细信息在界面上即可。这样就演示了数据怎么加载显示到ListView，又怎么样从ListView上获取的过程。

而类似于ListView的效果效果，还有一个专门用来显示数据的控件，它叫DataGrid，从某种意义上来说，它甚至可以开发类似Excel表格的效果。不过，我们在下一节，还是以学习它的基础功能先。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：040-《ListView数据列表控件》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月6日