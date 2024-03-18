我们在前面的章节中讲过如何在C#代码中使用Animation动画，本节将讲解在C#代码中使用关键帧动画。其实两者的用法几乎是一模一样的，只是关键帧动画的实例化时，设置的参数不同而已。

首先我们在XAML前端代码中实现如下的效果。

```xml
<Canvas x:Name="canvas" MouseUp="canvas_MouseUp">
    <Canvas.Background>
        <LinearGradientBrush StartPoint="0.1,0.5" EndPoint="0.8,0.1" >
            <LinearGradientBrush.GradientStops>
                <GradientStop Color="LightBlue"  Offset="0" />
                <GradientStop Color="LightGoldenrodYellow"  Offset="0.5" />
                <GradientStop Color="LightPink" Offset="1" />
            </LinearGradientBrush.GradientStops>
        </LinearGradientBrush>
    </Canvas.Background>
</Canvas>
```

![[assets/WPF从小白到大佬：动画：：在Csharp代码中使用关键帧动画/46a3b01073a30efc747190f6b718ad4e_MD5.jpg]]

我们将Canvas控件背景设置成LinearGradientBrush 线性渐变，模拟云上晚霞的天空。

接下来，我们每次单击Canvas控件时就改变线性渐变的开始位置和结束位置，并且使用关键帧动画去做改变，这会产生一种穿行云间的感觉。

```cs
private void canvas_MouseUp(object sender, MouseButtonEventArgs e)
{
    //实例化关键帧动画和关键帧对象
    LinearGradientBrush brush = canvas.Background as LinearGradientBrush;
    PointAnimationUsingKeyFrames startAnimation = new PointAnimationUsingKeyFrames();
    PointAnimationUsingKeyFrames endAnimation = new PointAnimationUsingKeyFrames();
    LinearPointKeyFrame startKey = new LinearPointKeyFrame();
    LinearPointKeyFrame endKey = new LinearPointKeyFrame();
    startAnimation.KeyFrames.Add(startKey);
    endAnimation.KeyFrames.Add(endKey);
 
    //随机xy的值，并设置关键帧对象的Value和KeyTime
    Random random = new Random();
    double x = random.NextDouble();
    Thread.Sleep(1);
    double y = random.NextDouble();
    startKey.Value = new Point(x, y);
    startKey.KeyTime = TimeSpan.FromMilliseconds(2500);
 
    Thread.Sleep(1);
    x = random.NextDouble();
    Thread.Sleep(1);
    y = random.NextDouble();
    endKey.Value = new Point(x, y);
    endKey.KeyTime = TimeSpan.FromMilliseconds(1500);
 
    //开启动画
    brush.BeginAnimation(LinearGradientBrush.StartPointProperty, startAnimation);
    brush.BeginAnimation(LinearGradientBrush.EndPointProperty, endAnimation);
 
}
```

在上述代码中，首先我们实例化了两个关键帧动画，名叫startAnimation 和endAnimation ，然后，分别为这两个关键帧动画增加了一个关键帧对象（注：可以增加多个关键帧对象），然后随机两个Point对象赋值给关键帧对象的Value,设置好各自的KeyTime，最后是启用动画。

最后的效果如下：

![[assets/WPF从小白到大佬：动画：：在Csharp代码中使用关键帧动画/e8f708867e78578bc292d14df5e1ce56_MD5.jpg]]

随着每次鼠标的单击，Canvas的背景将随机产生渐变效果。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：103-《在C#代码中使用关键帧动画》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年11月15日