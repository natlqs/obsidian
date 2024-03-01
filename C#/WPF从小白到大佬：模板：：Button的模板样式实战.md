我们以Button按钮为例，演示其模板和样式的用法。首先我们定义两个样式，并在样式中定义了Button的控件模板(ControlTemplate)。

第一个样式

```xml
<Style x:Key="ButtonIconStyle" TargetType="Button">
    <Setter Property="Background"  Value="Transparent"/>
    <Setter Property="Foreground" Value="#646464"/>
    <Setter Property="FontSize" Value="16"/>
    <Setter Property="Height" Value="30"/>
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="Button">
                <Border Background="{TemplateBinding Background}"
                        CornerRadius="5"
                        Margin="0"
                        Height="{TemplateBinding Height}">
                    <Grid HorizontalAlignment="Center"
                          VerticalAlignment="Center">
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="auto"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Grid.Column="0" Text="{TemplateBinding Tag}"
                                   FontSize="{TemplateBinding FontSize}"
                                   Foreground="{TemplateBinding Foreground}"
                                   Margin="5 5 5 5"
                                   HorizontalAlignment="Center"
                                   VerticalAlignment="Center"/>
                        <TextBlock Grid.Column="1" 
                                   Text="{TemplateBinding Content}"
                                   FontSize="{TemplateBinding FontSize}"
                                   Foreground="{TemplateBinding Foreground}"
                                   Margin="0 5 5 5" 
                                   HorizontalAlignment="Center"
                                   VerticalAlignment="Center"/>
                    </Grid>
                </Border>
                <ControlTemplate.Triggers>
                    <Trigger Property="IsMouseOver" Value="True">
                        <Setter Property="Background" Value="LightPink"/>
                    </Trigger>
                    <Trigger Property="IsMouseOver" Value="False">
                        <Setter Property="Background" Value="Transparent"/>
                    </Trigger>
                </ControlTemplate.Triggers>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

在上面的样式中，我们在Template属性中实例化了一个ControlTemplate控件模板，为Button的模板（可视化树）定义了一个Border装饰器，为了实现图文按钮效果，所以在里面实例化了一个Grid,以及两个TextBlock，使用TemplateBinding将Button的属性与可视化树中的控件的属性进行模板绑定，巧妙的利用Button的Tag属性作为图标显示。最后，在ControlTemplate中实例化了两个触发器，条件是鼠标移上去或移开，改变Button的背景颜色。

第二个样式

```xml
<Style x:Key="ButtonIconBorderStyle" TargetType="Button">
    <Setter Property="Background"  Value="Transparent"/>
    <Setter Property="Foreground" Value="#646464"/>
    <Setter Property="FontSize" Value="16"/>
    <Setter Property="Height" Value="30"/>
    <Setter Property="Margin" Value="5"/>
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="Button">
                <Border Background="{TemplateBinding Background}"
                        BorderBrush="{TemplateBinding BorderBrush}"
                        BorderThickness="{TemplateBinding BorderThickness}"
                        CornerRadius="0"
                        Margin="0"
                        Height="{TemplateBinding Height}">
                    <Grid HorizontalAlignment="Center"
                          VerticalAlignment="Center">
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="auto"/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <TextBlock Grid.Column="0" Text="{TemplateBinding Tag}"
                                   FontSize="{TemplateBinding FontSize}"
                                   Foreground="{TemplateBinding Foreground}"
                                   Margin="5 5 5 5"
                                   HorizontalAlignment="Center"
                                   VerticalAlignment="Center"/>
                        <TextBlock Grid.Column="1" 
                                   Text="{TemplateBinding Content}"
                                   FontSize="{TemplateBinding FontSize}"
                                   Foreground="{TemplateBinding Foreground}"
                                   Margin="0 5 5 5" 
                                   HorizontalAlignment="Center"
                                   VerticalAlignment="Center"/>
                    </Grid>
                </Border>
                <ControlTemplate.Triggers>
                    <Trigger Property="IsMouseOver" Value="True">
                        <Setter Property="Background" Value="DarkGreen"/>
                        <Setter Property="BorderBrush" Value="Green"/>
                        <Setter Property="Foreground" Value="White"/>
                    </Trigger>
                    <Trigger Property="IsMouseOver" Value="False">
                        <Setter Property="Background" Value="Transparent"/>
                    </Trigger>
                </ControlTemplate.Triggers>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

我们在第一种样式的基础之上进行了细节优化，绑定了Button的边框到可视化树中的Border控件上，并优化了触发器的设置，从而形成不同的按钮呈现。最后我们来看一下两个按钮的呈现效果。

![[assets/WPF从小白到大佬：模板：：Button的模板样式实战/5245268f7f11ea048fdbb8cf91067852_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：070-《Button模板样式实战》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年10月9日