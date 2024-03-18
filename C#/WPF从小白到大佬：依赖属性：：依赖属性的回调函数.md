PropertyChangedCallback是一个委托，表示在依赖属性的有效属性值更改时调用的回调。也就是说，当我们修改了某个依赖属性的值后，还希望立即做一些事情，那就在注册（定义）一个依赖属性时，将一个方法名通过PropertyMetadata构造函数注入，一并注册到依赖属性系统当中。

一、什么是PropertyMetadata？

我们在定义一个依赖属性时，希望指明这个依赖属性的默认值，或者指明它的回调函数，这些信息都可以放到PropertyMetadata类中。

```cs
public class PropertyMetadata
{
public PropertyMetadata();
public PropertyMetadata(object defaultValue);
public PropertyMetadata(PropertyChangedCallback propertyChangedCallback);
public PropertyMetadata(object defaultValue, PropertyChangedCallback propertyChangedCallback);
public PropertyMetadata(object defaultValue, PropertyChangedCallback propertyChangedCallback, CoerceValueCallback coerceValueCallback);
 
public object DefaultValue { get; set; }
public PropertyChangedCallback PropertyChangedCallback { get; set; }
public CoerceValueCallback CoerceValueCallback { get; set; }
protected bool IsSealed { get; }
 
protected virtual void Merge(PropertyMetadata baseMetadata, DependencyProperty dp);
protected virtual void OnApply(DependencyProperty dp, Type targetType);
}
```

DefaultValue 属性：表示依赖属性的默认值。

PropertyChangedCallback 属性：一个回调委托对象。当依赖属性值发现改变时触发执行。

CoerceValueCallback 属性：一个回调委托对象。表示强制转换时执行的业务逻辑，它会先于PropertyChangedCallback 委托触发。

举例说明：

```cs
/// <summary>
/// 格子数量
/// </summary>
public int Count
{
    get { return (int)GetValue(CountProperty); }
    set { SetValue(CountProperty, value); }
}
 
public static readonly DependencyProperty CountProperty =
    DependencyProperty.Register("Count", typeof(int), typeof(TrayControl), 
        new PropertyMetadata(
            0,
            new PropertyChangedCallback(OnCountPropertyChangedCallback),
            new CoerceValueCallback(OnCountCoerceValueCallback)));
```

在上面的代码中，我们实例化了一个PropertyMetadata对象，并传入了3个参数，分别是0、PropertyChangedCallback和CoerceValueCallback。其中第一个参数0表示Count属性的默认值，当外界改变Count 值时，首先会触发OnCountCoerceValueCallback回调函数的执行，然后是OnCountPropertyChangedCallback回调函数的执行。

二、如何使用依赖属性的回调函数

接下来我们来举例说明依赖属性的定义、回调函数的定义和使用。

假定我们拥有一个自动分捡机器人，它会根据我们提前定义好的托盘格子以及每个格子的状态，自动去执行分捡动作。我们在定义托盘时，可以为每个格子定义一个X坐标和Y坐标，届时由伺服电机驱动机器人手臂去实现分捡。

我们在本例中只实现托盘的初始化，以及每个格子的尺寸初始化，状态的变化。这会用到自定义控件，并利用依赖属性以及回调函数去实现相关的业务逻辑。

第一步，定义格式的样式，我们在App.xaml的资料中定义一个Style。

```xml
<Style x:Key="CheckBoxDishStyle" TargetType="CheckBox">
    <Setter Property="Width" Value="60"/>
    <Setter Property="Height" Value="60"/>
    <Setter Property="Background" Value="White"/>
    <Setter Property="Margin" Value="2"/>
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="CheckBox">
                <Border Width="{TemplateBinding Width}" 
                                Height="{TemplateBinding Height}" 
                                CornerRadius="{Binding RelativeSource={RelativeSource Mode=Self},Path=Width}"
                                Background="{TemplateBinding Background}"
                                BorderBrush="#BCB33C" 
                                BorderThickness="2" >
                    <TextBlock Text="{TemplateBinding Name}" 
                               HorizontalAlignment="Center" 
                               VerticalAlignment="Center"/>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
    <Style.Triggers>
        <Trigger Property="IsChecked" Value="True">
            <Setter Property="Background" Value="#F38B76"/>
        </Trigger>
        <Trigger Property="IsMouseOver" Value="True">
            <Setter Property="Background" Value="#F38B76"/>
        </Trigger>
    </Style.Triggers>
</Style>
```

