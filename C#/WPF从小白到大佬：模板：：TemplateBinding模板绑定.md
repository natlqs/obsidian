TemplateBinding和Binding在使用上类似，但是从它的定义上看，它的Property属性是要求传入一个DependencyProperty依赖属性。

```cs
public class TemplateBindingExtension : MarkupExtension
{
    public TemplateBindingExtension();
    public TemplateBindingExtension(DependencyProperty property);
 
    public DependencyProperty Property { get; set; }
    public IValueConverter Converter { get; set; }
    public object ConverterParameter { get; set; }
 
    public override object ProvideValue(IServiceProvider serviceProvider);
 
}
```

控件的大多数属性都是DependencyProperty类型，所以我们就可以在定制控件的可视化树（控件模板）时，将控件的属性通过TemplateBinding绑定到模板里面的元素的属性上。

我们将前面的例子做一些调整。首先定义一个Person实体类和MainViewModel

```cs
public class ObservableObject : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;
 
    public void RaisePropertyChanged([CallerMemberName] string propertyName = "")
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
public class Person : ObservableObject
{
    private string name; 
    public string Name
    {
        get { return name; }
        set { name = value;RaisePropertyChanged(); }
    }
 
    private string occupation;
    public string Occupation
    {
        get { return occupation; }
        set { occupation = value; RaisePropertyChanged(); }
    }
 
    private int age;
    public int Age
    {
        get { return age; }
        set { age = value; RaisePropertyChanged(); }
    }
 
    private int money;
    public int Money
    {
        get { return money; }
        set { money = value; RaisePropertyChanged(); }
    }
 
    private string address;
    public string Address
    {
        get { return address; }
        set { address = value; RaisePropertyChanged(); }
    }
}
```

MainViewModel

```cs
public class MainViewModel : ObservableObject
{
    private Person person;
    public Person Person
    {
        get { return person; }
        set { person = value; RaisePropertyChanged(); }
    }
    public MainViewModel()
    {
        person = new Person()
        {
            Name = "Michael Jackson",
            Occupation = "Musicians",
            Age = 25,
            Money = 9999999,
            Address = "深圳市光明区智慧招商城B4栋5楼"
        };
    }
}
 
```

然后，我们在Window的Resource中定义一个Button的样式

```xml
<Style x:Key="CardButtonStyle" TargetType="Button">
    <Setter Property="Background" Value="#E7EAF4"/>
    <Setter Property="Foreground" Value="#20232E"/>
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="Button">
                <Border x:Name="border" 
                        Width="{TemplateBinding Width}"
                        Height="{TemplateBinding Height}"
                        Background="{TemplateBinding Background}"
                        BorderThickness="1" 
                        BorderBrush="Gray">
                    <Border.ToolTip>
                        <ContentPresenter/>
                    </Border.ToolTip>
                    <Grid>
                        <Grid.RowDefinitions>
                            <RowDefinition/>
                            <RowDefinition/>
                        </Grid.RowDefinitions>
                        <StackPanel Grid.Row="0" Margin="20">
                            <TextBlock Text="{Binding Name}" 
                                       Foreground="{TemplateBinding Foreground}" 
                                       FontWeight="Bold" FontSize="20"/>
                            <Rectangle Height="5"/>
                            <TextBlock Text="{Binding Occupation}" 
                                       Foreground="{TemplateBinding Foreground}" 
                                       FontSize="16"/>
                        </StackPanel>
                        <StackPanel Grid.Row="1" Orientation="Horizontal">
                            <TextBlock Grid.Column="0" Text="☻"  
                                       VerticalAlignment="Center"  Margin="20" 
                                       FontSize="50" Foreground="#E26441"/>
                            <StackPanel Margin="30 0 0 0" Width="150">
                                <TextBlock Text="COMPANY NAME"/>
                                <TextBlock Text="Age:">
                                    <Run Text="{Binding Age}"/>
                                </TextBlock>
                                <TextBlock Text="Money:">
                                    <Run Text="{Binding Money, StringFormat={}{0:C}}"/>
                                </TextBlock>
                                <TextBlock Text="Address:" TextWrapping="Wrap">
                                    <Run Text="{Binding Address}"/>
                                </TextBlock>
                            </StackPanel>
                        </StackPanel>
                    </Grid>
                </Border>
                <ControlTemplate.Triggers>
                    <Trigger Property="IsMouseOver" Value="True">
                        <Setter Property="Background" Value="#7AAB7D" />
                    </Trigger>
                </ControlTemplate.Triggers>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

在这个样式中，我们定义了一个ControlTemplate控件模板，而控件模板的可视化树稍微有点复杂。首先在最外层定义了一个Border对象，并通过TemplateBinding绑定了Width、Height和Background，这三个属性哪来的？就是Button的Width、Height和Background。ContentPresenter对象则放到了Border的ToolTip属性中。

然后，我们在Border中实例化了一个Grid对象，并在里面放了一些StackPanel和TextBlock，需要注意的是，TextBlock的Text采用了Binding对象，分别绑定了一些属性。这些绑定的数据源其实是从Button的DataContext中获得的。所以我们如果要顺利地使用这个样式，则在使用时，一定要给Button的DataContext属性赋值一个数据源。这个数据源就是Person实体。

前端使用样式的代码

```
<Button Content="将ControlTemplate定义在Style样式中"         Width="280" Height="200" Margin="10"         Click="Button_Click"        Style="{StaticResource CardButtonStyle}"        DataContext="{Binding Person}"></Button>
```

如上所示，我们给Button的DataContext绑定了一个Person数据源，这个Person会在Button的可视化树中被引用，从而得到我们想要的效果。

![[assets/WPF从小白到大佬：模板：：TemplateBinding模板绑定/ce87ddf6933ab49f04060bbfda3b8e3b_MD5.jpg]]

当鼠标移上去时，还会触发ControlTemplate的触发器，实现背景颜色的变化。

![[assets/WPF从小白到大佬：模板：：TemplateBinding模板绑定/b71bc93f24ca0340896b2ba6a0e757ed_MD5.jpg]]

从效果图上看，它像是一个复杂的布局，或者一个UserControl。其实它就是一个Button，只不过，我们通过样式、模板、触发器、绑定等手段，实现了这一效果。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：065-《TemplateBinding模板绑定》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月28日