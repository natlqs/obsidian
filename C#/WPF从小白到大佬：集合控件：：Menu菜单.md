Menu控件继承于MenuBase，而MenuBase继承于ItemsControl。所以学习Menu之前，要先了解一下MenuBase基类。它是一个抽象类，拥有一个ItemContainerTemplateSelector模板选择器，并重写了一些关于键盘和鼠标的方法。

Menu的子项必须为MenuItem。这个MenuItem和前面的TreeViewItem类似，拥有共同的HeaderedItemsControl父类，也就是说，MenuItem本身也是一个集合控件，若要以代码形式加载Menu的内容，也必须要掌握递归的加载思路。

在本节中，我们将以两种方式加载Menu的数据。但是在学习之前，先熟悉一下MenuItem元素，因为，实际上，我们主要是操作MenuItem元素。

一、MenuItem元素

```cs
public class MenuItem : HeaderedItemsControl, ICommandSource
{
    public static readonly RoutedEvent ClickEvent;
    public static readonly DependencyProperty UsesItemContainerTemplateProperty;
    public static readonly DependencyProperty ItemContainerTemplateSelectorProperty;
    public static readonly DependencyProperty IsSuspendingPopupAnimationProperty;
    public static readonly DependencyProperty IconProperty;
    public static readonly DependencyProperty InputGestureTextProperty;
    public static readonly DependencyProperty StaysOpenOnClickProperty;
    public static readonly DependencyProperty IsCheckedProperty;
    public static readonly DependencyProperty IsHighlightedProperty;
    public static readonly DependencyProperty IsCheckableProperty;
    public static readonly DependencyProperty IsPressedProperty;
    public static readonly DependencyProperty IsSubmenuOpenProperty;
    public static readonly DependencyProperty CommandTargetProperty;
    public static readonly DependencyProperty CommandParameterProperty;
    public static readonly DependencyProperty CommandProperty;
    public static readonly RoutedEvent SubmenuClosedEvent;
    public static readonly RoutedEvent SubmenuOpenedEvent;
    public static readonly RoutedEvent UncheckedEvent;
    public static readonly RoutedEvent CheckedEvent;
    public static readonly DependencyProperty RoleProperty;
 
    public MenuItem();
 
    public static ResourceKey SubmenuHeaderTemplateKey { get; }
    public static ResourceKey SubmenuItemTemplateKey { get; }
    public static ResourceKey SeparatorStyleKey { get; }
    public static ResourceKey TopLevelItemTemplateKey { get; }
    public static ResourceKey TopLevelHeaderTemplateKey { get; }
    public bool IsCheckable { get; set; }
    public object CommandParameter { get; set; }
    public IInputElement CommandTarget { get; set; }
    public bool IsSubmenuOpen { get; set; }
    public MenuItemRole Role { get; }
    public bool IsPressed { get; protected set; }
    public bool IsHighlighted { get; protected set; }
    public bool StaysOpenOnClick { get; set; }
    public string InputGestureText { get; set; }
    public object Icon { get; set; }
    public bool IsSuspendingPopupAnimation { get; }
    public ItemContainerTemplateSelector ItemContainerTemplateSelector { get; set; }
    public bool UsesItemContainerTemplate { get; set; }
    public bool IsChecked { get; set; }
    public ICommand Command { get; set; }
    protected override bool IsEnabledCore { get; }
    protected internal override bool HandlesScrolling { get; }
 
    public event RoutedEventHandler Unchecked;
    public event RoutedEventHandler Click;
    public event RoutedEventHandler Checked;
    public event RoutedEventHandler SubmenuClosed;
    public event RoutedEventHandler SubmenuOpened;
 
    public override void OnApplyTemplate();
    protected override DependencyObject GetContainerForItemOverride();
    protected override bool IsItemItsOwnContainerOverride(object item);
    protected override void OnAccessKey(AccessKeyEventArgs e);
    protected virtual void OnChecked(RoutedEventArgs e);
    protected virtual void OnClick();
    protected override AutomationPeer OnCreateAutomationPeer();
    protected override void OnGotKeyboardFocus(KeyboardFocusChangedEventArgs e);
    protected override void OnInitialized(EventArgs e);
    protected override void OnIsKeyboardFocusWithinChanged(DependencyPropertyChangedEventArgs e);
    protected override void OnItemsChanged(NotifyCollectionChangedEventArgs e);
    protected override void OnKeyDown(KeyEventArgs e);
    protected override void OnMouseEnter(MouseEventArgs e);
    protected override void OnMouseLeave(MouseEventArgs e);
    protected override void OnMouseLeftButtonDown(MouseButtonEventArgs e);
    protected override void OnMouseLeftButtonUp(MouseButtonEventArgs e);
    protected override void OnMouseMove(MouseEventArgs e);
    protected override void OnMouseRightButtonDown(MouseButtonEventArgs e);
    protected override void OnMouseRightButtonUp(MouseButtonEventArgs e);
    protected virtual void OnSubmenuClosed(RoutedEventArgs e);
    protected virtual void OnSubmenuOpened(RoutedEventArgs e);
    protected virtual void OnUnchecked(RoutedEventArgs e);
    protected override void PrepareContainerForItemOverride(DependencyObject element, object item);
    protected override bool ShouldApplyItemContainerStyle(DependencyObject container, object item);
    protected internal override void OnVisualParentChanged(DependencyObject oldParent);
 
}
```

