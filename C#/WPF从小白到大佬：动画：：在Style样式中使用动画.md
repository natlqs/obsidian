通过前面的学习，我们会发现，只要是在XAML代码中定义和执行动画，都需要用到Trigger中的事件触发器去启动一个动画。除了控件（FrameworkElement基类）拥有Triggers属性，Style样式也拥有Triggers属性，这也就意味着，我们可以在定义某个控件的Style样式时，再定义一些动画交互。

下面，我们为Rectangle矩形控件定义一个样式，在样式中再利用事件触发器去定义和执行一些动画。

```xml
<Window.Resources>
    <Style x:Key="RectangleStyle" TargetType="Rectangle">
        <Setter Property="Width" Value="50"/>
        <Setter Property="Height" Value="50"/>
        <Setter Property="Margin" Value="10,5"/>
        <Setter Property="StrokeThickness" Value="1"/>
        <Setter Property="Stroke" Value="DarkCyan"/>
        <Setter Property="HorizontalAlignment" Value="Left"/>
        <Style.Triggers>
            <EventTrigger RoutedEvent="Loaded">
                <BeginStoryboard>
                    <Storyboard>
                        <DoubleAnimation Duration="0:0:0.5" 
                                         To="450"
                                         Storyboard.TargetProperty="Width"/>
                    </Storyboard>
                </BeginStoryboard>
            </EventTrigger>
            <EventTrigger RoutedEvent="MouseEnter">
                <BeginStoryboard>
                    <Storyboard>
                        <DoubleAnimation Duration="0:0:0.5" 
                                         To="350"
                                         Storyboard.TargetProperty="Width"/>
                    </Storyboard>
                </BeginStoryboard>
            </EventTrigger>
            <EventTrigger RoutedEvent="MouseLeave">
                <BeginStoryboard>
                    <Storyboard>
                        <DoubleAnimation Duration="0:0:0.5" 
                                         To="50"
                                         Storyboard.TargetProperty="Width"/>
                    </Storyboard>
                </BeginStoryboard>
            </EventTrigger>
        </Style.Triggers>
    </Style>
</Window.Resources>
```

我们在Style样式的Triggers属性中定义了3个事件触发器，而每个事件触发器都拥有各自在的故事板，在故事板中定义了一个DoubleAnimation 动画，并将动画的输出值作用到Rectangle矩形的宽度。

于是，我们就可以在前端XAML中去实例化一些矩形并引用该样式。

```xml
<StackPanel>
    <Rectangle Fill="LightCoral" Style="{StaticResource RectangleStyle}"/>
    <Rectangle Fill="LightGreen" Style="{StaticResource RectangleStyle}"/>
    <Rectangle Fill="LightGoldenrodYellow" Style="{StaticResource RectangleStyle}"/>
    <Rectangle Fill="LightGray" Style="{StaticResource RectangleStyle}"/>
    <Rectangle Fill="LightPink" Style="{StaticResource RectangleStyle}"/>
</StackPanel>
```

![[assets/WPF从小白到大佬：动画：：在Style样式中使用动画/b40fbd46f77373e2caf8e2d74deea4b4_MD5.jpg]]

矩形的默认长度为50像素，当启动之后，会执行Loaded事件触发器，以动画的方式将矩形的宽度改为450。

![[assets/WPF从小白到大佬：动画：：在Style样式中使用动画/99ba708c723d0bb15c3dad51d35c4f4d_MD5.jpg]]

一旦鼠标移到某个矩形上，将执行MouseEnter事件触发器动画，将矩形的宽度以动画的方式改为350像素。当鼠标离开这个矩形时，将执行MouseLeave事件触发器动画，将矩形的宽度改为50像素。

![[assets/WPF从小白到大佬：动画：：在Style样式中使用动画/f5c41fe510e40aee881761ed9cbc90e0_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：105-《在Style样式中使用动画》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年11月17日