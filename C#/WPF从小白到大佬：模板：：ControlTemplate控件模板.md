**一、控件模板概述**

在前面的章节中，我们介绍了逻辑树和可视化树，界面通常由多个控件构成，多个控件会构成一个树，这棵树就是逻辑树，逻辑树指界面上所有控件的组织关系；而每个控件内部也有一定的组织关系（可视化树），这个组织关系定义了控件的结构和外观。WPF为每个控件的定义了一个默认的结构外观，也就是默认的控件模板。

在进一步学习控件模板之前，我们先了解一下模板与样式的区别。

比如一个人的肤色、臂长、身高、五官等，这个可以通过样式设定，所以大街上我们可以看到形形色色的人，有的是白种人，有的是黄种人，有的身高1米5，有的身高2米，有的五官好看，有的不好看。但是这些人都有皮肤、两只手、两只眼睛等等。这是因为他们的模板都是相同的。

咦？那有的人生下来就只有一只手，或者一只眼，这做何解释？这就是因为他的模板与大多数人不一样。

我们再以WPF中的Button为例。默认情况下，Button按钮的内容只能显示文字，我们可以设置它的Content属性即可。也可以设置它的Width和Height，改变它的尺寸，但是，它始终是一个矩形的按钮。假如我们希望得到一个圆形的按钮、或者带图标的按钮，这个时候就需要去改变按钮的内部结构外观——ControlTemplate控件模板。

**FrameworkElement基类有一个Template属性就是指控件的ControlTemplate模板，这就意味着，几乎所有的WPF控件都是可以修改它的结构和外观（可视化树）的**。注意，不能仅替换控件的可视化树的一部分；若要更改控件的可视化树，必须将该控件的 Template 属性设置为新的完整 ControlTemplate。

**二、查看控件的默认模板**

下面的操作演示了如何查看控件的默认模板。

比如我们有一个Button按钮，在设计界面中用鼠标单击右键-编辑模板-编辑副本。

![[assets/WPF从小白到大佬：模板：：ControlTemplate控件模板/c62dc82c1557af0b245fd12a80eb5e5b_MD5.jpg]]

此时，会弹出一个对话框，如下所示。名称表示定义当前按钮的样式key的名称，定义位置默认在此文档中，于是会生成当前按钮的默认样式。我们可以从当前按钮的默认样式中找到它内部的可视化树——控件模板。

![[assets/WPF从小白到大佬：模板：：ControlTemplate控件模板/9bd056c0b0cea77cd0270ca5d8614ca4_MD5.jpg]]

这些模板样式代码会生成到Window.Resources中。代码如下

```xml
    <Window.Resources>
        <Style x:Key="FocusVisual">
            <Setter Property="Control.Template">
                <Setter.Value>
                    <ControlTemplate>
                        <Rectangle Margin="2" SnapsToDevicePixels="true" Stroke="{DynamicResource {x:Static SystemColors.ControlTextBrushKey}}" StrokeThickness="1" StrokeDashArray="1 2"/>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
        <SolidColorBrush x:Key="Button.Static.Background" Color="#FFDDDDDD"/>
        <SolidColorBrush x:Key="Button.Static.Border" Color="#FF707070"/>
        <SolidColorBrush x:Key="Button.MouseOver.Background" Color="#FFBEE6FD"/>
        <SolidColorBrush x:Key="Button.MouseOver.Border" Color="#FF3C7FB1"/>
        <SolidColorBrush x:Key="Button.Pressed.Background" Color="#FFC4E5F6"/>
        <SolidColorBrush x:Key="Button.Pressed.Border" Color="#FF2C628B"/>
        <SolidColorBrush x:Key="Button.Disabled.Background" Color="#FFF4F4F4"/>
        <SolidColorBrush x:Key="Button.Disabled.Border" Color="#FFADB2B5"/>
        <SolidColorBrush x:Key="Button.Disabled.Foreground" Color="#FF838383"/>
        <Style x:Key="ButtonStyle1" TargetType="{x:Type Button}">
            <Setter Property="FocusVisualStyle" Value="{StaticResource FocusVisual}"/>
            <Setter Property="Background" Value="{StaticResource Button.Static.Background}"/>
            <Setter Property="BorderBrush" Value="{StaticResource Button.Static.Border}"/>
            <Setter Property="Foreground" Value="{DynamicResource {x:Static SystemColors.ControlTextBrushKey}}"/>
            <Setter Property="BorderThickness" Value="1"/>
            <Setter Property="HorizontalContentAlignment" Value="Center"/>
            <Setter Property="VerticalContentAlignment" Value="Center"/>
            <Setter Property="Padding" Value="1"/>
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="{x:Type Button}">
                        <Border x:Name="border" BorderBrush="{TemplateBinding BorderBrush}" BorderThickness="{TemplateBinding BorderThickness}" Background="{TemplateBinding Background}" SnapsToDevicePixels="true">
                            <ContentPresenter x:Name="contentPresenter" Focusable="False" HorizontalAlignment="{TemplateBinding HorizontalContentAlignment}" Margin="{TemplateBinding Padding}" RecognizesAccessKey="True" SnapsToDevicePixels="{TemplateBinding SnapsToDevicePixels}" VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
                        </Border>
                        <ControlTemplate.Triggers>
                            <Trigger Property="IsDefaulted" Value="true">
                                <Setter Property="BorderBrush" TargetName="border" Value="{DynamicResource {x:Static SystemColors.HighlightBrushKey}}"/>
                            </Trigger>
                            <Trigger Property="IsMouseOver" Value="true">
                                <Setter Property="Background" TargetName="border" Value="{StaticResource Button.MouseOver.Background}"/>
                                <Setter Property="BorderBrush" TargetName="border" Value="{StaticResource Button.MouseOver.Border}"/>
                            </Trigger>
                            <Trigger Property="IsPressed" Value="true">
                                <Setter Property="Background" TargetName="border" Value="{StaticResource Button.Pressed.Background}"/>
                                <Setter Property="BorderBrush" TargetName="border" Value="{StaticResource Button.Pressed.Border}"/>
                            </Trigger>
                            <Trigger Property="IsEnabled" Value="false">
                                <Setter Property="Background" TargetName="border" Value="{StaticResource Button.Disabled.Background}"/>
                                <Setter Property="BorderBrush" TargetName="border" Value="{StaticResource Button.Disabled.Border}"/>
                                <Setter Property="TextElement.Foreground" TargetName="contentPresenter" Value="{StaticResource Button.Disabled.Foreground}"/>
                            </Trigger>
                        </ControlTemplate.Triggers>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </Window.Resources>
```

