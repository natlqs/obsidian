Application类、FrameworkElement基类和FrameworkContentElement基类都有一个叫Resources的属性，其类型就是ResourceDictionary，我们称为资源字典。

一、ResourceDictionary资源字典的定义

```cs
public class ResourceDictionary : IDictionary, ICollection, IEnumerable, ISupportInitialize, IUriContext, INameScope
{
    public ResourceDictionary();
 
    public object this[object key] { get; set; }
 
    public ICollection Keys { get; }
    public DeferrableContent DeferrableContent { get; set; }
    public bool InvalidatesImplicitDataTemplateResources { get; set; }
    public bool IsReadOnly { get; }
    public bool IsFixedSize { get; }
    public Uri Source { get; set; }
    public Collection<ResourceDictionary> MergedDictionaries { get; }
    public ICollection Values { get; }
    public int Count { get; }
 
    public void Add(object key, object value);
    public void BeginInit();
    public void Clear();
    public bool Contains(object key);
    public void CopyTo(DictionaryEntry[] array, int arrayIndex);
    public void EndInit();
    public object FindName(string name);
    public IDictionaryEnumerator GetEnumerator();
    public void RegisterName(string name, object scopedElement);
    public void Remove(object key);
    public void UnregisterName(string name);
    protected virtual void OnGettingValue(object key, ref object value, out bool canCache);
 
}
```

从ResourceDictionary的定义上看，它内部拥有两个集合，分别是Keys和Values，并且它还拥有一个迭代器，可以根据key名称快速地访问某个Values集合中的元素；最后就是这个迭代器的返回值是object类型，说明一个问题：**资源字典的集合中的元素可以是任意类型的实例**。这句话非常重要，我们接下来就演示它的用途。

此外，资源字典还有一个很重要的属性——MergedDictionaries。字面意思可以理解成合并的资源字典集合。也就是说，Applicaton类的Resources属性本身就可以定义许多的资源，而这些资源可以是某个Style样式，也可以是某个画笔，或某个转换器，还可以是ResourceDictionary，但是要把ResourceDictionary放到MergedDictionaries集合中。

最后一个比较重要的属性是Source。它表示当前资源字典的数据源——也就是要加载资源的 统一资源标识符 (URI)。

接下来，我们以四种方式定义Style，并演示ResourceDictionary的一些用法。

二、资源字典与样式的用法

2.1创建一个资源字典文件

![[assets/WPF从小白到大佬：样式：：ResourceDictionary资源字典/8dc4a023791fbe552dab503994f4460f_MD5.jpg]]

在项目中新建一个Style文件夹，右键-添加-资源字典文件。创建一个Button.xaml的资源文件，并在其中写下内容。

```cs
<ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Style x:Key="BlueButtonStyle" TargetType="Button">
        <Setter Property="Width" Value="100"/>
        <Setter Property="Height" Value="30"/>
        <Setter Property="Background" Value="Blue"/>
        <Setter Property="Foreground" Value="White"/>
        <Setter Property="Margin" Value="3"/>
    </Style>
</ResourceDictionary>
```

注意，资源文件都是以ResourceDictionary实例开头的对象，然后我们在其中编写了一个Style样式。

回到项目的App.xaml文件中，编写如下内容

```cs
<Application x:Class="HelloWorld.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:HelloWorld"
             StartupUri="MainWindow.xaml">
    <Application.Resources>
        <ResourceDictionary>
            
            <SolidColorBrush x:Key="ButtonBackground" Color="Red"/>
            <SolidColorBrush x:Key="ButtonForeground" Color="White"/>
            
            <Style x:Key="ButtonStyle" TargetType="Button">
                <Setter Property="Width" Value="100"/>
                <Setter Property="Height" Value="30"/>
                <Setter Property="Background" Value="{StaticResource ButtonBackground}"/>
                <Setter Property="Foreground" Value="{StaticResource ButtonForeground}"/>
                <Setter Property="Margin" Value="3"/>
            </Style>
            
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Style/Button.xaml"/>
            </ResourceDictionary.MergedDictionaries>
            
        </ResourceDictionary>          
    </Application.Resources>
</Application
```

我们在Application的Resources属性中实例化了一个ResourceDictionary对象，并在其中定义了两个SolidColorBrush对象，一个style样式，以及在MergedDictionaries集合中添加了一个ResourceDictionary对象，其数据源指向了Button.xaml资源文件。

最后，我们来到主窗体的前端代码：

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
    <Window.Resources>
        <Style x:Key="GreenButtonStyle" TargetType="Button">
            <Setter Property="Width" Value="100"/>
            <Setter Property="Height" Value="30"/>
            <Setter Property="Background" Value="Green"/>
            <Setter Property="Foreground" Value="White"/>
            <Setter Property="Margin" Value="3"/>
        </Style>
    </Window.Resources>
    <StackPanel VerticalAlignment="Center">
        <Button Content="红色按钮" Style="{StaticResource ButtonStyle}"/>
        <Button Content="蓝色按钮" Style="{StaticResource BlueButtonStyle}"/>
        <Button Content="绿色按钮" Style="{StaticResource GreenButtonStyle}"/>
        <Button Content="橙色按钮">
            <Button.Style>
                <Style TargetType="Button">
                    <Setter Property="Width" Value="100"/>
                    <Setter Property="Height" Value="30"/>
                    <Setter Property="Background" Value="Orange"/>
                    <Setter Property="Foreground" Value="White"/>
                    <Setter Property="Margin" Value="3"/>
                </Style>
            </Button.Style>
        </Button> 
    </StackPanel>
</Window>
```

在上面的代码中，我们在Window的Resources定义了一个样式，同时在Button中也定义了一个样式，如此，我们就演示了4种定义style的方法。

最后来看一下最终的呈现效果

![[assets/WPF从小白到大佬：样式：：ResourceDictionary资源字典/cf40b0913b2a23f9ef47bbda42f4776d_MD5.jpg]]

总结：Resources属性的值只能是一个ResourceDictionary对象，一个ResourceDictionary对象中可以定义多个资源。如果有多个ResourceDictionary对象，则必须使用MergedDictionaries属性。任意类型都可以在Resources中被实例化，但是必须在实例化时指明一个key，因为在xaml中要引入定义好的资源，都是以key进行查找的。

上面的示例中，我们演示了style样式的Setters集合，接下来，我们来演示一下style样式的Triggers集合。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：049-《ResourceDictionary资源字典》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月11日