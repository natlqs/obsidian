TreeView其实是一个比较复杂的控件，像操作系统的资源管理器就是一个TreeView。所以它常用于显示文件夹、目录等具有层级结构的数据。TreeView由节点和分支构成，每个节点可以包含零个或多个子节点，分支表示父子关系。在TreeView中，每个节点表示为TreeViewItem对象，可以通过TreeView的Items属性来获取或设置TreeViewItem对象集合。

在使用TreeView加载节点时，需要掌握一些递归思想。

一、TreeViewItem元素简介

TreeViewItem作为TreeView唯一的元素类型，它继承于HeaderedItemsControl（带标题），而HeaderedItemsControl又继承于ItemsControl，由此可见，TreeViewItem元素本身也是一个集合控件。

TreeViewItem有两个常用的属性，分别是IsSelected属性和IsExpanded属性，IsSelected表示当前元素是否选中，IsExpanded表示当前元素是否展开。

二、TreeView类的定义

```cs
public class TreeView : ItemsControl
{
    public static readonly DependencyProperty SelectedItemProperty;
    public static readonly DependencyProperty SelectedValueProperty;
    public static readonly DependencyProperty SelectedValuePathProperty;
    public static readonly RoutedEvent SelectedItemChangedEvent;
 
    public TreeView();
 
    public string SelectedValuePath { get; set; }
    public object SelectedValue { get; }
    public object SelectedItem { get; }
    protected internal override bool HandlesScrolling { get; }
 
    public event RoutedPropertyChangedEventHandler<object> SelectedItemChanged;
 
    protected virtual bool ExpandSubtree(TreeViewItem container);
    protected override DependencyObject GetContainerForItemOverride();
    protected override bool IsItemItsOwnContainerOverride(object item);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected override void OnGotFocus(RoutedEventArgs e);
    protected override void OnIsKeyboardFocusWithinChanged(DependencyPropertyChangedEventArgs e);
    protected override void OnItemsChanged(NotifyCollectionChangedEventArgs e);
    protected override void OnKeyDown(KeyEventArgs e);
    protected virtual void OnSelectedItemChanged(RoutedPropertyChangedEventArgs<object> e);
 
}
```

SelectedValuePath属性：获取或设置SelectedItem或SelectedValue的路径。

SelectedValue属性：获取SelectedItem的值

SelectedItem属性：获取当前选中的项

三、TreeView示例

接下来，我们以一个简单的示例，模仿操作系统的资源管理器的目录加载。

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
        <StackPanel Orientation="Horizontal">
            <TextBlock Text="根目录" VerticalAlignment="Center" Margin="3"/>
            <TextBox x:Name="_TextBox" Width="380" Height="25" Margin="3"/>
            <Button Content="选择..." MinWidth="45" Margin="3" Click="Button_Click"/>
        </StackPanel>
        <TreeView x:Name="_TreeView" Grid.Row="1" SelectedItemChanged="_TreeView_SelectedItemChanged"/>
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
 
    private void Button_Click(object sender, RoutedEventArgs e)
    {
        System.Windows.Forms.FolderBrowserDialog dialog = new System.Windows.Forms.FolderBrowserDialog();
        if (dialog.ShowDialog() == System.Windows.Forms.DialogResult.OK)
        {
            _TextBox.Text = dialog.SelectedPath;
            LoadTreeView(dialog.SelectedPath);
 
        }
    }
 
    private void LoadTreeView(string rootPath)
    {
        // 设置根节点
        TreeViewItem rootNode = new TreeViewItem();
        rootNode.Header = "根目录";
 
        // 加载子文件夹和文件
        LoadSubDirectory(rootNode, rootPath);
 
        // 将根节点添加到TreeView中
        _TreeView.Items.Add(rootNode);
    }
 
    private void LoadSubDirectory(TreeViewItem node, string path)
    {
        try
        {
            DirectoryInfo dirInfo = new DirectoryInfo(path);
 
            // 加载子文件夹
            foreach (DirectoryInfo subDirInfo in dirInfo.GetDirectories())
            {
                TreeViewItem subNode = new TreeViewItem();
                subNode.Header = subDirInfo.Name;
 
                LoadSubDirectory(subNode, subDirInfo.FullName);
 
                node.Items.Add(subNode);
            }
 
            // 加载文件
            foreach (FileInfo fileInfo in dirInfo.GetFiles())
            {
                TreeViewItem subNode = new TreeViewItem();
                subNode.Header = fileInfo.Name;
 
                node.Items.Add(subNode);
            }
        }
        catch (Exception ex)
        {
            MessageBox.Show(ex.Message);
        }
    }
 
    private void _TreeView_SelectedItemChanged(object sender, RoutedPropertyChangedEventArgs<object> e)
    {
        // 获取选中的节点
        TreeViewItem selectedNode = _TreeView.SelectedItem as TreeViewItem;
 
        // 显示选中节点的Header
        if (selectedNode != null)
        {
            MessageBox.Show(selectedNode.Header.ToString());
        }
    }
}
```

![[assets/WPF从小白到大佬：集合控件：：TreeView树控件/707ced1c8affc054c2f74ad80131d6c4_MD5.jpg]]

首先，通过鼠标操作，选择TreeView的根目录，然后，利用DirectoryInfo获取当前所有目录，再利用递归调用，一层一层的获取所有子目录，最后以TreeViewItem元素一层层加载到控件中。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：044-《TreeView树控件》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月8日