在前面的章节中，我们已了解过样式的写法，在这里，我们关注一下Setter项目设置Template属性的写法，因为Template属性是ContorlTemplate类型，所以在上面的代码中实例化了一个ControlTemplate对象，并且，TargetType="{x:Type Button}"，表示这个ContorlTemplate实例是给Button定义的模板。

而在ContorlTemplate对象中，定义了一棵可视化树。

```xml
<Border x:Name="border" 
        BorderBrush="{TemplateBinding BorderBrush}" 
        BorderThickness="{TemplateBinding BorderThickness}" 
        Background="{TemplateBinding Background}" 
        SnapsToDevicePixels="true">
    <ContentPresenter x:Name="contentPresenter" 
                      Focusable="False" 
                      HorizontalAlignment="{TemplateBinding HorizontalContentAlignment}" 
                      Margin="{TemplateBinding Padding}" 
                      RecognizesAccessKey="True" 
                      SnapsToDevicePixels="{TemplateBinding SnapsToDevicePixels}" 
                      VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
</Border>
```

这里定义了一个Border装修器，里面有一个ContentPresenter对象。什么是ContentPresenter对象？

三、什么是ContentPresenter对象？

ContentPresenter继承于FrameworkElement，说明它也是一个控件。从命名上看，它叫内容主持者，本质上它只是一个占座的，为谁占座？为ContentControl内容控件占座。因为Button继承于ContentControl，所以Button也有Content属性，在ContentTemplate中的ContentPresenter可视为等于Content属性。

> 友情提示
> 
> ContentPresenter 定义边框属性，使你无需使用其他 Border 元素即可在 ContentPresenter 周围绘制 边框 。 属性为 ContentPresenter.BorderBrush、 ContentPresenter.BorderThickness、 ContentPresenter.CornerRadius 和 ContentPresenter.Padding。

四、控件模板的几种设置方式

4.1将ControlTemplate定义在在控件中

```xml
<Button Content="将ControlTemplate定义在在控件中" 
        Width="280" Height="40" Margin="10" Foreground="#747787">
    <Button.Template>
        <ControlTemplate TargetType="Button">
            <Border Background="Transparent" CornerRadius="5" BorderThickness="1" BorderBrush="#C9CCD5">
                <ContentPresenter  HorizontalAlignment="Center" VerticalAlignment="Center"/>
            </Border>
        </ControlTemplate>
    </Button.Template>
</Button>
```

4.2将ControlTemplate定义在资源中

```xml
<Window.Resources>
    <ControlTemplate x:Key="ButtonTemplate" TargetType="Button">
        <Border Background="#C6D2FC" CornerRadius="5" BorderThickness="1" BorderBrush="#545BAD">
            <ContentPresenter  HorizontalAlignment="Center" VerticalAlignment="Center"/>
        </Border>
    </ControlTemplate>
</Window.Resources>
 
<Button Content="将ControlTemplate定义在资源中" 
        Template="{StaticResource ButtonTemplate}" 
        Width="280" Height="40" Margin="10" Foreground="#707CA5"/>
```

4.3将ControlTemplate定义在Style样式中

```xml
<Button Content="将ControlTemplate定义在Style样式中" 
        Width="280" Height="40" Margin="10" Foreground="White">
    <Button.Style>
        <Style TargetType="Button">
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="Button">
                        <Border Background="#7AAB7D" CornerRadius="5">
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
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </Button.Style>
</Button>
```

上面三种方式，都可以去定义控件的ContentTemplate，不同的定义方式，可见度不一样而已。比如第一种定义方式，只能是当前那个控件私有，第二种定义方式，它就是一个公有的模板实例。

![[assets/WPF从小白到大佬：模板：：ControlTemplate控件模板/4b4cab34f848bd2608a27fb51fbb9a61_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：063-《ControlTemplate控件模板》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年9月28日










