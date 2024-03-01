Ellipse继承于Shape，Shape继承于FrameworkElement，所以，它可以设置其 Width 和 Height。 使用其 Fill 属性指定用于绘制椭圆形内部的 Brush。 使用其 Stroke 属性指定用于绘制椭圆形轮廓的 Brush。 StrokeThickness 属性指定椭圆形轮廓的粗细。

下面的示例演示了椭圆和正圆。

前端代码

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d"
        Title="WPF中文网 - wpfsoft.com - 椭圆课程" Height="350" Width="500">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <Ellipse Width="100" 
                 Height="50" 
                 Stroke="Black" 
                 StrokeThickness="2" 
                 Fill="Red">
            <Ellipse.Triggers>
                <EventTrigger RoutedEvent="Loaded">
                    <BeginStoryboard>
                        <Storyboard>
                            <DoubleAnimation From="100" 
                                             To="200" 
                                             Duration="0:0:2" 
                                             AutoReverse="True" 
                                             RepeatBehavior="Forever"
                                             Storyboard.TargetProperty="(Ellipse.Width)" />
                            <DoubleAnimation From="50" 
                                             To="100" 
                                             Duration="0:0:2" 
                                             AutoReverse="True" 
                                             RepeatBehavior="Forever"
                                             Storyboard.TargetProperty="(Ellipse.Height)" />
                        </Storyboard>
                    </BeginStoryboard>
                </EventTrigger>
            </Ellipse.Triggers>
        </Ellipse>
        <Grid Grid.Column="1">
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition Height="auto"/>
            </Grid.RowDefinitions>
            <Ellipse Width="{Binding ElementName=slider,Path=Value}" 
                     Height="{Binding ElementName=slider,Path=Value}" 
                     Fill="Green"/>
            <Slider x:Name="slider" Grid.Row="1" Value="50" Maximum="200"/>
        </Grid>
    </Grid>
</Window>
```

![[assets/WPF从小白到大佬：图形控件：：Elipse椭圆/136153f6265571d96cd8c03ac1d356e8_MD5.jpg]]

在上面的例子中，我们实例化了两个椭圆，第一个椭圆用了一个事件Triggers和Storyboard故事板，演示了椭圆大小的动画改变，在第二个椭圆中，宽度和高度相等，便出现了一个正圆，利用Binding对象将Slider的Value和椭圆的尺寸绑定起来，以此滑动改变椭圆大小。

注：关于动画部分，我们将在后面专门讲解。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：078-《Ellipse椭圆》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年10月17日