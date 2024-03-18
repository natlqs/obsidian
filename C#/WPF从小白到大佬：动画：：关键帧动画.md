在DoubleAnimationBase抽象基类下面继承了3个子类，分别是DoubleAnimation、DoubleAnimationUsingKeyFrames和DoubleAnimationUsingPath，其中DoubleAnimationUsingKeyFrames被称为关键帧动画，DoubleAnimationUsingPath被称为路径动画，而DoubleAnimation动画我们已经在上一节学过，这里就不在赘述。在本节课程，我们以DoubleAnimationUsingKeyFrames关键帧动画为例，讲解什么是关键帧动画，以及怎么使用它。

一、什么是关键帧动画

在WPF中所有以KeyFrames结尾的动画都叫关键帧动画。它其实与上一节的动画类似，也是对某个目标属性进行动画处理。DoubleAnimation拥有Form/To，表示从一个值过渡到另一个值。而关键帧动画使用关键帧对象进行描述，而且一个关键帧动画可以拥有多个关键帧对象。

什么是关键帧对象？其实就是一个KeyFrames属性集合中的元素。所以我们需要看一下DoubleAnimationUsingKeyFrames的定义。

二、DoubleAnimationUsingKeyFrames的定义

```cs
public class DoubleAnimationUsingKeyFrames : DoubleAnimationBase, IKeyFrameAnimation, IAddChild
{
    public DoubleAnimationUsingKeyFrames();
 
    public DoubleKeyFrameCollection KeyFrames { get; set; }
    public bool IsAdditive { get; set; }
    public bool IsCumulative { get; set; }
 
    public DoubleAnimationUsingKeyFrames Clone();
    public DoubleAnimationUsingKeyFrames CloneCurrentValue();
    public bool ShouldSerializeKeyFrames();
    protected virtual void AddChild(object child);
    protected virtual void AddText(string childText);
    protected override void CloneCore(Freezable sourceFreezable);
    protected override void CloneCurrentValueCore(Freezable sourceFreezable);
    protected override Freezable CreateInstanceCore();
    protected override bool FreezeCore(bool isChecking);
    protected override void GetAsFrozenCore(Freezable source);
    protected override void GetCurrentValueAsFrozenCore(Freezable source);
    protected sealed override double GetCurrentValueCore(double defaultOriginValue, double defaultDestinationValue, AnimationClock animationClock);
    protected sealed override Duration GetNaturalDurationCore(Clock clock);
    protected override void OnChanged();
 
}
```

在上面的代码中，KeyFrames属性表示DoubleAnimationUsingKeyFrames的关键帧对象集合，这个集合中的元素类型为DoubleKeyFrame，但是DoubleKeyFrame其实是一个抽象类哦，所以真正在使用这个集合时，其中的元素都是DoubleKeyFrame的子类，比如LinearDoubleKeyFrame（线性内插关键帧）。

DoubleKeyFrame有哪些子类？

```cs
//离散:两个关键帧之间突变（到达时间点的时候硬切换，没有过渡效果）
System.Windows.Media.Animation.DiscreteDoubleKeyFrame
 
//缓动:使用缓动函数曲线实现弹性变化
System.Windows.Media.Animation.EasingDoubleKeyFrame
 
//线性:两个关键帧之间均匀变化
System.Windows.Media.Animation.LinearDoubleKeyFrame
 
//样条:使用贝塞尔曲线实现更精确的加速和减速控制
System.Windows.Media.Animation.SplineDoubleKeyFrame
```

结论：一个关键帧对象表示一个动画片段，而一个关键帧动画包含多个关键帧对象。所以，我们可以利用关键帧动画设计出更复杂的动画效果。下面我们以一个实例来说明关键帧的用法。

三、关键帧动画实例

这次我们在XAML前端代码中演示关键帧动画的用法。首先在窗体资源中定义一个故事板。

```xml
<Storyboard x:Key="KeyFrameStoryboard">
    <PointAnimationUsingKeyFrames
        Storyboard.TargetName="ellipse"
        Storyboard.TargetProperty="Fill.GradientOrigin"
        AutoReverse="True" 
        RepeatBehavior="Forever">
        <LinearPointKeyFrame Value="0.25,0.25" KeyTime="0:0:0"/>
        <LinearPointKeyFrame Value="0.75,0.35" KeyTime="0:0:1"/>
        <LinearPointKeyFrame Value="0.25,0.75" KeyTime="0:0:2"/>
        <LinearPointKeyFrame Value="0.85,0.85" KeyTime="0:0:3"/>
    </PointAnimationUsingKeyFrames>
</Storyboard>
```

