一、动画的概念

动画本质上是一系列快速播放的图像。每两张图像之间略有区别，如果每秒种超过24张图像在我们眼前闪过，大脑会产生一种错觉，觉得这组图像就是一个不断变化的场景。所以动画与时间相关，确切的说——动画的背后有一个计时系统提供服务支持。

WPF集成了一个高效的计时系统，可以轻松地对控件和其他图形对象进行动画处理，以及完成计时系统和重绘屏幕的所有后台任务。所以，对于动画的概念而言，时间线才是最重要的概念。于是，WPF设计了一个TimeLine类型来表示时间线，而所有的动画都是这条时间线上的产物，所以，WPF的所有动画都继承于TimeLine基类。

TimeLine基类被定义在System.Windows.Media.Animation命名空间之中，它表示一段时间，开发者可以指定该时间段的长度、开始时间、重复次数、该时间段内时间进度的快慢等。TimeLine基类下面有3个子类，分别是AnimationTimeline、TimelineGroup和MediaTimeline。

WPF的时间线

|   |   |
|---|---|
|时间线|说明|
|AnimationTimeline|一种生成输出值的时间线，将动画与属性关联时，动画在播放属性时会更新该属性的值，从而对它进行“动画处理”。比如在某一个时间段将按钮的宽度变宽。|
|TimelineGroup|表示多个时间线的集合。例如Storyboard故事板。|
|MediaTimeline|一种控制媒体文件播放的时间线。它提供对媒体计时的控制，其方式与动画时间线对象控制动画的方式相同。|

TimeLine基类的子子孙孙：

![[assets/WPF从小白到大佬：动画：：什么是动画/0737e13b4989aa2777f893998b2bfdac_MD5.jpg]]

二、TimeLine基类介绍

```cs
public abstract class Timeline : Animatable
{
    public static readonly DependencyProperty AccelerationRatioProperty;
    public static readonly DependencyProperty AutoReverseProperty;
    public static readonly DependencyProperty BeginTimeProperty;
    public static readonly DependencyProperty DecelerationRatioProperty;
    public static readonly DependencyProperty DesiredFrameRateProperty;
    public static readonly DependencyProperty DurationProperty;
    public static readonly DependencyProperty FillBehaviorProperty;
    public static readonly DependencyProperty NameProperty;
    public static readonly DependencyProperty RepeatBehaviorProperty;
    public static readonly DependencyProperty SpeedRatioProperty;
 
    protected Timeline();
    protected Timeline(TimeSpan? beginTime);
    protected Timeline(TimeSpan? beginTime, Duration duration);
    protected Timeline(TimeSpan? beginTime, Duration duration, RepeatBehavior repeatBehavior);
 
    public bool AutoReverse { get; set; }
    public double SpeedRatio { get; set; }
    public RepeatBehavior RepeatBehavior { get; set; }
    public string Name { get; set; }
    public FillBehavior FillBehavior { get; set; }
    public Duration Duration { get; set; }
    public double DecelerationRatio { get; set; }
    public TimeSpan? BeginTime { get; set; }
    public double AccelerationRatio { get; set; }
 
    public event EventHandler CurrentTimeInvalidated;
    public event EventHandler CurrentStateInvalidated;
    public event EventHandler CurrentGlobalSpeedInvalidated;
    public event EventHandler RemoveRequested;
    public event EventHandler Completed;
 
    public static int? GetDesiredFrameRate(Timeline timeline);
    public static void SetDesiredFrameRate(Timeline timeline, int? desiredFrameRate);
    public Timeline Clone();
    public Timeline CloneCurrentValue();
    public Clock CreateClock(bool hasControllableRoot);
    public Clock CreateClock();
    protected override bool FreezeCore(bool isChecking);
    protected override void GetAsFrozenCore(Freezable sourceFreezable);
    protected override void GetCurrentValueAsFrozenCore(Freezable sourceFreezable);
    protected virtual Duration GetNaturalDurationCore(Clock clock);
    protected internal virtual Clock AllocateClock();
    protected internal Duration GetNaturalDuration(Clock clock);
 
}
```

既然所有的动画都要继承TimeLine时间线，所以，我们首先要了解这个基类为我们提供了哪些可用的属性、方法和事件。

三个经常使用的计时属性为 Duration、AutoReverse 和 RepeatBehavior。

**Duration属性**：表示当前动画的时间线的长度（默认值为1秒钟）。通常用 TimeSpan 值指定，例如：TimeSpan.FromSeconds(Double) 方法，它的格式：