MenuItem从鼠标的交互上，提供了两种方式。第一种是提供了Click事件，开发者可以订阅该事件以编写相应的业务逻辑。第二种是提供了ICommand接口属性和CommandParameter命令参数，以WPF命令的形式开发业务逻辑。

下面，我们以一种交互方式为例。

二、Menu示例

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
            <RowDefinition Height="auto"/>
            <RowDefinition/>
        </Grid.RowDefinitions>        
        <Menu x:Name="_Menu">
            <MenuItem Header="文件">
                <MenuItem Header="新建" Click="MenuItem_Click"/>
                <MenuItem Header="打开" Click="MenuItem_Click">
                    <MenuItem.Icon>
                        <Image Source="/Images/logo.png"/>
                    </MenuItem.Icon>
                </MenuItem>
            </MenuItem>                      
            <MenuItem Header="编辑"/>
            <MenuItem Header="视图"/>
            <MenuItem Header="项目"/>
            <MenuItem Header="调试"/>
            <MenuItem Header="测试"/>
            <MenuItem Header="分析"/>
            <MenuItem Header="工具"/>
            <MenuItem Header="帮助"/>
        </Menu>
        <TextBlock x:Name="_TextBlock" Grid.Row="1" HorizontalAlignment="Center" VerticalAlignment="Center"></TextBlock>
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
 
    private void MenuItem_Click(object sender, RoutedEventArgs e)
    {
        var item = sender as MenuItem;
        _TextBlock.Text = $"你单击了 {item.Header}";
    }
}
```

![[assets/WPF从小白到大佬：集合控件：：Menu菜单/afbb898d58984383c4037d5b3a7f404d_MD5.jpg]]

上面演示了Menu最基本的用法，如果希望采用数据绑定的方式加载菜单，则可以参考下面的作法。

三、Menu数据绑定

我们需要创建一个实体类，来代表Menu的每一个子项。

```cs
/// <summary>
/// 主菜单的实体
/// </summary>
public class MenuModel
{
    public string Name { get; set; }
    public List<MenuModel> Children { get; set; } = new List<MenuModel>();
    public string View { get; set; }
}
```

在前端代码中，需要设置Menu的ItemTemplate元素模板。

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
            <RowDefinition Height="auto"/>
            <RowDefinition/>
        </Grid.RowDefinitions>        
        <Menu x:Name="_Menu">
            <Menu.ItemTemplate>
                <HierarchicalDataTemplate ItemsSource="{Binding Children}">
                    <TextBlock Text="{Binding Name}"/>
                </HierarchicalDataTemplate>
            </Menu.ItemTemplate>
        </Menu>
    </Grid>
</Window>
```

因为MenuModel实体中有Children集合，所以在前端将Children作为HierarchicalDataTemplate的ItemsSource。并将Name显示出来。

最后，实例化一些子项数据，形成一个数据源，将这个数据源绑定到Menu的ItemsSource即可

来看看后端代码

```cs
public partial class MainWindow : Window
{
    public List<MenuModel> Menus { get; set; } = new List<MenuModel>();
 
    public MainWindow()
    {
        InitializeComponent();            
         
        for (int i = 0; i < 5; i++)
        {
            MenuModel parent = new MenuModel();
            parent.Name = $"一级菜单 ";
            for (int j = 0; j < 5; j++)
            {
                MenuModel child = new MenuModel();
                child.Name = $"二级菜单 ";
                parent.Children.Add(child);
            }
            Menus.Add(parent);
        }
 
        _Menu.ItemsSource = Menus;
    }
    
}
```

![[assets/WPF从小白到大佬：集合控件：：Menu菜单/744ce2970f225d34f111038a543a92e9_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：045-《Menu菜单控件》-源代码 -1，045-《Menu菜单控件》-源代码 -2  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月8日
