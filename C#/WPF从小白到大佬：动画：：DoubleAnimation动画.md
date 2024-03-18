DoubleAnimation动画只是AnimationTimeline众多子类中的一个，因为比较常用，我们将它作为本例中的动画对象。

```cs
public class DoubleAnimation : DoubleAnimationBase
{
    public static readonly DependencyProperty FromProperty;
    public static readonly DependencyProperty ToProperty;
    public static readonly DependencyProperty ByProperty;
    public static readonly DependencyProperty EasingFunctionProperty;
 
    public DoubleAnimation();
    public DoubleAnimation(double toValue, Duration duration);
    public DoubleAnimation(double toValue, Duration duration, FillBehavior fillBehavior);
    public DoubleAnimation(double fromValue, double toValue, Duration duration);
    public DoubleAnimation(double fromValue, double toValue, Duration duration, FillBehavior fillBehavior);
 
    public double? From { get; set; }
    public double? To { get; set; }
    public double? By { get; set; }
    public IEasingFunction EasingFunction { get; set; }
    public bool IsAdditive { get; set; }
    public bool IsCumulative { get; set; }
 
    public DoubleAnimation Clone();
    protected override Freezable CreateInstanceCore();
    protected override double GetCurrentValueCore(double defaultOriginValue, double defaultDestinationValue, AnimationClock animationClock);
 
}
```

从定义上看，它有4个（依赖）属性和2个普通属性，下表中罗列了它们的说明。

|   |   |
|---|---|
|属性名|说明|
|From|获取或设置动画的起始值。|
|To|获取或设置动画的结束值。|
|By|获取或设置动画更改其起始值所依据的总数。|
|EasingFunction|获取或设置应用于此动画的缓动函数。|
|IsAdditive|是否应将目标属性的当前值添加到此动画的起始值。|
|IsCumulative|动画重复时是否累计该动画的值。|

我们在上一节提到过，动画是可以做成资源的。所以，在本例中，我们将动画定义成一个资源。

```xml
<Window.Resources>
    <Storyboard x:Key="WidthStoryboard" TargetProperty="Width">
        <DoubleAnimation 
                     From="200" 
                     To="300" 
                     Duration="0:0:1.5"
                     AutoReverse="True" 
                     RepeatBehavior="Forever">
        </DoubleAnimation>
    </Storyboard>
    <Storyboard x:Key="HeightStoryboard" TargetProperty="Height">
        <DoubleAnimation 
                     From="200" 
                     To="300" 
                     Duration="0:0:1.5"
                     AutoReverse="True" 
                     RepeatBehavior="Forever">
        </DoubleAnimation>
    </Storyboard>
</Window.Resources>
```

在这个动画中，我们规定了在1.5秒内输出一个double值，由200变成300，并且反转播放和无限循环播放。至于将这个动画用在何处，并不是DoubleAnimation所关心的事情。谁关心这个值作用在何处？故事板的TargetProperty属性指示了输出的double要作用于目标对象的哪个属性。就像上面的WidthStoryboard和HeightStoryboard，说明Width和Height两个属性将在1.5秒内反复在200-300之间来回进行动画渲染。

至于这两个故事用在何处？它并不关心，哪个控件调用了这两个故事板，哪个控件就拥有了这样的动画效果。

如何调用动画？动画的启用必须要有一个触发机制，而触发器就可以触发一个动画。FrameworkElement基类的Triggers集合用来定义触发器。我们可以利用EventTrigger事件触发器在XAML代码中触发一个TriggerAction，比如BeginStoryboard就是TriggerAction的子类，在BeginStoryboard中可以设置一个Storyboard故事板。

```xml
<Ellipse.Triggers>
    <EventTrigger RoutedEvent="Loaded" >
        <EventTrigger.Actions>
            <BeginStoryboard Storyboard="{StaticResource WidthStoryboard }"/>
            <BeginStoryboard Storyboard="{StaticResource HeightStoryboard }"/>
        </EventTrigger.Actions>
    </EventTrigger>
</Ellipse.Triggers>
```

比如，我们要给一个Ellipse控件启用上面的动画，就可以在Ellipse的Triggers中实例化一个EventTrigger对象，并在EventTrigger对象的Actions属性中实例化两个BeginStoryboard实例，各自引用资源中已定义的两个Storyboard。

**注意：WidthStoryboard 和HeightStoryboard 动画输出值是分别作用于目标对象的Width和Height两个属性，在使用前要确定Ellipse控件有没有Width和Height属性。其次，就算Ellipse控件有Width和Height属性，也要确认Width和Height属性的类型是不是double，毕竟DoubleAnimation动画值出的值可是double值哦，生产什么就消费什么，不可张冠李戴。**

前端完整代码：

```xml
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld" 
        xmlns:controls="clr-namespace:HelloWorld.Controls"
        xmlns:helper="clr-namespace:HelloWorld.MVVM"
        mc:Ignorable="d" 
        Title="WPF中文网 - 动画 - www.wpfsoft.com" Height="350" Width="500">    
    <Window.DataContext>
        <local:MainViewModel/>
    </Window.DataContext>
    <Window.Resources>
        <Storyboard x:Key="WidthStoryboard" TargetProperty="Width">
            <DoubleAnimation 
                         From="200" 
                         To="300" 
                         Duration="0:0:1.5"
                         AutoReverse="True" 
                         RepeatBehavior="Forever">
            </DoubleAnimation>
        </Storyboard>
        <Storyboard x:Key="HeightStoryboard" TargetProperty="Height">
            <DoubleAnimation 
                         From="200" 
                         To="300" 
                         Duration="0:0:1.5"
                         AutoReverse="True" 
                         RepeatBehavior="Forever">
            </DoubleAnimation>
        </Storyboard>
    </Window.Resources>
    <Grid x:Name="grid" Background="Transparent"
          MouseMove="grid_MouseMove">
        <Ellipse x:Name="ellipse" 
                 Width="200" 
                 Height="200">
            <Ellipse.Triggers>
                <EventTrigger RoutedEvent="Loaded" >
                    <EventTrigger.Actions>
                        <BeginStoryboard Storyboard="{StaticResource WidthStoryboard }"/>
                        <BeginStoryboard Storyboard="{StaticResource HeightStoryboard }"/>
                    </EventTrigger.Actions>
                </EventTrigger>
            </Ellipse.Triggers>
            <Ellipse.Fill>
                <RadialGradientBrush GradientOrigin="0.25,0.25" 
                                     RadiusX="0.75" 
                                     RadiusY="0.75">
                    <RadialGradientBrush.GradientStops>
                        <GradientStop Color="White" Offset="0" />
                        <GradientStop Color="Goldenrod" Offset="0.65" />
                        <GradientStop Color="Gray" Offset="0.8" />
                    </RadialGradientBrush.GradientStops>
                </RadialGradientBrush>                
            </Ellipse.Fill>
        </Ellipse>
    </Grid>
</Window>
```

最后，F5运行调试，我们会看到Ellipse被不断放大缩小哦。另外，除了在XAML代码中运用动画，我们还可以在C#代码中使用动画效果，下一节来演示这样的用法。

![[assets/WPF从小白到大佬：动画：：DoubleAnimation动画/4bf2b066c7a07bc13431ae1cef835587_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：100-《DoubleAnimation动画》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年11月09日