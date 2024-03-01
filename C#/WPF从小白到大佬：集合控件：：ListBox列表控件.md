ListBox是一个列表控件，用于显示条目类的数据，默认每行只能显示一个内容项，当然，我们可以通过修改它的数据模板，来自定义每一行（元素）的数据外观，达到显示更多数据的目的。

**一、ListBox的定义**
```cs
public class ListBox : Selector
{
    public static readonly DependencyProperty SelectionModeProperty;
    public static readonly DependencyProperty SelectedItemsProperty;
 
    public ListBox();
 
    public IList SelectedItems { get; }
    public SelectionMode SelectionMode { get; set; }
    protected object AnchorItem { get; set; }
    protected internal override bool HandlesScrolling { get; }
 
    public void ScrollIntoView(object item);
    public void SelectAll();
    public void UnselectAll();
    protected override DependencyObject GetContainerForItemOverride();
    protected override bool IsItemItsOwnContainerOverride(object item);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected override void OnIsMouseCapturedChanged(DependencyPropertyChangedEventArgs e);
    protected override void OnKeyDown(KeyEventArgs e);
    protected override void OnMouseMove(MouseEventArgs e);
    protected override void OnSelectionChanged(SelectionChangedEventArgs e);
    protected override void PrepareContainerForItemOverride(DependencyObject element, object item);
    protected bool SetSelectedItems(IEnumerable selectedItems);
 
}
```

**二、属性分析**

ListBox自身的属性比较少，SelectionMode 属性比较重要，它可以决定当前的ListBox控件是单选还是多选，它的值为Extended时，表示用户需要按下shift键才能多选。如果SelectionMode为多选状态，那么多选的结果保存在哪去了？

答案是SelectedItems 属性。

另外，ListBox还自带了滚动条，如果内容超出显示区域，这时滚动条便起作用。

我们在上一章节提过DisplayMemberPath、SelectedValuePath、SelectedItem和SelectedValue，那么，我们以一个实际的例子来说明这几个属性的用途。

**三、ListBox示例**

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
    <StackPanel>
        <ListBox x:Name="listbox" MinHeight="100" 
                 DisplayMemberPath="Name" 
                 SelectedValuePath="Age"/>
        <Button Content="查看结果" Click="Button_Click"/>
        <TextBlock x:Name="_TextBlock"/>
    </StackPanel>
</Window>
```

后端代码

```cs
using System;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Threading;
 
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
 
            listbox.Items.Add(new Person { Name = "张三", Age = 22, Address = "广东省廉江市车板镇大坝村" });
            listbox.Items.Add(new Person { Name = "李四", Age = 23, Address = "江西省景德镇市市辖区" });
            listbox.Items.Add(new Person { Name = "王五", Age = 24, Address = "上海市虹口区" });
        }
 
        private void Button_Click(object sender, RoutedEventArgs e)
        {
            var selectedItem = listbox.SelectedItem;
            var selectedValue = listbox.SelectedValue;
            _TextBlock.Text = $"{selectedItem},{selectedValue}";
        }
    }
}
```

代码分析

在前端代码中，我们设置了DisplayMemberPath属性值为“Name”，而SelectedValuePath属性值为“Age"，这两个值实际上是Person类的两个属性，F5启动调试后，我们可以在界面上看到张三、李四和王五的名字，但是看不到他们的年龄和地址，这是因为ListBox默认每行只能显示一个内容项，而且这里我们设置了DisplayMemberPath属性，只能显示名字。

![[assets/WPF从小白到大佬：集合控件：：ListBox列表控件/d1985482b75a926ff06ab41b30497354_MD5.jpg]]

我们选中ListBox中的李四，然后单点查看结果按钮，SelectedItem属性得到了一个Person类，所以输出的值为HelloWorld.Person，而SelectedValue属性得到了李四的年龄，所以输出的结果是23。

> 重庆教主说
> 
> 如果把SelectionMode属性设为多选Multiple或Extended，试试看，会发生什么效果呢？

Items属性是一个只读属性，所以我们只能能过Items的Add方法向集合增加元素。

**四、ListBoxItem子项**

其实，ListBox还有它专门配合业务开发的子项控件——ListBoxItem。ListBoxItem继承于ContentControl内容控件，仔细想，这意味着什么？还记得我们在分享ContentControl提过”它有一个叫Content属性“一嘴吗？Content属性可以容纳任意引用类型，也就是说，ListBoxItem也可以容纳任意引用类型，也就是说，ListBox的子项也可以容纳任意的引用类型。

这么一说，感觉ListBoxr还蛮强大的呢！

所以，ListBoxItem可以这样使用

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
    <StackPanel>        
        <ListBox x:Name="listbox">
            <ListBoxItem>
                <Button Content="这是一个按钮"/>
            </ListBoxItem>
            <ListBoxItem>
                <Border Height="30" Background="Red"/>
            </ListBoxItem>
            <ListBoxItem Content="这是一个字符串"/>
            <ListBoxItem>
                <ProgressBar Maximum="100" Value="50" Height="25" Width="450"/>
            </ListBoxItem>
            这里直接写字符串
            <ListBoxItem>
                <StackPanel Orientation="Horizontal">
                    <CheckBox Content="复选框"/>
                    <RadioButton Content="单选框 "/>
                </StackPanel>
            </ListBoxItem>
        </ListBox>
        <Button Content="查看结果" Click="Button_Click"/>
        <TextBlock x:Name="textblock" TextWrapping="Wrap"/>
    </StackPanel>
</Window>
```

