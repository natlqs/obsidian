我们来观察一下ControlTemplate控件模块的定义

```cs
public class ControlTemplate : FrameworkTemplate
{
    public ControlTemplate();
    public ControlTemplate(Type targetType);
 
    public Type TargetType { get; set; }
    public TriggerCollection Triggers { get; }
 
    protected override void ValidateTemplatedParent(FrameworkElement templatedParent);
 
}
```

可以看到在ControlTemplate中定义了一个Triggers 集合，说明可以定义一些触发器，以实现控件的交互效果。我们之前在《样式》章节中学过几种触发器，在这里用例子演示一下ControlTemplate的触发器的用法。

我们将上一章节的例子进行一些微调，因为在使用触发器时，有些细节需要注意。

一、在控件中的ControlTemplate的触发器

```xml
<Button Content="将ControlTemplate定义在在控件中" 
        Width="280" 
        Height="40" 
        Margin="10" 
        Foreground="#747787">
    <Button.Template>
        <ControlTemplate TargetType="Button">
            <Border x:Name="border" 
                    Background="Transparent" 
                    CornerRadius="5" 
                    BorderThickness="1" 
                    BorderBrush="#C9CCD5">
                <ContentPresenter x:Name="contentPresenter" 
                                  HorizontalAlignment="Center" 
                                  VerticalAlignment="Center"/>
            </Border>
            <ControlTemplate.Triggers>
                <Trigger Property="IsMouseOver" Value="True">
                    <Setter Property="Content" Value="MouseOver" TargetName="contentPresenter"/>
                </Trigger>
                <Trigger Property="IsMouseOver" Value="False">
                    <Setter Property="Content" Value="将ControlTemplate定义在在控件中" TargetName="contentPresenter"/>
                </Trigger>
            </ControlTemplate.Triggers>
        </ControlTemplate>
    </Button.Template>
</Button>
```

在上面的例子中，我们在Triggers集合中增加了两个Trigger 对象，条件是当鼠标移上去或鼠标移开的时候，更改了Button的Content属性。

二、在Resources定义的ControlTemplate的触发器

```xml
<Window.Resources>
    <ControlTemplate x:Key="ButtonTemplate" TargetType="Button">
        <Border Background="#C6D2FC" 
                CornerRadius="5" 
                BorderThickness="1" 
                BorderBrush="#545BAD">
            <ContentPresenter  HorizontalAlignment="Center" 
                               VerticalAlignment="Center"/>
        </Border>
        <ControlTemplate.Triggers>
            <Trigger Property="IsMouseOver" Value="True">
                <Setter Property="Width" Value="300"/>
            </Trigger>
            <Trigger Property="IsMouseOver" Value="False">
                <Setter Property="Width" Value="280"/>
            </Trigger>
        </ControlTemplate.Triggers>
    </ControlTemplate>
</Window.Resources>
 
<Button Content="将ControlTemplate定义在资源中" 
        Template="{StaticResource ButtonTemplate}" 
        Height="40" Margin="10" Foreground="#707CA5"/>
```

上面演示了定义在资源中的控件模板的触发器，需要注意的一点是，我们在触发器执行时更改了Button的宽度，这个时候，要把Button原先设置的Width删除，因为”就近原则“，直接设置控件的属性值大于天，而在模板中设置控件的属性值的权重要小一些。

三、Style样式中的ConControlTemplate的触发器

```xml
<Button Content="将ControlTemplate定义在Style样式中" 
        Width="280" Height="40" Margin="10" >
    <Button.Style>
        <Style TargetType="Button">
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="Button">
                        <Border x:Name="border" 
                                CornerRadius="5" 
                                BorderThickness="1" 
                                BorderBrush="#7AAB7D">
                            <Grid>
                                <Grid.ColumnDefinitions>
                                    <ColumnDefinition Width="auto"/>
                                    <ColumnDefinition/>
                                </Grid.ColumnDefinitions>
                                <TextBlock Grid.Column="0" Text="☻" 
                                           VerticalAlignment="Center" 
                                           Margin="3" FontSize="18"/>
                                <ContentPresenter Grid.Column="1" 
                                                  HorizontalAlignment="Center" 
                                                  VerticalAlignment="Center"/>
                            </Grid>
                        </Border>
                        <ControlTemplate.Triggers>
                            <Trigger Property="IsMouseOver" Value="True">
                                <Setter Property="Foreground" Value="#7AAB7D"/>
                                <Setter Property="Background" Value="White" TargetName="border"/>
                            </Trigger>
                            <Trigger Property="IsMouseOver" Value="False">
                                <Setter Property="Foreground" Value="White"/>
                                <Setter Property="Background" Value="#7AAB7D" TargetName="border"/>
                            </Trigger>
                        </ControlTemplate.Triggers>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </Button.Style>
</Button>
```

ControlTemplate中使用触发器，还有一个好处是：可以指定设置某个可视化树中的控件对象。比如上面这段代码，当鼠标移上去的时候，我们修改了border对象的背景色，这个背景色其实并不是Button本身的背景颜色，而是Button内部可视化树中的Border的背景颜色。只需要利用Setter的TargetName属性来指定就行了。

而鼠标移上去的时候，我们还修改了Foreground前景颜色，这个Foreground才是Button本身的属性。说明什么问题？说明ControlTemplate中的触发器不但可以修改控件的属性，还可以修改控件模板中的可视化树的元素的属性，它真是太好用了。

我们来看一下运行结果。

![[assets/WPF从小白到大佬：模板：：ControlTemplate的触发器/4b4cab34f848bd2608a27fb51fbb9a61_MD5.jpg]]

鼠标移上去之前

![[assets/WPF从小白到大佬：模板：：ControlTemplate的触发器/787ef537009a098d733f295bb1e91260_MD5.jpg]]

鼠标移上去之后

要充分了解ControlTemplate控件模板的定义和触发器的使用，还离不开TemplateBinding（模板绑定），它将控件的属性和控件的可视化树元素的属性建立绑定关系，在设计控件模板时，可以更好的设计出控件的结构、外观和交互效果。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：064-《ControlTemplate的触发器》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月28日