|设置|所得值|
|---|---|
|0:0:5.5|5.5 秒。|
|0:30:5.5|30 分 5.5 秒。|
|1:30:5.5|1 小时 30 分 5.5 秒。|

**AutoReverse属性**：表示动画在时间线到达 Duration 的终点后是否倒退。为true表示倒退播放。

**RepeatBehavior属性**：表示当前动画的时间线的播放次数。默认值为1.0，表示只播放1次。

由于一个动画组可能由多条时间线动画组成，而有的时间线长，有的时间线短，那么，若父级时间线还没走完，而子级时间线走完时该怎么办呢？**FillBehavior 属性**（枚举型）指定时间线结束时的行为方式。FillBehavior.HoldEnd表示在达到活动期的终点后，时间线将保持其进度，直至其父级的活动期和保持期结束为止。FillBehavior.Stop表示如果时间线超出活动期，而其父级在活动期内，则该时间线将停止。

三、AnimationTimeline子类介绍

根据计时进度生成输出值，作用于目标属性进行动画处理。AnimationTimeline 对象可以声明为 资源、在多个对象之间共享、使只读以提高性能、克隆和线程安全。它有许许多多的动画子类可以使用。比如经常使用的DoubleAnimation 动画。

```cs
System.Windows.Media.Animation.BooleanAnimationBase
System.Windows.Media.Animation.ByteAnimationBase
System.Windows.Media.Animation.CharAnimationBase
System.Windows.Media.Animation.ColorAnimationBase
System.Windows.Media.Animation.DecimalAnimationBase
System.Windows.Media.Animation.DoubleAnimationBase
System.Windows.Media.Animation.Int16AnimationBase
System.Windows.Media.Animation.Int32AnimationBase
System.Windows.Media.Animation.Int64AnimationBase
System.Windows.Media.Animation.MatrixAnimationBase
System.Windows.Media.Animation.ObjectAnimationBase
System.Windows.Media.Animation.Point3DAnimationBase
System.Windows.Media.Animation.PointAnimationBase
System.Windows.Media.Animation.QuaternionAnimationBase
System.Windows.Media.Animation.RectAnimationBase
System.Windows.Media.Animation.Rotation3DAnimationBase
System.Windows.Media.Animation.SingleAnimationBase
System.Windows.Media.Animation.SizeAnimationBase
System.Windows.Media.Animation.StringAnimationBase
System.Windows.Media.Animation.ThicknessAnimationBase
System.Windows.Media.Animation.Vector3DAnimationBase
System.Windows.Media.Animation.VectorAnimationBase
```

上面列出来的全是AnimationTimeline的子类，而且全是抽象子类，真正能使用的还在下一级，因为这类动画都是输出一个值，并且将这个值赋值给目标对象的属性，在设定的时间段内，这个值不断变化，从而达到目标对象的属性值不断变化，最终绘制在界面上形成动画，又因为目标对象的属性类型是多种多种的，为了尽可能的去实现各种属性的动画，所以WPF才不得不实现这些动画子类。

虽然子类较多，但用法相同，学一两种动画即可举一反三。

四、TimelineGroup子类介绍

TimelineGroup下面有一个子类叫ParallelTimeline，而ParallelTimeline下面有一个叫子类叫Storyboard （故事板）。说白了，AnimationTimeline动画只是一个单一的时间线动画，往往一个动画是有多条时间线程，比如在一个动漫片段中，一个人往左走，一个人往右走，天上的白云飘飘，树上的叶子摇曳，这4个对象都在各自的时间线上呈现动画，将它们组合起来，就形成了一个TimelineGroup，也叫容器时间线或前情提要，它是时间线的集合，典型的就是Storyboard故事板。

Storyboard 是一种容器时间线，它为其包含的时间线提供目标信息。可以使用 Storyboard 对象将影响各种对象和属性的时间线组合成一个时间线树，以便于组织和控制复杂的计时行为。可以使用 Storyboard 对可动画处理的类的依赖属性进行动画处理。

五、MediaTimeline子类介绍

MediaTimeline是一个 Timeline 对象，它提供对媒体计时的控制，其方式与动画时间线对象控制动画的方式相同。例如， MediaTimeline 具有关联的 Duration 和 BeginTime 属性可用于指定媒体开始的时间及其播放时间。可通过两种方法使用 MediaTimeline将 关联Timeline到 MediaElement 。

好，关于动画的概述我们就分享到这里，下一节，我们将以实际的例子来演示AnimationTimeline一部分子类的用法

——重庆教主 2023年10月9日