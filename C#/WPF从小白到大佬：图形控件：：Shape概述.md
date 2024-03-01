形状是WPF另一大系列控件。WPF所有的形状都继承于Shape基类。那么，WPF提供了哪些可用的形状呢？我们用一张图来说明它的子类。

![[assets/WPF从小白到大佬：图形控件：：Shape概述/290221874de4d095ad79152bbb3af0a1_MD5.jpg]]

|   |   |
|---|---|
|形状名称|说明|
|Ellipse|椭圆形|
|Line|在两个点之间绘制直线。|
|Rectangle|绘制矩形。|
|Polyline|绘制一系列相互连接的直线。|
|Polygon|绘制多边形，它是由一系列相互连接的线条构成的闭合形状。|
|Path|绘制一系列相互连接的直线和曲线。|

Shape是一个抽象基类，它不能被实例化，所以我们在使用时只能实例化它的子类。而Shape的父类是FrameworkElement，所以，所有的Shape子类都是一个UIElement 类，因此形状对象可以用在面板和大多数控件中。 由于 Canvas 面板支持其子对象的绝对位置，因此特别适合创建复杂的图形。

我们来看看Shape的定义

一、Shape的定义

```cs
public abstract class Shape : FrameworkElement
{
    public static readonly DependencyProperty StretchProperty;
    public static readonly DependencyProperty StrokeDashArrayProperty;
    public static readonly DependencyProperty StrokeDashOffsetProperty;
    public static readonly DependencyProperty StrokeLineJoinProperty;
    public static readonly DependencyProperty StrokeDashCapProperty;
    public static readonly DependencyProperty StrokeMiterLimitProperty;
    public static readonly DependencyProperty StrokeStartLineCapProperty;
    public static readonly DependencyProperty StrokeThicknessProperty;
    public static readonly DependencyProperty StrokeProperty;
    public static readonly DependencyProperty FillProperty;
    public static readonly DependencyProperty StrokeEndLineCapProperty;
 
    protected Shape();
 
    public Brush Stroke { get; set; }
    public PenLineCap StrokeEndLineCap { get; set; }
    public PenLineCap StrokeStartLineCap { get; set; }
    public double StrokeThickness { get; set; }
    public Brush Fill { get; set; }
    public double StrokeDashOffset { get; set; }
    public virtual Geometry RenderedGeometry { get; }
    public Stretch Stretch { get; set; }
    public DoubleCollection StrokeDashArray { get; set; }
    public double StrokeMiterLimit { get; set; }
    public PenLineCap StrokeDashCap { get; set; }
    public virtual Transform GeometryTransform { get; }
    public PenLineJoin StrokeLineJoin { get; set; }
    protected abstract Geometry DefiningGeometry { get; }
 
    protected override Size ArrangeOverride(Size finalSize);
    protected override Size MeasureOverride(Size constraint);
    protected override void OnRender(DrawingContext drawingContext);
 
}
```

Shape基类提供了许多公共属性，如下表所示。

二、属性成员

|                    |                                                          |
| ------------------ | -------------------------------------------------------- |
| 属性名称               | 说明                                                       |
| Stroke             | 获取或设置Shape的边框颜色画刷                                        |
| StrokeEndLineCap   | 获取或设置Shape描述线的末端的样式                                      |
| StrokeStartLineCap | 获取或设置Shape描述线的开头的样式                                      |
| StrokeThickness    | 获取或设置Shape边框的厚度                                          |
| Fill               | 获取或设置Shape的内部填充颜色                                        |
| StrokeDashOffset   | 获取或设置短划线模式内短划线开始处的距离                                     |
| RenderedGeometry   | 获取或设置Shape的几何                                            |
| Stretch            | 获取或设置Shape的填充模式                                          |
| StrokeDashArray    | 获取或设置勾勒形状轮廓的短划线和间隙的模式的值                                  |
| StrokeMiterLimit   | 获取或设置一个限制到一半的斜接长度比                                       |
| StrokeDashCap      | 获取或设置 System.Windows.Media.PenLineCap 枚举值，该值指定如何绘制虚线的末端。 |
| GeometryTransform  | 获取或设置Shape的转换                                            |
| StrokeLineJoin     | 获取或设置Shape的顶点处使用的联接类型。                                   |
| DefiningGeometry   | 获取Shape的Geometry                                         |