```cs
public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();            
        }
 
        private void Button_Click(object sender, RoutedEventArgs e)
        {
            try
            {
                var selectedItem = listbox.SelectedItem;
                var content = ((ContentControl)selectedItem).Content;
                textblock.Text = $"selectedItem={selectedItem}\r\ncontent={content}";
            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message);
            }            
        }
    }
```

![[assets/WPF从小白到大佬：集合控件：：ListBox列表控件/29a00e809000cc518c226a7c279eed06_MD5.jpg]]

如上所示，我们在ListBoxj控件里面实例化了好几个ListBoxItem，但是每个ListBoxItem的Content属性都不一样，有Button，Border ，ProgressBar ，字符串，最后，我们来获取这些选中项的内容。

![[assets/WPF从小白到大佬：集合控件：：ListBox列表控件/766aac4f3d340a9dfec4ce973e45b33a_MD5.jpg]]

除了直接写的字符串不能转换之外，其它项的结果，SelectedItem属性总是ListBoxItem，而Content可以是我们设置的其它控制。

要全面了解ListBoxItem，不能不看看它的定义。

```cs
public class ListBoxItem : ContentControl
{
    public static readonly DependencyProperty IsSelectedProperty;
    public static readonly RoutedEvent SelectedEvent;
    public static readonly RoutedEvent UnselectedEvent;
 
    public ListBoxItem();
 
    public bool IsSelected { get; set; }
 
    public event RoutedEventHandler Selected;
    public event RoutedEventHandler Unselected;
 
    protected override AutomationPeer OnCreateAutomationPeer();
    protected override void OnMouseEnter(MouseEventArgs e);
    protected override void OnMouseLeave(MouseEventArgs e);
    protected override void OnMouseLeftButtonDown(MouseButtonEventArgs e);
    protected override void OnMouseRightButtonDown(MouseButtonEventArgs e);
    protected virtual void OnSelected(RoutedEventArgs e);
    protected virtual void OnUnselected(RoutedEventArgs e);
    protected internal override void OnVisualParentChanged(DependencyObject oldParent);
}
```

如上所示，可以看到ListBoxItem有一个叫IsSelected属性，表示该项是否被选中，同时，它还有两个事件，分别是Selected选中和Unselected未选中，我们可以去订阅这两个事件，以此来做一些业务。

关于ListBox以及ListBoxItem，我们就先介绍这么多，实际上它的用法远不止这些，如果加上模板、样式、数据绑定、触发器，它还可以实现许多意想不到的效果。关于这部分的内容，请参阅模板样式章节关于ListBox控件的用法。

所以，ListBox列表控件默认情况下，只能显示一个数据项，那如果我想把Person类的Name、Age、Address三个属性值都显示出来呢？有办法——ListView控件可以做到。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：039-《ListBox列表控件》-源代码-1，039-《ListBox列表控件》-源代码-2  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月1日



