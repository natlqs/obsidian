WPF 中除了逻辑树的概念，还存在可视化树的概念。 可视化树描述由 Visual 基类表示的可视化对象的结构。 为控件编写模板时，将定义或重新定义适用于该控件的可视化树。 对于出于性能和优化考虑需要对绘图进行较低级别控制的开发人员来说，他们也会对可视化树感兴趣。 在传统 WPF 应用程序编程中，可视化树的一个应用是：路由事件的事件路由大多遍历可视化树而非逻辑树。 通过可视化树对事件进行路由可使控件在可视化级别实现组合以处理事件或创建事件资源库。

遍历可视化树需要用到WPF提供的VisualTreeHelper 类，而VisualTreeHelper 类提供 GetChild、GetParent和 GetChildrenCount方法。下面，我们以上一章节的例子来遍历Window窗体的可视化树内容。

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
            <Button Content="当前可视化树" Click="Button_Click" HorizontalAlignment="Center" VerticalAlignment="Center"/>
        </Border>
        <Border Grid.Column="1" x:Name="_RightBorder" >
            <TreeView x:Name="_TreeView" Margin="5"/>
        </Border>
    </Grid>
    
</Window>
```

我们没有修改XAML代码的结构，以便和逻辑树进行对比。

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
         TreeViewItem item = new TreeViewItem() { Header = "可视化树根" };
 
         VisualTree(item, this);
 
         _TreeView.Items.Add(item);
     }
 
   private void VisualTree(TreeViewItem item, object element)
   {
     if (!(element is DependencyObject)) return;
 
     TreeViewItem treeViewItem = new TreeViewItem { Header = element.GetType().Name };
 
     item.Items.Add(treeViewItem); 
 
     for (int i = 0; i < VisualTreeHelper.GetChildrenCount(element as DependencyObject); i++)
     {
         VisualTree(treeViewItem,VisualTreeHelper.GetChild(element as DependencyObject, i));
     }
   }
 
 }
```

![[assets/WPF从小白到大佬：模板：：VisualTree可视化树/d72bcdd5b5a4d8400328559c9708862e_MD5.jpg]]

从运行的结果来看，我们会发现它的内容比逻辑树更丰富一些。请注意观察红色框中的内容，它就是前端XAML代码中的控件，而绿色框中的内容是哪来的？其实，这些内容是控件的模板代码——即控件的内部组成。比如从上到下数第一个绿色框中的内容，它就是Window窗体的模板结构，第二个绿框代码则是Button按钮的模板结构，最后一个绿框代码，则是TreeView控件的模板结构。

总结：可视化树 > 逻辑树

理解WPF的逻辑树与可视化树，对于学习WPF的模板、路由事件是十分有帮助的。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：062-《VisualTree可视化树》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff