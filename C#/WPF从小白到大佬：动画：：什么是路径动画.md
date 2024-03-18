AnimationTimeline抽象基类下面有3种动画，分别是普通动画、关键帧动画和路径动画。像前面学过的DoubleAnimation就是普通动画，DoubleAnimationUsingKeyFrames就是关键帧动画，那么路径动画是什么？DoubleAnimationUsingPath就是一个路径动画。也就是说，`<Type>+AnimationUsingPath`这种命令格式的动画，就是路径动画。

路径动画是一种使用 PathGeometry（路径几何）作为输入的动画，我们曾经在介绍[《Path路径》](https://www.wpfsoft.com/2023/10/20/2535.html)时讲过PathGeometry，要灵活使用路径动画，则必须要掌握PathGeometry基础知识。

WPF提供了3种路径动画，如下表所示

|   |   |   |
|---|---|---|
|路径动画名称|输入值类型|说明|
|DoubleAnimationUsingPath|Double|沿着路径针对对象进行动画处理（双重动画）|
|PointAnimationUsingPath|Point|沿着路径针对对象进行动画处理（点动画）|
|MatrixAnimationUsingPath|Matrix|沿着路径针对对象进行动画处理（矩阵动画）|

我们以DoubleAnimationUsingPath为例，来说明路径动画的用法。

```cs
public class DoubleAnimationUsingPath : DoubleAnimationBase
{
    public static readonly DependencyProperty PathGeometryProperty;
    public static readonly DependencyProperty SourceProperty;
 
    public DoubleAnimationUsingPath();
 
    public PathGeometry PathGeometry { get; set; }
    public PathAnimationSource Source { get; set; }
    public bool IsAdditive { get; set; }
    public bool IsCumulative { get; set; }
 
    public DoubleAnimationUsingPath Clone();
    protected override Freezable CreateInstanceCore();
    protected override double GetCurrentValueCore(double defaultOriginValue, double defaultDestinationValue, AnimationClock animationClock);
    protected override void OnChanged();
 
}
```

可以看到DoubleAnimationUsingPath有一个PathGeometry（路径几何）属性，用来设置路径内容。Source 属性会根据路径的方位输出一个动画值，它的类型为PathAnimationSource枚举，其中有3个值，分别如下：

|   |   |
|---|---|
|||
|PathAnimationSource.X|指定沿动画序列路径前进过程中的 x 坐标偏移量。|
|PathAnimationSource.Y|指定沿动画序列路径前进过程中的 y 坐标偏移量。|
|PathAnimationSource.Angle|指定沿动画序列路径前进过程中的旋转正切角。|

最后，把这个Source值使用到目标对象(可能是某个控件)的属性即可。

在XAML代码中使用DoubleAnimationUsingPath时，依然要先实例化一个Storyboard故事板，DoubleAnimationUsingPath实例将放到Storyboard故事板的Children属性中。对了，Storyboard故事板本身并没有Children属性，而是继承了它的TimelineGroup基类。

例如：

```xml
<Window.Resources>
    <Storyboard x:Key="PathStoryboard" RepeatBehavior = "Forever" AutoReverse="True">
        <DoubleAnimationUsingPath BeginTime="00:00:00" Duration="00:00:05"
                                  Storyboard.TargetName="EllipseTranslateTransform"
                                  Storyboard.TargetProperty="X"
                                  Source="X">
            <DoubleAnimationUsingPath.PathGeometry>
                <PathGeometry >
                    <PathGeometry.Figures>
                        <PathFigure IsFilled="False" StartPoint="5,5">
                            <BezierSegment Point1="200,50" Point2="50,200"  Point3="350,250"/>
                        </PathFigure>
                    </PathGeometry.Figures>                        
                </PathGeometry>
            </DoubleAnimationUsingPath.PathGeometry>
        </DoubleAnimationUsingPath>
        <DoubleAnimationUsingPath BeginTime="00:00:00" Duration="00:00:05"
                                  Storyboard.TargetName="EllipseTranslateTransform"
                                  Storyboard.TargetProperty="Y"
                                  Source="Y">
            <DoubleAnimationUsingPath.PathGeometry>
                <PathGeometry >
                    <PathGeometry.Figures>
                        <PathFigure IsFilled="False" StartPoint="5,5">
                            <BezierSegment Point1="200,50" Point2="50,200"  Point3="350,250"/>
                        </PathFigure>
                    </PathGeometry.Figures>
                </PathGeometry>
            </DoubleAnimationUsingPath.PathGeometry>
        </DoubleAnimationUsingPath>
    </Storyboard>
</Window.Resources>
```

我们在PathStoryboard实例中定义了两个DoubleAnimationUsingPath，两个DoubleAnimationUsingPath的PathGeometry 内容都相同，一个赋值给X属性，一个赋值给Y属性。什么意思？将路径动画的X输出值赋值到椭圆的TranslateTransform的X属性，将路径动画的Y输出值赋值到椭圆的TranslateTransform的Y属性，从而控制椭圆的平移路径。关于椭圆的定义如下：

```xml
<Canvas x:Name="canvas" MouseUp="canvas_MouseUp">
    <Canvas.Triggers>
        <EventTrigger RoutedEvent="Loaded">
            <BeginStoryboard Storyboard="{StaticResource PathStoryboard}"/>
        </EventTrigger>
    </Canvas.Triggers>
    <Canvas.Background>
        <LinearGradientBrush StartPoint="0.1,0.5" EndPoint="0.8,0.1" >
            <LinearGradientBrush.GradientStops>
                <GradientStop Color="LightBlue"  Offset="0" />
                <GradientStop Color="LightGoldenrodYellow"  Offset="0.5" />
                <GradientStop Color="LightPink" Offset="1" />
            </LinearGradientBrush.GradientStops>
        </LinearGradientBrush>
    </Canvas.Background>
 
    <Ellipse x:Name="ellipse" 
         Width="100" 
         Height="100" Canvas.Left="0" Canvas.Top="0">
        <Ellipse.RenderTransform>
            <TranslateTransform x:Name="EllipseTranslateTransform"/>
        </Ellipse.RenderTransform>
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
    <Path Stroke="Black" Fill="LightPink" StrokeThickness="5">
        <Path.Data>
            <PathGeometry>
                <PathGeometry.Figures>
                    <PathFigure IsFilled="False" StartPoint="5,5">
                        <BezierSegment Point1="200,50" Point2="50,200"  Point3="350,250"/>
                    </PathFigure>
                </PathGeometry.Figures>
            </PathGeometry>
        </Path.Data>
    </Path>
</Canvas>
```

如何启动这个路径动画？我们在Canvas的Loaded事件中通过触发器启动这个动画。

![](https://www.wpfsoft.com/wp-content/uploads/2023/11/2023111608515487.jpg)

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：104-《什么是路径动画》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年11月16日