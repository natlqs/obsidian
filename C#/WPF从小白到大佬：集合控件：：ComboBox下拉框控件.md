ComboBox表示带有下拉列表的控件，实际上您可以把它看成两个部分组成，一个类似TextBox文本输入框，所以它有一个Text文本属性，用于获取ComboBox框的文本值，另一个是类似ListBox的列表框，用于显示ComboBox绑定的所有数据源。

ComboBox继承于Selector，所以，它只能是单选操作。由于这个控件由两个部分构成，所以在用法上，我们也可以有两种主要用法——类似TextBox用法和类似ListBox用法。

我们在使用这个控件之前，先熟悉一下它的定义

一、ComboBox定义

```cs
[Localizability(LocalizationCategory.ComboBox)]
    [StyleTypedProperty(Property = "ItemContainerStyle", StyleTargetType = typeof(ComboBoxItem))]
    [TemplatePart(Name = "PART_EditableTextBox", Type = typeof(TextBox))]
    [TemplatePart(Name = "PART_Popup", Type = typeof(Popup))]
public class ComboBox : Selector
{
    public static readonly DependencyProperty MaxDropDownHeightProperty;
    public static readonly DependencyProperty IsDropDownOpenProperty;
    public static readonly DependencyProperty ShouldPreserveUserEnteredPrefixProperty;
    public static readonly DependencyProperty IsEditableProperty;
    public static readonly DependencyProperty TextProperty;
    public static readonly DependencyProperty IsReadOnlyProperty;
    public static readonly DependencyProperty SelectionBoxItemProperty;
    public static readonly DependencyProperty SelectionBoxItemTemplateProperty;
    public static readonly DependencyProperty SelectionBoxItemStringFormatProperty;
    public static readonly DependencyProperty StaysOpenOnEditProperty;
 
    public ComboBox();
 
    public bool ShouldPreserveUserEnteredPrefix { get; set; }
    public bool IsEditable { get; set; }
    public string Text { get; set; }
    public bool IsReadOnly { get; set; }
    public object SelectionBoxItem { get; }
    public double MaxDropDownHeight { get; set; }
    public string SelectionBoxItemStringFormat { get; }
    public bool StaysOpenOnEdit { get; set; }
    public bool IsSelectionBoxHighlighted { get; }
    public bool IsDropDownOpen { get; set; }
    public DataTemplate SelectionBoxItemTemplate { get; }
    protected internal override bool HandlesScrolling { get; }
    protected internal override bool HasEffectiveKeyboardFocus { get; }
 
    public event EventHandler DropDownClosed;
    public event EventHandler DropDownOpened;
 
    public override void OnApplyTemplate();
    protected override DependencyObject GetContainerForItemOverride();
    protected override bool IsItemItsOwnContainerOverride(object item);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected virtual void OnDropDownClosed(EventArgs e);
    protected virtual void OnDropDownOpened(EventArgs e);
    protected override void OnIsKeyboardFocusWithinChanged(DependencyPropertyChangedEventArgs e);
    protected override void OnIsMouseCapturedChanged(DependencyPropertyChangedEventArgs e);
    protected override void OnKeyDown(KeyEventArgs e);
    protected override void OnMouseLeftButtonUp(MouseButtonEventArgs e);
    protected override void OnPreviewKeyDown(KeyEventArgs e);
    protected override void OnSelectionChanged(SelectionChangedEventArgs e);
    protected override void PrepareContainerForItemOverride(DependencyObject element, object item);
 
}
```

二、属性成员

|   |   |
|---|---|
|属性名称|说明|
|ShouldPreserveUserEnteredPrefix|是否保留用户的输入，或者输入替换匹配项。|
|IsEditable|是否启用或禁用编辑文本框中文本|
|Text|获取或设置当前选定项的文本。|
|IsReadOnly|文本内容是否只读|
|SelectionBoxItem|获取在选择框中显示的项。|
|MaxDropDownHeight|获取或设置一个组合框下拉列表的最大高度。|
|SelectionBoxItemStringFormat|指定选择框中文本的显示格式|
|StaysOpenOnEdit|在编辑输入框文本时，希望下拉框保持打开，则为true|
|IsSelectionBoxHighlighted|是否突出显示SelectionBoxItem|
|IsDropDownOpen|是否打开组合框下拉列表。|
|SelectionBoxItemTemplate|获取选择框内容的项模板。|

接下来，我们还是一个实际的例子来说明combobox控件的用法。

三、ComboBox示例

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
        <StackPanel>
            <ComboBox x:Name="combobox1" IsEditable="True"  Height="30" Margin="20,10" 
                      TextBoxBase.TextChanged="combobox1_TextChanged"/>
            <ComboBox x:Name="combobox2" StaysOpenOnEdit="True" VerticalAlignment="Top" 
                      SelectionChanged="combobox2_SelectionChanged"
                      Height="30" Margin="20,10" DisplayMemberPath="Name">
            </ComboBox>
        </StackPanel>
        
        
        <StackPanel Grid.Column="1">
            <StackPanel Orientation="Horizontal" Margin="5">
                <TextBlock Text="电话:"/>
                <TextBlock x:Name="_TextBlockTel"/>
            </StackPanel>
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
 
            List<Person> list = new List<Person>();
 
            list.Add(new Person { Name = "张三", Age = 22, Address = "广东省廉江市车板镇大坝村" });
            list.Add(new Person { Name = "李四", Age = 23, Address = "江西省景德镇市市辖区" });
            list.Add(new Person { Name = "王五", Age = 24, Address = "上海市虹口区" });
 
            combobox2.ItemsSource = list;
        }
 
        private void combobox1_TextChanged(object sender, TextChangedEventArgs e)
        {
            _TextBlockTel.Text = combobox1.Text;
        }
 
        private void combobox2_SelectionChanged(object sender, SelectionChangedEventArgs e)
        {
            ComboBox combobox = sender as ComboBox;
            if (combobox == null) return;
 
            var person = combobox.SelectedItem as Person;
            if (person == null) return;
 
            _TextBlockName.Text = person.Name;
            _TextBlockAge.Text = person.Age + "岁";
            _TextBlockAddress.Text = person.Address;
        }
    }
}
```

![[assets/WPF从小白到大佬：集合控件：：ComboBox下拉框控件/11034751b20b20040b0d2400e3c5e29a_MD5.jpg]]

我们在xaml中实例化了两个ComboBox，第一个直接当成了TextBox来使用；第二个则绑定了一个数据源，并在Xaml中指定了DisplayMemberPath属性显示Person的Name，最后在后端代码中，依然使用SelectedItem 属性获取当前选中项，转化成Person，以获取实际的选中数据。

这些就是该控件的基本用法。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：042-《ComboBox下拉框控件》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月6日