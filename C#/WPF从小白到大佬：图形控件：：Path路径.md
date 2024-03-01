```cs
public sealed class Path : Shape
{
    public static readonly DependencyProperty DataProperty;
 
    public Path();
 
    public Geometry Data { get; set; }
 
    protected override Geometry DefiningGeometry { get; }
}
```

从定义上看，Path只有一个Data属性，这个属性的类型为Geometry。而Geometry又是一个抽象类，所以我们不能直接使用它，那它肯定会有一系列可以实例化的子类。没错，Geometry表示一个几何，而几何的图形可以分为好几种。

|   |   |
|---|---|
|几何名称|说明|
|LineGeometry|直线几何|
|RectangleGeometry|矩形几何|
|EllipseGeometry|椭圆几何|
|PathGeometry|路径几何|
|StreamGeometry|PathGeometry的轻量级替代品，不支持 Bidning、动画等功能|
|CombinedGeometry|多图形组合，形成单一几何几何图形|
|GeometryGroup|多图形组合，形成几何图形组|

接下来，我们分别讲一下这几种几何的用法。

**一、LineGeometry直线几何**

```cs
<Path Stroke="Blue" Fill="Red">
    <Path.Data>
        <LineGeometry  StartPoint="10,20" EndPoint="100,200"/>
    </Path.Data>
</Path>
```

**二、RectangleGeometry矩形几何**

```cs
<Path Stroke="Blue" Fill="Red">
    <Path.Data>
        <RectangleGeometry Rect="50,20,30,40" />
    </Path.Data>
</Path>
```

**三、EllipseGeometry椭圆几何**

```cs
<Path Stroke="Yellow" Fill="LightGreen">
    <Path.Data>
        <EllipseGeometry Center="150,80" RadiusX="60" RadiusY="50"/>
    </Path.Data>
</Path>
```

![[assets/WPF从小白到大佬：图形控件：：Path路径/ba6052fdbb3791b1961c63491d57bd04_MD5.jpg]]

从上面的3个例子来看，Line、Rectangle、Ellipse控件能够画出来的效果，Path都可以画出来。而接下来我们要分享的是，Line、Rectangle、Ellipse控件画不出来的效果，Path也能画出来。那就是PathGeometry路径几何。

**四、PathGeometry路径几何**

PathGeometry微微有点复杂。它有一个Figures属性，可以容纳很多较复杂的图形。Figures是一个集合，其中的元素是PathFigure类型，而PathFigure中的Segments属性又是一个集合，其中的元素类型为PathSegment。

PathSegment是一个抽象类，我们可以实例化PathSegment的子类放到PathFigure中，然后把PathFigure放到PathGeometry中，这样就可以绘制不同的路径图形了。那么PathSegment有哪些子类呢？

|   |   |
|---|---|
|LineSegment|直线段|
|ArcSegment|圆弧线段|
|BezierSegment|三次方贝塞尔曲线段|
|QuadraticBezierSegmnt|二次方贝塞尔曲线段|
|PolyLineSegment|折线段|
|PolyBezierSegment|多三次方贝塞尔曲线段|
|PolyQuadraticBezierSegment|多二次方贝塞尔曲|
|||

PathFigure有一个StartPoint属性表示起点坐标，而Segments集合中的元素就是上面那张表中的各种线段实例，它们将依次首尾相接，最终绘制成形。

我们以LineSegment和ArcSegment为例。