这个Storyboard 中，我们实例化了一个PointAnimationUsingKeyFrames ，也就是点关键帧动画。这里有3个地方需要注意，第一点，Storyboard.TargetName指向了目标对象；第二点，Storyboard.TargetProperty指向了目标对象的属性；第三点，我们在PointAnimationUsingKeyFrames 的KeyFrames 属性中实例化了4个LinearPointKeyFrame 关键帧对象，这4个对象的Value值首尾相接实现动画处理，而后面的KeyTime,表示时间段上的连续。从这个Value的变化，我们可以得出结论，关键帧可以提供多个动画值输出给目标对象的属性，而一般的动画只有单一个值的变化。

怎么使用这个故事板？

```xml
<Ellipse x:Name="ellipse" 
         Canvas.Left="-200"
         Canvas.Top="50"
         Width="200" 
         Height="200">
    <Ellipse.Triggers>
        <EventTrigger RoutedEvent="Loaded">
            <BeginStoryboard Storyboard="{StaticResource KeyFrameStoryboard}"/>
        </EventTrigger>
    </Ellipse.Triggers>
    <Ellipse.Fill>
        <RadialGradientBrush GradientOrigin="0.25,0.25" 
                             RadiusX="0.75" 
                             RadiusY="0.75">
            <RadialGradientBrush.GradientStops>
                <GradientStop Color="White" Offset="0" />
                <GradientStop Color="LightCoral" Offset="0.65" />
                <GradientStop Color="Gray" Offset="0.8" />
            </RadialGradientBrush.GradientStops>
        </RadialGradientBrush>                
    </Ellipse.Fill>
</Ellipse>
```

我们在椭圆的的Loaded事件中，利用事件触发器EventTrigger去触发这个KeyFrameStoryboard。于是，椭圆的画刷的GradientOrigin属性便被这个动画一直修改。椭圆中间的白点将随着动画输出的坐标点而运动。

![[assets/WPF从小白到大佬：动画：：关键帧动画/17d20b875bfb1f803ddd3370b98a5dd5_MD5.jpg]]

下面是直接在Canvas的触发器中定义并使用关键帧动画的XAML代码。

```xml
<Canvas.Triggers>
    <EventTrigger RoutedEvent="Canvas.Loaded">
        <BeginStoryboard>
            <Storyboard>
                <DoubleAnimationUsingKeyFrames 
                    Storyboard.TargetName="ellipse"
                    Storyboard.TargetProperty="(Canvas.Left)">
                    <LinearDoubleKeyFrame Value="0" KeyTime="0:0:0.5"/>
                    <EasingDoubleKeyFrame Value="150" KeyTime="0:0:2">
                        <EasingDoubleKeyFrame.EasingFunction>
                            <SineEase EasingMode="EaseInOut"/>
                        </EasingDoubleKeyFrame.EasingFunction>
                    </EasingDoubleKeyFrame>
                </DoubleAnimationUsingKeyFrames>
            </Storyboard>
        </BeginStoryboard>
    </EventTrigger>
    <EventTrigger RoutedEvent="Canvas.MouseLeftButtonUp">
        <BeginStoryboard>
            <Storyboard>
                <DoubleAnimationUsingKeyFrames Storyboard.TargetName="ellipse"
                                           Storyboard.TargetProperty="(Canvas.Left)">
                    <LinearDoubleKeyFrame Value="700" KeyTime="0:0:2"/>
                </DoubleAnimationUsingKeyFrames>
            </Storyboard>
        </BeginStoryboard>
    </EventTrigger>
    <EventTrigger RoutedEvent="Canvas.MouseRightButtonUp">
        <BeginStoryboard>
            <Storyboard>
                <DoubleAnimationUsingKeyFrames Storyboard.TargetName="ellipse"
                                           Storyboard.TargetProperty="(Canvas.Left)">
                    <EasingDoubleKeyFrame Value="150" KeyTime="0:0:1">
                        <EasingDoubleKeyFrame.EasingFunction>
                            <CircleEase EasingMode="EaseOut"/>
                        </EasingDoubleKeyFrame.EasingFunction>
                    </EasingDoubleKeyFrame>
                </DoubleAnimationUsingKeyFrames>
            </Storyboard>
        </BeginStoryboard>
    </EventTrigger>
</Canvas.Triggers>
```

在Canvas的事件触发器中，我们一共定义了3种情况，分别是Canvas.Loaded、Canvas.MouseLeftButtonUp和Canvas.MouseRightButtonUp，每一种事件都会触发一个关键帧动画。通过DoubleAnimationUsingKeyFrames关键帧动画去控制ellipse对象相对于Canvas的左边距。

