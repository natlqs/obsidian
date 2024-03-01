一、集合控件概述

很多时候，我们需要显示大量的数据，这些数据虽然众多，但是数据类型结构相同的，由于内容控件只能显示单个元素，要显示或操作多个元素组成的集合，那么，集合控件就派上用场了。WPF中的集合控件种类丰富，有类似表格的DataGrid，有单列表的ListBox，也有介于两者之前的ListView，还有，软件的菜单通常也是一个集合控件，以及软件下方的状态栏，同样也是一个集合控件。

这些集合控件都有一个共同的基类控件，那就是ItemsControl类，下面我们以表格的形式展示一下即将要学习的集合控件。

|   |   |
|---|---|
|控件名|说明|
|ItemsControl|集合控件的基类，本身也是一个可以实例化的控件|
|ListBox|一个列表集合控件|
|ListView|表示用于显示数据项列表的控件，它可以有列头标题|
|DataGrid|表示可自定义的网格中显示数据的控件。|
|ComboBox|表示带有下拉列表的选择控件，通过单击控件上的箭头可显示或隐藏下拉列表。|
|TabControl|表示包含多个共享相同的空间在屏幕上的项的控件。|
|TreeView|用树结构(其中的项可以展开和折叠)中显示分层数据的控件|
|Menu|表示一个 Windows 菜单控件，该控件可用于按层次组织与命令和事件处理程序关联的元素。|
|ContextMenu|表示使控件能够公开特定于控件的上下文的功能的弹出菜单。|
|StatusBar|表示应用程序窗口中的水平栏中显示项和信息的控件。|

二、ItemsControl类定义

```cs
public class ItemsControl : Control, IAddChild, IGeneratorHost, IContainItemStorage
{
    public static readonly DependencyProperty ItemsSourceProperty;
    public static readonly DependencyProperty HasItemsProperty;
    public static readonly DependencyProperty DisplayMemberPathProperty;
    public static readonly DependencyProperty ItemTemplateProperty;
    public static readonly DependencyProperty ItemTemplateSelectorProperty;
    public static readonly DependencyProperty ItemStringFormatProperty;
    public static readonly DependencyProperty ItemBindingGroupProperty;
    public static readonly DependencyProperty ItemContainerStyleProperty;
    public static readonly DependencyProperty ItemContainerStyleSelectorProperty;
    public static readonly DependencyProperty ItemsPanelProperty;
    public static readonly DependencyProperty IsGroupingProperty;
    public static readonly DependencyProperty GroupStyleSelectorProperty;
    public static readonly DependencyProperty AlternationCountProperty;
    public static readonly DependencyProperty AlternationIndexProperty;
    public static readonly DependencyProperty IsTextSearchEnabledProperty;
    public static readonly DependencyProperty IsTextSearchCaseSensitiveProperty;
 
    public ItemsControl();
 
    public int AlternationCount { get; set; }
    public GroupStyleSelector GroupStyleSelector { get; set; }
    public ObservableCollection<GroupStyle> GroupStyle { get; }
    public bool IsGrouping { get; }
    public ItemsPanelTemplate ItemsPanel { get; set; }
    public StyleSelector ItemContainerStyleSelector { get; set; }
    public Style ItemContainerStyle { get; set; }
    public BindingGroup ItemBindingGroup { get; set; }
    public string ItemStringFormat { get; set; }
    public DataTemplateSelector ItemTemplateSelector { get; set; }
    public DataTemplate ItemTemplate { get; set; }
    public string DisplayMemberPath { get; set; }
    public bool HasItems { get; }
    public ItemContainerGenerator ItemContainerGenerator { get; }
    public IEnumerable ItemsSource { get; set; }
    public ItemCollection Items { get; }
    public bool IsTextSearchCaseSensitive { get; set; }
    public bool IsTextSearchEnabled { get; set; }
    protected internal override IEnumerator LogicalChildren { get; }
 
    public static DependencyObject ContainerFromElement(ItemsControl itemsControl, DependencyObject element);
    public static int GetAlternationIndex(DependencyObject element);
    public static ItemsControl GetItemsOwner(DependencyObject element);
    public static ItemsControl ItemsControlFromItemContainer(DependencyObject container);
    public override void BeginInit();
    public DependencyObject ContainerFromElement(DependencyObject element);
    public override void EndInit();
    public bool IsItemItsOwnContainer(object item);
    public bool ShouldSerializeGroupStyle();
    public bool ShouldSerializeItems();
    public override string ToString();
    protected virtual void AddChild(object value);
    protected virtual void AddText(string text);
    protected virtual void ClearContainerForItemOverride(DependencyObject element, object item);
    protected virtual DependencyObject GetContainerForItemOverride();
    protected virtual bool IsItemItsOwnContainerOverride(object item);
    protected virtual void OnAlternationCountChanged(int oldAlternationCount, int newAlternationCount);
    protected virtual void OnDisplayMemberPathChanged(string oldDisplayMemberPath, string newDisplayMemberPath);
    protected virtual void OnGroupStyleSelectorChanged(GroupStyleSelector oldGroupStyleSelector, GroupStyleSelector newGroupStyleSelector);
    protected virtual void OnItemBindingGroupChanged(BindingGroup oldItemBindingGroup, BindingGroup newItemBindingGroup);
    protected virtual void OnItemContainerStyleChanged(Style oldItemContainerStyle, Style newItemContainerStyle);
    protected virtual void OnItemContainerStyleSelectorChanged(StyleSelector oldItemContainerStyleSelector, StyleSelector newItemContainerStyleSelector);
    protected virtual void OnItemsChanged(NotifyCollectionChangedEventArgs e);
    protected virtual void OnItemsPanelChanged(ItemsPanelTemplate oldItemsPanel, ItemsPanelTemplate newItemsPanel);
    protected virtual void OnItemsSourceChanged(IEnumerable oldValue, IEnumerable newValue);
    protected virtual void OnItemStringFormatChanged(string oldItemStringFormat, string newItemStringFormat);
    protected virtual void OnItemTemplateChanged(DataTemplate oldItemTemplate, DataTemplate newItemTemplate);
    protected virtual void OnItemTemplateSelectorChanged(DataTemplateSelector oldItemTemplateSelector, DataTemplateSelector newItemTemplateSelector);
    protected override void OnKeyDown(KeyEventArgs e);
    protected override void OnTextInput(TextCompositionEventArgs e);
    protected virtual void PrepareContainerForItemOverride(DependencyObject element, object item);
    protected virtual bool ShouldApplyItemContainerStyle(DependencyObject container, object item);
 
}
```