第二步，创建一个用户控件，取名为TrayControl。并在XAML中定义一个container容器， 用于显示初始化的格式。

```xml
<ScrollViewer>
    <WrapPanel x:Name="container"/>
</ScrollViewer>    
```

第三步，在TrayControl的C#后台语言中，定义如下几个依赖属性。

```cs
/// <summary>
/// 托盘
/// </summary>
public partial class TrayControl : UserControl
{
    public TrayControl()
    {
        InitializeComponent();            
    }
 
 
    /// <summary>
    /// 格子大小
    /// </summary>
    public int Size
    {
        get { return (int)GetValue(SizeProperty); }
        set { SetValue(SizeProperty, value); }
    }
 
    public static readonly DependencyProperty SizeProperty =
        DependencyProperty.Register("Size", typeof(int), typeof(TrayControl), 
            new PropertyMetadata(60,new PropertyChangedCallback(OnSizePropertyChangedCallback)));
 
    private static void OnSizePropertyChangedCallback(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        TrayControl control = d as TrayControl;
        control.Initlize();
    }
 
 
 
    /// <summary>
    /// 格子数量
    /// </summary>
    public int Count
    {
        get { return (int)GetValue(CountProperty); }
        set { SetValue(CountProperty, value); }
    }
 
    public static readonly DependencyProperty CountProperty =
        DependencyProperty.Register("Count", typeof(int), typeof(TrayControl), 
            new PropertyMetadata(
                0,
                new PropertyChangedCallback(OnCountPropertyChangedCallback),
                new CoerceValueCallback(OnCountCoerceValueCallback)));
 
    //这里演示当依赖属性值等于10，强制与10相乘，输出100
    private static object OnCountCoerceValueCallback(DependencyObject d, object baseValue)
    {
        int count = (int)baseValue;
        if (count == 10)
        {
            return count * 10;
 
        }
        return baseValue;
    }
 
    private static void OnCountPropertyChangedCallback(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        TrayControl control = d as TrayControl;
        control.Initlize();
        
    }
 
    private void Initlize()
    {
        SelectedCount = 0;
        container.Children.Clear();
        SelectedItems.Clear();
 
        if (Count > 0)
        {                
            for (int i = 0; i < Count; i++)
            {
                CheckBox checkBox = new CheckBox();
                checkBox.Style = Application.Current.Resources["CheckBoxDishStyle"] as Style;
                checkBox.Width = Size;
                checkBox.Height = Size;
                checkBox.Tag = new Point(i * 10, Size + i * 2);
                checkBox.Name = "_"+i.ToString();
                checkBox.Checked += (sender, args) =>
                {
                    SelectedCount++;
                    SelectedItems.Add(checkBox);
                };
                checkBox.Unchecked += (sender, args) =>
                {
                    SelectedCount--;
                    SelectedItems.Remove(checkBox);
 
                };
                container.Children.Add(checkBox);
            }
        }
    }
 
 
    public int SelectedCount
    {
        get { return (int)GetValue(SelectedCountProperty); }
        set { SetValue(SelectedCountProperty, value); }
    }
 
    public static readonly DependencyProperty SelectedCountProperty =
        DependencyProperty.Register("SelectedCount", typeof(int), typeof(TrayControl),
            new PropertyMetadata(0));
 
 
 
    public List<CheckBox> SelectedItems
    {
        get { return (List<CheckBox>)GetValue(SelectedItemsProperty); }
        set { SetValue(SelectedItemsProperty, value); }
    }
 
    public static readonly DependencyProperty SelectedItemsProperty =
        DependencyProperty.Register("SelectedItems", typeof(List<CheckBox>), typeof(TrayControl), 
            new PropertyMetadata(new List<CheckBox>()));
 
 
}
 
```

