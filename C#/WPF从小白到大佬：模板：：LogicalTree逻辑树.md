WPF使用了若干树结构形式来定义程序元素之间的关系，比如逻辑树和可视化树。要介绍逻辑树和可视化树，我们要先了解两个基类，它们分别是FrameworkElement类和Visual类，FrameworkElement类主要实现控件的布局、逻辑树、支持数据绑定和动态资源引用、控件的样式定义和动画，而Visual类更关注控件的命中测试、坐标转换和边界框计算，即控件呈现更基础的服务支持。所以，FrameworkElement类与WPF的控件“更靠近一些”，比如设置控件的宽度、高度、透明度、显示或隐藏控件，像这些由控件组成的XAML代码就是一棵逻辑树。

那么，逻辑树有什么用途呢？

> 官方解释
> 
> 借助逻辑树，内容模型可以方便地循环访问其可能的子对象，从而实现扩展。 此外，逻辑树还为某些通知提供框架，例如在加载逻辑树中的所有对象时。 基本上，逻辑树是框架级别的近似运行时对象图（排除了视觉对象），但其足以用于对你自己的运行时应用程序组合执行多种查询操作。  
> 此外，静态和动态资源引用具有相同的解析过程：针对最初发出请求的对象，沿逻辑树向上查找 Resources 集合，然后沿逻辑树继续向上，检查每一个 FrameworkElement 或 FrameworkContentElement，以查找另一个包含 ResourceDictionary（可能包含该键）的 Resources 值。

有时候，我们需要在后端代码中查找前端XAML某个控件，以便对控件进行某个操作，那么，就可以借助逻辑树来遍历元素。LogicalTreeHelper 类就是专门来遍历查找WPF的逻辑树的。LogicalTreeHelper 类提供用于逻辑树遍历的 GetChildren、GetParent 和 FindLogicalNode 方法。

下面，我们一个例子来说明LogicalTreeHelper 类的使用过程。

前端代码

```xml
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld" 
        xmlns:forms="clr-namespace:System.Windows.Forms;assembly=System.Windows.Forms"
        mc:Ignorable="d" FontSize="14"
        Title="WPF中文网 - 模板 - www.wpfsoft.com" Height="350" Width="500">    
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="auto"/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <Border Grid.Column="0" x:Name="_LeftBorder" Width="188" Background="LightCyan">
            <Button Content="当前逻辑树" Click="Button_Click" HorizontalAlignment="Center" VerticalAlignment="Center"/>
        </Border>
        <Border Grid.Column="1" x:Name="_RightBorder" >
            <TreeView x:Name="_TreeView" Margin="5"/>
        </Border>
    </Grid>
    
</Window>
```

在上面的前端代码中，根节点是window，在window里面有一个grid控件，在grid控件中有两个ColumnDefinition对象和两个Border 对象，在两个Border 对象中，分别有一个button对象 和TreeView对象。

接下来，我们通过LogicalTreeHelper 类来遍历Window窗体的XAML代码形成的逻辑树，看看结果如何。

后端代码

```cs
/// <summary>
/// MainWindow.xaml 的交互逻辑
/// </summary>
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
 
        this.DataContext = new MainViewModel();           
 
    }
 
    private void Button_Click(object sender, RoutedEventArgs e)
    {
        TreeViewItem item = new TreeViewItem() { Header = "逻辑树根" };
 
        LogicalTree(item, this);
 
        _TreeView.Items.Add(item);
    }
 
    private void LogicalTree(TreeViewItem item, object element)
    {
        if (!(element is DependencyObject)) return;
 
        TreeViewItem treeViewItem = new TreeViewItem { Header = element.GetType().Name };
 
        item.Items.Add(treeViewItem);
 
        var elements = LogicalTreeHelper.GetChildren(element as DependencyObject);
 
        foreach (object child in elements)
        {
            LogicalTree(treeViewItem, child);
        }
            
    }
}
```

在后端代码中，我们利用LogicalTreeHelper.GetChildren方法一层一层地获取child对象，通过递归函数将这个树加载到TreeView当中。

![[assets/WPF从小白到大佬：模板：：LogicalTree逻辑树/edf82427c214913a975e2567c948d280_MD5.jpg]]

最后，MainWindow的逻辑树就被我们全部显示出来，它与XAML代码保持一致，罗列了前端代码中控件形成的树的样式。

实际上，每个控件因为有Template模板——即每个控件内部也有其构成的元素，换句话说，单看每个控件的组成，其实也是一棵树，只不过，逻辑树并没有将它们显示出来。如果想要在树上不单看到控件组成的树结构，还要看到控件的内部组成结构，那就得用可视化树（VisualTree）。

下一节，我们来介绍VisualTree。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：061-《LogicalTree逻辑树》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月21日