在Canvas.Loaded事件中，我们将椭圆从左到右以动画的形式显示在窗体的中间；

在Canvas.MouseLeftButtonUp事件中，我们让椭圆从中间位置往右运动，直至消失；

在Canvas.MouseRightButtonUp事件中，我们又让椭圆从右边回到窗体的中间位置。

![[assets/WPF从小白到大佬：动画：：关键帧动画/3fd23bc32e22da9137f856ae4884782e_MD5.jpg]]

最终形成的源代码如下所示：

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
        <Storyboard x:Key="KeyFrameStoryboard">
            <PointAnimationUsingKeyFrames
                Storyboard.TargetName="ellipse"
                Storyboard.TargetProperty="Fill.GradientOrigin"
                AutoReverse="True" 
                RepeatBehavior="Forever">
                <LinearPointKeyFrame Value="0.25,0.25" KeyTime="0:0:0"/>
                <LinearPointKeyFrame Value="0.75,0.35" KeyTime="0:0:1"/>
                <LinearPointKeyFrame Value="0.25,0.75" KeyTime="0:0:2"/>
                <LinearPointKeyFrame Value="0.85,0.85" KeyTime="0:0:3"/>
            </PointAnimationUsingKeyFrames>
        </Storyboard>
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
    <Canvas x:Name="canvas" Background="Transparent">
        <Canvas.Triggers>
            <EventTrigger RoutedEvent="Canvas.Loaded">
                <BeginStoryboard>
                    <Storyboard>
                        <DoubleAnimationUsingKeyFrames 
                            Storyboard.TargetName="ellipse"
                            Storyboard.TargetProperty="(Canvas.Left)">
                            <LinearDoubleKeyFrame Value="0" KeyTime="0:0:0.5"/>
                            <EasingDoubleKeyFrame Value="150" KeyTime="0:0:2">
                                <EasingDoubleKeyFrame.EasingFunction>
                                    <SineEase EasingMode="EaseInOut"/>
                                </EasingDoubleKeyFrame.EasingFunction>
                            </EasingDoubleKeyFrame>
                        </DoubleAnimationUsingKeyFrames>
                    </Storyboard>
                </BeginStoryboard>
            </EventTrigger>
            <EventTrigger RoutedEvent="Canvas.MouseLeftButtonUp">
                <BeginStoryboard>
                    <Storyboard>
                        <DoubleAnimationUsingKeyFrames Storyboard.TargetName="ellipse"
                                                   Storyboard.TargetProperty="(Canvas.Left)">
                            <LinearDoubleKeyFrame Value="700" KeyTime="0:0:2"/>
                        </DoubleAnimationUsingKeyFrames>
                    </Storyboard>
                </BeginStoryboard>
            </EventTrigger>
            <EventTrigger RoutedEvent="Canvas.MouseRightButtonUp">
                <BeginStoryboard>
                    <Storyboard>
                        <DoubleAnimationUsingKeyFrames Storyboard.TargetName="ellipse"
                                                   Storyboard.TargetProperty="(Canvas.Left)">
                            <EasingDoubleKeyFrame Value="150" KeyTime="0:0:1">
                                <EasingDoubleKeyFrame.EasingFunction>
                                    <CircleEase EasingMode="EaseOut"/>
                                </EasingDoubleKeyFrame.EasingFunction>
                            </EasingDoubleKeyFrame>
                        </DoubleAnimationUsingKeyFrames>
                    </Storyboard>
                </BeginStoryboard>
            </EventTrigger>
        </Canvas.Triggers>
        <Ellipse x:Name="ellipse" 
                 Canvas.Left="-200"
                 Canvas.Top="50"
                 Width="200" 
                 Height="200">
            <Ellipse.Triggers>
                <EventTrigger RoutedEvent="Loaded">
                    <BeginStoryboard Storyboard="{StaticResource KeyFrameStoryboard}"/>
                </EventTrigger>
            </Ellipse.Triggers>
            <Ellipse.Fill>
                <RadialGradientBrush GradientOrigin="0.25,0.25" 
                                     RadiusX="0.75" 
                                     RadiusY="0.75">
                    <RadialGradientBrush.GradientStops>
                        <GradientStop Color="White" Offset="0" />
                        <GradientStop Color="LightCoral" Offset="0.65" />
                        <GradientStop Color="Gray" Offset="0.8" />
                    </RadialGradientBrush.GradientStops>
                </RadialGradientBrush>                
            </Ellipse.Fill>
        </Ellipse>
    </Canvas>
</Window>
```

这是在前端XAML代码中使用关键帧中的情况，下一节，我们演示在C#后端使用关键帧。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：102-《关键帧动画》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年11月10日