二、ItemsControl类分析

由于我们还没有讲模板、样式、数据绑定等内容，所以关于ItemsControl类的分析，我们先关注一些与模板样式和数据绑定无关的内容，先讲讲ItemsControl最基础的内容。

2.Items属性

ItemsControl类作为集合控件的基类，它提供了一个非常重要的属性，那就是Items属性。这个属性的类型是ItemCollection，也就是一个集合列表，那么这个列表的元素内容是什么呢？

![[assets/WPF从小白到大佬：集合控件：：ItemsControl基类/721200b49d20618805595cdd35a69a03_MD5.jpg]]

答案是object。

说明我们可以在集合控件中放任意引用类型的元素。

2.2DisplayMemberPath属性

这个属性用来获取或设置要显示的内容，它通常指某个数据源的某个属性名称，所以它是string类型。

2.3HasItems属性

这个属性用来判断当前集合控件是否有元素。

24.IsTextSearchCaseSensitive属性

这个属性如果为true，则搜索元素时区分大小写。

2.5 IsTextSearchEnabled属性

表示是否启用文字搜索。

好，接下来的几个属性将在后续进行学习，不过，我们先在这里了解一下它们的用途。

**2.6 ItemsPanel属性[重要]**

由于一个集合控件里面会显示多个数据项（一个数据代表一个家），那么这些数据项怎么排版？是像StackPanel一样水平或垂直排列，还是像WrapPanel瀑布流一样排例？这个ItemsPanel属性来决定。

**2.7 ItemTemplate属性[重要]**

在集合控件里，数据项有可能是一个复杂的实体，那么这些数据以什么样的UI布局界面呈现？也就是说，数据本身穿什么衣服？ItemTemplate属性就是来决定数据的外观的。如果把每个Item元素看成一个家，那么前面的ItemsPanel属性就是来决定邻里之间的实际距离以及房子和房子的排例走势。

**2.8 ItemContainerStyle属性[重要]**