```cs
<Path Stroke="Black" Fill="LightPink" StrokeThickness="5">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigure StartPoint="150,200">
                    <LineSegment  Point="300,200"/>
                    <ArcSegment Point="300 50" 
                                Size="100 100" 
                                SweepDirection="Clockwise" 
                                IsLargeArc="False"/>
                    <ArcSegment Point="300 200" 
                                Size="100 100" 
                                SweepDirection="Clockwise" 
                                IsLargeArc="False"/>
                </PathFigure>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

![[assets/WPF从小白到大佬：图形控件：：Path路径/46265f6793dba9d86a57374d34b87025_MD5.jpg]]

首先，PathFigure 图形的起点坐标为（150,200），然后第一个元素是线段，终点坐标为（300,200），图形的坐标原点是左上角（0，0），所以，往下就是Y轴正半轴方向，往右就是X轴正半轴方向。

然后，画了两条圆弧，第一条圆弧的起点坐标就是线段的终点坐标，即（300,200），圆弧的终点坐标为（300 50），大小为（100 100），第二条圆弧的终点坐标又回到了线段的终点坐标（300 200），于是就出现了图中的样子。

ArcSegment的常用属性如下：  
Point：指明圆弧连接的终点；  
Size：指明完整椭圆的横轴半径和纵轴半径；  
IsLargeArc：指明是否使用大弧去连接 ；  
SweepDirection ：指明圆弧是顺时针方向还是逆时针方向；  
RotationAngle：指明圆弧椭圆的旋转角度；

接下来，我们再讲一下**BezierSegment**贝塞尔曲线。

**BezierSegment**需要4个坐标点来完成图形的绘制，分别是起点，控制点1，控制点2和终点。

```cs
<PathFigure IsFilled="False" StartPoint="5,5">
    <BezierSegment Point1="200,50" Point2="50,200"  Point3="350,250"/>
</PathFigure>
```

如上所示，（5，5）表示起点（StartPoint属性），Point1属性（200,50）和Point2属性（50,200）表示两个控制点，Point3属性（350,250）表示终点。

![[assets/WPF从小白到大佬：图形控件：：Path路径/b66484ca252a553dc5e518e5bca24dce_MD5.jpg]]

五、Path的标记语法

通过上面的示例我们会发现要绘制复杂的图形，需要实例化各种子类，代码繁琐，这时就需要了解Path的路径标记语法，它大大减少了代码量。您可以从以下的表格或[微软官网](https://learn.microsoft.com/zh-cn/dotnet/desktop/wpf/graphics-multimedia/path-markup-syntax?view=netframeworkdesktop-4.8)中获得相关知识。

|   |   |   |   |   |
|---|---|---|---|---|
|命令|用途|语法|示例|对应标签语法|
|M|移动到起点坐标|M 起点|M 150,200|<PathFigure StartPoint="150,200">|
|L|绘制直线|L 终点|L 300,200|<LineSegment Point="300,200"/>|
|H|水平直线|H 终点横坐标|||
|V|垂直直线|V 终点横坐标|||
|A|绘制圆弧|A 母椭圆尺寸 旋转角度 是否大弧 顺时针/逆时针 终点|A 180，80 45 1 1 150，150|<ArcSegment Size="180,80" RotationAngle="45" IsLargeArc="True" SweepDirection="Clockwise" Point="150,150" />|
|C|三次方贝塞尔曲线|C 控制点1 控制点2 终点|C 200,50 50,200 350,250|<BezierSegment Point1="200,50" Point2="50,200" Point3="350,250"/>|
|Q|二次方贝塞尔曲线|Q 控制点1 终点|Q 200,50 350,250|<QuadraticBezierSegmnt Point1="200,50" Point3="350,250"/>|
|S|平滑三次方贝塞尔曲线|S 控制点2 终点|S 200,50 350,250||
|T|平滑二次方贝塞尔曲线|T 终点|T 350,250||
|Z|闭合图形|Z|M 10,150 L40,150 L40,250 L10,250 Z|<Path Fill="HotPink" Data="M 10,150 L40,150 L40,250 L10,250 Z" />|

```cs
<Path Fill="HotPink" Data="M 10,150 L40,150 L40,250 L10,250 Z"/>
```

![[assets/WPF从小白到大佬：图形控件：：Path路径/422b215c73fbb57f483f1a292bd3c997_MD5.jpg]]

最后，我们通过Path的路径标记语法画了一个封闭的矩形图形，可以看到代码量的书写大大减少了。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：082-《Path路径》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年10月20日