在上面的代码中，我们定义了Count属性表示格式的数量，Size属性表示格式的尺寸，SelectedCount属性表示非空格子的数量，SelectedItems属性表示非空格式的集合。

在Count属性和Size属性发生改变时，会去执行各自的回调函数，在回调函数中调用了Initlize方法成员，该 方法成员的功能是根据Count数量和Size尺寸实例化一些CheckBox（表示格子）对象，同时生成了一些演示坐标放到CheckBox的Tag属性中，方便将来的驱动器使用，最后将其放到container容器中显示出来。

关于CheckBox，我们在它的Style样式中，利用IsChecked属性呈现两种效果，即空白状态和填充状态。

待这个用户控件做好了，我们就可以在主窗体中使用它了。

第四步，使用控件

```xml
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld" xmlns:controls="clr-namespace:HelloWorld.Controls"
        mc:Ignorable="d" Background="LightGray"
        Title="WPF中文网 - www.wpfsoft.com" Height="350" Width="500">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition Height="45"/>
        </Grid.RowDefinitions>
        <controls:TrayControl x:Name="tray" 
                              Margin="5" Background="White"
                              Size="{Binding ElementName=sliderSize,Path=Value}"
                              Count="{Binding ElementName=sliderCount,Path=Value}"/>
        <StackPanel Grid.Row="1" Orientation="Horizontal">
            <StackPanel >
                <StackPanel Orientation="Horizontal">
                    <TextBlock Text="托盘尺寸" Margin="3" VerticalAlignment="Center"/>
                    <Slider x:Name="sliderSize"  Width="200" Value="30" 
                            Maximum="100" VerticalAlignment="Center"/>
                </StackPanel>
                <Rectangle Height="5"/>
                <StackPanel Orientation="Horizontal">
                    <TextBlock Text="托盘数量" Margin="3" VerticalAlignment="Center"/>
                    <Slider x:Name="sliderCount"  Width="200" Value="0" 
                            Maximum="28" VerticalAlignment="Center"/>
                </StackPanel>
            </StackPanel>
            <TextBlock Text="当前装配数量：" Margin="3" VerticalAlignment="Center">
                <Run Text="{Binding ElementName=tray,Path=SelectedCount}"/>
                <Run Text="总数量："/>
                <Run Text="{Binding ElementName=tray,Path=Count}"/>
            </TextBlock>
            <Button Content="提交" Width="50" Height="25" Click="Button_Click"/>
        </StackPanel>
        
    </Grid>
    
</Window>
```

![[assets/WPF从小白到大佬：依赖属性：：依赖属性的回调函数/659734de875172680ee6aff9d19058d3_MD5.jpg]]

在主窗体中，我们实例化了TrayControl自定义控件，同时，将它的Count和Size分别绑定到Slider滑动务的Value上，方便我们初始化托盘。然后将它的SelectedCount绑定到TextBlock控件显示，最后在提交按钮中，获取当前非空格子的坐标，这样就可以将这些坐标交给运动设备，以此实现硬件的相关功能。

最后，在提交按钮中，我们只需要获取TrayControl的SelectedItems属性，便可以获取所有非空格子的运动坐标了。

```cs
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
 
       
    }
 
    private void Button_Click(object sender, RoutedEventArgs e)
    {
        foreach (var item in tray.SelectedItems)
        {
            MessageBox.Show($"{item.Name.ToString()} 移动坐标 = ({item.Tag.ToString()})");
        }
    }
}
```

![[assets/WPF从小白到大佬：依赖属性：：依赖属性的回调函数/39bcd12f86f670ff746120320c9bc226_MD5.jpg]]

总结：在这个示例中，由于在TrayControl自定义控件中的前端UI控件并不需要绑定后台的依赖属性，所以，我们并没有将当前类型交给当前类型的DataContext属性中，但是TrayControl被实例化后，它所定义的4个依赖属性于主窗体中的其它控件是可以建立绑定关系的，因为它们都是依赖属性。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：084-《PropertyChangedCallback》-源代码.rar  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年10月26日