ItemTemplate属性只能决定数据的外观，相当于这个家的内部装修以及家电家具的样式，而这个家外墙的装饰，则必须由ItemContainerStyle属性来承包。

**2.9 ItemContainerStyleSelector属性[重要]**

当我们选中这个集合控件中的某一项，并希望突出这一项，那就可以在ItemTemplateSelector属性中进行定义，也就是说，选择了某一项，某一项的外墙装饰发生改变。那同时要改变内部的样式呢？

**2.10 ItemTemplateSelector属性[重要]**

如果选中了某一项，并希望它的数据模块被重新定义，以突出这一项被选中，可以设置ItemTemplateSelector属性

**2.11 Template属性[重要]**

还记得吗？ItemsControl类继承于Control类，而Control类中有一个叫Template的属性，所以ItemsControl类自然也就拥有了这个属性，这是一个什么属性？它是ControlTemplate类，也就是控件模板，所以，如果我们希望把ItemsControl类本身的外观进行重定义，那就需要去设置Template属性

> 重庆教主友情提示
> 
> 上面我们一口气讲了这么多的模板概念，虽然看起来是在学习ItemsControl类的属性，但是别忘记了，将来要学习的那些集合子控件全都继承于ItemsControl类，意味着它们也都有这些模板属性可以使用呢，是不是有一种事半功倍的感觉呢！

接下来，我们来举个例子

三、ItemsControl示例

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
        <ItemsControl>
            <Button Content="content" Margin="0,5"/>
            <Border Height="30" Background="LightPink" Margin="0,5"/>
            <TextBlock Text="WPF中文网 www.wpfsoft.com" Background="LightGray" Margin="0,5"/>
            <ItemsControl Height="35" Background="AliceBlue"/>
            <CheckBox Content="CheckBox元素"/>
            <StackPanel Orientation="Horizontal" Margin="0,5">
                <RadioButton Content="初级"/>
                <RadioButton Content="中级"/>
                <RadioButton Content="高级"/>
            </StackPanel>
            这是一串字符串
            <Label Content="这是Label控件" Margin="0,5"/>
            <Control Background="Red" Height="30"/>
            <ProgressBar Value="50" Height="20" Maximum="100" />
        </ItemsControl>
    </Grid>
</Window>
```

![[assets/WPF从小白到大佬：集合控件：：ItemsControl基类/937087cc6777f92dd7209050b342356e_MD5.jpg]]

首先，我们在XMAL中实例化了一个ItemsControl控件，然后在ItemsControl里面实例化了一系列子控件，它们分别是Button、Border、TextBlock、ItemsControl、CheckBox、StackPanel、RadioButton、字符串、Label、Control和ProgressBar。

除了Control没有显示出来，其它全部都呈现在ItemsControl控件之中，因为这些子控件全部都在ItemsControl类的Items集合里面，那么，Control虽然能实例化，为什么没有显示出来呢？就连没有控件的字符串都能显示出来，这里面肯定有原因。

是的，这里我们引出一个知识点，那就是控件模板，因为Control基类虽然有Background属性，但是我们并没有给Control基类的Template属性设置一个控件模板，所以Control能实例化，但是不能显示。只能看到一个高度为30的空白区域。

而Border在设置Background属性后，为什么能显示？因为Border是一个装饰器，它继承于Decorator基类。

为什么单纯的字符串也能显示呢？因为实际上这个字符串外面被包裹了一层ContentPresenter实例，这个字符串是被赋值到了ContentPresenter的Content属性上，而ContentPresenter的ContentTemplate有一个默认模板。

四、总结

ItemsControl集合基类可以显示绝大多数控件，也就意味着，ListBox，ListView，DataGrid，ComboBox，TabControl，TreeView，Menu，ContextMenu，StatusBar这些子控件在显示集合元素时，每一个元素的外观可以呈现出更复杂、更漂亮的UI效果，从而可以设计出更友好的交互界面。

有了这样一个基调，那接下来我们来一一细说各个子控件的基础功能，待学习模板和样式章节后，进一步探索这些子控件的强大功能。另外，ListBox，ListView，DataGrid，ComboBox，TabControl这5个控件又都有一个共同的基类——Selector类，Selector继承于ItemsControl基类。Selector基类又是一个怎样的类？它会给我们提供哪些功能呢？

下一节，我们先从Selector基类说起。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：038-《ItemsControl集合控件基类》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月1日




