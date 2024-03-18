我们同样以DoubleAnimation动画为例，演示如何在C#代码中实现动画效果。动画的本质是在一段时间内输出一个值，我们要做的事情就是把这个值赋值到某个依赖属性上，然后触发这个动画。

所以，要实现动画，可以简要分为3步，第一步，实例化一个目标对象，第二步，实例化一个动画对象，第三步，将动画对象输出的值赋值到目标对象的属性并启动该动画。前面两步由开发者完成，第三步由WPF来完成，那么，WPF是如何启动一个动画的？

在Animatable抽象基类中，有一个BeginAnimation()方法成员可以完成上述第三步的操作。

```cs
public abstract class Animatable : Freezable, IAnimatable, IResource
{
    public bool HasAnimatedProperties { get; }
 
    public static bool ShouldSerializeStoredWeakReference(DependencyObject target);
    public void ApplyAnimationClock(DependencyProperty dp, AnimationClock clock);
    public void ApplyAnimationClock(DependencyProperty dp, AnimationClock clock, HandoffBehavior handoffBehavior);
    public void BeginAnimation(DependencyProperty dp, AnimationTimeline animation);
    public void BeginAnimation(DependencyProperty dp, AnimationTimeline animation, HandoffBehavior handoffBehavior);
    public Animatable Clone();
    public object GetAnimationBaseValue(DependencyProperty dp);
    protected override bool FreezeCore(bool isChecking);
 
}
```

BeginAnimation()方法成员表示开启一个动画，第一个参数dp表示要被动作作用的依赖属性，第二个参数animation表示一个动画实例。

这里还在一个目标对象，它在哪？比如我们要在button上开启一个动画。我们可以这样做（伪代码）：

```cs
Button button = new Button();
button.BeginAnimation(dp, animation);
```

此时，这个目标对象就是button。至于button为什么也有BeginAnimation()，那是因为Button继承了UIElement基类，而UIElement基类拥有BeginAnimation()方法成员。

下面我们还是以上一节的例子为例，为Ellipse椭圆实例化一个ScaleTransform对象，因为ScaleTransform继承了Animatable 抽象基类，所以就可以为它做一个动画。

前端代码如下：

```xml
<Grid x:Name="grid" Background="Transparent" MouseUp="grid_MouseUp">
    <Ellipse x:Name="ellipse" 
             Width="200" 
             Height="200">
        <Ellipse.RenderTransform>
            <ScaleTransform CenterX="100" CenterY="100"/>
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
</Grid>
```

在Grid 的鼠标单击事件中，我们执行下面的代码

```cs
private void grid_MouseUp(object sender, MouseButtonEventArgs e)
{
    Point mousePoint = e.GetPosition(grid);
    ScaleTransform scaleTransform = ellipse.RenderTransform as ScaleTransform;
    DoubleAnimation scaleDoubleAnimation = new DoubleAnimation()
    {
        To = (mousePoint.X + mousePoint.Y) / 200,
        Duration = new TimeSpan(0, 0, 0, 0, 250),
    };
    scaleTransform.BeginAnimation(ScaleTransform.ScaleXProperty, scaleDoubleAnimation);
    scaleTransform.BeginAnimation(ScaleTransform.ScaleYProperty, scaleDoubleAnimation);
}
```

在鼠标事件的回调函数中，我们实例化了一个DoubleAnimation ，这个动画表示目标值是由鼠标当前坐标计算得来的。然后找到ellipse的ScaleTransform实例，ScaleTransform实例有ScaleXProperty和ScaleYProperty两个依赖属性，分别表示X轴方向和Y轴方向的放大比，最后在scaleTransform实例上用BeginAnimation()方法成员开启动画。

F5运行调试，随着我们每一次鼠标的不同位置的单击，椭圆将被我们随机进行放大的动画处理效果。

![[assets/WPF从小白到大佬：动画：：在Csharp代码中使用动画/4bf2b066c7a07bc13431ae1cef835587_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：101-《在C#代码中使用动画》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年11月9日